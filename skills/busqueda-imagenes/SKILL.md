---
name: busqueda-imagenes
description: Preparar la búsqueda de las imágenes que faltan en una web de Xana — una ficha por hueco sin imagen en docs/imagenes/busqueda-stock.md con dónde va, qué transmite, palabras clave para Magnific en inglés y español, formato y tamaño mínimo, estilo y qué evitar, o la descripción para generarla con IA si Belén lo indica. Usar al cerrar el inventario de imágenes o cuando falten fotos para una sección.
---

# Búsqueda de imágenes

Todo el detalle está en el flujo [`flujos/imagenes.md`](../../flujos/imagenes.md)
§2 y §3 paso 1 (las rutas de esta skill son relativas a su carpeta). Plantilla:
[`plantillas/proyecto/busqueda-stock.md`](../../plantillas/proyecto/busqueda-stock.md).

**Stock, solo Magnific, y descarga siempre Belén.** Claude prepara la búsqueda;
no descarga de bancos de imágenes. **IA solo cuando Belén lo indica.**

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Estructura con los huecos | `docs/02-estructura/estructura.md` | Parar: sin huecos no se sabe qué buscar |
| Copy de cada sección | `docs/03-contenidos/` | Buscar igual con el objetivo de la sección |
| Qué ha mandado el cliente | `docs/imagenes/originales/` e `inventario.md` | Mirarlas antes: **las del cliente van primero** |
| Estilo de fotografía de la marca | `brand/README.md` | Preguntar a Belén |
| ¿Stock o IA? | Belén | Por defecto, stock en Magnific |

## 2. Pasos

1. **Cerrar el inventario**: cada hueco de la estructura con su imagen del
   cliente, de stock o pendiente. Si una del cliente no llega a la resolución
   del hueco, **se piden los originales en alta**, no se amplía.
2. **Una ficha por imagen que falta** en `docs/imagenes/busqueda-stock.md`:
   - Dónde va: página, sección y hueco.
   - Qué transmite: la idea, no solo el objeto.
   - Palabras clave para Magnific **en inglés y en español**, con variantes
     (sinónimos, plano, luz).
   - Formato y proporción del hueco y **tamaño mínimo** del original, contando
     densidad ×2 (anchos del flujo §3 paso 5).
   - Estilo según la marca: luz, color, gente sí o no, encuadre.
   - Qué evitar: lo que choca con la marca, rótulos legibles de otra empresa,
     uniformes, instalaciones reconocibles.
   - Número de candidatas (normalmente 3).
3. **Si Belén indica IA**: en lugar de palabras clave, una descripción para
   generarla (escena, encuadre, luz, proporción, estilo, qué no debe aparecer).
   Se marcará `IA` en el inventario.
4. **No repetir** la misma imagen en dos huecos.

## 3. Al terminar

- Pasa las fichas a Belén para que busque y descargue las candidatas en
  `docs/imagenes/originales/`, anotando la referencia de Magnific de cada una.
- Checklist **Búsqueda y entrada (03)** del flujo §7.
- Cuando lleguen las candidatas, sigue `xana:preparar-imagenes`.
