# Flujo · Brand board

Tener la identidad del cliente clara **en términos de web** antes de montar el
design system: qué logo va dónde, qué colores sirven para texto en pantalla,
qué tipografías se pueden cargar y con qué jerarquía. Si hay manual, se traduce;
si no lo hay, se hace un brand board.

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | El cliente dice si trae manual (tabla 2 del [cuestionario](../plantillas/cliente/arranque.md)) y manda logos |
| [04 · Marca](../fases/04-marca.md) | Se traduce el manual, se completa o se hace el brand board; el cliente aprueba |
| [05 · Design system](../fases/05-design-system.md) | `brand/README.md` es la entrada de los tokens |

Puede empezar justo después del arranque, a la vez que la investigación. Se
vuelve a abrir cuando el cliente manda un logo o un color nuevo.

## Qué tenemos (escenarios)

**🟢 Hay manual.** Se traduce a decisiones de web en `brand/README.md`. El
manual está pensado para imprenta y no responde a lo que pregunta una web:

| Lo que dice el manual | La decisión de web que hay que tomar | Ejemplo real |
| --- | --- | --- |
| Paleta de imprenta | Qué colores pasan AA como texto y sobre qué fondo. Los que no, se quedan para fondos y superficies, y se crean **extensiones para pantalla** con su ratio anotado | Clays: el único color del manual que vale para texto sobre claro es el grafito; se añaden arena oscura y dos grafitos como extensión (`brand/README.md` §4) |
| Logo sobre fondos | Qué pasa cuando el hero es una foto o una sección es oscura | Clays: el manual prohíbe el logo sobre foto → cabecera sólida o pastilla blanca; en oscuro plano, versión en blanco (`brand/README.md` §3) |
| Versiones del logo | Cuál va en cabecera, pie, móvil estrecho y favicon; cuáles hay en blanco; cuáles no existen | Clays: horizontal en cabecera, sin icono en pie y móvil; la versión blanca no estaba en el manual y la mandó el cliente después (`brand/README.md` §2) |
| Tipografías | Si se pueden usar en web (licencia, archivos WOFF2) y, si no, con qué se sustituyen | Xana: Astonpoliz no está disponible para web → Roboto provisional → laboratorio → Inter + Mona Sans (`xanasystem-web/docs/brand.md`, `docs/design.md` §1) |
| Pesos y jerarquía | Qué peso y tamaño lleva cada rol en pantalla | Clays: Montserrat 500 en display, 600 en H2 y antetítulo, 300 en entradillas (`brand/README.md` §5) |
| Modo oscuro | Si lo hay, es una adaptación accesible, no otra identidad | `xanasystem-web/docs/brand.md`, nota inicial |

**🟡 Solo logo y colores.** Se hace lo de 🟢 con lo que haya y se completa lo
que falta con las mismas piezas del brand board: tipografías, estilo de
fotografía, iconos, tono. Se devuelve al cliente para aprobar.

**🔴 No hay nada.** Es el paso que más pesa de la fase, porque de él sale la
identidad. Va en dos tiempos: primero **tres propuestas de marca** para elegir
([abajo](#tres-propuestas-de-marca)) y después, **con la elegida, el brand
board** ([contenido](#qué-lleva-el-brand-board)). APC se hizo así: propuestas y
brand board en Claude Design (`C:\Users\Belén\Xana\APC\identidad-marca\`), con
paleta, tipografías y un recurso gráfico propio; el logo definitivo llegó
después (`APC-gestion-patrimonial/CLAUDE.md` § Identidad visual).

**Web de otro producto de la misma marca.** Se usa el manual de la empresa y
se anota qué cambia (xanasystem-web: identidad de Xana Technologies con
«SYSTEM» en lugar de «TECHNOLOGIES», `docs/brand.md`).

## Tres propuestas de marca

Solo en 🔴. Decidido por Belén el 15-09-2026, a partir del trabajo de marca de
APC («Territorio 6 · Marfil y terracota · El arco»).

- **Tres propuestas** en **una página web**, una debajo de otra y con la misma
  estructura, para compararlas. Son direcciones distintas de verdad, no tres
  variaciones de color.
- **Belén elige** una (o mezcla) y se anota la elección y el porqué. **El brand
  board se hace después, con la elegida.**
- La página es un [laboratorio](#páginas-de-laboratorio): `noindex, nofollow`,
  rótulo visible, estilos y fuentes aislados, textos reales del cliente, y se
  borra al decidir.

Cada propuesta lleva:

| Bloque | Qué incluye |
| --- | --- |
| **Nombre** | Número y nombre con carácter («Marfil y terracota · El arco») |
| **Paleta** | Cada color con nombre, hex, RGB y **para qué se usa** (fondo, texto, acción, datos…) |
| **Proporción** | Barra con el peso de cada color en la web |
| **Reglas** | Normas de uso del color (APC: el terracota solo en cursiva, arco, botón o filete) |
| **Tipografía** | Pareja con licencia web y escala por rol (display, título, cita, entradilla, cuerpo, etiqueta, dato) aplicada a **textos reales del cliente** |
| **Recurso gráfico** | La pieza propia de la marca, si la hay (APC: «El arco») |

Con el **contraste calculado** de las combinaciones de texto de cada paleta y
su veredicto (AA, AAA, solo texto grande, falla).

## Qué lleva el brand board

| Pieza | Qué se decide |
| --- | --- |
| **Logo y usos** | Versiones (horizontal, vertical, sin icono, isotipo), en color y en blanco; área de seguridad; sobre qué fondos va cada una; qué no se hace (sobre foto, recolorear); favicon |
| **Paleta** | Cada color con **nombre, hex y papel**: fondo, texto, acento, acción, solo degradado. Tabla de **contraste** de las combinaciones de texto con su veredicto (AA, AAA, solo texto grande, falla) |
| **Pareja tipográfica y jerarquía** | Titulares y texto; tabla de roles (display, H1, H2, H3, antetítulo, cuerpo, entradilla, botón) con peso y tamaño; de dónde se cargan |
| **Fotografía** | Qué se fotografía, luz, encuadre, personas sí o no, qué evitar (stock corporativo genérico) |
| **Iconos** | Estilo (línea o relleno, grosor), si hay set propio o cuál se usa |
| **Recurso gráfico** | Si lo hay: forma, cuándo se usa (APC: «El Arco», `docs/GUIA-DE-ESTILO.md` §5.2) |
| **Tono de voz** | Tres o cuatro rasgos y qué evitar; es el que usa [contenidos](contenidos-seo.md) |
| **Ejemplos aplicados** | La marca puesta en piezas de la web: cabecera, hero, una sección, botón, pie. Es lo que el cliente entiende de verdad |

Formato de las tablas de paleta y tipografía: el de
`clays-of-the-world/brand/README.md` §4 y §5.

## Pasos y quién

### 00 · Arranque

1. **Cliente** manda manual y logos (plantilla de [material](../plantillas/cliente/material.md)).
2. **Claude** apunta el escenario en `CHECKLIST.md` y deja el material en
   `brand/`.

### 04 · Marca

1. **Claude** lee el manual (o lo que haya) y escribe `brand/README.md` con el
   [formato](#plantillas): qué dice la marca y qué decisión de web sale de cada
   cosa. Calcula los contrastes; no los estima.
2. **Claude** pide lo que falte y bloquea: logos en SVG, versión en blanco,
   archivos de las tipografías o su licencia para web.
3. **Belén** decide lo visual: extensiones de paleta, sustitutos tipográficos,
   qué versión del logo va en cada sitio. Si hay que comparar opciones,
   [página de laboratorio](#páginas-de-laboratorio).
4. En 🔴: **Claude** prepara las [tres propuestas de marca](#tres-propuestas-de-marca)
   y **Belén** elige una. Con la elegida, **Belén** hace el brand board con
   ayuda de Claude ([herramientas](#herramientas)).
5. **Belén** lo envía al cliente. **El cliente aprueba** el brand board (o las
   decisiones que cambian su manual) **antes del design system**.
6. **Claude** anota lo aprobado en `brand/README.md` con fecha.

## Páginas de laboratorio

Cuando hay que decidir entre opciones de tipografía o de color, se decide
**viéndolas sobre contenido real de la web**, no en una lámina.

| Laboratorio | Qué hace | Fuente |
| --- | --- | --- |
| Tipografía | Selectores de titular y texto, parejas rápidas, sliders de peso, interletraje, tamaño; menú, hero, párrafos, cifras, tabla, pangrama y escala con textos de la web | `xanasystem-lab/src/pages/lab-tipografias.astro` |
| Color | Propuesta de un papel nuevo para un color de marca (olivo = método) aplicada a secciones reales, con tokens locales aislados en la página | `APC-gestion-patrimonial/src/pages/demo-color.astro` |
| Composición | Variantes de una sección con los mismos textos para elegir molde | `clays-of-the-world/src/pages/pruebas-lab.astro`, datos en `src/data/pruebas/lab.ts` |

Reglas:

- **`noindex, nofollow`** desde que se crea y un rótulo visible «laboratorio ·
  no publicada».
- **Aislado**: sus variables y fuentes viven solo en esa página; la web real no
  se entera (el laboratorio de Xana carga Google Fonts solo ahí; la web
  auto-aloja las fuentes).
- **Contenido real**, no «lorem ipsum»: los textos de la web o del brief.
- **Lo que gana va a los tokens** y el laboratorio **se borra** (el historial
  lo conserva). Mientras no se decide, lo que propone no se usa en páginas
  reales (APC `docs/GUIA-DE-ESTILO.md` §2.1: el olivo «no adoptado»). Las
  páginas de prueba publicadas son una trampa conocida
  ([trampas](../trampas-y-lecciones.md#las-páginas-de-prueba-se-publican-y-engordan-el-despliegue)).

## Herramientas

| Herramienta | Para |
| --- | --- |
| **Figma** | Brand board que Belén quiere editar a mano o que pasará al diseño de la fase 06 |
| **Canvas de diseño** | Brand board o ejemplos aplicados editables sin Figma, publicados por enlace |
| **Pencil** | Ejemplos aplicados en archivos `.pen` |
| **Página de laboratorio** | Decidir tipografía, color o composición en el navegador |

## Plantillas

Formato de `brand/README.md` (sale de `clays-of-the-world/brand/README.md`):

````markdown
# {Cliente} · Identidad

Origen: manual {nombre, versión, fecha} · brand board {fecha} · solo logo y colores
Aprobado por el cliente: {no / sí, fecha}
Tokens: `src/styles/tokens.css`

## 1. La marca
{A qué se dedica, ejes de identidad, significado del símbolo si lo tiene.}

## 2. Versiones del logo
| Versión | Uso en web | Archivo |
| --- | --- | --- |
| Horizontal | Cabecera | `logo/logo-horizontal.svg` |
| Sin icono / blanco | Pie, fondo oscuro, móvil estrecho | … |
| Favicon | — | `logo/favicon.svg` |
Lo que no existe y hay que pedir: {…}

## 3. Uso sobre fondos
- Manual: {…}
- **Consecuencia para la web:** {cabecera sólida / pastilla sobre foto / versión blanca en oscuro}

## 4. Paleta
| Nombre | Hex | Papel |
| --- | --- | --- |
### Contraste
| Combinación | Ratio | Veredicto |
| --- | --- | --- |
### Extensiones para pantalla
| Token | Hex | Por qué | Ratio |
| --- | --- | --- | --- |

## 5. Tipografía
{Familias, de dónde se cargan, licencia web.}
| Rol | Familia | Peso | Tamaño |
| --- | --- | --- | --- |

## 6. Fotografía, iconos y recurso gráfico
## 7. Tono de voz
## 8. Archivos fuente y pendientes
````

## Dónde queda cada cosa

```
brand/
├── README.md          ← la marca traducida a web; se lee antes de tocar nada visual
├── logo/              ← logos en SVG (y los originales que mande el cliente)
└── brand-board.*      ← enlace o exportación del brand board, si se hace
```

- El manual original (PDF) no se sube si pesa o es confidencial: se anota dónde
  está (clays `brand/README.md` §6; xanasystem-web lo tiene en `docs/fuentes/`).
- **`brand/README.md` dice qué es la marca y qué decisión de web sale;
  `tokens.css` tiene los valores.** Si un proyecto separa los dos documentos
  (xanasystem-web `docs/brand.md` + `docs/design.md`), el primero no repite
  tokens.

## Reglas y trampas

- **Los colores del manual no se tocan ni se amplían.** Lo que haga falta para
  pantalla va aparte, marcado como extensión, con su ratio (clays `CLAUDE.md`
  § Design system).
- **Contraste medido, no estimado.** Cada combinación de texto con su ratio.
- **Un color que falla como texto sobre claro puede valer sobre oscuro**
  (clays: arena natural 2,5:1 sobre blanco arcilla, 5,0:1 sobre grafito 900).
- **La regla del logo es del logo, no del fondo.** Una sección oscura es
  legítima; lo que no lo es es el logo encima sin la versión o la pastilla que
  toca (clays `brand/README.md` §4).
- **En oscuro plano, versión en blanco; la pastilla, solo sobre foto** (clays
  `brand/README.md` §3).
- **Solo los logos oficiales.** Si hay variantes antiguas o rechazadas en la
  carpeta, se anotan como prohibidas (Xana: solo `XT26.Logos`, memoria
  `logos-oficiales-xt26` de xanasystem-web).
- **Logos en SVG.** Un PNG no escala bien en cabecera retina; si el cliente no
  tiene SVG se pide (clays `brand/README.md` §6, pendiente). Un «SVG» con un PNG
  incrustado no es un vector (clays, favicon, §2).
- **Tipografía de manual no disponible para web**: no se simula ni se carga sin
  licencia; se decide sustituto en laboratorio y se anota (Xana).
- **Fuentes auto-alojadas** en la web real; servicios externos solo en el
  laboratorio ([design system](design-system.md)).
- **Los pesos se combinan entre elementos, no dentro de un titular** (clays
  `brand/README.md` §5).
- **El cliente aprueba antes del design system.** Tokens montados sobre una
  marca sin aprobar se rehacen.

## Checklist

**04 · Marca**

- [ ] Escenario apuntado en `CHECKLIST.md`
- [ ] Logos en `brand/logo/`, en SVG; versiones en blanco y favicon (o pedidos)
- [ ] Uso del logo sobre foto y sobre oscuro decidido
- [ ] Paleta con nombre, hex y papel
- [ ] Contraste calculado de cada combinación de texto
- [ ] Extensiones para pantalla con su ratio, separadas de la marca
- [ ] Tipografías con licencia y archivos para web, o sustituto decidido
- [ ] Jerarquía tipográfica por rol
- [ ] Fotografía, iconos, recurso gráfico y tono (en 🟡 y 🔴)
- [ ] En 🔴: tres propuestas en página web, elegida una por Belén con su porqué
- [ ] Ejemplos aplicados (en 🔴)
- [ ] Laboratorios con `noindex`, borrados tras decidir
- [ ] Revisado por Belén
- [ ] **Aprobado por el cliente**, con fecha, antes del design system

## Fuentes

clays-of-the-world (`brand/README.md`, `CLAUDE.md` § Design system y § Marca,
`src/pages/pruebas-lab.astro`, `src/data/pruebas/lab.ts`) · xanasystem-web
(`docs/brand.md`, `docs/design.md` §1, memoria `logos-oficiales-xt26`) ·
xanasystem-lab (`src/pages/lab-tipografias.astro`) · APC-gestion-patrimonial
(`CLAUDE.md` § Identidad visual, `docs/GUIA-DE-ESTILO.md` §2 y §5.2,
`src/pages/demo-color.astro`).
