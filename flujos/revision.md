# Flujo · Revisión antes de publicar

Control de calidad de la web terminada, **en la URL temporal y antes de
conectar el dominio**. No se construye nada: se comprueba y se pasa la lista
de fallos. Corresponde al «Antes de publicar» del *Protocolo de publicación
ESTÁNDAR*.

## 1. Cuándo se usa

- Fase [08](../fases/08-revision.md): cuando la construcción está completa y
  el cliente ha revisado los contenidos.
- De nuevo, en pequeño, después de cualquier cambio grande en una web ya
  publicada (sección nueva, módulo nuevo, etiqueta nueva en GTM).

## 2. Qué tenemos

| Situación | Qué cambia |
| --- | --- |
| Web de una página | Se revisa la página y las legales, gracias y 404 |
| Multipágina | Se revisa cada página indexable con su metadato |
| Idiomas | Cada punto, por idioma |
| Queda algún `[[ ]]` o `REVISAR` abierto | No se publica: se pasa la lista a Belén para reclamarlo |

## 3. Pasos y quién

1. **Claude** hace `npm run build` + `npm run preview` (no se juzga peso ni
   velocidad en `dev`) y recorre la checklist del §5 sobre el build y sobre la
   URL temporal.
2. **Claude** entrega el informe (§4) a Belén: qué falla, dónde, gravedad y
   propuesta. **No arregla nada sin permiso** si el arreglo cambia diseño o
   contenido; los fallos técnicos evidentes se proponen en bloque.
3. **Belén** valida y decide qué se arregla antes de publicar.
4. Tras los arreglos, **Claude** repite solo los puntos afectados.
5. **Cliente**: visto bueno final sobre la URL temporal.

## 4. Formato del informe

```markdown
# Revisión antes de publicar · {proyecto} · {fecha}

URL revisada: {url temporal} · Build: {commit}

## Bloquea la publicación
- [ ] {qué} — {dónde} — {propuesta}

## Conviene arreglar
- [ ] …

## Anotado (no bloquea)
- …

## Pendiente del cliente
- [ ] {dato que falta} ({dónde se usa})
```

Bloquea: datos legales incompletos, cookies antes de aceptar, formulario que
no envía, `noindex` en páginas que deben indexarse, textos o imágenes
provisionales, enlaces rotos, errores en consola que rompen algo.

## 5. Checklist

### SEO
- [ ] Cada página indexable con título y descripción propios (de `seo-metadatos.md`)
- [ ] Imagen para redes 1200×630 con URL absoluta; se ve bien al compartir
- [ ] Favicon y `apple-touch-icon`
- [ ] Una sola H1 por página; H2 en orden
- [ ] Canonical correcto; `site` con el dominio definitivo
- [ ] `noindex` solo en legales (`noindex, follow`), gracias, 404, styleguide y pruebas
- [ ] `sitemap.xml` solo con indexables; `robots.txt` sin `Disallow` que tape un `noindex`
- [ ] JSON-LD válido y sin datos inventados
- [ ] URLs coherentes con la estructura; 404 funciona
- [ ] Redirecciones de la web antigua hechas (si hay)
- Detalle → [SEO técnico](seo-tecnico.md)

### Legales y cookies
- [ ] Los 4 elementos en el pie de todas las páginas: aviso legal, privacidad, cookies, preferencias de privacidad
- [ ] Ningún `[[ ]]` ni `REVISAR` en los textos
- [ ] Proveedores declarados = los que usa la web (Vercel, Brevo, Cloudflare, Google y los de módulos)
- [ ] En ventana privada: antes de decidir no hay `_ga`; tras rechazar, tampoco; tras aceptar, `_ga` y `_ga_<ID>`
- [ ] Tabla de cookies = cookies reales; duración de `cc_cookie` = la declarada
- [ ] Sin embeds de terceros cargados sin consentimiento
- Detalle → [legales](legales.md) · [cookies y analítica](cookies-analitica.md)

### Contenido
- [ ] Ningún `[[ ]]`, texto provisional ni `placeholder` visible
- [ ] Ninguna imagen marcada provisional en el inventario
- [ ] Ningún texto da a entender que una foto de stock es del cliente
- [ ] Cifras y datos confirmados por el cliente
- [ ] Ortografía y nombres propios (marca, razón social) revisados
- [ ] Páginas de prueba y laboratorio fuera del build publicado

### Funcionamiento
- [ ] Formulario en la URL temporal: envío correcto → `/gracias`; errores de validación; sin JavaScript; 429
- [ ] Todos los enlaces internos y externos funcionan; los externos abren donde deben
- [ ] Menú móvil, anclas y selector de idioma
- [ ] Vídeos cargan con póster; se paran fuera de pantalla
- [ ] Sin errores en la consola del navegador
- [ ] La web se lee sin JavaScript (nada oculto)
- Detalle → [formulario](formulario.md)

### Móvil
- [ ] 375 px, 768 px y escritorio: sin scroll horizontal, textos que no se cortan, botones que caben
- [ ] Imágenes con el recorte correcto en móvil

### Rendimiento
- [ ] Peso de `dist/` razonable; sin PNG enormes generados
- [ ] Primera imagen visible con `eager` + `fetchpriority`; el resto `lazy`
- [ ] Fuentes alojadas en la web
- [ ] Vídeos en R2, no en el repo
- Detalle → [imágenes](imagenes.md) · [vídeos](videos.md)

### Accesibilidad
- [ ] Contraste AA en texto (ratios anotados en tokens)
- [ ] Navegación completa con teclado; foco visible
- [ ] Skip-link; `alt` en imágenes con contenido, `alt=""` en decorativas
- [ ] Formularios con etiquetas visibles y errores anunciados
- [ ] Con «reducir movimiento» activado no hay animaciones de desplazamiento
- Detalle → [maquetación](maquetacion.md)

### Cierre
- [ ] Informe entregado a Belén
- [ ] Arreglos hechos y comprobados
- [ ] Visto bueno del cliente sobre la URL temporal
- → [publicación](publicacion.md)

## 6. Reglas y trampas

- Juzgar peso y velocidad con `build` + `preview`, nunca en `dev`
  ([trampas](../trampas-y-lecciones.md#astro-y-build)).
- Las cookies se comprueban en el navegador, no leyendo el código: lo que
  dispara GTM se decide en GTM.
- `getComputedStyle` sirve para geometría; para colores en el preview puede
  dar valores obsoletos ([trampas](../trampas-y-lecciones.md#verificación-y-herramientas)).
- «Funciona» solo se dice después de comprobarlo; si algo no se ha podido
  comprobar, se dice.
- Lo que falla y no se arregla antes de publicar se apunta en el
  `CHECKLIST.md` del proyecto.
