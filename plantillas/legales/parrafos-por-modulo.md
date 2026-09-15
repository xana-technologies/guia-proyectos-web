# Legales · Párrafos por módulo

Lo que se añade o cambia en los textos base cuando la web lleva un módulo. Cada
bloque dice **dónde va**, trae el texto **ES y EN** listo y qué comprobar. Las
marcas `<!-- MÓDULO … -->` de las plantillas señalan el punto de inserción.

Regla de todos: **el párrafo entra cuando la web hace eso, no antes.** Un
párrafo de newsletter sin newsletter es el error de magnaceramica al revés.

| Módulo | Aviso legal | Privacidad | Cookies | Muro |
| --- | --- | --- | --- | --- |
| [Contenido incrustado](#contenido-incrustado-vimeo-youtube-google-maps) | — | Tratamiento + destinatario + transferencia | Categoría «Terceros» + filas | Categoría nueva |
| [Idiomas](#idiomas) | Párrafo opcional | — | — | Textos por idioma |
| [Confirmación al usuario](#confirmación-al-usuario-del-formulario) | — | Cambia el tratamiento del formulario | — | — |
| [Newsletter](#newsletter) | — | Tratamiento nuevo | Solo si pone cookies | — |
| [Actividad regulada](#actividad-regulada) | Párrafo en §4 + página propia | — | — | — |

Opcionales que no son módulo: [imágenes de banco](#imágenes-de-banco),
[relación comercial](#relación-comercial-fuera-de-la-web),
[aviso de borrador](#aviso-de-borrador) y
[casilla del formulario](#casilla-de-consentimiento-del-formulario).

---

## Contenido incrustado (Vimeo, YouTube, Google Maps)

**Cuándo.** La web incrusta un reproductor o un mapa de un tercero
([módulo multimedia](../../modulos/multimedia.md)). Los vídeos propios en R2 no
cuentan: no son de terceros.

**Montaje que presupone el texto.** El iframe **no se carga hasta aceptar** la
categoría «Terceros»; antes se enseña una fachada (imagen + enlace al
proveedor). festival-viento carga Vimeo y My Maps sin consentimiento: qué no
hacer.

**Variables.** `{{EMBED_PROVIDER}}`: nombre del servicio (Vimeo, YouTube,
Google Maps). `{{EMBED_PROVIDER_LEGAL_NAME}}`: entidad que figura en su política
de privacidad (**REVISAR** en la política del proveedor en el momento; YouTube y
Maps en el EEE: Google Ireland Limited). `{{EMBED_DESCRIPTION}}`: qué se
incrusta.

### Privacidad · §2, tratamiento nuevo (después de «Medición del uso»)

**ES**

> ### Contenido de terceros incrustado (solo si lo acepta)
>
> - **Datos:** dirección IP, datos del navegador y del dispositivo, e
>   identificadores de las cookies del proveedor, que el propio proveedor recoge
>   al cargar el contenido.
> - **Finalidad:** mostrar {{EMBED_DESCRIPTION}} dentro del Sitio web.
> - **Base jurídica:** su consentimiento, otorgado en el panel de cookies
>   (categoría «Terceros»). Hasta que lo acepta, el contenido no se carga y en
>   su lugar se muestra un enlace al proveedor.
> - **Destinatarios:** {{EMBED_PROVIDER_LEGAL_NAME}}, que trata estos datos
>   conforme a su propia política de privacidad.
> - **Conservación:** la que establece el proveedor en su política de
>   privacidad.

**EN**

> ### Embedded third-party content (only if you accept it)
>
> - **Data:** IP address, browser and device data, and the provider's cookie
>   identifiers, collected by the provider itself when the content loads.
> - **Purpose:** displaying {{EMBED_DESCRIPTION}} on the Website.
> - **Legal basis:** your consent, given through the cookie panel
>   ("Third-party" category). Until you accept, the content is not loaded and a
>   link to the provider is shown instead.
> - **Recipients:** {{EMBED_PROVIDER_LEGAL_NAME}}, which processes this data in
>   accordance with its own privacy policy.
> - **Retention:** as set out by the provider in its privacy policy.

<!-- REVISAR: no se califica al proveedor como encargado ni como responsable;
     es un punto de revisión jurídica si el proyecto la tiene. -->

**§3 Destinatarios**, línea nueva — ES: «Contenido incrustado, solo con su
consentimiento: {{EMBED_PROVIDER_LEGAL_NAME}}.» · EN: «Embedded content, only
with your consent: {{EMBED_PROVIDER_LEGAL_NAME}}.»

**§4 Transferencias**, línea nueva — ES: «{{EMBED_PROVIDER}} puede implicar
transferencias de datos fuera del Espacio Económico Europeo, en los términos que
indica su política de privacidad.» · EN: «{{EMBED_PROVIDER}} may involve
transfers of data outside the European Economic Area, as described in its
privacy policy.»

### Cookies

**§2**, categoría «Terceros»: cambiar el final por ES «Se utilizan en esta web
solo con su consentimiento.» · EN «Used on this website only with your
consent.»

**§3**, sustituir la frase «no incrusta contenido de terceros que las instale»:

- **ES:** «Si acepta el contenido de terceros, {{EMBED_PROVIDER}} instala sus
  propias cookies al cargar {{EMBED_DESCRIPTION}}.»
- **EN:** «If you accept third-party content, {{EMBED_PROVIDER}} sets its own
  cookies when {{EMBED_DESCRIPTION}} loads.»

**Filas de la tabla:** una por cookie **detectada en el navegador** tras
aceptar la categoría (DevTools → Aplicación → Cookies, dominio del proveedor).
Nombre, finalidad y duración, de la política de cookies del proveedor. No se
copian listas de otra web ni de memoria; lo que no se ha podido comprobar va
`[[ ]]`.

### Muro (`CookieConsent.astro` y textos en `src/data/legal/*.ts`)

Categoría nueva (`thirdParty`, no `readOnly`), con `autoClear` si el proveedor
pone cookies en el dominio propio.

- **ES** · título «Contenido de terceros» · descripción «Vídeos, mapas y otros
  contenidos de servicios externos integrados en la web. Al activarlos, el
  proveedor puede instalar sus propias cookies.»
- **EN** · title "Third-party content" · description "Videos, maps and other
  content from external services embedded in the website. When enabled, the
  provider may set its own cookies."

La descripción del aviso inicial (`banner.description`) también tiene que
nombrarlo.

---

## Idiomas

**Cuándo.** La web está en más de un idioma ([módulo idiomas](../../modulos/idiomas.md)).

**Qué cambia sin párrafo nuevo:**

- Los tres documentos y el muro, **completos en cada idioma**, frase a frase
  (clays `es.ts` es traducción de `en.ts`).
- Cada enlace entre documentos apunta a la versión del **mismo idioma**.
- `entity.ts` no se traduce; sí el país, el registro y la actividad
  (`entityText` de cada idioma).
- La fecha se escribe en cada idioma.

**Párrafo opcional** en aviso legal §6. Solo si el cliente o su asesoría lo
piden; no está en ningún proyecto actual.

- **ES:** «Este Sitio web se ofrece en varios idiomas. En caso de discrepancia
  entre versiones, prevalece la versión en {{PREVAILING_LANGUAGE}}.»
- **EN:** «This Website is available in several languages. In the event of any
  discrepancy between versions, the {{PREVAILING_LANGUAGE}} version prevails.»

---

## Confirmación al usuario del formulario

**Cuándo.** `formConfig.sendConfirmation: true`. Exige el dominio del cliente
verificado en Brevo; sin eso cae en spam y no se activa (clays
`src/form.config.ts`, [módulo formulario avanzado](../../modulos/formulario-avanzado.md)).

**Privacidad §2, «Consultas enviadas por el formulario de contacto».**

- **Finalidad**, sustituir por — ES: «gestionar y responder las consultas que
  nos envía a través del formulario de contacto y enviarle un email automático
  que confirma que la hemos recibido.» · EN: «managing and responding to the
  enquiries you send us through the contact form, and sending you an automatic
  email confirming that we have received it.»
- **Párrafo** tras la lista — ES: «El email de confirmación se envía a la
  dirección que nos facilita, a través de Brevo, y no incluye comunicaciones
  comerciales.» · EN: «The confirmation email is sent to the address you
  provide, through Brevo, and does not include commercial communications.»

---

## Newsletter

**Cuándo.** La web recoge suscripciones a un boletín. No está en la base: llega
con su propia alta de proveedor y su casilla.

**Montaje que presupone el texto.** Casilla **separada** de la de privacidad y
**sin marcar**; quien solo envía una consulta no entra en la lista (APC
`politica-de-privacidad.astro`). Enlace de baja en cada envío.

**Variables.** `{{NEWSLETTER_PROVIDER}}` (entidad del proveedor, p. ej. Brevo
(Sendinblue SAS, Francia) si es el mismo) y `{{NEWSLETTER_DESCRIPTION}}`
(«nuestra newsletter con novedades de …»).

### Privacidad · §2, tratamiento nuevo

**ES**

> ### Newsletter (solo si se suscribe)
>
> - **Datos:** dirección de email y, si lo facilita, su nombre.
> - **Finalidad:** enviarle {{NEWSLETTER_DESCRIPTION}}.
> - **Base jurídica:** su consentimiento expreso, otorgado al marcar la casilla
>   de suscripción, que puede retirar en cualquier momento con el enlace de baja
>   de cada envío o escribiendo a {{PRIVACY_EMAIL}}.
> - **Destinatarios:** {{NEWSLETTER_PROVIDER}}, que gestiona los envíos como
>   encargado del tratamiento.
> - **Conservación:** hasta que se dé de baja y, después, durante los plazos
>   que exige la ley para atender posibles responsabilidades.

**EN**

> ### Newsletter (only if you subscribe)
>
> - **Data:** email address and, if you provide it, your name.
> - **Purpose:** sending you {{NEWSLETTER_DESCRIPTION}}.
> - **Legal basis:** your express consent, given by ticking the subscription
>   box, which you can withdraw at any time using the unsubscribe link in every
>   email or by writing to {{PRIVACY_EMAIL}}.
> - **Recipients:** {{NEWSLETTER_PROVIDER}}, which manages the mailings as a
>   data processor.
> - **Retention:** until you unsubscribe and, thereafter, for the periods
>   required by law in order to address any liabilities that may arise.

<!-- REVISAR: si la plataforma mide aperturas y clics, se añade a «Datos» y a
     la finalidad. Doble confirmación (double opt-in): sin decidir. -->

**§3 Destinatarios**, línea nueva — ES: «Envío de la newsletter:
{{NEWSLETTER_PROVIDER}}.» · EN: «Newsletter delivery: {{NEWSLETTER_PROVIDER}}.»

**Casilla de suscripción**

- **ES:** «Quiero recibir {{NEWSLETTER_DESCRIPTION}} por email. Puedo darme de
  baja en cualquier momento.»
- **EN:** «I would like to receive {{NEWSLETTER_DESCRIPTION}} by email. I can
  unsubscribe at any time.»

**Cookies.** Solo cambian si el formulario de suscripción es un embed del
proveedor que pone cookies: entonces aplica
[contenido incrustado](#contenido-incrustado-vimeo-youtube-google-maps).

---

## Actividad regulada

**Cuándo.** La actividad del cliente está supervisada (información financiera,
salud, abogacía…) y la web publica contenidos que se pueden leer como consejo.
Modelo: APC Gestión Patrimonial (`src/pages/informacion-financiera.astro`).
Es el caso típico en que el proyecto **requiere revisión jurídica**: se propone
a Belén.

**Capa 2.** `{{BUSINESS_ACTIVITY}}` lleva el registro, la autorización o la
entidad supervisora; se pide al cliente, no se busca.

**Aviso legal §4**, párrafo nuevo (ejemplo financiero de APC; se adapta la
materia):

- **ES:** «Los contenidos publicados en el Sitio web tienen una finalidad
  informativa y educativa. No constituyen asesoramiento financiero
  personalizado ni una recomendación adaptada a las circunstancias, objetivos o
  perfil de una persona concreta. Más información en
  [Información financiera]({{REGULATED_INFO_URL}}).»
- **EN:** «The contents published on the Website are for information and
  educational purposes only. They do not constitute personalised financial
  advice or a recommendation tailored to the circumstances, objectives or
  profile of any particular person. More information in
  [Financial information]({{REGULATED_INFO_URL}}).»

**Página propia** (`/informacion-financiera/`, `noindex, follow`, en el pie
junto a los legales): estructura de APC — naturaleza de los contenidos;
contenido educativo, información comercial y asesoramiento personalizado;
ausencia de recomendación y de promesas; decisiones informadas; alcance del
aviso. Se redacta por proyecto con el cliente, no hay texto base.

---

## Imágenes de banco

**Cuándo.** Las fotos de la web son de stock y podrían leerse como instalaciones
o equipo propios (clays `legal-copy-en.md` §3). Va en aviso legal §3.

- **ES:** «Algunas fotografías del Sitio web son imágenes de banco con licencia,
  utilizadas con fines ilustrativos, y no muestran instalaciones ni personas de
  {{COMPANY_TRADE_NAME}}.»
- **EN:** «Some photographs on the Website are licensed stock images used for
  illustrative purposes and do not depict the facilities or people of
  {{COMPANY_TRADE_NAME}}.»

---

## Relación comercial fuera de la web

**Cuándo.** Solo si el cliente quiere que la política cubra también lo que hace
con clientes y proveedores fuera de la web. Clays lo incluyó; la base no, porque
no es algo que la web haga. Va en privacidad §2 como tratamiento.

- **ES:** «**Gestionar la relación comercial.** Tramitar presupuestos, pedidos,
  entregas y documentación con clientes y proveedores. Base jurídica: la
  ejecución de un contrato y el cumplimiento de obligaciones legales.»
- **EN:** «**Managing the commercial relationship.** Handling quotations,
  orders, deliveries and documentation with customers and suppliers. Legal
  basis: performance of a contract and compliance with legal obligations.»

---

## Aviso de borrador

**Cuándo.** Mientras los textos no se den por buenos. Lo decide Belén: se pone
cuando el proyecto va a revisión jurídica y hasta que el jurista lo valida.
En código es `draftNotice` de cada documento; se quita del dato, no del
componente (clays `LegalDocument.astro`). Clays lo retiró sin revisión
jurídica por decisión de Xana (14-09-2026).

- **ES:** «Borrador pendiente de validación jurídica. Este texto es una base de
  trabajo. Antes de su publicación debe revisarlo y validarlo un profesional a
  partir de la identidad y la operativa reales de {{COMPANY_TRADE_NAME}}. Los
  datos marcados están pendientes de confirmar.»
- **EN:** «Working draft, pending legal review. This text is a starting point.
  Before publication it must be reviewed and approved by a qualified
  professional, based on the actual identity and operations of
  {{COMPANY_TRADE_NAME}}. Marked gaps are data still to be confirmed.»

---

## Casilla de consentimiento del formulario

No es de las páginas legales: vive en los textos del formulario
([flujo de formulario](../../flujos/formulario.md)). Está aquí para que diga lo
mismo que privacidad §2. La comprueba también el servidor (`consent !== 'on'`
en clays `api/contact.ts`).

- **ES:** «He leído la [política de privacidad]({{PRIVACY_POLICY_URL}}) y
  acepto que mis datos sean utilizados para responder a esta consulta.»
- **EN:** «I have read the [privacy policy]({{PRIVACY_POLICY_URL}}) and agree to
  my data being used to answer this enquiry.»

Error — ES: «Acepte la política de privacidad para enviar su consulta.»

## Fuentes

clays-of-the-world (`docs/03-contenidos/legal/`, `src/data/legal/es.ts`,
`src/data/es.ts` y `en.ts` → `consent`, `src/form.config.ts`) · APC
(`politica-de-privacidad.astro`, `informacion-financiera.astro`,
`LegalDoc.astro`) · festival-viento (embeds sin consentimiento) ·
xanatechnologies y magnaceramica (`sendConfirmation` en `api/contact.ts`).
