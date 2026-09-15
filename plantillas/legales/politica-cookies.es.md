# Plantilla · Política de cookies (ES)

> Capa 1 con la **capa 3 por defecto de la base ya redactada**: muro
> `vanilla-cookieconsent` v3 (`cc_cookie`) y Google Analytics 4 cargado por
> Google Tag Manager solo tras aceptar. Leyenda en [`README.md`](README.md).
> Ruta: `/politica-de-cookies/`; la sección 4 lleva el ancla `#preferences`.
>
> Estructura del *Protocolo de publicación ESTÁNDAR* (qué son, qué cookies,
> consentimiento, terceros, contacto). Redacción y tabla de clays-of-the-world
> (`src/data/legal/es.ts`, `src/components/CookieConsent.astro`).

---

# Política de cookies

Última actualización: {{COOKIE_EFFECTIVE_DATE}}

{{COMPANY_LEGAL_NAME}} utiliza cookies y tecnologías similares en
{{WEBSITE_URL}}. Esta política explica qué son, cuáles se utilizan y cómo puede
gestionarlas. No se instala ninguna cookie que requiera consentimiento hasta que
usted la acepta.

## 1. Qué son las cookies

Las cookies y tecnologías similares permiten almacenar información en el
dispositivo del usuario o acceder a la que ya está almacenada. Una cookie es un
pequeño archivo que se descarga en su dispositivo al visitar determinadas
páginas web; junto a ellas pueden utilizarse, cuando corresponda, tecnologías
equivalentes como el almacenamiento local.

## 2. Tipos de cookies según su finalidad

<!-- Las cinco categorías del protocolo. CAPA 3 · DEDUCIDO: la frase final de
     cada una dice si la web la usa. Con el módulo de embeds, «Terceros» pasa a
     «Se utilizan solo con su consentimiento» (parrafos-por-modulo.md). -->

- **Necesarias.** Imprescindibles para el funcionamiento del sitio. No requieren
  consentimiento. Se utilizan en esta web.
- **Preferencias.** Recuerdan determinadas elecciones del usuario. No se
  utilizan en esta web.
- **Analíticas.** Permiten medir el uso del sitio para mejorarlo. Se utilizan
  en esta web solo con su consentimiento.
- **Publicidad.** Permiten medir o personalizar publicidad. No se utilizan en
  esta web.
- **Terceros.** Las utilizan servicios externos integrados en el sitio, como
  vídeos o mapas. No se utilizan en esta web.

## 3. Cookies que utiliza esta web

<!-- {{COOKIE_INVENTORY_TABLE}} · CAPA 3 · DEDUCIDO de la base.
     - `cc_cookie`: nombre y 182 días en `CookieConsent.astro`
       (`cookie: { name: 'cc_cookie', expiresAfterDays: 182 }`) = «6 meses».
     - `_ga` y `_ga_<ID>`: lo que pone GA4.
     - NO se declaran `_gid`, `_gat` (Universal Analytics: GA4 no los pone) ni
       `PHPSESSID` (sesión PHP: una web Astro estática no la tiene), aunque
       figuren en la tabla del protocolo.
     - Vercel y Turnstile no ponen cookies (clays).
     Comprobar en el navegador antes de publicar (flujos/legales.md § Checklist).
     Proveedor de `cc_cookie`: la marca del titular (cookie propia). -->

Esta web utiliza una cookie propia estrictamente necesaria, para recordar su
elección sobre las cookies, y cookies de Google Analytics, que solo se instalan
si usted las acepta. No utiliza cookies publicitarias ni de redes sociales y no
incrusta contenido de terceros que las instale. El alojamiento del sitio y la
verificación de seguridad del formulario de contacto no instalan cookies.

| Cookie | Proveedor | Categoría | Finalidad | Duración |
| --- | --- | --- | --- | --- |
| `cc_cookie` | {{COMPANY_TRADE_NAME}} (propia) | Necesaria | Guarda su elección sobre las cookies para no mostrar el panel en cada visita. | 6 meses |
| `_ga` | Google Analytics | Analítica | Distingue a los usuarios únicos, para contar una sola vez a quien vuelve. | 2 años |
| `_ga_<ID>` | Google Analytics | Analítica | Mantiene el estado de la sesión en curso. | 2 años |

La tabla se actualiza cada vez que se añaden al sitio vídeos incrustados, mapas,
píxeles u otras herramientas que instalen cookies.

<!-- /CAPA 3 · COOKIE_INVENTORY_TABLE -->

## 4. Consentimiento y preferencias de privacidad

<!-- id="preferences": ancla del enlace «Preferencias de privacidad» del pie
     cuando no hay JavaScript. -->

<!-- {{COOKIE_SETTINGS_CONTROL}} · CAPA 3 · DEDUCIDO del muro de la base:
     aceptar / rechazar / elegir, enlace del pie con
     data-cc="show-preferencesModal", botón de esta sección, borrado de `_ga*`
     y recarga al retirar (`autoClear` con `reloadPage: true`). -->

Las cookies que requieren consentimiento permanecen desactivadas hasta que usted
las acepta. En su primera visita, un panel le permite aceptarlas, rechazarlas o
elegir por categorías. Rechazar es tan sencillo como aceptar.

Puede cambiar o retirar su consentimiento en cualquier momento desde el enlace
«Preferencias de privacidad», al pie de todas las páginas, o con el botón de
abajo. Si lo retira, se eliminan las cookies de analítica ya instaladas y la
página se recarga.

**[ Gestionar preferencias de cookies ]** <!-- Bloque `preferences`: enlace a
#preferences con data-cc="show-preferencesModal". -->

<!-- /CAPA 3 · COOKIE_SETTINGS_CONTROL -->

## 5. Terceros y transferencias

Algunas de estas tecnologías pertenecen a terceros —en esta web, Google
Analytics— y, en determinados casos, pueden implicar transferencias
internacionales de datos. Encontrará más información en la
[Política de privacidad]({{PRIVACY_POLICY_URL}}).

## 6. Gestionar las cookies desde el navegador

También puede gestionar o bloquear las cookies desde la configuración de su
navegador. Chrome, Firefox, Safari, Edge y otros ofrecen esta opción en su
apartado de privacidad. Desactivar determinadas cookies puede afectar al
funcionamiento de algunas partes del sitio.

## 7. Actualizaciones y contacto

Esta política puede actualizarse cuando cambien las cookies utilizadas o la
normativa aplicable. La fecha de la última revisión figura al principio de esta
página.

Para cualquier consulta sobre cookies puede escribir a {{PRIVACY_EMAIL}}.
