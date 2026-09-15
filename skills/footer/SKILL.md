---
name: footer
description: Montar el footer de una web de Xana con los 4 elementos legales obligatorios (aviso legal, política de privacidad, política de cookies y «Preferencias de privacidad» con data-cc), copyright, contacto, redes y navegación desde los datos, y la cortina de cierre si se usa. Usar al construir o cambiar el pie de la web.
---

# Footer

Todo el detalle está en el flujo [`flujos/maquetacion.md`](../../flujos/maquetacion.md)
§9 «Pie» (las rutas de esta skill son relativas a su carpeta). Los enlaces
legales vienen de [`flujos/legales.md`](../../flujos/legales.md) · Robots y pie,
y el de preferencias de [`flujos/cookies-analitica.md`](../../flujos/cookies-analitica.md) §4.
Implementación de referencia: `clays-of-the-world/src/components/Footer.astro`.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Enlaces legales | `src/data/legal/index.ts` (`legalLinks()`) | Montar los legales (`xana:montar-legales`) o dejar las rutas y anotarlo |
| Muro de cookies | `CookieConsent.astro` | El enlace de preferencias lleva igual a la política; anotar que falta el muro |
| Contacto, redes, nombre | Datos globales del sitio | `Pending`; perfiles sociales de prueba marcados `placeholder`, nunca publicados |
| ¿Pie oscuro? | Diseño, marca | Cuenta en el ritmo de superficies |

## 2. Pasos

1. **Los 4 elementos legales, siempre**, en la barra inferior:
   **Aviso legal · Política de privacidad · Política de cookies · Preferencias
   de privacidad**. Salen de los datos legales, no escritos en el pie.
2. **«Preferencias de privacidad»** es un enlace que con JavaScript abre el
   panel y sin él lleva a la explicación:

   ```astro
   <a href="/politica-de-cookies/#preferences" data-cc="show-preferencesModal">
     Preferencias de privacidad
   </a>
   ```
3. **Enlaces del mismo idioma** que la página.
4. **Copyright, contacto y navegación** desde los datos globales; el pie y la
   cabecera los reciben, no los repiten.
5. **Logo**: versión en blanco si el pie es oscuro.
6. **Ritmo de superficies**: un pie oscuro cuenta como sección oscura; la última
   sección antes del pie no puede ser oscura.
7. **Cortina de cierre** (el pie se destapa), solo si el diseño la pide: `main`
   con fondo opaco y `z-index`, pie `sticky` al fondo, y solo si el pie cabe
   entero en la ventana.
8. Iconos de redes SVG con `aria-hidden` y nombre accesible en el enlace.

## 3. Al terminar

- Pulsar los 4 enlaces legales; «Preferencias de privacidad» abre el panel, y
  sin JavaScript lleva a `/politica-de-cookies/#preferences`.
- A 360, 768, 1024 y 1440 px; con teclado.
- Checklist **Cabecera, pie y páginas de sistema** del flujo §15.
- Belén revisa en la URL temporal. Propón el commit y espera el sí
  ([forma de trabajo §5](../../forma-de-trabajo.md)).
