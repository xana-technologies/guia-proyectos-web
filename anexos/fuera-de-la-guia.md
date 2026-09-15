# Anexo · Fuera de la guía

La guía cubre **webs publicadas**, que se hacen siempre con **Astro + base**
(formulario, legales, muro de cookies y analítica). Esto es lo que no sigue
esa regla y cómo tratarlo.

## Por qué toda web publicada lleva Astro, por simple que sea

Con la base, la web más pequeña ya tiene portada, tres páginas legales,
página de gracias y 404: seis páginas con la misma cabecera, pie y muro. Sin
Astro habría que copiar esas partes en cada una, y el formulario necesitaría
una función de servidor montada aparte. Astro publica HTML y CSS igualmente,
sin JavaScript de framework, y todos los proyectos comparten plantilla, flujos
y lecciones.

**Ejemplo:** grupomerhi se hizo en HTML, CSS y JS sin build porque era una sola
pantalla sin formulario, legales ni cookies. Si se hiciera hoy, sería una
[landing](../tipos-de-proyecto/landing.md) con la base.

Una web de empresa publicada casi siempre necesita al menos el aviso legal;
confirmarlo con quien lleve la parte jurídica si hay dudas.

## Prototipos y maquetas

**Qué son:** algo para enseñar una idea antes de construir, sin publicar como web.

**Cómo se hacen:** Figma, canvas de diseño o un artifact. Ejemplo: el
rediseño de `samboro.com/portal-clientes/` se entregó como prototipo en un
artifact antes de tocar el WordPress.

**Reglas:** el briefing del cliente manda sobre las suposiciones (copy literal,
secciones obligatorias); no se tocan colores ni tipografías de la marca.

## Páginas temporales

«Próximamente», pruebas de pocas semanas, páginas internas. Si se publican en
el dominio del cliente y recogen datos o miden visitas, pasan a ser una web y
llevan la base. Si no, pueden resolverse sin la guía, con `noindex`.

## Webs existentes en WordPress

- Se mantienen dentro de su tema y sus plugins (Samboro: tema Uncode).
- Un rediseño se prototipa aparte y se decide si se rehace en Astro.
- Si se rehace, se aplica el módulo [migración](../modulos/migracion.md)
  (xanatechnologies venía de WordPress).
- El *Protocolo de publicación ESTÁNDAR* está pensado para este tipo de web;
  lo que cambia en Astro está en [publicación](../flujos/publicacion.md#6-diferencias-con-el-protocolo).

## Aplicaciones: XPM-front

**Qué es:** la aplicación privada Xana Product Manager (PIM y DAM), no una web
pública. Tiene sus propias reglas y no sigue esta guía.

| | XPM-front |
| --- | --- |
| Stack | Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS 4, `next-intl`, `zustand`, Storybook |
| Runtime y despliegue | Bun; imagen Docker publicada en GHCR desde GitHub Actions |
| Commits | Prefijos `Update:` / `Fix:` / `Refactor:`, español, presente, ≤ 74 caracteres, atómicos (código, traducciones, docs) |
| Calidad | ESLint con `no-console: error` y `eslint-disable` con descripción |
| Dependencias | `bunfig.toml` con `minimumReleaseAge` de 3 días |
| Documentación | `docs/` por dominio, patrones y bitácora |

**Qué sí puede aprovecharse aquí:** la idea de exigir antigüedad mínima a las
dependencias y de documentar por dominio.

Fuentes: `XPM-front/package.json`, `bunfig.toml`, `eslint.config.mjs`,
`docs/CONVENCIONES_COMMITS.md`, `.github/workflows/build.yml`.

## Bancos de pruebas

- `xanasystem-lab`: worktree de xanasystem-web para experimentos (animaciones,
  tipografías). No se despliega.
- `claysoftheworld`: carpeta vacía de un primer intento; se puede borrar
  ([deuda](deuda-proyectos.md)).
