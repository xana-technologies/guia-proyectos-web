# Módulo · Movimiento avanzado

Animaciones más allá de la aparición al hacer scroll, que es de la base
([maquetación](../flujos/maquetacion.md)): efectos ligados al scroll, parallax,
scroll suave, secuencias con GSAP e intros. Referencias:
**xanasystem-web**, **xanatechnologies** y **APC-gestion-patrimonial**.

Cada efecto, con su ID, dónde verlo, estado y código →
[catálogo de animaciones](../catalogo-animaciones.md).

## Cuándo se usa (y cuándo no)

- **Sí:** el diseño o las referencias lo piden y Belén lo aprueba
  (xanatechnologies usa Lenis porque lo usa su referencia:
  `SmoothScroll.astro`).
- **No por defecto.** El listón aceptado en APC es el reveal sobrio (fundido +
  18 px de subida, escalonado en listas) y lo que se añadió de más se rechazó
  (memoria `apc-movimiento-web`).
- **Proponer y esperar visto bueno antes de aplicarlo a todas las páginas**
  (APC; [forma de trabajo](../forma-de-trabajo.md)).

## Qué añade al código

Por orden de preferencia: CSS primero, un script pequeño después, una librería
solo para una pieza que lo necesite (xanasystem-web `CLAUDE.md` regla 1:
cada dependencia de JavaScript se justifica).

### 1. CSS ligado al scroll

**Banner que se estrecha y redondea al llegar al final** (APC, los cuatro
`Cierre.astro`):

```css
/* src/components/sections/home/Cierre.astro */
@media (min-width: 900px) {
  @supports (animation-timeline: scroll()) {
    .cierre__card {
      animation: banner-reveal linear both;
      animation-timeline: scroll(root block);
      animation-range: 84% 100%;
      transform-origin: center top;
    }
  }
}
@media (prefers-reduced-motion: reduce) {
  /* … animation: none */
}
```

**Texto que se enciende palabra a palabra al pasar** con `animation-timeline:
view()` (xanasystem-web `SolutionBlock.astro`, `EmpresaHero.astro`). En Firefox
no funciona y el fundido de entrada sí (memoria `reveal-blur-in`): el estado
sin animación tiene que leerse.

Trampa: un ancestro con `overflow: hidden` lo deja quieto → `overflow: clip`
([trampas · CSS](../trampas-y-lecciones.md#css), «Un reveal ligado al scroll
no anima»).

### 2. Parallax escribiendo variables

APC (`src/layouts/BaseLayout.astro`): el script solo calcula y escribe
`--py`; el CSS de cada elemento decide qué hacer con ella.

```ts
// [data-parallax="0.05"] = fracción de la altura del elemento que se desplaza
// [data-parallax-zoom] da margen para que no se vean los bordes
const update = () => {
  ticking = false;
  const vh = window.innerHeight;
  parallax.forEach((el) => {
    const r = el.getBoundingClientRect();
    if (r.bottom < 0 || r.top > vh) return;          // fuera de pantalla, nada
    const speed = Number(el.dataset.parallax) || 0.05;
    const p = (r.top + r.height / 2 - vh / 2) / (vh / 2 + r.height / 2);
    el.style.setProperty("--py", `${(p * speed * r.height).toFixed(1)}px`);
  });
};
const onScroll = () => {
  if (ticking) return;
  ticking = true;
  requestAnimationFrame(update);
};
window.addEventListener("scroll", onScroll, { passive: true });
```

- La escala de holgura (`--pz`) se fija una vez al cargar, no al entrar en
  pantalla, o la imagen da un salto.
- Todo dentro de `if (!reduce)`.
- Valores usados en APC: 0,03–0,06, zoom 1,08–1,1.
- Otro ejemplo, un fondo a 0,25× del scroll: festival-viento `Globos.astro`.

### 3. Scroll suave (Lenis)

xanatechnologies `src/components/SmoothScroll.astro`:

```ts
const reduce = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
const coarse = window.matchMedia('(pointer: coarse)').matches;

if (!reduce && !coarse) {
  const lenis = new Lenis({ duration: 1.7, smoothWheel: true, autoRaf: true, … });
  // Quieto mientras está el loader o un modal del muro
  const hold =
    document.body.classList.contains('is-loading') ||
    html.classList.contains('show--consent') ||
    html.classList.contains('show--preferences');
}
```

- **Apagado en táctil y con `prefers-reduced-motion`.**
- **Parado con el muro abierto**, y el panel del muro con `data-lenis-prevent`
  → [trampas · Analítica y cookies](../trampas-y-lecciones.md#analítica-y-cookies)
  («El panel de preferencias no hace scroll»).
- Las anclas de la página pasan por `lenis.scrollTo`.

### 4. GSAP para una pieza

Solo en xanasystem-web y solo en el reel de la portada
(`src/components/ui/HeroReel.astro`): escenas que se suceden en bucle con
`gsap.timeline({ repeat: -1 })`.

```ts
import { gsap } from 'gsap';
var mq = window.matchMedia('(prefers-reduced-motion: reduce)');
if (mq.matches) return;
```

- La pieza es decorativa: `aria-hidden="true"`.
- En móvil, con `mobileMode="focus"`, las piezas van colocadas desde el
  principio, sin entradas y salidas, «que producía saltos».

### 5. Intro o loader

xanatechnologies `src/components/Hero.astro`: contador 000→100 y un punto de
luz que se expande antes de mostrar la portada.

- **Coste:** unos 4,9 s en cada carga (2,6 s de cuenta + 0,55 s + 1,5 s de
  estallido, con el velo levantándose desde el 40 % del estallido durante
  1,1 s), con la página bloqueada (`body.is-loading`, `scrollTo(0, 0)`).
- Salvaguardas: se oculta sin JavaScript (`<noscript>`), con
  `prefers-reduced-motion` se reduce a un fundido de 500 ms, y un temporizador
  lo quita pase lo que pase.
- Proponerlo sabiendo ese coste, con Belén.

## Qué no gustó

| Qué | Dónde | Fuente |
| --- | --- | --- |
| Titulares troceados línea a línea o palabra a palabra | APC | Memoria `apc-movimiento-web`: «mejor quita este efecto… no me gusta» |
| Entrada palabra a palabra del `h1` de la portada | xanasystem-web | Memoria `reveal-blur-in`: se prefirió cascada por elemento |
| Desenfoque en la entrada de los heros | xanasystem-web | Memoria `reveal-blur-in`: se quitó, misma entrada que el resto |

Lo que en APC quedó por probar si se pide más movimiento: parallax suave y
máscara en imágenes, arcos que se dibujan al entrar y transiciones entre
páginas.

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| Muro de cookies | Con Lenis: `data-lenis-prevent` en el modal y Lenis parado mientras está abierto |
| Accesibilidad | `prefers-reduced-motion` apaga **lo que se mueve**, no lo que solo cambia de color (clays `CLAUDE.md`); todo efecto con JavaScript parte del estado legible |
| Sin JavaScript | Contenido visible: `<noscript>` que anula el estado oculto (xanasystem-web `BaseLayout.astro`) o clase `js-reveal` puesta por script antes de pintar (APC) |
| Rendimiento | Cada librería suma JavaScript: justificarla; el loader retrasa la primera visión |
| Legales, SEO, formulario, sitemap | Sin cambios |

## Qué pedir al cliente de más

Nada al cliente. **A Belén:** referencias de movimiento concretas (qué web, qué
efecto) y dónde se aplica.

## Reglas y trampas

- **CSS antes que JavaScript; JavaScript propio antes que librería.**
- **Parallax: variable, no `transform` entero**, para que el CSS del elemento
  componga con lo demás; y nada fuera de pantalla.
- **Decisiones de movimiento, página a página**: se prueba en una y se
  pregunta antes de extender.
- **En el preview**, `requestAnimationFrame` y el scroll suave no se ven; no
  es un fallo → [trampas · Verificación](../trampas-y-lecciones.md#verificación-y-herramientas).
- **Tras cambiar un `<script is:inline>` o estilos scoped**, reiniciar el
  servidor de desarrollo → [trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build).

## Checklist

- [ ] Efecto aprobado por Belén en una página antes de extenderlo
- [ ] Hecho con CSS si se puede; librería justificada en el `CLAUDE.md`
- [ ] `prefers-reduced-motion`: nada se desplaza
- [ ] Sin JavaScript todo se lee
- [ ] Firefox: sin `animation-timeline`, el contenido se ve igual
- [ ] Táctil: sin scroll suave
- [ ] Muro abierto: la página no se mueve y el panel hace scroll
- [ ] Ancestros de efectos ligados al scroll con `overflow: clip`
- [ ] Peso del JavaScript y tiempo hasta ver la portada medidos con `build` + `preview`

## Proyecto de referencia

- **xanasystem-web**: `src/components/ui/HeroReel.astro`,
  `src/components/sections/SolutionBlock.astro`,
  `src/layouts/BaseLayout.astro`, `docs/design.md` §1 (Motion), memorias
  `reveal-blur-in`, `overflow-clip-view-timeline`, `preview-smooth-scroll-no-anima`.
- **xanatechnologies**: `src/components/SmoothScroll.astro`,
  `src/components/Hero.astro` (loader), `src/layouts/Base.astro`.
- **APC-gestion-patrimonial**: `src/layouts/BaseLayout.astro` (reveal y
  parallax), `src/components/sections/*/Cierre.astro`, memoria
  `apc-movimiento-web`.
