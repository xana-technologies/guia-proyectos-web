---
name: crear-manual-marca
description: Crear la identidad de un cliente de Xana que no tiene manual de marca proponiendo 3 opciones en una página web para elegir — cada una con nombre, paleta (nombre, hex, RGB y uso), proporción de uso, reglas de color, pareja tipográfica con su escala sobre textos reales y un recurso gráfico. Usar cuando no hay manual de marca y hay que proponer una identidad.
---

# Crear el manual de marca

Contexto en el flujo [`flujos/brand-board.md`](../../flujos/brand-board.md),
escenario 🔴 y «Qué lleva el brand board» (las rutas de esta skill son
relativas a su carpeta). El formato de las 3 propuestas lo decidió Belén el
15-09-2026 a partir del trabajo de marca de APC.

Si el cliente **tiene manual**, no es esta skill: es `xana:traducir-manual-marca`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Qué es la empresa, a quién va, tono | `docs/00-arranque/brief.md` | Pedirlo: sin esto las propuestas son decoración |
| Conclusiones del sector | `docs/01-investigacion/informe.md` | Seguir, y decirlo |
| Textos reales de la web | `docs/03-contenidos/` o brief | Frases del brief; nunca «lorem ipsum» |
| Logo | `brand/logo/` | Las propuestas se hacen igual; el logo llega después (como en APC) |
| Referencias visuales de Belén | `docs/05-referencias-visuales/` | Preguntar a Belén si quiere pasar alguna antes |

Explica a Belén el planteamiento de las 3 direcciones (en una línea cada una) y
espera su visto bueno antes de construir la página.

## 2. Qué lleva cada propuesta

Las **3 propuestas** tienen que ser direcciones distintas de verdad, no tres
variaciones de color. Cada una:

| Bloque | Qué incluye |
| --- | --- |
| **Nombre** | Número y nombre con carácter (en APC: «Marfil y terracota · El arco») |
| **Paleta** | Cada color con nombre, hex, RGB y **para qué se usa** (fondo, texto, acción, datos…) |
| **Proporción** | Barra con el peso de cada color en la web |
| **Reglas** | Normas de uso del color (en APC: el terracota solo en cursiva, arco, botón o filete) |
| **Tipografía** | Pareja con licencia web (fontsource) y escala por rol — display, título, cita, entradilla, cuerpo, etiqueta, dato — aplicada a **textos reales del cliente** |
| **Recurso gráfico** | La pieza propia de la marca, si la hay (en APC, «El arco») |

Contraste **calculado** de las combinaciones de texto de cada paleta, con su
veredicto (AA, AAA, solo grande, falla).

## 3. Pasos

1. Montar la página en el proyecto, por ejemplo `src/pages/pruebas-marca.astro`,
   con `noindex, nofollow`, rótulo visible «laboratorio · no publicada» y sus
   estilos y fuentes aislados en la página (flujo · Páginas de laboratorio).
2. Las tres propuestas una debajo de otra, con la misma estructura, para
   compararlas.
3. Comprobar en el navegador que se ve bien en escritorio y móvil.
4. Enseñársela a Belén, que elige (o mezcla). Anotar la elección y el porqué.

## 4. Al terminar

- La propuesta elegida pasa a `brand/README.md` con el formato del flujo, y de
  ahí al brand board visual con `xana:crear-brand-board`.
- **La página de laboratorio se borra** (o se renombra con `_` delante) antes de
  publicar.
- El cliente aprueba la marca antes del design system.
