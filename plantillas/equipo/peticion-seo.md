# Petición de SEO · para Eva

*Se envía en la fase [03 · Contenidos y SEO](../../fases/03-contenidos-seo.md),
cuando la estructura está aprobada. Eva hace el SEO; aquí solo se dice qué
necesitamos recibir.*

---

**Asunto:** SEO · {proyecto}

Hola Eva, la estructura de **{cliente}** ({dominio}) está cerrada. Te paso lo
que tenemos y lo que necesitamos.

## Lo que te pasamos

- Brief: {enlace o resumen}
- Estructura aprobada (páginas y secciones con su objetivo): {enlace}
- Idiomas: {idiomas}
- Competencia analizada: {enlace a `docs/01-investigacion/`}
- Web antigua, si la hay: {URL}

## Lo que necesitamos de ti

Por **cada página** (en una one-page, por la página y por sección principal)
y **por idioma**:

| Campo | Qué es | Límite orientativo |
| --- | --- | --- |
| Palabra clave principal | La búsqueda a la que responde la página | 1 |
| Palabras clave secundarias | Variantes y búsquedas relacionadas | 3–5 |
| Título SEO | El que sale en Google y en la pestaña | ~60 caracteres |
| Descripción SEO | El texto bajo el título en Google | ~155 caracteres |
| H1 propuesto | Titular principal de la página | — |
| Notas para el copy | Temas o preguntas que el texto debería cubrir | — |

Y para la web en general:
- Nombre del sitio (`og:site_name`) y descripción general.
- URLs propuestas si es multipágina.
- Si hay web antigua: qué URLs importan y adónde deben redirigir.

**Formato:** una tabla (hoja de cálculo o documento) con una fila por página
e idioma.
**Plazo:** {fecha}, para escribir los textos sobre esto.

Gracias.

---

*Para Claude — qué hacer con lo que llegue:*
- Guardarlo en `docs/03-contenidos/seo-metadatos.md` (tabla por página e idioma).
- Palabras clave → escribir o adaptar el copy; H1 → titular de la página.
- Título y descripción → props de `Base.astro` por página → [SEO técnico](../../flujos/seo-tecnico.md).
- URLs y redirecciones → estructura y [migración](../../modulos/migracion.md).
- Si falta algo, preguntar a Eva; no inventar palabras clave.
