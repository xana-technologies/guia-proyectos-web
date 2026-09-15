# Módulo · Formulario avanzado

Lo que va más allá del [formulario de la base](../flujos/formulario.md):
formulario en varios pasos, varios formularios en la misma web, confirmación
por correo a quien escribe, adjuntos e histórico de envíos. Todo parte del
mismo sistema (endpoint propio + Brevo + Turnstile + firewall); aquí solo se
cuenta lo que cambia.

## Cuándo se usa (y cuándo no)

| Necesidad | ¿Qué hay? | Referencia |
| --- | --- | --- |
| Formulario en varios pasos | Montado | xanasystem-web `/demo` |
| Varios formularios en la web | Montado antes (`/auditoria` + `/demo`) | Memoria `formularios-patron-magna` de xanasystem-web |
| Confirmación al usuario | Preparada en código, **desactivada en todos los proyectos** | `sendConfirmation` en los `form.config.ts`; runbook de ingeniería (variante B) |
| Adjuntos | **No soportado**: se escala a ingeniería | `Documentacion_Tecnica_Formularios_Astro.md` §14 |
| Histórico de envíos | **Fuera del sistema**: solo el registro de Brevo | Misma documentación §1 y §14 |
| Tope de envíos por IP y día | **Aparcado** | Misma documentación §11 y §14 |

**No se usa** si basta con nombre, email, teléfono, mensaje y casilla: eso es
la base.

## Qué añade al código

### Formulario en varios pasos

xanasystem-web `src/components/sections/demo/DemoHero.astro` +
`src/pages/api/demo.ts`: paso 1 «¿Qué necesitas?» (motivo, intereses, mensaje),
paso 2 datos de contacto.

- Un solo `<form>`; cada paso es un bloque con `data-step` y los siguientes
  empiezan `hidden`.
- Grupos de casillas con `<fieldset>` + `<legend>`.
- Barra de progreso con `role="group"` y `aria-label` («paso 1 de 2»).
- Botones Anterior / Siguiente (`type="button"`) y Enviar (`type="submit"`),
  que aparece en el último paso.
- **Validación por paso** en el navegador; el servidor valida **todos** los
  campos a la vez.
- Altura fija igual a la del paso más alto, para que la tarjeta no salte.
- **Turnstile con render explícito**, porque el widget vive en un paso oculto
  al cargar:

  ```html
  <script is:inline>
    window.__turnstileReady = new Promise((res) => { window.__onTurnstileLoad = res; });
  </script>
  <script is:inline
    src="https://challenges.cloudflare.com/turnstile/v0/api.js?onload=__onTurnstileLoad&render=explicit"
    async defer></script>
  ```

- Un campo de enrutado (`motivo`) permite que una sola página de conversión
  sirva para demo y contacto (`/contacto` redirige a `/demo`).

**Lo que la referencia aún no cumple de la base** (y hay que añadir al
reutilizarla): `placeholder` en los campos, errores como frases en vez de
códigos, `GET` sin 405, envío sin JavaScript (el paso 2 está `hidden` en el
HTML y no hay 303), estado de éxito en la propia página en vez de `/gracias`,
`form.config.ts` con `??` y remitente `forms.xanasystem.com`
([deuda](../anexos/deuda-proyectos.md)). La regla de la base se mantiene: **sin
JavaScript, todos los pasos visibles y el envío funciona**; y **todo envío
correcto acaba en `/gracias`**, que es donde se mide la conversión.

### Varios formularios

xanasystem-web tuvo dos: `/auditoria` (`src/pages/api/auditoria.ts`, con las
preguntas en `src/data/auditoria.ts` compartidas por la interfaz y el endpoint,
y la puntuación recalculada en el servidor) y `/demo`. `/auditoria` se retiró y
redirige a `/demo`.

Lo que cambia respecto a uno solo:

- **Un endpoint por formulario** o un campo que los distinga; los datos que
  comparten interfaz y servidor, en `src/data/`.
- **La regla de firewall cubre todas las rutas**: la de la base es `Request
  Path` *equals* `/api/contact` ([formulario](../flujos/formulario.md) §6).
- **Lo que decide un resultado se calcula en el servidor**, no se confía en el
  navegador (memoria `formularios-patron-magna`).
- **Prueba real por formulario** ([publicación](../flujos/publicacion.md) §5.2)
  y conversión separada en GTM si se quieren distinguir.

### Confirmación al usuario

Ya está en el endpoint (xanasystem-web `src/pages/api/demo.ts`; también
xanatechnologies y magnaceramica `api/contact.ts`):

```ts
// 5) Confirmación opcional al usuario (desactivada por defecto).
if (formConfig.sendConfirmation) {
  await sendEmail({
    sender: { name: formConfig.fromName, email: formConfig.fromEmail },
    to: [{ email, name: nombre }],
    subject: formConfig.confirmationSubject,
    htmlContent: `<p>Hola ${escapeHtml(nombre)},</p><p>Hemos recibido tu solicitud…</p>`,
  }).catch((e) => console.error('Confirmación falló', e));
}
```

- **Solo con el dominio del cliente verificado en Brevo** (DKIM y DMARC) y un
  remitente bajo ese dominio. Sin eso cae en spam: Brevo envía desde IP
  compartidas y DMARC solo pasa por DKIM (runbook §2.2 y variante B).
- Se pide a ingeniería con el punto 4 de
  [`equipo/alta-formulario.md`](../plantillas/equipo/alta-formulario.md).
- **Va después del aviso y no bloquea la respuesta** (`.catch`): si falla, el
  aviso al cliente ya ha salido (documentación técnica §6.8).
- Es la **única excepción** a «a `to` nunca llega nada del visitante»: solo el
  email ya validado.
- Sin contenido comercial (lo dice el texto legal).
- Idioma y plantilla: **sin proyecto con la confirmación activa**; se deciden
  al activarla.

### Adjuntos

No soportado de serie. Implica recibir binarios en la función: límite de
tamaño, validación de tipo, antivirus y ancho de banda. **Se escala a
ingeniería, no se improvisa** (documentación técnica §14). En festival-viento
el sorteo pide la foto del ticket en un formulario externo (QR pendiente en
`Sorteo.astro`), fuera de la web.

### Histórico de envíos

El sistema no guarda nada: los datos solo transitan (documentación técnica §2).
Lo que queda es **Brevo → Transactional → Logs**. Si el cliente pide histórico
(el protocolo lo resolvía con un plugin de WordPress), es un cambio de
arquitectura: **consultar con ingeniería**
([publicación](../flujos/publicacion.md) §6).

### Tope por IP y día (aparcado)

El firewall frena ráfagas (ventana máxima de 10 minutos en Pro), no cuenta por
día. Un tope diario necesitaría un contador con estado (Vercel KV o Upstash)
consultado antes de llamar a Brevo. **Decidido dejarlo aparcado**
(documentación técnica §14).

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| Legales | Con confirmación: cambian la finalidad y se añade un párrafo en privacidad → [§ Confirmación al usuario](../plantillas/legales/parrafos-por-modulo.md#confirmación-al-usuario-del-formulario). Campos nuevos: la lista de datos de privacidad §2 = los campos del formulario. Adjuntos o almacenamiento: tratamiento nuevo, sin texto base (revisión jurídica) |
| Formulario | Casilla de privacidad comprobada en el servidor en todos los formularios; `/gracias` como final de todos |
| Firewall | Regla para cada endpoint |
| Analítica | Conversión por formulario si se quieren separar |
| Ingeniería | Verificación del dominio del cliente (confirmación); lo demás, consulta previa |
| Revisión | Prueba real por formulario, por paso y por idioma; la confirmación llega y no cae en spam |

## Qué pedir al cliente de más

- **Campos y opciones** de cada formulario y a quién llega cada uno
  → [`cliente/formulario.md`](../plantillas/cliente/formulario.md).
- **Con confirmación:** acceso a su DNS (o su técnico) para los registros de
  Brevo, y el texto que quiere que reciba el visitante.
- **Con adjuntos o histórico:** para qué los necesita, volumen y cuánto tiempo
  hay que conservar los datos, antes de hablar con ingeniería.

## Reglas y trampas

- **`sendConfirmation: false` por defecto**; `true` solo con el dominio del
  cliente verificado (comentario en los `form.config.ts`).
- **No reutilizar el remitente compartido para la confirmación.**
- **Todas las reglas de la base valen en cada paso**: etiquetas visibles,
  mínimos iguales en navegador y servidor, `method="post"`, campo trampa,
  `turnstile.reset()` tras cada intento → [formulario](../flujos/formulario.md) §8.
- **El token de Turnstile vale una vez**: con render explícito, también
  reiniciarlo tras un error → [trampas · Formulario](../trampas-y-lecciones.md#formulario).
- **Un POST de prueba desde la terminal da 403 en local**: falta `Origin`
  (xanasystem-web) → misma sección de trampas.

## Checklist

**Varios pasos**
- [ ] Sin JavaScript: todos los pasos visibles y envío con 303 a `/gracias`
- [ ] Validación por paso en el navegador y completa en el servidor
- [ ] Turnstile con render explícito y `reset()` tras cada intento
- [ ] Foco al título del paso al avanzar; progreso anunciado
- [ ] Sin `placeholder`; errores como códigos traducidos desde los datos

**Varios formularios**
- [ ] Firewall para cada ruta
- [ ] Datos compartidos interfaz/servidor en `src/data/`
- [ ] Prueba real de cada uno

**Confirmación**
- [ ] Dominio del cliente verificado (DKIM, DMARC) y remitente creado por ingeniería
- [ ] `FORM_FROM_EMAIL` con ese remitente; `sendConfirmation: true`
- [ ] Párrafo de privacidad cambiado
- [ ] Prueba real: llega y no cae en spam

**Adjuntos, histórico, tope diario**
- [ ] Consultado con ingeniería antes de construir nada

## Proyecto de referencia

- **xanasystem-web**: `src/components/sections/demo/DemoHero.astro`,
  `src/pages/api/demo.ts`, `src/form.config.ts`, `src/pages/demo.astro`,
  memoria `formularios-patron-magna`.
- **magnaceramica**: `docs/forms/Runbook_Ingenieria_Alta_Cliente_Formularios.md`
  (variante B) y `docs/forms/Documentacion_Tecnica_Formularios_Astro.md` (§2,
  §6.8, §9.2–9.3, §11, §14). Remitente y otros detalles de esos documentos están
  superados por el [flujo de formulario](../flujos/formulario.md).
- Plantilla de la guía: [`equipo/alta-formulario.md`](../plantillas/equipo/alta-formulario.md).
