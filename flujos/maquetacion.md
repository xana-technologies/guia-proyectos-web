# Flujo · Maquetación

Montar las páginas con los moldes y el contenido como datos: móvil, que se lea
sin JavaScript, accesible y con el movimiento justo. **Va en todas las webs.**
La implementación de referencia es **clays-of-the-world**
(`src/layouts/Base.astro`, `src/styles/base.css`, `src/components/`).

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [05 · Design system](../fases/05-design-system.md) | Tiene que estar hecho: tokens, componentes base, primeros moldes ([design system](design-system.md)) |
| [06 · Diseño en Figma](../fases/06-diseno-figma.md) | En el caso A, antes; en el B, va y viene ([diseño en Figma](diseno-figma.md)) |
| [07 · Construcción](../fases/07-construccion.md) | Paso 2 del orden de montaje, sobre la URL temporal |
| [08 · Revisión](../fases/08-revision.md) | Sin JavaScript, teclado, contraste, móvil, textos traducidos |

Animaciones más allá de la aparición al scroll (GSAP, Lenis, parallax
escribiendo variables, loaders) → módulo
[movimiento avanzado](../modulos/movimiento-avanzado.md). Varias páginas con
navegación → [multipágina](../modulos/multipagina.md). Varios idiomas →
[idiomas](../modulos/idiomas.md).

## 2. Qué tenemos (escenarios)

| Situación | Qué se hace |
| --- | --- |
| Estructura y textos aprobados | Se monta con los textos definitivos en los datos |
| Faltan datos | Hueco con `Pending` (`[[ ]]`); si faltan **todas** las cifras de un bloque, el bloque sale en vez de llenarse de marcas (clays `CLAUDE.md` § Estructura) |
| Diseño de Figma (caso A) o nodo de ajuste (B) | Valores exactos del nodo → [diseño en Figma](diseno-figma.md) |
| Sin diseño (B, C) | Molde del design system + referencias visuales |
| Diseño solo de escritorio | Se propone la adaptación móvil (§5) |
| Web en un idioma que se traducirá | Contenido como datos desde el primer componente (§4) |

## 3. Pasos y quién

1. **Claude**: layout base (`Base.astro`): `<head>` con metadatos, fuentes,
   marca `data-reveal`, skip-link, cabecera, `<slot />`, pie, muro de cookies
   y el script de aparición (§6, §8).
2. **Claude**: cabecera y pie (§9).
3. **Claude**: sección a sección, de arriba abajo: elige molde, escribe los
   datos, monta, revisa en móvil y sin JavaScript.
4. **Belén** revisa cada sección en la URL temporal; feedback puntual →
   cambio mínimo ([forma de trabajo](../forma-de-trabajo.md#2-cómo-se-itera)).
5. **Claude**: 404 y página de gracias (§10).
6. **Claude**: verificación (§12) antes de decir que está.
7. Commit por **pieza terminada**, con permiso.

## 4. Secciones: moldes y contenido como datos

- **Cada sección es un molde que recibe datos**; la página solo ordena
  secciones. No se incrusta texto en el marcado:

  ```astro
  ---
  // clays-of-the-world/src/components/sections/Band.astro
  interface Props {
    content: BandContent;
  }
  const { content } = Astro.props;
  ---
  <Section id={content.id} variant="inverse" container="none" flush>
  ```

  Los textos viven en `src/data/<idioma>.ts` con sus tipos en
  `src/data/types.ts` (clays). «Activar `i18n` en Astro después es barato;
  sacar cadenas de dentro de quince ficheros `.astro` no lo es» (clays
  `CLAUDE.md` § Idioma).
- APC declara los datos repetibles como arrays en el frontmatter de cada
  sección y los datos globales en `src/config/site.ts`
  (`GUIA-DE-ESTILO.md` §6). Vale para una web en un solo idioma que no se
  traducirá; **por defecto, el patrón de clays**.
- Hasta los rótulos de los scripts son contenido: viajan en el marcado
  (`data-label-open`, `data-label-close`), no escritos en el JavaScript
  (clays `Header.astro`).
- Datos globales (nombre, contacto, redes, enlaces legales) en un solo sitio;
  el pie y la cabecera los reciben, no los repiten.

### Clases BEM

`.bloque`, `.bloque__elemento`, `.bloque--variante` y estados `.is-*`
(`.band__media`, `.band--media-left`, `.is-current` en clays; «BEM-lite» en
APC, `GUIA-DE-ESTILO.md` §6). Estilos en el `<style>` scoped del componente;
lo compartido, en `base.css`. Lo que llega por `<slot>` necesita `:global()`
→ [trampa](../trampas-y-lecciones.md#los-estilos-del-componente-no-se-aplican-al-contenido-del-slot).

## 5. Móvil

- **Mobile-first**: la versión de una columna primero y la rejilla dentro de
  `@media (min-width: …)` (APC `GUIA-DE-ESTILO.md` §3.5; xanasystem-web
  `docs/design.md` § Responsive).
- **Al apilar, la imagen va primero en el marcado** si es lo que ancla la
  sección; en escritorio la coloca la rejilla (clays `sections/Band.astro`).
  Ojo con el orden de foco si la columna de la imagen tiene algo interactivo.
- **Los textos crecen al traducir entre un 15 % y un 30 %** (alemán, francés):
  - nada de anchos fijos calculados sobre el texto; titulares y párrafos con
    `max-width` en `ch` (clays `SectionHeader.astro`: `20ch`);
  - botones y etiquetas que aguanten que el texto crezca;
  - el punto de corte del menú puede depender del idioma: en clays pasa a
    escritorio a 1140px en inglés y a 1480px en español (`Header.astro`).
- Hijos de un grid apilados en la misma celda necesitan `min-width: 0` →
  [trampa](../trampas-y-lecciones.md#scroll-horizontal-en-móvil-por-un-bloque-que-sobresale).
- Probar a 360, 768, 1024 y 1440px (xanasystem-web `docs/design.md`).

## 6. Mejora progresiva: sin JavaScript no queda nada oculto

**El estado legible es el que se pinta por defecto**; el efecto solo lo
altera si puede completarse. Vale para la aparición al scroll, los textos que
se encienden, los carruseles y el vídeo (clays `CLAUDE.md` § Movimiento).

El documento se marca **antes del primer pintado**, en el `<head>`, y el CSS
solo oculta si la marca está:

```astro
<!-- clays-of-the-world/src/layouts/Base.astro -->
<script is:inline>
  document.documentElement.dataset.reveal = '';
</script>
```

```css
/* clays-of-the-world/src/styles/base.css */
html[data-reveal] .reveal,
html[data-reveal] [data-stagger] > * {
  opacity: 0;
  transform: translateY(var(--reveal-travel));
  transition:
    opacity var(--duration-slower) var(--ease-out-soft),
    transform var(--duration-slower) var(--ease-out-soft);
}
```

- APC hace lo mismo con una clase (`.js-reveal` en `<html>`,
  `BaseLayout.astro`). xanasystem-web oculta siempre y lo deshace con un
  `<noscript><style>…!important</style></noscript>`: funciona, pero el
  contenido depende de que ese bloque esté; **se prefiere la marca en
  `<html>`**.
- Lo mismo con cualquier estado que solo tiene sentido con JavaScript: el
  carrusel de fotos de `Band` lleva `data-paused` en el marcado y es el
  script el que lo quita (clays).
- Un texto «apagado» que se enciende al hacer scroll: el color final va por
  defecto, y el apagado solo si hay JavaScript y `color-mix` (clays
  `sections/CompanyIntro.astro`).

## 7. Aparición al scroll

### Con `IntersectionObserver` (lo normal)

Una sola vez por elemento, con escalonado en grupos:

```ts
// clays-of-the-world/src/layouts/Base.astro
const targets = document.querySelectorAll('.reveal, [data-stagger]');

if (targets.length > 0 && !matchMedia('(prefers-reduced-motion: reduce)').matches) {
  const observer = new IntersectionObserver(
    (entries) => {
      for (const entry of entries) {
        if (!entry.isIntersecting) continue;
        entry.target.classList.add('is-visible');
        observer.unobserve(entry.target);
      }
    },
    { rootMargin: '0px 0px -12% 0px' }
  );
  for (const target of targets) observer.observe(target);
} else {
  for (const target of targets) target.classList.add('is-visible');
}
```

- **`unobserve` al aparecer**: no se repite al subir y bajar.
- **Escalonado vigilando el contenedor**, no cada hijo: si se vigila cada
  hijo, el retardo se suma a la diferencia de altura y los últimos llegan
  tarde dos veces. En clays el retardo lo pone el CSS (`[data-stagger] >
  *:nth-child(n)` con `--reveal-stagger`, hasta el sexto); en APC lo reparte
  el script (`data-reveal-stagger="70"`). En una lista más alta que la
  pantalla el escalonado sobra.
- **Gestos distintos según lo que entra** (clays `base.css`): `.reveal` sube
  y aparece (bloques con forma); `.reveal--fade` solo funde (prosa larga);
  `.reveal--rise` sale de detrás de su borde (titulares cortos, pide un
  `<span>` dentro); `.reveal--veil` levanta un velo del color del fondo
  (fotografía).
- **Sobrio.** Fundido y una subida corta (28px en clays, 18px en APC, 10px
  en xanasystem-web). Belén descartó los titulares troceados línea a línea en
  APC y palabra a palabra en xanasystem-web (memorias `apc-movimiento-web` y
  `reveal-blur-in`): no se proponen.
- Con `prefers-reduced-motion` todo aparece sin animar (las duraciones valen
  0, [design system §4.4](design-system.md#44-radios-sombras-capas-y-movimiento)).

### CSS scroll-driven (sin JavaScript)

Para efectos ligados al recorrido del scroll, no a un disparo. Siempre dentro
de `@supports` y de `prefers-reduced-motion: no-preference`, de forma que
**sin soporte (Firefox) el elemento queda en su estado final**:

```css
/* festival-viento/src/styles/global.css */
@media (prefers-reduced-motion: no-preference) {
  @supports (animation-timeline: view()) {
    .revelar {
      animation: revelar-fade linear both;
      animation-timeline: view();
      animation-range: entry 0% entry 55%;
    }
  }
}
```

Usos reales: aparición de tarjetas (festival-viento), paralaje de fotos a
sangre con `[data-parallax]` (clays `base.css`), texto que se enciende por
palabras (xanasystem-web `SolutionBlock`), banda de cierre que se estrecha y
redondea (APC `Cierre`, `animation-timeline: scroll()`).

- Un ancestro con `overflow: hidden` rompe `view()`: usar `overflow: clip` →
  [trampa](../trampas-y-lecciones.md#un-reveal-ligado-al-scroll-no-anima).
- Paralaje con JavaScript, GSAP, Lenis → [movimiento avanzado](../modulos/movimiento-avanzado.md).

## 8. Accesibilidad

- **Skip-link** como primer elemento del `<body>`, a `#main`, visible al
  recibir foco (clays `base.css` `.skip-link`; texto traducido desde el
  layout).
- **`:focus-visible` con anillo propio** en el reset:

  ```css
  /* clays-of-the-world/src/styles/reset.css */
  :focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 3px;
    border-radius: var(--radius-sm);
  }
  ```

  **Nunca `outline: none` sin sustituto.** La única excepción documentada es
  el contenedor con `tabindex="-1"` del muro de cookies, que no es un control
  → [trampa](../trampas-y-lecciones.md#al-abrir-el-muro-aparece-un-borde-de-color-alrededor-de-la-caja).
- **Una sola `<h1>` por página**; las secciones abren en `h2`
  (`SectionHeader` con `as`, clays; APC `GUIA-DE-ESTILO.md` §7). Si el
  titular grande de una sección es una declaración, esa declaración **es** el
  `h2`, sin otro titular escondido (clays `CompanyIntro.astro`).
- **Sección sin encabezado visible** → `aria-label` (`Section` con `label`).
- **FAQ con `<details>`/`<summary>` nativos** y JSON-LD `FAQPage` con las
  mismas preguntas (xanasystem-web `sections/FAQAccordion.astro`; APC
  `gestion-patrimonial/Faq`). Funciona sin JavaScript y el contenido está en
  el HTML. `name` común en los `<details>` si solo debe abrirse uno.
- **`<dialog>` con enlaces reales**: un `<dialog>` cerrado es `display: none`
  para buscadores y sin JavaScript. Lo que abre el visor son **enlaces** a una
  página de verdad con el mismo contenido; el script les quita el salto y abre
  `showModal()` (clays `Lightbox.astro` y `/gallery`) →
  [trampa](../trampas-y-lecciones.md#lo-que-está-dentro-de-un-dialog-no-existe-para-buscadores-ni-sin-javascript).
- **Menú móvil**: botón con `aria-expanded` y `aria-controls`, rótulo que
  cambia entre abrir y cerrar, **Escape cierra y devuelve el foco al botón**,
  y se cierra al seguir un enlace:

  ```ts
  // clays-of-the-world/src/components/Header.astro
  document.addEventListener('keydown', (event) => {
    if (event.key === 'Escape' && toggle.getAttribute('aria-expanded') === 'true') {
      setOpen(false);
      toggle.focus();
    }
  });
  ```

  APC oculta el panel con `hidden` y cambia el `aria-label`
  (`nav/SiteHeader.astro`). festival-viento lo resuelve sin JavaScript con
  `<details>` (`SiteHeader.astro`): válido para una landing de anclas.
- **Iconos**: SVG con `aria-hidden="true"` y el nombre en texto oculto o
  `aria-label` en el enlace (clays `Header.astro`, redes).
- **`alt` descriptivo** en imágenes de contenido; `alt=""` en decorativas
  ([imágenes](imagenes.md)).
- **Contraste AA** con los tokens anotados; lo que va sobre foto se mide
  sobre la foto ([design system §4.1](design-system.md#41-colores-en-capas)).
- `lang` correcto en `<html>`, también en páginas internas (clays `Base.astro`).

## 9. Cabecera y pie

### Cabecera

- **Fija arriba** con `scroll-padding-top` en el `html` para que las anclas no
  queden debajo (clays `reset.css`: `calc(var(--header-h) + var(--space-4))`).
- **`sticky`** si no cambia de alto (APC: translúcida con `backdrop-filter`;
  xanasystem-web: píldora de cristal que aparece al hacer scroll).
- **Si se condensa al bajar, `fixed` con un separador** del alto original: una
  `sticky` que encoge mueve todo el contenido mientras se lee (clays
  `Header.astro`, `.header__spacer`).
- Encima de un hero con foto, la cabecera no deja el logo sobre la imagen
  ([design system §8](design-system.md#8-reglas-visuales-recurrentes)): barra
  sólida (clays). Una cabecera transparente sobre el hero que se vuelve
  sólida al salir de él (festival-viento, `is-scrolled`) solo vale si lo que
  lleva encima no es el logotipo.
- Orden a la derecha: lo que se hace (CTA) antes de lo que se consulta
  (redes, idioma) (clays `Header.astro`).

### Pie

Lleva siempre, en la barra inferior, **los 4 elementos legales**: aviso
legal, política de privacidad, política de cookies y **«Preferencias de
privacidad»**, que es un enlace con `data-cc` (con JavaScript abre el panel;
sin él lleva a la política) → [legales](legales.md) y
[cookies y analítica](cookies-analitica.md). Los enlaces salen de los datos
legales, no escritos en el pie (clays `src/data/legal/index.ts`,
`legalLinks()`; APC `config/site.ts`, `legalNav`). Además: copyright, datos
de contacto y navegación.

Si el pie es oscuro (clays, APC), **cuenta** en el ritmo de superficies. Cortina de cierre (el pie se destapa): `main` con fondo opaco y
`z-index`, pie `sticky` al fondo, y en clays solo si el pie cabe entero en la
ventana (`html[data-footer-reveal]`, `Base.astro` y `base.css`; APC
`GUIA-DE-ESTILO.md` §3.1).

## 10. 404 y página de gracias

| Página | Cómo | Fuente |
| --- | --- | --- |
| **404** | Con cabecera y pie (quien cae tiene que poder volver), `noindex`, salidas a portada y contacto. En webs con varios idiomas, `export const prerender = false` para salir en el idioma del prefijo | clays `src/pages/404.astro` |
| **Gracias** | `noindex, nofollow`, fuera del sitemap, una sola salida (portada). Es donde se mide la conversión en GTM | clays `src/pages/[...lang]/thank-you.astro`; [formulario](formulario.md) |

Las dos comparten composición en clays (`components/StatusMessage.astro`).
En webs en español, la ruta es `/gracias/`.

## 11. Rendimiento básico

- Imágenes con `<Picture>`/`<Image>` desde `src/assets/`, `widths` y `sizes`
  reales, `loading="eager"` solo en la del hero → [imágenes](imagenes.md).
- Vídeos con póster, `muted`, `playsinline`, carga diferida y arranque en
  pantalla → [vídeos](videos.md).
- Fuentes alojadas; nada de Google Fonts
  ([design system §5](design-system.md#5-fuentes-alojadas-en-la-web)).
- Scripts pequeños por componente; nada de librerías de animación para una
  aparición al scroll.
- Scroll con `requestAnimationFrame` y listeners `passive` (APC
  `BaseLayout.astro`, festival-viento `SiteHeader.astro`).
- Páginas de prueba fuera del build
  ([design system §9](design-system.md#9-styleguide-y-páginas-de-laboratorio)).

## 12. Verificación

- **`npm run build` + `npm run preview`** para juzgar carga e imágenes; en
  `dev` las imágenes son lentas y los estilos scoped pueden estar viejos →
  [trampas de Astro](../trampas-y-lecciones.md#astro-y-build).
- **Sin JavaScript** (desactivado en el navegador): todo el texto y las fotos
  visibles, menú usable, enlaces del visor llevan a su página.
- **Con `prefers-reduced-motion`**: nada se mueve, nada queda oculto.
- **Teclado**: skip-link, foco visible en todo, menú con Escape, `<details>`.
- **Geometría con `getBoundingClientRect` o `getComputedStyle`** (anchos,
  alturas, desbordes). **Los colores no se leen con `getComputedStyle`** en el
  preview: llegan obsoletos →
  [verificación y herramientas](../trampas-y-lecciones.md#verificación-y-herramientas).
- Viewport con ancho y alto explícitos antes de medir, o los reveal no
  disparan →
  [captura en blanco](../trampas-y-lecciones.md#captura-en-blanco-la-página-parece-vacía).
- Nunca «ya está» sin comprobarlo; si no se ha podido verificar, se dice
  ([forma de trabajo](../forma-de-trabajo.md#4-comprobar-antes-de-decir-que-funciona)).

## 13. Dónde queda cada cosa

| Ruta | Qué hay |
| --- | --- |
| `src/layouts/Base.astro` | `<head>`, marca `data-reveal`, skip-link, cabecera, pie, muro, script de aparición y cortina |
| `src/styles/base.css` | `.reveal` y variantes, `[data-stagger]`, `[data-parallax]`, cortina del pie, `.skip-link`, `.visually-hidden` |
| `src/components/Header.astro` · `Footer.astro` | Cabecera con menú accesible · pie con los 4 elementos legales |
| `src/components/sections/` | Moldes |
| `src/data/<idioma>.ts` · `src/data/types.ts` | Textos y sus tipos |
| `src/data/legal/` | Textos legales y `legalLinks()` para el pie |
| `src/pages/404.astro` · `src/pages/[...lang]/thank-you.astro` | 404 y gracias (sin `[...lang]` si hay un solo idioma) |
| `CLAUDE.md` · `CHECKLIST.md` | Cuestiones abiertas (móvil sin diseñar, bloques retirados por falta de datos) |

## 14. Reglas y trampas

- Contenido como datos desde el primer componente.
- Estado legible por defecto; la marca de JavaScript en `<html>` antes del
  primer pintado.
- Aparición una sola vez, sobria; nada de titulares troceados.
- Scroll-driven dentro de `@supports` y `prefers-reduced-motion:
  no-preference`; `overflow: clip`, no `hidden`.
- Un selector `+` que falla solo en `dev` →
  [trampa](../trampas-y-lecciones.md#un-selector--funciona-en-el-build-y-no-en-dev).
- Bandas a sangre y `scrollbar-gutter` →
  [trampa](../trampas-y-lecciones.md#las-bandas-a-sangre-no-llegan-al-borde-de-la-ventana).
- Embeds de terceros no se cargan sin consentimiento →
  [trampa](../trampas-y-lecciones.md#embeds-de-vimeo-y-google-maps-que-cargan-sin-consentimiento).
- Texto con degradado que sale de un color →
  [trampa](../trampas-y-lecciones.md#un-texto-con-degradado-sale-de-un-solo-color).

## 15. Checklist

**Estructura**
- [ ] Cada sección es un molde con datos; ningún texto incrustado
- [ ] Clases BEM; estilos con tokens
- [ ] Una sola `h1`; secciones en `h2`
- [ ] Datos pendientes con `Pending`; bloques sin ningún dato, retirados y anotados

**Móvil e idiomas**
- [ ] Revisado a 360, 768, 1024 y 1440px; sin scroll horizontal
- [ ] Anchos de texto en `ch`; botones y menú aguantan un 30 % más de texto

**Mejora progresiva y movimiento**
- [ ] Marca `data-reveal` en `<html>` desde el `<head>`
- [ ] Sin JavaScript: todo visible, menú usable, enlaces reales en visores
- [ ] Aparición una vez, escalonado por contenedor
- [ ] Con `prefers-reduced-motion`: nada se mueve, nada oculto
- [ ] Scroll-driven con `@supports`; ancestros con `overflow: clip`

**Accesibilidad**
- [ ] Skip-link a `#main`
- [ ] `:focus-visible` visible en todos los controles; ningún `outline: none` sin sustituto
- [ ] FAQ con `<details>` y JSON-LD `FAQPage`
- [ ] Menú móvil con `aria-expanded`, `aria-controls` y Escape
- [ ] Contraste AA; texto sobre foto medido
- [ ] `alt` en imágenes de contenido, `aria-hidden` en iconos

**Cabecera, pie y páginas de sistema**
- [ ] Cabecera que no tapa las anclas ni deja el logo sobre foto
- [ ] Pie con aviso legal, privacidad, cookies y «Preferencias de privacidad»
- [ ] 404 con cabecera, pie y `noindex`
- [ ] Gracias con `noindex, nofollow`, fuera del sitemap

**Verificación**
- [ ] `npm run build` sin errores y revisado con `npm run preview`
- [ ] Geometría medida; colores revisados con inspección, no con `getComputedStyle`
