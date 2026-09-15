---
name: publicar
description: Acompañar la publicación de una web de Xana en su dominio real — comprobaciones antes de tocar el DNS, dominio principal con o sin www, registros DNS, verificación del certificado y redirecciones, y después SEO en producción, formulario real y analítica. Claude prepara y comprueba; el DNS y el despliegue los hace Belén.
disable-model-invocation: true
---

# Publicar

Todo el detalle está en el flujo [`flujos/publicacion.md`](../../flujos/publicacion.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.**

Esta skill **solo se usa cuando Belén la llama** (`/xana:publicar`).

## Reglas que no se saltan

- **Publica siempre Belén**: push y después su commit manual desde GitHub con
  la cuenta `xanasystem`.
- **Claude nunca ejecuta** `vercel deploy`, `vercel --prod` ni `vercel link`.
- **Claude nunca dice «publicado»** tras un push: «subido a GitHub, pendiente de
  tu paso en GitHub».
- **El DNS lo cambia Belén** (o el técnico del cliente con las
  [instrucciones de DNS](../../plantillas/cliente/instrucciones-dns.md)). Claude
  no toca DonDominio ni ningún panel.
- Commit y push, solo con su sí ([forma de trabajo §5](../../forma-de-trabajo.md)).

## 1. Antes de empezar

- La **revisión está cerrada** (`xana:revision-final`) y el cliente ha dado el
  visto bueno. Si no, parar y decirlo.
- ¿Dominio nuevo, con web antigua, usado antes en otra cuenta de Vercel? ¿Quién
  gestiona el DNS? (flujo §2). Con web antigua, redirecciones **antes** del
  cambio → módulo [migración](../../modulos/migracion.md).

## 2. Antes de tocar el DNS

Comprobar en el código y pedir a Belén que confirme lo de Vercel (flujo §3):

- `site` con el dominio definitivo; dominio en `security.allowedDomains`.
- Variables de producción en Vercel, ninguna vacía (y que hace falta desplegar
  tras cambiarlas).
- Dominio en el widget de Turnstile; regla de firewall publicada; Spend
  Management activo.
- `"regions": ["fra1"]` en `vercel.json`.

## 3. Para publicar

1. **Dominio principal**: con o sin `www`, decidido con el cliente y anotado en
   `CLAUDE.md` (flujo §4.1).
2. **Belén añade los dominios en Vercel** y copia los registros **de la
   pantalla de Vercel** (cambian con el tiempo). Si hace falta, preparar las
   instrucciones para el técnico del cliente.
3. **DNS** (flujo §4.3): A de `@`, CNAME de `www`, TXT `_vercel` solo si Vercel
   lo pide. Recordar **no borrar** MX, TXT de otros servicios ni subdominios.
4. **Comprobar** cuando propague: candado, la otra versión redirige, `http`
   redirige a `https`.

## 4. Después de publicar

Recorrer con Belén el flujo §5, comprobando lo que se pueda desde aquí:

- **SEO**: `/robots.txt` y `/sitemap.xml` en el dominio, sin `noindex` en
  indexables; Search Console y sitemap enviado (Belén).
- **Formulario**: correos definitivos, prueba real por formulario e idioma,
  confirmación del cliente, comprobaciones de ingeniería.
- **Analítica**: GA4 en GTM, visita en tiempo real tras aceptar, retención a 14
  meses, informe, datos a las 24 h.

## 5. Al terminar

- Lo pendiente, a `CHECKLIST.md` con responsable.
- Sigue `xana:entrega`.
