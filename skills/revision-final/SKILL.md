---
name: revision-final
description: Revisar una web de Xana terminada antes de publicarla, en la URL temporal y sobre el build — SEO, legales y cookies, contenido, funcionamiento, móvil, rendimiento y accesibilidad — y entregar a Belén un informe con lo que bloquea la publicación, lo que conviene arreglar y lo pendiente del cliente. Usar cuando la construcción está completa o tras un cambio grande en una web publicada.
---

# Revisión final

Todo el detalle está en el flujo [`flujos/revision.md`](../../flujos/revision.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar**: la checklist completa y el formato del informe están ahí.

**Aquí no se construye nada: se comprueba y se informa.** No se arregla nada sin
permiso si cambia diseño o contenido.

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| URL temporal desplegada con el último commit | Pedir a Belén el despliegue; sin eso solo se revisa el build local, y se dice |
| Idiomas y páginas | Cada punto se revisa **por idioma** y, en multipágina, por página indexable |
| Si es una revisión pequeña tras un cambio | Solo los apartados que toca el cambio |

## 2. Pasos

1. **`npm run build` + `npm run preview`**. Peso y velocidad nunca se juzgan en `dev`.
2. **Recorrer la checklist del flujo §5**, apartado por apartado:
   - **SEO**: metadatos, OG, H1, canonical, `noindex`, sitemap, robots, JSON-LD, 404, redirecciones.
   - **Legales y cookies**: los 4 elementos del pie, ningún `[[ ]]`, proveedores
     reales, cookies **comprobadas en el navegador** en ventana privada
     (antes de decidir, al rechazar y al aceptar), sin embeds sin consentimiento.
   - **Contenido**: nada provisional, stock no atribuido al cliente, datos
     confirmados, ortografía, **ninguna página de prueba publicada**.
   - **Funcionamiento**: formulario en la URL temporal, enlaces, menú, anclas,
     vídeos, consola sin errores, sin JavaScript.
   - **Móvil**: 375, 768 y escritorio.
   - **Rendimiento**: peso de `dist/`, sin PNG generados, primera imagen
     `eager`, fuentes alojadas, vídeos en R2.
   - **Accesibilidad**: contraste AA, teclado, skip-link, `alt`, formularios,
     reducir movimiento.
3. Para geometría, `getBoundingClientRect` o `getComputedStyle`; los **colores**
   no se leen con `getComputedStyle` en el preview.
4. **Informe** con el formato del flujo §4 (la guía no fija carpeta para
   guardarlo: se entrega a Belén y, si quiere conservarlo, se le pregunta dónde):
   - **Bloquea la publicación**: datos legales incompletos, cookies antes de
     aceptar, formulario que no envía, `noindex` donde no toca, provisionales,
     enlaces rotos, errores que rompen algo.
   - **Conviene arreglar**, **Anotado** y **Pendiente del cliente**.
   - Cada punto con qué, dónde y propuesta.

## 3. Al terminar

- Entrega el informe a Belén y **espera a que decida qué se arregla**. Los
  fallos técnicos evidentes se proponen en bloque.
- Tras los arreglos, repite solo los puntos afectados.
- Lo que no se arregle antes de publicar, a `CHECKLIST.md`.
- Nunca digas que algo funciona sin haberlo comprobado; lo que no se pudo
  comprobar, dilo.
- Con la revisión cerrada y el visto bueno del cliente, Belén llama a `xana:publicar`.
