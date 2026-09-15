# Guía de proyectos web · Xana

Cómo se hace una web en Xana, de la primera reunión al día después de publicar.
Sale de los proyectos reales de `C:\Proyectos` (clays-of-the-world,
xanatechnologies, xanasystem-web, magnaceramica, APC-gestion-patrimonial,
festival-viento, grupomerhi) y del *Protocolo de publicación ESTÁNDAR*.

**Está pensada sobre todo para Claude**: cuando empieza un proyecto, Claude la
lee, sabe lo que va siempre y prepara las plantillas sin que haya que reenviar
información. También tiene partes para el equipo (Eva, ingeniería) y
documentos para el cliente.

> **Estado (15-09-2026):** primera redacción completa. Cada dato cita el
> proyecto del que sale; las decisiones pendientes están marcadas en cada
> documento y la deuda de los proyectos actuales, en
> [`anexos/deuda-proyectos.md`](anexos/deuda-proyectos.md).

## Reglas generales

- **Toda web publicada es Astro + base.** La base lleva siempre: formulario,
  textos legales, muro de cookies y analítica (GTM + GA4). Lo que no sigue la
  guía está en [`anexos/fuera-de-la-guia.md`](anexos/fuera-de-la-guia.md).
- **Lo que manda el cliente varía.** Las fases de entrada tienen tres
  escenarios: 🟢 nos lo dan completo · 🟡 a medias · 🔴 no nos lo dan.
- **No se inventan datos.** Lo que falta se marca `[[ ]]` y no se publica.
- **Commit y push, solo con permiso. La publicación en Vercel la hace siempre
  Belén** con un commit manual desde GitHub. Detalle en
  [`forma-de-trabajo.md`](forma-de-trabajo.md).

## Cómo está organizada

| Carpeta | Qué es |
| --- | --- |
| [`fases/`](fases/README.md) | **El proceso**, en orden: qué se hace, quién, qué queda hecho |
| [`flujos/`](flujos/README.md) | **Las tareas que se repiten**, paso a paso, con sus plantillas y reglas |
| [`modulos/`](modulos/README.md) | Extras que solo llevan algunas webs (idiomas, multipágina…) |
| [`tipos-de-proyecto/`](tipos-de-proyecto/README.md) | Base + módulos + estructura por defecto para cada tipo de web |
| [`plantillas/`](plantillas/README.md) | Ficheros listos para copiar: cliente, equipo, proyecto, legales, código |
| [`skills/`](skills/README.md) | **El plugin `xana`**: tareas concretas que Claude carga solas en cada proyecto |
| [`trampas-y-lecciones.md`](trampas-y-lecciones.md) | Lo que ya ha fallado: síntoma → causa → solución |
| [`forma-de-trabajo.md`](forma-de-trabajo.md) | Cómo se colabora: planes, feedback, commits, permisos |
| [`anexos/`](anexos/) | Lo que queda fuera de la guía y la deuda de los proyectos actuales |

## Las fases de un vistazo

| Fase | Necesita antes |
| --- | --- |
| [00 · Arranque](fases/00-arranque.md) | — |
| [01 · Investigación](fases/01-investigacion.md) | 00 |
| [02 · Estructura](fases/02-estructura.md) | 01 |
| [03 · Contenidos y SEO](fases/03-contenidos-seo.md) | 02 |
| [04 · Marca](fases/04-marca.md) | 00 |
| [05 · Design system](fases/05-design-system.md) | 04 |
| [06 · Diseño en Figma](fases/06-diseno-figma.md) | 02 y 05 |
| [07 · Construcción](fases/07-construccion.md) | 03, 05 (y 06 en el caso A) |
| [08 · Revisión antes de publicar](fases/08-revision.md) | 07 |
| [09 · Publicación](fases/09-publicacion.md) | 08 |
| [10 · Entrega y mantenimiento](fases/10-entrega-mantenimiento.md) | 09 |

## Quién es quién

| Rol | Quién |
| --- | --- |
| Diseño, dirección del proyecto y publicación | Belén |
| Construcción, documentación, revisión | Claude |
| SEO y palabras clave | Eva |
| Claves de Brevo, dominios de envío, IP de oficina | Ingeniería (Aaron, Alejandro) |
| Datos, material y aprobaciones | Cliente |
