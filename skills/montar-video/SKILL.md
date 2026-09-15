---
name: montar-video
description: Montar un vídeo propio en una web de Xana (fondo de hero o pieza de sección) — alojado en Cloudflare R2 con la URL en src/config/media.ts, <video muted playsinline> sin autoplay en el HTML, póster, arranque solo en pantalla con video-autoplay.ts, pausa accesible, respeto a reducir movimiento y hero sin perjudicar la carga. Usar al añadir o revisar un vídeo en la web.
---

# Montar un vídeo

Todo el detalle está en el flujo [`flujos/videos.md`](../../flujos/videos.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.**

**Vídeos siempre en Cloudflare R2, nunca en el repositorio** (ni en `public/`).

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| El archivo del vídeo | Montar la sección con el **póster como imagen fija**; el vídeo entra cuando llegue |
| Vídeo subido a R2 (lo sube Belén) | Pedírselo con el nombre de fichero sin espacios |
| ¿Se comprime? | **Lo decide Belén por proyecto**; se anota el porqué en los datos y en `CHECKLIST.md`. Si se comprime, `-movflags +faststart` |
| ¿Fondo decorativo o contenido? | Preguntar: cambia el arranque y la pausa |
| Solo en Vimeo o YouTube | Pedir el archivo. Si el embed es imprescindible → consentimiento y módulo [multimedia](../../modulos/multimedia.md) |

## 2. Pasos

1. **URL en un solo sitio**: `R2_BASE` y cada vídeo en `src/config/media.ts`;
   los componentes y los datos solo leen de ahí.
2. **Datos**: `VideoMedia` en `src/data/` con `autoplay`, póster (`src` + `alt`)
   y etiquetas `play` y `pause` traducidas.
3. **Marcado** (flujo §3 paso 3): `muted playsinline loop`,
   `preload="metadata"` (o `none`), **sin atributo `autoplay`**, `aria-label` con
   el `alt` del póster o `aria-hidden` si es decorativo. Póster optimizado con
   `getImage` o primer fotograma. Sin vídeo en los datos, `<Picture>`.
4. **Arranque y pausa**: copiar `clays-of-the-world/src/scripts/video-autoplay.ts`
   e importarlo en el componente. Contrato: `data-video`, `data-autoplay`,
   `data-pause-label` y `<button data-play aria-controls="id-del-vídeo">`.
5. **Hero** (flujo §3 paso 5): la imagen es el LCP; vídeo con `data-src`,
   `preload="none"`, asignado tras `load`, solo en escritorio y sin «reducir
   movimiento».
6. **Pausa (WCAG 2.2.2)**: vídeo de contenido con botón de pausa siempre. Un
   hero sin botón es una excepción que **decide Belén** y se anota en el componente.
7. **Texto sobre vídeo**: contraste medido sobre los fotogramas más claros.

## 3. Al terminar

- Comprueba con `build` + `preview` o en la URL temporal (no en `dev`):
  - Al cargar se ve el póster; en móvil no se descarga lo que no se muestra.
  - Fuera de pantalla se para; al volver sigue; la pausa manual se respeta.
  - Con «reducir movimiento» no arranca; sin JavaScript se ve el póster.
  - El botón funciona con teclado y cambia de nombre.
- `git ls-files '*.mp4'` no devuelve nada.
- Checklists **Construcción** y **Revisión** del flujo §7.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
