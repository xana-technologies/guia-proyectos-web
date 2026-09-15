---
name: palabras-clave
description: Preparar la petición de SEO para una web de Xana (palabras clave, títulos, descripciones y H1 por página e idioma) con la estructura aprobada, y guardar la respuesta en docs/03-contenidos/seo-metadatos.md comprobando que no falta nada. Usar cuando haya que pedir el SEO o cuando llegan las palabras clave y metadatos.
---

# Palabras clave

Todo el detalle está en el flujo [`flujos/contenidos-seo.md`](../../flujos/contenidos-seo.md)
(pasos 1, 3 y 4 de «03 · Contenidos y SEO» y apartado «Metadatos») y en la
plantilla [`plantillas/equipo/peticion-seo.md`](../../plantillas/equipo/peticion-seo.md)
(las rutas de esta skill son relativas a su carpeta).

**Las palabras clave las decide SEO, no Claude.** Claude prepara la petición y
ordena lo que vuelve; si falta algo, pregunta.

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| Estructura **aprobada** (`docs/02-estructura/estructura.md`) | Parar: sin ella no se pueden asignar palabras clave por página |
| Brief e idiomas | `docs/00-arranque/brief.md` |
| Informe de investigación | Se adjunta si existe; si no, se dice |

## 2. Preparar la petición

1. Rellenar la plantilla de petición con el brief, la estructura aprobada, los
   idiomas y el informe de investigación.
2. En multipágina, incluir las palabras clave **propuestas** de la estructura,
   marcadas como propuesta.
3. Dejarla en el proyecto y pedir a Belén que la envíe. Claude no la manda.

## 3. Cuando vuelve la respuesta

1. Guardarla en `docs/03-contenidos/seo-metadatos.md`: una tabla por página e
   idioma con palabra clave principal y secundarias, título, descripción y H1,
   con la fecha.
2. Comprobar que está todo: cada página indexable de la estructura, en cada
   idioma. Lo que falte → **preguntar**, nunca inventarlo.
3. Revisar que casa con la web (dominio con o sin `www`, rutas que existen). Si
   no, ajustar y **anotar el aviso** en el documento.
4. Añadir la tabla de qué se indexa y qué no.
5. Largos orientativos: título ~60 caracteres, descripción ~155. Si algo se
   pasa mucho, avisar; no recortarlo por nuestra cuenta.

## 4. Al terminar

- Dile a Belén qué ha llegado y qué falta.
- Los H1 y palabras clave pasan al copy con `xana:escribir-textos`, y los
  títulos y descripciones al código con `xana:montar-seo-tecnico`.
