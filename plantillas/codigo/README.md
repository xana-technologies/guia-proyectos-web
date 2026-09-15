# Plantillas · Código

El código de la base: lo que lleva cualquier web de Xana antes de empezar a
maquetar. Se usa en el [arranque](../../flujos/arranque-proyecto.md) para crear
el proyecto y en la [construcción](../../fases/07-construccion.md) para montar
cada pieza.

> **Hoy no hay repo plantilla.** Se creará en GitHub, organización
> `xanasystem`, cuando la guía esté redactada. Mientras tanto, cada pieza se
> copia del proyecto de referencia que indica la tabla. Rutas comprobadas el
> 15-09-2026; clays-of-the-world se edita en paralelo, así que **antes de
> copiar se comprueba que la ruta sigue existiendo**.

## Cómo se usa

**Mañana (repo plantilla):** Belén crea el repositorio desde la plantilla («Use
this template»), Claude lo clona, cambia los datos del proyecto (§ Qué se
adapta siempre) y borra lo que el proyecto no use.

**Hoy (copiando):**

1. `npm create astro@latest` con la plantilla mínima e instalar las versiones
   del [flujo de arranque §3](../../flujos/arranque-proyecto.md#3-stack-y-versiones).
2. Copiar cada pieza de la tabla y adaptarla. Clays va en **Astro 5**: se copian
   los ficheros, **no** su `package.json`.
3. Cambiar los imports relativos por el alias `@/` al copiar (clays usa rutas
   relativas aunque su `tsconfig.json` declara alias que no usa).
4. Buscar restos del proyecto de origen (§ Qué se adapta siempre).
5. `npm run build` y `npm run check`.

## Qué trae

### Configuración

| Pieza | Copiar de | Qué adaptar |
| --- | --- | --- |
| `package.json` | Versiones y `engines`: `xanatechnologies/package.json` · scripts: `clays-of-the-world/package.json` | Nombre; dependencias según el flujo de arranque |
| `astro.config.mjs` | `clays-of-the-world/astro.config.mjs` | `site`, `security.allowedDomains` (dominio, `www`, URL temporal) |
| `tsconfig.json` | `APC-gestion-patrimonial/tsconfig.json` | — (alias `@/*`) |
| `.gitignore` | `clays-of-the-world/.gitignore` + `*.mp4` de `magnaceramica/.gitignore` | Añadir `docs/imagenes/originales/` y `.claude/settings.local.json` (ver flujo §6) |
| `.editorconfig` | `clays-of-the-world/.editorconfig` | — |
| `.gitattributes` | `clays-of-the-world/.gitattributes` | — |
| `vercel.json` | `clays-of-the-world/vercel.json` (igual en `xanatechnologies/vercel.json`) | — |
| `.env.example` | `clays-of-the-world/.env.example` | Quitar `info@clays.es` y nombres de Clays; comentarios de dominio |
| `.claude/launch.json` | Flujo de arranque §6 (formato de `clays-of-the-world/.claude/launch.json`) | Nombre del proyecto |

### Diseño y estructura

| Pieza | Copiar de | Qué adaptar | Flujo |
| --- | --- | --- | --- |
| `src/styles/tokens.css` · `reset.css` · `base.css` | `clays-of-the-world/src/styles/` | La paleta y la tipografía son de Clays: se vacía el bloque de marca y se rehace | [design system](../../flujos/design-system.md) |
| `src/layouts/Base.astro` | `clays-of-the-world/src/layouts/Base.astro` | Metadatos, favicon, `lang` por defecto | [SEO técnico](../../flujos/seo-tecnico.md) |
| Componentes base: `Button`, `Logo`, `Section`, `SectionHeader`, `Arrow`, `SocialIcon`, `StatusMessage` | `clays-of-the-world/src/components/` | Estilos a los tokens del proyecto | [design system](../../flujos/design-system.md) |
| `Pending.astro` (marca `[[ ]]`) | `clays-of-the-world/src/components/Pending.astro` | — | [legales](../../flujos/legales.md) |
| `Header.astro` · `Footer.astro` (pie con los 4 elementos legales) | `clays-of-the-world/src/components/` | Composición del proyecto; conservar los 4 enlaces | [legales](../../flujos/legales.md) |
| Moldes de sección (`Band`, `Showcase`, `Editorial`…) | `clays-of-the-world/src/components/sections/` | Solo como punto de partida; los moldes se deciden por proyecto | [design system](../../flujos/design-system.md) |
| Contenido como datos: `index.ts`, `types.ts`, un fichero por idioma | `clays-of-the-world/src/data/` (`index.ts`, `types.ts`, `en.ts`, `es.ts`) | Vaciar el contenido; conservar el patrón | [maquetación](../../flujos/maquetacion.md) |
| `/styleguide` | `clays-of-the-world/src/pages/styleguide.astro` | Secciones según las piezas del proyecto; `noindex, nofollow` | [design system](../../flujos/design-system.md) |
| `404.astro` | `clays-of-the-world/src/pages/404.astro` | Textos desde los datos; en una web de un solo idioma puede ser estática | [SEO técnico](../../flujos/seo-tecnico.md) |
| `robots.txt.ts` · `sitemap.xml.ts` | `clays-of-the-world/src/pages/` | Lista de páginas indexables | [SEO técnico](../../flujos/seo-tecnico.md) |

### Formulario

| Pieza | Copiar de | Qué adaptar |
| --- | --- | --- |
| `src/pages/api/contact.ts` | `clays-of-the-world/src/pages/api/contact.ts` | Ruta de gracias, campos |
| `src/form.config.ts` | `clays-of-the-world/src/form.config.ts` | **Cambiar `??` por `||`** (clays aún tiene `??`; referencia con `||`: `xanatechnologies/src/form.config.ts`) |
| `src/lib/email/contact-notification.ts` | `clays-of-the-world/src/lib/email/contact-notification.ts` | Colores, logo, textos; para Outlook y modo oscuro, `xanatechnologies/src/pages/api/contact.ts` |
| `src/components/sections/Contact.astro` | `clays-of-the-world/src/components/sections/Contact.astro` | Estilos y textos |
| Página de gracias | `clays-of-the-world/src/pages/[...lang]/thank-you.astro` | `/gracias/` en español; sin `[...lang]` si hay un solo idioma |

Detalle: [flujo de formulario](../../flujos/formulario.md).

### Legales, cookies y analítica

| Pieza | Copiar de | Qué adaptar |
| --- | --- | --- |
| `src/data/legal/` (`entity.ts`, `types.ts`, `index.ts`, `en.ts`, `es.ts`) | `clays-of-the-world/src/data/legal/` | `entity.ts` con los datos del cliente o `null`; textos desde `plantillas/legales/` |
| `LegalDocument.astro` · `LegalText.astro` | `clays-of-the-world/src/components/` | — |
| `src/layouts/Legal.astro` | `clays-of-the-world/src/layouts/Legal.astro` | `robots` a `noindex, follow` (clays tiene `nofollow`, en deuda) |
| Páginas legales | `clays-of-the-world/src/pages/[...lang]/legal-notice.astro`, `privacy-policy.astro`, `cookies-policy.astro` | En español: `aviso-legal`, `politica-de-privacidad`, `politica-de-cookies` |
| `CookieConsent.astro` | `clays-of-the-world/src/components/CookieConsent.astro` | `GTM_ID` (quitar el de clays), textos, variables `--cc-*` |

Detalle: [legales](../../flujos/legales.md) · [cookies y analítica](../../flujos/cookies-analitica.md).

### Imágenes y vídeo

| Pieza | Copiar de | Qué adaptar |
| --- | --- | --- |
| Arranque de vídeo en pantalla, pausa, `prefers-reduced-motion` | `clays-of-the-world/src/scripts/video-autoplay.ts` | — |
| URL de vídeos centralizadas (R2) | `magnaceramica/src/config/media.ts` | URL base del proyecto |
| Optimizar imágenes a WebP | `magnaceramica/scripts/convert-to-webp.mjs` (Node + `sharp`) · `clays-of-the-world/scripts/optimizar-imagenes.py` (Python + Pillow) | Rutas y anchos objetivo |
| Imagen Open Graph 1200×630 | `xanatechnologies/scripts/make-og-image.mjs` (`sharp`) · `clays-of-the-world/scripts/generar-og.py` (Pillow) | Foto y logo |
| Favicon (`.svg`, `.ico`, `apple-touch-icon.png`) | `clays-of-the-world/scripts/generar-favicon.py` (Pillow) | Emblema del cliente |

Detalle: [imágenes](../../flujos/imagenes.md) · [vídeos](../../flujos/videos.md).

### Solo con módulos

| Pieza | Módulo | Copiar de |
| --- | --- | --- |
| Rutas por idioma con `getStaticPaths` | [idiomas](../../modulos/idiomas.md) | `clays-of-the-world/src/pages/[...lang]/` y `langStaticPaths()` en `src/data/index.ts` |
| Selector de idioma | [idiomas](../../modulos/idiomas.md) | `clays-of-the-world/src/components/LangSwitcher.astro`, `LangMenu.astro` |
| Visor de galería | [multimedia](../../modulos/multimedia.md) | `clays-of-the-world/src/components/Lightbox.astro`, `src/pages/[...lang]/gallery.astro` |
| Scroll suave | [movimiento avanzado](../../modulos/movimiento-avanzado.md) | `xanatechnologies/src/components/SmoothScroll.astro` |
| Pieza animada con GSAP | [movimiento avanzado](../../modulos/movimiento-avanzado.md) | `xanasystem-web/src/components/ui/HeroReel.astro` |
| Redirecciones con barra final | [migración](../../modulos/migracion.md) | `xanatechnologies/scripts/fix-redirect-slashes.mjs` |

## Qué no trae

- Páginas y componentes de prueba (`clays-of-the-world/src/pages/pruebas-*`,
  `src/components/pruebas/`, `src/data/pruebas/`).
- Imágenes, vídeos, logos y textos de otros proyectos.
- Secciones propias de un cliente (`Event.astro` de TECNA, `Products.astro`).

## Qué se adapta siempre

Buscar en todo el proyecto antes del primer commit:

| Buscar | Por qué |
| --- | --- |
| `claysoftheworld`, `clays`, `Clays of the World` | Dominio, correos y marca del proyecto de origen |
| `GTM-` | ID de contenedor de otro cliente |
| `forms.xanasystem.com` | Remitente no verificado; va `no-reply@xanatechnologies.com` |
| `??` en `form.config.ts` | Tiene que ser `||` |
| `site:` y `allowedDomains` | Dominios del proyecto |
| `[[ ]]` y `placeholder` | Datos de prueba que no se publican |

## Decisiones (15-09-2026)

- **Idiomas: depende del proyecto.** La plantilla es de **un solo idioma**. Si
  el briefing especifica varios idiomas, se prepara con el módulo
  [idiomas](../../modulos/idiomas.md) desde el principio.
- **Scripts de imágenes en Node + `sharp`** (no hace falta instalar nada más
  que el proyecto). Los de clays en Python quedan como referencia de
  parámetros (anchos, calidades, recorte de la imagen OG, favicon) para
  portarlos.
- **`.claude/launch.json` va en el repo**, para que el preview de Claude arranque
  sin configurarlo; `.claude/settings.local.json` se ignora.

## Pendiente para el repo plantilla

- [ ] Portar a Node + `sharp` los scripts de clays (`optimizar-imagenes.py`,
      `generar-og.py`, `generar-favicon.py`), sin ampliar imágenes y corrigiendo
      la orientación EXIF
- [ ] Pasar la base de clays a Astro 7 + `@astrojs/vercel` 11 y comprobar build
- [ ] Imports con `@/`
- [ ] Documentar aquí cómo se actualiza la plantilla cuando cambia la guía

## Fuentes

Rutas comprobadas en disco el 15-09-2026 en clays-of-the-world (rama
`feature/web-astro`), xanatechnologies, magnaceramica, APC-gestion-patrimonial
y xanasystem-web.
