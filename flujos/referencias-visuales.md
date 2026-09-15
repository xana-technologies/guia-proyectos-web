# Flujo · Referencias visuales

Primer paso del design system: las webs y piezas que marcan cómo se va a ver
la web. **Las elige siempre Belén.** Claude las ordena y anota, de cada una,
qué se toma y qué no, para que al montar tokens, moldes y secciones se sepa
por qué se hace cada cosa.

Lo que las webs **cuentan** (secciones, argumentos, tono) no es esto: es la
[investigación](investigacion.md).

## Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | El cliente puede dar referencias en el [cuestionario](../plantillas/cliente/arranque.md) (§3, opcional) |
| [05 · Design system](../fases/05-design-system.md) | **Primer paso.** Belén elige, Claude ordena y anota, Belén decide la dirección |
| [06 · Diseño](../fases/06-diseno-figma.md) y [07 · Construcción](../fases/07-construccion.md) | Se consulta al montar cada sección («qué se toma» de cada una) |

Necesita la marca (fase [04](../fases/04-marca.md)): una referencia se lee
contra `brand/README.md`.

## Qué tenemos (escenarios)

| | Qué pasa |
| --- | --- |
| **Belén elige** (siempre) | Manda enlaces o capturas. Son la dirección visual del proyecto |
| **El cliente las pone en el briefing** | Entran marcadas como del cliente y **tienen prioridad**: lo que Belén añade las complementa, no las contradice. Si una del cliente choca con su propia marca o con otra suya, se le pasa a Belén para que lo hable con el cliente |
| **El cliente no dice nada** | Solo las de Belén. No se le piden al cliente fuera del cuestionario |
| **Belén pide ideas a Claude** | Claude puede proponer, pero van en un apartado **«Propuestas de Claude, sin elegir»** y no cuentan como referencia hasta que Belén las pase a la tabla |

Las referencias que el cliente da **por lo que cuentan** (no por cómo se ven)
van a la investigación.

## Pasos y quién

1. **Cliente** (opcional, en 00): referencias en el cuestionario, con qué le
   gusta de cada una.
2. **Belén** elige las suyas y se las pasa a Claude: enlace, captura o las dos.
3. **Claude** guarda cada captura en `docs/05-referencias-visuales/` con nombre
   `fuente_descripcion.ext` (xanasystem-web `docs/referencias-visuales/README.md`).
   Si solo hay enlace, captura la página entera.
4. **Claude** escribe `referencias.md` con el [formato](#plantillas): de dónde
   sale, si es web real o concepto, quién la eligió, qué se toma de cada una
   (composición, tipografía, fotografía, movimiento) y qué no.
5. **Claude** contrasta cada una con `brand/README.md` y anota los choques y
   cómo se resuelven, **enlazando** a la regla de marca sin copiarla (clays:
   el logo de `a4` va sobre foto, prohibido por el manual; se resuelve como
   `a2`, `docs/01-referencias/inspiracion/referencias.md` § Notas).
6. **Belén** decide la dirección: con qué 2–3 se queda y qué pesa más. Claude
   lo escribe arriba del documento.
7. **Claude** usa la tabla al montar el [design system](design-system.md) y
   cita la referencia en las decisiones que salen de ella.

## Qué se anota de cada una

| Apartado | Qué se escribe |
| --- | --- |
| **Qué es** | Nombre y enlace al original. **Web real o concepto** (Dribbble, plantilla): los conceptos usan fotos de stock y contenido de relleno, las webs reales no (clays: «`a4` es la única web real de la carpeta») |
| **Quién** | Belén o cliente |
| **Composición** | Estructura de la pieza que interesa: hero partido, mockup a lo ancho bajo el hero, cifras superpuestas, lista numerada en acordeón |
| **Tipografía** | Pareja, contraste de pesos, antetítulos, tamaño del titular |
| **Fotografía** | Qué se fotografía, real o stock, tratamiento, velos |
| **Movimiento** | Apariciones, hovers, vídeo. Si no hay nada que tomar, «—» |
| **Qué no** | Lo que no se copia y por qué: paleta que choca con la marca, tono de consumo, render 3D (xanasystem-web `inspiracion/flowsync_landing-completa.md` § Qué NO copiar) |
| **Dónde se usa** | Sección o bloque de nuestra estructura al que sirve (xanasystem-web `nuroai_landing-completa.md` § Patrones reutilizables) |

**Se toma un patrón concreto de cada pieza, no el conjunto** (clays
`docs/01-referencias/README.md`: «es aspiracional: interesa un patrón concreto
de cada pieza»).

## Plantillas

Formato de `docs/05-referencias-visuales/referencias.md`:

````markdown
# Referencias visuales · {cliente}

Dirección decidida por Belén: {no / sí, fecha}
{Con qué 2–3 se queda y qué pesa más de cada una, en dos o tres líneas.}

## Referencias

| Archivo | Qué es | Quién | Real / concepto |
| --- | --- | --- | --- |
| `fuente_descripcion.png` | [Nombre](url) | Cliente · Belén | Real · Concepto |

### fuente_descripcion

- **Composición:** {…}
- **Tipografía:** {…}
- **Fotografía:** {…}
- **Movimiento:** {… o —}
- **Qué no:** {…}
- **Dónde se usa:** {sección de `docs/02-estructura/estructura.md`}

## Choques con la marca
- {referencia}: {qué choca} → {cómo se resuelve} (ver `brand/README.md` §{n})

## Propuestas de Claude, sin elegir
<!-- Solo si Belén pidió ideas. No cuentan hasta que Belén las pase arriba. -->
````

## Dónde queda cada cosa

```
docs/05-referencias-visuales/
├── referencias.md               ← tabla, anotaciones y dirección decidida
└── fuente_descripcion.{png,webp} ← capturas, renombradas
```

- Una sola carpeta y un solo `referencias.md` por proyecto. Si hay muchas, se
  agrupan por apartado dentro del documento (hero, secciones, tipografía), no
  en subcarpetas vacías.
- Nada de aquí se sirve en la web ni se copia al código (xanasystem-web
  `docs/referencias-visuales/README.md` § Qué NO meter).
- Clays las tiene en `docs/01-referencias/inspiracion/` y xanasystem-web en
  `docs/referencias-visuales/`; en proyectos nuevos, fase 05.

## Reglas y trampas

- **Las elige Belén.** Claude no busca referencias por su cuenta y las
  presenta como dirección decidida. Si propone, van aparte y sin elegir.
- **Las del cliente, primero**, y marcadas como suyas.
- **Cada captura con su nota.** Una carpeta de imágenes con nombres de hash y
  sin anotación no dice qué se quería tomar (festival-viento `docs/referencias/`:
  cinco `.webp` sin nombre ni nota).
- **Renombrar al guardar**: `akeneo_home_hero.png`, no `Captura de pantalla
  2026-06-10.png`.
- **Un solo documento vivo.** Si la dirección cambia, se actualiza
  `referencias.md`; no se reparten notas de estilo entre varios documentos que
  luego se contradicen (xanasystem-web `docs/design.md` §4 apunta a
  `docs/fuentes/refs/`, pero las capturas están en `docs/referencias-visuales/`).
- **La marca manda sobre la referencia.** Si una referencia pide algo que el
  manual prohíbe, se toma el patrón y se resuelve con la regla de marca.
- **Las reglas de marca no se copian aquí**: se enlaza `brand/README.md`, que es
  la fuente única (clays `referencias.md` § Notas).
- **Concepto ≠ web real.** De un concepto de Dribbble se toma la composición; su
  fotografía es de stock y su contenido es relleno.
- **Sin dirección decidida no se cierra el design system.** Clays avanzó con las
  referencias abiertas y quedó como cuestión pendiente
  (`clays-of-the-world/CLAUDE.md` § Cuestiones abiertas).

## Checklist

- [ ] Referencias del cliente (si las dio) marcadas y con prioridad
- [ ] Referencias de Belén recibidas
- [ ] Capturas renombradas `fuente_descripcion.ext`
- [ ] Cada una con enlace, real o concepto, quién, composición, tipografía,
      fotografía, movimiento, qué no y dónde se usa
- [ ] Choques con la marca anotados y resueltos con enlace a `brand/README.md`
- [ ] Propuestas de Claude, si las hay, aparte y sin elegir
- [ ] **Dirección decidida por Belén**, con fecha, antes de los tokens

## Fuentes

clays-of-the-world (`docs/01-referencias/README.md`,
`docs/01-referencias/inspiracion/referencias.md`, `CLAUDE.md` § Cuestiones
abiertas) · xanasystem-web (`docs/referencias-visuales/README.md`,
`inspiracion/flowsync_landing-completa.md`, `inspiracion/nuroai_landing-completa.md`,
`docs/design.md` §4) · APC-gestion-patrimonial (`CLAUDE.md` § Materiales de
referencia: Velmora Wealth, First Financial Coaching, Kelly Duke) ·
festival-viento (`docs/referencias/`) · cuestionario de arranque
(`plantillas/cliente/arranque.md` §3).
