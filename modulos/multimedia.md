# Módulo · Multimedia

Galería con visor, varios vídeos y contenido incrustado de terceros (Vimeo,
YouTube, Google Maps). La imagen suelta y el vídeo de fondo del hero son de la
base → [imágenes](../flujos/imagenes.md) y [vídeos](../flujos/videos.md).
Referencia: **clays-of-the-world**.

## Cuándo se usa (y cuándo no)

- **Sí:** una galería que se abre en grande (clays: rejilla de toda la
  colección y cada foto una a una), un carrusel con visor (magnaceramica), más
  de un vídeo con reproducción propia (clays: hero y sección 04), o un embed de
  un tercero (festival-viento: Vimeo y Google My Maps).
- **No:** fotos dentro de secciones y un vídeo de ambiente → flujos de
  [imágenes](../flujos/imagenes.md) y [vídeos](../flujos/videos.md).

## Qué añade al código

### Galería con visor (clays)

Tres piezas:

| Pieza | Qué es |
| --- | --- |
| `src/components/sections/Gallery.astro` | La sección: un abanico de fotos destacadas y un CTA |
| `src/pages/[...lang]/gallery.astro` | **Página de verdad** con todas las fotos sin recortar, cada una con su ancla `#foto-N` |
| `src/components/Lightbox.astro` | El visor: `<dialog>` con dos estados, rejilla y foto a foto |

Cómo encaja:

- **Lo que abre el visor son enlaces** a `/gallery` o `/gallery#foto-N`, no
  botones. El script les quita el salto y abre el `<dialog>`. Sin JavaScript
  se llega a la página, y la página es lo que ven los buscadores: un `<dialog>`
  cerrado es `display: none` para todos.
- **El destino decide el estado**: `/gallery` abre la rejilla; `#foto-N`, esa
  foto grande.
- `showModal()` da atrapado de foco, Escape y fondo inerte; el script añade
  bloquear el scroll del fondo, las flechas y anunciar el cambio (`aria-live`).
- Se monta **una vez por página**, fuera de `<main>`.
- **Masonry sin JavaScript**: cada pieza ocupa tantas filas de la rejilla como
  pide su marco, calculado en el servidor, con `grid-auto-flow: dense`. Se
  conserva el orden del marcado (no `columns`).
- **Mismos anchos en sección y visor**, para que la rejilla salga de la caché:

  ```ts
  /* Es la misma lista que la del abanico (`sections/Gallery.astro`), y tiene
     que seguir siéndolo. */
  const ANCHOS = [360, 520, 780, 1040, 1560];
  ```

- Las fotos de la rejilla se piden con `IntersectionObserver` cuando la
  galería se acerca a la pantalla; las grandes, al llegar a una y sus dos
  vecinas.

Ver [trampas · Imágenes y vídeo](../trampas-y-lecciones.md#imágenes-y-vídeo)
(«El visor de la galería abre vacío», «Lo que está dentro de un `<dialog>` no
existe para buscadores»).

**Otro montaje (magnaceramica `src/components/BrandBlock.astro`):** carrusel
por marca y visor creado por script (`div` con `role="dialog"` y
`aria-modal`), que carga el original a resolución completa (`data-full`). Sirve
como carrusel; para una galería se usa el de clays.

### Varios vídeos (clays)

`src/scripts/video-autoplay.ts`, importado desde cada componente con vídeo.
Contrato de marcado:

```html
<video data-video [data-autoplay] data-pause-label="…" id="…">
<button data-play aria-controls="<id del vídeo>" aria-label="…">
```

- Arranca solo (`data-autoplay`) **únicamente en pantalla** y nunca con
  `prefers-reduced-motion`: ahí se queda el póster.
- Fuera de pantalla se pausa; al volver se reanuda solo si estaba en marcha.
- **Una pausa pulsada a mano se respeta siempre.**
- El botón escucha `play`/`pause` del vídeo y cambia su `aria-label`.
- Sin JavaScript ningún vídeo arranca.

Vídeos en R2 con la URL base en un solo fichero (magnaceramica
`src/config/media.ts`). Detalle → [vídeos](../flujos/videos.md).

### Embeds de terceros

**No hay todavía un embed con consentimiento montado en ningún proyecto.** Lo
que hay en festival-viento es lo que no se hace:

- Vimeo en `src/components/Caracteristicas.astro`: `<iframe>` de fondo con
  `autoplay`, que además carga `player.vimeo.com/api/player.js` para el botón
  de pausa.
- Google My Maps en `src/components/Mapa.astro`, con una capa que bloquea el
  scroll hasta hacer clic.

Los dos cargan al pintarse, sin preguntar.

**Por orden de preferencia:**

1. **Sin embed**: vídeo propio en R2; mapa como imagen o SVG con enlace
   (festival-viento `CLAUDE.md` reglas 3 y 5).
2. **Embed con fachada**: imagen + enlace al proveedor, y el `<iframe>` solo se
   inserta si el visitante acepta la categoría «Terceros» del muro. Es el
   montaje que presupone el texto legal
   ([`parrafos-por-modulo.md`](../plantillas/legales/parrafos-por-modulo.md#contenido-incrustado-vimeo-youtube-google-maps)).

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| Muro | Con embed: categoría `thirdParty` (no `readOnly`) y su texto en el aviso inicial → [cookies y analítica](../flujos/cookies-analitica.md) |
| Legales | Con embed: tratamiento, destinatario y transferencia en privacidad; categoría y filas en cookies, **detectadas en el navegador** → [§ Contenido incrustado](../plantillas/legales/parrafos-por-modulo.md#contenido-incrustado-vimeo-youtube-google-maps). Fotos de stock que parezcan del cliente → [§ Imágenes de banco](../plantillas/legales/parrafos-por-modulo.md#imágenes-de-banco) |
| SEO | Página de galería indexable y en el sitemap (clays lista `/gallery/`); `alt` de cada foto en los datos |
| Rendimiento | `<Picture>` con `fallbackFormat`; pesos con `build` + `preview`, no en `dev` → [trampas · Imágenes y vídeo](../trampas-y-lecciones.md#imágenes-y-vídeo) |
| Idiomas | Imágenes en un solo fichero de datos y `alt` y pies traducidos; los pies se casan por posición (clays) → [idiomas](idiomas.md) |

## Qué pedir al cliente de más

- **Fotos originales** para la galería y un **pie** por foto si lo quiere.
- **Qué fotos son reales y cuáles de banco** (clays: no se escriben pies que
  hagan pasar el stock por instalaciones propias).
- **Vídeos originales** y cuáles se pueden publicar →
  [`cliente/material.md`](../plantillas/cliente/material.md).
- Si pide un embed: **por qué no puede ser vídeo propio o imagen**.

## Reglas y trampas

- **Sin JavaScript no puede quedar contenido invisible**: la galería existe
  como página y los disparadores son enlaces (clays `CLAUDE.md` § Movimiento y
  JavaScript).
- **Un embed no se carga sin consentimiento**; `loading="lazy"` solo lo retrasa.
- **Nada en el texto legal que no esté en la web**, y al revés.
- Recortar a un marco común es un recorte que se mira foto a foto (comentario
  de `Lightbox.astro`).
- Selector `+` que falla solo en `dev` → usar `~` (magnaceramica
  `BrandBlock.astro`; [trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build)).
- Decisiones cerradas no se reabren: en clays los vídeos se publican sin
  comprimir por decisión de Xana (14-09-2026).

## Checklist

**Galería**
- [ ] Página de galería real con anclas por foto, indexable
- [ ] Disparadores del visor son enlaces a esa página
- [ ] `<dialog>` + `showModal()`; Escape, foco, flechas y anuncio del cambio
- [ ] Anchos compartidos entre sección y visor; `sizes` según el marco
- [ ] Sin JavaScript se ven todas las fotos

**Vídeos**
- [ ] Todos en R2, URL base en un fichero
- [ ] Arranque solo en pantalla, pausa fuera, pausa manual respetada
- [ ] Con `prefers-reduced-motion`, póster

**Embeds**
- [ ] Justificado que no puede ser vídeo propio o imagen
- [ ] Fachada; el `<iframe>` solo tras aceptar «Terceros»
- [ ] Rechazar: ninguna cookie del proveedor · aceptar: solo las declaradas
- [ ] Párrafos de privacidad y cookies añadidos

## Proyecto de referencia

- **clays-of-the-world**: `src/components/Lightbox.astro`,
  `src/components/sections/Gallery.astro`, `src/pages/[...lang]/gallery.astro`,
  `src/scripts/video-autoplay.ts`, `CLAUDE.md` § Estructura de la web y
  § Cuestiones abiertas, `CHECKLIST.md` §8.
- **magnaceramica**: `src/components/BrandBlock.astro`, `src/config/media.ts`.
- **festival-viento** (qué no hacer): `src/components/Caracteristicas.astro`,
  `src/components/Mapa.astro`.
