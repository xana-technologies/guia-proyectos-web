# Flujo · Entrega y mantenimiento

Cerrar el proyecto: qué recibe el cliente, qué se registra internamente y qué
hay que vigilar después. Es la «Documentación interna» del *Protocolo de
publicación ESTÁNDAR* más la entrega y el mantenimiento técnico.

## 1. Cuándo se usa

- Fase [10](../fases/10-entrega-mantenimiento.md), cuando la
  [publicación](publicacion.md) está comprobada (dominio, formulario real,
  datos en Analytics).
- El mantenimiento técnico, después, de forma periódica o cuando hay un aviso.

## 2. Qué tenemos

| Situación | Qué cambia |
| --- | --- |
| Hay contrato de mantenimiento | Se anota en Factura Directa y se planifican revisiones |
| No hay mantenimiento | La entrega deja claro cómo pedir cambios y qué pasa si algo se rompe |
| Quedan pendientes tras publicar | Se apuntan en `CHECKLIST.md` con responsable |

## 3. Pasos y quién

### Entrega al cliente
1. **Claude** prepara el documento de [entrega](../plantillas/cliente/entrega.md)
   con los datos del proyecto (`CLAUDE.md`, `CHECKLIST.md`).
2. **Belén** completa las condiciones comerciales (cambios, mantenimiento) y lo envía.

### Documentación interna (protocolo)
- [ ] **Tabla Clientes Xana**: añadir el proyecto
- [ ] **Factura Directa**: indicar si hay hito de facturación o inicio de mantenimiento con la publicación
- [ ] **Carpeta Drive**: ordenar y anotar lo pendiente

Lo hace **Belén**.

### Cierre del proyecto en el repo
1. **Claude** cierra el `CHECKLIST.md`: lo hecho, lo pendiente con responsable
   y fecha, y lo que depende del cliente.
2. **Claude** actualiza el `CLAUDE.md` del proyecto: estado «publicado»,
   dominio, cuentas, decisiones que se tomaron por el camino.
3. **Claude** propone a Belén añadir a la guía lo aprendido que no esté
   (trampas, decisiones que valen para otros proyectos). No lo añade sin permiso.
4. Commit con permiso ([forma de trabajo](../forma-de-trabajo.md)).

## 4. Mantenimiento técnico

| Qué | Cuándo | Cómo |
| --- | --- | --- |
| Actualizar Astro y dependencias | Periódicamente o por aviso de seguridad | Respetar la pareja Astro ↔ `@astrojs/vercel` (8 ↔ 5, 10 ↔ 6, 11 ↔ 7); `build` + revisión rápida antes de publicar |
| Clave de Brevo | Si se filtra o al dar de baja | Ingeniería crea una nueva → `BREVO_API_KEY` en Vercel → despliegue → borrar la antigua ([alta de formulario](../plantillas/equipo/alta-formulario.md)) |
| Firewall y uso de Vercel | Tras publicar y ante avisos de Spend Management | Picos de *Denied* en `/api/contact`, invocaciones anómalas |
| Brevo | Si el volumen sube sin tráfico real | Revisar Turnstile; la cuenta es compartida (300/día en el plan gratuito) |
| Search Console | Periódicamente | Errores de indexación, 404, redirecciones |
| Legales | Cada vez que cambia lo que hace la web o los datos de la empresa | Primero el texto, luego el cambio ([legales](legales.md)) |
| GTM | Antes de añadir cualquier etiqueta | Declararla en la política primero ([cookies y analítica](cookies-analitica.md)) |

## 5. Reglas y trampas

- Una web publicada no cambia hasta el paso manual de Belén en GitHub; los
  cambios de mantenimiento siguen el mismo flujo.
- Después de publicar, los cambios grandes pueden ir en una rama si se pide;
  por defecto, `main`.
- Si el cliente cambia la dirección que recibe el formulario, se cambia en
  Vercel (`FORM_TO`) y hace falta un despliegue.
- No prometer al cliente condiciones de mantenimiento que no haya fijado Belén.

## 6. Checklist

- [ ] Documento de entrega enviado
- [ ] Tabla Clientes Xana, Factura Directa y Drive
- [ ] `CHECKLIST.md` cerrado con pendientes y responsables
- [ ] `CLAUDE.md` del proyecto actualizado
- [ ] Lecciones propuestas para la guía
