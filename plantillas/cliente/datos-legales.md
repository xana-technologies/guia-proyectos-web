# Datos para los textos legales de la web

<!-- NOTA PARA CLAUDE (se borra antes de enviar):
     - Fase 00. Solo capa 2 de los legales. Lo demás lo deduce Claude.
     - Se copia a `docs/03-contenidos/legal/datos-legales.md` del proyecto,
       se cambia {{NOMBRE_DEL_CLIENTE}} y se pre-rellena la columna del dato
       con lo que ya se sepa (web antigua, briefing) marcado «(de vuestra web
       actual: ¿sigue vigente?)». Nunca con datos supuestos.
     - Belén lo revisa y lo envía; el formato (Word, PDF, formulario) da igual.
     - Cada campo corresponde a una variable de plantillas/legales/variables.md
       (entre corchetes en el título). Se vuelca en `src/data/legal/entity.ts`. -->

**Para:** {{NOMBRE_DEL_CLIENTE}} · **Preparado por:** Xana

Toda web necesita tres páginas legales: **aviso legal**, **política de
privacidad** y **política de cookies**. Los textos ya los tenemos redactados;
lo que no podemos poner nosotros son los datos de vuestra empresa.

**Cómo rellenarlo.** Escribid cada dato en su hueco y devolvednos el documento.
No hace falta redactar nada.

**Si no sabéis algún dato**, dejadlo en blanco y seguid: en cada campo os
decimos dónde suele estar o a quién preguntarlo. Mientras falte, en la web de
pruebas aparece una marca `[[ ]]` en su lugar y **esa página no se publica**.
No rellenamos ningún dato por nuestra cuenta: un NIF o un domicilio
aproximados en un aviso legal serían una declaración falsa.

---

## 1. Quién es el titular de la web

Estos datos se publican en el aviso legal y en la política de privacidad, a la
vista de cualquiera.

### 1.1 Razón social `[COMPANY_LEGAL_NAME]`

**Qué es.** El nombre legal de la empresa, con su forma jurídica (S.L., S.A.…).
No es la marca: si la web se llama «Cerámica Sol» pero la empresa es
«Industrias Sol 2020, S.L.», aquí va la segunda. Si sois autónomos, vuestro
nombre y apellidos.

**Ejemplo.** Industrias Sol 2020, S.L.

**Si no lo sabéis.** Está en las facturas y en la escritura de la empresa; lo
sabe vuestra gestoría.

**Vuestro dato:**

### 1.2 Nombre comercial o marca `[COMPANY_TRADE_NAME]`

**Qué es.** El nombre con el que os conoce la gente y que aparece en la web, si
es distinto de la razón social.

**Ejemplo.** Cerámica Sol

**Si no lo sabéis.** Si es el mismo que la razón social, dejadlo en blanco.

**Vuestro dato:**

### 1.3 NIF / CIF `[COMPANY_TAX_ID]`

**Qué es.** El número de identificación fiscal de la empresa de 1.1.

**Ejemplo.** B + 8 cifras en una sociedad española (B00000000).

**Si no lo sabéis.** Aparece en cualquier factura que emitáis.

**Vuestro dato:**

### 1.4 Domicilio social `[COMPANY_REGISTERED_ADDRESS]`

**Qué es.** La dirección oficial de la empresa, completa y con código postal.
Puede no ser la de la oficina o la tienda que se enseña en la web.

**Ejemplo.** C/ Mayor, 1, 2.º, 12001 Castellón de la Plana (Castellón)

**Si no lo sabéis.** Está en la escritura y en el Registro Mercantil; vuestra
gestoría lo tiene. Si os habéis mudado y no lo habéis cambiado en el registro,
decídnoslo: el que vale es el que figura oficialmente.

**Vuestro dato:**

### 1.5 País `[COMPANY_COUNTRY]`

**Qué es.** El país donde está registrada la empresa. Decide qué leyes y qué
autoridad de protección de datos aparecen en los textos.

**Ejemplo.** España

**Vuestro dato:**

### 1.6 Datos registrales `[COMPANY_REGISTRY_DETAILS]`

**Qué es.** Dónde está inscrita la empresa: registro, tomo, folio, hoja e
inscripción.

**Ejemplo.** Registro Mercantil de Castellón, tomo 0000, folio 00, hoja CS-00000,
inscripción 1.ª

**Si no lo sabéis.** Está en la escritura de constitución o lo tiene la
gestoría. Si sois autónomos, no tenéis: escribid «autónomo».

**Vuestro dato:**

### 1.7 Actividad `[BUSINESS_ACTIVITY]`

**Qué es.** A qué se dedica la empresa, en una o dos líneas. Si vuestra
actividad necesita una licencia, una autorización o estar inscritos en un
registro profesional o de un organismo supervisor, indicadlo con su número.

**Ejemplo.** Fabricación y venta de baldosas cerámicas. · *Con registro:*
Asesoramiento financiero; inscrita en el registro de [organismo] con el n.º
[número].

**Si no lo sabéis.** Vale la descripción de la escritura o del alta en
Hacienda. Si no sabéis si necesitáis registro, decídnoslo y lo vemos.

**Vuestro dato:**

## 2. Cómo os pueden contactar

### 2.1 Email de contacto `[COMPANY_EMAIL]`

**Qué es.** El email que se publica en la web para que os escriban.

**Ejemplo.** info@ceramicasol.es

**Vuestro dato:**

### 2.2 Email para privacidad `[PRIVACY_EMAIL]`

**Qué es.** Donde cualquier persona puede escribir para pedir qué datos suyos
tenéis, corregirlos o borrarlos. Tiene que leerlo alguien.

**Si no tenéis uno distinto**, marcad la casilla y usamos el de 2.1.

- [ ] Sirve el mismo email de contacto
- Otro email:

### 2.3 Email para asuntos legales `[LEGAL_CONTACT_EMAIL]`

**Qué es.** El email que aparece al final del aviso legal para consultas sobre
la web.

- [ ] Sirve el mismo email de contacto
- Otro email:

### 2.4 Teléfono `[COMPANY_PHONE]`

**Qué es.** Opcional: con un email es suficiente. Si lo ponéis, se publica.

**Ejemplo.** +34 964 00 00 00

**Vuestro dato:**

## 3. Protección de datos

### 3.1 ¿Tenéis delegado de protección de datos (DPO)? `[DPO_CONTACT]`

**Qué es.** Una persona o empresa designada para supervisar cómo tratáis los
datos personales. Solo algunas empresas están obligadas a tenerlo; si lo
tenéis, su contacto tiene que aparecer en la política de privacidad.

**Si no lo sabéis.** Lo más probable es que no, pero preguntadlo a vuestra
gestoría o asesoría: si lo tenéis, lo saben.

- [ ] No
- [ ] Sí → nombre y email de contacto:

### 3.2 ¿Tenéis una página propia para ejercer derechos de privacidad? `[RIGHTS_REQUEST_URL]`

**Qué es.** Algunas empresas tienen un formulario propio para estas
solicitudes. Casi nunca es el caso; si no, se atienden por el email de 2.2.

- [ ] No
- [ ] Sí → dirección de la página:

## 4. Revisión jurídica

### 4.1 ¿Una asesoría o un abogado revisará los textos?

**Por qué lo preguntamos.** Nosotros preparamos los textos a partir de una base
estándar y de lo que la web hace realmente, pero **no firmamos la validación
legal**. Si vuestra asesoría los revisa, su criterio manda sobre el nuestro.

- [ ] No
- [ ] Sí → nombre y email de contacto:

### 4.2 ¿Vuestra asesoría quiere mantener los textos legales que tenéis ahora?

**Por qué lo preguntamos.** Si ya tenéis textos redactados por un abogado, lo
tenemos en cuenta. Aun así los adaptamos a lo que hace la web nueva: una
política que habla de herramientas que la web no usa no sirve.

- [ ] No tenemos textos o no hace falta mantenerlos
- [ ] Sí → dónde están (enlace o documento):

---

## Lo que ya hacemos nosotros

No tenéis que decirnos nada de esto: sale de cómo está hecha la web.

- La redacción de las tres páginas, en todos los idiomas de la web.
- Qué datos recoge el formulario de contacto, qué herramientas los procesan y
  cuánto se guardan.
- Qué cookies pone la web, el aviso de cookies y el enlace para cambiar la
  elección.
- La legislación y la autoridad de protección de datos según el país, y las
  fechas.

Si la web va a hacer algo más —enviar una newsletter, mostrar vídeos o mapas de
otras plataformas, añadir otras herramientas de medición o publicidad—,
decídnoslo antes: cambia los textos y el aviso de cookies.

## Qué pasa cuando lo devolváis

1. Cargamos los datos en la web, en un solo sitio.
2. Desaparecen las marcas `[[ ]]` de las tres páginas.
3. Si hay revisión jurídica, os enviamos los textos para vuestra asesoría.
4. Con todo completo (y, si la hay, con el visto bueno), las páginas quedan
   listas para publicar.
