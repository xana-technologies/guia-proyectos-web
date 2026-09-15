# {{PROYECTO}} — web

<!-- NOTA PARA CLAUDE (se borra al copiar):
     - Se copia a `CLAUDE.md` en la raíz del proyecto en el arranque y se
       rellena con el briefing y el cuestionario. Lo que no se sepa, `{{ }}` se
       sustituye por «pendiente», nunca por un dato supuesto.
     - Aquí va lo propio del proyecto y las reglas que no se saltan. El cómo se
       hace cada tarea está en la guía; se enlaza, no se copia.
     - Se mantiene al día: cada decisión que cambia una regla o una cuestión
       abierta se apunta aquí en el momento, con fecha.
     - Modelos reales: clays-of-the-world y APC-gestion-patrimonial, `CLAUDE.md`. -->

{{Qué es la web, en dos o tres frases: empresa, a qué se dedica, a quién va
dirigida y qué tiene que conseguir.}}

**Guía de Xana:** `C:\Proyectos\guia-proyectos-web\`. Léela antes de empezar
una fase; ante un fallo, busca primero en `trampas-y-lecciones.md`.

## El proyecto

| | |
| --- | --- |
| Cliente | {{razón social / marca}} |
| Tipo de proyecto | {{landing · landing internacional · web de producto · web editorial · web de evento · base + módulos}} |
| Módulos | {{multipágina · idiomas · migración · blog · evento · multimedia · movimiento avanzado · formulario avanzado · ninguno}} |
| Idiomas | {{es · en…; cuál va sin prefijo}} |
| Dominio | {{dominio}} · {{con o sin `www`: pendiente hasta la fase 09}} |
| Gestor del dominio | {{DonDominio · técnico del cliente (nombre)}} |
| Web antigua | {{no · sí: URL, tecnología}} |
| Estado | {{arranque · construcción · revisión · publicado el {{fecha}}}} |

### Escenarios

| Fase | Escenario | Nota |
| --- | --- | --- |
| 01 · Investigación | {{🟢 · 🔴}} | |
| 02 · Estructura | {{🟢 · 🟡 · 🔴}} | |
| 03 · Contenidos | {{🟢 · 🟡 · 🔴}} | |
| 04 · Marca | {{🟢 · 🟡 · 🔴}} | |
| 06 · Figma | {{A · B · C}} | |
| Imágenes | {{🟢 · 🟡 · 🔴}} | |

## Enlaces y cuentas

| Qué | Dónde |
| --- | --- |
| Repositorio | `xanasystem/{{repo}}` · remoto `git@github-belenxana:xanasystem/{{repo}}.git` |
| Vercel | Equipo *Xana Technologies' projects* · proyecto `{{proyecto}}` |
| URL temporal | {{https://….vercel.app}} |
| Figma | {{enlace al fichero · nodos principales}} |
| Tag Manager | {{GTM-XXXXXXX · del cliente o creado por Xana}} |
| Analytics / Search Console | {{del cliente o de Xana; quién tiene acceso}} |
| Brevo | Clave del proyecto en Vercel (`BREVO_API_KEY`); la gestiona ingeniería |
| Turnstile | Widget con {{dominios}} + URL temporal + `localhost` |
| Vídeos (R2) | {{URL base o «no lleva vídeos»}} |
| Material fuera del repo | {{ruta de Drive o de la carpeta del cliente}} |

Nunca se escriben aquí claves, contraseñas ni la IP de la oficina.

## Reglas que no se saltan

### La base

- **Toda la web es Astro + base**: formulario, legales, muro de cookies y
  analítica (GTM + GA4 tras aceptar). Flujos en la guía: `flujos/formulario.md`,
  `flujos/legales.md`, `flujos/cookies-analitica.md`.
- **No se inventan datos.** Lo que falta se marca `[[ ]]` (componente
  `Pending`) y no se publica. Cifras, nombres y hechos, solo los que da el cliente.
- **El contenido es datos, nunca marcado.** Textos en `src/data/`; los
  componentes los reciben por props. Vale desde el primer componente.
- **`src/styles/tokens.css` es la única fuente de verdad** de color, tipografía
  y espaciado. Ningún componente escribe un valor a pelo; los componentes usan
  tokens semánticos. Contraste mínimo AA, anotado.
- **Todo lo que depende de JavaScript se monta al revés**: lo legible es lo que
  se pinta por defecto; sin JavaScript no queda contenido invisible.
- `prefers-reduced-motion` apaga lo que se mueve.
- **Imágenes**: de stock solo Magnific (descarga Belén); IA solo si Belén lo
  indica; una foto de stock no se presenta como del cliente. Originales en
  `docs/imagenes/originales/`, fuera de git.
- **Vídeos** en Cloudflare R2, nunca en el repo.
- **Lo que carga la web está declarado en la política, y al revés.** Primero el
  texto legal, luego el cambio.
- Páginas de prueba con `noindex, nofollow` y se borran al decidir.

### Propias de este proyecto

- {{reglas del manual de marca, del cliente o decididas por el camino, con fecha}}

## Git y publicación

- Se trabaja y se publica en **`main`**. {{Ramas: solo si se pide.}}
- **Commit** al terminar una pieza, con mensaje en español, imperativo y sin
  prefijos, **después del sí de Belén**. Nunca un commit por ronda de feedback.
- **Push** agrupado y también con permiso.
- `git status` antes de tocar y `git add <rutas>` solo con lo propio: Belén
  edita en paralelo. Nada de `git add -A`.
- **Push no publica.** Vercel rechaza los commits de `belen@xanasystem.com`:
  publica **Belén** con un commit manual desde GitHub con la cuenta
  `xanasystem`. Después de un push se dice «subido a GitHub, pendiente de tu
  paso en GitHub», nunca «publicado».
- **Claude no lanza `vercel deploy`, `vercel --prod` ni `vercel link`.**
- Las variables de entorno de Vercel no se aplican hasta el siguiente despliegue.

## Tono del copy

{{Tono de la marca en tres o cuatro frases: trato (tú / usted), largo de frase,
qué evitar. Sale del manual, del brand board o del cuestionario.}}

## Comandos

```bash
npm run dev      # localhost:4321 (o el que diga el log)
npm run build    # dist/
npm run preview  # sirve el build: la carga de imágenes se juzga aquí, no en dev
npm run check    # tipos de Astro
```

## Dónde está cada cosa

| Ruta | Qué hay |
| --- | --- |
| `CHECKLIST.md` | Estado real del proyecto, tarea a tarea |
| `brand/README.md` | La marca traducida a decisiones de web. **Léelo antes de tocar nada visual** |
| `brand/logo/` | Logos originales |
| `docs/00-arranque/` | Brief y lo que se ha pedido al cliente y a ingeniería |
| `docs/01-investigacion/` | Competencia y conclusiones |
| `docs/02-estructura/` | Páginas y secciones |
| `docs/03-contenidos/` | Copy, `seo-metadatos.md`, `legal/` |
| `docs/05-referencias-visuales/` | Referencias de Belén con qué se toma de cada una |
| `docs/imagenes/` | `inventario.md`, `busqueda-stock.md`, `originales/` (fuera de git) |
| `src/styles/` | `tokens.css` · `reset.css` · `base.css` |
| `src/data/` | Todo el contenido; `legal/` para los textos legales |
| `src/components/sections/` | Moldes y secciones |
| `/styleguide` | Referencia visual navegable del sistema |

## Estructura de la web

{{Páginas y secciones en orden, con su objetivo. Se rellena al cerrar la fase 02.}}

## Cuestiones abiertas

No des ninguna por resuelta sin confirmarlo:

- {{cuestión · desde cuándo · quién tiene que decidir}}

## Aviso

Este repositorio se edita desde varias sesiones a la vez. Antes de escribir,
comprueba `git status` y relee los archivos que vayas a tocar.
