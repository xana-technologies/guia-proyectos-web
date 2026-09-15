---
name: montar-seo-tecnico
description: Montar el SEO técnico de una web Astro de Xana — site, metadatos y canonical en Base.astro, Open Graph, sitemap, robots.txt, datos estructurados JSON-LD, noindex, un H1 por página, URLs, 404 y redirecciones. Usar al preparar el head de la web, llevar los metadatos de SEO al código o revisar qué se indexa.
---

# Montar el SEO técnico

Todo el detalle está en el flujo [`flujos/seo-tecnico.md`](../../flujos/seo-tecnico.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.** La implementación de referencia es
`clays-of-the-world/src/layouts/Base.astro`.

Esta skill **no escribe** títulos, descripciones ni H1: los lleva al código
desde `docs/03-contenidos/seo-metadatos.md`.

## 1. Antes de tocar nada

| Qué | Dónde mirarlo | Si falta |
| --- | --- | --- |
| Dominio definitivo, con o sin `www` | `CLAUDE.md` del proyecto | Preguntar a Belén: de `site` sale todo lo absoluto |
| Títulos, descripciones, H1 y URLs | `docs/03-contenidos/seo-metadatos.md` | Montar con `[[ ]]` y anotar que **no se publica** |
| Tipo de web (una página, multipágina, idiomas) | `CLAUDE.md`, `src/pages/` | Elegir sitemap propio o integración (flujo §3 paso 3) |
| Web antigua en el dominio | Briefing, `seo-metadatos.md` | Redirecciones → módulo [migración](../../modulos/migracion.md) |
| Datos de la empresa para `Organization` | `src/data/legal/entity.ts` | Lo que es `null` se omite; nunca se inventa |
| Imagen para redes y favicons | `public/` | Anotar que falta → `xana:rrss-favicon` |

Explica a Belén el plan y las dudas, y espera antes de empezar
([forma de trabajo §1](../../forma-de-trabajo.md)).

## 2. Pasos

Siguen el flujo §3, en el mismo orden:

1. **`site`** en `astro.config.mjs`.
2. **`Base.astro`**: título, descripción, canonical, `robots` como texto,
   favicons, `theme-color`, Open Graph y `twitter:card`. Títulos y
   descripciones en los datos, no en el componente.
3. **Sitemap**: solo páginas indexables. Endpoint propio por defecto;
   integración si hay muchas páginas o blog.
4. **`robots.txt` generado**, sin `Disallow` de páginas con `noindex`.
5. **JSON-LD** por tipo de página (tabla del paso 5), desde datos que ya
   existen: sin `null`, sin `Offer` si no hay precio público, la FAQ del mismo
   array que la pinta.
6. **`noindex`** según la tabla del paso 6: `noindex, follow` en legales;
   `noindex` en gracias, 404, styleguide y pruebas. Lo que tiene `noindex`,
   fuera del sitemap.
7. **Titulares, URLs y 404**: un H1 por página, H2 por sección, URLs en
   minúsculas con guiones y barra final coherente, 404 propia.
8. **Redirecciones**, si hay web antigua, con y sin barra final (paso 8).
9. **Si hay idiomas**: `hreflang`, `x-default` y `og:locale` → módulo
   [idiomas](../../modulos/idiomas.md).

## 3. Al terminar

- `npm run build` y mira el código fuente generado de cada tipo de página
  (portada, interior, legal, gracias, 404): canonical, robots, OG y JSON-LD
  como se esperan.
- Recorre la checklist **Construcción** del flujo §7 y di qué está comprobado
  y qué no.
- Si algo falla, busca el síntoma en
  [trampas · Astro y build](../../trampas-y-lecciones.md#astro-y-build)
  (sitemap que no aparece, `Disallow` que tapa el `noindex`, redirecciones con
  barra final).
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
- Search Console, sitemap enviado y comprobación en el dominio van en la
  publicación ([`flujos/publicacion.md`](../../flujos/publicacion.md)).
