# Inventario de imágenes · {{PROYECTO}}

<!-- NOTA PARA CLAUDE (se borra al copiar):
     - Se copia a `docs/imagenes/inventario.md` en el arranque (fase 00) y va en git.
     - Una fila por imagen que la web usa o va a usar, venga de donde venga.
     - Se empieza en 00 con lo que manda el cliente, se cierra en 03 (qué va en
       cada hueco y qué falta) y se actualiza en 07 al colocar.
     - Los binarios no van aquí: originales en `docs/imagenes/originales/`
       (excluida de git), másters en `src/assets/<seccion>/`.
     - Lo que falte y haya que buscar va a `busqueda-stock.md`.
     - Detalle del proceso: flujo de imágenes de la guía (`flujos/imagenes.md`).
     - Referencia de formato real: clays-of-the-world, `docs/05-imagenes/catalogo-stock.md`. -->

**Última revisión:** {{FECHA}}

Leyenda de **Origen**: `cliente` · `Magnific` (con su referencia) · `IA` (solo
si Belén lo ha indicado).
Leyenda de **Provisional**: `sí` = no se publica así; `no` = definitiva.

## Imágenes

| Archivo final | Origen | Referencia / original | Dónde se usa | Medida del máster | Provisional | Derechos y notas |
| --- | --- | --- | --- | --- | --- | --- |
| `seccion-descripcion-01.webp` | cliente | `nombre-con-el-que-llegó.jpg` | 01 Hero, fondo | 2880×1620 | no | |
| | Magnific | {{referencia o enlace}} | | | | Licencia descargada por Belén el {{fecha}} |
| | IA | {{herramienta y prompt en busqueda-stock.md}} | | | sí | Indicado por Belén el {{fecha}} |

## Huecos sin imagen

| Dónde va | Qué necesita | Se resuelve con | Estado |
| --- | --- | --- | --- |
| {{sección}} | {{qué tiene que enseñar}} | cliente · Magnific · IA | pedido · buscando · resuelto |

## Logos e iconos

| Archivo | Formato | De dónde sale | Notas |
| --- | --- | --- | --- |
| `brand/logo/logo-horizontal.svg` | SVG | cliente | Si solo llega PNG, se vuelve a pedir el SVG |

## Pendiente con el cliente

- [ ] {{p. ej. confirmar si las fotos son de sus instalaciones}}
- [ ] {{p. ej. pedir originales en alta de las que se sirven por debajo de ×2}}

## Reglas que se aplican a esta tabla

- Una imagen de stock **no se presenta como del cliente**: ni en el `alt` ni en
  los pies (clays-of-the-world, `CLAUDE.md` § Cuestiones abiertas).
- **Nada con `Provisional: sí` se publica.**
- Una misma foto no se repite en dos secciones.
- Si una foto del cliente parece generada o de stock, se anota aquí y se
  pregunta; no se da por hecho (clays-of-the-world, `catalogo-stock.md`).
