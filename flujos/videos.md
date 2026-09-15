# Flujo · Vídeos

Vídeos propios de la web (fondo de hero, pieza de sección): dónde se alojan,
cómo se marcan y cómo arrancan y se paran sin castigar la carga ni la
accesibilidad. Los póster se preparan en el [flujo de imágenes](imagenes.md).
Galerías con varios vídeos o embeds → módulo [multimedia](../modulos/multimedia.md).

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Pedir los vídeos originales o enlace de descarga, y si alguno es solo de uso interno ([material](../plantillas/cliente/material.md)) |
| [07 · Construcción](../fases/07-construccion.md) | Subir a R2, decidir si se comprime, marcar el `<video>`, script de arranque |
| [08 · Revisión](../fases/08-revision.md) | Póster al cargar, se paran fuera de pantalla, pausa, «reducir movimiento», nada en el repo |

## 2. Qué tenemos

| Situación | Qué se hace |
| --- | --- |
| El cliente manda vídeo | A R2; decidir por proyecto si se comprime (§3, paso 2) |
| El diseño pide vídeo y no hay | Se monta con el póster como imagen fija: la sección funciona igual y el vídeo entra cuando llegue (clays `VideoMedia.src` opcional) |
| El cliente solo lo tiene en Vimeo o YouTube | Se pide el archivo. Si el embed es imprescindible, va con consentimiento (§6) |
| Vídeo de fondo decorativo (hero) | Arranca solo, en bucle, sin sonido |
| Vídeo de contenido (sección) | Empieza parado o arranca solo según diseño, **con control de pausa** |

## 3. Pasos y quién

### 1. Alojamiento — Belén sube, Claude enlaza

**Siempre en Cloudflare R2, nunca en el repositorio** (ni en `public/`). Un
MP4 en git engorda cada clonado y cada despliegue para siempre.

La URL base va **en un solo sitio**, y los componentes solo leen de ahí:

```ts
// magnaceramica/src/config/media.ts
export const R2_BASE = "https://pub-b7e9a4dc61384ccaa77d47ef1c1910cb.r2.dev/";

export const media = {
  headerVideo: `${R2_BASE}magna-video-slide.mp4`,
  experienceVideo: `${R2_BASE}magna-video-experiencia.mp4`,
} as const;
```

Cambiar de bucket o poner dominio propio es cambiar `R2_BASE`. Nombres de
fichero sin espacios: en clays el del cliente (`VIDEO HOME MINAS.mp4`) va
codificado en la URL (`VIDEO%20HOME%20MINAS.mp4`).

### 2. ¿Se comprime? — Belén decide por proyecto

No hay regla fija: se decide y **se anota el porqué** en los datos y en el
`CHECKLIST.md`.

- **clays-of-the-world: tal cual**, por decisión de Xana (14-09-2026): hero
  13,9 MB, proceso 35 MB (`src/data/en.ts`, comentarios de `hero.media` y
  `facilities.media`).
- Si se comprime, exportar con **`-movflags +faststart`**: el índice del MP4
  al principio hace que el primer fotograma llegue enseguida. El de clays lo
  lleva al final y el navegador lo resuelve con dos peticiones por rangos
  (`Showcase.astro`).

### 3. Marcado — Claude

```astro
<!-- clays-of-the-world/src/components/sections/Showcase.astro -->
<video
  class="showcase__video"
  id={`${content.id}-video`}
  muted
  loop
  playsinline
  preload="metadata"
  aria-label={content.media.poster.alt}
  data-video
  data-autoplay={content.media.autoplay || undefined}
  data-pause-label={content.media.pause}
>
  <source src={content.media.src} />
</video>
```

| Atributo | Por qué |
| --- | --- |
| `muted` + `playsinline` | Sin los dos, iOS no deja arrancar solo y abre el vídeo a pantalla completa |
| `loop` | Fondos y piezas de ambiente |
| **Sin atributo `autoplay`** | Arranca el script, solo en pantalla y sin «reducir movimiento» (paso 4). Con `autoplay` en el HTML arranca siempre |
| `preload="metadata"` | No descarga el vídeo entero a quien no llega. `none` si además la imagen de debajo ya cubre la espera (APC). **Nunca `auto`** |
| `poster` | Imagen fija hasta que arranca. No admite `srcset`: pasarla por `getImage({ src, format: 'webp', width: 1920 })` y usar `.src` (clays `Hero.astro`) |
| `aria-label` | El `alt` del póster: describe lo que se ve. Si es puramente decorativo, `aria-hidden="true"` y `tabindex="-1"` (APC) |

Póster o primer fotograma: en la pieza de contenido de clays **no hay
`poster`**; se ve el primer fotograma del propio vídeo, así lo parado y lo que
arranca son la misma imagen, sin salto. `poster` en los datos queda como imagen
fija de recambio cuando no hay vídeo.

Sin vídeo en los datos, se pinta el póster con `<Picture>` y la sección
funciona igual (`Hero.astro` y `Showcase.astro` de clays, `content.media.src ?
<video> : <Picture>`).

### 4. Arranque y pausa — Claude

Se copia **`clays-of-the-world/src/scripts/video-autoplay.ts`** y se importa
desde cada componente que pinte un `<video data-video>` (Vite lo agrupa una vez
por página).

Lo que hace:

- **Arranca solo en pantalla** (`IntersectionObserver`, `threshold: 0.25`) y
  solo si lleva `data-autoplay` **y** no hay `prefers-reduced-motion: reduce`.
  Con «reducir movimiento» se queda el póster.
- **Se para al salir de pantalla** y se reanuda al volver si estaba en marcha.
- **Una pausa pulsada a mano se respeta siempre**: el observador no la
  desautoriza (`WeakSet` de vídeos parados).
- **Sin JavaScript no arranca ninguno**: se ve el póster, que es el estado
  legible.
- `play()` puede rechazarse (ahorro de batería, pestaña oculta): se ignora con
  `.catch(() => {})`.

**Contrato de marcado** (el script no busca por parentesco):

```html
<video data-video [data-autoplay] data-pause-label="Pause video" id="facilities-video">…</video>
<button type="button" data-play aria-controls="facilities-video" aria-label="Play video">…</button>
```

- El botón se encuentra por **`aria-controls` = id del vídeo**, porque en un
  hero el vídeo va en una capa que no recibe clics y el control está fuera.
- El botón refleja el estado escuchando `play`/`pause` del vídeo: pone
  `data-playing` y cambia `aria-label` entre la etiqueta de play (la suya) y
  `data-pause-label`. Las dos etiquetas son contenido y **se traducen** en los
  datos (`play`, `pause` de `VideoMedia` en `clays src/data/types.ts`).
- Sin vídeo, el distintivo no es botón: `<span aria-hidden="true">` (un botón
  que no hace nada no se anuncia).

### 5. Hero con vídeo sin perjudicar el LCP — Claude

El LCP del hero tiene que ser **una imagen**, no el vídeo. Patrón de
`APC-gestion-patrimonial/src/components/sections/home/Hero.astro`:

1. **La foto va primero**, con `<Image>` optimizada y `loading="eager"`. Es el
   **primer fotograma del vídeo**, para que el relevo no se note.
2. El `<video>` lleva la URL en **`data-src`**, no en `src`, con
   `preload="none"`: no se pide nada al cargar.
3. **Solo en escritorio y sin «reducir movimiento»**: en móvil manda la foto (no
   compensa el gasto de datos).
4. **Se asigna `src` tras el evento `load`** de la página.
5. Al `playing`, el vídeo aparece con fundido y la foto se apaga; si da
   `error`, vuelve la foto.

```ts
// APC-gestion-patrimonial/src/components/sections/home/Hero.astro
const heroApto =
  window.matchMedia("(min-width: 900px)").matches &&
  !window.matchMedia("(prefers-reduced-motion: reduce)").matches;

if (heroVideo && heroApto) {
  heroVideo.addEventListener("playing", () => media?.classList.add("is-video-ready"));
  heroVideo.addEventListener("error", () => media?.classList.remove("is-video-ready"));

  const arrancarHero = () => {
    if (heroVideo.src) return;
    heroVideo.src = heroVideo.dataset.src || "";
    heroVideo.play().catch(() => {});
  };

  if (document.readyState === "complete") arrancarHero();
  else window.addEventListener("load", arrancarHero, { once: true });
}
```

En CSS, `.hero__video { display: none }` en móvil y bajo
`prefers-reduced-motion: reduce`.

Clays hace lo mismo por otro camino: póster optimizado con `getImage` y
`preload="metadata"`, arranque por `video-autoplay.ts`. Asume que el póster no
tiene variantes por ancho («es el precio de tener vídeo», `Hero.astro`). Con
foto grande y móvil importante, preferir el patrón de APC.

### 6. Pausa y WCAG 2.2.2 — Claude propone, Belén decide

La WCAG 2.2.2 pide poder **parar lo que se mueve solo más de cinco segundos**.

- **Vídeo de contenido: control de pausa siempre** (botón `data-play`, paso 4).
- **Excepciones aceptadas**, siempre por decisión anotada en el componente:
  - Hero de fondo en bucle **sin botón** si no arranca con «reducir movimiento»
    (clays `Hero.astro`, decisión del 07-09-2026: «lo único que lo cubre es que
    bajo `prefers-reduced-motion` el vídeo no arranca»).
  - Vídeo solo en escritorio, `aria-hidden`, sin arranque con «reducir
    movimiento» (APC).
- Recuperar el control en un hero es añadir el botón con `aria-controls`: el
  script ya lo entiende.

## 4. Plantillas

- Código: `src/config/media.ts` (Magna) y `src/scripts/video-autoplay.ts`
  (clays), copiados del proyecto de origen hasta que exista el repo plantilla
  ([plantillas de código](../plantillas/codigo/README.md)).
- Cliente: apartado «Vídeos» de [material](../plantillas/cliente/material.md).
- Tipo de datos, de clays `src/data/types.ts`:

  ```ts
  export interface VideoMedia {
    src?: string;          // sin src → se pinta el póster
    autoplay: boolean;     // fondo: true · contenido: según diseño
    poster: Media;         // { src: ImageMetadata; alt: string }
    play?: string;         // etiquetas del control, traducidas
    pause?: string;
  }
  ```

## 5. Dónde queda cada cosa

```
Cloudflare R2 (bucket del proyecto)   los .mp4 · nunca en git
src/config/media.ts                   R2_BASE + URL de cada vídeo
src/data/<idioma>.ts                  VideoMedia por sección (autoplay, póster, etiquetas)
src/assets/<seccion>/                 póster como máster WebP → imágenes
src/scripts/video-autoplay.ts         arranque en pantalla y pausa
src/components/sections/*.astro       <video data-video> + <button data-play>
```

## 6. Reglas y trampas

- **Vídeos fuera del repo.** xanatechnologies tiene `public/video/hero-shrink.mp4`
  (10,4 MB) y `public/video/contact.mp4` (3,5 MB) en git
  ([deuda](../anexos/deuda-proyectos.md)).
- **URL de R2 en un solo sitio.** Clays la escribe entera en `src/data/en.ts`,
  APC en el `data-src` del componente y festival-viento en el `src` del
  `Hero.astro`: cambiar de bucket obliga a buscarla. Se centraliza como Magna.
- **Ni `autoplay` en el HTML ni `preload="auto"`.** Magna (`Hero.astro`) y
  festival-viento (`Hero.astro`) los llevan: el vídeo se descarga entero al
  entrar, compite con la imagen principal y arranca con «reducir movimiento».
- **Siempre `muted playsinline`** o iOS no lo arranca.
- **Parar fuera de pantalla**: no se decodifica lo que no se ve. Hasta
  xanatechnologies lo hace a mano (`Contact.astro`, `IntersectionObserver` +
  `pause()`, y sin arranque con «reducir movimiento»).
- **Embeds de Vimeo o YouTube ponen cookies** al pintarse (`loading="lazy"`
  solo lo retrasa). Preferir vídeo propio en R2; si el embed es imprescindible,
  no se carga hasta aceptar la categoría en el muro, con fachada, y se declara
  en la política → [cookies y analítica](cookies-analitica.md) ·
  [multimedia](../modulos/multimedia.md). Pasó en festival-viento
  (`Caracteristicas.astro`, Vimeo con `autoplay=1`)
  ([trampas](../trampas-y-lecciones.md#analítica-y-cookies)).
- **Texto sobre vídeo**: el contraste del titular se mide sobre los fotogramas
  más claros, no solo sobre el póster (pendiente en clays, `CHECKLIST.md`).
- Juzgar carga con `build` + `preview` o la URL temporal, no en `dev`
  ([trampas](../trampas-y-lecciones.md#imágenes-y-vídeo)).

## 7. Checklist

**Construcción**
- [ ] Vídeos en R2; ninguno en `git ls-files '*.mp4'`
- [ ] `R2_BASE` y URLs en `src/config/media.ts`
- [ ] Decisión de comprimir o no anotada; si se comprime, `+faststart`
- [ ] `<video muted loop playsinline preload="metadata|none">`, sin `autoplay` en el HTML
- [ ] Póster optimizado (`getImage` o primer fotograma) y `aria-label` o `aria-hidden`
- [ ] `video-autoplay.ts` importado; `data-video`, `data-autoplay` donde toque
- [ ] Vídeo de contenido con `<button data-play aria-controls>` y etiquetas traducidas
- [ ] Hero: la imagen es el LCP; vídeo tras `load`, solo escritorio (o decisión anotada)
- [ ] Excepción a la WCAG 2.2.2, si la hay, anotada en el componente
- [ ] Sin embeds de terceros sin consentimiento

**Revisión**
- [ ] Al cargar se ve el póster; en móvil no se descarga lo que no se muestra
- [ ] Fuera de pantalla se para; al volver sigue; la pausa manual se respeta
- [ ] Con «reducir movimiento» activado no arranca ninguno
- [ ] Sin JavaScript se ve el póster y la sección se entiende
- [ ] Botón de pausa accesible con teclado y con nombre correcto en cada estado
