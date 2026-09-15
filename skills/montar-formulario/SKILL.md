---
name: montar-formulario
description: Montar el formulario de contacto de una web Astro de Xana (Brevo + Turnstile + firewall de Vercel) y probarlo en la URL temporal. Usar cuando haya que añadir o adaptar el formulario de contacto, añadirle un campo o preparar sus variables de entorno.
---

# Montar el formulario

Todo el detalle está en el flujo [`flujos/formulario.md`](../../flujos/formulario.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.** Esta skill marca el orden, qué comprobar y dónde parar.

## 1. Antes de tocar nada

Comprueba en el proyecto y pregunta lo que falte. No se inventa ningún dato.

| Qué | Dónde mirarlo | Si falta |
| --- | --- | --- |
| Versión de Astro | `package.json` | — (decide la versión del adaptador, flujo §4 paso 1) |
| Clave de Brevo del proyecto | Preguntar a Belén | **Parar** y pedirla con [`plantillas/equipo/alta-formulario.md`](../../plantillas/equipo/alta-formulario.md). Nunca la de otro proyecto |
| Destinatarios del cliente | `CLAUDE.md` / `docs/` del proyecto | Montar con correos de prueba y anotar en `CHECKLIST.md` que **no se publica** hasta tenerlos ([`plantillas/cliente/formulario.md`](../../plantillas/cliente/formulario.md)) |
| Dominios | `CLAUDE.md` del proyecto | Preguntar: producción, `www` y URL temporal |
| Idiomas y campos extra | Briefing, `CLAUDE.md` | Los de la base (flujo §2) |

Si el proyecto pide varios pasos, confirmación al usuario, varios formularios
o histórico de envíos, no es esta skill: es el módulo
[`formulario avanzado`](../../modulos/formulario-avanzado.md).

Explica a Belén el plan y las dudas, y espera antes de empezar
([forma de trabajo §1](../../forma-de-trabajo.md)).

## 2. Pasos

1. **Adaptador y dominios permitidos** → flujo §4, pasos 1 y 2.
2. **Copiar y adaptar** los ficheros de clays-of-the-world → tabla del flujo §4
   paso 3. Después de copiar, busca `forms.xanasystem.com` y `??` en
   `form.config.ts`: los dos han roto formularios
   ([trampas · formulario](../../trampas-y-lecciones.md#formulario)).
3. **Formulario visible** con las reglas del flujo §8 y **plantilla del correo**
   con las del §9. Textos y mensajes de error desde los datos, no en el código.
4. **Turnstile y `.env` local** → flujo §4, pasos 4 y 5.
5. **Pasar a Belén** la lista de variables para Vercel (§5) y la regla de
   firewall (§6). Claude no configura Vercel ni despliega. Nunca escribas el
   valor de una clave en un mensaje ni en un fichero que vaya a git.
6. **Probar**:
   - En local: `npm run build` y envío con las claves de prueba de Turnstile.
   - En la URL temporal, cuando Belén haya desplegado: envío correcto, errores
     de validación, envío sin JavaScript y 429 al sexto envío.
7. **Legales**: la política de privacidad menciona el formulario, Brevo y
   Cloudflare ([`flujos/legales.md`](../../flujos/legales.md)).
8. **`CHECKLIST.md` del proyecto**: lo hecho y lo pendiente, con de quién depende.

## 3. Al terminar

- Recorre la checklist **Construcción** del flujo §11 y di qué está comprobado
  y qué no. Nunca «ya está» sin haberlo probado.
- Si algo falla → `xana:diagnosticar-formulario`.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
- Lo de la publicación (correos definitivos, prueba real por idioma) se hace en
  la fase 09, con la checklist **Publicación** del flujo §11.
