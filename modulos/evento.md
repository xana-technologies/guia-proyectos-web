# Módulo · Evento

Webs de un evento con fecha (festival, feria, campaña): programa como datos y
filtrable, calendario, mapa y cómo llegar, sorteos y qué pasa con la web cuando
el evento acaba. Referencia: **festival-viento** (Festival Internacional del
Viento de Castellón, 5–7 de junio de 2026). También clays-of-the-world, con una
sección de feria que caduca.

## Cuándo se usa (y cuándo no)

- **Sí:** la web existe por un evento con días, lugares y actividades
  (festival-viento), o una web corporativa lleva un bloque de un evento
  concreto (clays: presencia en TECNA 2026).
- **Solo el bloque de caducidad**, sin programa ni calendario, cuando el evento
  es una sección más de otra web (clays).
- **No:** una agenda que se actualiza siempre es contenido → [blog](blog.md).

## Qué añade al código

### Programa como datos tipados

Una fuente única en `src/data/`, un fichero por entidad, con ids tipados
(festival-viento):

| Fichero | Qué es |
| --- | --- |
| `dias.ts` | `DiaId`, nombre visible y fecha ISO (`'2026-06-05'`) |
| `zonas.ts` | `ZonaId`, nombre, descripción y `grupo` (`principal`, `chiringuito`, `otro`) |
| `tipos.ts` | `TipoId`, nombre, plural corto y descripción |
| `programa.ts` | Las actividades |
| `site.ts` | Nombre, edición, fechas, lugar, contacto, redes |

```ts
// src/data/programa.ts
export type Actividad = {
  id: string;
  titulo: string;
  dia: DiaId;
  zona: ZonaId;
  inicio: string;            // HH:MM
  fin?: string;              // si falta, es puntual
  cruzaMedianoche?: boolean; // «22:00 a 01:00»
  tipos: TipoId[];
  descripcion?: string;
  pendienteConfirmacion?: boolean;
};
```

El cliente mandó el programa en Excel (`docs/project-brief.md` §4); se pasa a
estos datos y el componente no conoce ningún texto.

### Programa filtrable sin librerías

`src/components/Programa.astro`:

- Pestañas por día (`role="tablist"`, `aria-selected`) y dos desplegables de
  filtro, zona y tipo (`role="listbox"`, `aria-expanded`).
- Cada actividad lleva `data-dia`, `data-zona` y `data-tipos`; el script
  alterna `hidden` y muestra «No hay actividades que coincidan» si no queda
  ninguna. Botón «Limpiar».
- Actividades ordenadas por día y hora de inicio en el servidor.
- El PDF del programa se sirve desde R2 con `target="_blank"`, no con
  `download`, que el navegador ignora en URL de otro dominio (commit `6ce84cc`).

### Calendario `.ics` por actividad

`src/pages/ics/[id].ics.ts` genera un fichero por actividad en el build:

```ts
export function getStaticPaths() {
  return actividades.map((act) => ({ params: { id: act.id }, props: { act } }));
}

const dtstart = toIcsDate(dia.fecha, act.inicio);
const fechaFin = act.cruzaMedianoche ? nextDay(dia.fecha) : dia.fecha;
const dtend = toIcsDate(fechaFin, act.fin ?? addHour(act.inicio));
```

Devuelve `text/calendar` con líneas separadas por `\r\n`. **Estado:** el botón
se quitó de la interfaz y el endpoint sigue publicado (commit `6ce84cc`). El
`.ics` por día que pedía el brief no se hizo.

### JSON-LD del evento

En el layout (festival-viento `src/layouts/Base.astro`), desde `site.ts`:

```ts
{
  '@type': 'Festival',
  name: site.nombre,
  startDate: site.fechaInicio,
  endDate: site.fechaFin,
  eventStatus: 'https://schema.org/EventScheduled',
  eventAttendanceMode: 'https://schema.org/OfflineEventAttendanceMode',
  location: { '@type': 'Place', name: site.lugar, address: { … } },
  isAccessibleForFree: true,
}
```

### Mapa y cómo llegar

- **Decidido en festival-viento:** mapa SVG ilustrado con las zonas y enlace
  externo a Google Maps; sin embeds (`CLAUDE.md` regla 5, brief §3).
- **Lo construido:** un `<iframe>` de Google My Maps sin consentimiento
  (`src/components/Mapa.astro`). Es lo que no hay que hacer → [trampas ·
  Analítica y cookies](../trampas-y-lecciones.md#analítica-y-cookies) y
  [multimedia](multimedia.md).
- Cómo llegar como texto (transporte, aparcamientos) en su propia sección
  (`InfoPractica.astro`, `src/data/info-practica.ts`).

### Sorteos

`src/components/Sorteo.astro`: pasos para participar, centros adheridos,
condiciones del día del premio y un hueco para el **QR de un formulario
externo** (`TODO`) donde se adjunta la foto del ticket. Las bases y el
formulario no están en la web.

### Caducidad

Clays deja la sección de la feria lista para quitarse
(`src/pages/[...lang]/index.astro`):

```astro
{/* Sección con fecha de caducidad: pasada la feria se borra esta línea y el
    bloque `event` de los datos. No hay que tocar nada más. */}
<Event content={content.event} />
```

En festival-viento no está decidido qué pasa con la web al acabar; el brief
habla de futuras ediciones.

## Qué cambia en la base

| Pieza | Qué cambia |
| --- | --- |
| Base | **Se aplica igual** (formulario, legales, muro y analítica): toda web publicada la lleva ([fuera de la guía](../anexos/fuera-de-la-guia.md)). festival-viento no la tiene y su `CLAUDE.md` (regla 3) pide evitar cookies y legales: decidir con Belén al arrancar |
| Legales | Titular del sitio claro (en festival-viento está pendiente: asociación o Xana). Si hay mapa o vídeo de terceros → [§ Contenido incrustado](../plantillas/legales/parrafos-por-modulo.md#contenido-incrustado-vimeo-youtube-google-maps). Si el sorteo recoge datos en la web → tratamiento nuevo (sin texto base: revisión jurídica) |
| Formulario | Si el sorteo pide adjuntos, no lo cubre la base → [formulario avanzado](formulario-avanzado.md) |
| SEO | JSON-LD `Event`/`Festival` con fechas reales; `og:image` absoluta (festival-viento la tiene relativa: [deuda](../anexos/deuda-proyectos.md)) → [SEO técnico](../flujos/seo-tecnico.md) |
| Sitemap | Los `.ics` no son páginas: fuera |
| Revisión | Fechas iguales en `site.ts`, textos, JSON-LD y `.ics` |

## Qué pedir al cliente de más

- **Programa completo** (hojas de cálculo valen), con día, hora de inicio y fin,
  lugar y tipo de cada actividad; qué está sin confirmar.
- **Zonas y cómo llegar**: transporte, aparcamientos, accesos.
- **Logos de patrocinadores en SVG** y si hay niveles (festival-viento: todos
  al mismo nivel).
- **Bases del sorteo** y quién gestiona el formulario y los datos.
- **Titular legal del sitio** y qué pasa con la web después del evento.
- Cartel, fotos y vídeo de ediciones anteriores → [`cliente/material.md`](../plantillas/cliente/material.md).

## Reglas y trampas

- **Una fecha, un sitio.** En festival-viento `site.ts` tiene `fechasTexto`
  «5, 6 y 7 de junio» y `fechasCortoTexto` «6 y 7 de junio» (deuda). Los textos
  de fechas se derivan de los datos.
- **`.ics` con zona horaria.** El de festival-viento escribe horas sin zona
  (`DTSTART:20260605T100000`) y cada calendario las interpreta a su manera
  (deuda).
- **`.ics` con escapado en todos los campos de texto**: hoy solo se escapan
  las comas y saltos de `DESCRIPTION`.
- **`UID` con el dominio real**: el `.ics` usa `festivaldelvientocastellon.es`
  y `site.url` es `festivalviento.com`.
- **Instituciones y colaboradores, como patrocinadores**, no como titulares ni
  como contacto (festival-viento `CLAUDE.md` regla 2, memoria
  `project_ownership`).
- **Público en móvil**: móvil primero (festival-viento regla 5).
- Un bloque con fecha se quita en un solo sitio: la línea de la página y su
  bloque de datos.

## Checklist

**Construcción**
- [ ] Días, zonas, tipos y actividades tipados en `src/data/`
- [ ] Programa filtrable por día, zona y tipo, sin librerías
- [ ] Mensaje de «sin resultados» y botón para limpiar filtros
- [ ] `.ics` con zona horaria, escapado y `UID` del dominio real (si se enlaza)
- [ ] Actividades que cruzan medianoche terminan al día siguiente
- [ ] JSON-LD `Event`/`Festival` con fechas de `site.ts`
- [ ] Mapa sin embeds, o con consentimiento
- [ ] Base completa: formulario, legales, muro, analítica

**Revisión**
- [ ] Las fechas coinciden en todos los sitios
- [ ] Un `.ics` abierto en Google Calendar y en Outlook da la hora correcta
- [ ] Filtros usables con teclado y en móvil
- [ ] Decidido qué pasa con la web al acabar el evento

## Proyecto de referencia

- **festival-viento**: `CLAUDE.md`, `docs/project-brief.md`,
  `src/data/{dias,zonas,tipos,programa,site}.ts`, `src/components/Programa.astro`,
  `Mapa.astro`, `Sorteo.astro`, `src/pages/ics/[id].ics.ts`,
  `src/layouts/Base.astro`, commit `6ce84cc`.
- **clays-of-the-world**: `src/pages/[...lang]/index.astro` (sección `Event`
  con caducidad).
