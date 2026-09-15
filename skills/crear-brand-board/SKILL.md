---
name: crear-brand-board
description: Hacer el brand board visual de la marca de un cliente de Xana — logo y usos, paleta con nombres y hex, pareja tipográfica, fotografía, iconos, recurso gráfico, tono y ejemplos aplicados a piezas de la web — para enseñárselo al cliente. Usar cuando la marca ya está decidida (manual traducido o propuesta elegida) y hay que presentarla en un documento visual.
---

# Crear el brand board

Todo el detalle está en el flujo [`flujos/brand-board.md`](../../flujos/brand-board.md),
apartados «Qué lleva el brand board» y «Herramientas» (las rutas de esta skill
son relativas a su carpeta).

El brand board **presenta** una marca decidida; no la decide. Si todavía no hay
marca, antes va `xana:crear-manual-marca` (sin manual) o
`xana:traducir-manual-marca` (con manual).

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Marca decidida | `brand/README.md` | Parar: primero la marca |
| Logos en SVG | `brand/logo/` | Hacerlo con lo que haya y marcar qué falta |
| Fotos | Del cliente o de stock ya elegidas | Huecos marcados, sin inventar fotos «de la empresa» |
| Herramienta | Preguntar a Belén | Figma si lo va a editar a mano o pasará a la fase 06; canvas de diseño si lo quiere editable sin Figma; Pencil para ejemplos en `.pen` |

## 2. Qué lleva

Tabla «Qué lleva el brand board» del flujo:

- **Logo y usos**: versiones, en color y en blanco, sobre qué fondos va cada una.
- **Paleta**: cada color con nombre y hex, y su papel.
- **Pareja tipográfica** y muestra de jerarquía.
- **Fotografía**: qué se fotografía, luz, encuadre, qué evitar.
- **Iconos** y **recurso gráfico**, si los hay.
- **Tono de voz**: tres o cuatro rasgos.
- **Ejemplos aplicados**: la marca en cabecera, hero, una sección, botón y pie.
  Es lo que el cliente entiende de verdad.

Los datos (hex, nombres, pesos) salen **literales** de `brand/README.md`: el
brand board no introduce valores nuevos.

## 3. Pasos

1. Montarlo en la herramienta que haya elegido Belén, con los textos reales de
   la web.
2. Si se usa la skill de Figma o la de canvas, cargarla antes de escribir en
   esa herramienta.
3. Guardar el enlace o la exportación en `brand/brand-board.*` y anotarlo en
   `brand/README.md`.

## 4. Al terminar

- Belén lo revisa y lo envía al cliente. **El cliente aprueba antes del design
  system**; se anota con fecha en `brand/README.md`.
- Checklist **04 · Marca** del flujo (fotografía, iconos, recurso, tono y
  ejemplos aplicados).
