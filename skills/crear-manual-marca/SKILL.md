---
name: crear-manual-marca
description: Crear la identidad de un cliente de Xana que no tiene manual de marca proponiendo 3 opciones en una página web para elegir — cada una con nombre, paleta (nombre, hex, RGB y uso), proporción de uso, reglas de color, pareja tipográfica con su escala sobre textos reales y un recurso gráfico. Usar cuando no hay manual de marca y hay que proponer una identidad.
---

# Crear el manual de marca

Todo el detalle está en el flujo [`flujos/brand-board.md`](../../flujos/brand-board.md),
escenario 🔴 y apartado **«Tres propuestas de marca»** (qué lleva cada
propuesta) (las rutas de esta skill son relativas a su carpeta). **Léelo antes
de empezar.**

Es el paso que más pesa de la fase de marca: **de la propuesta elegida sale el
brand board** (`xana:crear-brand-board`). Si el cliente **tiene manual**, no es
esta skill: es `xana:traducir-manual-marca`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Qué es la empresa, a quién va, tono | `docs/00-arranque/brief.md` | Pedirlo: sin esto las propuestas son decoración |
| Conclusiones del sector | `docs/01-investigacion/informe.md` | Seguir, y decirlo |
| Textos reales de la web | `docs/03-contenidos/` o brief | Frases del brief; nunca «lorem ipsum» |
| Logo | `brand/logo/` | Las propuestas se hacen igual; el logo puede llegar después |
| Referencias visuales de Belén | `docs/05-referencias-visuales/` | Preguntar a Belén si quiere pasar alguna antes |

Explica a Belén las 3 direcciones en una línea cada una y espera su visto
bueno antes de construir la página.

## 2. Pasos

1. Montar la página de laboratorio en el proyecto (por ejemplo
   `src/pages/pruebas-marca.astro`) con las reglas del flujo · Páginas de
   laboratorio: `noindex, nofollow`, rótulo visible, estilos y fuentes aislados.
2. Las **3 propuestas** una debajo de otra, con los bloques de la tabla del flujo
   y la misma estructura en las tres. Direcciones distintas de verdad.
3. **Contraste calculado** de las combinaciones de texto de cada paleta.
4. Tipografías con licencia web; textos reales del cliente.
5. Comprobar la página en escritorio y móvil.
6. Enseñársela a Belén, que **elige una** (o mezcla). Anotar la elección y el
   porqué en `brand/README.md`.

## 3. Al terminar

- La propuesta elegida pasa a `brand/README.md` con el formato del flujo y de
  ahí al brand board con `xana:crear-brand-board`.
- **La página de laboratorio se borra** (o se renombra con `_` delante) antes de
  publicar.
- El cliente aprueba la marca antes del design system.
