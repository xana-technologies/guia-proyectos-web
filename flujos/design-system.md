# Flujo · Design system

Convertir la marca en piezas reutilizables antes de maquetar: tokens,
tipografías, componentes base, moldes de sección y una página interna que lo
enseña todo. **Va en todas las webs.** La implementación de referencia es
**clays-of-the-world** (`src/styles/tokens.css`, `src/components/`,
`src/pages/styleguide.astro`); hasta que exista el repo plantilla, se copia
de ahí.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [04 · Marca](../fases/04-marca.md) | Llega el manual o el brand board: es la entrada de este flujo |
| [05 · Design system](../fases/05-design-system.md) | Referencias → tokens → fuentes → componentes → moldes → styleguide |
| [06 · Diseño en Figma](../fases/06-diseno-figma.md) | La librería de Figma se genera **desde** estos tokens ([diseño en Figma](diseno-figma.md)) |
| [07 · Construcción](../fases/07-construccion.md) | Cada sección elige molde y usa solo tokens ([maquetación](maquetacion.md)) |
| [08 · Revisión](../fases/08-revision.md) | Contraste AA anotado, styleguide y laboratorios fuera de Google |

## 2. Qué tenemos (escenarios)

| Situación | De dónde salen los tokens | Ejemplo |
| --- | --- | --- |
| 🟢 Manual de identidad completo | Del manual, **literal**, en la capa de marca; lo que falte para pantalla, como extensión | clays-of-the-world: 6 colores del manual en `tokens.css` §1 y `brand/README.md` con las decisiones de web |
| 🟢 El cliente trae Figma con design system | **De ese Figma**: variables con `get_variable_defs` y valores de nodo con `get_design_context`, no del PDF ni a ojo | festival-viento: `global.css` «Paleta: Design System Figma» |
| 🟡 Manual a medias (colores sí, tipografía o usos no) | Lo que hay, literal; lo que falta se propone y lo aprueba el cliente | — |
| 🔴 Sin marca | Brand board nuestro ([brand board](brand-board.md)) y de ahí los tokens | APC-gestion-patrimonial: `tokens.css` «colores crudos del brand board» |

En todos los casos **las referencias visuales van antes** que los tokens: las
elige Belén (el cliente solo si las pone en el briefing) →
[referencias visuales](referencias-visuales.md).

## 3. Pasos y quién

1. **Belén: referencias visuales** → [referencias visuales](referencias-visuales.md).
   Claude las ordena y anota qué se toma de cada una.
2. **Claude: la marca traducida a web**, en `brand/README.md`: versiones del
   logo y dónde va cada una, paleta con su uso según el manual, tabla de
   contrastes, tipografía por rol. Es lo que se lee antes de tocar nada visual
   (clays-of-the-world, `brand/README.md`).
3. **Claude: tokens** en `src/styles/tokens.css` (§4).
4. **Claude: fuentes alojadas** (§5).
5. **Claude: reset y base**: `reset.css` (sin opiniones de marca) y `base.css`
   (tipografía y primitivas de layout con tokens semánticos) (clays).
6. **Claude: componentes base** (§6).
7. **Claude: moldes de sección** a medida que se montan las primeras
   secciones (§7).
8. **Claude: `/styleguide`** con todo lo anterior (§9).
9. **Belén revisa el styleguide** y decide. Si el proyecto va a Figma, el
   siguiente paso es [generar la librería en Figma](diseno-figma.md).

## 4. Tokens

**`tokens.css` es la única fuente de verdad.** Ningún componente escribe un
color, un tamaño de fuente, un espaciado o una duración a pelo; si hace falta
un valor nuevo, se añade primero como token (clays `CLAUDE.md` § Design
system; APC `docs/GUIA-DE-ESTILO.md` §1; xanasystem-web `docs/design.md` §1).

### 4.1 Colores en capas

Hay dos formas en uso:

**Tres bloques** (clays-of-the-world):

```css
/* src/styles/tokens.css */
/* 1. PALETA CORPORATIVA (literal del manual — no modificar) */
--cw-grafito-oscuro: #494647;
--cw-arena-natural:  #BA9471;

/* 2. EXTENSIONES DE PALETA (derivadas — necesarias para web) */
--cw-arena-oscura: #7A5537; /* Enlaces y texto de acento SOBRE CLARO — 5.5:1 sobre blanco arcilla.
                               Inservible sobre oscuro (2.1:1): usar arena natural */

/* 3. TOKENS SEMÁNTICOS (usar SIEMPRE estos en componentes) */
--color-text:        var(--cw-grafito-oscuro); /* 9.3:1 sobre blanco — AAA */
--color-text-accent: var(--cw-arena-oscura);   /* 6.6:1 sobre blanco — AA  */
```

- El bloque 1 **no se modifica ni se amplía**: es la marca.
- El bloque 2 existe porque la paleta es de impresión y no llega a AA en
  texto. Cada tono nuevo va aquí, **marcado como extensión y con su ratio**.
- Los componentes solo usan el bloque 3 (`--color-*`).

**Dos capas** (xanasystem-web y APC):

```css
/* xanasystem-web/src/styles/tokens.css */
/* Capa 1 (--brand-*): colores crudos del manual, fijos en ambos modos.
   Capa 2 (--c-*):    roles semánticos, se redefinen en dark.
   Los componentes consumen SOLO capa 2 (y --grad-* para acentos de marca). */
--brand-navy: #0b3c70;
--c-cta: #09305a; /* navy CTA — más profundo que --brand-navy, para el botón primario */
```

Aquí los tonos derivados (`--c-cta`, `--c-accent-deep` en APC) se escriben
directamente en la capa 2, con el hex a pelo y sin ratio anotado.

**Recomendación: tres bloques siempre**, aunque el bloque 2 quede vacío.
Separa lo que es de la marca (y no se toca) de lo que hemos inventado para
pantalla, y obliga a anotar el porqué y el contraste de cada tono inventado.
Con dos capas, un derivado se confunde con un rol y nadie sabe si se puede
cambiar. El nombre del prefijo da igual (`--cw-*`, `--brand-*`); lo que cuenta
es la separación. Si la web tiene modo oscuro, se redefine solo el bloque 3,
como hace xanasystem-web con la capa 2.

Reglas del color:

- **Contraste AA mínimo, anotado en el comentario** del token semántico y en
  la tabla del styleguide (clays `CLAUDE.md`). Si una combinación **no**
  cumple a sabiendas, se escribe que no cumple, por qué y cuál sería el valor
  que cumple (clays `--color-border-field`, `--color-text-dormant`).
- Texto sobre fotografía: el ratio depende de la foto y del velo, se mide
  sobre los píxeles reales y se anota con la foto medida (clays
  `--scrim-light`, `--color-text-accent-media`).
- Canales sueltos para lo que necesita alfa (`--cw-grafito-900-rgb: 46, 44, 45`)
  en vez de escribir `rgba(46, 44, 45, …)` en cada componente (clays). En
  APC, `color-mix(in srgb, var(--token) N%, transparent)` en lugar de un hex
  nuevo (`GUIA-DE-ESTILO.md` §2.1).

### 4.2 Espaciado sin huecos

```css
/* clays-of-the-world/src/styles/tokens.css */
--space-1: 0.25rem;  --space-2: 0.5rem;  --space-3: 0.75rem; --space-4: 1rem;
--space-5: 1.5rem;   --space-6: 2rem;    --space-7: 3rem;    --space-8: 4rem;
--space-9: 6rem;     --space-10: 8rem;
--section-y: clamp(var(--space-8), 8vw, var(--space-10));
```

**Numeración correlativa**: el número es el escalón, no los píxeles. En
xanasystem-web la escala va por píxeles (`--sp-4`, `--sp-6`, `--sp-8`,
`--sp-12`…) y `--sp-5` o `--sp-10` no existen: usarlos anula el `gap` o el
`padding` sin error → [trampa «Todo sale muy junto»](../trampas-y-lecciones.md#todo-sale-muy-junto-columnas-pegadas-cabeceras-sin-padding).
APC tiene el mismo riesgo (`--sp-7`, `--sp-9` y `--sp-11` no existen).

Junto a la escala: ritmo vertical de sección fluido (`--section-y`), margen
lateral (`--gutter`), ancho máximo (`--container-max`) y medida de lectura
(`--measure: 68ch`) (clays).

### 4.3 Tipografía fluida con `clamp`

```css
/* clays-of-the-world/src/styles/tokens.css — ratio 1.25, fluidos entre 360 y 1280px */
--text-base: 1rem;                                          /* 16px */
--text-xl:   clamp(1.25rem, 0.5vw + 1.1rem, 1.5625rem);     /* 20 → 25 */
--text-3xl:  clamp(1.75rem, 2.0vw + 1.3rem, 2.441rem);      /* 28 → 39 */
--text-4xl:  clamp(2.0rem,  3.0vw + 1.4rem, 3.052rem);      /* 32 → 49 */
```

- Los cuerpos pequeños, fijos; los titulares, fluidos. **Comentario con los
  píxeles de los dos extremos** en cada token.
- Pesos, interlineados y tracking también como tokens (`--font-weight-*`,
  `--leading-*`, `--tracking-eyebrow`). Xanasystem-web los centraliza por rol
  (`--fw-h1: 500`, `--fw-heading: 600`, `--fw-lede: 350`).
- Si el cliente pide un tamaño fijo, se respeta: xanasystem-web pasó los ledes
  a 18px fijos por petición expresa (`docs/design.md` § Cabeceras de sección).
- Mona Sans con `tabular-nums` → [trampa del cero barrado](../trampas-y-lecciones.md#los-ceros-salen-barrados-mona-sans).

### 4.4 Radios, sombras, capas y movimiento

- Radios y sombras como escala (`--radius-sm…full`, `--shadow-sm…lg`), con
  el color de la sombra sacado de la marca (clays usa el grafito; APC,
  sombras «cálidas»).
- Curvas con su uso: `--ease-out` para controles, `--ease-out-soft` para lo
  que aparece al hacer scroll (clays).
- **Las duraciones valen `0ms` con `prefers-reduced-motion`**, y con ellas el
  recorrido y el escalonado. Así ninguna transición escrita con tokens
  necesita repetir la media query:

  ```css
  /* clays-of-the-world/src/styles/tokens.css */
  @media (prefers-reduced-motion: reduce) {
    :root {
      --duration-fast: 0ms;
      --duration-base: 0ms;
      --duration-slow: 0ms;
      --duration-slower: 0ms;
      --reveal-travel:  0px;
      --reveal-stagger: 0ms;
    }
  }
  ```

  xanasystem-web y festival-viento lo resuelven con un
  `*, *::before, *::after { transition-duration: 0.01ms !important }` global.
  **Se prefiere el de los tokens**: el global también apaga lo que solo cambia
  de color, y un fundido de color es precisamente el recambio del movimiento
  (clays `CLAUDE.md` § Movimiento; `sections/CompanyIntro.astro`).
- Un bucle infinito a `0ms` no degrada limpio: además de la duración, el
  componente se envuelve en la media query (clays `--duration-pulse`).

## 5. Fuentes alojadas en la web

**Nunca `@import` ni `<link>` a Google Fonts**: cada visita manda la IP del
visitante a Google sin consentimiento y retrasa el primer pintado.

- **Por defecto, fontsource** importado en el layout base:

  ```astro
  ---
  // clays-of-the-world/src/layouts/Base.astro
  import '@fontsource-variable/montserrat';
  import '../styles/tokens.css';
  ---
  ```

  Variable si existe (`@fontsource-variable/*`, un fichero para todos los
  pesos); si no, solo los pesos y estilos que se usan (APC,
  `BaseLayout.astro`: `@fontsource/spectral/400.css`, `500.css`, `600.css`,
  `400-italic.css`, `500-italic.css`).
- **Si la fuente no está en fontsource**: `.woff2` en `public/fonts/`,
  `@font-face` con `font-display: swap` y `<link rel="preload">` (xanasystem-web,
  `BaseLayout.astro`, Inter y Mona Sans).
- La pila de respaldo en el token (`"Montserrat Variable", "Montserrat",
  ui-sans-serif, system-ui, …`).
- festival-viento carga Outfit e Inter con `@import` de Google Fonts
  (`src/styles/global.css`, línea 7): **deuda**, no ejemplo.

## 6. Componentes base

Los cinco que lleva toda web. Referencia: `clays-of-the-world/src/components/`.

| Componente | Qué hace | Props |
| --- | --- | --- |
| `Button.astro` | CTA. `<a>` si lleva `href`, `<button>` si no | `href`, `variant` (`primary` · `secondary` · `ghost`), `size` (`md` · `lg`), `arrow`, `class` |
| `Logo.astro` | Logotipo con las reglas del manual | `variant` (`horizontal` · `vertical` · `sin-icono`), `width`, `inverse`, `plate` |
| `Section.astro` | Envoltorio de sección: ritmo vertical, fondo, ancla | `id`, `variant` (`default` · `subtle` · `inverse`), `container` (`default` · `narrow` · `none`), `label`, `flush` |
| `SectionHeader.astro` | Antetítulo + titular | `eyebrow`, `title`, `as` (`h1` · `h2` · `h3`), `align` |
| `Pending.astro` | Marca visible de dato pendiente `[[ ]]` | `what`, `label` |

Lo que no se ve en las props y hay que copiar:

- **Button**: el contraste decide las variantes (el arena de Clays no llega a
  AA como fondo de botón, así que el primario es grafito); variante para
  superficie oscura (`:global(.section--inverse) .btn--primary` y
  `.btn--on-dark`) escrita **en el botón**, no repetida en cada sección.
  Xanasystem-web añade `outline`, `tertiary`, `sm`, `fullWidth` y `glass`
  (`src/components/ui/Button.astro`); APC, `ghost-dark` para fondo oscuro.
  Regla de APC que vale para todas: **todo CTA usa `Button`**, no un `<a>`
  estilado a mano (`GUIA-DE-ESTILO.md` §4.1).
- **Logo**: avisa en consola de build si el ancho baja del mínimo del manual
  y si se pide una versión en blanco que no existe:

  ```astro
  ---
  // clays-of-the-world/src/components/Logo.astro
  if (width < logo.min) {
    console.warn(`[Logo] ancho ${width}px por debajo del mínimo del manual (${logo.min}px) para la versión "${variant}".`);
  }
  ---
  ```
- **Section**: pone `data-surface="dark"` en las inversas para que la
  cabecera sepa qué tiene debajo sin una lista de ids en el script.
- **SectionHeader**: el titular con `max-width` en `ch` (20ch), para que
  aguante el texto traducido.
- **Pending**: el aviso va en `title` y en texto oculto para lector de
  pantalla. **Nada con `Pending` se publica** ([contenidos y SEO](contenidos-seo.md)).

## 7. Moldes de sección

**Las secciones no se componen una a una: se elige molde.** Cada molde
resuelve una forma de contar, no una sección concreta; inventar una
composición nueva es la excepción (clays `CLAUDE.md`). Cada molde recibe su
contenido como datos (`content`), nunca texto incrustado.

Moldes construidos en clays-of-the-world (`src/pages/styleguide.astro` §08):

| Molde | Forma | Cuándo |
| --- | --- | --- |
| `CompanyIntro` | Raíl estrecho con el rótulo y declaración grande | Un bloque de texto que es el argumento entero, sin imagen |
| `Band` | Banda a sangre: panel oscuro con bloques cortos y foto (o varias que se turnan) | La sección **enumera** tres o cuatro razones |
| `Showcase` | Cabecera a dos columnas, pieza de vídeo o imagen grande y columnas de apoyo | El peso lo lleva una pieza audiovisual |
| `Editorial` | Titular y foto a un lado, texto a dos columnas al otro | Hay que **explicar en prosa**. Es lo más caro en alto: preguntarse antes si hace falta |
| `Note` | Columna de texto corta y dos fotos desiguales apoyadas por la base | Algo que importa, pero menos que sus vecinas |
| `Figures` | Banda oscura con foto de fondo y lista numerada de datos | Enumerar datos sobre imagen. Obliga a medir contraste |
| `Gallery` | Carrusel con pie encima de cada foto | Imágenes que se recorren, no que se comparan |
| `Products` | Cabecera y fila de tarjetas del mismo alto | Catálogo corto donde cada entrada se reconoce por su imagen |
| `Event` | Banda corta con titular, texto y datos en una línea | Pieza de una sola vez que caduca |

Recetas equivalentes en APC (`docs/GUIA-DE-ESTILO.md` §5): índice de filas
numeradas (servicios, proceso), rejilla con líneas en vez de tarjetas con
sombra (contenidos), banda de cierre oscura con imagen y CTA.

- **El formulario no es un molde** (no cuenta nada): va aparte
  (clays `sections/Contact.astro`).
- Molde nuevo → a la tabla del styleguide con forma, cuándo y dónde se usa.
- Si un molde queda sin uso se dice en la tabla, no se borra sin preguntar
  (clays, `Editorial`: «Sin uso»).

## 8. Reglas visuales recurrentes

Se aplican sin preguntar salvo que el proyecto diga otra cosa en su `CLAUDE.md`.

| Regla | Por qué | Fuente |
| --- | --- | --- |
| **No dos secciones oscuras seguidas.** El pie cuenta como oscura | Se leen como una sola y el oscuro deja de marcar el cambio | clays `CLAUDE.md` § Design system; styleguide §08 «Ritmo de superficies» |
| **Un solo fondo alternativo, y en pocas secciones** | Si lo llevan tres, deja de separar; un tercer tono cálido enturbia | clays `--color-bg-alt` |
| **Si se alternan fondos claros, nunca dos seguidos iguales** | Separar secciones sin líneas | xanasystem-web memoria `alternancia-fondos-seccion`; APC `GUIA-DE-ESTILO.md` §3.4 |
| **Antetítulo + titular** como apertura de sección (`SectionHeader`). El antetítulo puede omitirse si no aporta | Patrón del manual (clays); convención de la home (xanasystem) | clays `CLAUDE.md` § Marca; xanasystem-web memoria `eyebrows-estilo-home` |
| **Una sola palabra por antetítulo cuando es la etiqueta del menú** | Quien llega desde el menú ve dónde ha caído; las tríadas se parten en cuatro renglones | clays `CLAUDE.md` § Estructura |
| **Titulares con `text-wrap: balance`; párrafos con `pretty`** | Ninguna palabra sola en la última línea. `balance` en texto largo acorta todas las líneas y deja hueco; pasadas unas 6 líneas el navegador lo ignora | clays `base.css`; APC `global.css`; xanasystem-web `docs/design.md` § Equilibrio de líneas |
| **Contraste AA anotado en el token y en el styleguide** | Que la próxima combinación no se elija a ojo | clays `CLAUDE.md` |
| **El logo nunca directamente sobre foto ni sobre oscuro**: versión en blanco sobre oscuro plano, pastilla (`plate`) sobre foto, o cabecera sólida | Regla de manual habitual | clays `brand/README.md` §3 y `Logo.astro` |
| **Un solo peso por titular** | Partir una frase en dos pesos llama la atención sobre la tipografía | clays `base.css` |
| **Color de acento en la acción, no como decoración** | Si todo es acento, nada lo es | APC `GUIA-DE-ESTILO.md` §2.1 (terracota = acción) |

Xanasystem-web aplica `balance` también a ledes y descripciones cortas por
preferencia de Belén (memoria `texto-lineas-equilibradas`), con `pretty` como
excepción para las frases largas. Si el proyecto no dice nada, vale la regla
de la tabla.

## 9. Styleguide y páginas de laboratorio

### `/styleguide`

Página interna con todo el sistema, en español, **publicada pero fuera de
Google**:

```astro
---
// clays-of-the-world/src/pages/styleguide.astro
---
<Base
  title="Design System · Clays of the World"
  description="Referencia interna del sistema de diseño."
  lang="es"
  chrome={false}
  robots="noindex, nofollow"
>
```

- `robots="noindex, nofollow"`, **sin `Disallow` en `robots.txt`** (taparía
  el `noindex`) y fuera del sitemap → [SEO técnico](seo-tecnico.md).
- `chrome={false}`: sin cabecera, pie ni muro de cookies (no mide visitas).
- `lang="es"` aunque la web esté en otro idioma.
- Secciones de Clays: 01 logotipo · paleta · tipografía · componentes base ·
  superficies · texto sobre fotografía · layout · 08 moldes de sección (con
  ritmo de superficies y tabla de filetes) · superficie inversa.

### Páginas de laboratorio (`pruebas-*`, `demo-*`)

Sirven para comparar opciones con Belén. Todo lo que está en `src/pages/` se
compila y se publica
([trampa](../trampas-y-lecciones.md#las-páginas-de-prueba-se-publican-y-engordan-el-despliegue)).

1. **Mientras se decide**: la página con `noindex, nofollow` y sus componentes
   en una carpeta propia (`src/components/pruebas/`, `src/data/pruebas/` en
   clays).
2. **Cuando se elige** la opción: se lleva a la sección real y **se borra** la
   página con sus componentes (el historial la conserva), como `demo-arcos.astro`
   en APC.
3. **Si hay que conservarla en el repositorio sin publicarla**: prefijo `_` en
   el nombre (`src/pages/_pruebas-cabecera.astro`). Según la documentación de
   Astro (*Routing › Excluding pages*), los ficheros con `_` delante no los
   reconoce el enrutador y no llegan a `dist/`. Tampoco se pueden abrir en
   `dev`: para volver a verla, se quita el prefijo.
4. **Antes de publicar**: `src/pages/` no tiene ninguna `pruebas-*` ni `demo-*`
   sin prefijo ([revisión](revision.md)).

## 10. Dónde queda cada cosa

| Ruta | Qué hay |
| --- | --- |
| `docs/05-referencias-visuales/` | Referencias y `referencias.md` ([referencias visuales](referencias-visuales.md)) |
| `brand/README.md` | La marca traducida a decisiones de web; `brand/logo/` con los originales |
| `src/styles/tokens.css` | Tokens: marca · extensiones · semánticos · tipografía · espaciado · layout · radios, sombras y movimiento |
| `src/styles/reset.css` · `base.css` | Reset sin marca · tipografía base, `.container`, `.section`, `.eyebrow`, `.reveal`, `.skip-link` |
| `src/components/` | `Button`, `Logo`, `Section`, `SectionHeader`, `Pending` |
| `src/components/sections/` | Moldes |
| `src/components/pruebas/` | Componentes de laboratorio (se borran al decidir) |
| `src/pages/styleguide.astro` | `/styleguide`, con `noindex` |
| `CLAUDE.md` del proyecto | Reglas del design system que no se saltan y enlace a Figma |

## 11. Reglas y trampas

- Un token que no existe anula la declaración sin avisar →
  [muy junto](../trampas-y-lecciones.md#todo-sale-muy-junto-columnas-pegadas-cabeceras-sin-padding).
- `scrollbar-gutter: stable both-edges` impide bandas a sangre →
  [trampa](../trampas-y-lecciones.md#las-bandas-a-sangre-no-llegan-al-borde-de-la-ventana).
- Estilos de componente que no llegan al `<slot>` →
  [trampa](../trampas-y-lecciones.md#los-estilos-del-componente-no-se-aplican-al-contenido-del-slot).
- Colores copiados de Figma que no cuadran →
  [trampa](../trampas-y-lecciones.md#los-colores-y-degradados-copiados-de-figma-no-cuadran).
- Un color transitorio que no cumple AA (texto «apagado» antes de encenderse)
  solo vale si el estado legible es el que se pinta por defecto (clays
  `--color-text-dormant`) → [maquetación](maquetacion.md).
- Texto decorativo sin contraste (el «404» a cuerpo de cartel) solo con
  `aria-hidden` y el mensaje dicho en otro sitio (clays `--color-text-decorative`).
- Cambiar un token semántico cambia toda la web: si el cambio es para una
  sección, se crea el token de esa sección o se aísla
  ([diseño en Figma](diseno-figma.md)).

## 12. Checklist

**Tokens y fuentes**
- [ ] Referencias visuales de Belén ordenadas y anotadas
- [ ] `brand/README.md` con logo, paleta, contrastes y tipografía por rol
- [ ] Paleta de marca literal y separada; extensiones con porqué y ratio
- [ ] Componentes solo con tokens semánticos; ningún hex, `px` de fuente ni espaciado a pelo
- [ ] Escala de espaciado correlativa, sin huecos
- [ ] Titulares con `clamp` y los píxeles de los extremos comentados
- [ ] Duraciones, recorrido y escalonado a `0` con `prefers-reduced-motion`
- [ ] Fuentes con fontsource (o `woff2` propio con `preload`); ningún Google Fonts

**Componentes y moldes**
- [ ] `Button`, `Logo`, `Section`, `SectionHeader`, `Pending`
- [ ] Botón con variante sobre oscuro; logo con aviso de ancho mínimo
- [ ] Moldes documentados en el styleguide con forma y cuándo
- [ ] Ritmo de superficies revisado (pie incluido); un solo fondo alternativo

**Styleguide y laboratorios**
- [ ] `/styleguide` con `noindex, nofollow`, sin cabecera ni muro, fuera del sitemap
- [ ] Tabla de contrastes en el styleguide
- [ ] Laboratorios con `noindex` mientras se decide; borrados o con prefijo `_` antes de publicar
