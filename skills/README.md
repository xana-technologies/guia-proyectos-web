# Skills · plugin de Xana

Este repositorio es también un **plugin de Claude Code** llamado `xana`. Cada
skill es una tarea concreta que Claude carga sola cuando la tarea encaja con su
descripción (o que se llama con `/xana:<nombre>`).

## Reglas

- **Una skill = una tarea** que se pide en una frase y acaba en algo que se
  puede ver. No un flujo entero.
- **Capa fina**: dice cuándo se usa, el orden, qué comprobar y dónde parar, y
  enlaza al flujo y a las plantillas. El detalle sigue en un solo sitio: la guía.
- **Nombre por la tarea**, sin nombres de personas. Se muestra siempre con el
  prefijo `xana:`, que pone el plugin.
- **Se escribe cuando su flujo tiene contenido real.** Si cambia un flujo, se
  revisan sus skills.
- Estructura: `skills/<nombre>/SKILL.md`, con `name` igual al nombre de la carpeta.
- Las normas que valen siempre no son skills: van en el `CLAUDE.md` o en la
  plantilla base.

## Instalar

Una vez por equipo, desde un terminal:

```bash
claude plugin marketplace add C:\Proyectos\guia-proyectos-web
claude plugin install xana@xana
```

## Lista

Estado: ✅ hecha · ⏳ pendiente de escribir.

### Arranque y contenido

| Skill | Qué entrega | Flujo | Estado |
| --- | --- | --- | --- |
| `xana:analizar-briefing` | Tipo de web, módulos, escenarios y web antigua | [arranque](../flujos/arranque-proyecto.md) | ✅ |
| `xana:preparar-proyecto` | Repo, carpetas, `CLAUDE.md`, `CHECKLIST.md` y Vercel | [arranque](../flujos/arranque-proyecto.md) | ✅ |
| `xana:kit-cliente` | Lo que hay que pedir al cliente | [arranque](../flujos/arranque-proyecto.md) | ✅ |
| `xana:investigar-sector` | Competencia y conclusiones | [investigación](../flujos/investigacion.md) | ✅ |
| `xana:proponer-estructura` | Mapa de páginas y secciones | [estructura](../flujos/estructura.md) | ✅ |
| `xana:palabras-clave` | Petición de palabras clave, títulos y descripciones | [contenidos y SEO](../flujos/contenidos-seo.md) | ✅ |
| `xana:escribir-textos` | Textos con el tono, `[[ ]]` donde falta, contenido como datos | [contenidos y SEO](../flujos/contenidos-seo.md) | ✅ |

### Marca y diseño

| Skill | Qué entrega | Flujo | Estado |
| --- | --- | --- | --- |
| `xana:traducir-manual-marca` | Decisiones de web a partir del manual del cliente | [brand board](../flujos/brand-board.md) | ✅ |
| `xana:crear-manual-marca` | Página web con 3 propuestas de marca para elegir | [brand board](../flujos/brand-board.md) | ✅ |
| `xana:crear-brand-board` | Documento visual de la marca elegida | [brand board](../flujos/brand-board.md) | ✅ |
| `xana:ordenar-referencias` | `referencias.md`: qué se toma de cada referencia y qué no | [referencias visuales](../flujos/referencias-visuales.md) | ✅ |
| `xana:crear-tokens` | Paleta, escalas y tokens semánticos | [design system](../flujos/design-system.md) | ✅ |
| `xana:componentes-base` | Componentes base, moldes y styleguide | [design system](../flujos/design-system.md) | ✅ |
| `xana:figma-a-seccion` | Sección con los valores exactos del nodo de Figma | [diseño en Figma](../flujos/diseno-figma.md) | ✅ |

### Imágenes y vídeo

| Skill | Qué entrega | Flujo | Estado |
| --- | --- | --- | --- |
| `xana:busqueda-imagenes` | Por cada imagen que falta: palabras clave para Magnific o descripción para IA | [imágenes](../flujos/imagenes.md) | ✅ |
| `xana:preparar-imagenes` | Revisar, renombrar, inventario, optimizar y colocar | [imágenes](../flujos/imagenes.md) | ✅ |
| `xana:rrss-favicon` | Imagen para redes 1200×630 y favicons | [imágenes](../flujos/imagenes.md) | ✅ |
| `xana:montar-video` | R2, póster y arranque en pantalla | [vídeos](../flujos/videos.md) | ✅ |

### Construcción

| Skill | Qué entrega | Flujo | Estado |
| --- | --- | --- | --- |
| `xana:maquetar-seccion` | Sección con su molde, móvil, sin JS y accesible | [maquetación](../flujos/maquetacion.md) | ✅ |
| `xana:header` | Cabecera y navegación | [maquetación](../flujos/maquetacion.md) | ✅ |
| `xana:footer` | Footer con los 4 elementos legales y «Preferencias de privacidad» | [maquetación](../flujos/maquetacion.md) | ✅ |
| `xana:paginas-404-gracias` | Página de error y página de gracias | [maquetación](../flujos/maquetacion.md) | ✅ |
| `xana:animar` | Animaciones elegidas entre opciones del catálogo | [maquetación](../flujos/maquetacion.md) · [movimiento avanzado](../modulos/movimiento-avanzado.md) | ⏳ |
| `xana:montar-formulario` | Formulario montado y probado en la URL temporal | [formulario](../flujos/formulario.md) | ✅ |
| `xana:diagnosticar-formulario` | Causa y arreglo de un formulario que no va | [formulario](../flujos/formulario.md) | ✅ |
| `xana:montar-legales` | Las tres páginas legales con datos del cliente y de la web | [legales](../flujos/legales.md) | ✅ |
| `xana:montar-cookies-analitica` | Muro de cookies, GTM con consentimiento y tabla de cookies | [cookies y analítica](../flujos/cookies-analitica.md) | ✅ |
| `xana:montar-seo-tecnico` | Metadatos, OG, sitemap, robots y JSON-LD | [SEO técnico](../flujos/seo-tecnico.md) | ✅ |

### Cierre

| Skill | Qué entrega | Flujo | Estado |
| --- | --- | --- | --- |
| `xana:revision-final` | Recorrido por las checklists de todos los flujos | [revisión](../flujos/revision.md) | ✅ |
| `xana:publicar` | DNS, Search Console y pruebas reales. **Solo si la llama Belén** (`disable-model-invocation: true`) | [publicación](../flujos/publicacion.md) | ✅ |
| `xana:entrega` | Documento de entrega para el cliente | [entrega y mantenimiento](../flujos/entrega-mantenimiento.md) | ✅ |

## Pendiente de decidir

- `xana:ordenar-referencias`: si se mantiene y con qué nombre.
- Catálogo de tipos de animación, para que `xana:animar` ofrezca opciones.
