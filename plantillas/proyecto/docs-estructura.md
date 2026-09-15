# Estructura de `docs/` y `brand/`

<!-- NOTA PARA CLAUDE:
     - Esta plantilla no se copia como fichero: se crean las carpetas en el
       arranque (fase 00) y `docs/README.md` con la tabla de abajo.
     - Las carpetas vacías llevan `.gitkeep` hasta que tengan contenido
       (clays-of-the-world, `docs/02-estructura/.gitkeep`).
     - Los números siguen las fases de la guía. Si una fase no deja nada en
       `docs/`, no tiene carpeta (04 va en `brand/`; 06 es el enlace a Figma del
       `CLAUDE.md`; 07–10 viven en el código y en `CHECKLIST.md`). -->

`docs/` guarda todo lo que no es código: lo que manda el cliente, lo que
preparamos antes de construir y lo que hay que conservar para decidir. `brand/`
guarda la identidad. Los dos van en git salvo lo que se indica.

```
docs/
├── README.md                    ← esta tabla y el estado de cada carpeta
├── 00-arranque/
│   ├── brief.md                 ← qué es, a quién va, qué tiene que conseguir, tipo y módulos
│   ├── cuestionario-arranque.md ← plantilla cliente/arranque.md, enviada y devuelta
│   ├── accesos.md               ← plantilla cliente/accesos.md (sin contraseñas nunca)
│   ├── material.md              ← plantilla cliente/material.md
│   ├── formulario.md            ← plantilla cliente/formulario.md
│   ├── alta-formulario.md       ← petición a ingeniería (plantilla equipo/)
│   └── urls-web-antigua.md      ← solo con web antigua: inventario para las redirecciones
├── 01-investigacion/            ← informe de competencia y conclusiones
├── 02-estructura/               ← páginas y secciones, wireframes (SVG + PNG)
├── 03-contenidos/
│   ├── copy-<pagina>-<idioma>.md  ← textos por página (o uno por idioma en una one-page)
│   ├── seo-metadatos.md         ← lo que manda Eva: tabla por página e idioma
│   └── legal/
│       ├── README.md            ← cómo están montados los legales y qué falta
│       ├── datos-legales.md     ← plantilla cliente/datos-legales.md, enviada y devuelta
│       └── legal-copy-es.md     ← solo si hay revisión jurídica
├── 05-referencias-visuales/
│   ├── referencias.md           ← plantilla proyecto/referencias.md
│   └── a1-nombre.webp …         ← capturas, en WebP
└── imagenes/
    ├── inventario.md            ← plantilla proyecto/inventario-imagenes.md
    ├── busqueda-stock.md        ← plantilla proyecto/busqueda-stock.md
    └── originales/              ← EXCLUIDA DE GIT: lo que llega del cliente y de Magnific, sin tocar

brand/
├── README.md                    ← el manual (o el brand board) traducido a decisiones de web
├── logo/                        ← logos originales: SVG siempre; EPS/PNG si es lo que llega
└── brand-board/                 ← solo en el escenario 🔴 de la fase 04
```

## Qué va en cada carpeta

| Carpeta | Qué va | Qué no va | Fase |
| --- | --- | --- | --- |
| `00-arranque/` | Brief, plantillas enviadas y devueltas, inventario de URLs de la web antigua | Contraseñas, claves, IP de oficina | [00](../../fases/00-arranque.md) |
| `01-investigacion/` | Competencia y sector: qué cuentan, cómo, qué les falta; conclusiones | Referencias visuales (van en 05) | [01](../../fases/01-investigacion.md) |
| `02-estructura/` | Mapa de páginas, secciones con su objetivo, wireframes | — | [02](../../fases/02-estructura.md) |
| `03-contenidos/` | Copy aprobado o en revisión, metadatos SEO, legales. Los documentos del cliente con textos se guardan aquí tal como llegan (clays `web-copy-es.pdf`) | Datos inventados: lo que falta, `[[ ]]` | [03](../../fases/03-contenidos-seo.md) |
| `05-referencias-visuales/` | Capturas y `referencias.md` | Material de competencia (va en 01) | [05](../../fases/05-design-system.md) |
| `imagenes/` | Inventario y búsqueda en git; originales fuera de git | Másters optimizados (van en `src/assets/`) | [00 · 03 · 07](../../flujos/imagenes.md) |
| `brand/` | Reglas de marca para web, logos, brand board | Tokens (van en `src/styles/tokens.css`) | [04](../../fases/04-marca.md) |

## Qué no va en `docs/`

- **Vídeos**: en Cloudflare R2, nunca en el repo (`*.mp4` en `.gitignore`) →
  [vídeos](../../flujos/videos.md).
- **Estado del proyecto**: en `CHECKLIST.md`, en la raíz.
- **Reglas para Claude**: en `CLAUDE.md`, en la raíz.
- **Secretos**: ni aquí ni en ningún fichero del repo. Los de Vercel se
  guardan en Vercel.

## Fuentes

clays-of-the-world (`docs/README.md`, `docs/03-contenidos/legal/`,
`docs/01-referencias/inspiracion/referencias.md`, `brand/README.md`,
`brand/logo/`) · guía: [fases](../../fases/README.md),
[`cliente/arranque.md`](../cliente/arranque.md) (`brief.md`),
[`flujos/legales.md`](../../flujos/legales.md) (`legal/`),
[`flujos/imagenes.md`](../../flujos/imagenes.md) (`imagenes/`). Cambios respecto
a clays: `00-inbox/` desaparece (cada cosa va a su fase y las imágenes a
`imagenes/originales/`); `01-referencias/` se separa en `01-investigacion/` y
`05-referencias-visuales/`; `04-tecnico/` desaparece (dominio y accesos en
`00-arranque/`, el resto en el `CLAUDE.md`).
