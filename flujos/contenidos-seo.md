# Flujo · Contenidos y SEO

Los textos de la web, escritos sobre lo que la gente busca y sin inventar nada:
qué se pide a Eva, cómo se escribe o se adapta el copy, cómo se marca lo que
falta, dónde viven los textos en el código y cómo se cierran los metadatos y el
inventario de imágenes.

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | El cliente dice si trae textos (tabla 2 del [cuestionario](../plantillas/cliente/arranque.md)); se apunta el escenario |
| [03 · Contenidos y SEO](../fases/03-contenidos-seo.md) | Petición a Eva, copy, metadatos, cierre del inventario; el cliente aprueba |
| [07 · Construcción](../fases/07-construccion.md) | El copy aprobado pasa a `src/data/`; los metadatos, a los datos de cada página |
| [08 · Revisión](../fases/08-revision.md) | Ningún `[[ ]]` ni dato de prueba en lo que se publica |

Empieza con la [estructura](estructura.md) **aprobada**: sin ella Eva no puede
asignar palabras clave por página.

## Qué tenemos (escenarios)

**🟢 Copy cerrado.** Se usa **literal**. Solo se toca lo que rompe una regla de
la web, se anota con el porqué y se le dice al cliente. En clays el español
llegó en un documento del cliente y entró tal cual con dos cambios anotados: los
dos CTA que iban de tú pasaron a usted porque el resto trataba de usted, y el
dominio sin `www` (`clays-of-the-world/src/data/es.ts`, comentario inicial).
Lo que el documento no traía (antetítulos, `alt`, formulario, 404, gracias) se
escribió con su misma terminología y **no** se marca como pendiente de validar
(`clays-of-the-world/CLAUDE.md` § Idioma).

**🟡 Textos sueltos, briefs por página o web antigua.** Se reordenan según la
estructura, se adaptan al tono y al largo de cada sección, y lo que falta va en
`[[ ]]`. De la web antigua se toman hechos (que el cliente confirma), no frases
enteras. En xanasystem-web los briefs por página del cliente (`.docx`) se
pasaron a markdown y esa versión es la vinculante (`docs/contenido/*.md`,
`docs/README.md`).

**🔴 No hay textos.** Claude los escribe con lo que haya: brief, cuestionario,
investigación, material del cliente y SEO de Eva. **Sin inventar cifras,
hechos, nombres de producto, clientes ni certificaciones.** Lo que no se sabe va
en `[[ ]]` o no se escribe. En clays todo lo redactado por nosotros iba marcado
*Propuesto*, lo del cliente *Extraído* o *dado por cliente* (`docs/03-contenidos/web-copy-en.md`).

## Pasos y quién

### 03 · Contenidos y SEO

1. **Belén → Eva**: [petición de SEO](../plantillas/equipo/peticion-seo.md)
   con el brief, la estructura aprobada, los idiomas y el informe de
   investigación. **Claude** la prepara rellenada.
2. **Claude** escribe o adapta el copy en `docs/03-contenidos/` mientras llega
   el SEO: argumentos, bloques, CTA. En clays el copy existía antes de que
   llegaran los títulos y descripciones (14-09-2026, `seo-metadatos.md`).
3. **Eva** devuelve, por página e idioma: palabra clave principal y
   secundarias, título, descripción, H1 y notas para el copy.
4. **Claude** guarda lo de Eva en `docs/03-contenidos/seo-metadatos.md` y ajusta
   el copy: H1 = el que propone Eva; palabras clave en titulares y primeros
   párrafos sin forzar la frase. Si falta algo, **pregunta a Eva**; no inventa
   palabras clave.
5. **Claude** cierra el inventario de imágenes ([abajo](#cierre-del-inventario-de-imágenes)).
6. **Belén** revisa copy, metadatos e inventario y los envía al cliente.
7. **Cliente** aprueba los textos y contesta los `[[ ]]` que pueda. Lo que
   cambie se aplica en el documento con fecha.
8. **Claude** pasa a `CHECKLIST.md` los `[[ ]]` que queden abiertos.

### 07 · Construcción

1. **Claude** pasa el copy aprobado a `src/data/` ([abajo](#contenido-como-datos)).
   Desde ese momento **la versión viva está en los datos**; el documento se
   actualiza cuando cambia un texto, no al revés (clays `seo-metadatos.md`:
   «si se cambia uno, se cambian los dos sitios»).
2. **Claude** pone título y descripción de cada página desde los datos →
   [SEO técnico](seo-tecnico.md).

## Tono por defecto

Es el de clays (`CLAUDE.md` § Tono del copy y `web-copy-en.md` § Notas de tono)
y se usa **cuando la marca no define otro**:

- **Afirmativo y corto.** Frases nominales con punto: «A reliable source. A
  trusted partner.»
- **Hechos antes que adjetivos.** Minas propias, laboratorio propio, 35 años,
  Castellón; no «world-class».
- **Sin superlativos vacíos, sin exclamaciones, sin emojis.**
- **Cuando falta un dato, callarse antes que rellenar.**

**Se adapta a la marca.** Lo que diga el manual, el brief o el `CLAUDE.md` del
proyecto manda sobre el tono por defecto:

| Proyecto | Tono | Fuente |
| --- | --- | --- |
| clays-of-the-world | B2B técnico, afirmativo, usted en español | `CLAUDE.md` § Tono del copy, `src/data/es.ts` |
| xanasystem-web | B2B consultivo y cercano, orientado a valor; terminología obligatoria y claims prohibidos | `CLAUDE.md` reglas 5 y 6 |
| APC-gestion-patrimonial | Editorial y cercano, tú; sin promesas de rentabilidad ni urgencia; empieza por la persona | `CLAUDE.md` § Reglas de tono |
| festival-viento | Visual, motivador, sencillo, para familias | `docs/project-brief.md` §2 |

Reglas que valen con cualquier tono:

- **Un solo tratamiento en toda la web**, tú o usted, también en botones,
  errores y avisos del formulario (clays `es.ts`).
- **Terminología fija** del cliente en el `CLAUDE.md` del proyecto: nombres de
  producto y textos literales de los CTA (xanasystem-web regla 5: «Xana PIM»,
  nunca «Xana Product Manager»; «Acceso clientes», nunca «Iniciar sesión»).
- **Claims sin prueba, fuera**: «el mejor», «líder», «resultados garantizados»,
  cifras sin fuente (xanasystem-web regla 5, `docs/contenido/home.md` §15).
- **Actividad regulada**: sin promesas y con el aviso que toque (APC, aviso
  educativo en contenidos y pie).
- **Acrónimos explicados la primera vez** y la definición de lo que se vende en
  los primeros 100–150 palabras de la página, para buscadores y respuestas de
  IA (xanasystem-web regla 2).
- **Una sola `h1` por página**, la que propone Eva.

## Datos que faltan: `[[ ]]`

- **`[[ ]]` es un dato que falta entre varios que sí están.** Se escribe qué
  falta: `[[ X ]] Countries supplied`, `[[ nombre comercial ]]`. En la web se
  pinta visible en la URL temporal y **no se publica** en el dominio.
- **Si faltan todos los datos de un bloque, se quita el bloque**, no se marcan
  uno a uno: tres marcas seguidas a cuerpo grande pesan más que el texto al que
  acompañan. Pasó con las cifras de la §03 y la §04 de clays
  (`CLAUDE.md` § Estructura de la web).
- **Si al quitar el bloque la sección se queda sin argumento, sale la sección.**
- Todo lo quitado se anota en `CHECKLIST.md` y vuelve cuando llegue el dato.
- **Un dato de prueba no es un `[[ ]]`.** Si para ver la maqueta hace falta un
  valor de mentira, va marcado como `placeholder`, se pinta distinto y **nada
  con `placeholder` se publica** (clays `src/data/types.ts`, `FooterItem`).
- **No se rellena con texto plausible.** Una cifra o una ficha de producto
  inventadas son el dato más peligroso de la web: en clays la ficha «Other raw
  materials» fue una invención hasta que se quitó (`web-copy-en.md` §08).
- **Fotos de stock**: los pies y los `alt` describen lo que se ve sin decir
  que son del cliente (clays `CLAUDE.md` § Cuestiones abiertas).

## Antetítulo

- **El antetítulo es la etiqueta del menú, en una palabra**: `About us`,
  `Resources`, `Facilities`. Quien llega desde la barra ve confirmado dónde ha
  caído (clays `CLAUDE.md` § Estructura de la web).
- Si el cliente alarga una etiqueta, cambian **las dos a la vez**, menú y
  antetítulo; si aprieta, se acortan las dos (clays `web-copy-en.md` §00).
- Una sección sin entrada en el menú lleva un antetítulo corto de tema.
- **Antetítulo y titular no repiten texto** ni uno empieza por el otro
  (xanasystem-web `CLAUDE.md` regla 10).
- Se escribe en caja normal en los datos; las mayúsculas las pone el CSS.

## Contenido como datos

**Ningún texto se escribe en el marcado.** Todo lo que se lee o se oye —titulares,
párrafos, CTA, `alt`, etiquetas accesibles, errores del formulario, 404,
gracias— va en `src/data/` y los componentes lo reciben por props. Activar otro
idioma después es añadir un fichero; sacar cadenas de quince `.astro` no lo es.
**Vale desde el primer componente, aunque la web tenga un solo idioma**
(clays `CLAUDE.md` § Idioma).

Montaje de referencia (clays `src/data/`):

```
src/data/
├── types.ts    ← qué campos tiene cada sección (SiteContent, HeroContent…)
├── es.ts       ← el contenido en español (un fichero por idioma)
├── en.ts
└── index.ts    ← LANGS, getContent(lang)
```

```ts
// src/data/types.ts (extracto)
export interface KeyDatum {
  value: string | null; // null = el cliente no ha dado el dato → [[ ]]
  unit?: string;
  label: string;
  pending?: string;     // qué falta exactamente
}
```

Reglas del montaje:

- **Las imágenes se eligen en un solo idioma** y los demás solo traducen su
  `alt` (clays `es.ts` toma `src` de `en.ts`). Qué foto va en cada hueco es
  composición, no idioma.
- **Lo que es presentación no va en el texto**: la numeración `01 · 02` la pone
  un contador CSS (clays `types.ts`, `BandContent.items`).
- **Los saltos de línea forzados van en los datos** como una entrada por
  renglón, porque dependen del idioma (clays `HeroContent.headline`).
- **Textos con huecos llevan marcas** (`{n} de {total}`), no trozos
  concatenados: el orden cambia de un idioma a otro (clays `GalleryViewerContent`).
- **Datos estructurados, tipados**: programa, horarios, patrocinadores, en
  ficheros propios con su tipo (festival-viento `src/data/programa.ts`,
  `tipos.ts`, `site.ts`).
- **Datos del sitio en un solo sitio**: nombre, dominio, correo, redes
  (festival-viento `src/data/site.ts`). Los del titular legal van en
  `src/data/legal/entity.ts` ([legales](legales.md)).

Detalle de rutas por idioma, `hreflang` y selector: [módulo idiomas](../modulos/idiomas.md).

## Metadatos

- Se escriben en **`docs/03-contenidos/seo-metadatos.md`**: una tabla por página
  e idioma con título, descripción y H1, tal como los manda Eva, con fecha.
- Pasan a los datos de cada página (`meta` en clays `es.ts`: `title`,
  `description`, `siteName`, `imageAlt`) y de ahí al `<head>` →
  [SEO técnico](seo-tecnico.md).
- Título con la palabra clave delante y la marca detrás: `Arcillas de cocción
  blanca | Clays of the World`. Largos orientativos de la petición: título
  ~60 caracteres, descripción ~155.
- En el mismo documento, qué se indexa y qué no (clays `seo-metadatos.md` § Qué
  se indexa).
- Si Eva manda algo que no casa con la web (un dominio con `www` que no es el
  principal, una ruta que aún no existe), se ajusta y se anota el aviso (clays
  `seo-metadatos.md` § Aviso sobre el sitemap).

## Cierre del inventario de imágenes

Con el copy cerrado se sabe qué imagen va en cada sitio. Se aplica el
[flujo de imágenes](imagenes.md):

- Cada hueco de la estructura con su imagen: del cliente, stock o pendiente.
- Las que faltan, a `docs/imagenes/busqueda-stock.md` para buscarlas en
  Magnific (descarga Belén) o generarlas con IA **solo si Belén lo indica**.
- `alt` escrito junto al copy, en los datos.
- Pies y `alt` de stock sin atribuir al cliente.

## Plantillas

| Plantilla | Para | Fase |
| --- | --- | --- |
| [`equipo/peticion-seo.md`](../plantillas/equipo/peticion-seo.md) | Eva: qué tiene que devolver por página e idioma | 03 |
| [`cliente/arranque.md`](../plantillas/cliente/arranque.md) | Cliente: si trae textos (tabla 2) | 00 |

Formato del copy, `docs/03-contenidos/web-copy-{idioma}.md` (en multipágina, uno
por página: `docs/03-contenidos/{pagina}.md`, como `xanasystem-web/docs/contenido/`):

````markdown
# {Cliente} · Copy ({idioma})

Origen: **cliente** (literal) · **adaptado** (del material del cliente) ·
**propuesto** (escrito por nosotros) · `[[ ]]` dato que falta.
Aprobado por el cliente: {no / sí, fecha}

## 00 · Global
Meta → ver `seo-metadatos.md` · Menú: {etiquetas} · CTA: {primario} / {secundario}

## 01 · {Sección}
**Antetítulo** ({origen}): {etiqueta del menú}
**Titular** ({origen}): {…}
**Entradilla** ({origen}): {…}
**Bloques** ({origen}):
| Título | Texto |
| --- | --- |
**CTA** ({origen}): {texto} → {destino}
**Imagen**: {qué es · de dónde · alt}
> Notas: cambios con fecha y porqué; lo retirado, aquí marcado «retirado».

## Datos pendientes
- [ ] {dato} (sección {n})
````

## Dónde queda cada cosa

```
docs/03-contenidos/
├── web-copy-es.md          ← copy por sección (o {pagina}.md en multipágina)
├── seo-metadatos.md        ← lo que manda Eva, por página e idioma
├── material/               ← textos del cliente tal cual (PDF, docx)
└── legal/                  ← flujo de legales

docs/imagenes/              ← flujo de imágenes (inventario y búsqueda)

src/data/
├── types.ts · es.ts · index.ts
└── legal/                  ← flujo de legales
```

## Reglas y trampas

- **No se inventa.** Ni cifras, ni productos, ni clientes, ni certificaciones,
  ni «operaciones» que no se sabe qué son: se pregunta (clays, «Operations in
  Romania», `web-copy-en.md` §06).
- **El texto del cliente manda aunque sea menos rotundo** que el nuestro
  (clays `web-copy-en.md` §03). Si pierde un dato concreto, se le avisa.
- **Un texto nuevo del cliente puede contradecir la web.** Se comprueba contra
  el resto de secciones antes de montarlo (clays, «international sourcing»
  contra «minas propias», `web-copy-en.md` §01–§02).
- **La composición pone límite al largo.** Si un párrafo nuevo deshace la
  sección, se habla antes de reescribir la composición (clays `web-copy-en.md`
  §02 y §04). Al traducir se vuelve a mirar el reparto de líneas.
- **Nada de anchos fijos calculados sobre un idioma**: otros alargan un 15–30 %
  (clays `CLAUDE.md` § Idioma).
- **Palabras clave, solo de Eva.**
- **Datos y documento no se separan.** Si cambian en código, se actualiza el
  documento; si cambian en el documento, se cambia el código.
- **Un `[[ ]]` o un `placeholder` no se publican.**
- **Textos de terceros** (logo de una feria, marca de un socio): no se
  recolorean ni se recomponen; su nombre va como `alt` (clays `web-copy-en.md` §09).

## Checklist

**03 · Contenidos y SEO**

- [ ] Estructura aprobada antes de pedir el SEO
- [ ] Petición de SEO enviada a Eva y respuesta en `seo-metadatos.md`
- [ ] Copy por sección con su origen marcado
- [ ] Tono: el de la marca o el de por defecto; un solo tratamiento
- [ ] Terminología fija y CTA literales en el `CLAUDE.md` del proyecto
- [ ] Ningún dato inventado; lo que falta en `[[ ]]`, bloques sin datos quitados
- [ ] Antetítulo = etiqueta del menú; no repite el titular
- [ ] Una `h1` por página, la de Eva
- [ ] Textos de formulario, gracias, 404 y `alt` escritos
- [ ] Inventario de imágenes cerrado; búsqueda de stock preparada
- [ ] Pendientes en `CHECKLIST.md`
- [ ] Revisado por Belén
- [ ] **Aprobado por el cliente**, con fecha

**07 · Construcción**

- [ ] Todo el texto en `src/data/`, ninguno en el marcado
- [ ] Imágenes elegidas en un solo idioma; los demás, solo `alt`
- [ ] Metadatos de cada página desde los datos, iguales a `seo-metadatos.md`

**08 · Revisión**

- [ ] Ningún `[[ ]]` ni `placeholder` en lo que se publica

## Fuentes

clays-of-the-world (`CLAUDE.md` § Idioma, § Tono del copy, § Estructura de la
web; `docs/03-contenidos/web-copy-en.md`, `seo-metadatos.md`; `src/data/types.ts`,
`es.ts`, `index.ts`) · xanasystem-web (`CLAUDE.md` reglas 2, 5, 6 y 10;
`docs/contenido/home.md`; `docs/README.md`) · APC-gestion-patrimonial
(`CLAUDE.md` § Reglas de tono) · festival-viento (`docs/project-brief.md` §2,
`src/data/site.ts`, `tipos.ts`) · petición de SEO (`plantillas/equipo/peticion-seo.md`).
