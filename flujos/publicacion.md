# Flujo · Publicación

Poner la web en su dominio real y conectar lo que depende de él. Es el
«Para publicar» y «Después de publicar» del *Protocolo de publicación
ESTÁNDAR*, adaptado a Astro + Vercel. **Empieza cuando la
[revisión](revision.md) está cerrada y el cliente ha dado el visto bueno.**

## 1. Cómo se publica en Vercel (siempre)

- El proyecto de Vercel está conectado a GitHub y se construye desde `main`.
- **Publica siempre Belén**: después del push, hace un commit manual desde la
  web de GitHub con la cuenta `xanasystem`, que es lo que lanza el despliegue.
  Vercel rechaza los commits de `belen@xanasystem.com`.
- **Claude nunca publica** (ni `vercel deploy` ni `vercel --prod`: el CLI de
  Claude tiene otra sesión y ya publicó una vez en una cuenta personal).
- **Claude nunca dice «publicado»** después de un push. Dice: «subido a
  GitHub, pendiente de tu paso en GitHub».
- Commit y push, solo con permiso → [forma de trabajo](../forma-de-trabajo.md).

Mientras no hay dominio, la web se ve en la **URL temporal** (`*.vercel.app`).
Esta fase conecta el dominio a esa misma web.

## 2. Qué tenemos

| Situación | Qué cambia |
| --- | --- |
| Dominio nuevo, sin web | Solo DNS |
| El dominio tiene una web antigua | Redirecciones antes del cambio → módulo [migración](../modulos/migracion.md); no tocar registros de otros servicios (correo, subdominios) |
| El dominio estuvo en otra cuenta de Vercel | Vercel pide un **TXT `_vercel`** de verificación |
| Tenemos acceso al DNS (DonDominio) | Lo cambia Belén |
| Lo gestiona el técnico del cliente | Se le envían las [instrucciones de DNS](../plantillas/cliente/instrucciones-dns.md) |

## 3. Antes de tocar el DNS

- [ ] Revisión cerrada y visto bueno del cliente
- [ ] Variables de entorno de producción en Vercel (Production y Preview),
      ninguna vacía; tras cambiarlas hace falta un despliegue nuevo
- [ ] `site` de `astro.config.mjs` con el dominio definitivo (canonical,
      sitemap, Open Graph salen de ahí)
- [ ] Dominio definitivo en `security.allowedDomains` y en el widget de Turnstile
- [ ] Regla de firewall del formulario publicada; Spend Management activo
- [ ] Funciones en región europea (`"regions": ["fra1"]` en `vercel.json`,
      visible en Settings → Functions): la política de privacidad lo afirma

## 4. Para publicar

### 4.1 Dominio principal

Decidir con el cliente **con o sin `www`**. La otra versión redirige a la
principal. Anotarlo en el `CLAUDE.md` del proyecto (la usan `site`, sitemap
y canonical).

### 4.2 Añadir el dominio en Vercel

Vercel → proyecto → Settings → **Domains** → añadir `cliente.com` y
`www.cliente.com`, marcando cuál redirige a cuál. Vercel muestra los
registros exactos que hay que crear.

### 4.3 DNS

| Tipo | Nombre | Valor |
| --- | --- | --- |
| **A** | `@` | La IP que indique Vercel |
| **CNAME** | `www` | El destino que indique Vercel (`….vercel-dns-0xx.com.`) |
| **TXT** | `_vercel` | Solo si Vercel lo pide (dominio usado antes en otra cuenta) |

- **Copiar siempre los valores de la pantalla de Vercel**: han cambiado con el
  tiempo (`216.198.79.1` en xanatechnologies, `216.150.1.1` en grupomerhi).
- En DonDominio: zona DNS del dominio → modificar el A de `@` y crear o
  modificar el CNAME de `www`. **No borrar** registros MX (correo), TXT de
  otros servicios ni subdominios que apuntan a otro sitio.

### 4.4 Comprobar

- La propagación puede tardar horas. Comprobar con una herramienta de DNS
  (enlace en el protocolo) y en Vercel → Domains («Valid Configuration»).
- Vercel emite el certificado solo.
- [ ] `https://cliente.com` carga la web con candado
- [ ] La otra versión (`www` o sin `www`) redirige
- [ ] `http://` redirige a `https://`

## 5. Después de publicar

### 5.1 SEO

- [ ] `https://cliente.com/robots.txt` existe y **no bloquea** la web
- [ ] `https://cliente.com/sitemap.xml` existe y lista solo páginas indexables con el dominio definitivo
- [ ] Las páginas indexables **no** llevan `noindex` en producción
- [ ] Google Search Console: propiedad verificada (Belén) y sitemap enviado
- [ ] Si había web antigua: redirecciones comprobadas

### 5.2 Formulario

- [ ] `FORM_TO/CC/BCC` con los correos definitivos del cliente
- [ ] Clave de Brevo de producción (la del proyecto, no una de pruebas)
- [ ] Prueba real **por formulario y por idioma**
- [ ] El cliente confirma que le llega (y mira spam)
- [ ] Ingeniería: no cae en spam, aparece en el registro de Brevo, 429 desde fuera
- Detalle → [flujo de formulario](formulario.md)

### 5.3 Analítica

- [ ] GTM: contenedor con GA4 (y nada sin declarar en la política)
- [ ] Aceptar el muro en la web publicada y ver la visita en GA4 en tiempo real
- [ ] Retención de datos de GA4 en **14 meses** (propiedad → Administrar →
      Retención de datos): la política de privacidad lo afirma
- [ ] Informe tipo en Looker Studio
- [ ] A las 24 h, comprobar que llegan datos
- Tutorial interno (DTC, vale para cualquier web cambiando la ubicación):
  <https://drive.google.com/drive/folders/1OAte2uYteF-W7mL4LIYUKgAhkj48Uaw4>
- Detalle → [flujo de cookies y analítica](cookies-analitica.md)

### 5.4 Cierre

Pasa a [entrega y mantenimiento](entrega-mantenimiento.md): documentación
interna (Tabla Clientes Xana, Factura Directa, Drive) y entrega al cliente.

## 6. Diferencias con el protocolo

El protocolo está pensado sobre todo para WordPress:

| El protocolo dice | En nuestras webs |
| --- | --- |
| Metadatos con un plugin (Rank Math) | En el código de cada página → [SEO técnico](seo-tecnico.md) |
| Muro de cookies con un plugin | Nuestro componente → [cookies y analítica](cookies-analitica.md) |
| Tabla con `PHPSESSID`, `_gid`, `_gat` | Solo `cc_cookie`, `_ga`, `_ga_<ID>` |
| Textos legales y redirecciones con ChatGPT | Plantillas de la guía con Claude → [legales](legales.md) |
| «Añadir script de GTM a la web» | Ya está en el muro; solo carga tras aceptar |
| «Llevar el formulario a servidor de producción» | Variables de producción en Vercel |
| Plugin de histórico de envíos | No hay: registro de Brevo; si el cliente lo pide → [formulario avanzado](../modulos/formulario-avanzado.md) |

## 7. Reglas y trampas

- Push ≠ publicado. Publicar = paso manual de Belén.
- No lanzar despliegues desde el CLI de Claude.
- Las variables de entorno no se aplican hasta el siguiente despliegue.
- Una variable guardada vacía en Vercel rompe los valores por defecto si el
  código usa `??` → usar `||` (xanatechnologies).
- Sin el dominio en `allowedDomains`, el formulario da 403 solo en ese dominio.
- Previews de Vercel: Vercel les pone `noindex` por su cuenta; la producción
  no, así que el `noindex` de las páginas internas tiene que estar en el código.
- Registrar lo que queda pendiente tras publicar en el `CHECKLIST.md` del
  proyecto (en xanatechnologies quedaron `www`, prueba del formulario,
  firewall, Search Console y GA4).
