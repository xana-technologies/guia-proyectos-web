# 06 · Diseño en Figma

**Qué es.** El espacio de diseño del proyecto, siempre sobre el design system.
Casi siempre existe, pero no se usa igual en todos los proyectos. No bloquea:
en el caso B va y viene con la construcción.

**Necesita antes.** 02 y 05.

## Casos

| Caso | Cuándo | Cómo se trabaja |
| --- | --- | --- |
| **A · Diseño previo** | El cliente trae Figma o quiere aprobar el aspecto antes | Se diseña → el cliente aprueba → Claude construye con los valores exactos |
| **B · Código primero, Figma para ajustar** *(lo habitual)* | Claude construye con marca, estructura y design system | Lo que no sale bien a la primera se ajusta en Figma y se rehace con los valores exactos |
| **C · Sin Figma** | Webs muy simples | Se itera sobre la web |

## Reglas fijas

- El enlace a Figma queda en el `CLAUDE.md` del proyecto.
- Cuando algo viene de Figma, Claude saca los valores exactos
  (`get_design_context`), no aproxima a ojo.
- Aprobación del cliente: en A, sobre el diseño; en B y C, sobre la URL temporal.

## Flujos

[Diseño en Figma](../flujos/diseno-figma.md)
