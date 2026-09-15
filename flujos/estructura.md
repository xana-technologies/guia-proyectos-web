# Flujo · Estructura

Decidir qué páginas y secciones tiene la web, en qué orden, para qué sirve cada
una y qué dato necesita del cliente. Es lo que aprueba el cliente antes de
escribir textos y lo que recibe Eva para el SEO.

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | El cliente dice si trae estructura (tabla 2 del [cuestionario](../plantillas/cliente/arranque.md)); se apunta el escenario en `CHECKLIST.md` |
| [02 · Estructura](../fases/02-estructura.md) | Se sigue, se completa o se propone; el cliente aprueba |
| [03 · Contenidos y SEO](../fases/03-contenidos-seo.md) | Con la estructura aprobada se pide el SEO a Eva ([petición](../plantillas/equipo/peticion-seo.md)) |
| [06 · Diseño](../fases/06-diseno-figma.md) y [07 · Construcción](../fases/07-construccion.md) | Cada sección de la lista es una sección del diseño y del código |

Se vuelve a abrir cuando se añade o se quita una sección: se cambia primero la
lista y después el código.

## Qué tenemos (escenarios)

**🟢 Wireframe completo.** Se sigue. Cada desvío se anota en
`docs/02-estructura/estructura.md` con **qué** cambió y **por qué**. En clays
el wireframe dio las 10 secciones y el orden; al construir cambiaron cosas y
todas quedaron anotadas con su motivo (`docs/03-contenidos/web-copy-en.md`):

- Los antetítulos de tres palabras del wireframe (`RESOURCE · ORIGIN ·
  CONTROL`) se cayeron: el antetítulo pasó a ser la etiqueta del menú.
- La sección de cierre se montó y se quitó; la de «Corporate background» cambió
  de tema.
- Los bloques de cifras sin dato del cliente salieron.

Aun completo, casi nunca trae todo: el de clays era **solo escritorio** y sin
formulario («STRUCTURE ONLY · NO FINAL COPY · DESKTOP CONCEPT WIREFRAME»,
`docs/02-estructura/wireframe-onepage.svg`). Lo que falta se completa como en 🟡.

**🟡 Lista de secciones o boceto simple.** Se completa con lo que nadie nombra
y se devuelve para aprobar:

| Qué completar | Qué se decide |
| --- | --- |
| **Móvil** | Qué se apila, qué se esconde, cómo es el menú. En clays quedó sin definir el mosaico de la galería (`CLAUDE.md` § Cuestiones abiertas) |
| **Páginas que van siempre** | [Las de la base](#páginas-que-van-siempre) |
| **Destino de cada CTA** | A dónde lleva cada botón. En clays no había formulario en la estructura y el destino quedó aplazado (`CLAUDE.md` § Cuestiones abiertas) |
| **Estados** | Formulario (error, enviando, gracias), dato pendiente, sin JavaScript, menú abierto |
| **Orden** | Si el orden recibido no sigue a la investigación, se propone otro con el motivo |
| **Objetivo y dato de cada sección** | Lo que no venga, se escribe |

**🔴 No hay nada.** Claude la propone:

1. Estructura por defecto del [tipo de proyecto](../tipos-de-proyecto/README.md).
2. Ajustada con las conclusiones de la [investigación](investigacion.md):
   qué contar, qué pruebas pesan, orden habitual del sector.
3. Quitando lo que el cliente no puede sostener: si una sección solo tiene
   sentido con cifras o casos que no existen, no se propone, o se propone
   condicionada al dato.

| Tipo | Estructura por defecto |
| --- | --- |
| [Landing](../tipos-de-proyecto/landing.md) | hero → quiénes somos → qué hacemos → por qué nosotros → cifras o pruebas → contacto → pie |
| [Landing internacional](../tipos-de-proyecto/landing-internacional.md) | La de la landing, por idioma |
| [Web de producto](../tipos-de-proyecto/web-producto.md) | Página de módulo: hero → problema → definición → diferenciación → funcionamiento → funcionalidades → beneficios → ecosistema → FAQ → demo |
| [Web editorial / de servicios](../tipos-de-proyecto/web-editorial.md) | inicio → quiénes somos → servicios → conocimiento → contacto |
| [Web de evento](../tipos-de-proyecto/web-evento.md) | hero con fecha → qué es → programa → cómo llegar → información práctica → colaboradores → pie |

## Pasos y quién

### 00 · Arranque

1. **Claude** apunta el escenario en `CHECKLIST.md` y guarda lo que llegue en
   `docs/02-estructura/` (wireframe, CSV, PDF).

### 02 · Estructura

1. **Claude** lee las conclusiones de `docs/01-investigacion/informe.md`.
2. **Claude** escribe `docs/02-estructura/estructura.md` con el
   [formato](#plantillas): sigue (🟢), completa (🟡) o propone (🔴). Marca qué
   viene del cliente y qué es nuestro.
3. **Claude**, si hace falta ver la composición: wireframe rápido (ver
   [abajo](#wireframe-rápido)).
4. **Belén** revisa y lo envía al cliente.
5. **Cliente** aprueba o pide cambios. Se anotan en el documento con fecha.
6. **Claude** apunta en `CHECKLIST.md` los datos que cada sección necesita del
   cliente y aún no han llegado.
7. Con la aprobación, **Belén** manda a Eva la [petición de SEO](../plantillas/equipo/peticion-seo.md)
   → [contenidos y SEO](contenidos-seo.md).

### Wireframe rápido

Opcional. Se hace cuando la lista no basta para que el cliente entienda la
página (composición a dos columnas, galería, programa filtrable) o cuando Belén
lo pide. **Cajas grises y rótulos, sin copy final ni estilo**: como el de clays,
numerado, con qué medio lleva cada hueco y qué secciones son ancla del menú.

| Herramienta | Cuándo |
| --- | --- |
| Figma / FigJam | Si el proyecto va a tener Figma (fase 06) o Belén quiere tocarlo |
| Canvas de diseño | Para dejarlo editable por Belén sin Figma |
| Artifact | Para una vista rápida que se comparte por enlace |

Se hace **escritorio y móvil**. Si solo se hace uno, se anota cuál falta.

## Páginas que van siempre

Toda web, aunque nadie las pida. Van en la lista con su URL.

| Página | Ruta (web en español) | Indexa | De dónde sale |
| --- | --- | --- | --- |
| Aviso legal | `/aviso-legal/` | `noindex, follow` | [Legales](legales.md) |
| Política de privacidad | `/politica-de-privacidad/` | `noindex, follow` | [Legales](legales.md) |
| Política de cookies | `/politica-de-cookies/` | `noindex, follow` | [Legales](legales.md) · [cookies](cookies-analitica.md) |
| Gracias | `/gracias/` | `noindex` | [Formulario](formulario.md): todo envío correcto acaba aquí y es donde se mide la conversión |
| 404 | cualquier ruta que no exista | `noindex` | [SEO técnico](seo-tecnico.md) |

En inglés: `/legal-notice/`, `/privacy-policy/`, `/cookies-policy/`,
`/thank-you/`. Si la actividad es regulada, legales extra (APC:
`/informacion-financiera/`, `APC-gestion-patrimonial/CLAUDE.md` § Arquitectura).
Las páginas internas (`/styleguide`, laboratorios) no entran en la lista del
cliente: son del [design system](design-system.md) y llevan `noindex`.

## Multipágina

Además de las secciones de cada página ([módulo multipágina](../modulos/multipagina.md)):

- **Tabla de páginas** con URL, rol, palabra clave y CTA, como
  `xanasystem-web/docs/arquitectura.md` §3. La palabra clave la propone Claude
  a partir del brief y la **confirma Eva**; sin su confirmación va marcada
  como propuesta.
- **URLs** cortas, en el idioma de la web, en minúsculas, sin acentos, con
  guiones; planas cuando la página tiene entidad propia (`/pim`, no
  `/plataforma/pim`, xanasystem-web `CLAUDE.md` regla 3). La barra final se
  decide una vez y vale para todas (APC `trailingSlash: 'always'`).
- **Una palabra clave principal por página**, sin repetirla en otra (evita que
  compitan entre sí).
- **Menú**: agrupado no significa URL anidada (xanasystem-web `docs/brief.md` §6).
- **Enlaces internos**: qué página enlaza a cuál (hub-and-spoke en
  xanasystem-web, `/pim` como página madre, `docs/arquitectura.md` §4).
- **Esqueleto por tipo de página** cuando varias comparten estructura (páginas
  de módulo de xanasystem-web, `docs/design.md` §5). La portada tiene el suyo.
- **Alcance por fases**: qué páginas entran ahora y cuáles no se crean todavía
  (xanasystem-web `CLAUDE.md` regla 4).
- **Web antigua**: qué URL antigua redirige a cuál → [migración](../modulos/migracion.md).

## Plantillas

Formato de `docs/02-estructura/estructura.md`:

````markdown
# Estructura · {cliente}

Escenario: 🟢 wireframe · 🟡 lista/boceto · 🔴 propuesta
Tipo de proyecto: {tipo} · Módulos: {…}
Aprobada por el cliente: {no / sí, fecha}

## Páginas
<!-- Solo en multipágina -->

| URL | Página | Rol | Palabra clave (Eva) | CTA |
| --- | --- | --- | --- | --- |
| `/` | Inicio | … | {propuesta · confirmada} | … |
| `/aviso-legal/` · `/politica-de-privacidad/` · `/politica-de-cookies/` | Legales | Obligatorias | — | — |
| `/gracias/` | Gracias | Conversión (noindex) | — | Volver al inicio |
| 404 | No encontrada | — | — | Volver al inicio |

## {Página}: secciones

| # | Sección | Menú | Objetivo | Qué dato necesita | Origen |
| --- | --- | --- | --- | --- | --- |
| 01 | Hero | — | Decir qué hace la empresa y para quién en una frase | Claim, foto o vídeo | cliente / propuesta |
| 02 | {…} | {etiqueta} | {…} | {… · `[[ ]]` si falta} | {…} |

## Móvil
- {qué cambia respecto a escritorio}

## CTA
| Botón | Lleva a |
| --- | --- |

## Estados
- Formulario: error por campo · enviando · `/gracias/`
- Dato pendiente: `[[ ]]` visible en la URL temporal; bloque fuera si faltan todos
- Sin JavaScript: {qué pasa con menú, galería, filtros}

## Desvíos y cambios
| Fecha | Qué | Por qué | Quién |
| --- | --- | --- | --- |
````

- **Objetivo**: una frase, lo que la sección tiene que conseguir en quien la
  lee, no lo que contiene.
- **Qué dato necesita**: lo que tiene que dar el cliente (cifras, fotos,
  productos). Es la lista de lo que se le pide.
- **Menú**: la etiqueta, si la sección es ancla del menú. Será el antetítulo
  ([contenidos](contenidos-seo.md)).

## Dónde queda cada cosa

```
docs/02-estructura/
├── estructura.md        ← la lista aprobada, con desvíos y cambios
├── wireframe.{svg,png}  ← wireframe rápido o el del cliente, si lo hay
└── material/            ← lo que mande el cliente tal cual (CSV, PDF)
```

Fuentes del formato: clays-of-the-world (`docs/02-estructura/wireframe-onepage.svg`,
estructura resumida en `CLAUDE.md` § Estructura de la web) · xanasystem-web
(`docs/fuentes/estructura.csv` consolidado en `docs/arquitectura.md`) ·
festival-viento (tabla de secciones en `docs/project-brief.md` §3).

## Reglas y trampas

- **La lista manda sobre el código.** Una sección nueva entra primero en
  `estructura.md`; si el código y la lista no coinciden, se corrige la lista o
  se pregunta.
- **Cada sección tiene un objetivo.** Si dos persiguen lo mismo, sobra una.
  En clays, la §06 contaba lo mismo que la §02 y cambió de tema tres veces
  (`web-copy-en.md` §06).
- **Sin dato no hay sección.** Si una sección solo se sostiene con cifras que
  el cliente no tiene, sale (clays `CLAUDE.md` § Estructura de la web). Se
  anota y vuelve cuando llegue el dato.
- **El móvil no es opcional.** Un wireframe solo de escritorio deja decisiones
  abiertas que aparecen al construir (galería de clays).
- **Todo CTA tiene destino.** Un botón sin destino decidido bloquea la
  construcción de la sección (clays, «Destino de los CTA», aplazado).
- **Contenido dentro de un modal = página de verdad.** Si una sección abre un
  visor o un `<dialog>`, la estructura incluye la página que lo sustituye sin
  JavaScript (clays `/gallery`, `CLAUDE.md` § Estructura de la web).
- **Sección con fecha de caducidad**, aparte: una promo de feria o evento va en
  su propia sección para quitarla sin tocar nada más (clays `web-copy-en.md` §09).
- **Legales, gracias y 404 siempre en la lista**, aunque el wireframe no las
  dibuje.
- **Palabras clave: las confirma Eva.** Claude puede proponer, no decidir.
- **No crear páginas fuera de alcance** aunque estén en la arquitectura final
  (xanasystem-web `CLAUDE.md` regla 4), ni resucitar páginas retiradas
  (`/auditoria`, misma regla).

## Checklist

**02 · Estructura**

- [ ] Escenario apuntado; material del cliente en `docs/02-estructura/material/`
- [ ] Conclusiones de la investigación aplicadas
- [ ] Cada sección con objetivo, dato que necesita y origen
- [ ] Etiqueta de menú en las secciones ancla
- [ ] Destino de cada CTA
- [ ] Móvil decidido (o anotado qué falta)
- [ ] Estados: formulario, pendiente, sin JavaScript
- [ ] Aviso legal, privacidad, cookies, gracias y 404 en la lista
- [ ] Multipágina: tabla de URLs, palabra clave por página, enlaces internos,
      alcance de la fase, redirecciones si hay web antigua
- [ ] Desvíos del wireframe anotados con el porqué
- [ ] Datos pendientes del cliente en `CHECKLIST.md`
- [ ] Revisada por Belén
- [ ] **Aprobada por el cliente**, con fecha
- [ ] Petición de SEO enviada a Eva

## Fuentes

clays-of-the-world (`docs/02-estructura/wireframe-onepage.svg`, `CLAUDE.md`
§ Estructura de la web y § Cuestiones abiertas, `docs/03-contenidos/web-copy-en.md`,
`src/pages/`) · xanasystem-web (`docs/brief.md` §6–§11, `docs/arquitectura.md`,
`docs/design.md` §5, `CLAUDE.md` reglas 3, 4 y 7) · APC-gestion-patrimonial
(`CLAUDE.md` § Arquitectura del sitio) · festival-viento (`docs/project-brief.md` §3) ·
tipos de proyecto de esta guía.
