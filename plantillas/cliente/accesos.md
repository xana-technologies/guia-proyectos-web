# Accesos que necesitamos

*Plantilla para el cliente. Se envía en el arranque.*

---

Para publicar la web y medir las visitas necesitamos acceso a algunos
servicios. **Nunca nos envíes contraseñas por correo**: en todos los casos se
puede dar acceso como usuario invitado, o bien nos indicas quién lo gestiona
y nos ponemos en contacto.

## 1. Dominio

- ¿Cuál es el dominio de la web? (p. ej. empresa.com)
- ¿Dónde está registrado? (DonDominio, Arsys, GoDaddy…)
- ¿Quién lo gestiona? Nombre y correo de la persona o empresa:
- ¿El correo electrónico de la empresa usa ese mismo dominio? (Sí / No)
- ¿Prefieres la web **con** o **sin** `www`? (si no lo sabes, lo decidimos nosotros)

## 2. Google Search Console

- ¿Tenéis cuenta? (Sí / No / No lo sé)
- Si la tenéis: añade a **{correo de Xana}** como usuario con permiso completo.

## 3. Google Tag Manager y Google Analytics

- ¿Tenéis cuenta? (Sí / No / No lo sé)
- Si la tenéis: añade a **{correo de Xana}** como usuario (Tag Manager:
  publicar · Analytics: editor).
- Si no la tenéis, la creamos nosotros.

---

*Para Claude — qué hacer con las respuestas:*
- Dominio y gestor → `CLAUDE.md` del proyecto; si lo gestiona un tercero, en la
  publicación se le envían las [instrucciones de DNS](instrucciones-dns.md).
- Correo con el mismo dominio → en el cambio de DNS no tocar los MX.
- Con o sin `www` → `site` en `astro.config.mjs` y `allowedDomains`.
- Cuentas de Google existentes → reutilizar contenedor y propiedad; revisar
  qué etiquetas tiene el contenedor antes de conectarlo
  ([cookies y analítica](../../flujos/cookies-analitica.md)).
- Si el cliente envía contraseñas, no guardarlas en el repositorio ni en la guía.
