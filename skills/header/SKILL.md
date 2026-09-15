---
name: header
description: Montar la cabecera de una web de Xana — logo con las reglas de la marca, navegación con los datos de la estructura, CTA, menú móvil accesible (aria-expanded, aria-controls, Escape devuelve el foco), comportamiento fijo o sticky sin tapar las anclas ni dejar el logo sobre foto. Usar al construir o cambiar la cabecera o el menú.
---

# Cabecera

Todo el detalle está en el flujo [`flujos/maquetacion.md`](../../flujos/maquetacion.md)
§8 (menú móvil) y §9 (cabecera) (las rutas de esta skill son relativas a su
carpeta). Implementación de referencia: `clays-of-the-world/src/components/Header.astro`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Entradas del menú y anclas | `docs/02-estructura/estructura.md` (columna Menú) | Preguntar: sin etiquetas no hay menú |
| CTA principal y su destino | Estructura § CTA | Un CTA sin destino bloquea: preguntar |
| Versiones del logo y uso sobre foto u oscuro | `brand/README.md` | Barra sólida por defecto |
| ¿Hero con foto o vídeo debajo? | Estructura, diseño | Cambia si la cabecera puede ser transparente |
| Idiomas | `CLAUDE.md` | El punto de corte del menú puede depender del idioma |
| Nodo de Figma | Belén | Si lo hay, valores con `xana:figma-a-seccion` |

## 2. Pasos

1. **Datos**: etiquetas del menú, rótulos de abrir y cerrar, CTA y textos
   ocultos en `src/data/`; nada escrito en el componente ni en el script (los
   rótulos del script viajan en `data-label-open` / `data-label-close`).
2. **Logo** con `Logo` y las reglas de la marca: **nunca directamente sobre foto
   ni sobre oscuro**. Barra sólida o versión en blanco; transparente sobre el
   hero solo si lo que lleva encima no es el logotipo.
3. **Comportamiento** (flujo §9):
   - Fija con `scroll-padding-top` en `html` para que las anclas no queden debajo.
   - `sticky` si no cambia de alto.
   - Si se condensa al bajar, `fixed` con un separador del alto original.
4. **Orden**: lo que se hace (CTA) antes de lo que se consulta (redes, idioma).
5. **Menú móvil accesible** (flujo §8): botón con `aria-expanded` y
   `aria-controls`, rótulo que cambia, **Escape cierra y devuelve el foco al
   botón**, se cierra al seguir un enlace. En una landing de anclas vale
   `<details>` sin JavaScript.
6. **Sin JavaScript** el menú es usable.
7. Scroll con `requestAnimationFrame` y listeners `passive`.
8. Iconos SVG con `aria-hidden` y nombre accesible en el enlace.

## 3. Al terminar

- A 360, 768, 1024 y 1440 px, y con el idioma más largo: el menú no se parte.
- Con teclado: skip-link, foco visible, Escape.
- Sin JavaScript y con `prefers-reduced-motion`.
- Pulsar cada ancla: el título de la sección no queda debajo de la cabecera.
- Belén revisa en la URL temporal. Propón el commit y espera el sí
  ([forma de trabajo §5](../../forma-de-trabajo.md)).
