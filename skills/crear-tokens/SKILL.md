---
name: crear-tokens
description: Crear src/styles/tokens.css de una web de Xana a partir de la marca — colores en tres bloques (marca literal, extensiones con ratio, semánticos con contraste anotado), espaciado correlativo, tipografía fluida con clamp, radios, sombras y movimiento a 0 con reduced-motion — más fuentes alojadas con fontsource, reset.css y base.css. Usar al montar el design system o al añadir o cambiar un token.
---

# Crear los tokens

Todo el detalle está en el flujo [`flujos/design-system.md`](../../flujos/design-system.md)
§2, §4, §5 y §8 (las rutas de esta skill son relativas a su carpeta). **Léelo
antes de empezar.** Implementación de referencia:
`clays-of-the-world/src/styles/tokens.css`.

**`tokens.css` es la única fuente de verdad**: ningún componente escribe un
color, tamaño de fuente, espaciado o duración a pelo.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Marca aprobada | `brand/README.md` | Parar: tokens sobre una marca sin aprobar se rehacen |
| Dirección visual decidida | `docs/05-referencias-visuales/referencias.md` | Avisar: el design system no se cierra sin ella |
| ¿Figma del cliente con design system? | `CLAUDE.md` | Si lo hay, los tokens salen **de ese Figma** con `get_variable_defs` y `get_design_context` (cargar antes `figma-design-to-code`), no del PDF ni a ojo |
| ¿Modo oscuro? | Brief, marca | Solo se redefine el bloque semántico |

## 2. Pasos

1. **Colores en tres bloques**, aunque el segundo quede vacío (flujo §4.1):
   1. Paleta de marca **literal**, que no se toca ni se amplía.
   2. Extensiones para pantalla, cada una con su porqué y su ratio.
   3. Tokens semánticos (`--color-*`), los únicos que usan los componentes,
      con el **contraste anotado** en el comentario. Si una combinación no cumple
      a sabiendas, se escribe que no cumple y por qué.
2. **Contraste calculado**, no estimado. Sobre foto, medido sobre la foto real
   con su velo.
3. **Espaciado correlativo sin huecos** (`--space-1` … `--space-10`), más
   `--section-y`, `--gutter`, `--container-max` y `--measure`.
4. **Tipografía**: cuerpos pequeños fijos y titulares con `clamp`, con los
   píxeles de los dos extremos en un comentario; pesos, interlineados y
   tracking como tokens.
5. **Radios, sombras, curvas y duraciones**; con `prefers-reduced-motion`,
   duraciones, recorrido y escalonado a `0` en los tokens (no un
   `!important` global).
6. **Fuentes alojadas** (flujo §5): fontsource importado en el layout base, o
   `woff2` en `public/fonts/` con `preload`. **Nunca Google Fonts.**
7. **`reset.css` y `base.css`**: reset sin marca; base con tipografía,
   `.container`, `.section`, `.eyebrow`, `.skip-link`, titulares con
   `text-wrap: balance` y párrafos con `pretty` (flujo §8).

## 3. Al terminar

- Busca en los componentes hex, `px` de fuente o espaciados a pelo, y tokens
  que no existen (anulan la declaración sin avisar).
- Checklist **Tokens y fuentes** del flujo §12.
- Los componentes y el styleguide van con `xana:componentes-base`.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
