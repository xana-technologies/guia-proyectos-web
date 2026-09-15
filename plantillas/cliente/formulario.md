# Datos para el formulario de contacto

*Plantilla para el cliente. Se envía en el arranque. El formato da igual
(Word, correo, formulario): lo importante son las preguntas.*

---

Tu web tendrá un formulario de contacto. Cada vez que alguien lo rellene, os
llegará un correo con sus datos y podréis contestarle directamente desde ese
correo. Necesitamos saber:

**1. ¿A qué dirección o direcciones tienen que llegar los mensajes?**
Puede ser más de una. *Ejemplo: info@empresa.com, comercial@empresa.com*

**2. ¿Queréis que alguien más reciba copia?** *(opcional)*
- Copia visible:
- Copia oculta:

**3. ¿Qué datos queréis pedir a quien escribe?**
Por defecto pedimos: nombre, email, teléfono (opcional) y mensaje, más la
aceptación de la política de privacidad. Si necesitáis algo más (empresa,
país, producto de interés…), indicadlo:

**4. ¿Asunto del correo que os llega?** *(opcional)*
Si no decís nada, usaremos «Nueva consulta desde la web de [marca]».

**5. Si la web está en varios idiomas**, ¿los mensajes de todos los idiomas
van a la misma dirección?

---

**Qué pasa después:** mientras construimos la web, las pruebas llegan a
nuestras direcciones. Antes de publicar pondremos las vuestras, haremos un
envío real y os pediremos que confirméis que os ha llegado (revisad también
la carpeta de spam).

---

*Para Claude — qué hacer con las respuestas:*
- 1 y 2 → `FORM_TO`, `FORM_CC`, `FORM_BCC` (en Vercel, entornos Production y Preview).
- 3 → campos extra: tocar componente, endpoint y plantilla del correo; revisar privacidad.
- 4 → `FORM_SUBJECT`.
- 5 → si hay direcciones distintas por idioma, es [formulario avanzado](../../modulos/formulario-avanzado.md).
- Sin respuesta a 1 → no se publica.
