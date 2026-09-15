# Módulo · Migración desde web antigua

El cliente ya tiene una web publicada en el dominio y la nueva la sustituye:
inventario de URL antiguas, redirecciones 301, lo que se rescata antes de que
la antigua desaparezca y el cambio de DNS sin romper nada. Referencia:
**xanatechnologies** (venía de WordPress, publicada el 09-09-2026).

## Cuándo se usa (y cuándo no)

- **Sí:** el dominio sirve hoy otra web (pregunta 1.5 de
  [`cliente/arranque.md`](../plantillas/cliente/arranque.md)). Se sabe en el
  [arranque](../fases/00-arranque.md), porque hay que guardar cosas **antes**
  de publicar.
- **También** cuando una web nuestra renombra o retira páginas ya publicadas
  (APC `vercel.json`, xanasystem-web `/auditoria`): se aplica la parte de
  redirecciones → [multipágina](multipagina.md).
- **No:** dominio nuevo o sin web. Solo DNS → [publicación](../flujos/publicacion.md).
- Si la web antigua es un WordPress que **se mantiene**, no es migración →
  [fuera de la guía](../anexos/fuera-de-la-guia.md#webs-existentes-en-wordpress).

## Qué añade al código

### Redirecciones en `astro.config.mjs`

xanatechnologies lista las URL del sitemap antiguo y decide un destino para
cada una. No siempre hay equivalente 1:1:

```js
// 301s from the previous WordPress site (page-sitemap.xml, 2026-09-09). The old
// site had Spanish at the root and English under /en/; the new one is the
// opposite. Product pages belong to Xana System now — sent to the ES/EN home.
const OLD_ES_PAGES = ['/xana-technologies', '/kit-consulting', /* … */];
const OLD_EN_PAGES = ['/en/we-are-xana-technologies', /* … */];

const redirects = {
  '/en': '/',
  '/en/contact-us': '/#contact',
  '/contacto': '/es/#contact',
  '/aviso-legal': '/es/aviso-legal/',
  ...Object.fromEntries(OLD_ES_PAGES.map((p) => [p, '/es/'])),
  ...Object.fromEntries(OLD_EN_PAGES.map((p) => [p, '/'])),
};
```

Criterios que se usaron: página con equivalente → a su equivalente (legales,
contacto a su ancla); contenido que ya no está en esta web → a la portada del
mismo idioma; idiomas que cambian de raíz → también (`/en` → `/`).

### La barra final

Astro normaliza el origen a `^/old-url$` y WordPress enlazaba `/old-url/`, que
es lo que tienen Google y terceros. Un script tras el build hace que cada 301
acepte las dos formas (`scripts/fix-redirect-slashes.mjs`):

```js
const file = new URL('../.vercel/output/config.json', import.meta.url);
const config = JSON.parse(readFileSync(file, 'utf8'));
for (const route of config.routes ?? []) {
  if (route.status !== 301 || typeof route.src !== 'string') continue;
  const m = route.src.match(/^\^(\/[^$?]+?)\/?\$$/);
  if (!m) continue;
  route.src = `^${m[1]}/?$`;
}
writeFileSync(file, JSON.stringify(config, null, 2));
```

Y Vercel tiene que construir con ese script:

```json
// package.json
"build": "astro build && node scripts/fix-redirect-slashes.mjs && node scripts/format-sitemap.mjs"

// vercel.json
{ "framework": "astro", "buildCommand": "npm run build" }
```

Ver [trampas · Astro y build](../trampas-y-lecciones.md#astro-y-build) («Las
URL antiguas con barra final no redirigen»).

### Contenido retirado

Lo que no pasa a la web nueva se guarda en el repo sin publicar
(xanatechnologies: `docs/archive/kit-consulting/`) y su URL redirige.

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| Analítica | Si la web antigua tiene GTM/GA4, **se reutiliza el contenedor** (xanatechnologies conservó `GTM-NFJ2KXRX`) después de revisar qué etiquetas dispara → [cookies y analítica](../flujos/cookies-analitica.md) §2 |
| Legales | Los **datos del titular** de la web antigua sirven para pre-rellenar y se confirman; **tratamientos, proveedores y cookies no se copian** → [legales](../flujos/legales.md) (escenario «web antigua con legales») |
| Formulario | Sin cambios; los destinatarios de la antigua no se dan por buenos sin preguntar |
| SEO | Redirecciones de todas las URL del inventario; sitemap nuevo a Search Console; comprobar errores 404 después |
| Publicación | Cambio de DNS con cuidado de no borrar lo que no es de la web → [publicación](../flujos/publicacion.md) §2 y §4.3 |
| Idiomas | Si cambia qué idioma va en la raíz, redirigir los prefijos antiguos → [idiomas](idiomas.md) |

## Qué pedir al cliente de más

- **Acceso a lo que tenga**: dominio y quién gestiona el DNS, Search Console,
  Tag Manager y Analytics → [`cliente/accesos.md`](../plantillas/cliente/accesos.md).
- **Qué contenido de la web antigua desaparece** y si alguna página tiene que
  seguir existiendo.
- **Qué otros servicios cuelgan del dominio**: correo (MX), subdominios
  alojados en otro sitio, verificaciones TXT.

## Pasos y quién

1. **00 · Arranque — Claude**: inventario de URL desde el sitemap de la web
   antigua (en xanatechnologies, `page-sitemap.xml`) en
   `docs/00-arranque/`. Guarda también textos, imágenes, legales vigentes, ID
   del contenedor de GTM y de GA4. **La web antigua deja de verse en cuanto
   cambia el DNS** (xanatechnologies `CLAUDE.md`: «Old WordPress is no longer
   reachable on the domain»).
2. **02 · Estructura — Claude**: tabla URL antigua → destino, aprobada por
   Belén.
3. **07 · Construcción — Claude**: `redirects`, script de la barra final,
   `buildCommand`.
4. **08 · Revisión — Claude**: cada URL antigua, con y sin barra, da 301 al
   destino en la URL temporal.
5. **09 · Publicación — Belén**: DNS. Solo se tocan el A de `@` y el CNAME de
   `www`; si Vercel lo pide, TXT `_vercel`. En xanatechnologies el dominio
   estaba vinculado a otra cuenta de Vercel y hubo que añadirlo, sin borrar los
   TXT `_vercel` de subdominios alojados en otro sitio (memoria `xana-go-live`).
6. **Después — Claude y Belén**: redirecciones comprobadas en el dominio,
   sitemap enviado, 404 en Search Console.

## Reglas y trampas

- **Guardar antes de cambiar el DNS**: después no hay web antigua que consultar.
- **Toda URL del inventario tiene destino.** Una 404 en una URL que Google
  tiene indexada es tráfico perdido.
- **Con y sin barra final**: el script corre en el build de Vercel, no solo en
  local (`buildCommand`).
- **Redirigir al equivalente, no todo a la portada**, cuando el equivalente
  existe.
- **No borrar registros DNS ajenos a la web** (MX, TXT de otros servicios,
  subdominios) → [trampas · Git y publicación](../trampas-y-lecciones.md#git-y-publicación)
  («Vercel no acepta el dominio»).
- **Enlaces de la web nueva a la antigua**: revisar que ninguno apunte a
  rutas que dejan de existir (en xanatechnologies, `BRAND_URLS` apunta a
  `xanasystem.com/en/` → [deuda](../anexos/deuda-proyectos.md)).
- La redirección de `www` al dominio principal también es parte del cambio:
  en xanatechnologies quedó pendiente tras publicar.

## Checklist

**Antes de construir**
- [ ] Inventario de URL antiguas guardado en `docs/00-arranque/`
- [ ] Textos, imágenes, legales vigentes y IDs de GTM/GA4 guardados
- [ ] Registros DNS actuales anotados (MX, TXT, subdominios)
- [ ] Tabla URL antigua → destino aprobada

**Construcción y revisión**
- [ ] `redirects` en `astro.config.mjs` con todas las URL del inventario
- [ ] `scripts/fix-redirect-slashes.mjs` en `build` y `buildCommand` en `vercel.json`
- [ ] En la URL temporal, cada URL antigua con y sin barra → 301 al destino
- [ ] Contenido retirado archivado en `docs/archive/`
- [ ] Contenedor de GTM revisado antes de reutilizarlo

**Publicación y después**
- [ ] Solo A de `@`, CNAME de `www` y, si hace falta, TXT `_vercel`
- [ ] `www` redirige al principal
- [ ] Redirecciones comprobadas en el dominio
- [ ] Sitemap nuevo en Search Console; 404 revisados a los pocos días

## Proyecto de referencia

**xanatechnologies**: `astro.config.mjs` (`redirects`),
`scripts/fix-redirect-slashes.mjs`, `package.json` (`build`), `vercel.json`,
`docs/archive/kit-consulting/`, `CLAUDE.md` § Go-live status, memoria
`xana-go-live` (09-09-2026). Redirecciones de páginas renombradas: APC
`vercel.json` y xanasystem-web `astro.config.mjs`.
