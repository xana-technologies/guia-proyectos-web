# Flujo · Arranque de proyecto

Pasar de un briefing a un proyecto listo para trabajar: qué tipo de web es, qué
trae el cliente, qué se le pide, con qué se construye y dónde queda cada cosa.
Lo hace Claude al empezar **cualquier** web, sin que haya que pedírselo.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Todo este flujo |
| [07 · Construcción](../fases/07-construccion.md) | Se comprueba que la URL temporal sigue viva y que las versiones no se han movido |
| [10 · Entrega y mantenimiento](../fases/10-entrega-mantenimiento.md) | Al actualizar dependencias, se vuelven a mirar las parejas de versión (§3) |

## 2. Qué tenemos

### Qué se saca del briefing

| Qué | Dónde se mira | Qué decide | Dónde se apunta |
| --- | --- | --- | --- |
| **Tipo de proyecto** | Briefing, cuestionario §1 | Receta de [`tipos-de-proyecto/`](../tipos-de-proyecto/README.md): base + módulos + estructura por defecto | `CLAUDE.md` · `CHECKLIST.md` |
| **Módulos** | Idiomas, número de páginas, blog, evento, vídeos, animación, formularios especiales | Fichas de [`modulos/`](../modulos/README.md) que se añaden | `CLAUDE.md` · bloque de módulos de `CHECKLIST.md` |
| **Escenario de cada fase** | Cuestionario §2 (tabla «lo tengo completo / algo / no») | 🟢🟡🔴 de las fases 01, 02, 03, 04, imágenes y vídeos; caso A/B/C de Figma | Tabla de escenarios de `CLAUDE.md` y `CHECKLIST.md` |
| **Web antigua** | Cuestionario §1.5, dominio | Módulo [migración](../modulos/migracion.md); rescatar GTM/GA y datos legales | `CLAUDE.md` · `docs/00-arranque/urls-web-antigua.md` |
| **Accesos** | [`cliente/accesos.md`](../plantillas/cliente/accesos.md) | Quién toca el DNS, si se reutiliza GTM/GA4 | `CLAUDE.md` § Enlaces y cuentas |

Qué hacer con cada respuesta está al final de cada plantilla del cliente
(«*Para Claude*»). Aquí no se repite.

### Situaciones

| Situación | Qué cambia |
| --- | --- |
| **Proyecto nuevo, repo vacío** (lo normal) | Todo el flujo, se trabaja en `main` |
| **El repo ya tiene algo publicado en `main`** (una *coming soon* en otra tecnología) | Preguntar a Belén antes de tocar `main`. En clays se trabajó en `feature/web-astro` hasta el lanzamiento (clays `CHECKLIST.md` cabecera y `CLAUDE.md` § Git) |
| **El dominio tiene una web antigua** | Inventario de URLs desde el arranque → [migración](../modulos/migracion.md) |
| **El cliente ya tiene GTM, GA4 o Search Console** | Se pide acceso y se reutiliza → [cookies y analítica](cookies-analitica.md) |
| **No encaja en ningún tipo** | Base + los módulos que toquen |
| **No sigue «Astro + base»** (prototipo, WordPress existente, aplicación) | Fuera de este flujo → [`anexos/fuera-de-la-guia.md`](../anexos/fuera-de-la-guia.md) |

## 3. Stack y versiones

### Versiones

| Pieza | Versión | Fuente |
| --- | --- | --- |
| Astro | **7** | `xanatechnologies/package.json` (`^7.1.6`), `APC-gestion-patrimonial/package.json`, `magnaceramica/package.json` |
| `@astrojs/vercel` | **11** | `xanatechnologies/package.json` (`^11.0.10`), `magnaceramica/package.json` |
| Node | **≥ 22.12** (`"engines": { "node": ">=22.12.0" }`) | `xanatechnologies`, `APC-gestion-patrimonial`, `magnaceramica` (`package.json`) |

**Parejas del adaptador**: `@astrojs/vercel` **8 ↔ Astro 5 · 10 ↔ Astro 6 ·
11 ↔ Astro 7**. `npm install @astrojs/vercel` trae la última y no casa con una
Astro anterior ([trampa](../trampas-y-lecciones.md#astrojsvercel-no-instala-o-no-compila-con-la-versión-de-astro)).
Hoy clays-of-the-world va en 5/8 y xanasystem-web en 6/10: **al copiar de
ellos no se copian sus versiones**.

El adaptador va siempre: la base lleva formulario, y `src/pages/api/contact.ts`
es la única ruta en servidor. El resto de la web sigue siendo estática
(clays `astro.config.mjs`).

### Dependencias

| | Paquete | Para qué · condición | Fuente |
| --- | --- | --- | --- |
| **Sí** | `vanilla-cookieconsent` | Muro de cookies de la base | clays, magnaceramica, xanatechnologies (`package.json`) |
| **Sí** | `@fontsource/<fuente>` o `@fontsource-variable/<fuente>` | Tipografías alojadas en la propia web | clays (`@fontsource-variable/montserrat`), APC (`@fontsource-variable/instrument-sans`, `@fontsource/spectral`) |
| **Sí** | `sharp`, **solo en `scripts/`** | Optimizar lo que no pasa por `astro:assets`, imagen OG. No se añade a `package.json`: Astro 7 lo trae como dependencia opcional | `magnaceramica/scripts/convert-to-webp.mjs`, `xanatechnologies/scripts/make-og-image.mjs`; `astro` 7.1.6 `optionalDependencies` |
| **Sí** | `@astrojs/check` + `typescript` (desarrollo) | `npm run check` | clays (`devDependencies`) |
| **Con condiciones** | `@astrojs/sitemap` | Solo si el sitemap propio (`src/pages/sitemap.xml.ts`) no compensa, p. ej. muchas páginas con `i18n` de Astro. Genera `sitemap-index.xml`, no `sitemap.xml` → [SEO técnico](seo-tecnico.md) | xanatechnologies (`astro.config.mjs`); clays lo quitó (`CHECKLIST.md` §6) |
| **Con condiciones** | `gsap` | Solo para una pieza compleja que CSS no resuelve → [movimiento avanzado](../modulos/movimiento-avanzado.md) | `xanasystem-web/src/components/ui/HeroReel.astro` |
| **Con condiciones** | `lenis` | Solo con el módulo [movimiento avanzado](../modulos/movimiento-avanzado.md); modales con `data-lenis-prevent` | `xanatechnologies/src/components/SmoothScroll.astro` |
| **No** | Tailwind | CSS propio con tokens | APC `CLAUDE.md` § Stack; magnaceramica `CLAUDE.md` § Conventions |
| **No** | React, Vue u otro framework de interfaz | Componentes `.astro` | magnaceramica `CLAUDE.md` («no UI framework»); clays sustituyó su *coming soon* en React + Vite |
| **No** | Librerías de UI, de iconos o de animación | Iconos como componentes SVG propios (clays `Arrow.astro`, `SocialIcon.astro`); animación con CSS | — |
| **No** | Google Fonts por `@import` o `<link>` | Envía la IP del visitante a Google antes del consentimiento y añade una petición externa. Se usa fontsource | Decisión de la guía ([design system](design-system.md)); lo hacen aún `magnaceramica/src/layouts/Layout.astro` y `festival-viento/src/styles/global.css` |

Cualquier otra dependencia se **propone a Belén antes de instalarla**.

## 4. Pasos y quién

### 4.1 Leer y decidir — Claude

1. Leer el briefing, el cuestionario devuelto si ya lo hay y la web antigua si
   existe.
2. Escribir `docs/00-arranque/brief.md`: qué es la empresa, a quién va la web,
   qué tiene que conseguir, idiomas, tipo de proyecto y módulos.
3. Rellenar la tabla de escenarios (§2) con lo que se sabe; lo que no, «por
   confirmar con el cuestionario».
4. Si hay web antigua: inventario de sus URLs en
   `docs/00-arranque/urls-web-antigua.md` (sitemap antiguo, Search Console) y
   anotar GTM/GA y textos legales existentes.

### 4.2 Kit de arranque — Claude prepara, Belén envía

Claude copia las plantillas (tabla de §5), pone el nombre del cliente y
**pre-rellena solo lo que ya se sabe, marcado para confirmar**. Belén revisa y
envía al cliente las cinco del cliente juntas, y a ingeniería el alta de
formulario.

### 4.3 Repositorio — Belén y Claude

1. **Belén** crea el repositorio vacío en la organización **`xanasystem`** de
   GitHub. Nombre en minúsculas con guiones (`apc-gestion-patrimonial`,
   `clays-of-the-world`).
2. **Claude** crea la carpeta `C:\Proyectos\<repo>` y el proyecto:
   - **Hoy, sin repo plantilla**: `npm create astro@latest` con la plantilla
     mínima (de ahí sale la forma de `package.json` de APC y magnaceramica),
     fijar versiones (§3) y copiar las piezas de la base de los proyectos de
     referencia según [`plantillas/codigo/`](../plantillas/codigo/README.md).
   - **Cuando exista el repo plantilla** en `xanasystem`: Belén crea el
     repositorio desde él («Use this template») y Claude lo clona.
3. **Claude** deja la configuración (§6): `package.json` con `engines`,
   `tsconfig.json` con el alias, `.gitignore`, `.editorconfig`,
   `.gitattributes`, `vercel.json`, `.env.example`, `.claude/launch.json`.
4. **Claude** comprueba `npm run build` y `npm run check`.
5. **Claude** inicia git en `main` y añade el remoto con el alias SSH:

   ```
   git init -b main
   git remote add origin git@github-belenxana:xanasystem/<repo>.git
   ```

   El alias está en `~/.ssh/config` del equipo de Belén:

   ```
   Host github-belenxana
       HostName github.com
       User git
       IdentityFile ~/.ssh/id_ed25519_belenxana
       IdentitiesOnly yes
   ```

   `git@github.com` usa otra clave con una cuenta sin acceso a `xanasystem`
   ([trampa](../trampas-y-lecciones.md#permission-denied-al-hacer-push)).
6. **Claude** propone el primer commit y el push; los hace **con el sí de Belén**
   ([forma de trabajo](../forma-de-trabajo.md)).

### 4.4 Vercel desde el primer día — Belén

1. Crear el proyecto en el equipo **Xana Technologies' projects**, conectado al
   repositorio, rama de producción `main`.
2. Hacer el commit manual desde GitHub con la cuenta `xanasystem`: es lo que
   lanza el primer despliegue. Vercel rechaza los commits de
   `belen@xanasystem.com` (clays `CLAUDE.md` § Despliegue).
3. Pasar a Claude la **URL temporal** (`*.vercel.app`).
4. Tras el primer despliegue, comprobar en Settings → Functions que la región
   es europea (`fra1`, de `vercel.json`).

Después, **Claude** la anota en `CLAUDE.md` y `CHECKLIST.md` y la añade a
`security.allowedDomains` cuando monte el formulario
([formulario](formulario.md)). Claude **no** ejecuta `vercel link`,
`vercel deploy` ni `vercel --prod`: el CLI de su entorno tiene otra sesión y ya
publicó una vez en una cuenta personal
([trampa](../trampas-y-lecciones.md#la-web-aparece-publicada-en-una-cuenta-personal-de-vercel)).

### 4.5 Documentación del proyecto — Claude

1. `CLAUDE.md` desde [la plantilla](../plantillas/proyecto/CLAUDE.md): tipo,
   módulos, escenarios, dominio, cuentas, reglas de la base, tono si ya se sabe,
   cuestiones abiertas.
2. `CHECKLIST.md` desde [la plantilla](../plantillas/proyecto/CHECKLIST.md),
   con el bloque de cada módulo.
3. Carpetas de `docs/` y `brand/` según
   [`docs-estructura.md`](../plantillas/proyecto/docs-estructura.md).
4. Inventario de imágenes empezado con lo que ya haya llegado.
5. Commit con permiso.

### 4.6 Cuentas internas

| Qué | Cuál | Fuente |
| --- | --- | --- |
| Organización de GitHub | `xanasystem` | Remotos de clays, APC, magnaceramica, xanatechnologies |
| Acceso SSH | Alias `github-belenxana` → `~/.ssh/id_ed25519_belenxana` | `~/.ssh/config`; remotos de APC y magnaceramica |
| Identidad de los commits | `belen@xanasystem.com` (no despliega) | clays `CLAUDE.md` § Despliegue |
| Equipo de Vercel | *Xana Technologies' projects* | clays `CHECKLIST.md` §13 |
| Cuenta que despliega | `xanasystem` (en Vercel, `tech-8916`) | clays `CHECKLIST.md` §13 |
| Brevo | Una clave por proyecto, la da ingeniería | [formulario](formulario.md) |
| Cloudflare | Widget de Turnstile por proyecto; vídeos en R2 | [formulario](formulario.md) · [vídeos](videos.md) |
| GTM · GA4 · Search Console | Del cliente (con acceso) o creados por Belén | [cookies y analítica](cookies-analitica.md) |

## 5. Plantillas

### Kit de arranque

| Plantilla de la guía | Se copia a | Para | Lo envía |
| --- | --- | --- | --- |
| [`proyecto/CLAUDE.md`](../plantillas/proyecto/CLAUDE.md) | `CLAUDE.md` | Claude | — |
| [`proyecto/CHECKLIST.md`](../plantillas/proyecto/CHECKLIST.md) | `CHECKLIST.md` | Claude, Belén | — |
| [`proyecto/docs-estructura.md`](../plantillas/proyecto/docs-estructura.md) | Carpetas de `docs/` y `brand/`, `docs/README.md` | Claude | — |
| [`proyecto/inventario-imagenes.md`](../plantillas/proyecto/inventario-imagenes.md) | `docs/imagenes/inventario.md` | Claude | — |
| [`cliente/arranque.md`](../plantillas/cliente/arranque.md) | `docs/00-arranque/cuestionario-arranque.md` | Cliente | Belén |
| [`cliente/accesos.md`](../plantillas/cliente/accesos.md) | `docs/00-arranque/accesos.md` | Cliente | Belén |
| [`cliente/material.md`](../plantillas/cliente/material.md) | `docs/00-arranque/material.md` | Cliente | Belén |
| [`cliente/formulario.md`](../plantillas/cliente/formulario.md) | `docs/00-arranque/formulario.md` | Cliente | Belén |
| [`cliente/datos-legales.md`](../plantillas/cliente/datos-legales.md) | `docs/03-contenidos/legal/datos-legales.md` | Cliente | Belén |
| [`equipo/alta-formulario.md`](../plantillas/equipo/alta-formulario.md) | `docs/00-arranque/alta-formulario.md` | Ingeniería | Belén |

### Se preparan más adelante

| Plantilla | Se copia a | Fase |
| --- | --- | --- |
| [`equipo/peticion-seo.md`](../plantillas/equipo/peticion-seo.md) | Se envía a Eva | 03 |
| [`proyecto/busqueda-stock.md`](../plantillas/proyecto/busqueda-stock.md) | `docs/imagenes/busqueda-stock.md` | 03 |
| [`proyecto/referencias.md`](../plantillas/proyecto/referencias.md) | `docs/05-referencias-visuales/referencias.md` | 05 |
| [`plantillas/codigo/`](../plantillas/codigo/README.md) | Código de la base | 00 (proyecto) y 07 (piezas) |

## 6. Dónde queda cada cosa

### Raíz del proyecto

```
<repo>/
├── CLAUDE.md               ← reglas y datos del proyecto
├── CHECKLIST.md            ← estado real, fase a fase
├── package.json            ← versiones de §3 y "engines"
├── astro.config.mjs        ← site, adaptador, security.allowedDomains
├── tsconfig.json           ← alias @/
├── vercel.json
├── .env.example            ← variables sin valores secretos (sí va en git)
├── .gitignore · .editorconfig · .gitattributes
├── .claude/launch.json     ← servidores para el preview de Claude
├── brand/                  ← identidad (fase 04)
├── docs/                   ← ver plantillas/proyecto/docs-estructura.md
├── public/                 ← favicon.svg, favicon.ico, apple-touch-icon.png, og-image.jpg
├── scripts/                ← optimizar imágenes, imagen OG, favicon
└── src/
```

### `src/`

```
src/
├── assets/<seccion>/       ← imágenes que optimiza astro:assets
├── components/             ← Button, Logo, Section, SectionHeader, Pending, Header, Footer,
│   │                          CookieConsent, LegalDocument, LegalText…
│   └── sections/           ← moldes y secciones (en multipágina, sections/<pagina>/)
├── data/                   ← contenido como datos: types.ts, index.ts, un fichero por idioma
│   └── legal/              ← entity.ts, types.ts, es.ts (en.ts…), index.ts
├── layouts/                ← Base.astro, Legal.astro
├── lib/email/              ← plantilla del aviso del formulario
├── pages/                  ← index, 404, legales, gracias, styleguide, robots.txt.ts,
│   │                          sitemap.xml.ts (con idiomas: pages/[...lang]/)
│   └── api/contact.ts      ← única ruta en servidor
├── scripts/                ← video-autoplay.ts
├── styles/                 ← tokens.css, reset.css, base.css
└── form.config.ts
```

Fuente: clays-of-the-world (`src/`), APC-gestion-patrimonial
(`components/sections/<pagina>/`, `CLAUDE.md`). Si hay vídeos, sus URL van
centralizadas en un solo fichero (`magnaceramica/src/config/media.ts`) →
[vídeos](videos.md).

### Ficheros de configuración

**`package.json`** (versiones de `xanatechnologies/package.json`, scripts de
clays):

```json
{
  "type": "module",
  "engines": { "node": ">=22.12.0" },
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview",
    "check": "astro check"
  }
}
```

**`tsconfig.json`** — un solo alias, `@/` → `src/`, «nada de rutas relativas
largas» (APC `CLAUDE.md` y `tsconfig.json`):

```json
{
  "extends": "astro/tsconfigs/strict",
  "include": [".astro/types.d.ts", "**/*"],
  "exclude": ["dist"],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": { "@/*": ["src/*"] }
  }
}
```

**`.gitignore`** (clays `.gitignore` + `*.mp4` de magnaceramica + originales de
imágenes de la guía + ajustes locales de Claude de APC):

```gitignore
dist/
.astro/
node_modules/
npm-debug.log*
.DS_Store
.idea/

# Vercel
.vercel

# Variables de entorno: ninguna se sube; la plantilla sí
.env*
!.env.example

# Vídeos: se sirven desde Cloudflare R2
*.mp4

# Imágenes tal como llegan (cliente, Magnific): pesan y no se publican
docs/imagenes/originales/

# Ajustes locales de Claude Code
.claude/settings.local.json
```

**`.editorconfig`** y **`.gitattributes`**: LF en todo. Vercel construye en
Linux y el repositorio se edita desde varias sesiones; `.gitattributes`
normaliza al commitear y `.editorconfig` evita que el editor escriba CRLF. Se
copian tal cual de clays-of-the-world (`.editorconfig`: UTF-8, LF, 2 espacios,
sin recortar espacios en `.md`; `.gitattributes`: `* text=auto eol=lf` y
binarios marcados).

**`vercel.json`** (clays y xanatechnologies, más la región europea decidida el
15-09-2026):

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
  "framework": "astro",
  "buildCommand": "npm run build",
  "regions": ["fra1"]
}
```

`regions` hace que la función del formulario se ejecute en Frankfurt y no en
la región por defecto de Vercel. La política de privacidad de la base dice que
el formulario se procesa en la UE: sin esta línea, no es cierto
([legales](legales.md)).

Sin él, el proyecto de clays se creó con el preset «Other» y salida `public`
([trampa](../trampas-y-lecciones.md#vercel-construye-con-el-preset-equivocado-y-busca-la-salida-en-public)).

**`.claude/launch.json`** — el servidor de desarrollo y el del build para el
preview de Claude (clays `clays-dev` y `clays-preview`; `autoPort` de
xanatechnologies):

```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "<proyecto>-dev",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["run", "dev"],
      "port": 4321,
      "autoPort": true
    },
    {
      "name": "<proyecto>-preview",
      "runtimeExecutable": "npm",
      "runtimeArgs": ["run", "preview", "--", "--port", "4322"],
      "port": 4322
    }
  ]
}
```

Si otra sesión ocupa el puerto, Astro arranca en el siguiente: leer el puerto
real en el log ([trampa](../trampas-y-lecciones.md#el-preview-no-conecta-con-el-servidor-de-desarrollo)).
Una configuración por proyecto; no se acumulan entradas por cada prueba (clays
tiene siete).

## 7. Reglas y trampas

- **Las versiones no se copian del proyecto de referencia.** Clays va en
  Astro 5 y xanasystem-web en 6; la base es Astro 7 + adaptador 11.
- **Al copiar piezas de otro proyecto, se buscan sus restos**: `site` y
  `allowedDomains` del otro dominio, ID de GTM (`GTM-MGNFL56D` es el de clays),
  correos en `.env.example` (`info@clays.es`), textos y nombre de marca,
  remitente `forms.xanasystem.com` (el correcto es
  `no-reply@xanatechnologies.com`) y `??` en `form.config.ts` (clays lo tiene;
  va `||`). Ver [formulario](formulario.md).
- **Remoto siempre con `github-belenxana`**, nunca `github.com` ni HTTPS.
- **Push no publica.** Publica Belén desde GitHub con `xanasystem`; Claude dice
  «subido a GitHub, pendiente de tu paso en GitHub».
- **Claude no toca el CLI de Vercel.** El proyecto y las variables los crea
  Belén en la web de Vercel.
- **`main` por defecto.** Si `main` ya sirve algo publicado, se pregunta antes.
- **`git add` con rutas**, nunca `-A`: Belén edita en paralelo
  ([trampa](../trampas-y-lecciones.md#un-commit-de-claude-arrastra-trabajo-de-belén)).
- **Nada secreto en el repo**: ni claves, ni contraseñas que mande el cliente,
  ni la IP de la oficina. `.env.example` lleva los nombres, no los valores.
- **Originales y vídeos fuera de git desde el primer commit.** Lo que entra en
  el historial se queda aunque luego se borre (clays
  `docs/05-imagenes/catalogo-stock.md`: capturas en PNG que ya no se pueden
  aligerar). Las capturas de referencia entran en WebP.
- **No se inventan datos en las plantillas**: se pre-rellena solo lo que viene
  del briefing o de la web antigua, marcado «¿sigue vigente?».
- **Páginas de prueba** (`src/pages/pruebas-*`) con `noindex, nofollow` desde
  que se crean y borradas al decidir
  ([trampa](../trampas-y-lecciones.md#las-páginas-de-prueba-se-publican-y-engordan-el-despliegue)).
- **Si en Windows `dev` no detecta los cambios**, polling en Vite
  (`magnaceramica/astro.config.mjs`). No se pone por defecto
  ([trampa](../trampas-y-lecciones.md#el-servidor-de-desarrollo-no-detecta-las-ediciones-windows)).
- **Dependencia fuera de §3 → se propone antes.**

## 8. Checklist

**Decidir**
- [ ] `docs/00-arranque/brief.md` escrito
- [ ] Tipo de proyecto y módulos en `CLAUDE.md`
- [ ] Escenario de cada fase en `CLAUDE.md` y `CHECKLIST.md`
- [ ] Web antigua: sí/no; si sí, inventario de URLs y GTM/GA y legales anotados

**Kit**
- [ ] Cuestionario, accesos, material, formulario y datos legales copiados y pre-rellenados solo con lo confirmable
- [ ] Alta de formulario preparada para ingeniería
- [ ] Enviados por Belén

**Proyecto**
- [ ] Repo `xanasystem/<repo>` creado por Belén
- [ ] Astro 7, `@astrojs/vercel` 11, `engines` Node ≥ 22.12
- [ ] Solo dependencias de §3
- [ ] `tsconfig.json` con `@/`; `.gitignore` con `.env*`, `*.mp4` y `docs/imagenes/originales/`
- [ ] `.editorconfig`, `.gitattributes`, `vercel.json`, `.env.example`, `.claude/launch.json`
- [ ] Restos del proyecto de origen buscados y quitados
- [ ] `npm run build` y `npm run check` pasan
- [ ] Remoto `git@github-belenxana:xanasystem/<repo>.git`, rama `main`
- [ ] Primer commit y push con permiso

**Vercel**
- [ ] Proyecto en *Xana Technologies' projects*, conectado, producción en `main`
- [ ] Primer despliegue lanzado por Belén; URL temporal en `CLAUDE.md` y `CHECKLIST.md`

**Documentación**
- [ ] `CLAUDE.md` y `CHECKLIST.md` desde las plantillas, con los módulos
- [ ] `docs/` y `brand/` creados; `docs/imagenes/inventario.md` empezado

## Fuentes

clays-of-the-world (`CLAUDE.md`, `CHECKLIST.md` cabecera y §0, §13,
`package.json`, `tsconfig.json`, `astro.config.mjs`, `.gitignore`,
`.editorconfig`, `.gitattributes`, `vercel.json`, `.claude/launch.json`,
`docs/README.md`) · APC-gestion-patrimonial (`CLAUDE.md` § Stack y
despliegue, `tsconfig.json`, `package.json`, `.gitignore`, remoto) ·
magnaceramica (`.gitignore`, `CLAUDE.md`, `astro.config.mjs`, `package.json`,
remoto) · xanatechnologies (`package.json`, `vercel.json`, `astro.config.mjs`,
`.claude/launch.json`) · xanasystem-web (`package.json`) · `~/.ssh/config` del
equipo de Belén · memorias de la guía (git y Vercel, 14-09-2026).
