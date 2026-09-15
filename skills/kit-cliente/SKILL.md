---
name: kit-cliente
description: Preparar el kit de arranque de una web de Xana — copiar al proyecto las plantillas que se envían al cliente (cuestionario, accesos, material, formulario, datos legales) y la petición de alta de formulario a ingeniería, pre-rellenadas solo con lo que ya se sabe. Usar cuando haya que pedir al cliente lo necesario para empezar.
---

# Kit para el cliente

Todo el detalle está en el flujo [`flujos/arranque-proyecto.md`](../../flujos/arranque-proyecto.md)
§4.2 y §5 (las rutas de esta skill son relativas a su carpeta).

**Claude prepara, Belén revisa y envía.** Claude nunca manda nada al cliente ni
a ingeniería.

## 1. Antes de empezar

- Lee `docs/00-arranque/brief.md` (sale de `xana:analizar-briefing`). Si no
  existe, analiza antes el briefing.
- Mira qué ha llegado ya del cliente: no se le pide dos veces lo que ha dado.

## 2. Qué se copia

Tabla «Kit de arranque» del flujo §5:

| Plantilla | Se copia a |
| --- | --- |
| [`cliente/arranque.md`](../../plantillas/cliente/arranque.md) | `docs/00-arranque/cuestionario-arranque.md` |
| [`cliente/accesos.md`](../../plantillas/cliente/accesos.md) | `docs/00-arranque/accesos.md` |
| [`cliente/material.md`](../../plantillas/cliente/material.md) | `docs/00-arranque/material.md` |
| [`cliente/formulario.md`](../../plantillas/cliente/formulario.md) | `docs/00-arranque/formulario.md` |
| [`cliente/datos-legales.md`](../../plantillas/cliente/datos-legales.md) | `docs/03-contenidos/legal/datos-legales.md` |
| [`equipo/alta-formulario.md`](../../plantillas/equipo/alta-formulario.md) | `docs/00-arranque/alta-formulario.md` (para ingeniería) |

## 3. Pasos

1. Copiar cada plantilla y poner el nombre del cliente, el proyecto y el dominio.
2. **Pre-rellenar solo lo que ya se sabe** (briefing, web antigua), marcado
   «¿sigue vigente?». Nada plausible inventado: un NIF o un domicilio sacados
   de un registro público se proponen para confirmar, no se dan por buenos.
3. Quitar las preguntas que no tocan por los módulos del proyecto (por ejemplo,
   idiomas si la web va en uno) y dejar las que sí.
4. Nunca escribir en estos ficheros claves, contraseñas ni la IP de la oficina.
5. Apuntar en `CHECKLIST.md` qué se ha preparado y que queda pendiente de envío.

## 4. Al terminar

- Dile a Belén qué documentos están listos y qué has pre-rellenado, para que
  lo revise y envíe **las cinco del cliente juntas** y el alta a ingeniería.
- Cuando vuelvan las respuestas, cada plantilla dice al final qué hacer con
  ellas («*Para Claude*»).
- Checklist **Kit** del flujo §8.
