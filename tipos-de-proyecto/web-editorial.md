# Tipo · Web editorial / de servicios

## Qué es

Web de varias páginas de una empresa de servicios o de un profesional, donde el
contenido pesa: explica el enfoque, presenta a la persona que hay detrás y
suele crecer con un hub de conocimiento. Referencia:
**APC-gestion-patrimonial** (gestión patrimonial, «by Cristina Alonso»).

## Base + módulos

- **Base** completa (en APC, formulario sin conectar y legales en borrador:
  [deuda](../anexos/deuda-proyectos.md)).
- **[Multipágina](../modulos/multipagina.md)**.
- **[Blog](../modulos/blog.md)** cuando haya contenido (en APC el hub está
  maquetado con contenido de muestra).
- Si la actividad es regulada: [§ Actividad regulada](../plantillas/legales/parrafos-por-modulo.md#actividad-regulada).

## Estructura por defecto

### Páginas (APC `CLAUDE.md` § Arquitectura del sitio)

| Página | URL | Objetivo |
| --- | --- | --- |
| Inicio | `/` | Situar a la persona y su momento; llevar a hablar |
| Sobre la empresa | `/sobre-apc/` | Marca primero y la persona experta detrás |
| Servicios | `/gestion-patrimonial/` | Enfoque, servicios, proceso y FAQ en una página |
| Conocimiento | `/conocimiento/` | Hub de contenido |
| Contacto | `/contacto/` | Formulario; CTA permanente del sitio («Hablemos») |
| Legales | 3 + `/informacion-financiera/` si es regulada | — |
| Sistema | `/gracias/` (noindex), 404 | — |

### Inicio (`src/pages/index.astro`)

| # | Sección | Objetivo | Qué necesita del cliente |
| --- | --- | --- | --- |
| 1 | Hero | La promesa, desde la persona («Tu patrimonio se construye con decisiones…») | Mensaje principal |
| 2 | Servicios | Qué se ofrece, en tres líneas que enlazan a su ancla | Servicios y su nombre |
| 3 | Momento | Que el visitante se reconozca: situaciones vitales que se van relevando («Quizá el tuyo sea…») y enlace a contacto | Situaciones reales por las que acuden sus clientes |
| 4 | La persona | Autoridad detrás de la marca | Biografía, acreditaciones, foto |
| 5 | Conocimiento | Dos contenidos destacados | Artículos (o se retira hasta tenerlos) |
| 6 | Cierre | Llevar a contacto | — |

### Servicios (`src/pages/gestion-patrimonial.astro`)

Hero · Punto de partida («Antes de hablar de soluciones, necesitamos comprender
tu historia») · Servicios en tarjetas · Proceso en pasos · Para quién ·
Dimensión humana · FAQ con `FAQPage` · Cierre. Necesita del cliente: servicios,
proceso real, público, preguntas frecuentes y condiciones (en APC, inversión
mínima de 3.000 €; lo no confirmado se omite, como la pregunta de modalidad).

### Sobre la empresa (`src/pages/sobre-apc.astro`)

Hero · La persona · Origen · Principios · Enfoque («Acompañar no significa
decidir por ti») · Cierre. Necesita: historia, principios, trayectoria
**confirmada**.

### Contacto (`src/pages/contacto.astro`)

Principal con `h1` («Cuéntanos en qué momento estás») y formulario · Qué pasa
después.

## Particularidades

- **Empezar por la persona y su momento; después el servicio** (APC
  `CLAUDE.md` § Reglas de tono).
- **Actividad regulada**: sin promesas de rentabilidad, urgencia ni escasez;
  aviso educativo en contenidos y pie; página de información financiera; el
  proyecto **requiere revisión jurídica**.
- **Marca frente a persona**: la marca va primero; la empresa donde trabaja la
  persona solo aparece como trayectoria, nunca como prestadora (APC: Nuvion
  Capital).
- **JSON-LD** `ProfessionalService` + `Person` en el layout; `BreadcrumbList`
  en cada página; `CollectionPage` en el hub; `Article` en las fichas.
- **CTA permanente** en cabecera y cierres, a la misma página de contacto.
- **Tono editorial y movimiento sobrio** (titulares troceados, rechazados) →
  [movimiento avanzado](../modulos/movimiento-avanzado.md).
- **Datos pendientes, visibles**: email, teléfono, biografía y legales se
  marcan y no se publican hasta confirmarlos.

## Ejemplos

**APC-gestion-patrimonial**: `CLAUDE.md`, `src/config/site.ts`,
`src/pages/*.astro`, `src/components/sections/<pagina>/`,
`src/layouts/BaseLayout.astro`, memorias `acp-arquitectura-web` y
`apc-estado-desarrollo`.
