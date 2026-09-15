---
name: maquetar-seccion
description: Maquetar una sección de una web de Xana con un molde del design system y su contenido como datos — BEM y tokens, mobile-first y a prueba de traducción, legible sin JavaScript, accesible, con aparición al scroll sobria — y montar el layout base si aún no existe. Usar al construir o ajustar una sección o una página a partir de la estructura y los textos.
---

# Maquetar una sección

Todo el detalle está en el flujo [`flujos/maquetacion.md`](../../flujos/maquetacion.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo antes de empezar.**
Implementación de referencia: **clays-of-the-world** (`src/layouts/Base.astro`,
`src/styles/base.css`, `src/components/sections/`).

Si la sección viene de un nodo de Figma, es `xana:figma-a-seccion`. Cabecera,
pie, 404 y gracias tienen su skill: `xana:header`, `xana:footer`,
`xana:paginas-404-gracias`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Tokens, componentes base y moldes | `src/styles/tokens.css`, `src/components/` | Primero `xana:crear-tokens` y `xana:componentes-base` |
| La sección en la estructura (objetivo, dato, menú, CTA) | `docs/02-estructura/estructura.md` | Una sección nueva entra primero en la lista |
| Textos | `src/data/` o `docs/03-contenidos/` | Huecos con `Pending`; si faltan todos los datos de un bloque, el bloque sale |
| Referencias visuales | `docs/05-referencias-visuales/referencias.md` | Molde del design system sin más |
| Layout base (`Base.astro`) | `src/layouts/` | Montarlo primero (flujo §3 paso 1): `<head>`, fuentes, marca `data-reveal`, skip-link, cabecera, `<slot />`, pie, muro y script de aparición |

## 2. Pasos

1. **Elegir molde** del styleguide. Composición nueva solo si ninguno sirve, y
   se añade a la tabla de moldes.
2. **Datos** en `src/data/<idioma>.ts` con su tipo en `types.ts`: ningún texto
   en el marcado, ni siquiera los rótulos de los scripts.
3. **Marcado y estilos**: clases BEM, estilos scoped, **solo tokens**. Una sola
   `h1` por página; la sección abre en `h2`; sin encabezado visible, `aria-label`.
4. **Móvil primero** (flujo §5): una columna y la rejilla dentro de
   `@media (min-width: …)`; imagen primero en el marcado si ancla la sección;
   anchos de texto en `ch`; botones que aguanten un 30 % más de texto.
5. **Sin JavaScript no queda nada oculto** (flujo §6): el estado legible por
   defecto; lo que se anima solo se oculta con la marca en `<html>`.
6. **Aparición al scroll** (flujo §7): una sola vez, sobria, escalonado por
   contenedor. **Nada de titulares troceados** línea a línea ni palabra a
   palabra. Scroll-driven dentro de `@supports` y `no-preference`, con
   `overflow: clip`. Algo más elaborado → `xana:animar`.
7. **Accesibilidad** (flujo §8): foco visible, FAQ con `<details>`, `<dialog>`
   con enlaces reales, iconos con `aria-hidden`, contraste AA (sobre foto, medido).
8. **Reglas visuales**: no dos secciones oscuras seguidas (el pie cuenta), un
   solo fondo alternativo, antetítulo + titular.

## 3. Feedback

Cambio mínimo sobre lo pedido. Si toca un componente compartido o un token
global, se aísla para esa sección y se avisa.

## 4. Al terminar

Verificación del flujo §12, antes de decir que está:

- A 360, 768, 1024 y 1440 px, sin scroll horizontal.
- Sin JavaScript y con `prefers-reduced-motion`: todo visible, nada se mueve.
- Con teclado: foco visible en todo.
- Geometría con `getBoundingClientRect`; los colores, no con `getComputedStyle`.
- Peso e imágenes con `npm run build` + `npm run preview`.

Checklist del flujo §15 que toque. Belén revisa en la URL temporal. Commit por
**pieza terminada**, con su sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
