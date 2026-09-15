---
name: analizar-briefing
description: Leer el briefing de una web nueva de Xana (y el cuestionario del cliente o la web antigua si los hay) y sacar qué tipo de proyecto es, qué módulos lleva, el escenario 🟢🟡🔴 de cada fase y si hay web antigua, dejándolo en docs/00-arranque/brief.md. Usar al empezar un proyecto o cuando llega el cuestionario de arranque devuelto.
---

# Analizar el briefing

Todo el detalle está en el flujo [`flujos/arranque-proyecto.md`](../../flujos/arranque-proyecto.md)
§2 y §4.1 (las rutas de esta skill son relativas a su carpeta). Léelo antes de
empezar, junto con [`README.md`](../../README.md) y
[`fases/00-arranque.md`](../../fases/00-arranque.md).

Esta skill **decide y apunta**; no crea el repositorio
(`xana:preparar-proyecto`) ni prepara lo que se pide al cliente (`xana:kit-cliente`).

## 1. Qué leer

- El briefing, en el formato que llegue. Si es PDF o docx, pásalo a markdown en
  `docs/00-arranque/material/` para poder citarlo.
- El cuestionario devuelto, si ya está ([`plantillas/cliente/arranque.md`](../../plantillas/cliente/arranque.md)):
  al final de cada plantilla del cliente hay un bloque «*Para Claude*» con qué
  hacer con cada respuesta.
- La web antigua, si el dominio tiene una.

## 2. Qué sacar

Tabla «Qué se saca del briefing» del flujo §2:

| Qué | Cómo se decide |
| --- | --- |
| **Tipo de proyecto** | Receta de [`tipos-de-proyecto/`](../../tipos-de-proyecto/README.md). Si no encaja en ninguno: base + módulos |
| **Módulos** | Idiomas, número de páginas, blog, evento, vídeos, animación, formularios especiales → fichas de [`modulos/`](../../modulos/README.md) |
| **Escenario de cada fase** | 🟢🟡🔴 de investigación, estructura, contenidos, marca, imágenes y vídeos; caso A/B/C de Figma. Lo que no se sepa: «por confirmar con el cuestionario» |
| **Web antigua** | Sí o no. Si sí: módulo [migración](../../modulos/migracion.md), inventario de URLs, GTM/GA y legales existentes |
| **Si sigue «Astro + base»** | Si no (prototipo, WordPress existente, aplicación) → [`anexos/fuera-de-la-guia.md`](../../anexos/fuera-de-la-guia.md) y avisar a Belén |

**No se inventa nada**: lo que el briefing no dice queda como pregunta, no como
suposición.

## 3. Pasos

1. Escribir `docs/00-arranque/brief.md`: qué es la empresa, a quién va la web,
   qué tiene que conseguir, idiomas, tipo de proyecto y módulos.
2. Añadir la tabla de escenarios y la lista de preguntas abiertas.
3. Si hay web antigua: `docs/00-arranque/urls-web-antigua.md` con sus URLs
   (sitemap antiguo o Search Console) y anotar GTM/GA y textos legales.
4. Si el repo tiene algo publicado en `main` (una *coming soon*), **preguntar a
   Belén antes de tocar `main`** (flujo §2, Situaciones).
5. Si `CLAUDE.md` y `CHECKLIST.md` del proyecto ya existen, pasar ahí tipo,
   módulos y escenarios. Si no, quedan en `brief.md` para `xana:preparar-proyecto`.

## 4. Al terminar

- Enseña a Belén el tipo, los módulos, los escenarios y las preguntas abiertas,
  y espera su visto bueno: de esto dependen el kit del cliente y el proyecto.
- Recorre la checklist **Decidir** del flujo §8.
