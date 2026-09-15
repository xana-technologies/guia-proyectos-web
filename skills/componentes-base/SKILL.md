---
name: componentes-base
description: Montar los componentes base de una web de Xana (Button, Logo, Section, SectionHeader, Pending), documentar los moldes de sección y crear la página /styleguide con noindex, y gestionar las páginas de laboratorio. Usar al montar el design system después de los tokens, al crear un molde nuevo o al preparar el styleguide.
---

# Componentes base, moldes y styleguide

Todo el detalle está en el flujo [`flujos/design-system.md`](../../flujos/design-system.md)
§6, §7, §8 y §9 (las rutas de esta skill son relativas a su carpeta). **Léelo
antes de empezar.** Implementación de referencia:
`clays-of-the-world/src/components/` y `src/pages/styleguide.astro`.

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| `src/styles/tokens.css` | Hacer antes `xana:crear-tokens` |
| Versiones del logo y sus reglas en `brand/README.md` | `Logo` sin las versiones que falten, anotado |

## 2. Componentes base

Los cinco de la tabla del flujo §6, copiados de clays y adaptados:

- **`Button`**: `<a>` con `href`, `<button>` sin él. El contraste decide las
  variantes; la variante sobre oscuro va **en el botón**, no repetida en cada
  sección. **Todo CTA usa `Button`.**
- **`Logo`**: las reglas del manual, con aviso en consola si baja del ancho
  mínimo o se pide una versión que no existe.
- **`Section`**: ritmo vertical, fondo y ancla; `data-surface="dark"` en las
  inversas.
- **`SectionHeader`**: antetítulo + titular, titular con `max-width` en `ch`.
- **`Pending`**: la marca `[[ ]]`, con aviso para lector de pantalla. Nada con
  `Pending` se publica.

Solo tokens semánticos: ningún valor a pelo.

## 3. Moldes de sección

- Las secciones **eligen molde**; inventar composición es la excepción.
- Los moldes reciben el contenido como datos, nunca texto incrustado.
- Se construyen a medida que se montan las primeras secciones; referencia en la
  tabla del flujo §7.
- Molde nuevo → a la tabla del styleguide con forma, cuándo y dónde se usa. Un
  molde sin uso se marca, no se borra sin preguntar.

## 4. Styleguide

1. `src/pages/styleguide.astro` con `robots="noindex, nofollow"`, sin cabecera,
   pie ni muro (`chrome={false}`), en español y **fuera del sitemap**, sin
   `Disallow` en `robots.txt`.
2. Secciones: logotipo, paleta, tipografía, componentes, superficies, texto
   sobre fotografía, layout, moldes (con ritmo de superficies) y **tabla de
   contrastes**.
3. Revisar las reglas visuales del flujo §8 (no dos secciones oscuras seguidas,
   un solo fondo alternativo, un peso por titular…).

## 5. Páginas de laboratorio

Para comparar opciones con Belén (flujo §9): `noindex, nofollow` mientras se
decide, componentes en `src/components/pruebas/`, y **al elegir se borra** (o
prefijo `_`). Antes de publicar, ninguna `pruebas-*` ni `demo-*` sin prefijo.

## 6. Al terminar

- Comprueba el styleguide en el navegador, escritorio y móvil.
- Checklists **Componentes y moldes** y **Styleguide y laboratorios** del flujo §12.
- **Belén revisa el styleguide** y decide. Si el proyecto va a Figma, lo
  siguiente es la librería en Figma ([`flujos/diseno-figma.md`](../../flujos/diseno-figma.md)).
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
