---
name: montar-legales
description: Montar el aviso legal, la política de privacidad y la política de cookies de una web de Xana (texto base + datos del cliente + datos de la web), o actualizarlas cuando la web añade algo que las afecta (un vídeo incrustado, una newsletter, una etiqueta en GTM, un idioma). Usar al preparar, revisar o cambiar los textos legales.
---

# Montar los legales

Todo el detalle está en el flujo [`flujos/legales.md`](../../flujos/legales.md)
(las rutas de esta skill son relativas a su carpeta). **Léelo entero antes de
empezar.** Esta skill marca el orden, qué comprobar y dónde parar.

La regla que manda sobre todo lo demás: **la política cuenta lo que la web
hace, ni más ni menos**, y **no se inventa ningún dato** del titular.

## 1. Antes de tocar nada

| Qué | Dónde mirarlo | Si falta |
| --- | --- | --- |
| Datos del cliente (capa 2) | `docs/03-contenidos/legal/datos-legales.md` | Montar igual con `null` + `pending`; se ven `[[ ]]` y **no se publica**. Si ni siquiera se ha enviado la plantilla, prepararla ([`plantillas/cliente/datos-legales.md`](../../plantillas/cliente/datos-legales.md)) y pasársela a Belén |
| País del cliente | Datos legales, briefing | Si no es España: fuera de la base, legislación y autoridad en `[[ ]]` y avisar a Belén |
| Idiomas de la web | `CLAUDE.md` del proyecto | Los tres documentos en cada idioma, con URL traducida (flujo · Webs con varios idiomas) |
| Módulos montados (embeds, newsletter, confirmación, actividad regulada) | El código, no el briefing | Solo se añaden párrafos de módulos que están montados |
| Legales de la web antigua | `{{CURRENT_LEGAL_CONTENT}}` | Datos del titular solo para pre-rellenar y confirmar; tratamientos, proveedores y cookies **no se copian nunca** |
| Textos de la asesoría del cliente | Belén | Su redacción manda: contrastar con lo que hace la web y pasar las diferencias a Belén, sin corregirlas |

Explica a Belén el plan y las dudas, y espera antes de empezar
([forma de trabajo §1](../../forma-de-trabajo.md)).

## 2. Pasos

Siguen «Pasos y quién · 07 · Construcción» del flujo.

1. **Traer el montaje** de clays-of-the-world (lista en «Dónde queda cada cosa»).
2. **Capa 2 en `entity.ts`**. Antes, dos comprobaciones: razón social ≠ marca y
   domicilio social ≠ sede del pie.
3. **Plantillas a `src/data/legal/es.ts`** (y un fichero por idioma): capa 1
   literal, variables resueltas con [`plantillas/legales/variables.md`](../../plantillas/legales/variables.md),
   párrafos de [`parrafos-por-modulo.md`](../../plantillas/legales/parrafos-por-modulo.md)
   solo si el módulo existe. Para pasar texto a bloques, tabla «De la
   plantilla a los bloques».
4. **Capa 3 contra el código**, no contra la plantilla:
   - Proveedores: busca `googletagmanager`, `challenges.cloudflare.com`,
     `api.brevo.com`, `vimeo`, `youtube`, `google.com/maps`. Lo que aparece se
     declara; lo que no aparece, no.
   - Campos del formulario de la política = campos de `Contact.astro`.
   - Tabla de cookies y textos del muro = `CookieConsent.astro` (nombre y días
     de `cc_cookie`, categorías). Sin `_gid`, `_gat` ni `PHPSESSID`.
5. **Páginas, robots y pie**: `noindex, follow`, fuera del sitemap y los 4
   elementos del pie (flujo · Robots y pie). Si el footer aún no existe, deja
   anotado que `xana:footer` tiene que llevarlos.
6. **Preguntar a Belén**: aviso de borrador sí o no, y revisión jurídica sí o
   no. Si hay revisión, sacar la prosa a `docs/03-contenidos/legal/legal-copy-es.md`.
7. **`CHECKLIST.md` del proyecto**: cada `[[ ]]` pendiente y de quién depende.

## 3. Al terminar

- Recorre la checklist **07 · Construcción** del flujo y di qué está comprobado
  y qué no.
- La comprobación con navegador (rechazar, aceptar, retirar consentimiento,
  sin JavaScript) es la checklist **08 · Revisión**: hazla en la URL temporal
  si ya está desplegada, o déjala anotada.
- Nunca digas que la web «cumple el RGPD» o está «100 % legal».
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
