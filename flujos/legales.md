# Flujo · Legales

Aviso legal, política de privacidad y política de cookies: qué se pide, cómo se
redacta, dónde se monta y qué se comprueba. El muro de cookies y GTM tienen su
propio flujo: [cookies y analítica](cookies-analitica.md).

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Se manda al cliente la plantilla de datos legales |
| [07 · Construcción](../fases/07-construccion.md) | Se montan las tres páginas con capa 1 + 2 + 3 |
| [08 · Revisión](../fases/08-revision.md) | Se comprueba que no falta nada y que los textos cuentan lo que la web hace |

Va en **todas las webs**. Se vuelve a abrir cada vez que cambia lo que la web
hace: un vídeo incrustado, una newsletter, una etiqueta nueva en GTM. **Primero
el texto, luego el cambio.**

### Las tres capas

| Capa | Qué es | De dónde sale |
| --- | --- | --- |
| **1 · Texto base** | Aviso legal, privacidad, cookies. Igual en todas las webs, en español e inglés | [`../plantillas/legales/`](../plantillas/legales/README.md) |
| **2 · Datos del cliente** | Razón social, marca, NIF, domicilio, país, registro, actividad, emails, DPO | [`../plantillas/cliente/datos-legales.md`](../plantillas/cliente/datos-legales.md). Si falta, `[[ ]]` |
| **3 · Datos de la web** | URLs, fechas, legislación y autoridad por país, tratamientos, proveedores, tabla de cookies | Los deduce Claude del código y del navegador. La base ya viene redactada en las plantillas |

Más los **párrafos por módulo** (embeds, idiomas, confirmación al usuario,
newsletter, actividad regulada):
[`parrafos-por-modulo.md`](../plantillas/legales/parrafos-por-modulo.md).

## Qué tenemos (escenarios)

**🟢 El cliente manda todos los datos.** Se vuelcan en `entity.ts` tal como
llegan (clays, 14-09-2026). Antes, dos comprobaciones: que la razón social no
es la marca y que el domicilio es el social, no el de la tienda.

**🟡 Manda una parte.** Lo que llega, a `entity.ts`; lo que no, `value: null`
con su `pending`. Las páginas se montan igual y enseñan `[[ ]]` en la URL
temporal. Lo que falta se apunta en `CHECKLIST.md` y Belén lo reclama. **No se
publica en el dominio con un `[[ ]]`.**

**🔴 No manda nada.** Se montan las tres páginas completas con capa 1 y 3, y
toda la capa 2 en `[[ ]]`. El resto de la web avanza; los legales bloquean solo
la publicación.

**Web antigua con legales** (`{{CURRENT_LEGAL_CONTENT}}`). Se leen, pero:

- Los **datos del titular** sirven para pre-rellenar la plantilla del cliente,
  marcados «¿sigue vigente?». No se publican sin su confirmación: en clays el
  domicilio del BORME ya no era el vigente.
- Los **tratamientos, proveedores y cookies no se copian nunca**: describen otra
  web. Es el fallo de magnaceramica (declara `_gid`, `_gat`, Google Maps y
  Mailchimp, que la web no usa, y omite Brevo, Cloudflare y Vercel).

**La asesoría del cliente manda sus propios textos.** Su redacción manda. Claude
los contrasta con la capa 3 (proveedores, cookies, formulario) y pasa a Belén
las diferencias antes de publicar; no las corrige por su cuenta.

## Pasos y quién

### 00 · Arranque

1. **Claude** apunta qué módulos tocan los legales según el briefing (embeds,
   idiomas, confirmación, newsletter, actividad regulada) y el país del cliente.
2. **Claude** copia [`datos-legales.md`](../plantillas/cliente/datos-legales.md)
   a `docs/03-contenidos/legal/`, pone el nombre del cliente y pre-rellena lo
   que ya se sepa, marcado para confirmar.
3. **Belén** lo revisa y lo envía al cliente, junto con las demás plantillas.
4. **Cliente** lo devuelve. Si hay asesoría o DPO, lo dice ahí.

### 07 · Construcción

1. **Claude** trae el montaje: `src/data/legal/`, `LegalDocument.astro`,
   `LegalText.astro`, `Pending.astro`, `Legal.astro` y las tres páginas (origen:
   clays-of-the-world; ver [Dónde queda cada cosa](#dónde-queda-cada-cosa)).
2. **Claude** vuelca la capa 2 en `entity.ts`. Lo que falta, `null`.
3. **Claude** pasa las plantillas a `src/data/legal/es.ts` (y `en.ts` si hay
   inglés): capa 1 literal, variables de capa 3 y generales resueltas
   ([`variables.md`](../plantillas/legales/variables.md)), bloques
   `CAPA 3 · DEDUCIDO` **revisados contra el código**, módulos añadidos,
   comentarios fuera.
4. **Claude** comprueba que la tabla de cookies y los textos del muro dicen lo
   mismo que `CookieConsent.astro` (nombre y días de `cc_cookie`, categorías).
5. **Claude** monta el pie con los 4 elementos, `noindex, follow` y las páginas
   fuera del sitemap.
6. **Belén** decide si se pone el aviso de borrador (`draftNotice`) y si el
   proyecto requiere revisión jurídica.
7. Si hay revisión jurídica, **Claude** saca la prosa a
   `docs/03-contenidos/legal/legal-copy-es.md` (como `legal-copy-en.md` de
   clays) y **Belén** la envía. Lo que vuelva se aplica en los datos; si prosa y
   código se separan, **manda el código**.

### 08 · Revisión

1. **Claude** pasa la [checklist](#checklist) en la URL temporal, con el
   navegador: cookies tras aceptar y tras rechazar, enlaces, sin JavaScript.
2. **Belén** valida. Con un `[[ ]]` o un `REVISAR` abierto, no se publica.
3. En [09](../fases/09-publicacion.md), con el GTM de producción, se repite la
   comprobación de cookies ([cookies y analítica](cookies-analitica.md)).

## Plantillas

| Plantilla | Para | Fase |
| --- | --- | --- |
| [`cliente/datos-legales.md`](../plantillas/cliente/datos-legales.md) | Cliente: capa 2 en lenguaje claro | 00 |
| [`legales/aviso-legal.es.md`](../plantillas/legales/aviso-legal.es.md) · [`.en.md`](../plantillas/legales/aviso-legal.en.md) | Texto base | 07 |
| [`legales/politica-privacidad.es.md`](../plantillas/legales/politica-privacidad.es.md) · [`.en.md`](../plantillas/legales/politica-privacidad.en.md) | Texto base + tratamientos de la base | 07 |
| [`legales/politica-cookies.es.md`](../plantillas/legales/politica-cookies.es.md) · [`.en.md`](../plantillas/legales/politica-cookies.en.md) | Texto base + tabla de la base | 07 |
| [`legales/variables.md`](../plantillas/legales/variables.md) | Qué va en cada `{{ }}` | 07 |
| [`legales/parrafos-por-modulo.md`](../plantillas/legales/parrafos-por-modulo.md) | Textos por módulo, aviso de borrador, casilla | 07 |

## Dónde queda cada cosa

```
docs/03-contenidos/legal/
├── README.md             ← cómo están montados en este proyecto y qué falta
├── datos-legales.md      ← la plantilla enviada al cliente (y devuelta)
└── legal-copy-es.md      ← prosa para revisión jurídica, solo si la hay

src/data/legal/
├── entity.ts             ← capa 2. Lo único que sale del cliente. No se traduce
├── types.ts              ← los bloques con los que se escribe un documento
├── es.ts  (en.ts…)       ← capa 1 + 3 y textos del muro, uno por idioma
└── index.ts              ← getLegal, legalEntity, legalDocument, legalLinks (pie)

src/components/
├── LegalDocument.astro   ← pinta cualquiera de los tres documentos
├── LegalText.astro       ← párrafo con enlaces y huecos
├── Pending.astro         ← la marca [[ ]]
├── CookieConsent.astro   ← muro (flujo de cookies y analítica)
└── Footer.astro          ← los 4 elementos

src/layouts/Legal.astro   ← título, descripción, robots, composición

src/pages/
├── aviso-legal.astro
├── politica-de-privacidad.astro
└── politica-de-cookies.astro
```

Fuente: clays-of-the-world (`src/data/legal/`, `src/components/`,
`src/layouts/Legal.astro`, `docs/03-contenidos/legal/README.md`).

### `entity.ts`: un dato, un sitio

Ni un dato del titular se escribe en el marcado ni en los textos: el bloque
`identity` lo pinta desde aquí en el aviso legal y en privacidad. `null` es
«no ha llegado» (clays `src/data/legal/entity.ts`, `types.ts`):

```ts
export const entity: Pick<LegalEntity, 'legalName' | 'taxId' | 'email' | 'phone'> = {
  legalName: { value: null, pending: 'razón social con forma jurídica' },
  taxId: { value: null, pending: 'NIF' },
  email: { value: 'info@clays.es', href: 'mailto:info@clays.es' },
  phone: { value: '+34 687 24 69 65', href: 'tel:+34687246965' },
};
```

País, registro y actividad son texto y van en `entityText` de cada idioma;
`legalEntity(lang)` los junta. Clays solo tiene un email: si el cliente da
correo de privacidad propio o DPO, se añaden `privacyEmail` y `dpo` a
`EntityField` en `types.ts`.

### De la plantilla a los bloques

| En la plantilla | En `es.ts` |
| --- | --- |
| Tabla del titular / responsable | `{ kind: 'identity' }` |
| Párrafo | `{ kind: 'paragraph', text }`; con enlaces, array con `{ text, href }` |
| Lista de derechos, destinatarios | `{ kind: 'list', items }` |
| Tipos de cookies; tratamientos con título en negrita | `{ kind: 'terms', items: [{ term, text }] }` |
| Tabla de cookies | `{ kind: 'table', head, rows }` |
| `**[ Gestionar preferencias de cookies ]**` | `{ kind: 'preferences' }` en la sección con `id: 'preferences'` |
| Dato de capa 2 que falta dentro de un párrafo | `{ pending: 'qué falta' }` |

Si un texto pide algo que no encaja, se añade un bloque en `types.ts` y se pinta
en `LegalDocument.astro`: no se cuela marcado en los datos.

### Las páginas

Solo nombran su documento. Para una web en español con una sola lengua
(adaptado de clays `src/pages/[...lang]/legal-notice.astro`, que además genera
una ruta por idioma con `getStaticPaths`):

```astro
---
import Legal from '../layouts/Legal.astro';
---

<Legal slug="aviso-legal" lang="es" />
```

Rutas: `/aviso-legal/`, `/politica-de-privacidad/`, `/politica-de-cookies/`
en español; `/legal-notice/`, `/privacy-policy/`, `/cookies-policy/` en inglés.
El `slug` del documento es la ruta.

### Webs con varios idiomas

Decidido el 15-09-2026:

- **Los tres documentos se traducen, y también su URL**: en cada idioma, la
  ruta en ese idioma (`/aviso-legal/` y `/en/legal-notice/`). Las rutas salen
  de una tabla de nombres por idioma → [idiomas](../modulos/idiomas.md#dos-montajes).
- **Excepción, solo si el cliente lo especifica:** la política de cookies y el
  aviso legal pueden quedar en un solo idioma. Se anota en `CHECKLIST.md` que
  falta la traducción y en qué idiomas.
- La política de privacidad va siempre en todos los idiomas de la web.

### Robots y pie

En `Legal.astro`, `robots="noindex, follow"`, y las tres páginas fuera de
`sitemap.xml.ts`. El pie lleva siempre **Aviso legal · Política de privacidad ·
Política de cookies · Preferencias de privacidad**. El último es un enlace que
con JavaScript abre el panel y sin JavaScript lleva a la explicación (clays
`Footer.astro`, `legalLinks()` en `index.ts`):

```astro
<a href="/politica-de-cookies/#preferences" data-cc="show-preferencesModal">
  Preferencias de privacidad
</a>
```

## Reglas y trampas

- **La política cuenta lo que la web hace, ni más ni menos.** Cada proveedor y
  cada cookie del texto se encuentra en el código o en el navegador, y al revés.
  Magnaceramica declara lo que no usa y calla lo que usa.
- **No se inventan datos.** Un NIF o un domicilio plausibles en un aviso legal
  son una declaración falsa. Registros públicos y web antigua sirven para
  contrastar y proponer, no para publicar.
- **Razón social ≠ marca; domicilio social ≠ sede del pie** (clays `entity.ts`).
- **`_gid`, `_gat` y `PHPSESSID` no se declaran.** Están en la tabla del
  protocolo, pero `_gid` y `_gat` son de Universal Analytics (GA4 no los pone)
  y `PHPSESSID` es de una sesión PHP. El muro borra igualmente `_gid` y `_gat`
  (clays `CookieConsent.astro`).
- **Lo que se añade en GTM entra antes en la política.** Una etiqueta de
  Google Ads o un píxel ponen cookies que el texto no declara; si son
  publicitarias, además hace falta una categoría en el muro.
- **Retención de GA4.** La política dice `{{ANALYTICS_RETENTION}}` (14 meses en
  clays); hay que fijarlo en la propiedad de GA4 o el texto no es verdad.
- **Turnstile manda la IP a Cloudflare** (`remoteip` en `api/contact.ts`): por
  eso la base declara el tratamiento de seguridad, que clays no tenía.
- **`noindex, follow`, no `nofollow`.** Clays tiene `nofollow` por indicación
  puntual de SEO: está en [deuda](../anexos/deuda-proyectos.md).
- **La casilla de privacidad la comprueba el servidor**, no solo el navegador
  (clays `api/contact.ts`; en deuda en magnaceramica).
- **Enlaces del mismo idioma.** Un enlace a privacidad desde el aviso legal en
  inglés lleva a la inglesa (clays `es.ts` con prefijo `/es/`).
- **Fecha de revisión, no de despliegue.** Se escribe a mano y cambia cuando
  cambia el texto.
- **No es asesoramiento jurídico.** Ni en la web ni en la entrega al cliente se
  afirma que la web «cumple el RGPD» o está «100 % legal». Se dice que los textos
  se han preparado sobre una base estándar y lo que hace la web, y que la
  revisión jurídica corresponde a un profesional cuando el proyecto la requiere.
- **Aviso de borrador**: se quita del dato (`draftNotice`), nunca del
  componente. Clays lo retiró sin revisión jurídica por decisión de Xana; en
  otro proyecto la recomendación es mantenerlo hasta la revisión si la hay.
- **País distinto de España:** fuera de la base; legislación y autoridad en
  `[[ ]]`, aviso a Belén y revisión jurídica.

## Checklist

**00 · Arranque**

- [ ] Módulos que afectan a legales anotados en `CHECKLIST.md`
- [ ] `datos-legales.md` preparado, pre-rellenado solo con datos a confirmar
- [ ] Enviado por Belén

**07 · Construcción**

- [ ] `entity.ts` con lo recibido; lo que falta, `null` con `pending`
- [ ] Tres documentos en cada idioma de la web, frase a frase
- [ ] Campos del formulario de privacidad §2 = campos de `Contact.astro`
- [ ] Proveedores de privacidad §3 = servicios externos del código (buscar
      `googletagmanager`, `challenges.cloudflare.com`, `api.brevo.com`,
      `vimeo`, `youtube`, `google.com/maps`)
- [ ] Párrafos de módulo añadidos solo si el módulo está montado
- [ ] Fechas, legislación y autoridad según país
- [ ] `robots` `noindex, follow`; páginas fuera del sitemap
- [ ] Pie con los 4 elementos; «Preferencias de privacidad» con `data-cc` y
      `href` a `#preferences`
- [ ] Decidido con Belén: aviso de borrador sí/no, revisión jurídica sí/no

**08 · Revisión**

- [ ] Ningún `[[ ]]` ni `REVISAR` en las tres páginas
- [ ] Ventana privada, **rechazar**: solo `cc_cookie`
- [ ] Ventana privada, **aceptar**: `cc_cookie`, `_ga`, `_ga_<ID>` y nada que
      no esté en la tabla
- [ ] Retirar el consentimiento borra `_ga*` y recarga
- [ ] Sin JavaScript, «Preferencias de privacidad» lleva a
      `/politica-de-cookies/#preferences`
- [ ] Enlaces entre documentos van al mismo idioma y funcionan
- [ ] Duración de `cc_cookie` en la tabla = `expiresAfterDays` del muro
- [ ] Retención de GA4 configurada = la de la política
- [ ] Contenedor de GTM: solo GA4
- [ ] Si hubo revisión jurídica, sus cambios aplicados y el aviso de borrador
      retirado

## Fuentes

*Protocolo de publicación ESTÁNDAR* (estructura, variables, 4 elementos del pie,
tabla de cookies antigua) · clays-of-the-world (`src/data/legal/*`,
`LegalDocument.astro`, `Legal.astro`, `CookieConsent.astro`, `Footer.astro`,
`api/contact.ts`, `docs/03-contenidos/legal/`) · APC Gestión Patrimonial
(`LegalDoc.astro`, legales en español, `informacion-financiera.astro`) ·
magnaceramica (`src/pages/privacy-policy.astro`, `cookies-policy.astro`: qué no
hacer).
