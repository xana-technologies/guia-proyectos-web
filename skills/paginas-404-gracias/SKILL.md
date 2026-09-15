---
name: paginas-404-gracias
description: Montar la página 404 y la página de gracias del formulario de una web de Xana — 404 con cabecera, pie, noindex y salidas a portada y contacto (en el idioma de la ruta si hay varios); gracias en /gracias/ con noindex, nofollow, fuera del sitemap y una sola salida, donde se mide la conversión. Usar al preparar las páginas de error o de confirmación de envío.
---

# Páginas 404 y gracias

Todo el detalle está en el flujo [`flujos/maquetacion.md`](../../flujos/maquetacion.md)
§10 (las rutas de esta skill son relativas a su carpeta). Qué se indexa:
[`flujos/seo-tecnico.md`](../../flujos/seo-tecnico.md) §3 paso 6. Por qué gracias
importa: [`flujos/formulario.md`](../../flujos/formulario.md) §3.
Implementación de referencia: `clays-of-the-world/src/pages/404.astro`,
`src/pages/[...lang]/thank-you.astro` y `components/StatusMessage.astro`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Textos de las dos páginas | `src/data/` | Escribirlos con `xana:escribir-textos`; nada en el marcado |
| Idiomas | `CLAUDE.md` | Un idioma: rutas sin `[...lang]` |
| Cabecera y pie | `src/components/` | Montarlos antes, o anotar que faltan |

## 2. 404

1. `src/pages/404.astro` **con cabecera y pie**: quien cae tiene que poder volver.
2. `noindex`.
3. Salidas a la portada y a contacto.
4. Con varios idiomas, `export const prerender = false` para que salga en el
   idioma del prefijo ([trampas](../../trampas-y-lecciones.md#la-404-sale-siempre-en-el-idioma-por-defecto)).
5. Un «404» decorativo a cuerpo grande sin contraste suficiente solo con
   `aria-hidden` y el mensaje dicho en texto.

## 3. Gracias

1. Ruta **`/gracias/`** en webs en español (`/thank-you/` en inglés).
2. `noindex, nofollow` y **fuera del sitemap**.
3. **Una sola salida**: la portada.
4. Es a donde llega **todo envío correcto** del formulario, con y sin JavaScript,
   y donde GTM mide la conversión: la ruta tiene que coincidir con la del
   endpoint (`api/contact.ts`) y con la de GTM.

Las dos pueden compartir composición (un componente de mensaje de estado).

## 4. Al terminar

- Abrir una ruta que no existe y ver la 404 (en cada idioma, si hay varios).
- Enviar el formulario de prueba y llegar a gracias.
- Ver en el código generado el `robots` de las dos y que no están en el sitemap.
- Checklist **Cabecera, pie y páginas de sistema** del flujo §15.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
