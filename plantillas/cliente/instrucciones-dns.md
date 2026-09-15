# Instrucciones para conectar el dominio

*Plantilla para el cliente o su técnico. Se envía en la publicación, con los
valores copiados de Vercel → Settings → Domains.*

---

Hola,

La nueva web de **{marca}** está lista para publicarse en **{dominio}**. Para
conectarla necesitamos que quien gestiona el dominio ({proveedor, p. ej.
DonDominio}) cambie estos registros DNS:

| Tipo | Nombre / host | Valor | Para qué |
| --- | --- | --- | --- |
| A | `@` (dominio raíz) | `{IP de Vercel}` | Que `{dominio}` muestre la web nueva |
| CNAME | `www` | `{destino de Vercel}` | Que `www.{dominio}` también funcione |
| TXT | `_vercel` | `{valor}` | *Solo si aparece aquí:* verificación del dominio |

**Importante**
- **No borrar ni modificar** los registros de correo (MX) ni otros registros
  TXT o subdominios que ya existan: el correo y otros servicios seguirían
  funcionando igual.
- Si ya existe un registro A para `@` o un CNAME para `www`, se **sustituye**
  su valor por el nuestro.
- El cambio puede tardar unas horas en verse. El certificado de seguridad
  (https) se activa solo.

Cuando esté hecho, avisadnos y comprobamos que todo funciona.

Gracias.

---

*Para Claude:*
- Rellenar con los valores exactos de la pantalla de Vercel, nunca de memoria.
- Indicar si la versión principal es con o sin `www`.
- Si el dominio tiene web antigua con otras URLs, las redirecciones tienen que
  estar hechas antes de enviar esto ([migración](../../modulos/migracion.md)).
