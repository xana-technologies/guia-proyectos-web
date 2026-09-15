# Fases

El proceso de cualquier web, en orden. Cada fase es corta: qué es, qué se hace
según lo que traiga el cliente, quién lo hace, qué queda hecho y qué flujos
usa. El detalle de cada tarea está en [`../flujos/`](../flujos/README.md).

La columna **Necesita antes** dice qué puede adelantarse. Por ejemplo, la
marca (04) puede empezar justo después del arranque, a la vez que la
investigación.

| Fase | Qué es | Escenarios | Necesita antes |
| --- | --- | --- | --- |
| [00 · Arranque](00-arranque.md) | Brief, qué trae el cliente, accesos, cuentas, inventario, web antigua | — | — |
| [01 · Investigación](01-investigacion.md) | Competencia y sector | 🟢 en el briefing · 🔴 la hacemos | 00 |
| [02 · Estructura](02-estructura.md) | Páginas y secciones | 🟢🟡🔴 | 01 |
| [03 · Contenidos y SEO](03-contenidos-seo.md) | Palabras clave → textos → metadatos · cierre del inventario | 🟢🟡🔴 | 02 |
| [04 · Marca](04-marca.md) | Manual → web, o brand board | 🟢🟡🔴 | 00 |
| [05 · Design system](05-design-system.md) | Referencias visuales → tokens → componentes → moldes → styleguide | — | 04 |
| [06 · Diseño en Figma](06-diseno-figma.md) | A · diseño previo · B · código primero y Figma para ajustar · C · sin Figma | A/B/C | 02 y 05 |
| [07 · Construcción](07-construccion.md) | URL temporal, maquetación, imágenes, vídeos, formulario, legales, cookies, SEO técnico, rondas de feedback | — | 03, 05 (y 06 en A) |
| [08 · Revisión antes de publicar](08-revision.md) | Comprobar todo en la URL temporal | — | 07 |
| [09 · Publicación](09-publicacion.md) | Dominio, Search Console, formulario definitivo, analítica e informe | — | 08 |
| [10 · Entrega y mantenimiento](10-entrega-mantenimiento.md) | Entrega al cliente, documentación interna, mantenimiento técnico | — | 09 |

## Escenarios

| | Significa |
| --- | --- |
| 🟢 | El cliente lo da completo: se usa y se anotan los cambios con el porqué |
| 🟡 | Lo da a medias: se completa lo que falta y se devuelve para aprobar |
| 🔴 | No lo da: lo hacemos nosotros y el cliente lo aprueba |

Lo que completamos o proponemos **lo aprueba el cliente antes de pasar a la
fase que depende de ello**. Saber desde el arranque qué escenario toca en cada
fase condiciona el trabajo.
