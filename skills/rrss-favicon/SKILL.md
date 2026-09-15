---
name: rrss-favicon
description: Generar la imagen para redes sociales (public/og-image.jpg, 1200×630) y los favicons (favicon.svg, favicon.ico 16/32/48, apple-touch-icon.png 180 opaco) de una web de Xana a partir del logo y de una foto real. Usar al preparar la imagen que sale al compartir la web o los iconos de la pestaña.
---

# Imagen para redes y favicons

Todo el detalle está en el flujo [`flujos/imagenes.md`](../../flujos/imagenes.md)
§3 paso 7 (las rutas de esta skill son relativas a su carpeta). Cómo se enlazan
en el `<head>`: [`flujos/seo-tecnico.md`](../../flujos/seo-tecnico.md) §3 paso 2.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Logo en SVG (y versión en blanco) | `brand/logo/` | Pedirlo. Un «SVG» con un PNG dentro no es un vector: avisar |
| Reglas del logo sobre foto | `brand/README.md` | Si el manual prohíbe el logo sobre foto, pastilla |
| Foto para la imagen de redes | `src/assets/` | Mejor real y del cliente; si no, preguntar a Belén |
| ¿Varios idiomas ahora o después? | `CLAUDE.md` | Sin texto en la imagen |

## 2. Imagen para redes

1. Copiar y adaptar el script más cercano (tabla del flujo §3 paso 7), en
   **Node + `sharp`**. Si se copia el de xanatechnologies, cambiar su ruta
   absoluta escrita en el script.
2. **1200×630, JPEG**, en `public/og-image.jpg`.
3. **Sin texto** si la web tiene o tendrá varios idiomas; con un solo idioma se
   puede añadir el lema.
4. Logo con las reglas del manual (pastilla blanca sobre foto si hace falta).

## 3. Favicons

En `public/`:

| Fichero | Medida | Nota |
| --- | --- | --- |
| `favicon.svg` | vectorial | Del logo en SVG |
| `favicon.ico` | 16, 32 y 48 px | Fondo transparente |
| `apple-touch-icon.png` | 180 px | **Fondo opaco** (iOS pinta la transparencia de negro) y con aire para el redondeo |

A tamaños tan pequeños, normalmente el isotipo, no el logo entero.

## 4. Al terminar

- Enseña a Belén la imagen de redes y los favicons: **los aprueba ella**.
- Comprueba que `Base.astro` los enlaza (`og:image` absoluta con `width`,
  `height` y `alt`; `.ico` con `sizes` antes del SVG) → `xana:montar-seo-tecnico`.
- Cuando haya URL temporal, compartirla en una red o mensajería para ver que sale.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
