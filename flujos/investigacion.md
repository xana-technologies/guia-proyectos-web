# Flujo · Investigación

Entender el sector y la competencia antes de decidir qué cuenta la web: qué
secciones tienen otras webs, qué argumentos y pruebas usan, con qué tono, a
qué llaman y qué les falta. El resultado son **conclusiones**, no un catálogo
de webs: qué contar, qué evitar y cómo diferenciarse.

**No incluye referencias visuales.** Cómo se ve una web se decide en la fase
05 con las referencias que elige Belén
([referencias visuales](referencias-visuales.md)). Aquí se mira qué dicen las
webs y en qué orden, no cómo lucen.

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | El cliente dice si trae análisis de competencia (tabla 2 del [cuestionario](../plantillas/cliente/arranque.md)) y quiénes son sus competidores (pregunta 4). Se apunta el escenario en `CHECKLIST.md` |
| [01 · Investigación](../fases/01-investigacion.md) | Se usa lo que trae o se hace, y se escriben las conclusiones |
| [02 · Estructura](../fases/02-estructura.md) | Las conclusiones deciden secciones y orden |
| [03 · Contenidos y SEO](../fases/03-contenidos-seo.md) | Las conclusiones deciden argumentos y tono; el informe se pasa a Eva con la [petición de SEO](../plantillas/equipo/peticion-seo.md) |

Se vuelve a abrir si el cliente cambia de posicionamiento a mitad de proyecto.

## Qué tenemos (escenarios)

**🟢 Viene en el briefing.** Se usa y se completa si hace falta. Dos casos
reales:

- **clays-of-the-world**: el cliente trajo un anexo «Análisis de la
  comunicación de la competencia» dentro de su presentación
  (`docs/01-referencias/competencia/anexo-competencia-presentacion-v2.pdf`).
  Agrupa los competidores en tres tipos, dice de cada uno qué modelo tiene,
  qué claim usa y qué debería hacer Clays frente a él, y llega a una
  diferencia: recursos mineros propios **más** abastecimiento global. Esa
  frase acabó siendo la entradilla del hero
  (`docs/03-contenidos/web-copy-en.md` §01).
- **xanasystem-web**: el brief trae una sección «Competencia en la que nos
  inspiramos» (Plytix, Akeneo, Sales Layer, Inriver) con, para cada uno,
  **Inspiración** y **Adaptación para Xana**, y la regla de no copiar su
  estructura literalmente (`docs/brief.md` §5).

Qué se hace: **volcarlo a markdown** en `docs/01-investigacion/` (un PDF no se
puede buscar ni citar desde el resto de la documentación; en clays quedó
pendiente, `docs/01-referencias/README.md`), comprobar que las webs citadas
siguen existiendo y completar lo que falte de la [ficha](#qué-se-analiza-de-cada-web)
(normalmente secciones, pruebas y carencias). Las conclusiones del cliente
**mandan**: lo que añadimos va marcado como nuestro.

**🔴 No viene.** Claude investiga y escribe el informe con la
[plantilla](#plantillas). Punto de partida: los competidores que nombre el
cliente en el cuestionario; si no nombra ninguno, Claude propone la lista y
Belén la confirma antes de analizar.

**🟡 Solo una lista de nombres.** Es un 🔴 con la lista ya dada: se analiza
cada una con la ficha.

## Qué se busca

| Tipo | Qué es | Cuántas |
| --- | --- | --- |
| **Competidores directos** | Venden lo mismo al mismo cliente | Todos los que nombre el cliente; si no hay lista, 3–5 |
| **Competidores por modelo** | Mismo sector, otro modelo de negocio (grupo grande, especialista, distribuidor) | 1–2 por tipo. En clays: grandes grupos mineros · productores especializados · formuladores y traders |
| **Webs del sector que el cliente admira** | Las que nombre en el cuestionario (§3) por lo que cuentan | Las que dé. Si las da por cómo se ven, van a [referencias visuales](referencias-visuales.md) |
| **Webs que conviven con la nuestra** | La web oficial o institucional que ya existe del mismo tema | Si las hay. En festival-viento, la del Ayuntamiento: la nuestra la complementa, no la sustituye, y de ahí sale qué **no** repetir (`festival-viento/CLAUDE.md` regla 2) |

Cómo busca Claude en 🔴: búsqueda web con las palabras con las que el cliente
se describe y con las del sector, en el idioma y país del público. Cada web
entra con su **URL y fecha de consulta**.

## Qué se analiza de cada web

Una ficha por web, siempre los mismos apartados:

| Apartado | Qué se anota |
| --- | --- |
| **Quién es** | Tipo de competidor, país, a quién vende. Una línea |
| **Secciones y orden** | La portada de arriba abajo, en una línea: `hero → cifras → productos → …`. En multipágina, además, el menú |
| **Argumentos** | Qué dice que le hace mejor, con sus palabras. Claim principal entre comillas |
| **Pruebas** | Con qué lo sostiene: cifras, certificaciones, clientes, casos, años, fotos reales o de stock. Las cifras, como **«declara…»** (TerraTrends «declara comercializar aproximadamente 400.000 toneladas al año», anexo de clays) |
| **Tono** | Técnico o de marketing, tú o usted, frases cortas o largas, superlativos |
| **Llamadas a la acción** | Qué pide y dónde: presupuesto, demo, contacto, ficha técnica, registro |
| **Carencias** | Qué no cuenta o cuenta mal y nuestro cliente sí puede sostener |
| **Qué tomar / qué no** | Para el cliente, en una o dos frases. Es lo que llega a las conclusiones |

Qué **no** se anota: colores, tipografía, fotografía como estilo, animaciones.
Si algo visual llama la atención, se le pasa a Belén como posible referencia,
fuera del informe.

## Pasos y quién

### 00 · Arranque

1. **Claude** lee la tabla 2 y las preguntas 3–4 del
   [cuestionario](../plantillas/cliente/arranque.md) y apunta el escenario en
   `CHECKLIST.md`.
2. **Claude** copia lo que traiga el cliente a `docs/01-investigacion/material/`.

### 01 · Investigación

1. **Claude**, en 🟢: vuelca el material a `informe.md` respetando lo que dice
   el cliente, y completa las fichas. En 🔴 sin lista: propone competidores y
   **Belén** confirma.
2. **Claude** analiza cada web con la ficha y escribe las conclusiones.
3. **Belén** revisa. Las conclusiones no se envían al cliente como documento:
   le llegan convertidas en la estructura propuesta, que sí aprueba
   ([estructura](estructura.md)).
4. **Claude** apunta en `CHECKLIST.md` las preguntas al cliente que hayan
   salido (datos que harían falta para diferenciarse: cifras, certificaciones,
   casos).

### 02 y 03 · Cómo alimenta lo que viene

| Conclusión | Dónde se usa |
| --- | --- |
| **Qué contar** | Secciones de la [estructura](estructura.md) y argumentos del copy |
| **Qué pruebas pesan en el sector** | Qué datos pedir al cliente; bloques de cifras o de confianza. Si el cliente no los da, el bloque no se hace ([contenidos](contenidos-seo.md)) |
| **Qué evitar** | Reglas del copy: claims prohibidos, competidores que no se nombran (xanasystem-web `docs/contenido/home.md` §15) |
| **Cómo diferenciarse** | Mensaje del hero y orden de las secciones |
| **Orden habitual del sector** | Punto de partida para ajustar la estructura por defecto del [tipo de proyecto](../tipos-de-proyecto/README.md) |
| **Tono** | Tono del copy cuando la marca no lo define |
| **El informe entero** | Se pasa a Eva con la [petición de SEO](../plantillas/equipo/peticion-seo.md) |

## Plantillas

La plantilla del informe va aquí mismo. Se copia a
`docs/01-investigacion/informe.md`.

````markdown
# Investigación · {cliente}

Escenario: 🟢 del cliente · 🔴 hecha por Claude · 🟡 lista del cliente
Fecha: {dd-mm-aaaa} · Revisado por Belén: {sí/no, fecha}
Material del cliente: {material/…, o «no hay»}

## Conclusiones

### Qué contar
- {argumento} — {por qué: qué web lo usa, qué carencia cubre}

### Qué pruebas necesitamos
- {tipo de prueba} — {si el cliente la tiene, dónde; si no, `[[ ]]` y pedirla}

### Qué evitar
- {claim, tono, estructura o nombre que no conviene} — {por qué}

### Cómo diferenciarse
{Una o dos frases. Solo con hechos que el cliente pueda sostener.}

### Para la estructura
- Orden habitual en el sector: {hero → … }
- Qué cambiamos y por qué: {…}

### Preguntas para el cliente
- [ ] {dato que falta para sostener una conclusión}

## Competidores

| Web | Tipo | URL | Consultada |
| --- | --- | --- | --- |
| {nombre} | {directo / por modelo / admirada / convive} | {url} | {fecha} |

### {Nombre}

- **Quién es:** {…}
- **Secciones y orden:** {hero → … }
- **Argumentos:** {… «claim literal»}
- **Pruebas:** {… «declara…»}
- **Tono:** {…}
- **Llamadas a la acción:** {…}
- **Carencias:** {…}
- **Qué tomar / qué no:** {…}
````

- *Marcado:* lo que viene del cliente se deja tal cual; lo añadido por Claude
  lleva *(añadido)* detrás.
- *Conclusiones arriba*: son lo que se lee; las fichas son el respaldo.

## Dónde queda cada cosa

```
docs/01-investigacion/
├── informe.md        ← conclusiones + fichas (plantilla de arriba)
└── material/         ← lo que manda el cliente tal cual (PDF, presentación)
```

- Las capturas de webs **no** se guardan aquí: si Belén quiere alguna como
  referencia visual, va a `docs/05-referencias-visuales/`.
- Clays tiene inspiración y competencia juntas en `docs/01-referencias/`; en
  proyectos nuevos van separadas por fase.

## Reglas y trampas

- **Conclusiones, no inventario.** Un informe que describe diez webs y no dice
  qué hacer no sirve a la estructura. Cada conclusión nombra de qué ficha sale.
- **Diferenciarse con hechos del cliente.** Una diferencia que el cliente no
  puede sostener (una cifra, un certificado que no tiene) no es conclusión: es
  una pregunta para él. En clays, las cifras que no llegaron salieron de la web
  (`clays-of-the-world/CLAUDE.md` § Estructura de la web).
- **Lo del competidor es lo que declara.** Sus cifras y claims se citan como
  suyos, con URL; no se dan por verificados.
- **No copiar textos.** Se cita el claim entre comillas y se resume el resto.
  Ninguna frase de un competidor pasa al copy.
- **No copiar estructuras enteras.** Se toma la lógica, se adapta al cliente
  (xanasystem-web `docs/brief.md` §5: «sin copiar literalmente su estructura»).
- **Un competidor más amplio no es el modelo.** Si trabaja en muchos sectores,
  lo útil es su forma de contar, no su amplitud (Sibelco en el anexo de clays:
  «Clays no debería copiar su amplitud»).
- **Los competidores no se nombran en la web** salvo decisión expresa del
  cliente (xanasystem-web `docs/contenido/home.md` §15).
- **PDF del cliente → markdown.** Si no, no se puede buscar ni citar
  (`clays-of-the-world/docs/01-referencias/README.md`).
- **Nada visual.** Si el análisis empieza a hablar de colores o de fotografía
  como estilo, se ha salido del flujo.

## Checklist

**00 · Arranque**

- [ ] Escenario apuntado en `CHECKLIST.md`
- [ ] Material del cliente en `docs/01-investigacion/material/`

**01 · Investigación**

- [ ] Material del cliente volcado a markdown, sin cambiar lo que dice
- [ ] Lista de webs confirmada por Belén (si no la dio el cliente)
- [ ] Cada web con URL, fecha y todos los apartados de la ficha
- [ ] Cifras de competidores como «declara», claims entre comillas
- [ ] Conclusiones: qué contar, qué pruebas, qué evitar, cómo diferenciarse,
      para la estructura
- [ ] Cada diferencia sostenida por un hecho del cliente o convertida en pregunta
- [ ] Preguntas para el cliente pasadas a `CHECKLIST.md`
- [ ] Nada visual en el informe
- [ ] Revisado por Belén

## Fuentes

clays-of-the-world (`docs/01-referencias/competencia/anexo-competencia-presentacion-v2.pdf`,
`docs/01-referencias/README.md`, `docs/03-contenidos/web-copy-en.md` §01,
`CLAUDE.md`) · xanasystem-web (`docs/brief.md` §5, `docs/contenido/home.md`
§15) · festival-viento (`CLAUDE.md` regla 2) · cuestionario de arranque
(`plantillas/cliente/arranque.md`).
