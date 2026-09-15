---
name: traducir-manual-marca
description: Traducir el manual de identidad de un cliente de Xana a decisiones de web en brand/README.md — versiones del logo y dónde va cada una, uso sobre foto y fondo oscuro, paleta con papel y contraste medido, extensiones para pantalla, tipografías con licencia web y jerarquía por rol. Usar cuando el cliente trae manual de marca (o solo logo y colores) en la fase de marca.
---

# Traducir el manual de marca

Todo el detalle está en el flujo [`flujos/brand-board.md`](../../flujos/brand-board.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar**: la tabla «Lo que dice el manual → la decisión de web» y el formato
de `brand/README.md` están ahí.

Si el cliente **no tiene manual**, no es esta skill: es `xana:crear-manual-marca`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Manual (PDF) | `brand/` o material del cliente | Escenario 🔴 → `xana:crear-manual-marca` |
| Logos en SVG, versión en blanco, favicon | `brand/logo/` | **Pedirlos y dejarlo bloqueado** en `CHECKLIST.md`. Un «SVG» con un PNG dentro no vale |
| Archivos o licencia web de las tipografías | Cliente | Sustituto a decidir por Belén (laboratorio) |
| ¿El cliente trae Figma con design system? | `CLAUDE.md` del proyecto | Si lo trae, los tokens salen de ese Figma (`xana:crear-tokens`) |

## 2. Pasos

1. Escribir `brand/README.md` con el formato del flujo: qué dice la marca y
   **qué decisión de web sale** de cada cosa.
2. **Logo**: cada versión con su uso en web (cabecera, pie, móvil estrecho,
   favicon) y qué pasa sobre foto y sobre oscuro. Solo logos oficiales; los
   antiguos, anotados como prohibidos.
3. **Paleta** con nombre, hex y papel. **Contraste calculado, no estimado**, de
   cada combinación de texto. Los colores del manual **no se tocan**: lo que haga
   falta para pantalla va aparte como extensión, con su ratio.
4. **Tipografía**: licencia web y de dónde se carga (fontsource o `woff2`
   propio, nunca Google Fonts), y tabla de roles con peso y tamaño.
5. En 🟡 (solo logo y colores): completar fotografía, iconos y tono con las
   piezas del brand board, marcado como propuesta.
6. Si hay que elegir entre opciones (sustituto tipográfico, extensión de color),
   **las decide Belén**, viéndolas en una página de laboratorio con contenido
   real y `noindex, nofollow` (flujo · Páginas de laboratorio).

## 3. Al terminar

- Recorre la checklist **04 · Marca** del flujo.
- Pide a Belén que lo revise. Las decisiones que cambian el manual las
  **aprueba el cliente antes del design system**; lo aprobado se anota con fecha.
- Después sigue `xana:ordenar-referencias` y `xana:crear-tokens`.
