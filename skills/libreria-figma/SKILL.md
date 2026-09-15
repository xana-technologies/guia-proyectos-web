---
name: libreria-figma
description: Crear o actualizar en Figma la librería del design system de una web de Xana a partir de src/styles/tokens.css y los componentes base — variables con la misma separación en capas y los mismos nombres, y componentes con sus variantes enlazadas a variables. Usar al pasar de la fase de design system a la de diseño en Figma, o cuando cambian los tokens y hay que llevarlos a Figma.
---

# Librería en Figma

Todo el detalle está en el flujo [`flujos/diseno-figma.md`](../../flujos/diseno-figma.md)
§3 «Escribir en Figma» y §4 «Paso 05 → 06: la librería» (las rutas de esta skill
son relativas a su carpeta). **Léelo antes de empezar.**

**En Figma se diseña con los tokens y componentes del design system, no con
valores sueltos.** La librería sale del código, no al revés.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Tokens y componentes base | `src/styles/tokens.css`, `src/components/`, `/styleguide` | Primero `xana:crear-tokens` y `xana:componentes-base` |
| Styleguide revisado por Belén | Belén | Esperar: una librería sobre tokens sin revisar se rehace |
| Fichero de Figma | `CLAUDE.md` del proyecto | Pedir a Belén que lo cree o lo comparta, y anotar el enlace. Si hay que crearlo, cargar antes `figma-create-new-file` |
| ¿El cliente trae su propio Figma con design system? | `CLAUDE.md` | Entonces no se genera: los tokens salen de ese Figma (`xana:crear-tokens`) |

Explica a Belén qué vas a crear (variables, componentes, página del fichero) y espera.

## 2. Pasos

1. **Cargar las skills del plugin de Figma**: `figma-use` (obligatoria antes de
   cualquier `use_figma`) y `figma-generate-library`.
2. **Variables** con los tokens de `tokens.css`, con **la misma separación en
   capas y los mismos nombres**: marca, extensiones y semánticos; espaciado,
   tipografía, radios y sombras. Nada que no esté en `tokens.css`.
3. **Componentes base** (Button, Logo, Section, SectionHeader…) con sus variantes
   enlazadas a variables, no a valores fijos.
4. Todo en la página de librería del fichero; anotar su enlace en `CLAUDE.md`.
5. Si se usa Code Connect para enlazar cada componente de Figma con su
   `.astro`, cargar `figma-code-connect` (ficheros `.figma.ts` junto a los componentes).

## 3. Al terminar

- Comparar la librería con `/styleguide`: mismos colores, escalas y variantes.
- Belén la revisa.
- Checklist **Siempre** del flujo §9: enlace en `CLAUDE.md`, caso A/B/C anotado,
  librería generada desde `tokens.css`.
- Si después cambia un token, primero en `tokens.css` y luego en la librería,
  nunca al revés.
