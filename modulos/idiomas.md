# Módulo · Idiomas

La web en más de un idioma: rutas por idioma, contenido como datos, `hreflang`,
selector, 404 en el idioma de la ruta y la base (legales, muro, formulario)
traducida. Hay dos montajes probados: **clays-of-the-world** y
**xanatechnologies**.

## Cuándo se usa (y cuándo no)

- **Sí:** el cliente lo pide en el arranque (pregunta 1.4 de
  [`cliente/arranque.md`](../plantillas/cliente/arranque.md)) o se sabe que
  llegará. En clays se decidió al empezar que acabaría en tres idiomas; el
  español entró el 14-09-2026 y el francés vendrá después.
- **Si llegará más adelante, pero no ahora:** no se monta el módulo, pero
  **el contenido se escribe como datos desde el primer componente**, para que
  añadir un idioma después sea barato (clays `CLAUDE.md` § Idioma; festival
  `CLAUDE.md` regla 6: «diseñar pensando en que vendrá, pero no implementar
  todavía»).
- **No:** una web en un solo idioma no lleva prefijos, selector ni `hreflang`.

## Qué añade al código

### Dos montajes

| | clays-of-the-world | xanatechnologies |
| --- | --- | --- |
| Rutas | Sin la opción `i18n` de Astro. Páginas en `src/pages/[...lang]/`, que generan todas las versiones con `getStaticPaths` | `i18n` de Astro. Una página por idioma: `src/pages/index.astro` y `src/pages/es/index.astro` |
| URL | **No se traducen**: `/gallery` y `/es/gallery` | Se traducen las legales: `/legal-notice/` y `/es/aviso-legal/` (`LEGAL_ROUTES`) |
| Idioma sin prefijo | Inglés, porque las URL ya indexadas no cambian | Inglés (la web antigua tenía el español en la raíz: hubo que redirigir, ver [migración](migracion.md)) |
| Contenido | Un fichero por idioma: `src/data/en.ts`, `es.ts`, y `index.ts` con `getContent(lang)` | Un fichero con los dos: `src/i18n/content.ts` |
| Alternativas de cada página | Se calculan cambiando el prefijo (`translatePath`) | Prop `alternates` de `Base.astro`, página a página |
| Detección del idioma del navegador | No | Sí, con memoria de la elección |
| Sitemap | Endpoint propio `src/pages/sitemap.xml.ts` con `hreflang` | `@astrojs/sitemap` con `i18n` + `scripts/format-sitemap.mjs` |

**Decisión (15-09-2026): el estándar es el montaje de clays.** Una página es un
solo fichero para todos los idiomas y el idioma siguiente es un fichero de
textos más; cambiar el diseño de una sección se hace una vez. Del sistema de
Astro solo se toma la idea de idioma por defecto sin prefijo.

Dos reglas añadidas al montaje de clays:

- **Las legales se traducen, también su URL** (`/aviso-legal/` ·
  `/en/legal-notice/`). Para eso las rutas legales salen de una **tabla de
  nombres por idioma** en los datos (como `LEGAL_ROUTES` de xanatechnologies),
  no del nombre del fichero; el resto de páginas pueden mantener el mismo
  nombre en todos los idiomas.
- **Excepción:** si el cliente lo especifica, la política de cookies y el aviso
  legal pueden quedar en un solo idioma. Se anota en `CHECKLIST.md` que falta
  la traducción → [legales](../flujos/legales.md#webs-con-varios-idiomas).

### Rutas (clays)

```ts
// src/data/index.ts
export const LANGS = ['en', 'es'] as const;
export const DEFAULT_LANG: Lang = 'en';

export function langStaticPaths() {
  return LANGS.map((lang) => ({
    params: { lang: lang === DEFAULT_LANG ? undefined : lang }, // sin prefijo
    props: { lang },
  }));
}

/** La misma ruta en otro idioma: `/es/gallery` → `/gallery`, y al revés. */
export function translatePath(pathname: string, to: Lang): string { … }
```

```astro
---
// src/pages/[...lang]/index.astro
export function getStaticPaths() {
  return langStaticPaths();
}
const { lang } = Astro.props as { lang: Lang };
const content = getContent(lang);
---
```

Añadir un idioma (comentario de `src/data/index.ts`): `fr.ts`, una entrada en
`LANGS` y en `contents`, y su opción en el selector de cada fichero de datos.

**Con `i18n` de Astro** (xanatechnologies `astro.config.mjs`):

```js
i18n: {
  defaultLocale: 'en',
  locales: ['en', 'es'],
  routing: { prefixDefaultLocale: false },
},
```

### Contenido

- **Ningún texto en el marcado**: los componentes lo reciben por props
  (clays `CLAUDE.md` § Idioma).
- **Las imágenes, en un solo sitio.** `es.ts` toma las imágenes de `en.ts` y
  solo traduce el `alt`; los pies de la galería se casan por posición (clays).
- En xanatechnologies el inglés es la versión de referencia y el español una
  adaptación, «not a literal translation», con las mismas cifras, destinos y
  consentimientos (`src/i18n/content.ts`).

### `<head>`: canonical, `hreflang`, `og:locale`

En `Base.astro` (clays):

- `hreflang` de cada versión que existe + `x-default` al idioma sin prefijo.
- **Solo en páginas indexables y traducidas** (sin `robots`); la 404 no lleva.
- `og:locale` del idioma de la página y `og:locale:alternate` de los demás.
- `<html lang={lang}>`.

xanatechnologies pone además los `hreflang` en las legales, que son `noindex`.

### Selector de idioma

- **Cada idioma escrito entero y en el suyo** («Español», no «Spanish» ni
  «ES»), con su atributo `lang` y `hreflang` (clays `LangMenu.astro`,
  `LangSwitcher.astro`).
- **Lleva a la misma página en el otro idioma**, no a la portada
  (`translatePath` en `Base.astro`). La 404 es la excepción: `translated={false}`.
- Un idioma sin versión todavía (`href: null`) se pinta como `<span>`, sin
  enlace.
- Dónde: en clays, en la barra legal del pie (`LangSwitcher`) y en la cabecera
  como `<details>` que funciona sin JavaScript (`LangMenu`). En
  xanatechnologies, un conmutador EN | ES en cabecera y pie.

### Detección del idioma del navegador (opcional)

Solo en xanatechnologies (`src/layouts/Base.astro`). Reglas que trae:

```html
<script is:inline define:vars={{ spanishPath }}>
  try {
    var chosen = localStorage.getItem('xana-lang');
    var langs = navigator.languages || [navigator.language || ''];
    var wantsEs = langs.some(function (l) { return /^es\b/i.test(l); });
    if (!chosen && wantsEs) location.replace(spanishPath + location.hash);
  } catch (e) {}
</script>
```

- Solo desde el idioma por defecto hacia el otro, y **nunca si el visitante ya
  eligió** con el selector (el clic guarda `xana-lang` en `localStorage`).
- **Desactivada en la 404** (`detectLanguage={false}`): taparía el error.

### 404 en el idioma de la ruta

`src/pages/404.astro` con `export const prerender = false`: elige idioma por el
prefijo de la ruta pedida (`langFromPath` en clays; regex `/^\/es(\/|$)/` en
xanatechnologies). La función ya existe por el formulario. Ver
[trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build) («La 404
sale siempre en el idioma por defecto»).

## Qué cambia en la base

| Pieza | Qué cambia | Dónde |
| --- | --- | --- |
| Legales | Los tres documentos y el muro completos en cada idioma; enlaces al mismo idioma; `entity.ts` no se traduce | [`parrafos-por-modulo.md` § Idiomas](../plantillas/legales/parrafos-por-modulo.md#idiomas) · [legales](../flujos/legales.md) |
| Muro de cookies | Textos por idioma desde los datos | [cookies y analítica](../flujos/cookies-analitica.md) §6 |
| Formulario | Mensajes y etiquetas por idioma; **página de gracias por idioma** (`/thank-you`, `/es/thank-you`); sin JavaScript, el 303 va a la de su idioma (clays commit `19a8244`). El aviso por correo sigue en el idioma de quien lo recibe | [formulario](../flujos/formulario.md) §2 y §9 |
| Analítica | La conversión cuenta la página de gracias de cada idioma («Page Path termina en `thank-you`», clays `CHECKLIST.md` §5) | [cookies y analítica](../flujos/cookies-analitica.md) |
| SEO | Title y description por idioma (clays `docs/03-contenidos/seo-metadatos.md`); sitemap con alternativas; Search Console recibe el sitemap una vez | [SEO técnico](../flujos/seo-tecnico.md) |
| Revisión | Cada página en cada idioma; prueba real del formulario por idioma | [revisión](../flujos/revision.md) · [publicación](../flujos/publicacion.md) §5.2 |

## Qué pedir al cliente de más

- **Qué idiomas y cuál manda** si hay discrepancia (solo si lo piden: párrafo
  opcional del aviso legal).
- **Textos en cada idioma**, o quién traduce. En clays el cliente dio la copy
  en español (`docs/03-contenidos/web-copy-es.pdf`); lo que no venía
  (antetítulos, `alt`, formulario, legales, 404) lo tradujo Xana con la misma
  terminología y se da por copy de la web.
- **Trato** (tú/usted) en cada idioma: en clays, usted en todo, también en dos
  CTA que el cliente traía de tú.
- **Datos legales literales** en cada idioma que el cliente tenga (en clays
  queda pendiente sustituir el objeto social traducido por el literal).
- Metadatos SEO por idioma → [petición a Eva](../plantillas/equipo/peticion-seo.md).

## Reglas y trampas

- **La URL de un idioma no cambia al añadir otro.** El idioma cuyas URL ya
  están indexadas va sin prefijo (clays).
- **Nada de anchos calculados sobre el texto de un idioma**: alemán y francés
  alargan un 15–30 % (clays `CLAUDE.md`). En español, la cabecera de clays pasa
  al menú desplegable por debajo de 1480 px porque el CTA no cabe antes
  (commit `19a8244`).
- **No listar en el sitemap una versión que aún no existe**: da 404 en Search
  Console (comentario de clays `sitemap.xml.ts`).
- `@astrojs/sitemap` no deja el sitemap en `/sitemap.xml` → [trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build).
- La detección por navegador **no puede tapar la 404** ni ignorar la elección
  del visitante.
- La 404 estática sale siempre en un idioma: va en servidor.

## Checklist

**Construcción**
- [ ] Idioma sin prefijo decidido y anotado en el `CLAUDE.md` del proyecto
- [ ] Todo el texto en datos, un fichero por idioma; imágenes en un solo sitio
- [ ] Páginas en `src/pages/[...lang]/` (o `i18n` de Astro, si se decide)
- [ ] `<html lang>`, canonical, `hreflang` + `x-default`, `og:locale` y alternates
- [ ] Selector con nombres en su idioma, `lang` y `hreflang`; lleva a la misma página
- [ ] 404 con `prerender = false` en el idioma de la ruta
- [ ] Legales y muro completos en cada idioma; enlaces entre documentos al mismo idioma
- [ ] Formulario: textos, errores y página de gracias por idioma
- [ ] Sitemap con cada página en cada idioma y alternativas cruzadas

**Revisión y publicación**
- [ ] Recorrer la web en cada idioma: ningún texto del otro idioma
- [ ] Cambiar de idioma desde una página interna lleva a la misma página
- [ ] `/es/loquesea` da 404 en español
- [ ] Componentes con textos un 30 % más largos
- [ ] Envío real del formulario en cada idioma; conversión en GTM con todas las páginas de gracias

## Proyecto de referencia

- **clays-of-the-world**: `src/data/index.ts`, `src/data/en.ts` y `es.ts`,
  `src/pages/[...lang]/`, `src/layouts/Base.astro`, `LangMenu.astro`,
  `LangSwitcher.astro`, `src/pages/sitemap.xml.ts`, `src/pages/404.astro`,
  `src/data/legal/es.ts`, `CHECKLIST.md` §11, commits `19a8244` y `443802c`.
- **xanatechnologies**: `astro.config.mjs` (`i18n`, sitemap),
  `src/layouts/Base.astro` (detección y `hreflang`), `src/i18n/content.ts`,
  `src/pages/es/`, `src/pages/404.astro`, `CLAUDE.md` § Project structure.
