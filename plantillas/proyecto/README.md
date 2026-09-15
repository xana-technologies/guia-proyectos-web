# Plantillas · Proyecto

Lo que Claude copia a cada proyecto nuevo, fuera del código. Cómo y cuándo se
usan: [flujo de arranque](../../flujos/arranque-proyecto.md).

Cada plantilla lleva arriba una nota para Claude (en un comentario HTML o en
cursiva) que se borra al copiarla. Los huecos van entre `{{ }}`: lo que no se
sepa se deja «pendiente», nunca con un dato supuesto.

| Plantilla | Se copia a | Fase | Qué es |
| --- | --- | --- | --- |
| [`CLAUDE.md`](CLAUDE.md) | `CLAUDE.md` | 00 | Reglas y datos del proyecto: tipo, módulos, escenarios, cuentas, reglas de la base, git y publicación, tono, dónde está cada cosa, cuestiones abiertas |
| [`CHECKLIST.md`](CHECKLIST.md) | `CHECKLIST.md` | 00 | Estado real por fases 00–10, escenarios, bloque por módulo, pendientes con terceros |
| [`docs-estructura.md`](docs-estructura.md) | Carpetas de `docs/` y `brand/` | 00 | Árbol y qué va en cada carpeta |
| [`inventario-imagenes.md`](inventario-imagenes.md) | `docs/imagenes/inventario.md` | 00 · 03 · 07 | Cada imagen: origen, dónde se usa, provisional, derechos |
| [`busqueda-stock.md`](busqueda-stock.md) | `docs/imagenes/busqueda-stock.md` | 03 | Ficha por imagen que falta: palabras clave para Magnific o prompt de IA |
| [`referencias.md`](referencias.md) | `docs/05-referencias-visuales/referencias.md` | 05 | Referencias de Belén (o del cliente): qué se toma y qué no |

## Fuentes

clays-of-the-world (`CLAUDE.md`, `CHECKLIST.md`, `docs/README.md`,
`docs/01-referencias/inspiracion/referencias.md`,
`docs/05-imagenes/catalogo-stock.md`) · APC-gestion-patrimonial (`CLAUDE.md`).
