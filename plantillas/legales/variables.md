# Legales · Variables

Todas las variables de las plantillas de legales, con su capa, de dónde salen y
qué se pone si no hay dato. Los nombres son los del *Protocolo de publicación
ESTÁNDAR*; las pocas que añade la guía están marcadas **(guía)**.

**Capas.** **Cliente** = capa 2, se pide con
[`../cliente/datos-legales.md`](../cliente/datos-legales.md). **Web** = capa 3,
la deduce Claude del proyecto. **General** = sale del país y la fecha.

**Si falta un dato:** capa cliente → `[[ ]]` visible (`value: null` o
`{ pending }` en código) y **no se publica**; nunca un valor plausible. Capa web
o general → lo resuelve Claude; si no puede comprobarlo, `<!-- REVISAR -->` y
se consulta con Belén.

## Capa 2 · Cliente

| Variable | Qué es | De dónde sale | Por defecto / si falta |
| --- | --- | --- | --- |
| `{{COMPANY_LEGAL_NAME}}` | Razón social con forma jurídica (S.L., S.A.) o nombre y apellidos si es autónomo. **No es la marca** | Cliente | `[[ ]]` |
| `{{COMPANY_TRADE_NAME}}` **(guía)** | Marca o nombre comercial con el que se presenta la web | Cliente; si no lo dice, el nombre que usa la web | Si coincide con la razón social, se usa la razón social y se quitan las frases `SI: hay nombre comercial` |
| `{{COMPANY_REGISTERED_ADDRESS}}` | Domicilio social completo, con código postal. Puede no ser la sede del pie | Cliente | `[[ ]]`. En clays se guarda por partes (`registeredAddress`) porque también lo usa el JSON-LD |
| `{{COMPANY_COUNTRY}}` | País del domicilio. Decide la legislación y la autoridad (capa general) | Cliente | `[[ ]]`. Es texto: se traduce (`entityText.country`) |
| `{{COMPANY_TAX_ID}}` | NIF / CIF o identificación fiscal del país | Cliente | `[[ ]]` |
| `{{COMPANY_REGISTRY_DETAILS}}` | Registro Mercantil, tomo, folio, hoja, inscripción | Cliente (escritura de constitución) | `[[ ]]` si es sociedad; fila fuera si es autónomo. Es texto: se traduce |
| `{{BUSINESS_ACTIVITY}}` | Actividad principal y, si los hay, registros, licencias o autorizaciones | Cliente | `[[ ]]`. Es texto: se traduce. En actividad regulada, obligatorio con su registro |
| `{{COMPANY_EMAIL}}` | Email publicado en contacto y pie | Cliente | `[[ ]]` |
| `{{COMPANY_PHONE}}` **(guía)** | Teléfono. Opcional si hay email | Cliente | Fila fuera |
| `{{PRIVACY_EMAIL}}` | Email para privacidad y ejercicio de derechos | Cliente | `{{COMPANY_EMAIL}}` **solo si el cliente confirma** que sirve el mismo (clays); si no, `[[ ]]` |
| `{{LEGAL_CONTACT_EMAIL}}` | Email de contacto del aviso legal | Cliente | Igual que el anterior: `{{COMPANY_EMAIL}}` si lo confirma |
| `{{DPO_CONTACT}}` | Contacto del delegado de protección de datos | Cliente | Si no tiene DPO, la fila se quita (no `[[ ]]`) |
| `{{RIGHTS_REQUEST_URL}}` | Formulario o página propia del cliente para ejercer derechos | Cliente | Casi nunca existe: se quita la frase `SI: hay RIGHTS_REQUEST_URL` |

## Capa 3 · Web

| Variable | Qué es | De dónde sale | Por defecto de la base |
| --- | --- | --- | --- |
| `{{WEBSITE_URL}}` | URL principal, sin barra final en el texto (`claysoftheworld.com`, no `https://…/`) | Dominio definitivo del proyecto (`astro.config.mjs` → `site`) | Si aún no hay dominio, `[[ ]]`: no se pone la URL temporal de Vercel |
| `{{LEGAL_NOTICE_URL}}` **(guía)** | Ruta del aviso legal | Idioma de la web | `/aviso-legal/` (ES) · `/legal-notice/` (EN) |
| `{{PRIVACY_POLICY_URL}}` | Ruta de la política de privacidad | Idioma de la web | `/politica-de-privacidad/` (ES) · `/privacy-policy/` (EN) |
| `{{COOKIE_POLICY_URL}}` | Ruta de la política de cookies | Idioma de la web | `/politica-de-cookies/` (ES) · `/cookies-policy/` (EN) |
| `{{PROCESSING_ACTIVITIES_TABLE}}` | Tratamientos: datos, finalidad, base, destinatarios, conservación | Código: formulario, muro, GTM, módulos | Redactado en `politica-privacidad.*.md` §2: formulario, seguridad, analítica |
| `{{RECIPIENT_CATEGORIES}}` | Proveedores y categorías de destinatarios | Código (`api/contact.ts`, `CookieConsent.astro`, hosting) | Vercel Inc. · Brevo (Sendinblue SAS) · Cloudflare, Inc. · Google Ireland Limited · herramientas del cliente por categoría |
| `{{INTERNATIONAL_TRANSFER_DETAILS}}` | Transferencias fuera del EEE | Proveedores de la lista anterior | Google LLC (Marco UE-EE. UU.), Cloudflare (garantías, sin afirmar Marco), Vercel (sede en EE. UU.; formulario procesado en región europea, decisión 15-09-2026) |
| `{{AUTOMATED_DECISION_INFO}}` | Decisiones automatizadas y perfiles | Lo que hace la web | «No se toman decisiones automatizadas ni se elaboran perfiles» |
| `{{COOKIE_INVENTORY_TABLE}}` | Tabla de cookies reales | Navegador (DevTools → Aplicación → Cookies) tras aceptar y tras rechazar, + `CookieConsent.astro` | `cc_cookie` (necesaria, 6 meses) · `_ga` (2 años) · `_ga_<ID>` (2 años) |
| `{{COOKIE_SETTINGS_CONTROL}}` | Cómo se acepta, rechaza y cambia | `CookieConsent.astro` y pie | Panel en la primera visita + «Preferencias de privacidad» en el pie + botón en §4 |
| `{{CURRENT_LEGAL_CONTENT}}` | Legales ya publicados en la web antigua | Web antigua del cliente | No se pinta en ningún texto: sirve para proponer datos de capa 2 **a confirmar** (ver flujo § Qué tenemos) |
| `{{ANALYTICS_RETENTION}}` **(guía)** | Plazo de conservación de los datos de GA4 | Propiedad de GA4 → Administrar → Retención de datos | «14 meses» (clays; **decisión 15-09-2026: siempre 14 meses**). **Hay que fijarlo en la propiedad**: si no se cambia, GA4 conserva menos y la política diría otra cosa |

## General · País y fecha

| Variable | Qué es | Por defecto |
| --- | --- | --- |
| `{{LEGAL_NOTICE_EFFECTIVE_DATE}}` | Fecha de la última revisión del **texto**, no del despliegue (clays `types.ts`) | Mes y año: «septiembre de 2026» / «September 2026» |
| `{{PRIVACY_EFFECTIVE_DATE}}` | Igual | Igual |
| `{{COOKIE_EFFECTIVE_DATE}}` | Igual. Cambia cada vez que cambia la tabla | Igual |
| `{{GOVERNING_LAW}}` | Legislación aplicable | Ver tabla por país |
| `{{DATA_PROTECTION_LAW}}` **(guía)** | Normativa de protección de datos que se cita en la entrada de privacidad | Ver tabla por país |
| `{{SUPERVISORY_AUTHORITY_NAME}}` | Autoridad de protección de datos | Ver tabla por país |
| `{{SUPERVISORY_AUTHORITY_URL}}` | Web de la autoridad | Ver tabla por país |

### Por país

| País | `GOVERNING_LAW` | `DATA_PROTECTION_LAW` | Autoridad |
| --- | --- | --- | --- |
| **España** (ES) | la legislación española | el Reglamento General de Protección de Datos (UE) 2016/679 (RGPD) y la Ley Orgánica 3/2018, de Protección de Datos Personales y garantía de los derechos digitales (LOPDGDD) | Agencia Española de Protección de Datos (AEPD) · `https://www.aepd.es` |
| **España** (EN) | Spanish law | the General Data Protection Regulation (EU) 2016/679 (GDPR) and Spanish Organic Law 3/2018 on the Protection of Personal Data and Guarantee of Digital Rights | Spanish Data Protection Agency (AEPD) · `https://www.aepd.es` |
| **Otro país** | `[[ ]]` | `[[ ]]` | `[[ ]]` |

Otro país no está en la base: ningún proyecto real lo ha tenido. Se dejan los
tres huecos, se avisa a Belén y se propone revisión jurídica.

## Variables de módulo

Las que solo aparecen al activar un módulo están en
[`parrafos-por-modulo.md`](parrafos-por-modulo.md), junto a su texto:
`{{EMBED_PROVIDER}}`, `{{EMBED_PROVIDER_LEGAL_NAME}}`, `{{EMBED_DESCRIPTION}}`, `{{NEWSLETTER_PROVIDER}}`,
`{{NEWSLETTER_DESCRIPTION}}`, `{{PREVAILING_LANGUAGE}}`,
`{{REGULATED_INFO_URL}}`.

## Fuentes

*Protocolo de publicación ESTÁNDAR* (nombres, capas y tabla de cookies
antigua) · clays-of-the-world (`src/data/legal/entity.ts`, `types.ts`, `es.ts`,
`CookieConsent.astro`, `docs/03-contenidos/legal/formulario-datos-titular.md`).
