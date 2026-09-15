# Flujo · Cookies y analítica

Muro de cookies y Google Tag Manager → Google Analytics 4 cargado **solo
después de aceptar**. **Va en todas las webs.** La implementación de
referencia es `clays-of-the-world/src/components/CookieConsent.astro`.

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Pedir acceso al Tag Manager y Analytics del cliente, o acordar que se crean |
| [07 · Construcción](../fases/07-construccion.md) | Montar el muro y la carga de GTM; enlace «Preferencias de privacidad» en el pie |
| [08 · Revisión](../fases/08-revision.md) | Ninguna cookie de analítica antes de aceptar; tabla de cookies = lo que carga la web |
| [09 · Publicación](../fases/09-publicacion.md) | GA4 dentro de GTM, informe tipo, datos a las 24 h |

La política de cookies y la de privacidad están en el [flujo de legales](legales.md).
Los dos flujos se tocan en un punto: **lo que carga el muro tiene que estar
declarado en la política, y al revés.**

## 2. Qué tenemos

| Situación | Qué se hace |
| --- | --- |
| El cliente ya tiene GTM/GA4 (web antigua) | Se reutiliza el contenedor; revisar qué etiquetas tiene antes de conectarlo |
| No tiene | Se crea contenedor y propiedad (Belén) |
| La web lleva vídeos o mapas de terceros | Categoría y cookies extra → módulo [multimedia](../modulos/multimedia.md) |
| El cliente quiere publicidad (Ads, píxeles) | Fuera de la base: nueva categoría en el muro, política y Consent Mode de publicidad. Consultar antes |

## 3. Cómo funciona

```
Primera visita ─► muro (vanilla-cookieconsent)
                   ├── Rechazar  → no se carga nada; solo cc_cookie
                   └── Aceptar «Analítica»
                         → gtag('consent','default', analítica concedida, publicidad denegada)
                         → se inyecta gtm.js → GTM dispara GA4 → _ga, _ga_<ID>
Retirar consentimiento ─► se borran _ga* y se recarga la página
```

- **GTM no va en el `<head>`** como dicen las instrucciones de Google: así
  dispararía GA4 y pondría cookies antes de que nadie aceptara.
- **Sin el `<noscript>` de GTM**: sin JavaScript no hay muro, así que el
  iframe cargaría el contenedor sin consentimiento. Sin JavaScript no hay
  muro, pero tampoco analítica.
- El ID del contenedor (`GTM-XXXXXXX`) **no es secreto**: va en la
  configuración del sitio, sin variable de entorno.

## 4. Pasos y quién

1. **Belén**: contenedor de GTM y propiedad de GA4 (del cliente o nuevos);
   pasa el ID del contenedor.
2. **Claude**: `npm i vanilla-cookieconsent` y copiar `CookieConsent.astro`
   de clays, adaptando:
   - textos desde los datos (idioma de la página);
   - `GTM_ID`;
   - variables `--cc-*` con los tokens del proyecto;
   - duración de `cc_cookie` igual a la que declara la política (182 días).
3. **Claude**: el componente va en el layout base, en todas las páginas
   públicas (no en styleguide ni pruebas).
4. **Claude**: en el pie, los 4 elementos legales. El cuarto:

   ```astro
   <a href="/politica-de-cookies/#preferences" data-cc="show-preferencesModal">
     Preferencias de privacidad
   </a>
   ```

   Con JavaScript abre el panel; sin él lleva a la explicación.
5. **Belén (en GTM)**: etiqueta de GA4 con activador en todas las páginas, y
   la conversión como vista de página de `/gracias`.
6. **Revisión y publicación**: §7.

## 5. El código que importa

```ts
function cargarAnalitica() {
  if (analiticaCargada) return;
  analiticaCargada = true;

  window.dataLayer = window.dataLayer || [];
  // `arguments` y NO un array: GTM y gtag.js solo reconocen los comandos
  // que llegan como objeto Arguments. Con un array la cola se llena pero
  // los comandos no se ejecutan (pasaba en Magna).
  window.gtag = function () {
    window.dataLayer.push(arguments);
  };

  window.gtag('consent', 'default', {
    analytics_storage: 'granted',
    ad_storage: 'denied',
    ad_user_data: 'denied',
    ad_personalization: 'denied',
  });

  window.dataLayer.push({ 'gtm.start': Date.now(), event: 'gtm.js' });
  const script = document.createElement('script');
  script.async = true;
  script.src = `https://www.googletagmanager.com/gtm.js?id=${GTM_ID}`;
  document.head.appendChild(script);
}

CookieConsent.run({
  cookie: { name: 'cc_cookie', expiresAfterDays: 182 },
  onConsent: alDecidir,   // alDecidir: si acceptedCategory('analytics') → cargarAnalitica()
  onChange: alDecidir,
  categories: {
    necessary: { enabled: true, readOnly: true },
    analytics: {
      autoClear: {
        cookies: [{ name: /^_ga/ }, { name: '_gid' }, { name: '_gat' }],
        reloadPage: true, // GTM ya cargado no se descarga: hay que recargar
      },
    },
  },
  // language: textos del muro desde los datos, ver §6
});
```

Los textos viajan al script como JSON dentro del HTML
(`<script type="application/json" id="cc-textos">`), no importados: así el
contenido legal no entra en el paquete de JavaScript.

## 6. Textos del muro

Categorías visibles: **Necesarias** (bloqueada, siempre activa) y
**Analítica**. Botones con el mismo peso visual para aceptar y rechazar.

| Clave | Español | Inglés |
| --- | --- | --- |
| Título | Usamos cookies | We use cookies |
| Descripción | Usamos cookies propias necesarias para que la web funcione y, si lo aceptas, cookies de Google Analytics para saber cómo se usa. Más información en la política de cookies. | We use necessary cookies to make this site work and, if you accept, Google Analytics cookies to understand how it is used. More information in our cookie policy. |
| Aceptar todo | Aceptar todas | Accept all |
| Rechazar | Rechazar | Reject all |
| Configurar | Configurar | Manage preferences |
| Panel · título | Preferencias de privacidad | Privacy preferences |
| Guardar | Guardar preferencias | Save preferences |
| Necesarias | Imprescindibles para que la web funcione y para recordar tu elección sobre cookies. No se pueden desactivar. | Required for the site to work and to remember your cookie choice. They cannot be switched off. |
| Analítica | Nos permiten medir las visitas y cómo se usa la web con Google Analytics. Solo se activan si las aceptas. | They let us measure visits and how the site is used with Google Analytics. Only enabled if you accept them. |

*Textos base: se ajustan al tono de cada marca sin cambiar lo que dicen.*

## 7. Tabla de cookies de la base

| Cookie | Tipo | Proveedor | Finalidad | Duración |
| --- | --- | --- | --- | --- |
| `cc_cookie` | Necesaria | Propia | Guardar la elección sobre cookies | 6 meses |
| `_ga` | Analítica | Google Analytics | Distinguir usuarios | 2 años |
| `_ga_<ID>` | Analítica | Google Analytics | Mantener el estado de la sesión | 2 años |

- **No se declaran `_gid`, `_gat` ni `PHPSESSID`** aunque salgan en el
  *Protocolo de publicación ESTÁNDAR*: `_gid` y `_gat` son de la versión
  antigua de Analytics y GA4 no los pone; `PHPSESSID` es de webs PHP. El muro
  borra `_gid` y `_gat` igualmente por si quedan de visitas antiguas.
- Si en GTM se añade otra etiqueta (Ads, LinkedIn, Hotjar…), **antes** hay que
  declararla en la política y, si es publicitaria, abrir categoría nueva.

## 8. Reglas y trampas

- `dataLayer.push(arguments)`, nunca un array.
- Ni snippet de GTM en `<head>` ni `<noscript>`.
- Lo que se dispara lo decide GTM, no el código: revisar el contenedor antes de publicar.
- Embeds de terceros (Vimeo, YouTube, Google Maps) ponen cookies: sin
  consentimiento no se cargan (fachada) → [multimedia](../modulos/multimedia.md).
  Pasó en festival-viento.
- La política no declara lo que la web no usa (Magna declaraba Google Maps y
  Mailchimp).
- El muro al abrirse mueve el foco a un contenedor con `tabindex="-1"`: quitarle
  el anillo de foco para que no se dibuje un borde en la caja (clays).
- En webs con scroll suave (Lenis), el panel necesita `data-lenis-prevent`
  para poder desplazarse (xanatechnologies).

## 9. Checklist

**Construcción y revisión**
- [ ] `CookieConsent.astro` en el layout de las páginas públicas
- [ ] Textos en el idioma de cada página; estilos con tokens
- [ ] En una ventana privada: al entrar, solo `cc_cookie` tras decidir; ninguna `_ga` antes de aceptar
- [ ] Al aceptar: carga `gtm.js` y aparecen `_ga` y `_ga_<ID>`
- [ ] Al retirar: se borran y la página recarga
- [ ] «Preferencias de privacidad» en el pie abre el panel
- [ ] Tabla de cookies de la política = cookies reales
- [ ] Sin embeds de terceros sin consentimiento

**Publicación**
- [ ] GA4 configurado dentro de GTM; nada más en el contenedor sin declarar
- [ ] Conversión: vista de página de `/gracias`
- [ ] Retención de datos de GA4 en 14 meses, como dice la política
- [ ] Informe tipo creado (Looker Studio)
- [ ] A las 24 h llegan datos a GA4
