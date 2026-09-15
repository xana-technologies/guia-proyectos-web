# {{PROYECTO}} — Checklist

<!-- NOTA PARA CLAUDE (se borra al copiar):
     - Se copia a `CHECKLIST.md` en la raíz del proyecto en el arranque.
     - Es el estado real del proyecto, tarea a tarea. Se actualiza al cerrar
       cada pieza, no al final.
     - Aquí van los puntos de la base. El detalle de cada tarea está en la guía
       (`C:\Proyectos\guia-proyectos-web\`); cuando se llega a una fase, se pegan
       debajo las casillas de la checklist del flujo que toca.
     - Módulos: al final, un bloque por módulo con la checklist de su ficha.
     - Una fase que no se hace (p. ej. 01 en 🟢) no se borra: se marca y se dice por qué.
     - Formato de origen: clays-of-the-world, `CHECKLIST.md`. -->

**Tipo:** {{tipo de proyecto}} · **Módulos:** {{módulos o «ninguno»}}
**Última revisión:** {{FECHA}}
**Estado global:** {{una o dos líneas}}
**URL temporal:** {{*.vercel.app}} · **Dominio:** {{dominio o «pendiente»}}

Leyenda: `[x]` hecho · `[ ]` pendiente · `[~]` parcial · 🚫 bloqueado por algo
que tiene que llegar de fuera (cliente, Eva, ingeniería).

## Escenarios

| Fase | Escenario | Qué ha traído el cliente / qué falta |
| --- | --- | --- |
| 01 · Investigación | 🟢 · 🔴 | |
| 02 · Estructura | 🟢 · 🟡 · 🔴 | |
| 03 · Contenidos | 🟢 · 🟡 · 🔴 | |
| 04 · Marca | 🟢 · 🟡 · 🔴 | |
| 06 · Figma | A · B · C | |
| Imágenes | 🟢 · 🟡 · 🔴 | |
| Vídeos | 🟢 · 🟡 · 🔴 · no lleva | |
| Legales (datos) | 🟢 · 🟡 · 🔴 | |

---

## 00 · Arranque

Guía: `flujos/arranque-proyecto.md`

- [ ] Brief en `docs/00-arranque/brief.md`; tipo y módulos en `CLAUDE.md`
- [ ] Escenarios anotados en la tabla de arriba
- [ ] Web antigua: {{no · sí → `docs/00-arranque/urls-web-antigua.md` y módulo migración}}
- [ ] Kit al cliente enviado por Belén: cuestionario, accesos, material, formulario, datos legales
- [ ] Alta de formulario enviada a ingeniería
- [ ] Repo `xanasystem/{{repo}}`, remoto con `github-belenxana`, rama `main`
- [ ] Astro 7 + `@astrojs/vercel` 11, Node ≥ 22.12; `npm run build` y `npm run check` pasan
- [ ] `.gitignore`, `.editorconfig`, `.gitattributes`, `vercel.json` (con `"regions": ["fra1"]`), `.env.example`
- [ ] Proyecto en Vercel conectado; URL temporal anotada aquí y en `CLAUDE.md`
- [ ] `docs/` y `brand/` creados; inventario de imágenes empezado

## 01 · Investigación

Guía: `flujos/investigacion.md`

- [ ] Informe de competencia y conclusiones en `docs/01-investigacion/`
- [ ] Revisado por Belén

## 02 · Estructura

Guía: `flujos/estructura.md`

- [ ] Páginas y secciones con su objetivo en `docs/02-estructura/`
- [ ] Aprobada por el cliente

## 03 · Contenidos y SEO

Guía: `flujos/contenidos-seo.md` · `flujos/imagenes.md`

- [ ] Petición de SEO enviada a Eva; respuesta en `docs/03-contenidos/seo-metadatos.md`
- [ ] Copy de cada página e idioma; los `[[ ]]` anotados en «Pendientes»
- [ ] Título y descripción de cada página e idioma
- [ ] Inventario de imágenes cerrado; `busqueda-stock.md` preparado para lo que falta
- [ ] Textos aprobados por el cliente

## 04 · Marca

Guía: `fases/04-marca.md` · `flujos/brand-board.md`

- [ ] `brand/README.md`
- [ ] Logos en SVG en `brand/logo/`
- [ ] Brand board aprobado por el cliente (solo 🔴)

## 05 · Design system

Guía: `flujos/referencias-visuales.md` · `flujos/design-system.md`

- [ ] Referencias de Belén en `docs/05-referencias-visuales/referencias.md`
- [ ] `src/styles/tokens.css` con contraste AA comprobado y anotado
- [ ] Componentes base y moldes de sección
- [ ] `/styleguide` con `noindex, nofollow`

## 06 · Diseño en Figma

Guía: `flujos/diseno-figma.md`

- [ ] Caso {{A · B · C}}; enlace en `CLAUDE.md`
- [ ] Aprobación del cliente (A: sobre el diseño; B y C: sobre la URL temporal)

## 07 · Construcción

Guía: `fases/07-construccion.md`

- [ ] Secciones montadas, textos como datos, móvil, legible sin JavaScript → `flujos/maquetacion.md`
- [ ] Imágenes optimizadas y colocadas; nada provisional → `flujos/imagenes.md`
- [ ] Vídeos en R2 con póster y arranque en pantalla → `flujos/videos.md`
- [ ] Formulario → `flujos/formulario.md` (pegar aquí su checklist «Construcción»)
- [ ] Legales → `flujos/legales.md` (pegar aquí su checklist «07 · Construcción»)
- [ ] Muro de cookies y GTM → `flujos/cookies-analitica.md` (pegar aquí su checklist)
- [ ] SEO técnico: metadatos, OG, sitemap, robots, JSON-LD, 404 → `flujos/seo-tecnico.md`
- [ ] Páginas de prueba borradas (o con `noindex, nofollow` mientras se decide)

## 08 · Revisión antes de publicar

Guía: `flujos/revision.md`

- [ ] Checklist del flujo pasada en la URL temporal
- [ ] Ningún `[[ ]]`, `placeholder` ni imagen provisional
- [ ] Visto bueno de Belén y del cliente

## 09 · Publicación

Guía: `flujos/publicacion.md`

- [ ] Con o sin `www` decidido; `site` y `allowedDomains` con el dominio definitivo
- [ ] DNS cambiado y comprobado (HTTPS, redirección de la otra versión)
- [ ] Search Console verificada y sitemap enviado
- [ ] Formulario con correos definitivos y prueba real confirmada por el cliente
- [ ] Retención de GA4 en 14 meses; funciones de Vercel en región europea
- [ ] GA4 recibe datos a las 24 h; informe tipo creado

## 10 · Entrega y mantenimiento

Guía: `flujos/entrega-mantenimiento.md`

- [ ] Documento de entrega enviado
- [ ] Tabla Clientes Xana, Factura Directa y Drive
- [ ] Pendientes de abajo con responsable y fecha
- [ ] `CLAUDE.md` en estado «publicado»; lecciones propuestas para la guía

---

## Módulos

<!-- Un bloque por módulo del proyecto, con la checklist de su ficha
     (`modulos/<modulo>.md`). Borrar este comentario si no hay módulos. -->

### {{Módulo}}

Guía: `modulos/{{modulo}}.md`

- [ ] {{…}}

---

## Pendientes con terceros

| Qué | De quién | Pedido el | Bloquea |
| --- | --- | --- | --- |
| {{dato}} | cliente · Eva · ingeniería | {{fecha}} | {{fase o publicación}} |
