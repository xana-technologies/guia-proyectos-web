# Plantilla · Aviso legal (ES)

> Capa 1 (texto base) con los huecos de las capas 2 y 3. Leyenda de marcas en
> [`README.md`](README.md); variables en [`variables.md`](variables.md).
> Ruta en una web en español: `/aviso-legal/`.
>
> Estructura del *Protocolo de publicación ESTÁNDAR* (7 apartados), con la
> redacción de clays-of-the-world (`src/data/legal/es.ts`) y APC
> (`src/pages/aviso-legal.astro`).

---

# Aviso legal

Última actualización: {{LEGAL_NOTICE_EFFECTIVE_DATE}}

Este aviso legal regula el acceso, la navegación y el uso del sitio web
{{WEBSITE_URL}} (el «Sitio web»)<!-- SI: hay nombre comercial distinto de la razón social -->, que opera bajo el nombre comercial {{COMPANY_TRADE_NAME}}<!-- /SI -->.
El acceso al Sitio web implica la aceptación de las condiciones que se recogen a
continuación.

## 1. Titular del sitio web

En cumplimiento del deber de información que establece la normativa de
servicios de la sociedad de la información y de comercio electrónico, se
facilitan a continuación los datos identificativos del titular.

<!-- Bloque `identity`: sale de `src/data/legal/entity.ts`, no se escribe a mano.
     Una fila sin dato se pinta [[ ]]; una fila que no aplica se quita. -->

| | |
| --- | --- |
| Titular | {{COMPANY_LEGAL_NAME}} |
| NIF | {{COMPANY_TAX_ID}} |
| Domicilio social | {{COMPANY_REGISTERED_ADDRESS}}, {{COMPANY_COUNTRY}} |
| Email | {{COMPANY_EMAIL}} |
| Teléfono | {{COMPANY_PHONE}} <!-- SI: el cliente lo da. Opcional --> |
| Datos registrales | {{COMPANY_REGISTRY_DETAILS}} <!-- SI: sociedad inscrita. Un autónomo no tiene: fila fuera --> |
| Actividad | {{BUSINESS_ACTIVITY}} |

<!-- SI: hay nombre comercial -->
{{COMPANY_TRADE_NAME}} es un nombre comercial cuya titularidad corresponde a la
entidad identificada arriba.
<!-- /SI -->

## 2. Objeto y condiciones de uso

<!-- CAPA 3 · DEDUCIDO: la base es una web informativa sin venta en línea.
     Si la web vende o contrata en línea, este párrafo es falso y hacen falta
     condiciones de contratación: fuera de la base, con revisión jurídica. -->
El Sitio web tiene por objeto presentar la actividad y los servicios de
{{COMPANY_TRADE_NAME}}. Tiene carácter informativo: no realiza ventas en línea
ni constituye una oferta comercial vinculante.
<!-- /CAPA 3 -->

El usuario se compromete a utilizar el Sitio web de forma lícita y a no
emplearlo para actividades que puedan dañar, alterar, inutilizar o sobrecargar
su funcionamiento, vulnerar derechos de terceros o acceder sin autorización a
sistemas o información.

El titular podrá modificar, suspender o interrumpir el acceso a los contenidos
del Sitio web, de forma temporal o definitiva, sin previo aviso.

## 3. Propiedad intelectual e industrial

Todos los contenidos del Sitio web —textos, fotografías, gráficos, imágenes,
diseño, logotipos, marcas y código fuente— pertenecen a {{COMPANY_LEGAL_NAME}}
o a terceros que han autorizado su uso, y están protegidos por la normativa de
propiedad intelectual e industrial. Queda prohibida su reproducción,
distribución, comunicación pública o transformación sin autorización expresa,
salvo en los usos permitidos por la ley. Las marcas de terceros que aparecen en
el Sitio web pertenecen a sus respectivos titulares.

<!-- OPCIONAL «Imágenes de banco» → parrafos-por-modulo.md § Imágenes de banco -->

## 4. Responsabilidad

El titular procura mantener la información y los servicios del Sitio web
disponibles y actualizados, pero no garantiza la ausencia absoluta de errores o
interrupciones, ni se hace responsable del uso que terceros puedan hacer de la
información publicada.

El Sitio web puede contener enlaces a páginas de terceros. El titular no es
responsable de los contenidos, políticas o servicios de esos sitios, que
gestionan sus respectivos responsables, salvo cuando legalmente corresponda.

<!-- MÓDULO actividad regulada → parrafos-por-modulo.md § Actividad regulada -->

## 5. Protección de datos y cookies

El tratamiento de los datos personales se rige por la
[Política de privacidad]({{PRIVACY_POLICY_URL}}). El uso de cookies y
tecnologías similares se describe en la
[Política de cookies]({{COOKIE_POLICY_URL}}).

## 6. Legislación aplicable y jurisdicción

Este aviso legal se rige por {{GOVERNING_LAW}}, sin perjuicio de los derechos
imperativos que correspondan a consumidores y usuarios conforme a la normativa
aplicable. Para la resolución de cualquier controversia derivada del acceso o
del uso del Sitio web, las partes se someten a los juzgados y tribunales que
resulten competentes conforme a esa normativa.

<!-- MÓDULO idiomas (opcional) → parrafos-por-modulo.md § Idiomas -->

## 7. Contacto

Para cualquier cuestión relacionada con este aviso legal puede escribir a
{{LEGAL_CONTACT_EMAIL}}.
