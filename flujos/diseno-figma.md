# Flujo · Diseño en Figma

El espacio de diseño del proyecto y cómo pasa Claude de Figma al código y del
código a Figma. **Siempre después del [design system](design-system.md)**: en
Figma se diseña con sus tokens y componentes, no con valores sueltos. Casi
todos los proyectos tienen Figma, pero no se usa igual en todos: ver los casos.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [02 · Estructura](../fases/02-estructura.md) | Wireframes o diagramas en FigJam, si se hacen ahí |
| [05 → 06](../fases/06-diseno-figma.md) | Llevar a Figma la librería del design system (tokens y componentes) |
| [06 · Diseño en Figma](../fases/06-diseno-figma.md) | Caso A: diseño completo antes de construir |
| [07 · Construcción](../fases/07-construccion.md) | Caso B: lo que no sale bien a la primera se ajusta en Figma y se rehace con sus valores |

## 2. Qué tenemos (escenarios)

| Caso | Cuándo | Cómo se trabaja | Qué aprueba el cliente |
| --- | --- | --- | --- |
| **A · Diseño previo** | El cliente trae Figma, o quiere aprobar el aspecto antes de construir | Se diseña → el cliente aprueba → Claude construye con los valores exactos | **El diseño** |
| **B · Código primero, Figma para ajustar** *(lo habitual)* | Claude puede construir con marca, estructura y design system | Claude construye; la sección que no convence se lleva a Figma, Belén la ajusta y Claude la rehace con los valores del nodo | **La URL temporal** |
| **C · Sin Figma** | Webs muy simples | Se itera sobre la web | **La URL temporal** |

Ejemplos: clays-of-the-world es B (secciones montadas desde nodos concretos:
`sections/Note.astro`, nodo `61:1196`; `sections/Contact.astro`, nodo `99:102`);
xanasystem-web cita nodos del Figma «XANA SYSTEM» en `src/styles/tokens.css`
(`222:3781`, degradado de progreso); festival-viento sacó sus tokens de un
design system en Figma (`src/styles/global.css`).

**Si el cliente trae Figma (A)**, los tokens se sacan de ese Figma
([design system §2](design-system.md#2-qué-tenemos-escenarios)) antes de
diseñar o construir nada.

## 3. Qué skill o herramienta usar y cuándo

Las **skills** del plugin de Figma son instrucciones que se cargan antes de
trabajar. **Se activan solas** cuando la tarea encaja con su descripción, y se
pueden lanzar a mano escribiendo `/nombre` (por ejemplo `/figma-use`). Las
**herramientas** son las llamadas que leen o escriben en Figma.

### Escribir en Figma

| Skill | Cuándo | Fase |
| --- | --- | --- |
| `figma-use` | **Siempre antes de escribir en Figma** con `use_figma` (crear o editar nodos, variables, componentes, auto-layout). Reglas base obligatorias: sin ella fallan cosas difíciles de depurar. Se carga junto a las demás skills de escritura | 02, 05→06, 06, 07 |
| `figma-generate-library` | Crear o actualizar en Figma **la librería del design system**: variables con los tokens, componentes con variantes enlazadas a variables. También para un solo componente | 05 → 06 |
| `figma-generate-design` | Llevar **una página o sección de la web a Figma** montándola con los componentes y variables de la librería. Es el camino del caso B: la sección construida pasa a Figma para ajustarla | 06, 07 (B) |
| `figma-code-connect` | **Mapear componentes de Figma ↔ código** (ficheros `.figma.ts`), para que Figma enseñe el componente Astro que corresponde | 06, cuando hay librería |
| `figma-use-figjam` | Wireframes y diagramas en **FigJam** con `use_figma` | 02 |
| `figma-generate-diagram` | Diagramas (flujo, arquitectura, secuencia) con `generate_diagram` en FigJam | 02 |
| `figma-create-new-file` | Antes de crear un fichero nuevo (diseño o FigJam) con `create_new_file` | 02, 05→06 |

### Leer de Figma para construir

| Herramienta · skill | Para qué |
| --- | --- |
| `figma-design-to-code` *(skill)* | Se carga **antes de `get_design_context`** cuando la tarea es implementar un diseño de Figma |
| `get_design_context` | **Valores exactos de un nodo**: colores, degradados, sombras, tamaños, espaciados. Es la fuente de lo que se escribe en CSS |
| `get_screenshot` | Ver el nodo tal cual para comparar composición; **no** para sacar valores |
| `get_variable_defs` | Variables (tokens) que usa un nodo o el fichero: de aquí salen los tokens cuando el cliente trae Figma |
| `get_metadata` | Estructura del fichero o página (ids, nombres, tamaños) para localizar el nodo antes de leerlo |

## 4. Pasos y quién

### Siempre

1. **Belén**: crea o comparte el fichero. **Claude** apunta el enlace en el
   `CLAUDE.md` del proyecto (fichero y, si hay, página de la librería).

### Paso 05 → 06: la librería

2. **Claude** (`figma-use` + `figma-generate-library`): variables con los
   tokens de `tokens.css` (misma separación en capas, mismos nombres) y los
   componentes base con sus variantes. Belén revisa.

### Caso A · Diseño previo

3. **Belén** diseña con la librería (escritorio **y** móvil) → **cliente
   aprueba** el diseño.
4. **Claude**, sección a sección: localiza el nodo (`get_metadata`), lee los
   valores (`get_design_context`), mapea a tokens y moldes (§5) y construye.
5. Anota los desvíos (§6) y los comenta con Belén antes de enseñar la URL.

### Caso B · Código primero (lo habitual)

3. **Claude** construye la sección con marca, estructura y design system
   ([maquetación](maquetacion.md)).
4. Si Belén o el cliente no la ven clara: **Claude** la lleva a Figma
   (`figma-use` + `figma-generate-design`) o **Belén** la redibuja.
5. **Belén** ajusta en Figma y pasa el enlace al nodo.
6. **Claude** rehace la sección con los valores exactos de ese nodo (§5).
7. Aprobación del cliente **sobre la URL temporal**.

### Caso C · Sin Figma

3. Se itera sobre la URL temporal. Los ajustes en píxeles que da Belén se
   aplican tal cual ([forma de trabajo](../forma-de-trabajo.md#2-cómo-se-itera)).

## 5. Del nodo a la sección

1. **Valores exactos, no a ojo.** Se leen con `get_design_context` antes de
   escribir CSS. Aproximar un degradado mirando la captura provocó varios
   rechazos en la sección «Cómo trabajamos» de xanasystem-web; cuadró al leer
   el `linear-gradient(145deg, #4aa3da, #a95cb5, #f453a4)` del nodo
   (memoria `figma-valores-exactos`).
2. **Mapear a tokens existentes.** Cada valor del nodo se busca en
   `tokens.css`. Si un espaciado de Figma cae entre dos escalones, se redondea
   al token más cercano (xanasystem-web, `docs/design.md` § Espaciado).
3. **Si el valor no existe como token, se propone un token**, con su porqué y
   su contraste; no se escribe a pelo en el componente (clays `CLAUDE.md` §
   Design system). Ejemplo: el velo claro del formulario de clays es
   `--scrim-light` en `tokens.css`, no un `rgba` en `Contact.astro`.
4. **Mapear a moldes.** Antes de crear una composición nueva, comprobar si la
   del nodo es un molde existente con otros datos
   ([design system §7](design-system.md#7-moldes-de-sección)).
5. **Medidas de composición en unidades que aguanten la traducción.** Un ancho
   de texto de Figma en píxeles pasa a `ch`: «379px en el Figma. En `ch` y no
   en px, para que la traducción parta igual» (clays `sections/Contact.astro`).
6. **Comentario con la referencia al nodo** en la cabecera del componente
   (`Montado desde Figma (nodo 61:1196)`, clays `sections/Note.astro`).
7. Verificar en el navegador teniendo en cuenta que el CSS scoped puede estar
   desactualizado en `dev` →
   [trampa](../trampas-y-lecciones.md#cambios-de-estilo-que-no-se-ven-en-dev-ni-recargando).

### Diseño sin versión móvil

Pasa a menudo: el wireframe de clays-of-the-world es solo escritorio
(`CLAUDE.md` § Cuestiones abiertas). Entonces:

- Claude **propone la adaptación** con las reglas de siempre: columnas que
  pasan a una, imagen arriba en las bandas, menú desplegable
  ([maquetación](maquetacion.md#5-móvil)).
- Lo que no tenga una solución evidente (un mosaico, una tabla, un carrusel)
  se **anota como cuestión abierta** en el `CLAUDE.md` y se decide con Belén,
  sin inventar el diseño («queda por definir el mosaico de la galería», clays).
- En el caso A, si el cliente aprueba sin móvil, se le dice que la adaptación
  la decide Xana.

## 6. Feedback y desvíos

- **Cambio mínimo ante feedback puntual.** Se toca lo que se ha pedido; no se
  rediseña la sección ([forma de trabajo](../forma-de-trabajo.md#2-cómo-se-itera)).
- **Si el cambio toca un componente compartido, se aísla**: copia propia para
  la sección pedida y las demás intactas, avisando del efecto. Los mockups del
  PIM de xanasystem-web los compartían tres secciones, y rehacerlos para una
  rompía las otras dos (memoria `alcance-cambios-seccion`). Lo mismo con un
  token semántico: si el cambio es para una sección, no se cambia el token
  global.
- **Los desvíos del diseño se anotan con su porqué**, en la cabecera del
  componente y, si afectan a un token, en su comentario:

  ```astro
  /**
   * Cuatro desvíos del Figma, decididos a propósito (14-09-2026), los cuatro por
   * contraste:
   *  - **El velo va al 0,75 y no al 0,6.** Con 0,6 la entradilla y el antetítulo
   *    ...
   */
  ```
  *(clays-of-the-world, `src/components/sections/Contact.astro`)*

  Motivos habituales: contraste AA, texto que no cabe al traducir, elemento
  legal obligatorio que el diseño no tiene (la casilla de consentimiento en
  Clays), accesibilidad.
- Si un desvío cambia lo que el cliente aprobó (caso A), se le enseña antes
  de publicar.

## 7. Dónde queda cada cosa

| Qué | Dónde |
| --- | --- |
| Enlace al fichero de Figma (y a la página de la librería) | `CLAUDE.md` del proyecto |
| Nodo del que sale una sección | Comentario en la cabecera del componente |
| Desvíos del diseño y su porqué | Cabecera del componente; comentario del token si lo toca |
| Tokens nuevos que pide el diseño | `src/styles/tokens.css`, con contraste anotado |
| Decisiones pendientes (móvil sin diseñar, piezas por definir) | `CLAUDE.md` § Cuestiones abiertas y `CHECKLIST.md` |
| Mapeos Figma ↔ código (si se usa Code Connect) | Ficheros `.figma.ts` junto a los componentes |

## 8. Reglas y trampas

- **Enlace a Figma en el `CLAUDE.md` del proyecto**, siempre.
- **Nunca aproximar a ojo desde una captura** →
  [trampa](../trampas-y-lecciones.md#los-colores-y-degradados-copiados-de-figma-no-cuadran).
- **Ningún valor de Figma a pelo en un componente**: token existente o token
  propuesto.
- Un espaciado de Figma que no existe en la escala, escrito como token
  inexistente, deja el `gap` a 0 →
  [trampa](../trampas-y-lecciones.md#todo-sale-muy-junto-columnas-pegadas-cabeceras-sin-padding).
- `figma-use` antes de cualquier `use_figma`; `figma-design-to-code` antes de
  `get_design_context`.
- Lo que Figma dibuja encima de una foto hay que **medirlo** sobre la foto
  real: el velo de 0,6 del Figma de Clays no llegaba a AA (clays
  `--scrim-light`).
- Un texto de Figma pensado en inglés o en español puede no caber traducido:
  anchos en `ch`, nada de anchos fijos sobre el texto ([maquetación](maquetacion.md#5-móvil)).
- En el caso B, Figma **no bloquea**: la web avanza y Figma entra solo en lo
  que no convence.

## 9. Checklist

**Siempre**
- [ ] Enlace a Figma en el `CLAUDE.md` del proyecto
- [ ] Caso (A, B o C) anotado en el `CLAUDE.md`
- [ ] Librería en Figma generada desde `tokens.css` (si hay Figma)

**Por sección que viene de Figma**
- [ ] Valores leídos con `get_design_context`, no de la captura
- [ ] Cada valor mapeado a un token; los que faltaban, propuestos como token
- [ ] Molde existente reutilizado o composición nueva justificada
- [ ] Nodo citado en la cabecera del componente
- [ ] Desvíos anotados con su porqué
- [ ] Versión móvil: del diseño o propuesta y aprobada por Belén
- [ ] Cambios de feedback mínimos; componentes compartidos aislados

**Aprobación**
- [ ] Caso A: el cliente aprueba el diseño antes de construir
- [ ] Casos B y C: el cliente aprueba sobre la URL temporal
