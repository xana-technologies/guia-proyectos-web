---
name: seccion-a-figma
description: Llevar a Figma una sección o página ya construida de una web de Xana, montada con los componentes y variables de la librería, para que Belén la ajuste allí (caso B, código primero). Es la inversa de xana:figma-a-seccion. Usar cuando una sección construida no convence y hay que pasarla a Figma para retocarla.
---

# De una sección a Figma

Todo el detalle está en el flujo [`flujos/diseno-figma.md`](../../flujos/diseno-figma.md)
§2 (caso B), §3 «Escribir en Figma» y §4 «Caso B · Código primero» (las rutas de
esta skill son relativas a su carpeta). **Léelo antes de empezar.**

El camino completo del caso B es: sección construida → **esta skill** la lleva
a Figma → Belén la ajusta → `xana:figma-a-seccion` la rehace con los valores del
nodo.

## 1. Antes de empezar

| Qué | Dónde | Si falta |
| --- | --- | --- |
| Librería en Figma | Página de librería del fichero | Primero `xana:libreria-figma`: la sección se monta con sus componentes y variables |
| Fichero de Figma | `CLAUDE.md` del proyecto | Pedírselo a Belén y anotarlo |
| Qué sección y qué no convence | Belén | Preguntar: se lleva solo esa sección, no la página entera salvo que lo pida |
| La sección tal como está | URL temporal o `dev` | Revisar que está al día antes de pasarla |

En el caso B **Figma no bloquea**: la web sigue avanzando mientras se ajusta la sección.

## 2. Pasos

1. **Cargar las skills del plugin de Figma**: `figma-use` (obligatoria antes de
   cualquier `use_figma`) y `figma-generate-design`.
2. **Montar la sección en Figma** con los componentes y variables de la
   librería, no con valores sueltos ni capturas pegadas.
3. **Escritorio y móvil**, con los textos reales de `src/data/`.
4. Poner la sección en la página del fichero que diga Belén, con un nombre que
   la identifique (sección y fecha).
5. Pasar a Belén el enlace al nodo.

## 3. Al terminar

- Belén ajusta en Figma y devuelve el enlace al nodo.
- Con ese enlace, `xana:figma-a-seccion`: valores exactos, tokens existentes o
  propuestos, nodo citado y desvíos anotados.
- Anotar en `CHECKLIST.md` que la sección está en ajuste en Figma.
- La aprobación del cliente en el caso B es **sobre la URL temporal**, no sobre Figma.
