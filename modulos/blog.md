# Módulo · Blog / hub de contenido

Artículos, noticias o recursos que crecen con el tiempo.

> **Sin proyecto terminado de referencia.** El único en marcha es el hub
> `/conocimiento/` de APC, que está maquetado con contenido de muestra y sin
> decidir cómo se gestionará. Esta ficha recoge solo lo que ya está hecho o
> decidido. La implementación se escribe cuando un proyecto la cierre.

## Cuándo se usa (y cuándo no)

- **Sí:** el cliente va a publicar contenido de forma continuada y quiere que
  se lea y posicione (APC: «Web y hub de conocimiento», `CLAUDE.md`).
- **No** en la primera fase si el contenido no existe todavía: xanasystem-web
  deja `/blog`, `/guias` y `/glosario` para fases posteriores y **no los crea
  todavía** (`CLAUDE.md` regla 4).
- Siempre encima de [multipágina](multipagina.md).

## Qué hay hecho (APC)

| Pieza | Estado | Fuente |
| --- | --- | --- |
| Página hub `/conocimiento/` | Maquetada: Explorar temas (con `h1` y migas), Destacado, Últimas ideas, Firma experta, Cierre | `src/pages/conocimiento.astro`, `src/components/sections/conocimiento/` |
| Contenido de las tarjetas | **De muestra**: todos los enlaces llevan a `/conocimiento/` | `Destacado.astro`, `UltimasIdeas.astro`, `ExplorarTemas.astro` |
| JSON-LD del hub | `CollectionPage` + `BreadcrumbList` | `src/pages/conocimiento.astro` |
| Sección en la portada | Dos tarjetas con tema y tiempo de lectura, contenido de muestra | `src/components/sections/home/Conocimiento.astro` |
| Temas | Planificación patrimonial · Gestión e inversión · Psicología del dinero · Educación financiera · Decisiones y etapas vitales · Fiscalidad y patrimonio | `ExplorarTemas.astro` |
| Fichas de artículo | No existen; no hay `src/content/` | — |

## Qué está por decidir

Antes de construir las fichas, con Belén:

1. **Gestión del contenido: Content Collections de Astro o CMS headless.**
   Pendiente en APC (`CLAUDE.md`: «por decidir … cuando se construya
   Conocimiento»). El briefing pide CMS; el proyecto no lo tiene (memoria
   `acp-arquitectura-web`). Antes de decidir hay que saber **quién va a
   publicar**: el cliente o nosotros.
2. **URL de las fichas**: `/conocimiento/{articulos|actualidad|recursos}/[slug]/`
   o `/conocimiento/[slug]/` con el tipo como taxonomía. APC lo deja para
   «antes de publicar» (memoria `acp-arquitectura-web`).
3. Listados, categorías, paginación y RSS: **sin decidir en ningún proyecto**.

Referencia de otro tipo de contenido editado en el repo: festival-viento
decidió Markdown/datos en Git sin CMS (`CLAUDE.md` regla 4), para un programa,
no para un blog.

## Qué cambia en la base (lo que ya se sabe)

| Pieza | Qué cambia |
| --- | --- |
| SEO | Cada ficha indexable y en el sitemap; JSON-LD previsto `Article`/`NewsArticle` + `BreadcrumbList` (APC `CLAUDE.md` § Reglas de tono y legales) → [SEO técnico](../flujos/seo-tecnico.md) |
| Legales | Si el contenido puede leerse como consejo en una actividad regulada: aviso en los contenidos y página propia → [`parrafos-por-modulo.md` § Actividad regulada](../plantillas/legales/parrafos-por-modulo.md#actividad-regulada) (APC: aviso educativo en contenidos y pie) |
| Legales | Si hay suscripción: [§ Newsletter](../plantillas/legales/parrafos-por-modulo.md#newsletter). Sin newsletter, no se añade |
| Muro | Si un artículo incrusta vídeos o mapas de terceros → [multimedia](multimedia.md) |
| Imágenes | Una por artículo como mínimo; no repetir; origen en el inventario → [imágenes](../flujos/imagenes.md) |

## Qué pedir al cliente de más

- **Quién publica y con qué frecuencia** (decide Collections o CMS).
- **Artículos iniciales**: textos, autor, fecha, tema, imagen. En APC, la
  «selección de contenidos iniciales del hub» sigue pendiente (`CLAUDE.md`).
- Temas o categorías, y quién firma (APC: sin autor en las tarjetas; «Firma
  experta» como sección aparte).

## Reglas y trampas

- **Contenido de muestra no se publica.** Las tarjetas con textos e imágenes de
  marcador se sustituyen o se retiran antes de publicar, igual que cualquier
  `[[ ]]`.
- **Tarjetas que enlazan a la propia página** (todas a `/conocimiento/`) no
  pueden quedar en producción.
- **No crear páginas vacías de blog** para «tenerlo preparado»
  (xanasystem-web regla 4).
- En actividad regulada, **tono sin promesas** (APC `CLAUDE.md` § Reglas de tono).

## Checklist

**Antes de construir**
- [ ] Decidido Content Collections o CMS, y anotado en el `CLAUDE.md`
- [ ] Decidida la estructura de URL de las fichas
- [ ] Artículos iniciales recibidos o fecha para tenerlos

**Antes de publicar**
- [ ] Ningún contenido ni imagen de muestra
- [ ] Ningún enlace de tarjeta a la propia página
- [ ] Fichas en el sitemap con su JSON-LD
- [ ] Aviso de actividad regulada si aplica

## Proyecto de referencia

APC-gestion-patrimonial (sin terminar): `CLAUDE.md` § Arquitectura del sitio,
`src/pages/conocimiento.astro`, `src/components/sections/conocimiento/`,
`src/components/sections/home/Conocimiento.astro`, memorias
`acp-arquitectura-web` y `apc-estado-desarrollo`.
