# Tipo · Landing internacional

## Qué es

Una [landing corporativa](landing.md) en varios idiomas que sustituye a una web
que ya está publicada en el dominio. Referencia: **xanatechnologies** (inglés
en `/` y español en `/es/`, sustituyó a un WordPress el 09-09-2026).

## Base + módulos

- **Base** completa, en cada idioma.
- **[Idiomas](../modulos/idiomas.md)**.
- **[Migración](../modulos/migracion.md)** si hay web antigua.
- (**[Movimiento avanzado](../modulos/movimiento-avanzado.md)** en
  xanatechnologies: Lenis e intro.)

## Estructura por defecto

La de la [landing](landing.md), igual en cada idioma. xanatechnologies la
recorta a lo que su mensaje necesita (`src/components/Landing.astro`):

| # | Sección | Objetivo | Qué necesita del cliente | En xanatechnologies |
| --- | --- | --- | --- | --- |
| 1 | **Hero** | Qué es la empresa, en una frase | Posicionamiento en cada idioma | «Product technology and digital strategy, connected.» |
| 2 | **Ecosistema / quiénes somos** | Cómo se organiza la empresa (grupo, marcas) | Estructura del grupo y papel de cada marca | «One startup. Two specialized brands.» |
| 3 | **Marcas o servicios** | Una pieza por marca o línea, con el mismo peso | Texto, destino y logo de cada una; URL de cada web por idioma | Xana System · Woman on Mars |
| 4 | **Contacto** | Convertir | Destinatarios; qué campos (país, región) | «Tell us what you're working on.» |
| 5 | **Pie** | Legales, selector de idioma, obligaciones | Ayudas públicas con sus logos y textos obligatorios, si las hay | Footer con «Funding & Institutional Support» |

## Particularidades

- **Todo por idioma**: textos, metadatos, legales, muro, formulario y su
  página de gracias; la 404 en el idioma de la ruta → [idiomas](../modulos/idiomas.md).
- **Qué idioma va sin prefijo** se decide al empezar. Si cambia respecto a la
  web antigua (xanatechnologies tenía el español en la raíz), todas las URL
  antiguas se redirigen → [migración](../modulos/migracion.md).
- **Detección del idioma del navegador** opcional, respetando la elección del
  visitante y nunca en la 404.
- **Enlaces a otras webs del grupo en el idioma de la página**, comprobando que
  esa versión existe (`BRAND_URLS` en `src/i18n/content.ts`; en
  [deuda](../anexos/deuda-proyectos.md) porque apunta a `xanasystem.com/en/`).
- **Ayudas públicas**: si la empresa las tiene, el pie lleva logos oficiales y
  el texto literal de la web antigua (xanatechnologies
  `scripts/make-funding-strips.mjs`, `content.ts → funding`).
- **Web antigua**: guardar textos, legales, contenedor de GTM y el sitemap
  **antes** de cambiar el DNS.
- Prueba real del formulario **en cada idioma** desde el dominio.

## Ejemplos

**xanatechnologies**: `src/components/Landing.astro`, `src/i18n/content.ts`,
`src/layouts/Base.astro`, `astro.config.mjs`, `src/components/Footer.astro`,
`CLAUDE.md`. Para una landing que añade idiomas sin migrar:
clays-of-the-world.
