# Tipo · Landing corporativa

## Qué es

Una página larga con secciones a las que se llega por anclas, más las páginas
que trae la base: tres legales, gracias y 404. Presenta una empresa y lleva al
formulario de contacto. La más pequeña posible es una sola sección con la base
(grupomerhi, si se hiciera hoy: [fuera de la guía](../anexos/fuera-de-la-guia.md)).

## Base + módulos

- **Base** siempre: [formulario](../flujos/formulario.md),
  [legales](../flujos/legales.md), [muro y analítica](../flujos/cookies-analitica.md).
- **[Multimedia](../modulos/multimedia.md)** si hay galería o varios vídeos
  (clays).
- Si tiene más de un idioma o sustituye a otra web →
  [landing internacional](landing-internacional.md).

## Estructura por defecto

Sale de las dos landings construidas: clays-of-the-world (orden de
`src/pages/[...lang]/index.astro`, desde el wireframe
`docs/02-estructura/wireframe-onepage.svg`) y magnaceramica (Hero → About →
Brands → Experience → CustomTiles → Contact → Footer).

| # | Sección | Objetivo | Qué necesita del cliente | clays · magna |
| --- | --- | --- | --- | --- |
| 1 | **Hero** | Decir qué hace la empresa y para quién en un vistazo | Frase de posicionamiento; foto o vídeo propio | Hero con vídeo de la mina · Hero con vídeo |
| 2 | **Quiénes somos** | Situar a la empresa: qué es, desde dónde, a qué se dedica | Descripción, origen, sede | CompanyIntro · About |
| 3 | **Qué hacemos** (una sección por argumento) | Contar la oferta con hechos: recursos, instalaciones, servicios, marcas | Qué ofrece y cómo; fotos reales de cada cosa | Recursos, Logística, Laboratorio · Brands, Custom Made Tiles |
| 4 | **Pruebas** | Respaldar con cifras o trayectoria | **Cifras confirmadas**, años, certificaciones | Figures · «Over 30 Years Connecting Spain and the UK» |
| 5 | **Galería** *(Multimedia)* | Enseñar más material del que cabe en las secciones | Fotos originales y pies | Gallery + `/gallery` · — |
| 6 | **Productos** | Qué se puede comprar o pedir | Familias o catálogo | Products · (en Brands) |
| 7 | **Contacto** | Convertir: formulario | A quién llegan los avisos ([plantilla](../plantillas/cliente/formulario.md)) | Contact · Contact |
| 8 | **Bloque temporal** *(si hay)* | Una feria, una campaña: se quita al pasar | Fecha, lugar, qué se ofrece | Event (TECNA 2026) · — |
| 9 | **Pie** | Contacto, redes y los 4 elementos legales | Email, teléfono, dirección, perfiles sociales | Footer · Footer |

Páginas fuera de la portada: `/aviso-legal/`, `/politica-de-privacidad/`,
`/politica-de-cookies/`, `/gracias/`, 404 (y `/gallery` con Multimedia).

Cómo proponerla o completarla → [estructura](../flujos/estructura.md).

## Particularidades

- **Menú = anclas escritas con la barra delante** (`/#company`), para que
  funcionen también desde `/gallery` o una legal (clays `src/data/en.ts`).
- **El antetítulo es la etiqueta del menú**, en una palabra: quien llega desde
  la barra ve dónde ha caído (clays `CLAUDE.md` § Estructura de la web).
- **CTA de la cabecera al formulario** de la propia página (clays:
  «Talk to our team» → `/#enquiry`).
- **Sin datos, sin bloque.** Si faltan todas las cifras de un bloque, sale el
  bloque, no una fila de `[[ ]]`; si la sección se queda sin argumento, sale la
  sección (clays).
- **Secciones con moldes**, no compuestas una a una (clays: `Band`,
  `Showcase`, `CompanyIntro`, `Editorial` y los demás de su `/styleguide` §08)
  → [design system](../flujos/design-system.md).
- **Móvil**: los wireframes suelen ser solo de escritorio (clays, magnaceramica,
  grupomerhi); el móvil se define en la estructura.
- Los contenidos que el wireframe pedía y no tienen dato se transforman o
  salen: en clays «Corporate background» pasó a presencia y el cierre volvió
  como formulario (comentario de `index.astro`).

## Ejemplos

- **clays-of-the-world**: `src/pages/[...lang]/index.astro`,
  `docs/02-estructura/wireframe-onepage.svg`, `CLAUDE.md`.
- **magnaceramica**: `src/pages/index.astro`, `CLAUDE.md` § Project structure.
- **grupomerhi**: una sola pantalla sin base; hoy sería esta receta.
