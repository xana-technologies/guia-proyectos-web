# Catálogo de animaciones

Las animaciones y efectos de movimiento que ya existen en las webs de Xana,
cada una con un **ID**, dónde verla, cómo está hecha y su estado. Sirve para
que la skill `xana:animar` ofrezca a Belén **opciones concretas y ya
probadas** para cada sección, en vez de describir efectos en abstracto.

Todo lo que hay aquí está leído en el código actual de cada proyecto (se cita la
ruta). Lo que no se ha podido comprobar va marcado **«comprobar»**.

## Reglas que valen para todas

No se repiten aquí: están en su sitio y se aplican siempre.

| Regla | Dónde está |
| --- | --- |
| **Estado legible por defecto; sin JavaScript no queda nada oculto.** La marca de JS va en `<html>` antes del primer pintado | [maquetación §6](flujos/maquetacion.md#6-mejora-progresiva-sin-javascript-no-queda-nada-oculto) |
| **`prefers-reduced-motion` apaga lo que se mueve, no lo que solo cambia de color** | [design system §4.4](flujos/design-system.md#44-radios-sombras-capas-y-movimiento) · [movimiento avanzado § Qué cambia en la base](modulos/movimiento-avanzado.md#qué-cambia-en-la-base) |
| **Duraciones, recorrido y escalonado con tokens**, que valen `0` con reducir movimiento. Un bucle infinito, además, dentro de la media query | [design system §4.4](flujos/design-system.md#44-radios-sombras-capas-y-movimiento) |
| **CSS antes que JavaScript; JavaScript propio antes que librería.** Una librería (GSAP, Lenis, WebGL) solo con el módulo | [movimiento avanzado](modulos/movimiento-avanzado.md#qué-añade-al-código) |
| **Animar `transform` y `opacity`** (y `clip-path`, `translate`, `scale`); nunca anchos, altos ni posiciones que recalculen la maqueta | Comentarios de clays `Band.astro` («Escala en vez de ancho») y `Gallery.astro` |
| **No retrasar el LCP**: lo que se ve de primeras no parte de `opacity: 0`, y el vídeo del hero no compite con la imagen | clays `Hero.astro` (comentario de la entrada del titular) · [vídeos §5](flujos/videos.md#5-hero-con-vídeo-sin-perjudicar-el-lcp--claude) |
| **Scroll-driven dentro de `@supports`** y ancestros con `overflow: clip` | [maquetación §7](flujos/maquetacion.md#css-scroll-driven-sin-javascript) · [trampa](trampas-y-lecciones.md#un-reveal-ligado-al-scroll-no-anima) |
| **Hover solo con `@media (hover: hover)`** cuando cambia tamaño o encuadre: en táctil el `:hover` se queda pegado | clays `Products.astro`, `Gallery.astro`, `Lightbox.astro` |
| **Proponer en una página y esperar visto bueno antes de extender** | [movimiento avanzado](modulos/movimiento-avanzado.md#cuándo-se-usa-y-cuándo-no) |
| Verificar: viewport fijo, `build` + `preview`; en el preview `requestAnimationFrame` y el scroll suave no avanzan | [trampas · Verificación](trampas-y-lecciones.md#verificación-y-herramientas) |

### `animation-timeline` (scroll-driven): compatibilidad

Según caniuse (datos de agosto de 2026): Chrome y Edge 115+, Safari 26+ (también
iOS 26), **Firefox 158+**; uso global ≈ 87 %. MDN la sigue marcando como
«Limited availability», no Baseline. En la práctica: **quien tenga un navegador
anterior ve el estado final quieto**, y así tiene que estar bien. Todas las
fichas que dependen de ella lo resuelven con `@supports`.

### Leyenda de estado

| Marca | Significado |
| --- | --- |
| ✅ | Aprobada por Belén y en uso. Se dice si el proyecto está publicado o en construcción |
| 🟡 | Probada en código, sin decidir (o en página de laboratorio) |
| ❌ | Rechazada por Belén (con la memoria que lo recoge). **No se ofrece** |
| ⚪ | Experimento o código que no usa ninguna página |

Técnica: **CSS** (transición o `@keyframes`) · **CSS scroll-driven**
(`animation-timeline`) · **JS IO** (`IntersectionObserver`) · **JS scroll**
(scroll + `requestAnimationFrame`) · **GSAP** · **Lenis** · **WebGL**.

## Índice

1. [Aparición al entrar en pantalla](#1-aparición-al-entrar-en-pantalla)
2. [Ligadas al scroll](#2-ligadas-al-scroll)
3. [Parallax y profundidad](#3-parallax-y-profundidad)
4. [Cabecera y pie](#4-cabecera-y-pie)
5. [Hover e interacción](#5-hover-e-interacción)
6. [Imagen y vídeo](#6-imagen-y-vídeo)
7. [Texto, cifras y mockups](#7-texto-cifras-y-mockups)
8. [Intro, loader y cursor](#8-intro-loader-y-cursor)
9. [Fondos y degradados](#9-fondos-y-degradados)
10. [Rechazadas](#10-rechazadas)
11. [Cómo ofrecer opciones](#11-cómo-ofrecer-opciones)
12. [Fuentes](#12-fuentes)

---

## 1. Aparición al entrar en pantalla

### `aparicion-subida` · Sube y aparece

Un bloque entra fundiéndose y subiendo unos píxeles cuando llega a la pantalla, una sola vez.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com (cabeceras de sección, paneles, formulario) · APC `/` y resto de páginas (`data-reveal`) |
| Técnica · coste | JS IO + CSS · un script de ~20 líneas en el layout |
| Sin JS / reducir movimiento | Todo visible (la marca en `<html>` no llega) / aparece sin animar |
| Cuándo sí / no | Sí: cualquier web, bloques con forma (cabeceras, tarjetas, paneles). No: prosa larga (→ `aparicion-fundido`) ni lo que se ve al cargar |
| Estado | ✅ clays (publicada) · ✅ APC (en construcción; memoria `apc-movimiento-web`: «listón de sobriedad aceptado») |
| Parámetros | clays: 28px, 700ms, `--ease-out-soft`, `rootMargin: 0px 0px -12% 0px` · APC: 18px, 620ms, `rootMargin: 0px 0px -10% 0px`, `threshold: 0.12` · xanasystem-web: → ver ficha propia más abajo |

**Código:** el de clays está entero en
[maquetación §6 y §7](flujos/maquetacion.md#6-mejora-progresiva-sin-javascript-no-queda-nada-oculto)
(`src/layouts/Base.astro` + `src/styles/base.css`). Variante APC con clase:

```css
/* APC-gestion-patrimonial/src/styles/global.css */
.js-reveal [data-reveal] {
  opacity: 0;
  transform: translateY(var(--reveal-shift)); /* 18px */
  transition:
    opacity var(--dur-reveal) var(--ease-out),   /* 620ms */
    transform var(--dur-reveal) var(--ease-out);
}
.js-reveal [data-reveal].is-revealed { opacity: 1; transform: none; }
```

### `aparicion-escalonada` · En cascada

Los hijos de una fila o lista corta entran uno detrás de otro.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com: bloques de la banda 03 (`Band`), productos, cifras · APC: pasos del proceso, tarjetas de servicios, FAQ |
| Técnica · coste | JS IO + CSS · mismo script que `aparicion-subida` |
| Sin JS / reducir movimiento | Todo visible / todo a la vez, sin animar (`--reveal-stagger: 0ms`) |
| Cuándo sí / no | Sí: 3–6 elementos que caben en pantalla a la vez. No: listas más altas que la pantalla (ya entran a destiempo) |
| Estado | ✅ clays (publicada) · ✅ APC |
| Parámetros | clays: `--reveal-stagger: 70ms`, retardo hasta el 6.º hijo y luego se repite · APC: `data-reveal-stagger="55"` a `"90"` según sección (70 por defecto) |

**Código:** clays vigila el **contenedor** y el retardo lo pone el CSS
(`[data-stagger] > *:nth-child(n)`), en
[maquetación §7](flujos/maquetacion.md#con-intersectionobserver-lo-normal).
APC lo reparte el script:

```ts
// APC-gestion-patrimonial/src/layouts/BaseLayout.astro
document.querySelectorAll<HTMLElement>("[data-reveal-stagger]").forEach((group) => {
  const step = Number(group.dataset.revealStagger) || 70;
  group.querySelectorAll<HTMLElement>("[data-reveal]").forEach((el, i) => {
    el.style.transitionDelay = `${i * step}ms`;
  });
});
```

Trampa: en APC cada hijo se vigila por separado, así que el retardo se suma a la
diferencia de altura; clays lo evita vigilando el contenedor.

### `aparicion-fundido` · Solo funde

Aparece sin recorrido, solo con opacidad.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, sección de cifras (`Figures`): antetítulo, entradilla y cuerpo |
| Técnica · coste | JS IO + CSS · clase `.reveal--fade` sobre `.reveal` |
| Sin JS / reducir movimiento | Visible / sin animar |
| Cuándo sí / no | Sí: prosa larga, párrafos (un bloque de veinte renglones subiendo «se lee como un salto de maquetación», `base.css`). No: tarjetas |
| Estado | ✅ clays (publicada) |
| Parámetros | 700ms, `--ease-out-soft` |

```css
/* clays-of-the-world/src/styles/base.css */
html[data-reveal] .reveal--fade { transform: none; }
```

### `titular-asoma` · El titular sale de detrás de su borde

El titular entero sube desde detrás de una máscara, sin fundirse.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, titular de la sección de cifras (`sections/Figures.astro`, `h2.reveal.reveal--rise`) |
| Técnica · coste | JS IO + CSS · pide marcado: la clase en el elemento que recorta y el texto en un `<span>` |
| Sin JS / reducir movimiento | Visible / sin movimiento |
| Cuándo sí / no | Sí: titulares cortos y grandes. No: párrafos; marcas de tono muy sereno (ver `hero-renglones`) |
| Estado | ✅ clays (publicada) |
| Parámetros | `translateY(110%)`, 700ms; `padding-bottom: 0.12em` para no cortar descendentes |

```css
/* clays-of-the-world/src/styles/base.css */
html[data-reveal] .reveal--rise {
  overflow: hidden;
  padding-bottom: 0.12em;
  margin-bottom: -0.12em;
}
html[data-reveal] .reveal--rise > * {
  display: block;
  transform: translateY(110%);
  transition: transform var(--duration-slower) var(--ease-out-soft);
}
html[data-reveal] .reveal--rise.is-visible > * { transform: none; }
```

No es un titular troceado: el bloque sube entero. Aun así, **ofrecerlo con
cuidado** en marcas editoriales y serenas (ver [rechazadas](#10-rechazadas)).

### `hero-renglones` · Titular del hero renglón a renglón

Cada renglón del titular del hero asoma desde su máscara al cargar, escalonado; después entra la pastilla con el texto.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, hero de la portada (`sections/Hero.astro`) |
| Técnica · coste | CSS `@keyframes` al cargar (no espera al observador). Los renglones vienen del **dato** (`headline` es un array), no se trocean por script |
| Sin JS / reducir movimiento | Titular puesto / sale puesto (duración y escalonado a 0) |
| Cuándo sí / no | Sí: hero con titular de 2–3 renglones cortos. No: marcas editoriales serenas; nunca troceado palabra a palabra |
| Estado | ✅ clays (publicada) · ❌ el equivalente en APC (memoria `apc-movimiento-web`) |
| Parámetros | `translateY(120%)`, `--duration-slow` (450ms), retardo `--i × 70ms`; pastilla: 700ms tras `70ms × 3` |

```css
/* clays-of-the-world/src/components/sections/Hero.astro */
html[data-reveal] .hero__line { overflow: hidden; padding-bottom: 0.12em; margin-bottom: -0.12em; }
html[data-reveal] .hero__line-inner {
  display: block;
  animation: hero-line var(--duration-slow) var(--ease-out-soft) both;
  animation-delay: calc(var(--i) * var(--reveal-stagger));
}
@keyframes hero-line { from { transform: translateY(120%); } to { transform: translateY(0); } }
```

- **No toca la opacidad**: el hero es el LCP y la máscara ya tapa el texto
  (comentario del componente).
- **Incoherencia con la guía**: [maquetación §7](flujos/maquetacion.md#con-intersectionobserver-lo-normal)
  dice «nada de titulares troceados». En APC Belén rechazó la entrada «línea a
  línea» en los `h1`; en clays está publicada. Ofrecer solo con la advertencia
  y nunca a una marca de tono editorial y sereno.

### `foto-velo` · Se levanta un velo sobre la foto

La foto está quieta en su sitio y una capa del color del fondo se desvanece encima.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, pieza de vídeo de la sección 04 (`sections/Showcase.astro`, `.reveal.reveal--veil`) |
| Técnica · coste | JS IO + CSS (`::after`) |
| Sin JS / reducir movimiento | Foto visible / sin velo |
| Cuándo sí / no | Sí: fotografía o vídeo grande donde interesa el material. No: sobre foto a sangre ya con parallax (dos efectos a la vez) |
| Estado | ✅ clays (publicada). Sustituyó a un barrido con `clip-path` («se lee como una transición», `base.css`) |
| Parámetros | `--duration-base` (250ms, rápido a propósito); color con `--veil-color` en secciones oscuras |

```css
/* clays-of-the-world/src/styles/base.css */
html[data-reveal] .reveal--veil { position: relative; opacity: 1; transform: none; }
html[data-reveal] .reveal--veil::after {
  content: ''; position: absolute; inset: 0; z-index: 2;
  background-color: var(--veil-color, var(--color-bg));
  border-radius: inherit; pointer-events: none;
  transition: opacity var(--duration-base) var(--ease-out-soft);
}
html[data-reveal] .reveal--veil.is-visible::after { opacity: 0; }
```

### `arco-se-dibuja` · El recurso gráfico se dibuja de arriba abajo

Un motivo decorativo (el arco de APC) se descubre con un recorte que baja.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC: icono del arco en los cierres (`sections/*/Cierre.astro`), arco de fondo de «Cómo podemos ayudarte» (`home/Servicios.astro`), `Proceso`, `ComoEmpezamos`, `Principios` |
| Técnica · coste | JS scroll (se mide en el mismo bucle del parallax, sin IO, «así nunca se quedan ocultos») + CSS `clip-path` |
| Sin JS / reducir movimiento | Visible / `clip-path: none` |
| Cuándo sí / no | Sí: marca con un recurso gráfico lineal propio. No: fotos (→ `foto-velo`) |
| Estado | ✅ APC (en construcción). Los iconos pequeños, **cerrados por abajo** (memoria `apc-arco-icono-cerrado`) |
| Parámetros | `--dur-arch: 1100ms`; se revela al pasar del 92 % del alto de la ventana |

```css
/* APC-gestion-patrimonial/src/styles/global.css */
.js-reveal [data-reveal="arch"] {
  opacity: 0;
  transform: none;
  clip-path: inset(0 0 100% 0);
  transition: clip-path var(--dur-arch) var(--ease-out), opacity var(--dur-base) var(--ease-out);
}
.js-reveal [data-reveal="arch"].is-revealed { opacity: 1; clip-path: inset(0 0 0 0); }
```

### `aparicion-scroll-css` · Aparición ligada al recorrido, sin JavaScript

Los bloques suben y aparecen a medida que entran por abajo; el avance va con el scroll, no con un disparo.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local): titulares e intros de todas las secciones, tarjetas de características, patrocinadores, zonas del mapa (`src/styles/global.css`, clase `.revelar`) |
| Técnica · coste | CSS scroll-driven `view()` · cero JS |
| Sin JS / reducir movimiento | Funciona igual / no se aplica (va dentro de `no-preference`) |
| Compatibilidad | Sin soporte: contenido visible y quieto (la regla está dentro de `@supports`) |
| Cuándo sí / no | Sí: webs sin JS de aparición, listas de tarjetas. No: el hero («El hero **no** se revela», festival `docs/design.md`); si se quiere que ocurra una sola vez (esto se deshace al subir) |
| Estado | 🟡 festival-viento (sin publicar; la regla está escrita en su `docs/design.md`: solo bloques, solo `opacity` + `translateY`, máx. 24px) |
| Parámetros | 24px; `animation-range: entry 0% entry 55%`; escalonado desplazando el rango: hijo 2 `entry 8% entry 63%`, hijo 3 `16%/71%`, hijo `4n` `24%/79%` |

El bloque base está en [maquetación §7](flujos/maquetacion.md#css-scroll-driven-sin-javascript).
El escalonado:

```css
/* festival-viento/src/styles/global.css */
@keyframes revelar-fade {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}
/* dentro de @media (prefers-reduced-motion: no-preference) y @supports (animation-timeline: view()) */
.carac__rejilla > .revelar:nth-child(2),
.sponsors__rejilla > .revelar:nth-child(4n+2) {
  animation-range: entry 8% entry 63%;
}
```

Trampa: el escalonado por `4n` supone cuatro columnas; en móvil (una columna) y
en listas verticales (`.mapa__zonas`) no se ve cascada.

### `aparicion-desenfoque` · Sube, aparece y se enfoca

Como `aparicion-subida`, pero el bloque parte desenfocado.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com: tarjeta del formulario de contacto (`Contact.astro`); título y tarjetas de marcas (`Brands.astro`, variante con `@keyframes` palabra a palabra) |
| Técnica · coste | JS IO + CSS (`filter: blur`) · el desenfoque es caro de pintar en bloques grandes |
| Sin JS / reducir movimiento | **Invisible: no hay `<noscript>` ni marca en `<html>`** (deuda) / visible |
| Cuándo sí / no | No se ofrece por defecto: el desenfoque de entrada se quitó en xanasystem-web |
| Estado | ✅ xanatechnologies (publicada; sin memoria de aprobación) · ❌ en xanasystem-web (memoria `reveal-blur-in`) |
| Parámetros | `translateY(14px)`, `blur(8px)`, `--dur-slow` 520ms, retardo `--reveal-i × 90ms` con tope en 6 hermanos; IO `rootMargin: '0px 0px -10% 0px'`, `threshold: 0.1` |

```css
/* xanatechnologies/src/styles/global.css */
[data-reveal] { --reveal-i: 0; opacity: 0; transform: translateY(14px); filter: blur(8px);
  transition: opacity var(--dur-slow) var(--ease-out), transform var(--dur-slow) var(--ease-out), filter var(--dur-slow) var(--ease-out);
  transition-delay: calc(var(--reveal-i) * 90ms); }
[data-reveal].is-visible { opacity: 1; transform: none; filter: none; will-change: auto; }
```

Lo aprovechable es el **índice por hermanos con tope** (`--reveal-i`, lo
calcula `Base.astro`), no el desenfoque.

### `aparicion-cascada-auto` · Cascada automática por sección

Cada hijo del contenedor de cada sección entra con fundido y 10px de subida, uno detrás de otro, sin marcar nada a mano.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local; `xanasystem.com` sirve todavía otra web, comprobado el 15-09-2026): todas las secciones de todas las páginas; heros con retardos a mano |
| Técnica · coste | `<script is:inline>` antes del primer pintado que pone `data-reveal` y `--reveal-delay` + JS IO + CSS |
| Sin JS / reducir movimiento | Visible por un `<noscript><style>…!important</style></noscript>` / visible (el estado oculto solo existe bajo `no-preference`) |
| Cuándo sí / no | Sí: webs con muchas secciones parecidas donde marcar cada bloque es ruido. No: secciones con piezas absolutas o con su propio movimiento (`data-no-autoreveal`) |
| Estado | ✅ xanasystem-web (en construcción): «cascada por elemento» aprobada (memoria `reveal-blur-in`) |
| Parámetros | 10px con `translate`, `--dur-base` 250ms; retardo `min(i, 6) × 80ms` (máx. 480ms); IO `rootMargin: '0px 0px -18% 0px'`, `threshold: 0.12`; hero de portada con 180/280/380/480ms inline |

```css
/* xanasystem-web/src/styles/global.css */
@media (prefers-reduced-motion: no-preference) {
  [data-reveal] {
    opacity: 0;
    translate: 0 10px;
    transition:
      opacity var(--dur-base) var(--ease-out),
      translate var(--dur-base) var(--ease-out);
    transition-delay: var(--reveal-delay, 0ms);
  }
  [data-reveal].is-in { opacity: 1; translate: none; }
}
```

- La marca en `<html>` de clays es preferible al `<noscript>`
  ([maquetación §6](flujos/maquetacion.md#6-mejora-progresiva-sin-javascript-no-queda-nada-oculto)).
- Usa `translate` y no `transform`: no pisa otros `transform` del elemento.
- Incoherencias en el propio proyecto: la memoria `reveal-blur-in` da otros
  retardos para el hero (0/90/200/300ms) y los comentarios de `global.css`
  citan un `scripts/reveal.js` que no existe y dicen «sin escalonado».

---

## 2. Ligadas al scroll

### `texto-se-enciende` · El párrafo se enciende al bajar

Una declaración grande entra en gris claro y las palabras pasan al color de texto a medida que sube por la pantalla.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, sección 02 (`sections/CompanyIntro.astro`) |
| Técnica · coste | JS scroll (un número, `--lit`) + CSS `color-mix` por palabra. El troceo lo hace el componente en el servidor |
| Sin JS / reducir movimiento | Texto oscuro, legible (`--lit: 1` por defecto) / **sigue funcionando**: solo cambia color |
| Cuándo sí / no | Sí: un bloque de texto que es el argumento entero, sin imagen. No: textos cortos; junto con otra aparición en el mismo bloque |
| Estado | ✅ clays (publicada) |
| Parámetros | Línea de lectura al 60 % de la ventana; `PISO = 0.15` (el principio ya encendido); `RECORRIDO = 1.25`; rampa `--ramp: 12` palabras |

```css
/* clays-of-the-world/src/components/sections/CompanyIntro.astro */
.intro__statement { --lit: 1; --ramp: 12; }
.intro__word {
  color: var(--color-text); /* respaldo sin color-mix */
  --on: clamp(0, calc((var(--lit) * (var(--total) + var(--ramp)) - var(--i)) / var(--ramp)), 1);
  color: color-mix(in srgb, var(--color-text) calc(var(--on) * 100%), var(--color-text-dormant));
}
```

```ts
// mismo fichero: por fotograma
const cruzado = (alto * 0.6 - top) / (height * 1.25);
const avance = 0.15 + 0.85 * Math.min(1, Math.max(0, cruzado));
bloque.style.setProperty('--lit', String(avance));
```

- El índice `--i` corre **a través de los bloques**, no dentro de cada uno
  (si no, se ven dos frentes a la vez).
- Separar por espacios no vale para chino o japonés (`Intl.Segmenter`).
- El gris «apagado» no llega a AA a sabiendas (`--color-text-dormant`):
  [design system §11](flujos/design-system.md#11-reglas-y-trampas).

### `cierre-se-estrecha` · La banda de cierre se estrecha y redondea

Al llegar al final de la página, la banda de cierre a sangre encoge un poco y redondea las esquinas, dejando ver el color del pie alrededor.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC, cierre de `/`, `/gestion-patrimonial/`, `/sobre-apc/` y `/conocimiento/` (`sections/*/Cierre.astro`) |
| Técnica · coste | CSS scroll-driven, `scroll(root block)` · sin JS |
| Sin JS / reducir movimiento | Funciona sin JS / `animation: none`, banda a sangre |
| Compatibilidad | Ver [arriba](#animation-timeline-scroll-driven-compatibilidad). Sin soporte: banda a sangre sin radio. Solo ≥ 900px |
| Cuándo sí / no | Sí: banda de cierre oscura con CTA justo antes de un pie del mismo color. No: si el cierre no es lo último antes del pie (el rango va por % de la página) |
| Estado | ✅ APC (en construcción; memoria `apc-estado-desarrollo`) |
| Parámetros | `animation-range: 84% 100%`; `scale(0.94)`; radio `clamp(1.5rem, 3vw, 2.5rem)` |

Código completo en [movimiento avanzado §1](modulos/movimiento-avanzado.md#1-css-ligado-al-scroll).
Los keyframes:

```css
/* APC-gestion-patrimonial/src/components/sections/home/Cierre.astro */
@keyframes banner-reveal {
  from { transform: scale(1);    border-radius: 0; }
  to   { transform: scale(0.94); border-radius: clamp(1.5rem, 3vw, 2.5rem); }
}
```

Trampa: el rango es sobre el **scroll de toda la página**; en una página muy
corta o muy larga el 84 % cae en otro sitio. Está duplicado en cuatro
componentes con cuatro nombres de `@keyframes`.

### `video-a-sangre` · La tarjeta de vídeo se ensancha hasta el borde

Un vídeo que empieza como tarjeta con márgenes y esquinas redondeadas se abre a todo el ancho, sin radio, al subir por la pantalla.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), tráiler de Características (`src/components/Caracteristicas.astro`) |
| Técnica · coste | JS scroll con `requestAnimationFrame` que escribe `--p` (0→1) + CSS |
| Sin JS / reducir movimiento | Tarjeta con márgenes / `--p: 0 !important` |
| Cuándo sí / no | Sí: una pieza audiovisual que merece protagonismo a mitad de página. No: con embed de terceros (el de festival es Vimeo, prohibido por su propio `CLAUDE.md`) |
| Estado | 🟡 festival-viento (sin publicar) |
| Parámetros | `p = 0` cuando asoma por abajo, `p = 1` cuando su borde superior llega al 25 % de la ventana; radio 1,25rem → 0; proporción 4/3 en móvil y 21/9 desde 768px |

```css
/* festival-viento/src/components/Caracteristicas.astro */
.carac__trailer-zona { --p: 0; padding-inline: calc(var(--gutter) * (1 - var(--p))); overflow-x: clip; }
.carac__trailer { max-width: calc(var(--ancho-max) + (100vw - var(--ancho-max)) * var(--p));
  border-radius: calc(1.25rem * (1 - var(--p))); overflow: hidden; }
@media (prefers-reduced-motion: reduce) { .carac__trailer-zona { --p: 0 !important; } }
```

```js
const inicio = vh; const fin = vh * 0.25; const t = rect.top;
let p = (inicio - t) / (inicio - fin);
if (p < 0) p = 0; else if (p > 1) p = 1;
zona.style.setProperty('--p', p.toFixed(4));
```

**Mejorable antes de reutilizarlo:** anima `max-width`, `padding` y
`border-radius` (maqueta en cada fotograma). Rehacerlo con `transform: scale`
y `clip-path: inset(… round …)`. Es el mismo gesto que `cierre-se-estrecha`,
pero al revés.

### `hero-se-encoge` · El hero de vídeo se convierte en tarjeta al bajar

El vídeo a pantalla completa gana márgenes y radio mientras su interior queda fijo, y un texto se releva por otro.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com/v2/ (`noindex`; `src/components/HeroShrink.astro`) y el mismo mecanismo en `CustomSolutionsBanner.astro` |
| Técnica · coste | JS scroll + `position: sticky` en una sección de 220vh |
| Sin JS / reducir movimiento | Hero quieto / sin encogimiento ni latido, **pero el vídeo sigue en autoplay** |
| Cuándo sí / no | Sí: hero con vídeo muy potente y web de producto con tono tecnológico. No: marcas serenas; si el vídeo no puede ir en R2 (el de v2 son 10,7 MB en el repo) |
| Estado | 🟡 xanatechnologies v2 (propuesta no publicada en la portada) |
| Parámetros | `PAD_X = 9` vw, `PAD_TOP = 96` px, `PAD_BOTTOM = 28` px, `RADIUS = 34` px; texto A sale en `p/0.42` subiendo 30px, texto B entra en `(p-0.5)/0.42` |

```js
// xanatechnologies/src/components/HeroShrink.astro
pad.style.paddingLeft = p * PAD_X + 'vw'; pad.style.paddingRight = p * PAD_X + 'vw';
card.style.borderRadius = p * RADIUS + 'px';
```

Mismo aviso que `video-a-sangre`: anima relleno y radio en vez de `transform`.

### `frase-fijada` · Una frase fijada que se escribe con el scroll

La sección queda fija durante varias pantallas: la frase aparece palabra a palabra, crece una raya, la frase se va y entran el párrafo y la línea final.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, portada, tras el hero (`src/components/Ecosystem.astro`) |
| Técnica · coste | JS scroll + `position: sticky` en una sección de 500vh (420vh con ≤ 640px de alto) |
| Sin JS / reducir movimiento | **500vh vacíos con el texto a `opacity: 0`** (deuda) / sin fijado, todo visible |
| Cuándo sí / no | Solo con el módulo de movimiento avanzado, en webs de producto «cinematográficas». No: marcas editoriales; nunca palabra a palabra con desenfoque en webs donde se rechazó |
| Estado | ✅ xanatechnologies (publicada; sin memoria de aprobación) · el gesto palabra a palabra está ❌ en APC y xanasystem-web |
| Parámetros | Tramos del recorrido: palabras 0,04–0,30 (ventana 0,16), raya 0,2–0,32, sale título 0,56–0,62, entra párrafo 0,66–0,73, cierre 0,7–0,77, sale todo 0,93–0,98; cada palabra sube 44px desde `blur(12px)`; brillo de la raya `2.6s infinite` |

```js
// xanatechnologies/src/components/Ecosystem.astro
el.style.opacity = e.toFixed(3);
el.style.transform = `translateY(${(rise * (1 - e)).toFixed(2)}px)`;
el.style.filter = e >= 1 ? 'none' : `blur(${(blur * (1 - e)).toFixed(2)}px)`;
// words.forEach((w, i) => { const s = WORDS_START + i * stagger; fadeRise(w, smooth(seg(p, s, s + WIN)), 44, 12); });
```

### `texto-se-enciende-css` · El párrafo se enciende al bajar, sin JavaScript

Mismo gesto que `texto-se-enciende`: la frase pasa de gris a oscuro palabra a palabra mientras cruza la pantalla, hecho solo con CSS.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local): portada (`sections/SolutionBlock.astro`) y hero de `/empresa` (`sections/empresa/EmpresaHero.astro`) |
| Técnica · coste | CSS scroll-driven con `view-timeline` con nombre · cero JS |
| Sin JS / reducir movimiento | Funciona sin JS / frase oscura entera |
| Compatibilidad | Sin soporte: frase oscura entera (dentro de `@supports`) |
| Cuándo sí / no | Sí: igual que `texto-se-enciende`; se prefiere esta si basta con que funcione en los navegadores con soporte. No: con un ancestro con `overflow: hidden` (no anima) |
| Estado | ✅ xanasystem-web (en construcción): el realce «se CONSERVA» (memoria `reveal-blur-in`) |
| Parámetros | Tramo por palabra `cover i/n·75%` → `+22%`; de `#9aa1ad` a `var(--c-heading)` |

```css
/* xanasystem-web/src/components/sections/SolutionBlock.astro */
@supports (animation-timeline: view()) {
  @media (prefers-reduced-motion: no-preference) {
    .solution__statement { view-timeline-name: --reveal; view-timeline-axis: block; }
    .w {
      animation: word-lit linear both;
      animation-timeline: --reveal;
      animation-range:
        cover calc(var(--i) / var(--n) * 75%)
        cover calc(var(--i) / var(--n) * 75% + 22%);
    }
  }
}
@keyframes word-lit { from { color: var(--reveal-dim); } to { color: var(--reveal-lit); } }
```

Cada palabra: `<span class="w" style={`--i:${i}`}>`; el párrafo lleva
`--n:${words.length}`. Diferencia con la de clays: aquí **se apaga con
reducir movimiento** (va dentro de `no-preference`), aunque solo cambia color.
El párrafo lleva además `data-reveal`: dos efectos en el mismo bloque, que
clays evita a propósito.

### `pasos-en-bucle` · El paso que cruza el centro se anima

En una lista de pasos con mockups, el paso que pasa por la franja central de la pantalla reproduce su pequeña animación en bucle.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local): `/pim` (`PimWorkflow.astro`); el mismo patrón en `/dam` (`DamCapabilities`, `DamPimVs`, `DamWorkflow`), `/catalogos` (`CatalogosSelective`), `/ia` (`IaCases`) |
| Técnica · coste | JS IO que pone `.is-anim` y la reinicia cada vuelta + CSS `@keyframes` bajo `:global(.wf__step.is-anim)` |
| Sin JS / reducir movimiento | Mockups completos y quietos / quietos |
| Cuándo sí / no | Sí: webs de producto que explican un proceso con pantallas. No: webs de servicios o editoriales |
| Estado | 🟡 xanasystem-web (en construcción; ninguna memoria lo aprueba ni lo rechaza) |
| Parámetros | Franja `rootMargin: '-45% 0px -45% 0px'`; `LOOP = 3800`ms (4200 en `DamCapabilities`, `threshold: 0.35`) |

```js
// xanasystem-web/src/components/sections/PimWorkflow.astro
const play = () => {
  if (!animStep) return;
  animStep.classList.remove('is-anim');
  void animStep.offsetWidth; // reflow → reinicia las animaciones
  animStep.classList.add('is-anim');
};
```

---

## 3. Parallax y profundidad

### `parallax-css` · La foto a sangre se desplaza dentro de su marco (CSS)

La foto se agranda un punto y se mueve más despacio que la página dentro de su recorte.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com: hero, fotos de la banda 03 (`Band`), fondo de cifras (`Figures`) |
| Técnica · coste | CSS scroll-driven `view()` · sin JS |
| Sin JS / reducir movimiento | Funciona sin JS / foto quieta |
| Compatibilidad | Sin soporte, foto quieta («quieta es el estado correcto», `base.css`) |
| Cuándo sí / no | Sí: fotografía a sangre o de fondo con recorte (`overflow: hidden`/`clip` en el contenedor). No: fotos pequeñas dentro de tarjetas; si hace falta componer con otra animación en el mismo `transform` (→ `parallax-js`) |
| Estado | ✅ clays (publicada) |
| Parámetros | `--parallax-shift: 6%` (del alto del propio elemento); `--parallax-scale: 1.14` (mínimo `1 + 2 × shift`); `animation-range: cover 0% cover 100%` |

```css
/* clays-of-the-world/src/styles/base.css */
@media (prefers-reduced-motion: no-preference) {
  @supports (animation-timeline: view()) {
    [data-parallax] {
      animation: parallax-drift linear both;
      animation-timeline: view();
      animation-range: cover 0% cover 100%;
      will-change: transform;
    }
  }
}
@keyframes parallax-drift {
  from { transform: translateY(calc(var(--parallax-shift) * -1)) scale(var(--parallax-scale)); }
  to   { transform: translateY(var(--parallax-shift)) scale(var(--parallax-scale)); }
}
```

«Acortar el rango no acorta el recorrido: lo acelera» (`base.css`).

### `parallax-js` · Parallax escribiendo variables (JS)

Mismo efecto, pero el script solo escribe `--py` y cada componente decide cómo usarla.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC: foto del hero, fondos de cierres y de servicios oscuros, fotos laterales (`data-parallax` en casi todas las secciones) |
| Técnica · coste | JS scroll con `requestAnimationFrame`, solo elementos en pantalla |
| Sin JS / reducir movimiento | Foto quieta / no se engancha |
| Cuándo sí / no | Sí: cuando el mismo `transform` tiene que componer con otra animación (ver `zoom-lento-fondo`), o hace falta en navegadores sin `animation-timeline`. No: si basta `parallax-css` |
| Estado | ✅ APC (en construcción) |
| Parámetros | `data-parallax` 0,03 (arcos), 0,04–0,05 (fotos laterales, hero), 0,06 (fondos a sangre); `data-parallax-zoom` 1,08 / 1,1 |

Código en [movimiento avanzado §2](modulos/movimiento-avanzado.md#2-parallax-escribiendo-variables).
El CSS que lo recibe:

```css
/* APC-gestion-patrimonial/src/styles/global.css */
[data-parallax] {
  transform: translate3d(0, var(--py, 0px), 0) scale(var(--pz, 1));
  will-change: transform;
}
[data-parallax-zoom="1.08"] { --pz: 1.08; }
[data-parallax-zoom="1.1"]  { --pz: 1.1; }
```

### `parallax-fondo` · La foto de fondo va a un cuarto de la velocidad

Una sección con foto de fondo cuya capa se desplaza más despacio que la página.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), sección `#globos` (`src/components/Globos.astro`) |
| Técnica · coste | JS scroll con `requestAnimationFrame`, ~30 líneas; `translate3d` directo |
| Sin JS / reducir movimiento | Foto quieta / no se engancha y el CSS fuerza `transform: none` |
| Cuándo sí / no | Sí: banda con foto de fondo sin texto encima que dependa del contraste. Hoy se prefiere `parallax-css` o `parallax-js` (variable en vez de `transform`) |
| Estado | 🟡 festival-viento (sin publicar) |
| Parámetros | Factor `-0.25` sobre la distancia entre el centro de la sección y el de la ventana; calcula solo a ±200px de la pantalla; la capa sobresale `inset: -10% 0` |

```js
// festival-viento/src/components/Globos.astro
const delta = (centroSeccion - centroViewport) * -0.25;
media.style.transform = `translate3d(0, ${delta.toFixed(1)}px, 0)`;
```

Trampa: el margen de sobra es un 10 % del alto de la sección y el recorrido
crece con el alto de la ventana; en pantallas altas puede asomar el borde
(comprobar). `matchMedia` se lee una vez.

### `fondo-fijo` · Fondo quieto mientras el texto sube

La textura de fondo se queda fija en la ventana y el contenido pasa por encima.

| Dato | Valor |
| --- | --- |
| Dónde verla | magnaceramica (local), sección «About» (`src/components/About.astro`) |
| Técnica · coste | CSS `background-attachment: fixed` · cero JS |
| Sin JS / reducir movimiento | Igual / igual (no se contempla) |
| Cuándo sí / no | Sí: una textura sin información (mármol). No: fotos con asunto (`cover` sobre la ventana entera las amplía); en móvil se desactiva |
| Estado | 🟡 magnaceramica (publicación de la versión Astro sin verificar) |
| Parámetros | `background-attachment: scroll` por debajo de 768px |

```css
/* magnaceramica/src/components/About.astro */
.about { background-image: url("/img/corporate-bg.webp"); background-size: cover; background-position: center; background-attachment: fixed; }
/* background-attachment: fixed is unreliable on mobile */
@media (max-width: 768px) { .about { background-attachment: scroll; } }
```

La imagen va en `public/`, sin optimizar
([trampa](trampas-y-lecciones.md#las-imágenes-de-public-pesan-lo-que-pesa-el-original)).

### `hero-sigue-raton` · El hero responde al ratón y se va al bajar

El halo del hero se mueve con el ratón y el contenido sube y se desvanece durante la primera pantalla de scroll.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, hero de la portada (`src/components/Hero.astro`) |
| Técnica · coste | JS `requestAnimationFrame` que se detiene con el hero fuera de pantalla (IO); ratón suavizado |
| Sin JS / reducir movimiento | Hero quieto / no se ejecuta |
| Cuándo sí / no | Sí: hero oscuro «tecnológico» con fondo abstracto. No: hero con foto de contenido. Solo con `(pointer: fine)` |
| Estado | ✅ xanatechnologies (publicada; sin memoria de aprobación) |
| Parámetros | `LIFT = 28` (%), halo `RISE = 30` px, contenido ±6px, suavizado 0,06; opacidad `1 - sp * 1.15` |
---

## 4. Cabecera y pie

### `cabecera-condensa` · La barra encoge al bajar

Pasados 160px la barra baja de alto y el logo se reduce; en escritorio pasa a cristal translúcido salvo cuando por debajo hay una superficie oscura.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com (`components/Header.astro`) |
| Técnica · coste | JS scroll (marca `data-scrolled` en `<html>`) + CSS transiciones |
| Sin JS / reducir movimiento | Barra blanca sólida a tamaño completo / cambio instantáneo |
| Cuándo sí / no | Sí: cabecera alta con logo grande. Va `fixed` con separador ([maquetación §9](flujos/maquetacion.md#cabecera)). No: con menú desplegable (móvil, y en clays español hasta 1480px) |
| Estado | ✅ clays (publicada, commit `07f102c` «hace de cristal la barra»). Coincide con la variante 3 de `/pruebas-cabecera` (flotante · cristal oscuro · cristal sobre claro), que sigue en el repo |
| Parámetros | Condensa a >160px y expande a <80px (histéresis); logo `scale(0.72)`; `--duration-base`; sombra `--shadow-sm` solo condensada; `scroll-padding-top` a `--header-h-compact` |

```ts
// clays-of-the-world/src/components/Header.astro
const CONDENSA = 160;
const EXPANDE = 80;
if (!condensada && y > CONDENSA) raiz.setAttribute('data-scrolled', '');
else if (condensada && y < EXPANDE) raiz.removeAttribute('data-scrolled');
// Cristal solo si lo que toca el borde inferior de la barra no es oscuro
const tocaOscura = oscuras.some((s) => {
  const { top, bottom } = s.getBoundingClientRect();
  return top <= linea && bottom > linea;
});
header.toggleAttribute('data-cristal', !tocaOscura);
```

```css
/* mismo fichero, escritorio */
:global(html[data-scrolled]) .header__inner { min-height: var(--header-h-compact); }
:global(html[data-scrolled]) .header__logo { transform: scale(0.72); }
@supports not (backdrop-filter: blur(1px)) {
  .header[data-cristal] { background-color: var(--color-bg); }
}
```

Las superficies oscuras las declara el marcado (`data-surface="dark"` en
`Section` inversa, hero y pie), no el script. A 0,72 el logo queda por debajo
del mínimo del manual y `Logo` no avisa (comentario del componente).

### `cabecera-translucida` · La cabecera se vuelve translúcida al bajar

Arriba del todo es opaca; en cuanto se baja, pasa a translúcida con desenfoque.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC, todas las páginas (`components/nav/SiteHeader.astro`) |
| Técnica · coste | JS scroll (clase `is-scrolled`) + CSS · `sticky`, no cambia de alto |
| Sin JS / reducir movimiento | Opaca / cambio de fondo (color: no se apaga) |
| Cuándo sí / no | Sí: cabecera que no cambia de alto, fondo claro. No: si lo que pasa por debajo son fotos oscuras con el logo encima |
| Estado | ✅ APC (en construcción) |
| Parámetros | Umbral `scrollY > 4`; `color-mix(… 88%, transparent)`; `blur(10px)`; `--dur-base` 280ms; con el menú abierto, siempre sólida |

```css
/* APC-gestion-patrimonial/src/components/nav/SiteHeader.astro */
.header { position: sticky; top: 0; background: var(--c-bg);
  transition: background var(--dur-base) var(--ease-out); }
.header.is-scrolled {
  background: color-mix(in srgb, var(--c-bg) 88%, transparent);
  backdrop-filter: blur(10px);
}
.header.is-open { background: var(--c-bg); backdrop-filter: none; }
```

Mejorable: el listener no pasa por `requestAnimationFrame` y no hay recambio
`@supports not (backdrop-filter…)` como en clays.

### `menu-activo` · El menú marca la sección que se está viendo

El enlace del menú de la sección visible queda marcado (`aria-current`).

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, menú de la portada (`components/Header.astro`) |
| Técnica · coste | JS IO |
| Sin JS / reducir movimiento | Sin marca activa / igual (no mueve nada) |
| Cuándo sí / no | Sí: landing de una página con menú de anclas. No: webs multipágina (fuera de la home no enciende nada) |
| Estado | ✅ clays (publicada) |
| Parámetros | `rootMargin: '-20% 0px -70% 0px'` (activa cuando la parte alta cruza el tercio superior) |

```ts
// clays-of-the-world/src/components/Header.astro
const observer = new IntersectionObserver((entries) => {
  for (const entry of entries) {
    if (!entry.isIntersecting) continue;
    for (const link of links) {
      if (link.hash === `#${entry.target.id}`) link.setAttribute('aria-current', 'true');
      else link.removeAttribute('aria-current');
    }
  }
}, { rootMargin: '-20% 0px -70% 0px' });
```

### `pie-cortina` · El pie se destapa

El contenido sube por encima del pie, que está pegado al fondo de la ventana, y lo deja a la vista al final.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com (todas las páginas con cabecera) · APC (escritorio, ≥ 900px) |
| Técnica · coste | CSS `sticky` + `main` opaco con `z-index`. clays añade un script que solo lo enciende si el pie cabe en la ventana |
| Sin JS / reducir movimiento | clays: pie normal / clays lo **apaga** («una capa deslizándose sobre otra es el efecto de profundidad que esa preferencia pide evitar») · APC: CSS puro, sigue |
| Cuándo sí / no | Sí: pie oscuro tras una banda de cierre. No: pie más alto que la ventana (su parte alta no se vería nunca) |
| Estado | ✅ clays (publicada) · ✅ APC |
| Parámetros | clays: `pie.offsetHeight <= innerHeight`, re-medido con `ResizeObserver` · APC: corte fijo a 900px |

Descripción y rutas en [maquetación §9](flujos/maquetacion.md#pie). Lo que
enciende la de clays:

```ts
// clays-of-the-world/src/layouts/Base.astro
const revisarCortina = () => {
  const cabe = pie.offsetHeight <= window.innerHeight;
  document.documentElement.toggleAttribute('data-footer-reveal', cabe && !quietud.matches);
};
new ResizeObserver(revisarCortina).observe(pie);
```

```css
/* clays-of-the-world/src/styles/base.css */
html[data-footer-reveal] main { position: relative; z-index: 1; background-color: var(--color-bg); }
html[data-footer-reveal] .footer { position: sticky; bottom: 0; }
```

Trampas: `main` pasa a contexto de apilado (capas con `z-index` negativo
necesitan un ancestro con `isolation: isolate`); el ancla al pie necesita
cancelar `scroll-margin-top` (clays `.footer-anchor`). APC pone
`overflow: hidden` en `main`, que rompería cualquier `view()` dentro
([trampa](trampas-y-lecciones.md#un-reveal-ligado-al-scroll-no-anima)).

### `cabecera-pastilla-contraste` · Pastilla transparente sobre el hero que se vuelve sólida

Una pastilla de cristal flotante con texto blanco sobre el hero; al salir de él pasa a blanca con texto oscuro, y vuelve a transparente mientras pasa por encima de un vídeo.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), `src/components/SiteHeader.astro` |
| Técnica · coste | JS scroll con `requestAnimationFrame` (clase `is-scrolled`) + CSS con variables `--hdr-*` |
| Sin JS / reducir movimiento | **Transparente con texto blanco en toda la página: ilegible sobre claro** (deuda) / cambio instantáneo |
| Cuándo sí / no | Sí: cabecera sin logotipo encima de la foto ([maquetación §9](flujos/maquetacion.md#cabecera)). No: con el logo del cliente sobre foto |
| Estado | 🟡 festival-viento (sin publicar) |
| Parámetros | Límite `hero.offsetHeight - cabecera.offsetHeight - 8`; `blur(16px)`; transiciones 0,3s; fondo sólido `rgba(255, 255, 255, 0.92)` |

```js
// festival-viento/src/components/SiteHeader.astro
const limite = (hero?.offsetHeight ?? window.innerHeight) - cabecera.offsetHeight - 8;
const pastHero = window.scrollY > limite;
cabecera.classList.toggle('is-scrolled', pastHero && !sobreVideo);
```

Para reutilizarlo: el estado sin JS tiene que ser el **sólido**, y la
transparencia, lo que añade el script. Su scrollspy (`rootMargin: '-20% 0px
-60% 0px'`) no pone `aria-current`: usar el de `menu-activo`.

### `cabecera-pildora-cristal` · Aparece el cristal detrás del menú al bajar

Arriba del todo la píldora de navegación es transparente; al bajar 8px se funde un fondo de cristal detrás.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), todas las páginas (`components/nav/Header.astro`) |
| Técnica · coste | JS scroll (`data-scrolled`) + CSS: el cristal va en `::before` |
| Sin JS / reducir movimiento | Píldora transparente / fundido (el global lo deja en 0,01ms) |
| Cuándo sí / no | Sí: cabecera `sticky` de alto fijo sobre fondos claros. No: sobre hero de foto con logo encima |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria) |
| Parámetros | Umbral `scrollY > 8`; `rgba(255, 255, 255, 0.70)`, `blur(16px) saturate(140%)`; radio 12px; `--dur-base` 250ms; también con el menú móvil abierto |

```css
/* xanasystem-web/src/components/nav/Header.astro */
.header__inner::before {
  content: ''; position: absolute; inset: 0; border-radius: 12px; z-index: -1;
  opacity: 0;
  transition: opacity var(--dur-base) var(--ease-out);
}
[data-header][data-scrolled='true'] .header__inner::before,
[data-header][data-mobile-open='true'] .header__inner::before {
  background: rgba(255, 255, 255, 0.70);
  -webkit-backdrop-filter: blur(16px) saturate(140%);
  backdrop-filter: blur(16px) saturate(140%);
  box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.10);
  opacity: 1;
}
```

**Por qué en `::before`**: un `backdrop-filter` en la propia píldora rompe el
`position: fixed` del megamenú que lleva dentro (comentario del componente).
Animar la opacidad de una capa es mejor que transicionar `background`.

---

## 5. Hover e interacción

### `hover-zoom-foto` · La foto se acerca al pasar el cursor

La imagen crece un poco dentro de su recorte; el marco no cambia.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com: tarjetas de productos (`Products`), rejilla del visor (`Lightbox`) · APC: artículos de «Conocimiento» en `/` y `/conocimiento/` |
| Técnica · coste | CSS |
| Sin JS / reducir movimiento | Igual / clays: instantáneo (duración a 0) · APC: 0,01ms global |
| Cuándo sí / no | Sí: tarjetas enlazadas con foto. No: fotos que no son enlace; dentro de algo que ya se mueve (clays lo quita en la rueda: «dos crecimientos a la vez se pelean») |
| Estado | ✅ clays (publicada) · ✅ APC («Hover sobrio: solo mueve la flecha y hace zoom a la imagen», memoria `apc-estado-desarrollo`) |
| Parámetros | clays: `scale(1.04)`, `--duration-slow` 450ms, `--ease-out-soft` · APC: `scale(1.02)`, `0.5s` (sin token) |

```css
/* clays-of-the-world/src/components/sections/Products.astro */
@media (hover: hover) {
  .product :global(img) { transition: transform var(--duration-slow) var(--ease-out-soft); }
  .product:hover :global(img) { transform: scale(1.04); }
}
```

El velo de contraste va en la tarjeta, no en la imagen, para que no se acerque
con ella y las medidas de legibilidad sigan valiendo (comentario de clays).

### `hover-flecha-desliza` · La flecha avanza

En un enlace o tarjeta, solo la flecha se desplaza unos píxeles; el color del texto no cambia.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC: «Leer artículo →» de Conocimiento, `.link-arrow` de «Cada patrimonio tiene su momento» |
| Técnica · coste | CSS |
| Sin JS / reducir movimiento | Igual / sin desplazamiento |
| Cuándo sí / no | Sí: marcas sobrias; enlaces de texto con flecha. No: botones que ya cambian de fondo |
| Estado | ✅ APC: Belén pidió que título y enlace **no** cambien de color (memoria `apc-estado-desarrollo`) |
| Parámetros | `translateX(3px)`, `--dur-fast` 160ms |

```css
/* APC-gestion-patrimonial/src/styles/global.css */
.link-arrow::after { content: "→"; transition: transform var(--dur-fast) var(--ease-out); }
.link-arrow:hover::after { transform: translateX(3px); }
```

Incoherencia: la regla general `.link-arrow:hover` sí cambia a terracota; los
componentes que no deben cambiar de color lo anulan uno a uno
(`home/Momento.astro`).

### `hover-flecha-relevo` · La flecha del botón sale y vuelve a entrar

Al pasar por el botón, la flecha diagonal se va por su esquina y reaparece por la contraria.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, botones con flecha (`components/Button.astro`, prop `arrow`) |
| Técnica · coste | CSS `@keyframes` |
| Sin JS / reducir movimiento | Igual / duración a 0, no se mueve |
| Cuándo sí / no | Sí: CTA con flecha diagonal ↗. No: flecha horizontal (→ `hover-flecha-desliza`) |
| Estado | ✅ clays (publicada) |
| Parámetros | `--duration-base` 250ms; salto al 115 % en el fotograma 50 % |

```css
/* clays-of-the-world/src/components/Button.astro */
.btn__arrows { position: relative; display: inline-grid; overflow: hidden; }
.btn:hover .btn__arrows :global(.arrow) { animation: btn-arrow var(--duration-base) var(--ease-out-soft); }
@keyframes btn-arrow {
  0%    { transform: translate(0, 0); }
  49.9% { transform: translate(115%, -115%); }
  50%   { transform: translate(-115%, 115%); }
  100%  { transform: translate(0, 0); }
}
```

«Un solo trazo, y esto es la clave»: con dos flechas, a media transición se
ven las dos (comentario del componente).

### `faq-mas-menos` · El «+» se convierte en «−»

Al abrir una pregunta, el trazo vertical del icono se pliega.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC, `/gestion-patrimonial/#preguntas-frecuentes` (`gestion-patrimonial/Faq.astro`) |
| Técnica · coste | CSS sobre `<details>` nativo |
| Sin JS / reducir movimiento | Funciona sin JS / cambio instantáneo |
| Cuándo sí / no | Sí: toda FAQ con `<details>` ([maquetación §8](flujos/maquetacion.md#8-accesibilidad)) |
| Estado | ✅ APC |
| Parámetros | `scaleY(0)`, `--dur-fast` 160ms; la pregunta pasa a color de acento al pasar |

```css
/* APC-gestion-patrimonial/src/components/sections/gestion-patrimonial/Faq.astro */
.faq__icon::before, .faq__icon::after {
  content: ""; position: absolute; background: var(--c-accent);
  transition: transform var(--dur-fast) var(--ease-out);
}
.faq__icon::after { left: 50%; top: 0; width: 1.5px; height: 100%; transform: translateX(-50%); }
.faq__item[open] .faq__icon::after { transform: translateX(-50%) scaleY(0); }
```

El contenido del `<details>` se abre de golpe: no hay animación de altura.

### `pulso-play` · Halo que late alrededor del botón de reproducir

Dos aros nacen del borde del disco de play, crecen y se apagan, alternados, mientras el vídeo está parado.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, sección 04 (`sections/Showcase.astro`, `.showcase__play`) |
| Técnica · coste | CSS `@keyframes` en `::before`/`::after`, infinito |
| Sin JS / reducir movimiento | Late igual / **no existe** (dentro de `no-preference`) |
| Cuándo sí / no | Sí: invitar a reproducir una pieza sin controles nativos. No: más de un elemento que late en la misma pantalla |
| Estado | ✅ clays (publicada) |
| Parámetros | `--duration-pulse: 2400ms`; aro de `scale(1)` a `1.7` y opacidad 0,7 → 0; segundo aro a media vuelta; se para con `[data-playing]`, hover y foco; hover del disco `scale(1.16)` |

```css
/* clays-of-the-world/src/components/sections/Showcase.astro */
@media (prefers-reduced-motion: no-preference) {
  .showcase__play::before, .showcase__play::after {
    content: ''; position: absolute; inset: 0;
    border: 1px solid var(--glass-border-hover); border-radius: inherit;
    opacity: 0; pointer-events: none;
    animation: play-halo var(--duration-pulse) var(--ease-out) infinite;
  }
  .showcase__play::after { animation-delay: calc(var(--duration-pulse) / -2); }
  .showcase__play[data-playing]::before, .showcase__play[data-playing]::after,
  button.showcase__play:hover::before, button.showcase__play:hover::after { animation: none; }
}
@keyframes play-halo { 0% { transform: scale(1); opacity: 0.7; } 100% { transform: scale(1.7); opacity: 0; } }
```

### `pastilla-sigue-cursor` · La etiqueta «View» acompaña al ratón

Dentro de cada foto de la galería, una pastilla de cristal sigue al puntero y dice que la foto se abre.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, galería de la portada (`sections/Gallery.astro`) |
| Técnica · coste | JS `pointermove` agrupado en `requestAnimationFrame` + CSS `translate` |
| Sin JS / reducir movimiento | Pastilla fija en su esquina al pasar / fija («seguir al cursor es movimiento de verdad») |
| Cuándo sí / no | Sí: fotos que abren un visor y no tienen otra pista. Solo con `(hover: hover) and (pointer: fine)`. No: móvil (se oculta < 900px) |
| Estado | ✅ clays (publicada; `CHECKLIST.md` §galería) |
| Parámetros | `translate` en `--duration-fast`; aparición `scale` con `--ease-pop`; pastilla apoyada en el cursor por su borde inferior |

```css
/* clays-of-the-world/src/components/sections/Gallery.astro */
.gallery__marco[data-pastilla-sigue] .carta__abrir {
  inset: 0 auto auto 0;
  translate: calc(var(--x, 50%) - 50%) calc(var(--y, 50%) - 100%);
  transition:
    opacity var(--duration-base) var(--ease-out),
    scale var(--duration-base) var(--ease-pop),
    translate var(--duration-fast) var(--ease-out);
}
.carta:hover .carta__abrir, .carta__enlace:focus-visible .carta__abrir { opacity: 1; scale: 1; }
@media (hover: none) and (min-width: 900px) { .carta__abrir { opacity: 1; scale: 1; } }
```

Variante de `hover-flecha-relevo` en xanatechnologies.com (`src/styles/global.css`,
`.cta`): dos trazos, uno sale y otro entra, en 0,55s con
`cubic-bezier(0.16, 1, 0.3, 1)`; con reducir movimiento solo se desplaza
`translate(2px, -2px)`. clays advierte que con dos trazos a media transición se
ven los dos: a 17px se nota, en botones grandes no tanto.

### `hover-borde-degradado` · Aparece un borde de degradado y la tarjeta sube

Al pasar el cursor, la tarjeta sube 2px, gana sombra y se enciende un borde fino en degradado de marca.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada, tarjetas de beneficios (`sections/BenefitCards.astro`); también en `/dam` y `/api` |
| Técnica · coste | CSS: `::before` con máscara `xor`/`exclude`, se funde su opacidad |
| Sin JS / reducir movimiento | Igual / cambio instantáneo |
| Cuándo sí / no | Sí: marca con degradado propio y tarjetas sobre fondo claro. No: marcas sobrias (APC pidió hover sin cambios de color) |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria sobre el hover) |
| Parámetros | `translateY(-2px)`, `--dur-fast` 150ms, grosor `--grad-border-width`, degradado `--grad-cyan-pink` |

```css
/* xanasystem-web/src/components/sections/BenefitCards.astro */
.benefit-card::before {
  content: ''; position: absolute; inset: 0;
  padding: var(--grad-border-width);
  border-radius: inherit;
  background: var(--grad-cyan-pink);
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
          mask-composite: exclude;
  opacity: 0;
  transition: opacity var(--dur-fast) var(--ease-out);
}
.benefit-card:hover { transform: translateY(-2px); box-shadow: var(--sh-2); border-color: transparent; }
.benefit-card:hover::before { opacity: 1; }
```

El degradado no se transiciona: se funde la capa. Texto con degradado al
pasar → [trampa](trampas-y-lecciones.md#al-animar-un-texto-con-degradado-el-color-cambia-o-el-paso-es-brusco).

### `hover-tarjeta-sube` · La tarjeta se eleva

La tarjeta sube unos píxeles y gana sombra.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local): tarjetas de características y del carrusel (−4px), patrocinadores (−2px), botones (−2px) · grupomerhi: píldoras de empresas (−2px) · xanatechnologies.com/v2/: tarjetas de Industry (−4px) |
| Técnica · coste | CSS |
| Sin JS / reducir movimiento | Igual / festival y xanatechnologies: instantáneo · grupomerhi: no lo contempla |
| Cuándo sí / no | Sí: tarjetas enlazadas sin foto. No: **a la vez que `hover-zoom-foto`** («Un elemento se eleva o hace zoom», festival `docs/design.md`); botones de xanasystem-web («el botón no se mueve», solo sombra, `ui/Button.astro`) |
| Estado | 🟡 festival-viento · 🟡 grupomerhi (fuera de la guía) · 🟡 xanatechnologies v2 |
| Parámetros | festival: −4px en `--t-media` 0,3s; grupomerhi: −2px y fondo 0,2 → 0,32 en 0,2s |

```css
/* grupomerhi/css/styles.css */
.company { backdrop-filter: blur(2px); transition: background-color 0.2s ease, transform 0.2s ease; }
.company:hover,
.company:focus-visible {
  background: var(--company-bg-hover);      /* rgba(255, 255, 255, 0.32) */
  transform: translateY(-2px);
  outline: none;
}
```

Trampas: grupomerhi quita el `outline` y deja como único indicador de foco un
cambio de opacidad del 0,2 al 0,32 sobre vídeo ([maquetación §8](flujos/maquetacion.md#8-accesibilidad)),
y le falta `-webkit-backdrop-filter`. Muchos hovers de festival-viento y
magnaceramica no tienen su `:focus-visible`.

### `hover-logos-color` · Los logos de clientes pasan de gris a color

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada (`sections/TrustBlock.astro`) |
| Técnica · coste | CSS (`filter: grayscale` y opacidad) |
| Sin JS / reducir movimiento | Igual / cambio de color (no es movimiento) |
| Cuándo sí / no | Sí: franja de logos de clientes de colores muy distintos. No: si el cliente exige sus logos siempre en color |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria) |
| Parámetros | `grayscale(1)` y opacidad 0,7 → color, 0,3s |

### `desplegable-cae` · El panel del desplegable cae y el chevron gira

Al abrir un filtro, acordeón o menú, el panel aparece bajando unos píxeles y el chevron gira 180°.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), filtros del programa (`Programa.astro`) · xanasystem-web (local): acordeones de `/pim`, `/dam`, `/ia`, `/catalogos`; megamenú; FAQ de portada (solo el chevron) |
| Técnica · coste | CSS `@keyframes` al pasar de `display: none` a visible |
| Sin JS / reducir movimiento | festival: el panel depende de JS; xanasystem-web: `<details>` o `:focus-within` / sin movimiento |
| Cuándo sí / no | Sí: cualquier desplegable. No: animar la altura del contenido |
| Estado | 🟡 festival-viento · 🟡 xanasystem-web |
| Parámetros | festival: `translateY(-5px)` en 0,13s, chevron 0,2s · xanasystem-web: −4px en 250ms, megamenú 4px en 150ms, chevron 150–250ms |

```css
/* festival-viento/src/components/Programa.astro */
.filtro__chevron { transition: transform 0.2s ease; }
.filtro[data-abierto] .filtro__chevron { transform: rotate(180deg); }
.filtro[data-abierto] .filtro__panel { display: block; animation: filtro-in 0.13s ease; }
@keyframes filtro-in { from { opacity: 0; transform: translateY(-5px); } to { opacity: 1; transform: translateY(0); } }
```

Festival escribe 0,1/0,13/0,15s a pelo, sin sus tokens `--t-*`.

### `pestanas-fundido` · Las pestañas cambian con fundido (y pueden avanzar solas)

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local): `/styleguide` y `/demo-tabs` (`sections/TabbedSection.astro`, sin avance automático); `/pim`, `/dam`, `/api`, `/catalogos`, `/ia` (`PimEcosystem.astro`, avanza sola); `/empresa` (`EmpresaDiferenciales`, avanza sola) |
| Técnica · coste | CSS (pestañas con radios, funcionan sin JS) + JS solo para centrar la pestaña o avanzar |
| Sin JS / reducir movimiento | Pestañas usables / sin fundido ni avance automático |
| Cuándo sí / no | Sí: 3–5 bloques paralelos del mismo peso. Avance automático solo si se para con hover, foco y pestaña oculta |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria) |
| Parámetros | Panel `translateY(4px)` en 320ms (`TabbedSection`), fundido 420ms; avance `DELAY = 4500`ms (`PimEcosystem`), 5000ms (`EmpresaDiferenciales`) |

```css
/* xanasystem-web/src/components/sections/TabbedSection.astro */
@media (prefers-reduced-motion: no-preference) {
  .tabsec__panel { animation: tabsec-fade 320ms var(--ease-out); }
  @keyframes tabsec-fade { from { opacity: 0; transform: translateY(4px); } to { opacity: 1; transform: none; } }
}
```

### `hover-poster-endereza` · El póster inclinado se endereza

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), llamada final (`LlamadaFinal.astro`, `.cta__poster-img`) |
| Técnica · coste | CSS |
| Sin JS / reducir movimiento | Igual / instantáneo |
| Cuándo sí / no | Sí: tono festivo o de evento. No: marcas corporativas o editoriales |
| Estado | 🟡 festival-viento |
| Parámetros | `rotate(-2.5deg)` → `rotate(0deg)`, 0,3s |

---

## 6. Imagen y vídeo

### `fotos-se-turnan` · Varias fotos se relevan con barras de avance

En una banda, tres fotos se turnan con fundido cada cinco segundos y unas barras sobre cristal marcan por cuál va.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, banda 03 (`sections/Band.astro`) |
| Técnica · coste | CSS (fundido + `@keyframes` de la barra) + JS IO; **el reloj es la animación de la barra** (`animationend`), no un `setInterval` |
| Sin JS / reducir movimiento | Primera foto fija (`data-paused` viene en el marcado) / no arranca y las barras no se pintan |
| Cuándo sí / no | Sí: 2–4 fotos del mismo asunto en una columna. No: fotos que hay que comparar; contenido distinto en cada foto |
| Estado | ✅ clays (publicada). **Sin botón de pausa por petición expresa**: queda fuera de la WCAG 2.2.2 a sabiendas (comentario del script) |
| Parámetros | `--band-interval: 5s` (no es token: «es el tiempo de mirar una foto»); fundido `--duration-slower` 700ms; barras de 2,5rem × 3px; se para fuera de pantalla (`threshold: 0.25`) |

```css
/* clays-of-the-world/src/components/sections/Band.astro */
.band__slide { position: absolute; inset: 0; opacity: 0;
  transition: opacity var(--duration-slower) var(--ease-out-soft); }
.band__slide.is-current { opacity: 1; }
.band__bar-fill { transform: scaleX(0); transform-origin: left center; }
.band__bar.is-current .band__bar-fill { animation: band-bar var(--band-interval) linear forwards; }
.band__bar.is-done .band__bar-fill { transform: scaleX(1); }
.band__media[data-paused] .band__bar-fill { animation-play-state: paused; }
@keyframes band-bar { from { transform: scaleX(0); } to { transform: scaleX(1); } }
```

```ts
// mismo fichero
banda.addEventListener('animationend', (evento) => {
  if (!(evento.target as HTMLElement).matches('[data-fill]')) return; // el parallax también burbujea
  actual = (actual + 1) % fotos.length;
  pintar();
});
```

### `galeria-rueda` · Galería en rueda que se arrastra

Cinco fotos verticales en abanico (la central mayor); se arrastran, la del centro encoge mientras crece la vecina, y al soltar encaja la más cercana. No tiene principio ni final.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com, galería de la portada (`sections/Gallery.astro`); comparativa de maquetas en `/pruebas-galeria` (local) |
| Técnica · coste | JS propio (Pointer Events + `requestAnimationFrame`, ~250 líneas) + CSS con variables `--w`, `--h`, `--x`, `--op` |
| Sin JS / reducir movimiento | Abanico quieto maquetado con flex / abanico quieto |
| Cuándo sí / no | Sí: 5 fotos (número impar y fijo) que se recorren, no se comparan. No: colecciones largas (→ visor, [multimedia](modulos/multimedia.md)) |
| Estado | ✅ clays (publicada, commit `07f102c` «convierte la galería en rueda») — ver incoherencia |
| Parámetros | Geometría desde el ancho de la central: 26,7 % del ancho (máx. 480px) en escritorio, 56 % (máx. 320px) en móvil; alturas 1,239 · 1,068 · 0,894; encaje 420ms cúbica de salida; umbral de arrastre 6px; empujón > 0,4 px/ms pasa de foto; se apaga a partir de 2,2 posiciones |

```css
/* clays-of-the-world/src/components/sections/Gallery.astro */
.gallery__marco[data-rueda-activa] { position: relative; height: var(--alto-rueda);
  overflow: hidden; cursor: grab; touch-action: pan-y; }
.gallery__marco[data-rueda-activa] .carta {
  position: absolute; left: 50%; bottom: 0; width: var(--w);
  translate: calc(var(--x) - 50%) 0; /* translate, NO transform: no pisa la aparición */
}
.gallery__marco[data-rueda-activa] .carta__figura { opacity: var(--op, 1); }
```

```ts
// mismo fichero: el arrastre mueve el progreso; el encaje lo lleva al entero
progreso = progresoInicial - avance / (RUEDA.paso[0] * anchoCentro);
const impulso = Math.abs(velocidad) > 0.4 ? -Math.sign(velocidad) : 0;
encajar(Math.round(progreso + impulso * 0.5));
```

- **No capturar el puntero al pulsar**, solo al superar el umbral, o el clic
  deja de abrir el visor (commit `c67dfc2`).
- Con teclado, la foto que recibe el foco se trae al centro (`focusin`).
- **Incoherencia**: `CHECKLIST.md` de clays dice «El abanico no se mueve en
  escritorio… se descartaron las dos [flechas y arrastre]», pero el código
  publicado activa la rueda en todos los anchos. Confirmar con Belén cuál vale
  antes de ofrecerla.

### `zoom-lento-fondo` · La foto de fondo respira

La foto de fondo de una banda oscura se acerca y se aleja muy despacio, en bucle, a la vez que hace parallax.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC: «Cada patrimonio tiene su momento» en `/` (`home/Momento.astro`), servicios de `/gestion-patrimonial/`, «Por qué nace APC» en `/sobre-apc/` (`Origen`), `/contacto/` (`Principal`) |
| Técnica · coste | CSS `@keyframes` sobre una **variable registrada con `@property`**, multiplicada por la escala del parallax |
| Sin JS / reducir movimiento | Zoom sigue, sin parallax / `animation: none` |
| Compatibilidad | `@property`: comprobar en caniuse. Sin soporte la variable no interpola y no hay zoom |
| Cuándo sí / no | Sí: banda oscura con foto de fondo y velo, texto encima. No: fotos de contenido; varias bandas seguidas con el mismo efecto |
| Estado | ✅ APC (en construcción) |
| Parámetros | De 1 a 1,18 en 18s, `ease-in-out infinite alternate` |

```css
/* APC-gestion-patrimonial/src/components/sections/gestion-patrimonial/Servicios.astro */
@property --serv-zoom { syntax: "<number>"; inherits: false; initial-value: 1; }
@keyframes serv-zoom { from { --serv-zoom: 1; } to { --serv-zoom: 1.18; } }
.serv__bg {
  transform: translate3d(0, var(--py, 0px), 0) scale(calc(var(--pz, 1) * var(--serv-zoom)));
  animation: serv-zoom 18s ease-in-out infinite alternate;
}
@media (prefers-reduced-motion: reduce) { .serv__bg { animation: none; } }
```

Es un bucle de más de cinco segundos sin pausa (WCAG 2.2.2): decorativo y
detrás de un velo; se apaga con reducir movimiento. Duplicado en cuatro
componentes con cuatro nombres de variable.

### `video-en-pantalla` · Vídeo que arranca al entrar y se para al salir

El vídeo de fondo o de sección arranca solo cuando se ve y se pausa fuera de pantalla.

| Dato | Valor |
| --- | --- |
| Dónde verla | claysoftheworld.com: hero y sección 04 (`src/scripts/video-autoplay.ts`) |
| Técnica · coste | JS IO |
| Sin JS / reducir movimiento | Póster / póster |
| Estado | ✅ clays (publicada) |

Todo el detalle está en [vídeos §3–§4](flujos/videos.md#4-arranque-y-pausa--claude).
Se ofrece junto con `pulso-play` si el vídeo es de contenido.

### `hero-foto-a-video` · La foto del hero se funde en vídeo

Al cargar se ve la foto (primer fotograma); cuando el vídeo empieza a reproducirse, aparece con fundido y la foto se apaga.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC, hero de `/` (`home/Hero.astro`) |
| Técnica · coste | JS (tras `load`) + CSS fundido; solo ≥ 900px |
| Sin JS / reducir movimiento | Foto / foto (`.hero__video { display: none }`) |
| Estado | ✅ APC (en construcción) |
| Parámetros | Fundido `--dur-reveal` 620ms; parallax 0,05 y zoom 1,08 en foto y vídeo |

Código en [vídeos §5](flujos/videos.md#5-hero-con-vídeo-sin-perjudicar-el-lcp--claude).

Variante en xanatechnologies.com (`Contact.astro`): vídeo con `autoplay` en el
HTML que se pausa fuera de pantalla con IO (`threshold: 0.1`) y sin `autoplay`
con reducir movimiento; está en `public/video/` (deuda, [vídeos §6](flujos/videos.md#6-reglas-y-trampas)).

### `ken-burns` · La foto hace zoom y se desplaza sin parar

La foto grande de cada marca se acerca y se desplaza lentamente, ida y vuelta, en bucle.

| Dato | Valor |
| --- | --- |
| Dónde verla | magnaceramica (local), bloques de marca Baldocer y Alaplana (`src/components/BrandBlock.astro`) |
| Técnica · coste | CSS `@keyframes` infinito sobre la `img` recortada |
| Sin JS / reducir movimiento | Igual / **mal resuelto**: el global pone 0,01ms sin `animation-iteration-count: 1` y el bucle sigue (comprobar en navegador) |
| Cuándo sí / no | Sí, con cambios: foto de ambiente sin texto encima. Para un fondo con texto se prefiere `zoom-lento-fondo` (más lento, compone con parallax). No: más de una en pantalla sincronizadas |
| Estado | 🟡 magnaceramica (publicación sin verificar; sin decisión escrita) |
| Parámetros | 10s `linear infinite alternate`, `scale(1)` → `scale(1.18) translate3d(-3%, -2%, 0)`; segundo bloque con `animation-delay: -5s` |

```css
/* magnaceramica/src/components/BrandBlock.astro */
.brand__main { overflow: hidden; }
.brand__main img { width: 100%; height: 100%; object-fit: cover;
  animation: brand-kenburns 10s linear infinite alternate; will-change: transform; }
.brand--panel-left .brand__main img { animation-delay: -5s; }
@keyframes brand-kenburns {
  from { transform: scale(1) translate3d(0, 0, 0); }
  to   { transform: scale(1.18) translate3d(-3%, -2%, 0); }
}
```

Para reutilizarlo: `@media (prefers-reduced-motion: reduce) { … animation: none }`
en el componente (como APC) y sin `will-change` permanente.

### `carrusel-snap-arrastre` · Fila de fotos con snap, arrastre y puntos

Fila de fotos que llega al borde derecho de la ventana, se desliza con encaje, se arrastra con el ratón y marca la posición con puntos que se alargan.

| Dato | Valor |
| --- | --- |
| Dónde verla | magnaceramica (local), bajo cada marca (`BrandBlock.astro`); abre un visor con fundido |
| Técnica · coste | CSS `scroll-snap` + JS propio (~70 líneas): puntos por posición de scroll, arrastre con Pointer Events |
| Sin JS / reducir movimiento | Deslizamiento nativo con snap; los puntos no hacen nada / transiciones a 0,01ms, pero `scrollTo({ behavior: 'smooth' })` sigue suave |
| Cuándo sí / no | Sí: muchas fotos de catálogo del mismo formato. No: galería con visor accesible (→ [multimedia](modulos/multimedia.md), la de clays) |
| Estado | 🟡 magnaceramica |
| Parámetros | Ítem `clamp(260px, 30vw, 450px)`, 4/3; punto 6px → 22px en 0,35s; umbral de arrastre 3px; zoom de la foto al pasar `scale(1.05)` 0,6s; visor: fundido 0,3s e imagen `scale(0.97 → 1)` |

```css
/* magnaceramica/src/components/BrandBlock.astro */
.carousel__track { display: flex; gap: var(--carousel-gap); overflow-x: auto; scroll-snap-type: x mandatory;
  scroll-behavior: smooth; scrollbar-width: none; cursor: grab; margin-right: calc(50% - 50vw); }
.carousel__track.is-dragging { cursor: grabbing; scroll-behavior: auto; scroll-snap-type: none; user-select: none; }
.carousel__item { flex: 0 0 clamp(260px, 30vw, 450px); aspect-ratio: 4 / 3; scroll-snap-align: start; overflow: hidden; }
.carousel__dot { width: 6px; height: 6px; border-radius: 999px; transition: width 0.35s var(--ease), background 0.35s var(--ease); }
.carousel__dot.is-active { width: 22px; background: rgba(144, 83, 53, 0.6); }
```

**Antes de reutilizarlo**: arrastre solo con `pointerType === 'mouse'` (en
táctil anula la inercia nativa); puntos con `type="button"`, `aria-current`
y área de toque suficiente; fotos del carrusel enfocables; el visor no mueve
ni atrapa el foco; animar la anchura del punto con `scale`; `scrollTo` con
`behavior` según reducir movimiento; selector `~` y no `+`
([trampa](trampas-y-lecciones.md#un-selector--funciona-en-el-build-y-no-en-dev)).

### `carrusel-infinito` · Carrusel que avanza solo y no tiene final

Fila de tarjetas que avanza sola cada tres segundos, con flechas y arrastre; al llegar al final vuelve al principio sin que se note.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), «Ven a vivirlo» (`src/components/VenAVivirlo.astro`) · variante de logos en xanasystem-web (local), portada (`TrustBlock.astro`) |
| Técnica · coste | `scroll-snap` + JS propio (~125 líneas): clones al principio y al final, salto invisible en `scrollend` |
| Sin JS / reducir movimiento | Carrusel nativo sin clones ni avance (las flechas no hacen nada) / sin avance automático; las flechas siguen con `smooth` |
| Cuándo sí / no | Solo si se corrigen las trampas de abajo. No: contenido que hay que leer tarjeta a tarjeta |
| Estado | 🟡 festival-viento · 🟡 xanasystem-web |
| Parámetros | festival: `setInterval` 3000ms; pausa con `mouseenter`/`touchstart`/`pointerdown`; arrastre solo ratón, umbral 5px · xanasystem-web: `DUR = 900`, `PAUSE = 4000`, avanza 6/4/3 logos según ancho, se para con hover o foco |

```js
// festival-viento/src/components/VenAVivirlo.astro
carrusel.scrollLeft = paso() * n;
const corregir = () => {
  if (saltando) return;
  const p = paso(); const total = p * n; const sl = carrusel.scrollLeft;
  if (sl < total) { saltando = true; carrusel.style.scrollSnapType = 'none'; carrusel.scrollLeft = sl + total;
    requestAnimationFrame(() => { carrusel.style.scrollSnapType = ''; saltando = false; }); }
  else if (sl >= total * 2) { saltando = true; carrusel.style.scrollSnapType = 'none'; carrusel.scrollLeft = sl - total;
    requestAnimationFrame(() => { carrusel.style.scrollSnapType = ''; saltando = false; }); }
};
carrusel.addEventListener('scrollend', corregir);
```

Trampas (festival): sin botón de pausa (WCAG 2.2.2); no se para fuera de
pantalla ni con el foco; los clones llevan `aria-hidden` pero sus enlaces
siguen siendo enfocables (añadir `inert`); `scrollend` no existe en Safari
antiguo (comprobar); `role="region"` sobre el `<ul>`.

### `fotos-flotan` · Fotos del collage que suben y bajan

Dos fotos giradas del collage flotan unos píxeles arriba y abajo, desfasadas.

| Dato | Valor |
| --- | --- |
| Dónde verla | festival-viento (local), sorteo (`src/components/Sorteo.astro`) |
| Técnica · coste | CSS `@keyframes` infinito |
| Sin JS / reducir movimiento | Igual / quietas, conservando el giro |
| Cuándo sí / no | Sí: tono lúdico de evento. No: marcas serenas o corporativas |
| Estado | 🟡 festival-viento. Contradice su propio `docs/design.md`: «Nada de movimiento decorativo en bucle» |
| Parámetros | 5s `ease-in-out infinite alternate`, `translateY(-10px)`; segunda foto `animation-delay: -2.5s` |

```css
/* festival-viento/src/components/Sorteo.astro */
.sorteo__foto { animation: sorteo-flotar 5s ease-in-out infinite alternate; }
.sorteo__foto--2 { --rot: 5deg; animation-delay: -2.5s; }
@keyframes sorteo-flotar {
  0%   { transform: translateY(0)     rotate(var(--rot)); }
  100% { transform: translateY(-10px) rotate(var(--rot)); }
}
@media (prefers-reduced-motion: reduce) {
  .sorteo__foto { animation: none; transform: rotate(var(--rot)); }
}
```

### `reel-escenas-gsap` · Reel de escenas del producto en el hero

Una caja de cristal en el hero donde se suceden en bucle escenas del producto: piezas que entran escalonadas, un cursor que hace clic, una ficha que entra con zoom.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), hero de la portada (`src/components/ui/HeroReel.astro`); `/pruebas-reel` |
| Técnica · coste | **GSAP** (`gsap.timeline({ repeat: -1 })`, dependencia `gsap ^3.15.0`) + `setTimeout` y transiciones CSS; escalado en móvil con `ResizeObserver` |
| Sin JS / reducir movimiento | **Caja de cristal vacía** (las escenas parten de `opacity: 0` y el `<noscript>` no las rescata) / escena 1 estática |
| Cuándo sí / no | Solo con el módulo de movimiento avanzado, en webs de producto de software. No: servicios, editorial, marcas con fotografía |
| Estado | 🟡 xanasystem-web (en construcción; ninguna memoria aprueba el reel; choca con `check-seleccion-color` e `imagenes-reales-producto`) |
| Parámetros | Entrada `y: 46, scale: 0.95`, 0,55s, `stagger: 0.16`, `back.out(1.25)`; espera 2,6s; salida `y: -160`, 0,75s, `power2.in`; escenas `FADE 0.7`, `TRANS 0.5`, `SCENE_DUR 4.5` |

Código base en [movimiento avanzado §4](modulos/movimiento-avanzado.md#4-gsap-para-una-pieza).
El arranque de la línea de tiempo:

```js
// xanasystem-web/src/components/ui/HeroReel.astro
var master = gsap.timeline({ repeat: -1, repeatDelay: 0 });
master
  .set(scenes, { autoAlpha: 0, scale: 1 })
  .set(s1, { autoAlpha: 1 })
  .set(s1els, { autoAlpha: 0, y: 46, scale: 0.95 })
  .to(s1els, { autoAlpha: 1, y: 0, scale: 1, duration: 0.55, stagger: 0.16, ease: 'back.out(1.25)' })
  .to({}, { duration: 2.6 })
  .to(s1els, { y: -160, autoAlpha: 0, duration: 0.75, stagger: 0.035, ease: 'power2.in' })
```

Para reutilizarlo: la escena 1 visible por defecto (sin JS) y oculta solo
cuando GSAP arranca; en móvil, sin entradas y salidas («que producía saltos»).

---

## 7. Texto, cifras y mockups

### `frases-que-rotan` · Una línea que va cambiando

Bajo «Quizá el tuyo sea», seis frases se relevan en la misma línea: entra una, se sostiene y sale.

| Dato | Valor |
| --- | --- |
| Dónde verla | APC, `/` «Cada patrimonio tiene su momento» (`home/Momento.astro`) |
| Técnica · coste | CSS `@keyframes` con retardos por `nth-child`; las seis frases están en el HTML |
| Sin JS / reducir movimiento | Rota igual (no depende de JS) / **se muestran las seis**, una debajo de otra |
| Cuándo sí / no | Sí: enumerar situaciones del cliente en un bloque de impacto. No: información que hay que leer entera; más de ~6 frases |
| Estado | ✅ APC (en construcción) — comprobar con Belén: no hay memoria que lo recoja |
| Parámetros | Ciclo de 21s, 3,5s por frase; entra desde `translateY(0.35em)`, sale a `-0.25em`; apiladas en la misma celda del grid para que no salte la altura |

```css
/* APC-gestion-patrimonial/src/components/sections/home/Momento.astro */
.momento__lista { display: grid; }
.momento__lista li { grid-area: 1 / 1; opacity: 0; animation: momento-rotar 21s var(--ease-out) infinite; }
.momento__lista li:nth-child(2) { animation-delay: 3.5s; } /* … hasta 17.5s */
@keyframes momento-rotar {
  0%     { opacity: 0; transform: translateY(0.35em); }
  3%     { opacity: 1; transform: none; }
  13.5%  { opacity: 1; transform: none; }
  16.67% { opacity: 0; transform: translateY(-0.25em); }
  100%   { opacity: 0; transform: translateY(0.35em); }
}
@media (prefers-reduced-motion: reduce) {
  .momento__lista { display: block; }
  .momento__lista li { opacity: 1; animation: none; }
}
```

Trampas: se mueve solo más de cinco segundos sin pausa (WCAG 2.2.2); el
global de APC ya pone `animation-duration: 0.01ms !important` con reducir
movimiento, y este componente lo remata mostrando la lista.

### `contador-cifras` · Las cifras cuentan desde cero

Al entrar en pantalla, cada cifra sube de 0 a su valor.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com/v2/ (`noindex`; `Numbers.astro`, solo con `data-theme="v2"`) · laboratorio de xanasystem (local, `xanasystem-lab/src/pages/lab.astro`, exp. 03 «KPIs») |
| Técnica · coste | JS IO + `requestAnimationFrame` |
| Sin JS / reducir movimiento | Cifra final / cifra final |
| Cuándo sí / no | Sí: 3–4 cifras reales y redondas en una banda. No: cifras pendientes (`[[ ]]`); cifras con decimales o unidades que cambian de ancho (fijar el ancho, sin `tabular-nums` en Mona Sans: [trampa](trampas-y-lecciones.md#los-ceros-salen-barrados-mona-sans)) |
| Estado | 🟡 xanatechnologies v2 · ⚪ laboratorio (no pasó a xanasystem-web; `KpiCard.astro` no lo importa ninguna página) |
| Parámetros | v2: 1500ms, curva cúbica, `threshold: 0.4` · lab: 1400ms, easeOutCubic, `threshold: 0.4`, tarjetas `scale(0.96)` en 0,5s (+100ms) |

La cifra final va en el HTML y el script la reescribe; lector de pantalla y
buscadores leen la final. Sin fragmento: ninguno de los dos está en una web
publicada (código en las rutas citadas).

### `maquina-de-escribir` · El texto se teclea solo

En un mockup (caja de IA, buscador), el texto aparece letra a letra con un cursor, se borra y vuelve a empezar.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada: tarjeta de IA (`ModuleCards.astro`) y buscador del bloque de problema (`ProblemBlock.astro`) · lab exp. 06 |
| Técnica · coste | JS (`setTimeout` por carácter) + IO para arrancar solo en pantalla |
| Sin JS / reducir movimiento | Texto completo / texto completo, sin arrancar |
| Cuándo sí / no | Sí: mockups de producto con buscador o IA. No: titulares o texto de contenido real |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria) · ⚪ lab 06 |
| Parámetros | IA: empieza con la mitad escrita, 30ms/carácter, IO `threshold: 0.4`, red de seguridad `setTimeout(start, 3000)` · buscador: 55ms/carácter, espera 2600ms, borrado 28ms/carácter, `threshold: 0.2` |

### `mockup-en-bucle` · Mockups con microanimaciones continuas

Pantallas de producto dibujadas en HTML con pequeños bucles: una fila que se selecciona, una tira de miniaturas que se desliza, barras que crecen, líneas que brillan.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada, tarjetas de módulos (`ModuleCards.astro`) |
| Técnica · coste | CSS `@keyframes` infinitos |
| Sin JS / reducir movimiento | Igual / quietos (solo con `no-preference`) |
| Cuándo sí / no | Sí: web de producto con varias tarjetas de módulo. No: más de un bucle por tarjeta; fotos reales |
| Estado | 🟡 xanasystem-web (en construcción; commit «microanimaciones + mockups DAM/PIM»; sin memoria) |
| Parámetros | `pim-sel 4s`, `dam-peek 14s` (`translateX(-230px)`), `api-glow 2.4s` (`stroke-opacity` 0,35 → 1), `cat-bar 5s` (`scaleX` 0,35 → 1, retardos 0,3/0,6/0,9s) |

```css
/* xanasystem-web/src/components/sections/ModuleCards.astro */
.mk-dam__track { animation: dam-peek 14s ease-in-out infinite; }
@keyframes dam-peek {
  0%, 28%  { transform: translateX(0); }
  55%, 80% { transform: translateX(-230px); }
  100%     { transform: translateX(0); }
}
```

Si hay que animar una pantalla por pasos, → `pasos-en-bucle`. Puntitos de
ventana en gris, nunca de colores (memoria `puntitos-ventana-gris`).

### `ficha-que-rota` · Una ficha de producto que cambia sola

La tarjeta de un mockup cambia de producto cada pocos segundos: la imagen se funde con un zoom leve y los textos suben escalonados.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada (`ProblemBlock.astro`) · lab exp. 02 (otro código) |
| Técnica · coste | Web Animations API + `setInterval` |
| Sin JS / reducir movimiento | Ficha fija / no arranca |
| Cuándo sí / no | Sí: enseñar variedad de catálogo en un mockup. No: sin parar fuera de pantalla (hoy corre siempre) |
| Estado | 🟡 xanasystem-web · ⚪ lab 02 (`MAX_SWAPS = 2`, cada 3800ms) |
| Parámetros | Cada 2800ms; salida imagen 250ms, textos 260ms (+35ms); entrada imagen `scale(1.08 → 1)` 550ms, textos `translateY(8px)` 440ms (+55ms) |

Imágenes distintas en cada vuelta (memoria `imagenes-reales-producto`). El
comentario del componente dice «solo mientras está en pantalla» y el código
no lo hace.

### `pulso-en-lineas` · Un destello recorre las líneas del diagrama

Un trazo de luz recorre cada línea de un diagrama de conexiones; en las del segundo tramo arranca más tarde, como si atravesara el centro.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-web (local), portada (`sections/ConnectionsDiagram.astro`) · lab exp. 04 (versión con líneas que se dibujan, IO) |
| Técnica · coste | CSS `@keyframes` sobre `stroke-dashoffset` (SVG) + entrada del diagrama con `view()` · cero JS (el JS solo redibuja en móvil) |
| Sin JS / reducir movimiento | Igual / sin destello (`display: none`) y sin entrada |
| Compatibilidad | La entrada usa `animation-timeline: view()`: sin soporte, diagrama en su estado final. El destello no depende de ella |
| Cuándo sí / no | Sí: diagrama de integraciones o flujo de datos. No: diagramas que hay que leer con calma (más de un bucle a la vista) |
| Estado | 🟡 xanasystem-web (en construcción; sin memoria) · ⚪ lab 04 |
| Parámetros | `stroke-dasharray: 12 120`; ciclo 5,6s (pasada del 0 al 40 %, pausa hasta el 100 %); retardo 0,85s en canales; entrada 28px con `animation-range: entry 0% cover 26%` |

```css
/* xanasystem-web/src/components/sections/ConnectionsDiagram.astro */
.cx-spark { display: none; stroke: #dff7fe; stroke-width: 1.4; stroke-dasharray: 12 120; opacity: 0.7; }
@media (prefers-reduced-motion: no-preference) {
  .cx-spark {
    display: block;
    filter: drop-shadow(0 0 1px rgba(233, 251, 255, 0.5)) drop-shadow(0 0 2.5px rgba(77, 212, 236, 0.25));
    animation: cx-flow 5.6s linear infinite;
  }
  .cx-spark--ch { animation-delay: 0.85s; }
}
@keyframes cx-flow {
  0% { stroke-dashoffset: 12; }
  40% { stroke-dashoffset: -120; }
  100% { stroke-dashoffset: -120; }
}
```

### Experimentos del laboratorio sin uso

Todos en `xanasystem-lab/src/pages/lab.astro` (rama `experimentos/animaciones`,
local). Ninguna memoria los aprueba ni los rechaza.

| ID | Qué se ve | Técnica y parámetros | Sin JS / reducir movimiento | Estado |
| --- | --- | --- | --- | --- |
| `medidor-hover` | Al pasar por una tarjeta de consumo, el medidor se llena y las barras hacen una onda | CSS: `scaleX(0.5 → 1)` 1,1s; barras `scaleY(1.08)` 1s con retardo `i × 30ms` | Igual / quieto | ⚪ (exp. 01) |
| `checks-se-dibujan` | Círculo y check que se trazan al aparecer | SVG `stroke-dashoffset` con `pathLength="1"`: círculo 0,5s, check 0,35s a los 0,5s; IO 0,4 | Dibujados / dibujados | ⚪ (exp. 03) |
| `esqueleto-brillo` | Bloques grises con brillo que dan paso al contenido | CSS `sk-shimmer 1.4s`, relevo `sk-phase` 7s | Igual / quieto | ⚪ (exp. 07). **No simular cargas en una web estática** |

---

## 8. Intro, loader y cursor

Todo este grupo es del [módulo de movimiento avanzado](modulos/movimiento-avanzado.md):
**no se ofrece por defecto**, solo cuando las referencias de Belén lo piden.

### `loader-contador` · Pantalla de carga con contador y estallido de luz

Fondo negro con un punto de luz y un contador 000→100; el punto crece hasta cubrir la pantalla y se levanta el velo sobre la portada.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com (portadas `/` y `/es/`; `src/components/Hero.astro`) |
| Técnica · coste | `requestAnimationFrame` (contador) + Web Animations API; página bloqueada (`body.is-loading`, `scrollTo(0, 0)`) |
| Sin JS / reducir movimiento | `<noscript>` lo oculta y arranca el hero / contador a 100 y fundido de 500ms |
| Cuándo sí / no | Sí: web «cinematográfica» de producto con referencia que lo tenga, y Belén aceptando el coste. No: webs que deben convertir (formulario, contacto), webs editoriales |
| Estado | ✅ xanatechnologies (publicada; sin memoria de aprobación) |
| Parámetros | Cuenta 2600ms; destello 550ms (`scale 1 → 2.4`); estallido 1500ms (`→ scale(40)`); velo desde 3750ms durante 1100ms; **4,85s** hasta ver la portada; el hero arranca a 3650ms |

Coste y salvaguardas en [movimiento avanzado §5](modulos/movimiento-avanzado.md#5-intro-o-loader).

```js
// xanatechnologies/src/components/Hero.astro
const COUNT = 2600; // ms counting up
const FLARE = 550;  // dot flares
const BURST = 1500; // dot expands into the field
const overlayDelay = COUNT + FLARE + BURST * 0.4;
const overlay = loader.animate([{ opacity: 1 }, { opacity: 0 }], {
  duration: 1100, delay: overlayDelay, easing: 'cubic-bezier(0.33, 1, 0.68, 1)', fill: 'forwards' });
window.setTimeout(goLive, COUNT + FLARE + BURST - 1000);
window.setTimeout(() => { goLive(); finishLoader(); }, COUNT + FLARE + BURST + 1500);
```

Precisión respecto a movimiento avanzado: el **temporizador de seguridad**
(6150ms) solo existe en la rama sin reducir movimiento; con reducir movimiento
el loader se quita al terminar el fundido (`finished.then(finishLoader, finishLoader)`).

### `logo-se-ensambla` · El logotipo se monta letra a letra

Tras el loader, las letras del logo aparecen desde grandes y desenfocadas hasta su sitio, en orden aleatorio; después sube la línea inferior y entran el lema y la indicación de scroll.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, hero (`Hero.astro`; logo en 6 PNG en `public/logo/hero/`) |
| Técnica · coste | CSS `@keyframes` en pausa hasta `.hero.is-live`; orden barajado con JS (`--i`) |
| Sin JS / reducir movimiento | `<noscript>` las pone en marcha / fundido de 0,8s sin escala ni desenfoque |
| Cuándo sí / no | Solo con el cliente propietario de la marca de acuerdo en trocear su logo. No: cualquier otro logo de cliente (el manual suele prohibir alterarlo) |
| Estado | ✅ xanatechnologies (publicada; sin memoria) |
| Parámetros | Letras 1,7s con `cubic-bezier(0.16, 1, 0.3, 1)`, 0,07s entre letras, `scale(1.55)` y `blur(14px)`; línea inferior 1,3s tras 0,93s; lema 1,2s tras 1,38s; indicación 1,2s tras 1,88s |

```css
/* xanatechnologies/src/components/Hero.astro */
.hero__glyph { opacity: 0; transform: scale(1.55); filter: blur(14px);
  animation: glyph-in var(--glyph-dur) var(--ease-expo) both paused;
  animation-delay: calc(var(--t0) + var(--i) * var(--glyph-stagger)); }
@keyframes glyph-in { from { opacity: 0; transform: scale(1.55); filter: blur(14px); }
  to { opacity: 1; transform: scale(1); filter: blur(0); } }
.hero.is-live .hero__glyph, .hero.is-live .hero__tagline, .hero.is-live .hero__scroll { animation-play-state: running; }
```

### `indicador-scroll` · Invitación a bajar

Una línea vertical por la que baja un brillo (xanatechnologies) o un ratón con un punto que baja (magnaceramica), en bucle, al pie del hero.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, hero («Explore», `Hero.astro`) · magnaceramica (local), hero («Discover more», `Hero.astro`) |
| Técnica · coste | CSS `@keyframes` infinito; en xanatechnologies el clic baja con Lenis |
| Sin JS / reducir movimiento | Igual / xanatechnologies: brillo quieto · magnaceramica: global sin `iteration-count: 1` (mal resuelto) |
| Cuándo sí / no | Sí: hero a pantalla completa sin nada que asome debajo. No: si ya asoma la sección siguiente |
| Estado | ✅ xanatechnologies · 🟡 magnaceramica |
| Parámetros | xanatechnologies: 2,4s, `translateY(-110%)` → `240%`, se desvanece en el primer 40 % del scroll, oculto con `max-height: 640px` · magnaceramica: 1,8s, punto de −4px a 10px |

```css
/* magnaceramica/src/components/Hero.astro */
.hero__scroll-mouse::before { content: ""; width: 3px; height: 8px; border-radius: 2px; background: var(--color-white);
  animation: scrollDot 1.8s var(--ease) infinite; }
@keyframes scrollDot {
  0% { opacity: 0; transform: translateY(-4px); }
  40% { opacity: 1; }
  80% { opacity: 0; transform: translateY(10px); }
  100% { opacity: 0; }
}
```

En magnaceramica el `aria-label` («Scroll to content») no coincide con el
texto visible («Discover more»).

### `cursor-propio` · Cursor con punto y anillo que lo sigue

El cursor nativo se sustituye por un punto blanco pegado al ratón y un anillo que lo sigue con retraso y crece sobre enlaces.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, todas las páginas (`src/components/Cursor.astro`) |
| Técnica · coste | JS `requestAnimationFrame` solo mientras hay movimiento |
| Sin JS / reducir movimiento | Cursor nativo / cursor nativo |
| Cuándo sí / no | Sí: web oscura «de experiencia» con referencia que lo pida. Solo con `(pointer: fine)`; se apaga en el primer `touchstart`. No: webs con formularios largos o público general |
| Estado | ✅ xanatechnologies (publicada; sin memoria) |
| Parámetros | Punto 6px, anillo 34px, seguimiento 0,18; anillo `scale: 1.9` sobre enlaces y 0,5 sobre campos; se oscurece sobre `.surface-light`; `z-index: 2147483647` (por encima del muro) |

```js
// xanatechnologies/src/components/Cursor.astro
tx += (x - tx) * 0.18; ty += (y - ty) * 0.18;
dot.style.transform = `translate3d(${x}px, ${y}px, 0)`;
tail.style.transform = `translate3d(${tx.toFixed(2)}px, ${ty.toFixed(2)}px, 0)`;
```

### `scroll-suave-lenis` · Scroll con inercia

La rueda del ratón desplaza la página con una inercia larga.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com, todas las páginas (`src/components/SmoothScroll.astro`) |
| Técnica · coste | **Lenis** (`lenis ^1.3.26`, 18,7 KB minificado) |
| Sin JS / reducir movimiento | Scroll nativo / apagado |
| Cuándo sí / no | Sí: solo si la referencia lo usa y hay efectos ligados al scroll que lo agradecen. No por defecto. Apagado en táctil |
| Estado | ✅ xanatechnologies (publicada; sin memoria) |
| Parámetros | `duration: 1.7`, `wheelMultiplier: 0.9`, `easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t))`; parado con el loader y con el muro abierto; anclas por `lenis.scrollTo` con `offset: -24` |

Código y trampas en [movimiento avanzado §3](modulos/movimiento-avanzado.md#3-scroll-suave-lenis)
(le falta decir que también se para durante el loader y el `offset` de las anclas).

---

## 9. Fondos y degradados

En xanasystem-web **no hay ningún degradado animado** (comprobado en `src/`);
`.glass`, `.grad-border` y `BackgroundBlob` son estáticos (el blob no lo
importa ninguna página). El cristal como superficie es una decisión de design
system, no de movimiento.

### `fondo-estrellas` · Fondo de espacio con estrellas que titilan

Una foto del espacio se desliza a lo largo de la página y encima un lienzo pinta estrellas en tres profundidades que titilan, derivan con el ratón y dejan pasar alguna estrella fugaz.

| Dato | Valor |
| --- | --- |
| Dónde verla | xanatechnologies.com: portadas, legales y 404 (`src/components/PageBackground.astro`) |
| Técnica · coste | JS con `<canvas>` 2D en un solo bucle `requestAnimationFrame`; foto `space.jpg` 177 KB |
| Sin JS / reducir movimiento | Foto fija sin estrellas / foto centrada y estrellas pintadas una vez, sin fugaces |
| Cuándo sí / no | Sí: marca tecnológica oscura cuya referencia lo pide. No: cualquier web con fondo claro o fotografía de contenido |
| Estado | ✅ xanatechnologies (publicada; sin memoria). Sustituyó a `degradado-webgl` («one fewer per-frame shader», `Landing.astro`) |
| Parámetros | Hasta `min(560, W*H/3200)` estrellas, `dpr ≤ 2`; foto ±22px con el ratón; estrellas `SWING = 70` px y `SCROLL_DRIFT = 0.12`; primera fugaz a los 5–12s y luego cada 9–19s; en táctil sin paralaje de ratón |

```js
// xanatechnologies/src/components/PageBackground.astro
const travel = Math.max(0, img.offsetHeight - window.innerHeight - 60);
const y = -(Math.min(scrollY, maxScroll) / maxScroll) * travel - 30;
img.style.transform = `translate3d(calc(-50% + ${(mx * -NUDGE).toFixed(2)}px), ${(y + my * -NUDGE).toFixed(2)}px, 0)`;
```

### `degradado-webgl` · Degradado vivo con shader

Un degradado azul, violeta y rosa que cambia con el tiempo y el scroll, pintado con WebGL.

| Dato | Valor |
| --- | --- |
| Dónde verla | No se ve en ninguna página: `xanatechnologies/src/components/PageGradient.astro` y `src/snippets/shader-nebula.js` (~4,9 KB), sin importar |
| Técnica · coste | **WebGL**, un shader por fotograma |
| Sin JS / reducir movimiento | Sin fondo / se para (según el código) |
| Cuándo sí / no | No se ofrece salvo petición expresa; se quitó por coste |
| Estado | ⚪ xanatechnologies (código sin uso; también `CustomSolutions.astro`: resplandor que late 8s y globo con paralaje, sin importar) |
| Parámetros | Comprobar en el código si se recupera |

### `texto-barrido-degradado` · El texto barre de gris a oscuro con un resplandor detrás

| Dato | Valor |
| --- | --- |
| Dónde verla | xanasystem-lab (local), `lab.astro` exp. 05 «caja de prompt de IA» |
| Técnica · coste | CSS: `background-clip: text` animando `background-position`; resplandor con `scale(1.05)` |
| Sin JS / reducir movimiento | Igual / apagado |
| Cuándo sí / no | Solo para una pieza de IA en web de producto. Trampas de degradados en texto: [ancho](trampas-y-lecciones.md#un-texto-con-degradado-sale-de-un-solo-color) y [animado](trampas-y-lecciones.md#al-animar-un-texto-con-degradado-el-color-cambia-o-el-paso-es-brusco) |
| Estado | ⚪ laboratorio (en la web la IA usa `maquina-de-escribir`) |
| Parámetros | `ia-text-sweep 3.6s`, `ia-glow-move 16s` |

---

## 10. Rechazadas

No se ofrecen. Si Belén pide «más movimiento», se tiran de las fichas ✅ y 🟡.

| ID | Qué era | Dónde | Fuente |
| --- | --- | --- | --- |
| `titular-linea-a-linea` | Cada palabra agrupada por línea visual subiendo escalonada, en los `h1` de todas las páginas | APC (se revirtió sin commit) | Memoria `apc-movimiento-web`: «mejor quita este efecto... no me gusta» |
| `clip-barrido-foto` | Barrido con `clip-path` sobre fotografía | clays (sustituido por `foto-velo`) | Comentario de `base.css`: «el barrido se lee como una transición» (decisión de diseño, no memoria) |
| `galeria-flechas` | Flechas que rotaban las fotos del abanico | clays | `CHECKLIST.md`: «se descartaron… es una composición cerrada y moverla la deshace» |
| `galeria-cinta` | Cinta de arrastre con avance horizontal al bajar la página | clays | `CHECKLIST.md`: «se cae la cinta de arrastre y su modo cinta» |

| `h1-palabra-a-palabra` | Entrada del `h1` de la portada palabra a palabra (estilo clarasight.com) | xanasystem-web | Memoria `reveal-blur-in`: «Se probó y se DESCARTÓ»; se prefirió cascada por elemento |
| `hero-desenfoque` | Entrada de los heros desde desenfocado | xanasystem-web | Memoria `reveal-blur-in`: «La usuaria pidió QUITAR el desenfoque de los heros» |

**Matiz:** en xanasystem-web lo rechazado es la **entrada** palabra a palabra;
el **encendido de color** palabra a palabra al hacer scroll
(`texto-se-enciende-css`) se conserva. Y xanatechnologies.com, publicada, usa
entradas palabra a palabra y con desenfoque (`frase-fijada`,
`aparicion-desenfoque`, `logo-se-ensambla`): son de esa marca y de su
referencia, no un patrón que se ofrezca a otras.

**Presentaciones (no webs)**: Belén rechazó la versión interactiva de la
presentación de publicación (comparador, puntos calientes, diagramas) y
extender el scroll guiado a más láminas (memorias
`publicacion-proceso-interactiva` y `presentacion-estilo-pitch-deck` de
xanasystem-web). Sirve como aviso: **más interacción no es mejor por defecto**.

---

## 11. Cómo ofrecer opciones

Guía para `xana:animar`. Se parte de la base (aparición sobria, que ya lleva
toda web por [maquetación](flujos/maquetacion.md#7-aparición-al-scroll)) y se
ofrece **más** solo si Belén lo pide o las referencias lo muestran.

### Por tipo de sección

De menos a más movimiento. Solo fichas ✅ o 🟡; las 🟡 se ofrecen avisando de
que no están decididas y con sus trampas corregidas.

| Sección | 1 · Poco | 2 · Medio | 3 · Más | No ofrecer |
| --- | --- | --- | --- | --- |
| **Hero** | `aparicion-cascada-auto` (cascada al cargar) | `parallax-css` en la foto · `hero-foto-a-video` | `hero-renglones` (no a marcas editoriales) · `reel-escenas-gsap` (solo producto, módulo) | `loader-contador`, `logo-se-ensambla`, `hero-desenfoque`, `h1-palabra-a-palabra` salvo petición expresa |
| **Bloque de texto** | `aparicion-fundido` | `texto-se-enciende-css` | `texto-se-enciende` (también en Firefox antiguo) | `titular-linea-a-linea`, dos efectos en el mismo bloque |
| **Rejilla de tarjetas** | `aparicion-escalonada` | + `hover-zoom-foto` (con foto) o `hover-tarjeta-sube` (sin foto) | + `hover-borde-degradado` (marca con degradado) | Elevar y hacer zoom a la vez; escalonado en listas más altas que la pantalla |
| **Cifras** | `aparicion-escalonada` | `titular-asoma` en el titular | `contador-cifras` 🟡 | Contar cifras pendientes o inventadas |
| **Galería** | `hover-zoom-foto` + visor ([multimedia](modulos/multimedia.md)) | `fotos-se-turnan` (2–4 fotos) · `foto-velo` | `galeria-rueda` (confirmar antes: ver incoherencia) | `ken-burns` en bucle, `carrusel-infinito` con avance y sin pausa |
| **Vídeo** | `video-en-pantalla` | + `pulso-play` | `video-a-sangre` 🟡 · `hero-se-encoge` 🟡 | `autoplay` en el HTML, embeds |
| **Cierre / CTA** | `aparicion-escalonada` | `parallax-js` o `zoom-lento-fondo` en la foto | `cierre-se-estrecha` | `frase-fijada` |
| **Cabecera** | Sólida sin cambios | `cabecera-translucida` · `cabecera-pildora-cristal` | `cabecera-condensa` | Transparente sin JS (`cabecera-pastilla-contraste` tal cual) |
| **Pie** | Estático | `pie-cortina` | — | Cortina con un pie más alto que la ventana |
| **Mockups de producto** | Estáticos | `mockup-en-bucle` (uno por tarjeta) · `maquina-de-escribir` | `pasos-en-bucle` · `pulso-en-lineas` | Más de un bucle a la vista; `esqueleto-brillo` |
| **FAQ, pestañas, menús** | `faq-mas-menos` · `desplegable-cae` | `pestanas-fundido` sin avance automático | `pestanas-fundido` con avance (con pausa en hover y foco) | Animar la altura del contenido |

Siempre, sea cual sea la sección: nada de la sección [10](#10-rechazadas);
`cursor-propio`, `scroll-suave-lenis`, `fondo-estrellas` y `degradado-webgl`
solo con el [módulo de movimiento avanzado](modulos/movimiento-avanzado.md) y
referencia de Belén; ningún bucle de más de cinco segundos en contenido sin
pausa ni reducir movimiento (WCAG 2.2.2).

**El tono manda más que la sección.** Marca editorial y serena (APC): columna
1, como mucho 2, y hover sobrio (solo flecha y zoom leve, sin cambios de
color). Marca de producto tecnológico (xanasystem, xanatechnologies): columnas
2 y 3.

### Cómo presentarlas a Belén

Máximo **tres opciones por sección**, en orden de menos a más movimiento, y
siempre la opción «sin más movimiento que la base». Para cada una:

```markdown
**Hero de la portada — tres opciones**

1. **Cascada al cargar** (`aparicion-cascada-auto`) — antetítulo, titular,
   texto y botones entran uno detrás de otro con un fundido corto.
   Verla: xanasystem-web en local, portada. Coste: nada (va en la base).
2. **Foto con profundidad** (`parallax-css`) — la foto de fondo se mueve más
   despacio que la página. Verla: claysoftheworld.com, hero.
   Coste: cero JS; en navegadores sin soporte, foto quieta.
3. **Titular renglón a renglón** (`hero-renglones`) — cada renglón asoma desde
   abajo al cargar. Verla: claysoftheworld.com, hero. Ojo: en APC se rechazó
   un efecto parecido.
```

- **Nombre llano + ID + qué se ve en una frase + dónde verla en vivo.** Primero
  la URL publicada (claysoftheworld.com, xanatechnologies.com); si solo está en
  local, la ruta y que hay que arrancar el proyecto, o montarla en una página de
  laboratorio con `noindex`
  ([design system §9](flujos/design-system.md#páginas-de-laboratorio-pruebas--demo-)).
- Decir el **estado** si no es ✅ y el **coste** si lo hay (JS, librería, tiempo
  hasta ver la portada).
- Cuando elige: se aplica en **una** página, se enseña en la URL temporal y se
  pregunta antes de extender
  ([movimiento avanzado](modulos/movimiento-avanzado.md#cuándo-se-usa-y-cuándo-no)).
- Si Belén aprueba o rechaza algo nuevo, **proponer** actualizar el estado de la
  ficha aquí (no cambiarlo sin permiso, [CLAUDE.md](CLAUDE.md)).

---

## 12. Fuentes

Código leído el 15-09-2026.

| Proyecto | Estado | Rutas |
| --- | --- | --- |
| **clays-of-the-world** | Publicada, claysoftheworld.com | `src/layouts/Base.astro`, `src/styles/base.css` y `tokens.css`, `src/components/Header.astro`, `Button.astro`, `Lightbox.astro`, `sections/Hero.astro`, `CompanyIntro.astro`, `Band.astro`, `Showcase.astro`, `Figures.astro`, `Products.astro`, `Gallery.astro`; `src/scripts/video-autoplay.ts`; `src/pages/pruebas-cabecera.astro`, `pruebas-lab.astro`, `components/pruebas/CabeceraPruebas.astro`; `CLAUDE.md` § Movimiento y JavaScript; `CHECKLIST.md` |
| **APC-gestion-patrimonial** | En construcción (apcgestionpatrimonial.com) | `src/layouts/BaseLayout.astro`, `src/styles/global.css` y `tokens.css`, `components/nav/SiteHeader.astro` y `SiteFooter.astro`, `sections/home/Hero.astro`, `Momento.astro`, `Servicios.astro`, `Conocimiento.astro`, `Cierre.astro`, `sections/gestion-patrimonial/Servicios.astro`, `Faq.astro`; memorias `apc-movimiento-web`, `apc-estado-desarrollo`, `apc-arco-icono-cerrado`, `apc-referencias-web` |
| **xanasystem-web** | En construcción (xanasystem.com sirve todavía otra web) | `src/layouts/BaseLayout.astro`, `src/styles/global.css` y `tokens.css`, `components/ui/HeroReel.astro`, `nav/Header.astro`, `sections/SolutionBlock.astro`, `empresa/EmpresaHero.astro`, `ConnectionsDiagram.astro`, `BenefitCards.astro`, `TrustBlock.astro`, `ModuleCards.astro`, `ProblemBlock.astro`, `TabbedSection.astro`, `PimEcosystem.astro`, `PimWorkflow.astro`; memorias `reveal-blur-in`, `overflow-clip-view-timeline`, `gradiente-texto-hover`, `puntitos-ventana-gris`, `imagenes-reales-producto`, `check-seleccion-color`, `presentacion-estilo-pitch-deck`, `publicacion-proceso-interactiva` |
| **xanasystem-lab** | Laboratorio local (rama `experimentos/animaciones`) | `src/pages/lab.astro` |
| **xanatechnologies** | Publicada, xanatechnologies.com (v2 en `/v2/` con `noindex`) | `src/components/Hero.astro`, `Ecosystem.astro`, `Brands.astro`, `Contact.astro`, `Cursor.astro`, `SmoothScroll.astro`, `PageBackground.astro`, `PageGradient.astro`, `HeroShrink.astro`, `Numbers.astro`, `src/styles/global.css`, `src/snippets/shader-nebula.js`; memorias sin decisiones de movimiento |
| **festival-viento** | Sin publicar confirmado | `src/styles/global.css`, `components/Globos.astro`, `VenAVivirlo.astro`, `SiteHeader.astro`, `Caracteristicas.astro`, `Sorteo.astro`, `Programa.astro`, `LlamadaFinal.astro`; `docs/design.md` |
| **magnaceramica** | Publicación de la versión Astro sin verificar | `src/components/BrandBlock.astro`, `About.astro`, `Hero.astro`, `src/styles/global.css` |
| **grupomerhi** | Fuera de la guía (HTML estático) | `css/styles.css` |

Compatibilidad: [caniuse · animation-timeline](https://caniuse.com/mdn-css_properties_animation-timeline)
(datos de agosto de 2026) y [MDN · animation-timeline](https://developer.mozilla.org/en-US/docs/Web/CSS/animation-timeline).
`@property`: comprobar.
