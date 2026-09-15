---
name: figma-a-seccion
description: Construir o rehacer una sección de una web de Xana a partir de un nodo de Figma con los valores exactos (get_design_context), mapeados a los tokens y moldes existentes, con el nodo citado y los desvíos anotados. Usar cuando Belén pasa un enlace a un nodo o frame de Figma para maquetarlo o para ajustar una sección ya construida.
---

# De Figma a una sección

Todo el detalle está en el flujo [`flujos/diseno-figma.md`](../../flujos/diseno-figma.md)
§2, §5 y §6 (las rutas de esta skill son relativas a su carpeta). **Léelo antes
de empezar.**

Antes de leer el nodo carga la skill **`figma-design-to-code`** del plugin de
Figma: es obligatoria antes de `get_design_context`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Enlace al nodo | Mensaje de Belén | Pedirlo. Anotar el fichero de Figma en `CLAUDE.md` si no está |
| Caso A, B o C | `CLAUDE.md` | Preguntar |
| Tokens y moldes | `src/styles/tokens.css`, `src/components/sections/` | Primero el design system |
| Textos de la sección | `src/data/` | Los del nodo no son el copy: manda el copy aprobado |
| ¿Hay versión móvil en Figma? | El fichero | Proponer la adaptación con las reglas de siempre (flujo · Diseño sin versión móvil) |

## 2. Pasos

1. **Localizar el nodo** con `get_metadata` si hace falta.
2. **Leer los valores exactos** con `get_design_context`. `get_screenshot` sirve
   para comparar composición, **nunca para sacar valores**.
3. **Mapear cada valor a un token existente.** Un espaciado entre dos escalones
   se redondea al más cercano. Si el valor no existe, **se propone un token** con
   su porqué y su contraste; nunca a pelo en el componente.
4. **Mapear a un molde existente** antes de crear una composición nueva.
5. **Anchos de texto en `ch`**, no en píxeles, para que aguanten la traducción.
6. **Medir lo que va sobre foto** (velos, texto) sobre la foto real.
7. **Citar el nodo** en la cabecera del componente (`Montado desde Figma (nodo …)`).
8. **Desvíos anotados con su porqué** en la cabecera (contraste AA, texto que no
   cabe, elemento legal que falta, accesibilidad).

## 3. Feedback sobre una sección ya hecha

- **Cambio mínimo**: se toca lo pedido, no se rediseña.
- **Si el cambio toca un componente o token compartido, se aísla** para esa
  sección y se avisa del efecto en las demás.

## 4. Al terminar

- Verificar en el navegador, escritorio y móvil. Si un estilo no cambia en
  `dev`, mira [trampas](../../trampas-y-lecciones.md#cambios-de-estilo-que-no-se-ven-en-dev-ni-recargando).
- Checklist **Por sección que viene de Figma** del flujo §9.
- En caso A, si un desvío cambia lo que aprobó el cliente, enseñárselo a Belén
  antes de la URL temporal.
- Lo que quede sin decidir (móvil de una pieza compleja) → `CLAUDE.md` §
  Cuestiones abiertas.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
