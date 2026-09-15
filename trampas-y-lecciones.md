# Trampas y lecciones

Lo que ya ha fallado en las webs de Xana, para no volver a caer. Cada entrada
empieza por **lo que se ve** (el síntoma), y dice por qué pasa, cómo se
arregla y en qué proyecto ocurrió. Todas están comprobadas en el código, el
`CLAUDE.md`, el `CHECKLIST.md` o las memorias del proyecto que se citan.

**Cómo usarla:** ante un fallo, busca aquí el síntoma antes de investigar. Si
aprendes algo que no está, **propón** añadirlo (no lo añadas sin permiso).

## Índice

- [Formulario](#formulario): 403 solo en Vercel · el servidor rechaza lo que
  el navegador aceptó · texto pegado por debajo del mínimo · remitente vacío ·
  remitente no verificado · 500 al abrir `/api/contact` · el segundo envío
  falla la verificación · 200 pero no llega · «unrecognised IP address» ·
  variable nueva que el endpoint no ve · datos en la URL sin JavaScript · POST
  de prueba rechazado en local
- [Analítica y cookies](#analítica-y-cookies): la analítica no ejecuta
  comandos · cookies antes de aceptar · sigue midiendo tras retirar el
  consentimiento · la política declara lo que no hay · embeds sin
  consentimiento · etiquetas nuevas en GTM · panel sin scroll con Lenis · borde
  alrededor del muro
- [Astro y build](#astro-y-build): el adaptador de Vercel no casa · estilos
  que no se actualizan en `dev` · el HMR no detecta cambios · selector `+`
  roto en `dev` · redirecciones con barra final · 404 en el idioma equivocado ·
  Vercel busca la salida en otra carpeta · páginas de prueba publicadas ·
  sitemap que no aparece · `Disallow` que tapa el `noindex`
- [Imágenes y vídeo](#imágenes-y-vídeo): imágenes lentas en `dev` · PNG
  enormes en el build · visor que abre vacío · contenido de un `<dialog>` que
  no existe para buscadores · imágenes de `public/` sin optimizar
- [CSS](#css): espaciado a 0 · reveal por scroll que no anima · degradado de un
  solo color · degradado que cambia de tono al animarlo · cero barrado en Mona
  Sans · scroll horizontal por un grid · mockup gigante al sangrarlo · estilos
  que no llegan al `<slot>` · bandas a sangre que no llegan al borde
- [Git y publicación](#git-y-publicación): push denegado · push hecho y la web
  no cambia · web publicada en una cuenta personal · commit con trabajo ajeno ·
  historial que se contradice · Vercel no acepta el dominio
- [Verificación y herramientas](#verificación-y-herramientas): capturas que se
  cuelgan · captura en blanco · el preview no conecta · colores obsoletos al
  inspeccionar · scroll suave que no se mueve · Chrome real no abre el `dev` ·
  falso negativo con acentos · colores de Figma que no cuadran

---

## Formulario

Detalle del sistema en [`flujos/formulario.md`](flujos/formulario.md).

### 403 «Cross-site POST form submissions are forbidden», solo en Vercel

- **Causa:** Astro compara la cabecera `Origin` del POST con el origen de la
  petición, y solo toma el dominio real (`Host` / `X-Forwarded-Host`) si está
  en `security.allowedDomains`. Con la lista vacía usa `localhost`, y
  `https://preview.cliente.com` ≠ `https://localhost`. En local no se ve porque
  todo es `localhost`.
- **Solución:** declarar todos los dominios desde los que se sirve la web
  (principal, `www`, preview). No desactivar la comprobación con
  `checkOrigin: false`.

  ```js
  security: {
    allowedDomains: [
      { hostname: 'cliente.com', protocol: 'https' },
      { hostname: 'www.cliente.com', protocol: 'https' },
    ],
  },
  ```
- **Dónde pasó:** clays-of-the-world, `astro.config.mjs` (commit `1300822`,
  14-09-2026).

### El servidor rechaza lo que el navegador dio por bueno

- **Causa:** los mínimos del formulario (`minlength`) y los del endpoint no
  coincidían: el navegador dejaba enviar y el endpoint respondía `validation`,
  y el visitante veía un error sin saber por qué.
- **Solución:** los mismos mínimos en los dos sitios (nombre 2, mensaje 5) y
  un comentario en cada lado que apunte al otro.
- **Dónde pasó:** clays-of-the-world, `src/pages/api/contact.ts` y
  `src/components/sections/Contact.astro` (commit `3a0afc8`).

### Un texto pegado o autocompletado por debajo del mínimo pasa la validación del navegador

- **Causa:** `validity.tooShort` solo se marca cuando el campo se ha editado
  tecleando. Un valor pegado o autocompletado pasa `checkValidity()` y lo
  rechaza el endpoint con el aviso genérico.
- **Solución:** en el script, comprobar el mínimo a mano además de
  `tooShort`:

  ```ts
  const texto = control.value.trim();
  const corto = control.minLength > 0 && texto.length > 0 && texto.length < control.minLength;
  ```
- **Dónde pasó:** clays-of-the-world, `src/components/sections/Contact.astro`
  (función `check`).

### Brevo rechaza el remitente aunque el código tiene un valor por defecto

- **Causa:** la variable `FORM_FROM_EMAIL` quedaba guardada **vacía** en
  Vercel, y con `??` una cadena vacía no cae al valor por defecto: llegaba `''`
  a Brevo («valid sender email required»). La plantilla de Magna usa `??`.
- **Solución:** `||` en todos los valores de `form.config.ts`.

  ```ts
  fromEmail: import.meta.env.FORM_FROM_EMAIL || 'no-reply@xanatechnologies.com',
  ```
- **Dónde pasó:** xanatechnologies, `src/form.config.ts` (commit `0906f3f`) y
  memoria `xana-go-live` (09-09-2026). Origen: `magnaceramica/docs/forms/Guia_Formularios_Astro.md`.

### `invalid_parameter` sobre el sender: el aviso no sale

- **Causa:** los documentos de Magna usan `no-reply@forms.xanasystem.com`,
  pero ese dominio **no está** en la cuenta de Brevo que se usa. Al copiar el
  sistema se arrastra al `.env.example`, al valor por defecto y a los
  comentarios.
- **Solución:** remitente **`no-reply@xanatechnologies.com`** en todas las
  webs (dominio autenticado con DKIM). Al copiar el sistema de otro proyecto,
  buscar `forms.xanasystem.com` y sustituirlo.
- **Dónde pasó:** xanatechnologies, `CLAUDE.md` § Contact form (commit
  `e68821c`); clays-of-the-world, commit `61ad360` y memoria
  `remitente-formularios-xana` (14-09-2026).

### 500 «fetch failed» en el log de Vercel sin que nadie haya enviado nada

- **Causa:** abrir `/api/contact` en el navegador es un `GET`. Sin manejador,
  Astro intenta servir su página de error, falla y la función acaba en 500.
- **Solución:** responder 405 a `GET`.

  ```ts
  export const GET: APIRoute = () =>
    new Response('Method Not Allowed', { status: 405, headers: { Allow: 'POST' } });
  ```
- **Dónde pasó:** clays-of-the-world, `src/pages/api/contact.ts` (commit
  `3a0afc8`).

### El segundo intento falla la verificación aunque sea una persona

- **Causa:** el token de Turnstile vale para **un** envío. Tras un error (por
  ejemplo de validación), el siguiente envío lleva un token ya gastado.
- **Solución:** `window.turnstile?.reset()` en el `finally` de cada intento.
- **Dónde pasó:** clays-of-the-world, `src/components/sections/Contact.astro`.

### Todo responde 200 pero el correo no llega

- **Causa:** un 200 de Brevo significa que **aceptó** la llamada, no que haya
  entregado: spam, bloqueo o remitente sin DKIM.
- **Solución:** registrar el `messageId` en el log del endpoint y buscarlo en
  Brevo → Transactional → Logs; mirar la carpeta de spam; confirmar el
  remitente (entrada anterior).
- **Dónde pasó:** documentado en clays-of-the-world
  (`src/pages/api/contact.ts`), xanatechnologies (`CLAUDE.md`) y
  `magnaceramica/docs/forms/Documentacion_Tecnica_Formularios_Astro.md` §15.

### `send` con `unauthorized` «unrecognised IP address»

- **Causa:** la cuenta de Brevo tiene activada la autorización de IP
  (Security → Authorized IPs) y la API key se usa desde una IP nueva (las
  funciones de Vercel).
- **Solución:** lo resuelve ingeniería en Brevo. No es la lista blanca de
  SMTP: la API HTTP no la necesita.
- **Dónde pasó:** `magnaceramica/docs/forms/Documentacion_Tecnica_Formularios_Astro.md`
  §15; recogido en clays-of-the-world, `src/pages/api/contact.ts`.

### Variable añadida en Vercel y el endpoint sigue sin verla (`config` o 500)

- **Causa:** las variables de entorno solo se aplican en el **siguiente
  despliegue**.
- **Solución:** avisar a Belén de que hace falta publicar de nuevo (Claude no
  despliega). El endpoint dice qué variable falta en `detail`, sin su valor.
- **Dónde pasó:** `magnaceramica/docs/forms/Documentacion_Tecnica_Formularios_Astro.md`
  §15; clays-of-the-world, `src/pages/api/contact.ts` (error `config`).

### Sin JavaScript, nombre, correo y mensaje acaban en la URL

- **Causa:** un `<form>` sin `method` es `GET`, y sin script el navegador lo
  envía como parámetros de la URL.
- **Solución:** `method="post"` siempre, aunque todavía no haya `action`.
- **Dónde pasó:** clays-of-the-world, `src/components/sections/Contact.astro`.

### Un POST de prueba desde la terminal da 403 en local

- **Causa:** Astro exige la cabecera `Origin` en los POST de formulario
  (protección CSRF). El navegador la manda sola; `curl` o `Invoke-WebRequest`
  no.
- **Solución:** probar desde el navegador, o añadir `Origin` a la petición de
  prueba. No es un fallo del endpoint.
- **Dónde pasó:** xanasystem-web, memoria `formularios-patron-magna`.

---

## Analítica y cookies

Detalle en [`flujos/cookies-analitica.md`](flujos/cookies-analitica.md).

### La analítica se carga pero no ejecuta los comandos

- **Causa:** `gtag.js` y GTM solo reconocen los comandos que llegan a
  `dataLayer` como objeto `Arguments`. Con un array (`(...args) =>
  dataLayer.push(args)`) la cola se llena pero `consent`, `config`, etc. no se
  ejecutan.
- **Solución:**

  ```ts
  window.gtag = function () {
    window.dataLayer.push(arguments);
  };
  ```
- **Dónde pasó:** magnaceramica, `src/components/CookieConsent.astro` (carga
  `gtag.js` con `push(args)`); corregido al copiarlo en clays-of-the-world,
  `src/components/CookieConsent.astro` (commit `d1b2027`).

### Google pone cookies antes de que el visitante acepte

- **Causa:** pegar el snippet oficial de GTM arriba del `<head>` lo carga
  siempre, y el `<noscript>` con iframe tras el `<body>` lo carga sin
  JavaScript, donde no hay muro que pregunte.
- **Solución:** cargar GTM desde el muro, solo al aceptar la categoría de
  analítica, con Consent Mode v2 declarado antes y la publicidad denegada. Sin
  `<noscript>`.
- **Dónde pasó:** clays-of-the-world, `src/components/CookieConsent.astro` y
  `CHECKLIST.md` §9.

### Tras retirar el consentimiento, la web sigue midiendo

- **Causa:** borrar las cookies no descarga GTM ya cargado; sigue midiendo
  hasta que el visitante cambia de página.
- **Solución:** `reloadPage: true` en el `autoClear` de la categoría.

  ```ts
  autoClear: {
    cookies: [{ name: /^_ga/ }, { name: '_gid' }, { name: '_gat' }],
    reloadPage: true,
  },
  ```
- **Dónde pasó:** magnaceramica, `src/components/CookieConsent.astro` (sin
  `reloadPage`); clays-of-the-world, `src/components/CookieConsent.astro`
  (commit `d1b2027`).

### La política de cookies declara cosas que la web no usa

- **Causa:** se rellenó con la plantilla del protocolo y no con lo que la web
  pone de verdad. Magna declara `_gid`, `_gat` (de Universal Analytics: GA4
  no los pone) y cookies de Google Maps sin tener ningún mapa; su política de
  privacidad nombra Mailchimp.
- **Solución:** la tabla sale de la web: con la base, `cc_cookie`, `_ga` y
  `_ga_<ID>`. Nada que no se cargue.
- **Dónde pasó:** magnaceramica, `src/pages/cookies-policy.astro` y
  `src/pages/privacy-policy.astro` (commit `8d69d4b`); criterio aplicado en clays-of-the-world,
  `CHECKLIST.md` §9.

### Embeds de Vimeo y Google Maps que cargan sin consentimiento

- **Causa:** un `<iframe>` de terceros pone cookies y carga JavaScript de
  seguimiento en cuanto se pinta (`loading="lazy"` solo lo retrasa). En
  festival-viento entraron aunque su `CLAUDE.md` (regla 3) prohíbe
  expresamente embeds de Vimeo, YouTube y Google Maps.
- **Solución:** vídeo propio en lugar de Vimeo/YouTube
  ([`flujos/videos.md`](flujos/videos.md)); para mapas, imagen o SVG estático
  con enlace. Si el embed es imprescindible, no se carga hasta que se acepta.
- **Dónde pasó:** festival-viento, `src/components/Caracteristicas.astro`
  (Vimeo, commit `221910d`) y `src/components/Mapa.astro` (Google My Maps,
  commits `f06459d` y `c83c1a8`).

### Una etiqueta nueva en GTM pone cookies que la política no declara

- **Causa:** lo que se dispara lo decide el contenedor de GTM, no el código.
  Añadir Google Ads, un píxel o Hotjar desde GTM no pasa por el repositorio.
- **Solución:** el contenedor dispara GA4 y nada más. Cualquier otra etiqueta
  se añade antes a la tabla de la política y, si es publicitaria, con su
  categoría en el muro. Revisar el contenedor antes de publicar.
- **Dónde pasó:** clays-of-the-world, `src/components/CookieConsent.astro` y
  `CHECKLIST.md` §9.

### El panel de preferencias no hace scroll

- **Causa:** Lenis (scroll suave) captura la rueda para toda la página,
  también dentro del modal.
- **Solución:** marcar el modal con `data-lenis-prevent` al montarlo.

  ```ts
  onModalReady: ({ modal }) => modal?.setAttribute('data-lenis-prevent', ''),
  ```
- **Dónde pasó:** xanatechnologies, `src/components/CookieConsent.astro`.

### Al abrir el muro aparece un borde de color alrededor de la caja

- **Causa:** la librería mueve el foco a un contenedor con `tabindex="-1"`
  (para el lector de pantalla) y el navegador le pinta el anillo de
  `:focus-visible`.
- **Solución:** quitar el anillo solo a ese contenedor.

  ```css
  html #cc-main [tabindex='-1']:focus-visible { outline: none; }
  ```
- **Dónde pasó:** clays-of-the-world, `src/components/CookieConsent.astro`.

---

## Astro y build

### `@astrojs/vercel` no instala o no compila con la versión de Astro

- **Causa:** cada versión mayor del adaptador pide una de Astro, y
  `npm install @astrojs/vercel` trae la última.
- **Solución:** fijar la pareja: **8 ↔ Astro 5 · 10 ↔ Astro 6 · 11 ↔ Astro 7**.
- **Dónde pasó:** clays-of-the-world, `astro.config.mjs` («Versión 8, no la 11:
  la 11 pide Astro 7», commit `6f743ed`); xanasystem-web, memoria
  `formularios-patron-magna` (v10 para Astro 6). Parejas en uso en los
  `package.json`: clays 8/5, xanasystem-web 10/6, xanatechnologies y
  magnaceramica 11/7.

### Cambios de estilo que no se ven en `dev`, ni recargando

- **Causa:** el HMR de Vite dice «hot updated» pero sigue sirviendo el CSS
  antiguo de los `<style>` scoped de los `.astro` (la caché está en la
  transformación, no en el navegador). Pasa también con los
  `<script is:inline>`.
- **Solución:** reiniciar el servidor de desarrollo y recargar. Tras
  reiniciar puede cambiar de puerto.
- **Dónde pasó:** xanasystem-web, memorias `vite-css-scoped-stale` y
  `reveal-blur-in`; APC-gestion-patrimonial, memoria `apc-estado-desarrollo`.

### El servidor de desarrollo no detecta las ediciones (Windows)

- **Causa:** los eventos nativos de ficheros no son fiables en ese equipo y
  unidad; el HMR y la caché de render no se enteran de los cambios.
- **Solución:**

  ```js
  vite: { server: { watch: { usePolling: true, interval: 200 } } },
  ```
- **Dónde pasó:** magnaceramica, `astro.config.mjs` (commit `8c20115`).

### Un selector `+` funciona en el build y no en `dev`

- **Causa:** en desarrollo Astro inserta el `<script>` del componente entre
  dos hermanos, y el combinador adyacente deja de casar.
- **Solución:** usar `~` (hermano general) cuando no haga falta que sean
  contiguos: `.brand ~ .brand { … }`.
- **Dónde pasó:** magnaceramica, `src/components/BrandBlock.astro` (commit
  `04e9ffa`).

### Las URL antiguas con barra final no redirigen (404)

- **Causa:** Astro normaliza el origen de las redirecciones a `^/old-url$`, y
  la web anterior (WordPress) enlazaba `/old-url/`, que es lo que tienen Google
  y terceros.
- **Solución:** script tras `astro build` que hace que cada regla 301 de
  `.vercel/output/config.json` acepte la barra opcional (`^/old-url/?$`), y
  `npm run build` como comando de build en Vercel.
- **Dónde pasó:** xanatechnologies, `scripts/fix-redirect-slashes.mjs` y
  `astro.config.mjs` (commit `efa2117`). Ver
  [`flujos/seo-tecnico.md`](flujos/seo-tecnico.md).

### La 404 sale siempre en el idioma por defecto

- **Causa:** una 404 estática no sabe qué ruta se pidió. En xanatechnologies,
  además, la redirección por idioma del navegador tapaba el error.
- **Solución:** `export const prerender = false` en `404.astro` para que elija
  idioma por el prefijo de la ruta; en xanatechnologies, `Base` con
  `detectLanguage={false}` en esa página.
- **Dónde pasó:** clays-of-the-world, `src/pages/404.astro` y `CLAUDE.md`;
  xanatechnologies, `CLAUDE.md` § Project structure.

### Vercel construye con el preset equivocado y busca la salida en `public`

- **Causa:** el proyecto se creó con el preset «Other» y salida `public`, que
  para Astro es la carpeta equivocada; la configuración dependía de lo que
  Vercel adivinara.
- **Solución:** `vercel.json` en el repo con `"framework": "astro"` y
  `"buildCommand": "npm run build"`. Con el preset de Astro, Vercel toma la
  salida del adaptador; no hace falta `outputDirectory`.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §13 (commit `733c573`).
  El `CHECKLIST.md` menciona también `outputDirectory: dist`, pero el
  `vercel.json` actual solo lleva `framework` y `buildCommand`, igual que el de
  xanatechnologies.

### Las páginas de prueba se publican y engordan el despliegue

- **Causa:** todo lo que está en `src/pages/` se compila y se publica. Las
  `/pruebas-*` de clays generaban la mayor parte de los 34 MB de
  `dist/_astro` con imágenes que la portada ya no usa, y tres eran indexables.
- **Solución:** `noindex, nofollow` desde que se crean y borrarlas cuando se
  elige la opción (el historial las conserva), como se hizo en APC con
  `demo-arcos.astro`.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §6 y §8
  (`src/pages/pruebas-*.astro`); APC-gestion-patrimonial,
  `src/pages/demo-color.astro`, `pruebas-header-contacto.astro` y
  `estilo.astro` (con `noindex`, pero publicadas) y memoria
  `apc-estado-desarrollo`.

### El sitemap no está en `/sitemap.xml`

- **Causa:** `@astrojs/sitemap` genera `sitemap-index.xml`, que no se
  encuentra en la ruta habitual.
- **Solución:** endpoint propio `src/pages/sitemap.xml.ts` que lista las
  páginas indexables con sus `hreflang`, o, si se usa la integración, enviar
  `sitemap-index.xml` a Search Console.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §6
  (`src/pages/sitemap.xml.ts`, commit `4aa1119`); xanatechnologies usa la integración,
  `CLAUDE.md` § Go-live status. Ver [`flujos/seo-tecnico.md`](flujos/seo-tecnico.md).

### Una página con `noindex` sigue en Google

- **Causa:** un `Disallow` en `robots.txt` impide al buscador leer la página
  y, por tanto, su `noindex`.
- **Solución:** `robots.txt` sin bloqueos; lo que no se indexa se marca con
  `noindex` en la propia página.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §6
  (`src/pages/robots.txt.ts`).

---

## Imágenes y vídeo

Detalle en [`flujos/imagenes.md`](flujos/imagenes.md) y
[`flujos/videos.md`](flujos/videos.md).

### Las imágenes tardan en cargar en `npm run dev`

- **Causa:** en desarrollo Astro convierte a AVIF en cada petición y no guarda
  nada (0,4–1,2 s por foto, en frío y en caliente).
- **Solución:** no juzgar la carga en `dev`: `npm run build` + `npm run
  preview`, o la preview de Vercel.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §8.

### El build genera megas de PNG que nadie pide

- **Causa:** `<Picture>` con `formats={['avif','webp']}` genera además una
  reserva en el formato original (PNG): 25 MB en clays.
- **Solución:**

  ```astro
  <Picture src={img} alt="…" formats={['avif', 'webp']} fallbackFormat="webp" />
  ```
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §2 (commit `c85ea6d`).

### El visor de la galería abre vacío y se va llenando

- **Causa:** tres a la vez: fotos `loading="lazy"` dentro de un `<dialog>`
  cerrado (no se piden hasta el clic); anchos del visor distintos a los de la
  sección (se descarga otra variante de la misma foto); y `sizes` que no
  contaba el recorte con `object-fit: cover` (fotos borrosas).
- **Solución:** precargar con `IntersectionObserver` cuando la galería se
  acerca a la pantalla, compartir la lista de `widths` entre sección y visor y
  calcular `sizes` según el marco de cada pieza.
- **Dónde pasó:** clays-of-the-world, `CHECKLIST.md` §8
  (`src/components/Lightbox.astro`, commit `d1b2027`).

### Lo que está dentro de un `<dialog>` no existe para buscadores ni sin JavaScript

- **Causa:** un `<dialog>` cerrado es `display: none` para todo el mundo.
- **Solución:** una página de verdad con el mismo contenido (`/gallery`) y
  disparadores que son **enlaces** a esa página, no botones; el script les
  quita el salto y abre el visor.
- **Dónde pasó:** clays-of-the-world, `CLAUDE.md` § Estructura de la web.

### Las imágenes de `public/` pesan lo que pesa el original

- **Causa:** `astro:assets` solo optimiza imágenes importadas desde `src/`.
  Lo de `public/` se sirve crudo, sin `srcset`. Hay casos que no pueden pasar
  por `<Image>`: `background-image` de CSS y `poster` de vídeo.
- **Solución:** contenido con `<Image>`/`<Picture>` desde `src/assets/`; lo
  que tenga que ir en `public/`, optimizado antes con `sharp`. En Windows,
  leer el fichero a un buffer antes de reescribirlo, o `sharp` bloquea el
  fichero.
- **Dónde pasó:** xanasystem-web, memoria `optimizacion-imagenes-public`
  (15,6 → 6 MB); magnaceramica, `CLAUDE.md` § Conventions.

---

## CSS

Detalle en [`flujos/maquetacion.md`](flujos/maquetacion.md) y
[`flujos/design-system.md`](flujos/design-system.md).

### Todo sale «muy junto»: columnas pegadas, cabeceras sin padding

- **Causa:** la escala de espaciado no es continua. Un token que no existe
  (`var(--sp-10)` sin fallback) invalida la declaración y `gap`/`padding` se
  quedan a 0, sin error visible.
- **Solución:** usar solo los tokens que existen y redondear al más cercano
  (5 → 6, 10 → 12). Mirar `tokens.css` antes de escribir un espaciado.
- **Dónde pasó:** xanasystem-web, memoria `escala-espaciado-tokens` y
  `CLAUDE.md` regla 13.

### Un reveal ligado al scroll no anima

- **Causa:** un ancestro con `overflow: hidden` se convierte en contenedor de
  scroll, y `view()` / `view-timeline` mide contra él, que no se desplaza.
- **Solución:** `overflow: clip` en ese ancestro: recorta igual y no crea
  contenedor de scroll.
- **Dónde pasó:** xanasystem-web, memoria `overflow-clip-view-timeline` (hero
  de `/empresa`).

### Un texto con degradado sale de un solo color

- **Causa:** el elemento con `background-clip: text` se estira al ancho del
  contenedor (hijo de un flex en columna), el degradado se reparte por todo
  ese ancho y los glifos solo captan el primer tramo.
- **Solución:** que mida lo que miden los glifos.

  ```css
  align-self: flex-start;
  width: fit-content;
  ```
- **Dónde pasó:** xanasystem-web, memoria `gradiente-texto-fit-content`
  (números de `DemoPasos`).

### Al animar un texto con degradado, el color cambia o el paso es brusco

- **Causa:** el degradado no es transicionable, así que se superpone una copia
  (`::after` con `content: attr(…)`) y se funde su opacidad. Con colores de
  marca translúcidos, la copia se mezcla con el texto de debajo y sale
  apagada; y si el degradado se aplica a la palabra en vez de al contenedor,
  recorre otra parte de la rampa.
- **Solución:** colores ya mezclados con blanco y opacos en la copia, apagar
  el texto base a la vez y fijar `background-size`/`background-position` al
  contenedor si hay que reproducir su degradado.
- **Dónde pasó:** xanasystem-web, memoria `gradiente-texto-hover`.

### Los ceros salen barrados (Mona Sans)

- **Causa:** Mona Sans dibuja el cero barrado con
  `font-variant-numeric: tabular-nums`, y no se desactiva con
  `font-feature-settings` (`'zero' 0`, `'tnum' 0`, `ss01`–`ss05`). Además, el
  atajo `font:` reinicia `font-feature-settings` heredado.
- **Solución:** quitar `tabular-nums` en textos con Mona Sans. Comprobarlo con
  una captura ampliada: a tamaño normal no se aprecia.
- **Dónde pasó:** xanasystem-web, memoria `mona-sans-cero-barrado`.

### Scroll horizontal en móvil por un bloque que sobresale

- **Causa:** los hijos de un grid apilados en la misma celda
  (`grid-area: 1 / 1`) tienen `min-width: auto` y no bajan de su ancho mínimo
  de contenido.
- **Solución:** `min-width: 0` en esos hijos.
- **Dónde pasó:** xanasystem-web, memoria `mockup-hero-escala-movil`
  (`DamPimVs` en `/dam`).

### Un mockup se vuelve gigante al hacerlo sangrar hasta el borde

- **Causa:** se ensanchó con `vw`, y su contenido escala con el ancho
  (container queries).
- **Solución:** acotar el ancho y anclarlo al borde con margen negativo, en
  vez de ensancharlo.
- **Dónde pasó:** xanasystem-web, memoria `mockup-sangra-derecha-centrado`.

### Los estilos del componente no se aplican al contenido del `<slot>`

- **Causa:** el CSS scoped de Astro no alcanza lo que llega por `<slot>` (por
  ejemplo, la prosa de una página legal).
- **Solución:** `:global()` bajo la clase del componente:
  `.legal__prose :global(h2) { … }`.
- **Dónde pasó:** APC-gestion-patrimonial, `LegalDoc.astro` (memoria
  `apc-estado-desarrollo`); magnaceramica, `LegalLayout.astro` (`CLAUDE.md`
  § Legal pages).

### Las bandas a sangre no llegan al borde de la ventana

- **Causa:** `scrollbar-gutter: stable both-edges` reserva el hueco de la
  barra a los dos lados, y ninguna sección puede tocar el borde.
- **Solución:** `scrollbar-gutter: stable` (solo un lado), asumiendo la
  asimetría de unos 15 px entre márgenes.
- **Dónde pasó:** clays-of-the-world, `CLAUDE.md` § Design system
  (`src/styles/reset.css`).

---

## Git y publicación

Detalle en [`flujos/publicacion.md`](flujos/publicacion.md). Recordatorio fijo:
**commit y push solo con permiso; publica siempre Belén** con un commit
manual desde GitHub con la cuenta `xanasystem`; **Claude nunca publica**.

### `Permission denied` al hacer push

- **Causa:** en este equipo `git@github.com` se autentica con una cuenta que
  no tiene acceso a los repos de `xanasystem`.
- **Solución:** remoto con el alias SSH `github-belenxana`.

  ```
  git remote set-url origin git@github-belenxana:xanasystem/<repo>.git
  ```
- **Dónde pasó:** APC-gestion-patrimonial, memoria `acp-git-remote-ssh` y
  `CLAUDE.md` § Stack y despliegue; magnaceramica, memoria
  `push-ssh-belenxana` (HTTPS tampoco servía: credencial caducada).

### Push hecho y la web no cambia

- **Causa:** Vercel se niega a compilar los commits firmados como
  `belen@xanasystem.com`: solo la cuenta `xanasystem` tiene permiso de
  despliegue en el equipo. Cada push vuelve a dejar el último commit con esa
  autoría.
- **Solución:** no es un fallo. Tras el push, **Belén** hace el commit manual
  desde GitHub con `xanasystem`. Claude dice «subido, pendiente de tu paso en
  GitHub», nunca «publicado», y agrupa los commits para subir una sola vez.
- **Dónde pasó:** clays-of-the-world, `CLAUDE.md` § Despliegue y
  `CHECKLIST.md` §13 (commit `41407f5`).

### La web aparece publicada en una cuenta personal de Vercel

- **Causa:** el CLI de Vercel que corre en la sesión de Claude vive dentro del
  contenedor de la app, con su propia sesión de Vercel, distinta de la de la
  terminal de Belén. Un `vercel deploy` desde ahí creó un proyecto duplicado en
  `belen-5889s-projects`.
- **Solución:** Claude no lanza `vercel deploy` ni `vercel --prod`. El
  duplicado se borra desde Settings → Delete Project (Belén).
- **Dónde pasó:** clays-of-the-world, `CLAUDE.md` § Despliegue y
  `CHECKLIST.md` §13 (31-08-2026). En xanatechnologies se encadenaron commit,
  push y `vercel --prod` sin permiso (memoria
  `feedback-no-commits-without-permission`, 09-09-2026).

### Un commit de Claude arrastra trabajo de Belén

- **Causa:** Belén edita en paralelo en su IDE, y `git add -A` metió sus
  cambios sin terminar en un commit con un mensaje que no era suyo.
- **Solución:** `git status` y `git add <rutas>` solo con lo que ha tocado
  Claude. Si un mismo fichero mezcla cambios de los dos, preguntar.
- **Dónde pasó:** xanasystem-web, memoria `git-add-selectivo` (junio 2026).

### El historial cuenta la conversación de diseño y no los cambios

- **Causa:** un commit por cada iteración de feedback: ocho en una tarde, tres
  contradiciéndose («pastilla de cristal claro», «devuelve la pastilla a
  cristal oscuro»…).
- **Solución:** un commit por **pieza terminada**, cuando Belén da el visto
  bueno, con el mensaje del estado final.
- **Dónde pasó:** clays-of-the-world, memoria `commits-por-pieza-terminada`
  (31-08-2026).

### Vercel no acepta el dominio: estaba vinculado a otra cuenta

- **Causa:** el dominio se había usado antes en otra cuenta de Vercel.
- **Solución:** Vercel pide un registro **TXT `_vercel`** de verificación;
  Belén lo crea en el DNS junto con los registros que indique Vercel. No borrar
  otros TXT de la zona (en xanatechnologies había de subdominios alojados en
  otro sitio).
- **Dónde pasó:** xanatechnologies, `CLAUDE.md` § Go-live status y memoria
  `xana-go-live` (09-09-2026).

---

## Verificación y herramientas

Lecciones de las herramientas de Claude (navegador integrado, preview),
no de las webs. **Verificar sin captura**: geometría con
`getBoundingClientRect`, peticiones directas al servidor, y el build.

### Las capturas se cuelgan o la pestaña se queda en `chrome-error://chromewebdata/`

- **Causa:** fallo del entorno de preview, no del código: la captura agota el
  tiempo y, si el servidor recompila mientras se navega, la pestaña no se
  recupera ni reiniciando.
- **Solución:** no reintentar. Verificar por el servidor:
  `Invoke-WebRequest http://localhost:<puerto>/<ruta>` y comprobar el 200 y el
  marcado; cuando el navegador responda, medir con `getBoundingClientRect`.
- **Dónde pasó:** xanasystem-web, memoria `preview-screenshot-roto`.

### Captura en blanco: la página parece vacía

- **Causa:** el navegador integrado arranca con el viewport a 0×0; el
  `IntersectionObserver` no dispara y todo lo que lleva reveal se queda a
  `opacity: 0`. Con el panel oculto, además, las transiciones no avanzan. Un
  `preset` de tamaño a veces también deja el ancho a 0.
- **Solución:** fijar el viewport con ancho y alto explícitos (p. ej.
  1280×900) antes de medir; para la captura, inyectar
  `[data-reveal]{opacity:1!important;translate:none!important}`.
- **Dónde pasó:** xanasystem-web, memorias `preview-viewport-fijo-980` y
  `preview-screenshot-roto`; APC-gestion-patrimonial, memoria
  `apc-estado-desarrollo`.

### El preview no conecta con el servidor de desarrollo

- **Causa:** otras sesiones ocupan 4321, 4322…; Astro arranca en el siguiente
  libre y el proxy del preview apunta a otro puerto.
- **Solución:** leer el puerto real en los logs (`Local http://localhost:<puerto>/`)
  y verificar contra ese puerto por HTTP directo, o pasarle esa URL a Belén.
- **Dónde pasó:** xanasystem-web, memoria `preview-puerto-4323`.

### `getComputedStyle` dice que un color no se ha aplicado y sí se ha aplicado

- **Causa:** en el preview, las lecturas de color, borde y sombra por
  `getComputedStyle` llegan obsoletas; las de `width` o `font-weight` sí son
  fiables. Los estados que dependen de `:checked` o de JavaScript tampoco se
  leen bien inspeccionando.
- **Solución:** usar `getComputedStyle` para geometría y propiedades que no
  son color; los colores estáticos, con la herramienta de inspección; los
  estados, razonando la regla y pidiendo a Belén que lo mire.
- **Dónde pasó:** xanasystem-web, memoria `preview-getcomputedstyle-color`.

### Un carrusel con flechas no se mueve en la prueba

- **Causa:** en el navegador de automatización `scrollBy({ behavior: 'smooth' })`
  no anima y `requestAnimationFrame` no dispara. Para un usuario real
  funciona.
- **Solución:** no «arreglarlo». Comprobar `overflow-x`, `scrollWidth >
  clientWidth` y que el listener está enganchado; el scroll instantáneo sí
  funciona para medir.
- **Dónde pasó:** xanasystem-web, memoria `preview-smooth-scroll-no-anima`.

### Chrome real no abre `localhost`

- **Causa:** el servidor de desarrollo corre dentro del entorno de Claude;
  desde el Chrome del equipo da `ERR_CONNECTION_REFUSED`.
- **Solución:** verificar con `npm run build` y peticiones HTTP desde la
  terminal; no contar con Chrome real como plan B.
- **Dónde pasó:** xanasystem-web, memoria `preview-viewport-fijo-980`.

### Un texto que está en la página «no aparece» al comprobarlo desde PowerShell

- **Causa:** las cadenas con acentos dan falso negativo en `.Contains()` por
  la codificación.
- **Solución:** buscar fragmentos sin acentos.
- **Dónde pasó:** xanasystem-web, memoria `preview-screenshot-roto`.

### Los colores y degradados copiados de Figma no cuadran

- **Causa:** se aproximaron a ojo desde la imagen.
- **Solución:** leer los valores literales del nodo (`get_design_context`)
  antes de escribir CSS, y verificar teniendo en cuenta que el CSS scoped
  puede estar desactualizado en `dev` (ver [Astro y build](#astro-y-build)).
- **Dónde pasó:** xanasystem-web, memoria `figma-valores-exactos` (sección
  «Cómo trabajamos» de `/empresa`).
