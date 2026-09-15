# Flujo · SEO técnico

Lo que necesitan buscadores y redes para leer la web: metadatos, canonical,
Open Graph, sitemap, `robots.txt`, datos estructurados, qué se indexa y qué no,
titulares, URLs, 404 y redirecciones. **Va en todas las webs.**

Los textos (título, descripción, H1, palabras clave) **no se escriben aquí**:
los manda Eva y quedan en `docs/03-contenidos/seo-metadatos.md` →
[contenidos y SEO](contenidos-seo.md) ·
[petición a Eva](../plantillas/equipo/peticion-seo.md). Este flujo los lleva al
código. Implementación de referencia: `clays-of-the-world/src/layouts/Base.astro`.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [03 · Contenidos y SEO](../fases/03-contenidos-seo.md) | Llega `seo-metadatos.md` (título, descripción, H1 por página e idioma; URLs; redirecciones) |
| [07 · Construcción](../fases/07-construccion.md) | `site`, `Base.astro`, OG, sitemap, robots, JSON-LD, `noindex`, 404, redirecciones |
| [08 · Revisión](../fases/08-revision.md) | Comprobar sobre el build y la URL temporal ([revisión §5 SEO](revision.md)) |
| [09 · Publicación](../fases/09-publicacion.md) | `site` definitivo, robots y sitemap en el dominio, Search Console ([publicación §5.1](publicacion.md)) |

## 2. Qué tenemos

| Situación | Qué cambia |
| --- | --- |
| Una página (landing) | Sitemap propio con `/` y poco más; JSON-LD `Organization` en la portada |
| Multipágina | Título y descripción por página; `BreadcrumbList` por página → módulo [multipágina](../modulos/multipagina.md) |
| Varios idiomas | `hreflang` + `x-default` en `<head>` y sitemap, `og:locale` por idioma → módulo [idiomas](../modulos/idiomas.md) |
| Web antigua en el dominio | Redirecciones 301 → módulo [migración](../modulos/migracion.md) |
| Eva no ha mandado aún los metadatos | Se monta con marcas `[[ ]]` y **no se publica** |
| Evento, producto de software, servicio profesional | JSON-LD de su tipo (§3, paso 5) |

## 3. Pasos y quién

### 1. `site` — Claude

```js
// clays-of-the-world/astro.config.mjs
export default defineConfig({
  site: 'https://claysoftheworld.com',
});
```

De `site` salen canonical, `og:url`, `og:image`, sitemap y `robots.txt`. **Con
el dominio definitivo y la versión elegida (con o sin `www`)** desde el
principio; se confirma antes de publicar ([publicación §3](publicacion.md)).
Sin `site`, `Astro.site` es `undefined` y todo lo absoluto se rompe (magnaceramica
y festival-viento no lo tienen, [deuda](../anexos/deuda-proyectos.md)).

### 2. `Base.astro` — Claude

Un solo layout con las props de SEO; cada página pasa las suyas desde los datos.

```astro
---
// clays-of-the-world/src/layouts/Base.astro (resumido)
interface Props {
  title: string;
  description?: string;
  lang?: Lang;
  robots?: string;      // 'noindex, follow'… Sin ella, indexable
}
const { title, lang = DEFAULT_LANG, robots } = Astro.props;
const description = Astro.props.description ?? content.meta.description;
const canonical = new URL(Astro.url.pathname, Astro.site);
// Absoluta: las redes no resuelven rutas relativas
const ogImage = new URL('/og-image.jpg', Astro.site);
---
<html lang={lang}>
<head>
  <title>{title}</title>
  <meta name="description" content={description} />
  <link rel="canonical" href={canonical} />
  {robots && <meta name="robots" content={robots} />}

  <link rel="icon" href="/favicon.ico" sizes="32x32" />
  <link rel="icon" href="/favicon.svg" type="image/svg+xml" />
  <link rel="apple-touch-icon" href="/apple-touch-icon.png" />

  <meta property="og:type" content="website" />
  <meta property="og:title" content={title} />
  <meta property="og:description" content={description} />
  <meta property="og:url" content={canonical} />
  <meta property="og:locale" content={t.ogLocale} />       <!-- es_ES, en_US -->
  <meta property="og:site_name" content={content.meta.siteName} />
  <meta property="og:image" content={ogImage} />
  <meta property="og:image:width" content="1200" />
  <meta property="og:image:height" content="630" />
  <meta property="og:image:alt" content={content.meta.imageAlt} />
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="theme-color" content="#FFFFFF" />

  <slot name="head" />   <!-- JSON-LD de cada página -->
</head>
```

- **Título y descripción** de `seo-metadatos.md`, **en los datos** (`meta` de
  `src/data/<idioma>.ts` en clays), no escritos en el componente.
- **Canonical** con `new URL(Astro.url.pathname, Astro.site)`: sin parámetros
  de consulta y con la barra final con que se sirve la página.
- **`robots`** como texto (`'noindex, follow'`) en vez de un booleano: clays
  necesita valores distintos por tipo de página (§3, paso 6). Con booleano
  (`noindex` en xanatechnologies y xanasystem-web) solo cabe un valor.
- **Favicons**: `.ico` primero con `sizes`, luego SVG; los que entienden SVG lo
  eligen igual. Ficheros → [imágenes §3 paso 7](imagenes.md).
- **`theme-color`** del fondo de la cabecera. Con modo oscuro, dos con `media`
  (`xanasystem-web/src/layouts/BaseLayout.astro`).
- **Twitter/X** lee las `og:`; basta `twitter:card`. xanatechnologies repite
  `twitter:title/description/image`: no hace daño, no hace falta.
- **`og:image`**: 1200×630, URL absoluta, con `width`, `height` y `alt`.
  xanasystem-web la hace absoluta pero sin medidas; APC no tiene `og:image`;
  festival-viento la pone relativa (`/img/cartel.jpg`) y las redes no la
  encuentran ([deuda](../anexos/deuda-proyectos.md)).
- Idiomas: `hreflang` por alternativa, `x-default` y `og:locale:alternate`
  solo en páginas indexables y traducidas (clays `alternativas`) →
  [idiomas](../modulos/idiomas.md).

### 3. Sitemap — Claude

Solo **páginas indexables**. Una página con `noindex` en el sitemap es una
señal contradictoria. Dos formas:

| | Endpoint propio `src/pages/sitemap.xml.ts` | Integración `@astrojs/sitemap` |
| --- | --- | --- |
| Proyecto | clays-of-the-world | xanatechnologies, xanasystem-web |
| URL | **`/sitemap.xml`** | `/sitemap-index.xml` + `/sitemap-0.xml` |
| Qué lista | Lo que se escribe a mano en `PAGINAS` | Todo `src/pages`, menos lo que quite `filter` |
| `hreflang` | Sí, desde el selector de idioma | Con la opción `i18n` y la `i18n` de Astro |
| Riesgo | Olvidar añadir una página nueva | Colar una página `noindex` si no está en el `filter` |
| **Cuándo** | **Por defecto**: one-page o pocas páginas, idiomas sin la `i18n` de Astro (`[...lang]`) | Muchas páginas o blog, con `i18n` de Astro |

Endpoint propio (clays):

```ts
// clays-of-the-world/src/pages/sitemap.xml.ts (resumido)
export const prerender = true;
const PAGINAS = ['/', '/gallery/'];   // solo indexables, con barra final

export const GET: APIRoute = ({ site }) => {
  // por cada idioma con ruta × página: <loc> + <xhtml:link hreflang> de
  // todas las versiones + x-default
  return new Response(xml, {
    headers: { 'Content-Type': 'application/xml; charset=utf-8' },
  });
};
```

Integración (xanatechnologies):

```js
// xanatechnologies/astro.config.mjs
const NOINDEX = ['/v2/', '/es/v2/', '/legal-notice/', '/privacy-policy/',
  '/cookies-policy/', '/es/aviso-legal/', '/es/politica-de-privacidad/',
  '/es/politica-de-cookies/'];

integrations: [
  sitemap({
    filter: (page) => !NOINDEX.some((p) => page.endsWith(p)),
    i18n: { defaultLocale: 'en', locales: { en: 'en', es: 'es' } },
  }),
],
```

Con la integración, **`sitemap-index.xml` es lo que se envía a Search Console**
y a lo que apunta `robots.txt`. `xanatechnologies/scripts/format-sitemap.mjs`
(tras `astro build`) solo lo indenta para leerlo; no cambia el XML.

La lista de `noindex` del `filter` y los `robots` de las páginas tienen que
decir lo mismo: xanasystem-web saca del sitemap `/styleguide` y `/demo-tabs`
pero esas páginas **no llevan `noindex`**.

### 4. `robots.txt` — Claude

**Generado**, para que la URL del sitemap salga de `site`:

```ts
// clays-of-the-world/src/pages/robots.txt.ts
export const GET: APIRoute = ({ site }) =>
  new Response(
    ['User-agent: *', 'Allow: /', '', `Sitemap: ${new URL('sitemap.xml', site).href}`, ''].join('\n'),
    { headers: { 'Content-Type': 'text/plain; charset=utf-8' } }
  );
```

- **Sin `Disallow` de páginas con `noindex`**: si el robot no entra, no lee el
  `noindex` y la URL puede indexarse igual
  ([trampas](../trampas-y-lecciones.md#astro-y-build)).
  `xanatechnologies/public/robots.txt` bloquea `/v2/` y `/es/v2/`, que llevan
  `noindex`: es justo lo que no hay que hacer.
- **Sí puede bloquear `/api/`**: no son páginas, no tienen `noindex` que leer
  (`Disallow: /api/` en xanatechnologies).
- Con la integración de sitemap, `Sitemap:` apunta a `sitemap-index.xml`.
- Las previews de Vercel ya llevan `X-Robots-Tag: noindex`; `robots.txt` solo
  cuenta en producción.

### 5. Datos estructurados (JSON-LD) — Claude

Siempre incrustados así, **desde datos que ya existen** y **sin inventar**:

```astro
<script type="application/ld+json" set:html={JSON.stringify(schema)} />
```

En clays va con `is:inline slot="head"` para que caiga en el `<head>` de
`Base`. Varias piezas → varios `<script>`, uno por tipo.

| Tipo | Dónde | Fuente | Regla |
| --- | --- | --- | --- |
| `Organization` | Portada | clays `[...lang]/index.astro` | Datos de `entity.ts` (razón social, NIF, correo, teléfono, domicilio). Campos a `null` fuera con el `replacer`; `sameAs` sin perfiles `placeholder` |
| `WebSite` | Layout, una vez | xanatechnologies `Base.astro` | `name`, `url`, `inLanguage` |
| `BreadcrumbList` | Cada página interior | xanasystem-web `pim.astro`, APC `gestion-patrimonial.astro` | URLs absolutas desde `site`, no `site.url + '/'` a mano |
| `FAQPage` | Página con preguntas | APC `gestion-patrimonial.astro`, xanasystem-web `FAQAccordion.astro` | **Del mismo array que pinta la FAQ**: lo que dice el marcado está en pantalla |
| `SoftwareApplication` / `Product` | Página de producto | xanasystem-web `pim.astro` | **Sin `Offer` si no hay precio público** |
| `Event` / `Festival` | Web de evento | festival-viento `Base.astro` | Fechas ISO, lugar, `isAccessibleForFree` solo si es verdad; `image` absoluta |
| `ProfessionalService` + `Person` | Servicio de una persona | APC `BaseLayout.astro`, `sobre-apc.astro` | `founder` con `sameAs` a su perfil real |

**`Organization` sin datos que falten** (clays):

```ts
// clays-of-the-world/src/pages/[...lang]/index.astro
const organization = {
  '@context': 'https://schema.org',
  '@type': 'Organization',
  name: content.meta.siteName,
  legalName: entity.legalName.value,
  url: Astro.site?.href,
  email: entity.email.value,
  telephone: entity.phone.value,
  taxID: entity.taxId.value,
  sameAs: content.social.links.filter((link) => !link.placeholder).map((link) => link.href),
  address: { '@type': 'PostalAddress', streetAddress: registeredAddress.street /* … */ },
};
// set:html={JSON.stringify(organization, (_clave, valor) => valor ?? undefined)}
```

**`FAQPage` del mismo array** (APC):

```astro
---
// APC-gestion-patrimonial/src/pages/gestion-patrimonial.astro
const faqs = [{ q: "¿Para quién están pensados estos servicios?", a: "…" } /* … */];
const faqLd = {
  "@context": "https://schema.org",
  "@type": "FAQPage",
  mainEntity: faqs.map((f) => ({
    "@type": "Question",
    name: f.q,
    acceptedAnswer: { "@type": "Answer", text: f.a },
  })),
};
---
<Faq faqs={faqs} />
<script type="application/ld+json" set:html={JSON.stringify(faqLd)} />
```

**Qué no hacer: `Offer` falso** (xanasystem-web, [deuda](../anexos/deuda-proyectos.md)):

```ts
// xanasystem-web/src/pages/pim.astro — declara el software gratis y en stock
offers: { '@type': 'Offer', availability: 'https://schema.org/InStock', price: '0', priceCurrency: 'EUR' },
```

Sin precio público, `SoftwareApplication` y `Product` van **sin `offers`**.

### 6. Qué se indexa y qué no — Claude

| Página | `robots` | En sitemap | Fuente |
| --- | --- | --- | --- |
| Portada y páginas de contenido | — (indexable) | Sí | — |
| Legales | **`noindex, follow`** | No | [legales](legales.md) · `xanatechnologies/src/layouts/Legal.astro` · `magnaceramica/src/layouts/LegalLayout.astro` |
| Gracias | `noindex` | No | [formulario](formulario.md) · clays `thank-you.astro` |
| 404 | `noindex` | No | clays `404.astro` · xanasystem-web `404.astro` |
| Styleguide | `noindex` | No | clays `styleguide.astro` |
| Pruebas, laboratorio, variantes | `noindex` desde que se crean, y se borran al decidir | No | [trampas](../trampas-y-lecciones.md#astro-y-build) |
| Redirecciones | — | No | xanasystem-web `filter` |

En clays, gracias, styleguide y pruebas llevan `noindex, nofollow`: vale igual
para páginas sin enlaces que seguir. En legales, no: `follow`
([deuda](../anexos/deuda-proyectos.md)).

### 7. Titulares, URLs y 404 — Claude

- **Un solo `<h1>` por página**: el H1 de `seo-metadatos.md`. En una one-page,
  el del hero (clays `Hero.astro`). En una página interior sin hero, el título
  de la sección principal pasa a H1 (`SectionHeader as="h1"` en clays
  `gallery.astro`).
- **Cada sección con `<h2>`**, sin saltar niveles; preguntas de FAQ en `<h3>`
  dentro de un bloque con `<h2>` (APC `Faq.astro`). El tamaño visual no decide
  el nivel.
- **URLs**: minúsculas, sin acentos, con guiones, en el idioma de la página
  (`/politica-de-privacidad/`, `/privacy-policy/`). Las de `seo-metadatos.md`
  si Eva las da. **Barra final coherente** en rutas, enlaces, canonical y
  sitemap (`trailingSlash: "always"` en `APC-gestion-patrimonial/astro.config.mjs`;
  clays sirve y lista con barra final).
- **404** propia, con cabecera, pie, salida a la portada y `noindex`. En webs con
  idiomas, en servidor para que elija idioma por la ruta
  ([trampas](../trampas-y-lecciones.md#astro-y-build)).

### 8. Redirecciones — Claude (lista de Eva o de la web antigua)

| Forma | Proyecto | Cuándo |
| --- | --- | --- |
| `redirects` en `astro.config.mjs` con el adaptador de Vercel | xanatechnologies | **Por defecto**: salen como 301 reales en la configuración de Vercel |
| `redirects` en `astro.config.mjs` **sin** servidor | xanasystem-web (comentario del config) | En build estático Astro genera una página con `<meta http-equiv="refresh">` + canonical, no un 301 |
| `vercel.json` → `redirects` con `permanent: true` | APC | Pocas reglas; hay que escribir la variante con y sin barra (`/sobre-acp` y `/sobre-acp/`) |

Astro quita la barra final del origen y las URL antiguas con barra dan 404:
`xanatechnologies/scripts/fix-redirect-slashes.mjs` corrige cada regla 301 de
`.vercel/output/config.json` a `^/old-url/?$` tras `astro build`
([trampas](../trampas-y-lecciones.md#astro-y-build)). Inventario de URLs
antiguas y seguimiento → [migración](../modulos/migracion.md).

## 4. Plantillas

- A Eva: [petición de SEO](../plantillas/equipo/peticion-seo.md). Lo que
  devuelve se guarda en `docs/03-contenidos/seo-metadatos.md`.
- Código: `Base.astro`, `sitemap.xml.ts`, `robots.txt.ts` y el JSON-LD de
  `index.astro`, copiados de clays hasta que exista el repo plantilla
  ([plantillas de código](../plantillas/codigo/README.md)).
- Tabla «Qué se indexa» de `clays-of-the-world/docs/03-contenidos/seo-metadatos.md`
  como modelo para el proyecto.

## 5. Dónde queda cada cosa

```
astro.config.mjs                  site · redirects · (integración sitemap)
docs/03-contenidos/seo-metadatos.md   títulos, descripciones, H1, URLs, qué se indexa
src/data/<idioma>.ts              meta: title, description, siteName, imageAlt
src/data/legal/entity.ts          datos de la empresa para Organization
src/layouts/Base.astro            <head>: title, description, canonical, robots, OG, hreflang
src/pages/sitemap.xml.ts          sitemap propio
src/pages/robots.txt.ts           robots generado
src/pages/404.astro               404 con noindex
public/og-image.jpg · favicons    → imágenes
scripts/fix-redirect-slashes.mjs  si hay redirecciones con barra final
```

## 6. Reglas y trampas

- **Nada de datos inventados en el JSON-LD**: ni `Offer` a precio 0, ni
  domicilios o teléfonos que no estén en `entity.ts`, ni perfiles sociales de
  prueba. Lo que falta se omite.
- **Un dato en un solo sitio**: título y descripción en los datos (no en
  `seo-metadatos.md` y en el componente a la vez sin avisar; clays lo advierte en
  el propio documento); la FAQ y su JSON-LD del mismo array.
- **Todo lo absoluto sale de `site`**: canonical, `og:url`, `og:image`, sitemap,
  robots, JSON-LD. Nada de dominios escritos a mano
  (`Astro.site ?? new URL('https://xanatechnologies.com')` es un recurso, no el
  patrón).
- **`noindex` en el código, no en `robots.txt`**; y lo que tiene `noindex` fuera
  del sitemap.
- **El sitemap no lista lo que no existe**: en clays se quitó `/es/` hasta que
  hubo versión en español (Search Console daría 404) (`seo-metadatos.md`).
- Sitemap que no aparece en `/sitemap.xml` con la integración
  ([trampas](../trampas-y-lecciones.md#astro-y-build)).
- Páginas de prueba publicadas e indexables
  ([trampas](../trampas-y-lecciones.md#astro-y-build)).
- **Previews**: Vercel les pone `noindex`; producción no. El `noindex` de las
  internas tiene que estar en el código ([publicación](publicacion.md)).

## 7. Checklist

**Construcción**
- [ ] `site` con el dominio definitivo (con o sin `www` decidido)
- [ ] `Base.astro` con `title`, `description`, canonical, `robots`, favicons, `theme-color`
- [ ] OG completo: `og:image` absoluta 1200×630 con `width`, `height`, `alt`; `og:locale`, `og:site_name`; `twitter:card` `summary_large_image`
- [ ] Título, descripción y H1 de cada página tomados de `seo-metadatos.md` y guardados en los datos
- [ ] Sitemap solo con indexables (y `hreflang` si hay idiomas)
- [ ] `robots.txt` generado, sin `Disallow` de páginas; `Sitemap:` correcto
- [ ] JSON-LD por tipo, desde datos reales, sin `null` ni `Offer` falso; FAQ del mismo array
- [ ] `noindex, follow` en legales; `noindex` en gracias, 404, styleguide y pruebas
- [ ] Un H1 por página; H2 por sección
- [ ] URLs en minúsculas con guiones y barra final coherente
- [ ] 404 propia con `noindex`
- [ ] Redirecciones de la web antigua con y sin barra final

**Revisión y publicación** → [revisión §5](revision.md) · [publicación §5.1](publicacion.md)
- [ ] Ver el código fuente del build de cada tipo de página: canonical, robots, OG y JSON-LD como se esperan
- [ ] JSON-LD validado sin errores
- [ ] Compartir la URL temporal en una red o mensajería: sale imagen, título y descripción
- [ ] `/robots.txt` y `/sitemap.xml` (o `sitemap-index.xml`) en el dominio definitivo
- [ ] Search Console verificada y sitemap enviado (Belén)
