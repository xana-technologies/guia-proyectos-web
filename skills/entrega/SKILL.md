---
name: entrega
description: Cerrar un proyecto web de Xana tras publicarlo — preparar el documento de entrega para el cliente con los datos del proyecto, cerrar CHECKLIST.md con pendientes y responsables, actualizar CLAUDE.md a «publicado» y proponer a Belén lo aprendido para la guía. Usar cuando la publicación está comprobada y hay que entregar la web.
---

# Entrega

Todo el detalle está en el flujo [`flujos/entrega-mantenimiento.md`](../../flujos/entrega-mantenimiento.md)
(las rutas de esta skill son relativas a su carpeta). Plantilla:
[`plantillas/cliente/entrega.md`](../../plantillas/cliente/entrega.md).

## 1. Antes de empezar

| Qué | Si falta |
| --- | --- |
| Publicación comprobada: dominio, formulario real, datos en Analytics | Parar: se entrega lo que funciona |
| ¿Hay contrato de mantenimiento? | Preguntar a Belén. **Claude no promete condiciones** que no haya fijado ella |

## 2. Pasos

1. **Documento de entrega**: copiar la plantilla al proyecto y rellenarla con
   los datos de `CLAUDE.md` y `CHECKLIST.md` (dominio, qué incluye la web,
   cuentas y accesos del cliente, cómo pedir cambios). **Las condiciones
   comerciales y de mantenimiento las completa Belén.** Nunca escribir claves ni
   contraseñas en el documento.
2. **No es asesoramiento jurídico**: el documento no afirma que la web «cumple el
   RGPD» ([`flujos/legales.md`](../../flujos/legales.md) · Reglas).
3. **Cerrar `CHECKLIST.md`**: lo hecho, lo pendiente con responsable y fecha, y
   lo que depende del cliente.
4. **Actualizar `CLAUDE.md`**: estado «publicado», dominio, cuentas y decisiones
   tomadas por el camino.
5. **Recordar a Belén la documentación interna** que hace ella: Tabla Clientes
   Xana, Factura Directa y carpeta de Drive.
6. **Lecciones para la guía**: repasar el proyecto y proponer a Belén lo que no
   esté en la guía (trampas nuevas, decisiones que valen para otros proyectos),
   con su fuente. **No se añade sin permiso.**

## 3. Al terminar

- Belén revisa y envía el documento al cliente.
- Checklist del flujo §6.
- Propón el commit y espera el sí ([forma de trabajo §5](../../forma-de-trabajo.md)).
- El mantenimiento posterior (dependencias, claves, firewall, Search Console,
  legales, GTM) está en el flujo §4.
