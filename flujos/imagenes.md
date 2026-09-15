# Flujo · Imágenes

Buscar, recibir, ordenar, optimizar y colocar las imágenes de la web.
**Se aplica siempre y a todas**, vengan del cliente, de Magnific o de IA: no
hay imagen que entre en la web sin pasar por el inventario y la optimización.

Vídeos → [vídeos](videos.md). Imagen para redes y favicons: se preparan aquí
(§3, paso 7) y se enlazan en el `<head>` según [SEO técnico](seo-tecnico.md).
Galería con visor → módulo [multimedia](../modulos/multimedia.md).

## 1. Cuándo se usa

| Fase | Qué se hace |
| --- | --- |
| [00 · Arranque](../fases/00-arranque.md) | Pedir el material ([plantilla](../plantillas/cliente/material.md)); crear `docs/imagenes/` y empezar el inventario con lo que llegue |
| [03 · Contenidos y SEO](../fases/03-contenidos-seo.md) | **Cierre del inventario**: qué imagen va en cada hueco, cuáles faltan; búsqueda en Magnific (o prompt de IA si Belén lo indica) |
| [07 · Construcción](../fases/07-construccion.md) | Selección, renombrado, optimización, colocación; imagen OG y favicons |
| [08 · Revisión](../fases/08-revision.md) | Ninguna provisional, `alt`, peso en el build, primera imagen `eager` |

## 2. Qué tenemos

| | Situación | Qué se hace |
| --- | --- | --- |
| 🟢 | El cliente manda todas | Se usan; igualmente se ordenan, renombran, inventarían y optimizan. Si alguna no llega a la resolución del hueco, **se piden los originales en alta**, no se amplía |
| 🟡 | Manda algunas | Las suyas primero; los huecos que quedan → búsqueda en Magnific (§3, paso 1) |
| 🔴 | No manda ninguna | Todo de stock → búsqueda en Magnific |
| IA | **Solo cuando Belén lo indica** | El paso 1 prepara la descripción para generarla en vez de palabras clave |

- **Stock, solo Magnific.** Claude prepara la búsqueda; **descarga siempre
  Belén** (la cuenta es suya). Claude no descarga de bancos de imágenes.
- Una foto del cliente no es «suya» hasta que lo confirma: en clays las doce
  primeras parecían generadas por IA porque coincidían con ficheros
  `IA - *.jpg` y resultaron reales; los `alt` y pies se escribieron sin darlas
  por instalaciones propias hasta la confirmación
  (`clays-of-the-world/docs/05-imagenes/catalogo-stock.md`).

## 3. Pasos y quién

### 1. Búsqueda — Claude (fase 03)

En `docs/imagenes/busqueda-stock.md`, **una ficha por imagen que falta**
(formato en §4). Cada ficha dice:

- **Dónde va**: página, sección y hueco (hero, banda, tarjeta…).
- **Qué transmite**: la idea, no solo el objeto.
- **Palabras clave para Magnific**, en inglés y en español, con variantes
  (sinónimos, plano, luz): Magnific busca mejor en inglés.
- **Formato y proporción** del hueco y **tamaño mínimo** del original, contando
  densidad ×2 (tabla de anchos en el paso 5).
- **Estilo** según la marca: luz, color, gente sí o no, encuadre.
- **Qué evitar**: lo que choca con la marca o con la regla de §6 (p. ej. rótulos
  legibles de otra empresa, uniformes, instalaciones reconocibles).
- **Nº de candidatas** que se quieren (normalmente 3).

Si Belén indica IA, la ficha lleva en lugar de las palabras clave una
**descripción para generar** (escena, encuadre, luz, proporción, estilo, qué no
debe aparecer) y la imagen se marca `IA` en el inventario.

**Belén** busca y descarga en Magnific y deja las candidatas en
`docs/imagenes/originales/`, anotando la referencia de cada una.

### 2. Entrada — Claude

**Todo lo que llega, sin tocar**, a `docs/imagenes/originales/` (cliente,
Magnific, IA; logos aparte a `brand/logo/`). Esa carpeta **está excluida de
git** y solo vive en el ordenador:

```gitignore
# .gitignore del proyecto
docs/imagenes/originales/
```

No se sube porque pesa mucho y no hace falta: el **inventario** permite
recuperarlo todo (Magnific por su referencia; las del cliente, pidiéndoselas
otra vez). En clays los originales estaban fuera del repo, en
`~/Xana/CLAYS OF THE WORLD/00.RECURSOS`, y el script los leía de ahí
(`clays-of-the-world/scripts/optimizar-imagenes.py`, constante `ORIGEN`).

### 3. Selección y orden — Claude propone, Belén decide

1. Claude **ve cada imagen** (no se asigna por el nombre del fichero) y
   propone dónde va cada una, con el porqué.
2. Renombra al colocarla: **`seccion-descripcion-01`**, en minúsculas, sin
   acentos, con guiones (`cantera-frente-excavadora`, `laboratorio-molino-muestras`
   en clays). El nombre describe lo que se ve, no de dónde vino (`IMG_0554.jpg`,
   `2148862892.jpg`).
3. Señala las que **no llegan** a la resolución del hueco y las que se parecen
   demasiado entre sí (en clays se quitó `cantera-terrazas-ladera` por
   parecerse a otras vistas de la cantera).

### 4. Inventario — Claude

`docs/imagenes/inventario.md`, **en git**. Una fila por imagen (formato en §4):
origen (`cliente`, `Magnific` + referencia, `IA`), fichero original, nombre
final, dónde se usa, si es **provisional** y derechos o autoría que haya que
citar. Se actualiza cada vez que entra, cambia de sitio o sale una imagen.

### 5. Optimización — Claude

Cada imagen se prepara **antes** de entrar en `src/assets/`. Lo que sale es
un **máster** WebP; las variantes que ve el visitante las genera Astro en el
build.

| Paso | Regla |
| --- | --- |
| **Recorte** | Al formato del hueco, si el hueco lo fija (tarjeta 4:5, banda 16:9…) |
| **Tamaño máximo** | Ancho del hueco ×2. Nunca se amplía (tabla abajo) |
| **Formato** | WebP con **escalera de calidades**: la más alta que pese menos que el original. Nunca un máster más pesado que el original |
| **Metadatos** | Fuera, **sobre todo el GPS** de las fotos de móvil |
| **Correcciones del cliente** | Orientación EXIF aplicada antes de quitar metadatos; CMYK → sRGB; fotos oscuras, ajuste suave de brillo solo si Belén lo aprueba |

Anchos objetivo, contando densidad ×2 (clays `optimizar-imagenes.py`):

| Ancho | Hueco |
| --- | --- |
| 2880 | Hero a sangre |
| 1920 | Sección ancha, banda separadora, pieza grande de galería, visor a pantalla completa |
| 1600 | Media columna, pieza mediana |
| 1200 | Tarjeta, textura, columna estrecha |

**Por qué la escalera.** Un original ya muy comprimido, recodificado a calidad
alta, conserva sus artefactos y los paga en bytes: en clays
`portada-cantera-aerea` a q90 pesaba más que el JPEG de partida y se bajó a
q82 (`CALIDAD_POR_IMAGEN`). Magna lo resuelve de forma general probando
calidades de mayor a menor hasta que el WebP pesa menos que el JPG.

**Por qué no se amplía.** Ampliar (Lanczos + enfoque) no añade detalle, solo
disimula. Lo que lo arregla es el original en alta: en clays las fotos del
cliente van «a tamaño nativo» (`None` en el plan) y la falta de resolución se
anotó como pendiente con el cliente.

#### Scripts que ya existen

No hay todavía un script común: se copia el más cercano y se adapta. Ninguno
hace todo lo de la tabla; lo que falta se añade al copiarlo.

**Decisión (15-09-2026): los scripts son en Node + `sharp`**, que ya viene con
el proyecto. Los de clays en Python sirven como referencia de parámetros hasta
que el repo plantilla los traiga portados
([plantillas de código](../plantillas/codigo/README.md#decisiones-15-09-2026)).

| Script | Qué hace | Parámetros reales | Qué le falta |
| --- | --- | --- | --- |
| `clays-of-the-world/scripts/optimizar-imagenes.py` (Pillow) | Lee originales de fuera del repo según un `PLAN` (carpeta, fichero, nombre final, ancho), redimensiona y guarda WebP en `src/assets/`. Informe con peso antes/después y aviso de **AMPLIADA** | `CALIDAD_WEBP = 90`; `CALIDAD_POR_IMAGEN` para excepciones (82); anchos 2880/1920/1600/1200 o `None` (nativo); `UMBRAL_AMPLIACION = 1.15`; `method=6`; `--dry-run` | Escalera de calidades; orientación EXIF (`convert("RGB")` no la aplica); conversión CMYK con perfil; **amplía** por encima de 1,15 (contra la regla) |
| `magnaceramica/scripts/convert-to-webp.mjs` (sharp) | Recorre `src/assets` y `public/img`, convierte cada JPG a WebP **con las mismas dimensiones** y **borra el JPG** | `QUALITY_LADDER = [74, 64, 56, 50]`: se queda con la primera que pesa menos que el JPG, o con la más ligera; `EFFORT = 6` | Tamaño máximo por hueco; recorte. Trabaja sobre la carpeta de destino: con nuestra estructura, los originales tienen que estar ya a salvo en `docs/imagenes/originales/` |
| `xanasystem-web/scripts/convert-fondos.mjs` (sharp) | Convierte PNG exportados de Figma a WebP para `public/fondos/` (fondos CSS) | Carpeta de origen como argumento; lista de trabajos con `width` por fichero (1100, 1600); `withoutEnlargement: true`; `quality: 78` | Escalera; es de uso puntual con nombres fijos |

**Regla de Windows con sharp**: leer el fichero a un buffer antes de
reescribirlo en el mismo sitio, o queda bloqueado
([trampas](../trampas-y-lecciones.md#imágenes-y-vídeo)).

**Ojo al relanzar un script de másters**: vuelve a codificar todo su plan, y
en clays dejó distintos byte a byte cuatro másters ya commiteados. Antes de
commitear, `git status src/assets` y devolver lo que no se quería cambiar
(`catalogo-stock.md`, «Ojo con el script de másters»).

### 6. Colocación — Claude

- **Contenido en `src/assets/<seccion>/`**, importado desde los datos
  (`src/data`), nunca escrito a mano en el componente. La imagen y su `alt`
  viajan juntos (`Media { src: ImageMetadata; alt: string }` en
  `clays-of-the-world/src/data/types.ts`).
- **`<Picture>` con AVIF y WebP, sin PNG de reserva** (sin `fallbackFormat`,
  Astro genera además PNG por cada ancho: 18 MB solo en el hero de clays):

  ```astro
  <!-- clays-of-the-world/src/components/sections/Hero.astro -->
  <Picture
    src={content.media.poster.src}
    alt={content.media.poster.alt}
    formats={['avif', 'webp']}
    fallbackFormat="webp"
    widths={[640, 1024, 1536, 1920, 2880]}
    sizes="100vw"
    loading="eager"
    fetchpriority="high"
  />
  ```

- **`widths` y `sizes` según el hueco real**, no `100vw` por defecto:
  `sizes="(min-width: 1200px) 1200px, 100vw"` en la pieza de ancho de contenido
  (`Showcase.astro`). Si la foto se recorta con `object-fit: cover`, se pide más
  ancho del que ocupa el marco (`APC-gestion-patrimonial/src/components/sections/home/Hero.astro`:
  `sizes="(max-width: 899px) 100vw, 1920px"`, porque la columna es alta y la
  foto apaisada).
- **Solo la primera imagen visible** lleva `loading="eager"` +
  `fetchpriority="high"` (es el LCP). **Todas las demás, `loading="lazy"`** y
  `decoding="async"`. En una lista, `loading={i === 0 ? 'eager' : 'lazy'}`
  (`clays-of-the-world/src/pages/[...lang]/gallery.astro`).
- **`alt`** desde los datos, traducido con el resto; describe lo que se ve.
  Decorativa → `alt=""`.
- **`public/` solo para lo que Astro no puede procesar**, y optimizado antes a
  mano (paso 5): fondos de CSS (`background-image`), `poster` de vídeo cuando no
  se puede pasar por `getImage`, logo PNG del correo del formulario, imagen OG y
  favicons (`magnaceramica/CLAUDE.md` § Conventions: `public/img/` solo para
  fondos CSS y pósters).
- Póster de vídeo optimizado sin `public/`: `getImage({ src, format: 'webp',
  width: 1920 })` y usar `.src` (`clays Hero.astro`) → [vídeos](videos.md).

### 7. Imagen para redes y favicons — Claude (Belén aprueba)

**Imagen OG**: `public/og-image.jpg`, **1200×630**, JPEG. Se enlaza con URL
absoluta desde `Base.astro` → [SEO técnico](seo-tecnico.md).

- **Sin texto** si la web tiene o tendrá varios idiomas: con solo la marca vale
  para todos; título y descripción los pinta cada red (clays `generar-og.py`).
  Con un idioma se puede añadir el lema (xanatechnologies `make-og-image.mjs`).
- **Foto real y del cliente** si la hay: es lo primero que se ve de la web.
- El logo respeta el manual (en clays, pastilla blanca: el manual prohíbe el
  logo directamente sobre foto).

| Script | Parámetros reales |
| --- | --- |
| `clays-of-the-world/scripts/generar-og.py` (Pillow) | `FOTO` desde `src/assets/`, `LOGO` `public/brand/logo-horizontal.png`; recorte a 1200×630 quitando cielo por arriba; pastilla blanca `MARGEN = 56`, `LOGO_ANCHO = 420`, `AIRE = (36, 28)`, `RADIO = 20`; JPEG `quality=85`, `optimize`, `progressive`. Resultado 143 KB |
| `xanatechnologies/scripts/make-og-image.mjs` (sharp) | Foto `resize(1200, 630, { fit: 'cover', position: 'top' })`, velo SVG en degradado, logo blanco a 520 px centrado, lema en SVG; `jpeg({ quality: 88, mozjpeg: true })`. **Ruta absoluta escrita en el script** (`C:/Proyectos/xanatechnologies`): cambiarla al copiar |

**Favicons** en `public/`:

| Fichero | Medida | Nota |
| --- | --- | --- |
| `favicon.svg` | vectorial | Del logo en SVG |
| `favicon.ico` | 16, 32 y 48 px | Fondo transparente |
| `apple-touch-icon.png` | 180 px | **Fondo opaco**: iOS pinta la transparencia de negro. Con aire alrededor para el redondeo de esquinas |

`clays-of-the-world/scripts/generar-favicon.py` los saca del logo PNG
reproduciendo el recorte del SVG del cliente (`ESCALA`, `DESPLAZAMIENTO` del
`<pattern>`); `.ico` con `sizes=[(16,16),(32,32),(48,48)]` y apple de 180 con
`aire = 18` sobre blanco. Aviso del propio script: el «SVG» del cliente era una
exportación de Figma con un PNG en base64 dentro (60 KB), no un vector.

## 4. Plantillas

| Plantilla | Se copia a | Fase |
| --- | --- | --- |
| [`proyecto/busqueda-stock.md`](../plantillas/proyecto/busqueda-stock.md) | `docs/imagenes/busqueda-stock.md` | 03 |
| [`proyecto/inventario-imagenes.md`](../plantillas/proyecto/inventario-imagenes.md) | `docs/imagenes/inventario.md` | 00 → 07 |

Al cliente: [material para la web](../plantillas/cliente/material.md)
(originales sin comprimir ni por WhatsApp, cuáles no se pueden usar o citan autor).

## 5. Dónde queda cada cosa

```
docs/imagenes/
├── originales/          sin tocar · EXCLUIDA DE GIT
├── busqueda-stock.md    fichas de búsqueda (git)
└── inventario.md        una fila por imagen (git)
brand/logo/              logos del cliente (SVG; PNG solo para el correo)
scripts/                 optimizar-imagenes · generar-og · generar-favicon
src/assets/<seccion>/    másters WebP de contenido (git)
public/
├── og-image.jpg         1200×630
├── favicon.svg · favicon.ico · apple-touch-icon.png
├── brand/logo-*.png     solo el logo del correo del formulario
└── img/ o fondos/       fondos CSS y pósters que no pasan por Astro
dist/_astro/             variantes AVIF/WebP del build (no se versiona)
```

## 6. Reglas y trampas

- **Ningún texto da a entender que una foto de stock o de IA son las
  instalaciones, el equipo o el producto del cliente.** Ni `alt`, ni pies, ni
  titulares al lado. En clays el copy lo recoge expresamente
  (`catalogo-stock.md`, «Lo que queda de stock»).
- **No se repite una imagen** en la misma web. Si no hay más remedio, se anota
  en el inventario con el porqué (en clays `material-arcilla-polvo` sale dos
  veces porque el cliente no mandó fotos de producto).
- **Nada provisional se publica.** Una imagen marcada provisional en el
  inventario bloquea la [revisión](revision.md).
- **Licencia comprobada antes de publicar** para lo de stock (pendiente abierto
  en clays).
- **Logos en SVG.** El PNG solo para el correo del formulario. Sin SVG se pide
  de nuevo al cliente antes de construir (en clays sigue abierto «Exportar
  logos a SVG»).
- **El peso se juzga con `npm run build` + `npm run preview`**, nunca en `dev`:
  en desarrollo Astro convierte a AVIF en cada petición
  ([trampas](../trampas-y-lecciones.md#imágenes-y-vídeo)).
- **Sin `fallbackFormat="webp"`, megas de PNG** en el build
  ([trampas](../trampas-y-lecciones.md#imágenes-y-vídeo)).
- **`public/` no se optimiza solo**
  ([trampas](../trampas-y-lecciones.md#imágenes-y-vídeo)); en festival-viento
  el favicon PNG pesa 675 KB ([deuda](../anexos/deuda-proyectos.md)).
- **Las páginas de prueba publican sus imágenes**: las `/pruebas-*` de clays
  hinchaban `dist/_astro` con fotos que la web ya no usa
  ([trampas](../trampas-y-lecciones.md#astro-y-build)). Al retirar una imagen,
  se borra de `src/assets/` y del inventario (queda en el historial).
- **Texto sobre foto**: el contraste se mide sobre la foto real, con el velo, y
  se vuelve a medir con cada foto nueva (`gallery.astro`, «AVISO DE CONTRASTE»)
  → [maquetación](maquetacion.md).
- **Capturas de referencia en WebP**: un PNG en el historial de git no se quita
  convirtiéndolo después (clays, 5,1 MB de capturas).

## 7. Checklist

**Búsqueda y entrada (03)**
- [ ] `docs/imagenes/originales/` en `.gitignore`
- [ ] Todo lo recibido, sin tocar, en `originales/`
- [ ] Ficha en `busqueda-stock.md` por cada hueco sin imagen (o descripción IA si Belén lo indicó)
- [ ] Belén ha descargado las candidatas de Magnific con su referencia
- [ ] Inventario cerrado: cada hueco con imagen o con decisión

**Construcción (07)**
- [ ] Cada imagen vista y asignada; nombres `seccion-descripcion-01`
- [ ] Inventario con origen, referencia, uso, provisional y derechos
- [ ] Másters WebP en `src/assets/<seccion>/`: recortados, ≤ ancho ×2, sin ampliar, sin metadatos, orientación y color corregidos
- [ ] Ningún máster pesa más que su original
- [ ] `<Picture formats={['avif','webp']} fallbackFormat="webp">` con `widths` y `sizes` del hueco
- [ ] `alt` desde los datos; `alt=""` en decorativas
- [ ] Solo la primera imagen visible con `eager` + `fetchpriority="high"`; el resto `lazy`
- [ ] En `public/` solo fondos CSS, pósters, logo del correo, OG y favicons, ya optimizados
- [ ] `og-image.jpg` 1200×630; `favicon.svg`, `favicon.ico`, `apple-touch-icon.png` opaco

**Revisión (08)**
- [ ] Ninguna imagen provisional; licencias de stock comprobadas
- [ ] Ningún texto atribuye el stock o la IA al cliente; ninguna imagen repetida sin anotar
- [ ] Peso de `dist/` medido con build + preview; sin PNG generados
- [ ] Recortes correctos en 375 px, 768 px y escritorio
