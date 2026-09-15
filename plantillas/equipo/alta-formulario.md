# Alta de formulario · petición a ingeniería

*Se envía en el arranque de cada proyecto. Complementa el
[flujo de formulario](../../flujos/formulario.md).*

---

**Asunto:** Alta de formulario · {proyecto}

Hola, empezamos la web de **{cliente}** ({dominio}). Para el formulario de
contacto necesito:

1. **Clave de Brevo del proyecto** (`form-{cliente}`).
2. **IP de la oficina** para la lista blanca del firewall, si ha cambiado.
3. Confirmar que el remitente sigue siendo **`no-reply@xanatechnologies.com`**.

*Solo si el proyecto lleva confirmación al usuario:*
4. Verificar el dominio del cliente en Brevo (DKIM y DMARC) y darme el
   remitente bajo ese dominio.

Os aviso cuando esté publicado para la comprobación final.

---

## Qué hace ingeniería

**Siempre**
- Crear la clave en Brevo (SMTP & API → API Keys), una por proyecto, guardarla
  en el gestor de secretos y entregarla. Brevo solo la muestra una vez.

**Solo con confirmación al usuario**
- Añadir el dominio del cliente en Brevo, publicar los registros (código de
  verificación, DKIM, DMARC con `rua`), esperar los checks verdes y crear el
  remitente. Sin DKIM, los correos caen en spam (Brevo envía desde IP
  compartida: SPF no alinea, DMARC solo pasa por DKIM). Si ya hay un DMARC, no
  duplicarlo: añadir el `rua`.

**Tras la publicación**
- Envío de prueba real: llega y no cae en spam.
- Aparece en Brevo → Transactional → Logs.
- Desde la oficina no se bloquea; desde fuera, el sexto envío en 10 minutos da 429.
- Spend Management del equipo sigue activo.

**Rotación o baja**
- Rotar: clave nueva → actualizar `BREVO_API_KEY` en Vercel → despliegue →
  borrar la antigua. Inmediato si una clave se filtra.
- Baja del cliente: borrar su clave (y, si lo tenía, su remitente y dominio).

**Límites de la cuenta compartida**
- El plan gratuito de Brevo da 300 correos/día para todos los proyectos. Si
  el volumen sube sin tráfico real, hay spam pasando: revisar Turnstile.
- Una suspensión de la cuenta afectaría a todos los clientes.

*Fuente: `magnaceramica/docs/forms/Runbook_Ingenieria_Alta_Cliente_Formularios.md`
(actualizado: el remitente por defecto ya no es `forms.xanasystem.com`).*
