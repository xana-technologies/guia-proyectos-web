# Tipo · Web de producto

## Qué es

Web de varias páginas que explica un producto o software **módulo a módulo**,
con una página por módulo que posiciona su palabra clave, y que lleva a pedir
una demo. Referencia: **xanasystem-web**.

## Base + módulos

- **Base** completa (xanasystem-web aún no tiene legales ni muro: [deuda](../anexos/deuda-proyectos.md)).
- **[Multipágina](../modulos/multipagina.md)**.
- **[Formulario avanzado](../modulos/formulario-avanzado.md)**: formulario de
  demo en dos pasos.
- **[Movimiento avanzado](../modulos/movimiento-avanzado.md)** si se pide
  (xanasystem-web: reel con GSAP en la portada).

## Estructura por defecto

### Páginas

| Página | URL (xanasystem-web) | Objetivo |
| --- | --- | --- |
| Portada | `/` | Qué es la plataforma y hacia qué módulo ir |
| Una por módulo | `/pim`, `/dam`, `/api`, `/catalogos`, `/ia` | Posicionar cada módulo; `/pim` es la página madre del clúster |
| Empresa | `/empresa` | Quién hay detrás |
| Conversión | `/demo` (`/contacto` redirige) | Pedir demo o hacer una consulta |
| Área privada | `/acceso-clientes` | Enlace de acceso («Acceso clientes») |
| Base | legales, `/gracias`, 404 | — |

Fuente: `CLAUDE.md` regla 4 (alcance de la fase 1).

### Portada (`src/pages/index.astro`)

| # | Sección | Objetivo | Qué necesita del cliente |
| --- | --- | --- | --- |
| 1 | Hero con visual del producto | El dolor que resuelve, en el titular | Mensaje principal; capturas o producto real para el visual |
| 2 | Qué es | Definición citable de la plataforma | Definición aprobada |
| 3 | Problema | La situación de partida del cliente | Dolores reales de sus clientes |
| 4 | Módulos | Mapa de la plataforma y enlace a cada página | Lista de módulos y una línea de cada uno |
| 5 | Conexiones | Cómo se integra con lo que el cliente ya usa | Sistemas con los que conecta |
| 6 | Beneficios | Para qué tipo de empresa es | Público objetivo |
| 7 | Confianza | Clientes o sectores que lo usan | Logos o casos **con permiso** |
| 8 | FAQ | Objeciones habituales | Preguntas reales de ventas |
| 9 | CTA de cierre | Llevar a la demo | — |

### Página de módulo (`docs/design.md` §5)

| # | Sección | Objetivo | Qué necesita del cliente |
| --- | --- | --- | --- |
| 1 | Hero | Módulo + palabra clave + mockup | Nombre oficial del módulo |
| 2 | Problema | Datos o sistemas desconectados | Dolores del módulo |
| 3 | Definición | Qué es, en las primeras 100–150 palabras, con el acrónimo explicado | Definición aprobada |
| 4 | Diferenciación | Frente a lo que se confunde (PIM frente a ERP, PIM frente a DAM…) | Diferencias que defienden |
| 5 | Cómo funciona | Pasos del flujo | Flujo real del producto |
| 6 | Funcionalidades | Casos de uso en tarjetas | Lista de funcionalidades |
| 7 | Comparativa *(opcional)* | Tabla | — |
| 8 | Beneficios | Por capacidad, **nunca por equipo** | Beneficios verificables |
| 9 | Ecosistema | Relación con los otros módulos (componente único) | — |
| 10 | FAQ | Con `FAQPage` | Preguntas del módulo |
| 11 | Demo | **Cierra la página**; sustituye al CTA de cierre | — |

Reglas fijas de esas páginas (`CLAUDE.md` regla 14): beneficios por capacidad,
sin bloque de «señales» y la demo cierra.

### Conversión (`src/pages/demo.astro`)

Hero con formulario en dos pasos · Qué verás en la demo · Qué ocurre después ·
FAQ. Sin CTA de cierre: la página es la conversión.

## Particularidades

- **Terminología y CTA literales**, con lista de prohibidos (regla 5: «Xana
  PIM», nunca «Xana Product Manager»; «Solicitar demo»; «Acceso clientes»,
  nunca «Login»).
- **Sin claims sin evidencia** ni cifras sin fuente (regla 5).
- **Mockups con fotos reales de producto, variadas**, y nombre y código que
  casan con la foto (regla 9).
- **Enlazado en clúster** alrededor de la página madre (regla 7).
- **JSON-LD** `SoftwareApplication` + `Product` + `BreadcrumbList` en cada
  módulo; `Offer` solo si hay precio real (el precio 0 está en deuda).
- **Crecimiento por fases**: la navegación admite secciones futuras
  (`/soluciones`, `/recursos`…) que **no se crean** hasta su fase (regla 4).

## Ejemplos

**xanasystem-web**: `CLAUDE.md`, `docs/design.md` §5, `src/pages/index.astro`,
`pim.astro`, `demo.astro`, `src/components/sections/`.
