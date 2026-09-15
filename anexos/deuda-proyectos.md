# Anexo · Deuda de los proyectos

Lo que está pendiente o mal hecho en los proyectos actuales, según el análisis
hecho al redactar la guía (14 y 15-09-2026). **No se toca ningún proyecto sin
permiso**: esto es una lista para decidir qué se arregla y cuándo. Cada punto
remite al flujo o trampa que explica cómo debería ser.

## magnaceramica

- **SEO**: sin `site`, canonical, Open Graph, sitemap, robots ni JSON-LD → [SEO técnico](../flujos/seo-tecnico.md).
- **Analítica**: `dataLayer.push` con array; los comandos no se ejecutan → [cookies y analítica](../flujos/cookies-analitica.md).
- **Legales**: la política de cookies declara `_gid`, `_gat` y Google Maps, que la web no usa; privacidad cita Mailchimp y no Brevo, Cloudflare ni Vercel → [legales](../flujos/legales.md).
- **Formulario**: el servidor no comprueba la casilla de privacidad; sin JavaScript no envía; campos sin `<label>` visible; remitente `forms.xanasystem.com` → [formulario](../flujos/formulario.md).
- **Fuentes**: Google Fonts por `<link>` en `src/layouts/Layout.astro` y `LegalLayout.astro`.
- **Vídeo del hero** con `preload="auto"`.
- `docs/forms/`: marcar como obsoleto y enlazar a la guía.

## xanasystem-web

- **Legales y cookies**: no hay páginas legales ni muro; enlaces a `/legal/*`, `/privacidad` y `/acceso-clientes` que no existen.
- **SEO**: JSON-LD con `Offer` a precio 0 e `InStock`; `/styleguide` y `/demo-tabs` fuera del sitemap pero **sin `noindex`**.
- **Formulario**: `form.config.ts` con `??`.
- **CSS**: tokens de espaciado inexistentes en `PimSignals`, `DamSignals`, `DamCapabilities`.
- **Documentación**: `docs/design.md` (estado, valores de CTA, ruta de referencias) y `docs/brand.md` (Roboto, CTA rosa) desactualizados; el brief cita `/auditoria`, retirada.
- **Repositorio** en la cuenta personal `belenXanaSystem`, no en `xanasystem`.
- **Formulario `/demo`**: `placeholder` en vez de etiquetas visibles, errores como frases, sin 405 a `GET`, no funciona sin JavaScript (paso 2 oculto, sin 303), éxito en la propia página en vez de `/gracias`, enlace a `/privacidad` inexistente → [formulario](../flujos/formulario.md).

## APC-gestion-patrimonial

- **SEO**: sin sitemap, robots ni `og:image`; `noindex, nofollow` en legales e `informacion-financiera` (la guía pide `noindex, follow`).
- **Formulario** sin conectar; faltan `/gracias/` y 404.
- **Páginas de laboratorio** publicadas (`estilo`, `demo-color`, `pruebas-header-contacto`).
- **CSS**: escala de espaciado con huecos (`--sp-7`, `--sp-9`, `--sp-11` no existen); el código de `Cierre.astro` copiado en cuatro ficheros; la sección `Momento` sin `h2`.
- **Documentación**: `CLAUDE.md` habla de auto-deploy en cada push a `main` (contradice que publica Belén); `docs/GUIA-DE-ESTILO.md` da el logo como pendiente y pone textos como arrays en `.astro`.

## festival-viento

- **Cookies**: embeds de Vimeo (`Caracteristicas.astro`) y Google My Maps (`Mapa.astro`) sin consentimiento, contra su propio `CLAUDE.md`.
- **SEO**: `og:image` relativa, sin canonical ni `site`.
- **Imágenes**: originales muy pesados en `src/assets`; favicon de 691 KB; `cartel.jpg` duplicado.
- **Fuentes**: Google Fonts por `@import` en `src/styles/global.css`.
- **Datos**: fechas incoherentes en `site.ts`; `.ics` sin zona horaria, solo escapa `DESCRIPTION` y su `UID` usa `festivaldelvientocastellon.es` en vez de `festivalviento.com`; `astro.config.mjs` vacío.
- **Base**: su `CLAUDE.md` pide cero cookies y evitar legales; la guía dice que toda web publicada lleva la base. Decidir.
- **Vídeo del hero** con `preload="auto"`.
- **Documentación**: `CLAUDE.md` regla 4 dice md/yaml/json y el contenido es TypeScript; `docs/referencias/` sin notas.
- **Repositorio** en la cuenta personal `belenXanaSystem`.

## clays-of-the-world

- **Versiones**: Astro 5 con `@astrojs/vercel` 8 (la guía fija Astro 7); `form.config.ts` con `??`.
- **Vercel**: proyecto duplicado en la cuenta personal pendiente de borrar.
- **Páginas de prueba**: cuatro `/pruebas-*` publicadas; `npm run check` con errores.
- **SEO**: `noindex, nofollow` en legales frente a `noindex, follow` documentado; `seo-metadatos.md` se contradice sobre `/es/`; el comentario de `sitemap.xml.ts` y `CHECKLIST.md` §11 siguen diciendo «solo inglés».
- **Documentación**: `CLAUDE.md` cita el molde `Feature`, que no existe (el styleguide lista nueve moldes); varios documentos dicen «solo en inglés» con el español ya en marcha; dos secciones seguidas con fondo alternativo sin resolver.
- **Scripts**: `optimizar-imagenes.py` amplía por encima de ×1,15 y no corrige la orientación EXIF.
- **Alias** de `tsconfig.json` declarados y sin usar; remoto por HTTPS en vez del alias SSH.

## xanatechnologies

- **Pendientes de salida**: redirección `www`, prueba real del formulario, regla de firewall, Search Console, GA4.
- **Vídeos en git**: `public/video/hero-shrink.mp4` (10,4 MB) y `contact.mp4` (3,5 MB) → [vídeos](../flujos/videos.md).
- **SEO**: `public/robots.txt` bloquea `/v2/`, que lleva `noindex` (el robot no puede leerlo).
- **Legales**: la política de cookies se trasladó desde el WordPress antiguo; revisarla contra lo que carga la web → [legales](../flujos/legales.md).
- `BRAND_URLS` apunta a `xanasystem.com/en/`, que la nueva web no tendrá; `README.md` de plantilla de Astro; remoto por HTTPS.

## grupomerhi

- Sin legales, cookies ni SEO; `lang="es"` con contenido en inglés; foco sin contorno visible; «Decker» sin enlace; vídeo con `preload="auto"` también en móvil.

## En varios proyectos

- **Ningún `CLAUDE.md` tiene enlace a Figma** (lo pide la fase 06).
- **URLs de R2 escritas a mano** en clays, APC y festival en vez de una URL base centralizada.
- `README.md` de plantilla de Astro en APC, festival y xanatechnologies.
- `claysoftheworld`: carpeta vacía, se puede borrar.
