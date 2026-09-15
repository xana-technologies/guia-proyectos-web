---
name: preparar-proyecto
description: Crear y dejar listo el proyecto de una web nueva de Xana — carpeta, Astro 7 con @astrojs/vercel 11, configuración (package.json, tsconfig, .gitignore, vercel.json, .env.example, .claude/launch.json), git con el remoto de xanasystem, CLAUDE.md, CHECKLIST.md y carpetas docs/ y brand/. Usar al montar el repositorio de un proyecto nuevo.
---

# Preparar el proyecto

Todo el detalle está en el flujo [`flujos/arranque-proyecto.md`](../../flujos/arranque-proyecto.md)
§3, §4.3–4.6, §6 y §7 (las rutas de esta skill son relativas a su carpeta).
**Léelo entero antes de empezar.**

## 1. Antes de tocar nada

| Qué | Si falta |
| --- | --- |
| `docs/00-arranque/brief.md` con tipo y módulos | Hacer antes `xana:analizar-briefing` |
| Repositorio vacío en `xanasystem` (lo crea Belén) | Pedírselo con el nombre en minúsculas con guiones |
| ¿`main` ya sirve algo publicado? | **Preguntar a Belén** antes de tocarlo |
| ¿Existe ya el repo plantilla en `xanasystem`? | Si no, `npm create astro@latest` + piezas según [`plantillas/codigo/`](../../plantillas/codigo/README.md) |

Explica a Belén el plan y espera ([forma de trabajo §1](../../forma-de-trabajo.md)).

## 2. Pasos

1. **Proyecto** en `C:\Proyectos\<repo>` con las versiones del flujo §3:
   Astro 7, `@astrojs/vercel` 11, Node ≥ 22.12. **Las versiones no se copian
   del proyecto de referencia** (clays va en 5/8, xanasystem-web en 6/10).
2. **Solo dependencias de la tabla §3.** Cualquier otra se propone antes.
3. **Configuración** con los ficheros del flujo §6: `package.json` con
   `engines`, `tsconfig.json` con `@/`, `.gitignore` (con `.env*`, `*.mp4` y
   `docs/imagenes/originales/` **desde el primer commit**), `.editorconfig`,
   `.gitattributes`, `vercel.json` con `regions: ["fra1"]`, `.env.example` sin
   valores, `.claude/launch.json`.
4. **Restos del proyecto de origen**, si se ha copiado algo: `site` y
   `allowedDomains` de otro dominio, ID de GTM ajeno, correos de otro cliente,
   remitente `forms.xanasystem.com`, `??` en `form.config.ts` (flujo §7).
5. **Comprobar** `npm run build` y `npm run check`.
6. **Git**: `git init -b main` y remoto con el alias
   `git@github-belenxana:xanasystem/<repo>.git`. Nunca `github.com` ni HTTPS.
7. **Documentación**: `CLAUDE.md` y `CHECKLIST.md` desde
   [`plantillas/proyecto/`](../../plantillas/proyecto/README.md) con tipo,
   módulos, escenarios y el bloque de cada módulo; carpetas de `docs/` y
   `brand/` según [`docs-estructura.md`](../../plantillas/proyecto/docs-estructura.md);
   `docs/imagenes/inventario.md` empezado.
8. **Primer commit y push**: propón el mensaje y espera el sí. `git add` con
   rutas, nunca `-A`.
9. **Vercel lo hace Belén** (flujo §4.4): proyecto en *Xana Technologies'
   projects*, commit manual desde GitHub con `xanasystem`. Claude **no** usa el
   CLI de Vercel. Cuando Belén pase la URL temporal, anotarla en `CLAUDE.md` y
   `CHECKLIST.md`.

## 3. Al terminar

- Recorre las checklists **Proyecto**, **Vercel** y **Documentación** del flujo
  §8 y di qué está comprobado y qué no.
- Tras un push di «subido a GitHub, pendiente de tu paso en GitHub», nunca
  «publicado».
- Si algo falla (push denegado, adaptador que no instala, preset equivocado en
  Vercel), busca en [trampas](../../trampas-y-lecciones.md).
