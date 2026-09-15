---
name: escribir-textos
description: Escribir, adaptar o montar los textos de una web de Xana — copy por sección con su origen marcado, tono de la marca o el de por defecto, [[ ]] donde falta un dato, antetítulos, textos de formulario, 404, gracias y alt, y paso del copy aprobado a src/data/. Usar al redactar o revisar textos, al recibir textos del cliente o al pasarlos al código.
---

# Escribir los textos

Todo el detalle está en el flujo [`flujos/contenidos-seo.md`](../../flujos/contenidos-seo.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar**: tono, `[[ ]]`, antetítulo, contenido como datos y el formato del
copy están ahí.

La regla que manda: **no se inventa nada**. Ni cifras, ni productos, ni
clientes, ni certificaciones. Lo que no se sabe va en `[[ ]]` o no se escribe.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Estructura aprobada | `docs/02-estructura/estructura.md` | Parar y decirlo |
| Tono y terminología | Manual de marca, brief, `CLAUDE.md` del proyecto | Tono por defecto del flujo |
| Palabras clave y H1 | `docs/03-contenidos/seo-metadatos.md` | Escribir igual y ajustar cuando lleguen |
| Textos del cliente | `docs/03-contenidos/material/` | Escenario 🔴 |
| Conclusiones de investigación | `docs/01-investigacion/informe.md` | — |

## 2. Qué escenario es

| Escenario | Qué se hace |
| --- | --- |
| 🟢 Copy cerrado | **Literal.** Solo se toca lo que rompe una regla de la web, anotado con el porqué. Lo que no traía se escribe con su terminología |
| 🟡 Textos sueltos o web antigua | Reordenar según la estructura, adaptar tono y largo; de la web antigua, hechos que el cliente confirma, no frases |
| 🔴 No hay textos | Escribirlos con brief, cuestionario, investigación y SEO, marcados como *propuesto* |

## 3. Pasos

1. **Copy en `docs/03-contenidos/web-copy-{idioma}.md`** (en multipágina, uno
   por página) con el formato del flujo y el **origen** de cada texto:
   cliente, adaptado o propuesto.
2. **Reglas del texto**: un solo tratamiento en toda la web, terminología fija,
   sin claims sin prueba, H1 = el de SEO, palabras clave sin forzar la frase.
3. **Antetítulo** = etiqueta del menú, sin repetir el titular.
4. **Datos que faltan**: `[[ qué falta ]]` si faltan unos pocos; si faltan todos
   los de un bloque, se quita el bloque; si la sección se queda sin argumento,
   sale. Todo se anota en `CHECKLIST.md`.
5. **No olvidar**: textos del formulario (errores incluidos), gracias, 404 y
   `alt` (en fotos de stock, sin atribuirlas al cliente).
6. Belén revisa y lo envía al cliente; los cambios aprobados se aplican con fecha.
7. **Al construir**, el copy aprobado pasa a `src/data/` (apartado «Contenido
   como datos»): ningún texto en el marcado, un fichero por idioma, `placeholder`
   marcado y nunca publicado. Desde ese momento la versión viva está en los
   datos, y si cambia, se cambian los dos sitios.

## 4. Al terminar

- Recorre la checklist del flujo que toque (**03 · Contenidos y SEO** o
  **07 · Construcción**).
- Las imágenes que faltan para cada hueco → `xana:busqueda-imagenes`.
- Si es código, propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
