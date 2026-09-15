# Flujo · Formulario

Formulario de contacto que envía un aviso por correo al cliente, protegido
contra bots y contra facturas sorpresa. **Va en todas las webs.**

Sustituye a los cuatro documentos de `magnaceramica/docs/forms/`, que se
quedaron desactualizados (remitente, `??`, error 403, códigos de error,
página de gracias). La implementación de referencia es
**clays-of-the-world**; hasta que exista el repo plantilla, se copia de ahí.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Pedir al cliente a quién llegan los avisos ([plantilla](../plantillas/cliente/formulario.md)) y a ingeniería la clave de Brevo ([plantilla](../plantillas/equipo/alta-formulario.md)) |
| [07 · Construcción](../fases/07-construccion.md) | Montarlo y probarlo en la URL temporal con correos de prueba |
| [08 · Revisión](../fases/08-revision.md) | Validación, errores, envío sin JavaScript, legales que lo mencionan |
| [09 · Publicación](../fases/09-publicacion.md) | Correos definitivos, firewall, prueba real por idioma, confirmación del cliente |

Si el proyecto necesita varios pasos, confirmación al usuario, varios
formularios o histórico de envíos → módulo
[formulario avanzado](../modulos/formulario-avanzado.md).

## 2. Qué tenemos

| Dato | Si el cliente lo da | Si no |
| --- | --- | --- |
| Destinatarios (to, cc, bcc) | Se usan en `FORM_TO/CC/BCC` | Se prueba con correos nuestros y **no se publica** hasta tenerlos |
| Campos | Se añaden (tocar tres sitios, §7) | Los de la base: nombre, email, teléfono opcional, mensaje, casilla de privacidad |
| Asunto y nombre del remitente | Se usan | Por defecto: «Nueva consulta desde la web de {marca}» / «Web {marca}» |
| Idiomas | Mensajes y página de gracias por idioma | Solo el idioma de la web |

## 3. Cómo funciona

```
Visitante ──POST──► Firewall de Vercel ──► /api/contact (única ruta en servidor)
                    5 envíos / 10 min       1. campo trampa `company` → finge éxito
                    por IP → 429            2. Turnstile verificado en servidor
                                            3. validación (la que manda)
                                            4. Brevo → aviso a FORM_TO
                                               con «Responder» a quien escribió
                                            ─► { ok: true } (JS) o 303 a /gracias (sin JS)
```

- **La web sigue siendo estática.** Solo `src/pages/api/contact.ts` lleva
  `export const prerender = false`, porque la clave de Brevo no puede llegar
  al navegador.
- **Se rechaza lo antes posible**: lo que corta el firewall no llega a la
  función ni se factura; el campo trampa y Turnstile van antes de la
  llamada cara (Brevo).
- **Todo envío correcto acaba en `/gracias`**: es donde se mide la conversión
  en Tag Manager (una vista de página).

## 4. Pasos y quién

### En el arranque (00)

1. **Belén → cliente**: [plantilla del formulario](../plantillas/cliente/formulario.md).
2. **Belén → ingeniería**: [alta de formulario](../plantillas/equipo/alta-formulario.md).
   Ingeniería crea una **clave de Brevo por proyecto** (`form-{cliente}`) y
   facilita la IP de la oficina. Si no hay clave, se para y se pide: no se
   reutiliza la de otro proyecto.

### En la construcción (07) — Claude

1. **Adaptador de Vercel** en `astro.config.mjs` (pareja de versión: 8 ↔ Astro 5,
   10 ↔ Astro 6, 11 ↔ Astro 7).
2. **Dominios permitidos**, o el formulario da 403 en Vercel:

   ```js
   // astro.config.mjs
   security: {
     allowedDomains: [
       { hostname: 'cliente.com', protocol: 'https' },
       { hostname: 'www.cliente.com', protocol: 'https' },
       { hostname: 'cliente.vercel.app', protocol: 'https' }, // URL temporal
     ],
   },
   ```

   No se desactiva la comprobación (`checkOrigin: false`): se declaran los
   dominios. Cada dominio desde el que se sirva la web tiene que estar aquí.
3. **Copiar los ficheros** de clays-of-the-world y adaptarlos:

   | Fichero | Qué es | Qué adaptar |
   | --- | --- | --- |
   | `src/pages/api/contact.ts` | Endpoint | Ruta de gracias, campos |
   | `src/form.config.ts` | Lee las variables de entorno | Valores por defecto, **`||` en vez de `??`** |
   | `src/lib/email/contact-notification.ts` | Plantilla del aviso (función pura) | Colores de marca a mano, logo, textos |
   | `src/components/sections/Contact.astro` | Formulario y su script | Estilos con los tokens del proyecto, textos desde datos |
   | `src/pages/[...lang]/thank-you.astro` | Página de gracias (`noindex`) | Ruta `/gracias/` en webs en español; sin `[...lang]` si la web tiene un solo idioma |
   | `.env.example` | Variables documentadas | Valores del proyecto |

4. **Claves de Turnstile** (Cloudflare → Turnstile): un widget con los
   dominios de producción, la URL temporal y `localhost`. En local se pueden
   usar las claves de prueba de Cloudflare:
   `PUBLIC_TURNSTILE_SITE_KEY=1x00000000000000000000AA` y
   `TURNSTILE_SECRET_KEY=1x0000000000000000000000000000000AA`.
5. **`.env` local** con correos de prueba (no se sube: está en `.gitignore`).
6. **Belén: variables en Vercel** (Settings → Environment Variables, entornos
   Production y Preview) y **regla de firewall** (§6). Las variables toman
   efecto en el siguiente despliegue, que hace Belén.
7. **Probar en la URL temporal** (§8).

### En la publicación (09)

1. Cambiar `FORM_TO/CC/BCC` a los correos definitivos del cliente en Vercel.
2. Añadir el dominio definitivo a Turnstile y a `allowedDomains` si faltaba.
3. Prueba real **por formulario y por idioma**; pedir al cliente que confirme
   que le llega.
4. Ingeniería comprueba: no cae en spam, aparece en el registro de Brevo, la
   IP de oficina no se bloquea y desde fuera el sexto envío da 429.

## 5. Configuración

### Variables de entorno

| Variable | Tipo | Valor | De dónde sale |
| --- | --- | --- | --- |
| `BREVO_API_KEY` | **Secreta** | Clave del proyecto | Ingeniería |
| `TURNSTILE_SECRET_KEY` | **Secreta** | Secret key del widget | Cloudflare |
| `PUBLIC_TURNSTILE_SITE_KEY` | Pública (la usa el widget) | Site key del widget | Cloudflare |
| `FORM_TO` | — | Destinatarios, separados por coma | Cliente |
| `FORM_CC` · `FORM_BCC` | — | Opcionales | Cliente |
| `FORM_FROM_EMAIL` | — | `no-reply@xanatechnologies.com` | Fijo (solo cambia si ingeniería lo indica) |
| `FORM_FROM_NAME` · `FORM_SUBJECT` | — | Nombre del remitente y asunto | Nosotros |
| `FORM_LOGO_URL` | — | Opcional: normalmente vacío (usa el logo de la propia web) | — |

- **Nunca** una variable secreta con prefijo `PUBLIC_`: llegaría al navegador.
- En Vercel **no se crean variables vacías**: si no se usa, no se crea.

### `form.config.ts`: `||` y no `??`

```ts
const parseList = (v: string | undefined) =>
  v ? v.split(',').map((s) => s.trim()).filter(Boolean) : [];

export const formConfig = {
  to: parseList(import.meta.env.FORM_TO),
  cc: parseList(import.meta.env.FORM_CC),
  bcc: parseList(import.meta.env.FORM_BCC),
  // `||`: una variable que existe pero está vacía también cae al valor por
  // defecto. Con `??` llegaría '' a Brevo, que rechaza el remitente vacío.
  fromName: import.meta.env.FORM_FROM_NAME || 'Web Cliente',
  fromEmail: import.meta.env.FORM_FROM_EMAIL || 'no-reply@xanatechnologies.com',
  subject: import.meta.env.FORM_SUBJECT || 'Nueva consulta desde la web',
  logoUrl: import.meta.env.FORM_LOGO_URL || undefined,
  sendConfirmation: false, // true solo en formulario avanzado con dominio del cliente verificado
};
```

### Remitente

- **`no-reply@xanatechnologies.com`** para todas las webs. Tiene que ser de un
  dominio verificado con DKIM en la cuenta de Brevo, o el aviso cae en spam.
- `no-reply@forms.xanasystem.com` (el de los documentos de Magna) **no está
  verificado** en la cuenta que se usa: no usarlo.
- La confirmación al usuario va desactivada: solo con el dominio del cliente
  verificado → [formulario avanzado](../modulos/formulario-avanzado.md).

## 6. Firewall y costes (Vercel)

**Regla** (Vercel → proyecto → Firewall → Configure → New Rule):

- Si `Request Path` *equals* `/api/contact`
- → `Rate Limit`, **Fixed Window**, **5 peticiones / 10 minutos**, clave **IP**
- Al superarlo: **Deny** (429) con **persistent action** (p. ej. 60 minutos)
- **Allow list** con la IP de la oficina (la da ingeniería; no se escribe en
  documentos)
- Review Changes → Publish

En el plan Pro la ventana máxima es de 10 minutos: el firewall frena ráfagas,
no cuenta envíos por día.

**Tres capas contra facturas**: DDoS automático de Vercel (no se factura) ·
esta regla (lo denegado no se factura) · **Spend Management** del equipo
(presupuesto on-demand de 1 $ con avisos y auto-pausa, ya activo; la pausa
afecta a todos los proyectos del equipo).

**Si hay un ataque**: Firewall → Bot Management → **Attack Challenge Mode**,
comprobar que la regla está publicada y avisar a ingeniería.

## 7. Contrato entre formulario y endpoint

| Respuesta | Cuándo | Qué muestra el formulario |
| --- | --- | --- |
| `200 { ok: true }` | Enviado (o campo trampa: se finge éxito) | Redirige a `/gracias` |
| `303` a `/gracias` | Envío **sin JavaScript** (petición sin `Accept: application/json`) | — |
| `400 { ok: false, error: 'invalid-request' }` | Cuerpo ilegible | Mensaje genérico |
| `400 … 'verification'` | Sin token o Turnstile lo rechaza | «Verificación fallida, inténtalo de nuevo» |
| `400 … 'validation'` | Algún campo no pasa (`detail` dice cuáles) | Mensaje de validación |
| `429` (sin JSON) | Lo devuelve el **firewall**, no el endpoint | «Demasiados envíos, espera unos minutos» |
| `500 … 'config'` | Falta una variable (`detail` dice cuál, nunca su valor) | Mensaje genérico con el correo de contacto |
| `502 … 'send'` | Brevo rechaza el envío | Mensaje genérico con el correo de contacto |
| `405` | `GET` a `/api/contact` | — (evita errores 500 en el log) |

- `error` es un **código**, no una frase: el texto se pone en el formulario
  desde los datos, que es donde se traduce.
- `detail` es para diagnosticar (el formulario lo escribe en la consola del
  navegador). **Nunca lleva claves.**

**Añadir un campo** = tocar tres sitios: el componente, la lectura y
validación del endpoint, y la plantilla del correo.

## 8. Reglas del formulario visible

- Etiquetas siempre visibles, **sin `placeholder`** (se confunde con un valor).
- `method="post"` siempre: con `get`, sin JavaScript, los datos irían en la URL.
- `aria-invalid` + `aria-describedby` en cada campo, estado con `role="status"`.
- Sin JavaScript funciona la validación nativa; con JavaScript, mensajes
  propios traducidos (`form.noValidate = true` al cargar).
- **Los mínimos del navegador y del servidor, iguales** (`minlength` 2 en
  nombre, 5 en mensaje). El script comprueba el mínimo a mano además de
  `validity.tooShort`, que no salta con texto pegado o autocompletado.
- No marcar error mientras se escribe por primera vez; sí al corregir.
- Casilla de privacidad obligatoria y **comprobada también en el servidor**.
- Campo trampa `company` fuera de pantalla (no `display: none`), con
  `tabindex="-1"` y `aria-hidden`.
- Hueco reservado para Turnstile (`min-height: 65px`) y
  `turnstile.reset()` después de cada intento: el token vale una vez.
- Botón desactivado y `aria-busy` mientras envía.

## 9. Plantilla del correo

- Función pura en `src/lib/email/`: se puede ejecutar con Node para ver el HTML.
- En el idioma **de quien lo recibe** (la oficina del cliente), no el de la web.
- Tablas, estilos en línea, fuentes de sistema; colores de marca escritos a
  mano (a un correo no llegan los tokens).
- Todo dato del visitante pasa por `escapeHtml`; a `to`, `cc` y asunto nunca
  llega nada del visitante (solo a `replyTo`, ya validado).
- Logo como URL absoluta de la propia web (no base64: Gmail no lo muestra),
  con `alt` con estilo por si se bloquean las imágenes.
- Para Outlook y modo oscuro: tarjeta en una sola celda, logo sobre fondo
  opaco, `color-scheme: light`, preheader. Referencia:
  `xanatechnologies/src/pages/api/contact.ts`.

## 10. Diagnóstico: «el formulario no va»

| Síntoma | Causa | Solución |
| --- | --- | --- |
| 403 «Cross-site POST form submissions are forbidden» en Vercel (en local va) | El dominio no está en `security.allowedDomains` | Añadirlo |
| No aparece el widget | Falta `PUBLIC_TURNSTILE_SITE_KEY` | Variable en Vercel + despliegue |
| `verification` con `invalid-input-secret` | La secret de Vercel no es la del widget | Revisar claves |
| `verification` con el dominio | El dominio no está dado de alta en el widget | Añadirlo en Cloudflare |
| `config` | Falta `BREVO_API_KEY`, `TURNSTILE_SECRET_KEY` o `FORM_TO` (`detail` lo dice) | Crear la variable + despliegue |
| `send` · `unauthorized` «unrecognised IP address» | Brevo tiene restringidas las IP | Brevo → Security → Authorized IPs (ingeniería) |
| `send` · `unauthorized` «Key not found» | Clave de Brevo mal copiada o revocada | Ingeniería |
| `send` · `invalid_parameter` sobre el sender | Remitente vacío o no verificado | `||` en `form.config.ts`; remitente `no-reply@xanatechnologies.com` |
| Validación rechaza lo que el navegador aceptó | Mínimos distintos | Igualar `minlength` y servidor |
| Todo 200 pero no llega | Brevo **aceptó**, no entregó: spam o bloqueo | Carpeta de spam y Brevo → Transactional → Logs con el `messageId` del log de Vercel |
| 429 en pruebas | Firewall | Probar desde la oficina (IP en lista blanca) |

Dónde mirar: consola del navegador (`[contact] Envío rechazado`) → Vercel →
Logs (`[contact] …`) → Brevo → Transactional → Logs.

## 11. Checklist

**Construcción**
- [ ] Clave de Brevo del proyecto recibida de ingeniería
- [ ] Adaptador de Vercel con la versión que corresponde a Astro
- [ ] `security.allowedDomains` con producción, `www` y URL temporal
- [ ] Ficheros copiados y adaptados; `form.config.ts` con `||`
- [ ] Widget de Turnstile con todos los dominios y `localhost`
- [ ] Variables en Vercel (Production y Preview), ninguna vacía, ninguna secreta con `PUBLIC_`
- [ ] Regla de firewall publicada con la IP de oficina en lista blanca
- [ ] Página `/gracias` con `noindex`
- [ ] Probado en la URL temporal: envío correcto, errores de validación, sin JavaScript, 429
- [ ] Privacidad menciona el formulario, Brevo y Cloudflare ([legales](legales.md))

**Publicación**
- [ ] `FORM_TO/CC/BCC` definitivos
- [ ] Dominio definitivo en Turnstile y `allowedDomains`
- [ ] Prueba real por formulario e idioma; el cliente confirma que le llega
- [ ] No cae en spam y aparece en el registro de Brevo
