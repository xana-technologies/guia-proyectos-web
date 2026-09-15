---
name: proponer-estructura
description: Seguir, completar o proponer la estructura de una web de Xana (páginas, secciones y orden, objetivo y dato de cada sección, móvil, CTA, estados) en docs/02-estructura/estructura.md, lista para que el cliente la apruebe. Usar en la fase de estructura, al recibir un wireframe o lista de secciones, o al añadir o quitar una sección.
---

# Proponer la estructura

Todo el detalle está en el flujo [`flujos/estructura.md`](../../flujos/estructura.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar**: el formato de `estructura.md` está dentro.

## 1. Qué escenario es

| Escenario | Qué se hace |
| --- | --- |
| 🟢 Wireframe completo | Se sigue. Cada desvío se anota con qué y por qué. Aun así, completar lo que falte (móvil, formulario) |
| 🟡 Lista o boceto | Completar móvil, páginas que van siempre, destino de cada CTA, estados, orden y objetivo de cada sección |
| 🔴 No hay nada | Estructura por defecto del [tipo de proyecto](../../tipos-de-proyecto/README.md), ajustada con la investigación y sin secciones que el cliente no puede sostener |

## 2. Antes de empezar

- Lee las conclusiones de `docs/01-investigacion/informe.md`. Si no hay
  investigación, dilo: la propuesta saldrá solo del tipo de proyecto.
- Guarda lo que mande el cliente en `docs/02-estructura/material/`.

## 3. Pasos

1. Escribir `docs/02-estructura/estructura.md` con el formato del flujo, marcando
   qué viene del cliente y qué es propuesta.
2. Cada sección con **objetivo** (lo que consigue en quien lee), **dato que
   necesita** y **origen**; etiqueta de menú en las secciones ancla.
3. **Siempre en la lista**: aviso legal, privacidad, cookies, gracias y 404.
4. **Destino de cada CTA** y **estados** (formulario, dato pendiente, sin JavaScript).
5. **Móvil**: qué cambia. Si no se decide, anotar qué falta.
6. Multipágina: tabla de URLs, palabra clave **propuesta** (la confirma Eva),
   enlaces internos, alcance de la fase y redirecciones si hay web antigua →
   módulo [multipágina](../../modulos/multipagina.md).
7. Si la lista no basta para entender la composición, **wireframe rápido**:
   cajas grises y rótulos, escritorio y móvil (flujo · Wireframe rápido).
8. Pasar a `CHECKLIST.md` los datos que cada sección necesita y aún no han llegado.

## 4. Reglas que no se saltan

- **Sin dato no hay sección**: si solo se sostiene con cifras que no existen,
  sale o se propone condicionada.
- Dos secciones con el mismo objetivo: sobra una.
- Contenido dentro de un modal → hace falta la página que lo sustituye sin JavaScript.
- No crear páginas fuera del alcance de la fase.

## 5. Al terminar

- Recorre la checklist **02 · Estructura** del flujo.
- Pide a Belén que la revise y la envíe al cliente. Los cambios que pida se
  anotan en «Desvíos y cambios» con fecha.
- Con la estructura aprobada sigue `xana:palabras-clave`.
