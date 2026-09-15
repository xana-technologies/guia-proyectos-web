# Plantilla · Política de privacidad (ES)

> Capa 1 con la **capa 3 por defecto de la base ya redactada**: formulario de
> contacto (Vercel + Brevo + Cloudflare Turnstile) y analítica (Google Tag
> Manager → GA4 con consentimiento). Lo marcado `CAPA 3 · DEDUCIDO` se revisa
> contra lo que la web hace de verdad antes de publicar. Leyenda en
> [`README.md`](README.md). Ruta: `/politica-de-privacidad/`.
>
> Estructura del *Protocolo de publicación ESTÁNDAR* (responsable, tratamientos,
> destinatarios, transferencias, conservación, derechos, decisiones
> automatizadas, cookies, cambios). Redacción de clays-of-the-world
> (`src/data/legal/es.ts`) y APC (`src/pages/politica-de-privacidad.astro`).

---

# Política de privacidad

Última actualización: {{PRIVACY_EFFECTIVE_DATE}}

{{COMPANY_TRADE_NAME}} trata los datos personales de quienes visitan el Sitio
web {{WEBSITE_URL}} o se ponen en contacto a través de él con transparencia y
con todas las garantías, de acuerdo con {{DATA_PROTECTION_LAW}}. Esta política
explica quién es el responsable, qué datos se tratan, con qué finalidades y
sobre qué bases jurídicas, a quién se comunican, cuánto tiempo se conservan y
cómo ejercer sus derechos.

## 1. Responsable del tratamiento

<!-- Bloque `identity` (el mismo dato que el aviso legal). Si el correo de
     privacidad o el DPO existen, se añaden como campos en `entity.ts`. -->

| | |
| --- | --- |
| Responsable | {{COMPANY_LEGAL_NAME}} |
| NIF | {{COMPANY_TAX_ID}} |
| Domicilio social | {{COMPANY_REGISTERED_ADDRESS}}, {{COMPANY_COUNTRY}} |
| Email de privacidad | {{PRIVACY_EMAIL}} |
| Delegado de protección de datos | {{DPO_CONTACT}} <!-- SI: el cliente tiene DPO. Si no, fila fuera (no [[ ]]) --> |

## 2. Datos que tratamos, finalidades y bases jurídicas

Tratamos datos personales únicamente cuando existe una base jurídica válida.
Para cada tratamiento se indica qué datos se tratan, con qué finalidad, sobre
qué base jurídica, quién los recibe y durante cuánto tiempo se conservan.

<!-- ================================================================
     {{PROCESSING_ACTIVITIES_TABLE}}
     CAPA 3 · DEDUCIDO · por defecto de la base (formulario + analítica).
     Revisar:
     - Campos del formulario contra `src/components/sections/Contact.astro`
       y `src/pages/api/contact.ts` (la base: nombre, email, teléfono
       opcional, mensaje; casilla de privacidad obligatoria).
     - Que la analítica es GA4 por GTM y nada más dentro del contenedor.
     - Módulos (confirmación al usuario, newsletter, embeds) →
       parrafos-por-modulo.md.
     En código: cada tratamiento es un `paragraph` (título en negrita) +
     `list`, o un `terms`. No hay bloque de subtítulo.
     ================================================================ -->

### Consultas enviadas por el formulario de contacto

- **Datos:** nombre, dirección de email, teléfono (opcional) y el contenido de
  su mensaje.
- **Finalidad:** gestionar y responder las consultas que nos envía a través del
  formulario de contacto.
- **Base jurídica:** su consentimiento, que otorga al marcar la casilla del
  formulario, y la aplicación de medidas precontractuales a petición suya
  cuando la consulta tiene ese fin.
- **Destinatarios:** Vercel, que aloja el Sitio web y ejecuta el envío del
  formulario, y Brevo, que nos hace llegar el mensaje por email. Ambos actúan
  como encargados del tratamiento.
- **Conservación:** los mensajes no se almacenan en el Sitio web: llegan a
  nuestro buzón de correo y se conservan allí durante el tiempo necesario para
  atender su consulta y, después, durante los plazos que exige la ley para
  atender posibles responsabilidades.

Le rogamos que nos envíe solo los datos necesarios para atender su consulta. No
incluya categorías especiales de datos (salud, ideología, etc.) ni información
confidencial que no sea imprescindible para responderle.

### Seguridad del formulario y del Sitio web

<!-- CAPA 3 · DEDUCIDO. Clays solo declara Turnstile. Aquí se añade lo que el
     código hace también: el endpoint manda la IP a Cloudflare (`remoteip` en
     `api/contact.ts`) y el firewall de Vercel limita envíos por IP.
     REVISAR: no hay plazo de conservación verificado de los proveedores; no
     poner cifras sin comprobarlas. -->

- **Datos:** dirección IP y señales técnicas del navegador, como el tipo de
  navegador, la resolución de pantalla o los tiempos de interacción.
- **Finalidad:** comprobar que los envíos del formulario proceden de una persona
  y no de un bot automatizado, limitar los envíos abusivos y mantener la
  seguridad y el funcionamiento del Sitio web.
- **Base jurídica:** nuestro interés legítimo en mantener la seguridad del Sitio
  web y protegerlo frente a abusos.
- **Destinatarios:** Cloudflare, que presta la verificación de seguridad
  (Cloudflare Turnstile), y Vercel, como proveedor de alojamiento. Ambos actúan
  como encargados del tratamiento.
- **Conservación:** estos datos no se almacenan en el Sitio web; los proveedores
  los tratan durante el tiempo necesario para prestar el servicio de seguridad.

La verificación de Cloudflare Turnstile no utiliza cookies ni sirve para
hacerle seguimiento.

### Medición del uso del Sitio web (solo si acepta las cookies de analítica)

- **Datos:** páginas visitadas, tiempo de permanencia, ubicación aproximada
  (país o ciudad), tipo de dispositivo y de navegador, e identificadores de las
  cookies de analítica. Se utilizan de forma agregada y no le identifican
  directamente.
- **Finalidad:** elaborar estadísticas agregadas de visitas para mejorar el
  Sitio web.
- **Base jurídica:** su consentimiento, otorgado a través del panel de cookies,
  que puede retirar en cualquier momento desde «Preferencias de privacidad», al
  pie de todas las páginas. Retirarlo no afecta a la licitud del tratamiento
  anterior.
- **Destinatarios:** Google Ireland Limited, que presta el servicio de Google
  Analytics como encargado del tratamiento.
- **Conservación:** los datos de analítica se conservan un máximo de
  {{ANALYTICS_RETENTION}}.

<!-- /CAPA 3 · PROCESSING_ACTIVITIES_TABLE -->

## 3. Destinatarios

Los datos personales no se venden ni se ceden a terceros. Pueden ser tratados
por proveedores necesarios para prestar nuestros servicios, que actúan como
encargados del tratamiento por cuenta nuestra con los contratos que exige la
normativa de protección de datos, y comunicados a autoridades u otros terceros
cuando exista una obligación legal o una base jurídica válida.

Categorías principales:

<!-- {{RECIPIENT_CATEGORIES}} · CAPA 3 · DEDUCIDO del stack de la base.
     El RGPD (art. 13.1.e) permite «destinatarios o categorías»: los
     proveedores que están en el código van con nombre (se sabe de cierto);
     los del cliente, por categoría (no se le pregunta). -->

- Alojamiento del Sitio web y ejecución del formulario: Vercel Inc.
- Envío por email de los mensajes del formulario: Brevo (Sendinblue SAS,
  Francia).
- Verificación de seguridad del formulario: Cloudflare, Inc.
- Analítica web, solo con su consentimiento: Google Ireland Limited.
- Correo electrónico y herramientas de gestión que utilizamos para atender su
  consulta.
<!-- REVISAR: ¿se declara también la empresa que desarrolla y mantiene la web
     (Xana), que tiene acceso a la cuenta de Vercel y a los registros de Brevo?
     Pendiente de decidir; no incluir sin confirmación. -->

<!-- /CAPA 3 · RECIPIENT_CATEGORIES -->

## 4. Transferencias internacionales

Cuando los datos se transfieran fuera del Espacio Económico Europeo,
utilizaremos los mecanismos y garantías exigidos por la normativa aplicable,
como decisiones de adecuación o cláusulas contractuales tipo.

<!-- {{INTERNATIONAL_TRANSFER_DETAILS}} · CAPA 3 · DEDUCIDO.
     - Google: redacción de clays (certificación en el Marco de Privacidad de
       Datos UE-EE. UU.).
     - Cloudflare: sin afirmar adhesión al Marco: no está comprobado (clays).
     - Vercel: decisión 15-09-2026, las funciones van SIEMPRE en región
       europea (`"regions": ["fra1"]` en `vercel.json`). Comprobar en Vercel →
       Settings → Functions antes de publicar. Si un proyecto no la tiene,
       este párrafo no es cierto.
     - Brevo no aparece: es una sociedad francesa (clays). -->

Google Analytics puede implicar transferencias a Google LLC, en Estados Unidos,
que está certificada en el Marco de Privacidad de Datos UE-EE. UU.

Cloudflare, Inc. tiene su sede en Estados Unidos. Cuando la verificación de
seguridad implique una transferencia de datos fuera del Espacio Económico
Europeo, se aplican las garantías que exige la normativa de protección de datos.

El Sitio web está alojado por Vercel Inc., con sede en Estados Unidos. El
formulario de contacto se procesa en servidores de Vercel situados en la Unión
Europea; si el servicio implicara algún tratamiento fuera del Espacio Económico
Europeo, se aplican igualmente las garantías que exige la normativa de
protección de datos.

<!-- /CAPA 3 · INTERNATIONAL_TRANSFER_DETAILS -->

## 5. Conservación

Los datos se conservan únicamente durante el tiempo necesario para las
finalidades indicadas en el apartado 2 y, después, durante los plazos exigidos
para cumplir obligaciones legales o atender posibles responsabilidades.
Transcurridos esos plazos, se eliminan de forma segura.

## 6. Sus derechos

Puede solicitar:

- el acceso a sus datos;
- su rectificación;
- su supresión;
- la limitación del tratamiento;
- la portabilidad;
- la oposición al tratamiento;
- la retirada del consentimiento otorgado.

Puede ejercer sus derechos escribiendo a {{PRIVACY_EMAIL}}<!-- SI: hay RIGHTS_REQUEST_URL --> o mediante {{RIGHTS_REQUEST_URL}}<!-- /SI -->,
indicando qué derecho desea ejercer.

Si considera que sus datos no se han tratado conforme a la normativa aplicable,
puede presentar una reclamación ante la autoridad de protección de datos
competente: {{SUPERVISORY_AUTHORITY_NAME}}
([{{SUPERVISORY_AUTHORITY_URL}}]({{SUPERVISORY_AUTHORITY_URL}})).

## 7. Decisiones automatizadas

<!-- {{AUTOMATED_DECISION_INFO}} · CAPA 3 · por defecto de la base. Cambia si
     la web perfila o puntúa (fuera de la base). -->
No utilizamos sus datos para tomar decisiones automatizadas ni para elaborar
perfiles, y no le enviamos comunicaciones comerciales sin un consentimiento
expreso y separado.
<!-- /CAPA 3 -->

## 8. Seguridad

Aplicamos las medidas técnicas y organizativas adecuadas para proteger los datos
personales frente al acceso no autorizado, la alteración, la pérdida o la
divulgación.

## 9. Cookies

La información sobre cookies y tecnologías similares está disponible en la
[Política de cookies]({{COOKIE_POLICY_URL}}).

## 10. Cambios y contacto

Esta política puede actualizarse cuando cambien nuestras actividades de
tratamiento o la normativa aplicable. La fecha de la última revisión figura al
principio de esta página.

Contacto de privacidad: {{PRIVACY_EMAIL}}.
