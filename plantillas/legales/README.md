# Plantillas · Legales

Texto base de los tres documentos legales, igual en todas las webs, en español e
inglés. Cómo se usan, en qué fase y dónde acaban en el proyecto: en el
[flujo de legales](../../flujos/legales.md).

## Índice

| Fichero | Qué es |
| --- | --- |
| [`aviso-legal.es.md`](aviso-legal.es.md) · [`.en.md`](aviso-legal.en.md) | Aviso legal / Legal notice. 7 apartados |
| [`politica-privacidad.es.md`](politica-privacidad.es.md) · [`.en.md`](politica-privacidad.en.md) | Política de privacidad / Privacy policy. 10 apartados, con los tratamientos de la base ya redactados |
| [`politica-cookies.es.md`](politica-cookies.es.md) · [`.en.md`](politica-cookies.en.md) | Política de cookies / Cookies policy. 7 apartados, con la tabla de la base |
| [`variables.md`](variables.md) | Todas las variables: capa, de dónde salen, valor por defecto, legislación por país |
| [`parrafos-por-modulo.md`](parrafos-por-modulo.md) | Lo que se añade con embeds, idiomas, confirmación al usuario, newsletter, actividad regulada; y opcionales (stock, borrador, casilla) |
| [`../cliente/datos-legales.md`](../cliente/datos-legales.md) | La plantilla que se manda al cliente (solo capa 2) |

## Las tres capas en una plantilla

| Capa | En la plantilla |
| --- | --- |
| **1 · Texto base** | El texto corrido. Se copia tal cual |
| **2 · Datos del cliente** | `{{COMPANY_…}}`, `{{PRIVACY_EMAIL}}`, `{{DPO_CONTACT}}`… Salen de `entity.ts` |
| **3 · Datos de la web** | Bloques entre `CAPA 3 · DEDUCIDO` y `/CAPA 3`, ya redactados para la base, y `{{WEBSITE_URL}}`, fechas, legislación |

## Marcas

| Marca | Significa |
| --- | --- |
| `{{VARIABLE}}` | Se sustituye. Ver [`variables.md`](variables.md) |
| `<!-- CAPA 3 · DEDUCIDO … -->` … `<!-- /CAPA 3 -->` | Texto por defecto de la base. **Se revisa contra lo que la web hace** (código y navegador); si no coincide, se cambia el texto, no la web |
| `<!-- REVISAR: … -->` | Punto sin comprobar. No se publica como afirmación hasta resolverlo |
| `<!-- SI: condición -->` … `<!-- /SI -->` | Se incluye solo si se cumple. Si no, se quita (no se deja `[[ ]]`) |
| `<!-- MÓDULO … → parrafos-por-modulo.md -->` | Punto de inserción de un módulo |
| `<!-- OPCIONAL … -->` | Párrafo que no es módulo y se decide por proyecto |
| `**[ … ]**` | Botón (bloque `preferences`) |

Los comentarios son para Claude: **no llegan a la web**. Al pasar a
`src/data/legal/es.ts` se quitan o se convierten en comentarios de código donde
sigan siendo útiles.

## Qué no es esto

No es asesoramiento jurídico ni garantiza que una web cumpla la normativa. Es
una base de trabajo coherente con lo que hace el stack de Xana; cuando el
proyecto lo requiere, la revisa un jurista con los datos y la operativa reales.

## Fuentes

*Protocolo de publicación ESTÁNDAR* (estructura y variables) ·
clays-of-the-world (`docs/03-contenidos/legal/legal-copy-en.md`,
`src/data/legal/es.ts`, `src/components/CookieConsent.astro`) · APC Gestión
Patrimonial (`src/pages/aviso-legal.astro`, `politica-de-privacidad.astro`,
`politica-de-cookies.astro`) · magnaceramica (`src/pages/cookies-policy.astro`,
`privacy-policy.astro`: qué no hacer).
