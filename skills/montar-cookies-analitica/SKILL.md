---
name: montar-cookies-analitica
description: Montar el muro de cookies (vanilla-cookieconsent) y la carga de Google Tag Manager y GA4 solo tras aceptar en una web de Xana, o revisar que no se ponen cookies sin consentimiento. Usar al añadir el muro, conectar la analítica, cambiar sus textos o comprobar qué cookies pone la web.
---

# Montar cookies y analítica

Todo el detalle está en el flujo [`flujos/cookies-analitica.md`](../../flujos/cookies-analitica.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.** La implementación de referencia es
`clays-of-the-world/src/components/CookieConsent.astro`.

La regla que manda: **nada de analítica antes de aceptar**, y **lo que carga el
muro está declarado en la política, y al revés** ([`xana:montar-legales`](../montar-legales/SKILL.md)).

## 1. Antes de tocar nada

| Qué | Dónde mirarlo | Si falta |
| --- | --- | --- |
| ID del contenedor de GTM | Belén | Montar el muro igual y dejar la carga de GTM sin ID, anotado en `CHECKLIST.md`. El ID no es secreto |
| GTM o GA4 heredados de la web antigua | Belén | Revisar qué etiquetas tiene el contenedor antes de conectarlo |
| Vídeos, mapas u otros embeds | El código | Categoría y fachadas → módulo [multimedia](../../modulos/multimedia.md) |
| Publicidad (Ads, píxeles) | Briefing | **Fuera de la base**: consultar a Belén antes de hacer nada (flujo §2) |
| Idiomas | `CLAUDE.md` del proyecto | Textos del muro en cada idioma (flujo §6) |
| Scroll suave (Lenis) | `package.json` | El panel necesita `data-lenis-prevent` |

Explica a Belén el plan y las dudas, y espera antes de empezar
([forma de trabajo §1](../../forma-de-trabajo.md)).

## 2. Pasos

1. **Instalar y copiar** → flujo §4 paso 2: `vanilla-cookieconsent` y
   `CookieConsent.astro` de clays, adaptando textos, `GTM_ID`, variables
   `--cc-*` con los tokens del proyecto y la duración de `cc_cookie`.
2. **Revisar el código clave** contra el flujo §5 antes de seguir:
   - `dataLayer.push(arguments)`, nunca un array.
   - Ni snippet de GTM en `<head>` ni `<noscript>`.
   - `autoClear` de `_ga*`, `_gid`, `_gat` con `reloadPage: true`.
   - Textos como JSON en el HTML, no importados en el script.
3. **Textos del muro** desde los datos (flujo §6), ajustados al tono de la
   marca sin cambiar lo que dicen. Aceptar y rechazar con el mismo peso visual.
4. **En el layout base** de las páginas públicas; no en styleguide ni pruebas.
5. **Enlace «Preferencias de privacidad»** en el pie (flujo §4 paso 4). Si el
   footer aún no existe, anotarlo para `xana:footer`.
6. **Pasar a Belén** lo que se hace en GTM: etiqueta de GA4 en todas las
   páginas y conversión como vista de página de `/gracias`.
7. **Comprobar en el navegador**, en una ventana privada:
   - Antes de decidir y al rechazar: solo `cc_cookie`, ninguna `_ga`.
   - Al aceptar: carga `gtm.js` y aparecen `_ga` y `_ga_<ID>`.
   - Al retirar: se borran y la página recarga.
   - «Preferencias de privacidad» abre el panel.
8. **Tabla de cookies de la política** = cookies reales (flujo §7).

## 3. Al terminar

- Recorre la checklist **Construcción y revisión** del flujo §9 y di qué está
  comprobado y qué no. Si GTM aún no tiene ID o etiquetas, las cookies de
  analítica no se pueden comprobar: dilo.
- Si algo falla, busca el síntoma en
  [trampas · analítica y cookies](../../trampas-y-lecciones.md#analítica-y-cookies).
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
- Lo de la publicación (GA4 en GTM, retención de 14 meses, informe, datos a
  las 24 h) va con la checklist **Publicación** del flujo §9.
