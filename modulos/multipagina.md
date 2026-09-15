# Módulo · Multipágina

Webs con varias páginas de contenido, no solo portada + legales + gracias +
404: arquitectura de URL, navegación y megamenú, migas de pan, JSON-LD por
página, enlazado interno y un esqueleto fijo por tipo de página. Referencias:
**xanasystem-web** (producto) y **APC-gestion-patrimonial** (servicios).

## Cuándo se usa (y cuándo no)

- **Sí:** cada tema necesita su propia página para posicionar o para contarse
  (xanasystem-web: una página por módulo con su palabra clave; APC: inicio,
  sobre APC, servicios, conocimiento, contacto).
- **No:** una landing cuyas secciones se recorren con anclas, aunque tenga
  alguna página suelta de apoyo. Clays tiene `/gallery`, que existe para que
  las fotos del visor sean indexables, y sigue siendo una
  [landing](../tipos-de-proyecto/landing.md).

## Qué añade al código

### Arquitectura de URL

| Decisión | xanasystem-web | APC |
| --- | --- | --- |
| Profundidad | Plana, en la raíz: `/pim`, `/dam`, **no** `/plataforma/pim` (`CLAUDE.md` regla 3) | Plana; el detalle del hub, un nivel más: `/conocimiento/articulos/[slug]/` (pendiente) |
| Barra final | Sin barra | `trailingSlash: 'always'` en `astro.config.mjs`; todos los `href` con barra |
| Slugs | Español, sin acentos ni mayúsculas, con guiones | Igual |

- **Una palabra clave principal por página** y ninguna página compitiendo con
  otra por la misma (xanasystem-web regla 3). Salen de
  [contenidos y SEO](../flujos/contenidos-seo.md).
- La barra final se decide al empezar y se respeta en enlaces, canonical y
  sitemap. El canonical se construye con `Astro.url.pathname`, así que un
  enlace con la otra forma genera otra URL.
- Lo que aún no existe no se enlaza (ver trampas).

### Navegación como datos

APC centraliza menú, CTA permanente y enlaces legales (`src/config/site.ts`):

```ts
export const nav = [
  { label: "Inicio", href: "/" },
  { label: "Sobre APC", href: "/sobre-apc/" },
  { label: "Cómo te acompañamos", href: "/gestion-patrimonial/" },
  { label: "Conocimiento", href: "/conocimiento/" },
] as const;

/** CTA permanente del sitio. */
export const primaryCta = { label: "Hablemos", href: "/contacto/" } as const;
```

El menú no repite «Contacto» porque ya lo lleva el CTA.

### Megamenú

`xanasystem-web/src/components/nav/MegaMenu.astro`: un `<button>` con
`aria-expanded`, `aria-controls` y `aria-haspopup` abre un panel `role="region"`
con `aria-label`; abre con clic y con hover, cierra con Escape y al abrir otro.
En móvil, la cabecera lo convierte en acordeones con el mismo patrón
(`Header.astro`).

### Migas de pan

`xanasystem-web/src/components/nav/Breadcrumb.astro`:

```astro
<nav class="breadcrumb" aria-label="Migas de pan">
  <ol class="breadcrumb__list">
    {items.map((crumb, i) => {
      const isLast = i === items.length - 1;
      return (
        <li class="breadcrumb__item">
          {crumb.href && !isLast
            ? <a href={crumb.href}>{crumb.label}</a>
            : <span aria-current="page">{crumb.label}</span>}
          {!isLast && <span aria-hidden="true">›</span>}
        </li>
      );
    })}
  </ol>
</nav>
```

El JSON-LD `BreadcrumbList` va aparte, en la página.

### JSON-LD por página

Cada página declara lo suyo; el layout, lo común.

| Página | Tipos | Fuente |
| --- | --- | --- |
| Todas (layout) | `ProfessionalService` + `Person` | APC `BaseLayout.astro` |
| Página de producto | `BreadcrumbList` + `SoftwareApplication` + `Product` | xanasystem-web `src/pages/pim.astro` |
| Conversión / contacto | `WebPage` + `ContactPage` + `BreadcrumbList` | xanasystem-web `src/pages/demo.astro` |
| Página con FAQ | `FAQPage` (lo emite el componente de FAQ) | xanasystem-web `DemoFaq`, APC `gestion-patrimonial.astro` |
| Hub de contenido | `CollectionPage` + `BreadcrumbList` | APC `src/pages/conocimiento.astro` |
| Persona | `Person` + `BreadcrumbList` | APC `src/pages/sobre-apc.astro` |

```astro
<script type="application/ld+json" is:inline set:html={JSON.stringify(breadcrumbLd)} />
```

Detalle de cada tipo → [SEO técnico](../flujos/seo-tecnico.md).

### Esqueleto fijo por tipo de página

- **Una carpeta de secciones por página** y la página solo importa y ordena
  (APC `CLAUDE.md`: `src/components/sections/<pagina>/`).
- **Las páginas del mismo tipo comparten orden de secciones.** xanasystem-web
  lo fija en `docs/design.md` §5 para las páginas de módulo; la portada no lo
  sigue. Si algo se repite en dos páginas, es componente (regla 4).
- Estructuras por defecto de cada tipo → [web de producto](../tipos-de-proyecto/web-producto.md)
  y [web editorial](../tipos-de-proyecto/web-editorial.md).

### Enlazado interno

Clúster con una página madre (xanasystem-web `CLAUDE.md` regla 7): la portada
enlaza a todas las de módulo; `/pim` a las demás; cada módulo vuelve a `/pim`;
todas llevan a la conversión. Bloque reutilizable:
`src/components/sections/InternalLinksBlock.astro`.

### Páginas renombradas o retiradas

Nunca 404 para una URL publicada:

- **En `astro.config.mjs`** (xanasystem-web): `'/contacto': '/demo'` y
  `'/auditoria': '/demo'`, con el comentario de por qué se retiró.
- **En `vercel.json`** (APC), con y sin barra:
  `{ "source": "/sobre-acp/", "destination": "/sobre-apc/", "permanent": true }`.

Si la web sustituye a otra → [migración](migracion.md).

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| SEO | `title` y `description` propios en cada página (props del layout); una sola `h1`; definición del tema en las primeras 100–150 palabras (xanasystem-web regla 2); sitemap con todas las indexables (xanasystem-web filtra redirecciones, styleguide y prototipos) → [SEO técnico](../flujos/seo-tecnico.md) |
| Formulario | Una página de conversión a la que llevan todos los CTA (`/demo` en xanasystem-web, `/contacto/` en APC); `/gracias/` igual |
| Legales | Las mismas tres páginas; si la actividad es regulada, página extra en el pie (APC `/informacion-financiera/`) → [`parrafos-por-modulo.md` § Actividad regulada](../plantillas/legales/parrafos-por-modulo.md#actividad-regulada) |
| Muro | Sin cambios: va en el layout de todas las páginas públicas |
| Revisión | Todas las páginas enlazadas desde algún sitio; ningún enlace a página inexistente; títulos y descripciones únicos → [revisión](../flujos/revision.md) |

## Qué pedir al cliente de más

- **Mapa de páginas aprobado** y qué páginas llegan en fases posteriores
  (xanasystem-web lista las que no se crean todavía).
- **Terminología y CTA literales** (xanasystem-web regla 5: «Solicitar demo»,
  nunca «Solicitar auditoría»).
- Textos por página y, de Eva, la palabra clave de cada una →
  [petición SEO](../plantillas/equipo/peticion-seo.md).

## Reglas y trampas

- **No enlazar páginas que no existen.** xanasystem-web enlaza `/legal/*`,
  `/privacidad` y `/acceso-clientes`, que no existen (el formulario de `/demo`
  lleva a `/privacidad`) → [deuda](../anexos/deuda-proyectos.md).
- **No declarar en JSON-LD lo que no hay**: `Offer` con precio 0 en
  xanasystem-web (deuda).
- **Barra final coherente**: sitemap, canonical y enlaces con la misma forma.
- **Cambio en un componente compartido → aislarlo** para la sección pedida
  (memoria `alcance-cambios-seccion` de xanasystem-web; [forma de trabajo](../forma-de-trabajo.md)).
- **Proponer antes de extender** a todas las páginas algo probado en una (APC).
- Páginas de laboratorio con `noindex` y borradas al decidir →
  [trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build).

## Checklist

**Estructura**
- [ ] Mapa de páginas con URL, palabra clave y objetivo, aprobado
- [ ] Barra final decidida y anotada en el `CLAUDE.md`
- [ ] Esqueleto de cada tipo de página escrito (en `docs/` del proyecto)

**Construcción**
- [ ] Menú, CTA permanente y legales como datos
- [ ] Megamenú y menú móvil accesibles con teclado (Escape cierra)
- [ ] Migas de pan en páginas internas + `BreadcrumbList`
- [ ] JSON-LD propio de cada página, sin datos que no existan
- [ ] `title`, `description` y `h1` únicos por página
- [ ] Enlazado interno según el clúster
- [ ] Redirecciones para URL renombradas o retiradas
- [ ] Sitemap con todas las indexables

**Revisión**
- [ ] Ningún enlace interno a 404
- [ ] Ninguna página huérfana

## Proyecto de referencia

- **xanasystem-web**: `CLAUDE.md` reglas 2–7 y 14, `docs/design.md` §2 y §5,
  `src/pages/pim.astro`, `demo.astro`, `src/components/nav/MegaMenu.astro`,
  `Breadcrumb.astro`, `Header.astro`, `InternalLinksBlock.astro`,
  `astro.config.mjs` (redirecciones y sitemap).
- **APC-gestion-patrimonial**: `CLAUDE.md` § Arquitectura del sitio,
  `src/config/site.ts`, `astro.config.mjs` (`trailingSlash`), `vercel.json`,
  `src/layouts/BaseLayout.astro`, `src/pages/*.astro`.
