---
name: preparar-imagenes
description: Preparar y colocar las imágenes de una web de Xana — ver cada imagen y proponer dónde va, renombrarla (seccion-descripcion-01), apuntarla en el inventario, optimizarla a máster WebP (recorte, ancho ×2 sin ampliar, escalera de calidades, sin metadatos) y colocarla con <Picture> AVIF/WebP, alt desde los datos y solo la primera eager. Usar cuando llegan imágenes del cliente, de Magnific o de IA, o al revisar el peso de las imágenes.
---

# Preparar las imágenes

Todo el detalle está en el flujo [`flujos/imagenes.md`](../../flujos/imagenes.md)
§3 pasos 2 a 6 (las rutas de esta skill son relativas a su carpeta). **Léelo
antes de empezar.** Plantilla del inventario:
[`plantillas/proyecto/inventario-imagenes.md`](../../plantillas/proyecto/inventario-imagenes.md).

**No hay imagen que entre en la web sin pasar por el inventario y la
optimización**, venga de donde venga.

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| `docs/imagenes/originales/` en `.gitignore` | Añadirlo **antes** de copiar nada: lo que entra en git se queda |
| Huecos de la estructura y copy | Preguntar dónde va cada una |
| Script de optimización en `scripts/` | Copiar el más cercano (tabla del flujo §3 paso 5) y completarlo. **Node + `sharp`** |

## 2. Pasos

1. **Entrada**: todo lo que llega, sin tocar, a `docs/imagenes/originales/`.
   Logos aparte, a `brand/logo/`.
2. **Selección** — Claude propone, Belén decide: **ver cada imagen** (no
   asignarla por el nombre del fichero), proponer dónde va con el porqué, y
   señalar las que no llegan a la resolución del hueco o se parecen demasiado.
3. **Renombrar** `seccion-descripcion-01`: minúsculas, sin acentos, con guiones,
   describiendo lo que se ve.
4. **Inventario** en `docs/imagenes/inventario.md`: origen (`cliente`,
   `Magnific` + referencia, `IA`), fichero original, nombre final, dónde se usa,
   si es **provisional** y derechos.
5. **Optimizar** a máster WebP en `src/assets/<seccion>/` (flujo §3 paso 5):
   - Recorte al formato del hueco.
   - Ancho máximo = hueco ×2 (2880 / 1920 / 1600 / 1200). **Nunca se amplía.**
   - Escalera de calidades: la más alta que pese menos que el original.
   - Orientación EXIF aplicada, CMYK → sRGB, y **metadatos fuera** (sobre todo GPS).
   - En Windows, leer a buffer antes de reescribir el mismo fichero.
6. **Colocar** (flujo §3 paso 6):
   - Importada desde `src/data/`, con su `alt` al lado. Decorativa, `alt=""`.
   - `<Picture formats={['avif','webp']} fallbackFormat="webp">` con `widths` y
     `sizes` del hueco real.
   - **Solo la primera imagen visible** con `loading="eager"` y
     `fetchpriority="high"`; el resto `lazy` y `decoding="async"`.
   - `public/` solo para lo que Astro no procesa, ya optimizado a mano.

## 3. Reglas que no se saltan

- **Ningún texto atribuye una foto de stock o de IA al cliente** (ni `alt`, ni
  pies, ni titulares al lado).
- Una imagen no se repite en la web sin anotarlo.
- **Nada provisional se publica.**
- Al retirar una imagen, se borra de `src/assets/` y del inventario.

## 4. Al terminar

- **Peso con `npm run build` + `npm run preview`**, nunca en `dev`: sin PNG
  generados en `dist/`.
- Recortes correctos a 375 px, 768 px y escritorio.
- Si se relanzó el script de másters, `git status src/assets` y devolver lo que
  no se quería cambiar.
- Checklist **Construcción (07)** del flujo §7.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
