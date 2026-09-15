# Tipo · Web de evento

## Qué es

Una landing larga de un evento con fecha: qué es, programa, dónde es, cómo
llegar y quién lo apoya. Público local y en móvil. Referencia:
**festival-viento** (Festival Internacional del Viento de Castellón, 5–7 de
junio de 2026).

## Base + módulos

- **Base** completa. festival-viento no la tiene y su `CLAUDE.md` pide evitar
  cookies y legales; la regla de la guía es que toda web publicada la lleva
  ([fuera de la guía](../anexos/fuera-de-la-guia.md)). Decidirlo con Belén al
  arrancar.
- **[Evento](../modulos/evento.md)**: programa, calendario, sorteo, caducidad.
- **[Multimedia](../modulos/multimedia.md)**: vídeo del hero, tráiler, fotos de
  ediciones anteriores.

## Estructura por defecto

Orden de `festival-viento/src/pages/index.astro`:

| # | Sección | Objetivo | Qué necesita del cliente |
| --- | --- | --- | --- |
| 1 | **Hero** | Qué, cuándo, dónde y si es gratis; CTA único «Ver el programa» | Fechas, lugar, precio, cartel o vídeo |
| 2 | **Ven a vivirlo** | Tipos de actividad, cada uno con su gancho | Lista de actividades y fotos de la edición anterior |
| 3 | **Programa** | Qué hay, cuándo y dónde, filtrable por día, zona y tipo | Programa completo (hoja de cálculo) y PDF si lo hay |
| 4 | **Todo lo que necesitas saber** | Lo que hay que saber antes de ir (horarios amplios, gratuito, para toda la familia) y tráiler | Horarios, precio, público; vídeo propio |
| 5 | **Mapa** | Dónde está cada zona | Zonas y qué pasa en cada una |
| 6 | **Actividad destacada** *(si hay)* | Lo que más atrae (festival-viento: globos aerostáticos) | Horarios, condiciones, fotos |
| 7 | **Información práctica** | Cómo llegar y aparcar | Transporte, aparcamientos, accesos |
| 8 | **Sorteo o promoción** *(si hay)* | Participar | Bases, mecánica, quién gestiona los datos |
| 9 | **Llamada final** | Última invitación a ir o a ver el programa | — |
| 10 | **Patrocinadores** | Reconocer a quien apoya | Logos en SVG y si hay niveles |
| 11 | **Pie** | Contacto, redes, anclas y los 4 elementos legales | Email, redes, titular legal |

El brief proponía además una franja de «datos rápidos» (fechas · lugar ·
acceso gratuito) tras el hero (`docs/project-brief.md` §3); en lo construido
van dentro del hero.

## Particularidades

- **Móvil primero**: público local y familiar que entra desde el móvil
  (`CLAUDE.md` regla 5).
- **Un CTA** en el hero, al programa.
- **Mapa sin embeds**: SVG o imagen con enlace a Google Maps; lo construido
  con My Maps sin consentimiento es lo que no se hace.
- **Vídeo propio**, no Vimeo ni YouTube (regla 3).
- **Patrocinadores al mismo nivel** si el cliente no pide niveles.
- **Web no oficial**: si la hace Xana y no el organismo, las instituciones van
  como patrocinadores y no se usan sus datos de contacto ni sus legales
  (regla 2).
- **Fechas en un solo sitio** (`src/data/site.ts`), también en el JSON-LD y en
  los `.ics`.
- **Qué pasa al acabar**: decidirlo en el arranque (siguiente edición, archivo,
  retirada).
- Solo castellano en la primera edición, diseñado para que entren más idiomas
  (regla 6) → [idiomas](../modulos/idiomas.md).

## Ejemplos

**festival-viento**: `CLAUDE.md`, `docs/project-brief.md`,
`src/pages/index.astro`, `src/components/`, `src/data/`. Bloque de evento
dentro de una landing: clays-of-the-world (sección TECNA 2026).
