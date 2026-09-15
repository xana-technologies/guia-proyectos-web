---
name: diagnosticar-formulario
description: Diagnosticar el formulario de contacto de una web de Xana cuando no funciona — error 403, 429, verification, validation, config o send, el widget de Turnstile no aparece, o responde bien pero el correo no llega. Usar cuando digan «el formulario no va» o haya un error al enviar.
---

# Diagnosticar el formulario

El sistema está en [`flujos/formulario.md`](../../flujos/formulario.md) y los
fallos ya vistos en [`trampas-y-lecciones.md` · Formulario](../../trampas-y-lecciones.md#formulario)
(rutas relativas a la carpeta de esta skill).

## 1. Recoger el síntoma exacto

Antes de tocar código, averigua:

- **Dónde pasa**: local, URL temporal o producción. Si solo pasa en Vercel,
  sospecha de dominios o variables.
- **Qué ve el visitante** y **qué responde el endpoint**: código HTTP, `error`
  y `detail`. Interprétalo con el contrato del flujo §7.
- **Qué dicen los registros**, en este orden: consola del navegador
  (`[contact] Envío rechazado`) → Vercel → Logs (`[contact] …`) → Brevo →
  Transactional → Logs. Lo que no puedas ver tú, pídeselo a Belén.

Nunca pidas ni muestres el valor de una clave: basta con saber si la variable
existe.

## 2. Buscar la causa

1. Busca el síntoma en **trampas y lecciones** y en la tabla del flujo §10.
2. Comprueba en el código que esa es la causa antes de cambiar nada. Lo que
   más se repite:
   - `security.allowedDomains` sin el dominio desde el que se sirve la web.
   - `??` en vez de `||` en `form.config.ts`, o remitente `forms.xanasystem.com`.
   - Mínimos distintos entre el formulario y el endpoint.
   - Falta `turnstile.reset()` tras cada intento.
   - `<form>` sin `method="post"` o endpoint sin respuesta 405 a `GET`.
3. **Falsos fallos** que no son del formulario:
   - 403 en una prueba desde la terminal: falta la cabecera `Origin`.
   - 429 en pruebas: el firewall; probar desde la IP de la oficina.
   - Variable recién añadida que no se ve: falta un despliegue.

## 3. Arreglar

- **Si es código**: cambio mínimo, solo lo que causa el fallo.
- **Si es configuración**, di quién lo hace y no lo hagas tú:
  - Variables de Vercel, regla de firewall y despliegue → Belén.
  - Dominios del widget de Turnstile → Belén (Cloudflare).
  - Clave de Brevo, IP autorizada, spam o remitente → ingeniería.
- Después del arreglo, **vuelve a probar** en el mismo sitio donde fallaba. Si
  no se puede comprobar hasta que Belén despliegue, dilo así.

## 4. Si la causa no estaba en la guía

Propón a Belén una entrada nueva para
[`trampas-y-lecciones.md`](../../trampas-y-lecciones.md) con el formato de las
demás (síntoma → causa → solución → dónde pasó). No la añadas sin permiso.
