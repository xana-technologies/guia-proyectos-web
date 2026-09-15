# Forma de trabajo

Cómo se colabora en los proyectos web: con Belén, con el cliente y con el
repositorio. Vale para cualquier proyecto; el `CLAUDE.md` de cada uno puede
añadir reglas propias.

## 1. Antes de tocar nada

- **Plan y preguntas primero.** Ante una tarea nueva, Claude explica cómo lo
  va a hacer y pregunta lo que no esté claro, y espera. «No hagas nada antes,
  dime cómo vas a hacerlo y si tienes dudas» (grupomerhi); preguntas de
  aclaración antes de implementar (festival-viento).
- **Leer el estado actual.** Varias sesiones y el editor de Belén pueden estar
  tocando el mismo repositorio: `git status` y releer los ficheros antes de
  editarlos (clays-of-the-world, xanasystem-web).
- **Consultar la guía**: lo que va siempre no se pregunta, se aplica
  ([README](README.md)); ante un fallo, [trampas y lecciones](trampas-y-lecciones.md).

## 2. Cómo se itera

- **Sección a sección, de arriba abajo**, con referencia visual (Figma,
  imagen o CSS de referencia) (festival-viento, grupomerhi).
- **Feedback puntual → cambio mínimo.** No se rediseña lo que no se ha pedido
  (memorias de xanasystem-web).
- **Si el cambio toca un componente compartido**, se aísla para la sección
  pedida en vez de cambiarlo en toda la web (xanasystem-web).
- **Proponer antes de extender.** Algo que funciona en una página no se aplica
  a todas sin preguntar (APC).
- **Valores exactos.** Si viene de Figma, se leen del nodo, no a ojo; los
  ajustes en píxeles que da Belén se aplican tal cual
  ([diseño en Figma](flujos/diseno-figma.md)).
- **Decisiones cerradas no se reabren.** Si algo se decidió (p. ej. publicar
  los vídeos sin comprimir en Clays), no se vuelve a proponer.

## 3. Datos y contenido

- **No se inventa nada**: ni cifras, ni datos del cliente, ni textos que
  aparenten ser hechos. Lo que falta se marca `[[ ]]` y no se publica
  ([contenidos y SEO](flujos/contenidos-seo.md)).
- El copy lo aprueba el cliente; los datos legales, también.
- Las fotos de stock no se presentan como del cliente
  ([imágenes](flujos/imagenes.md)).

## 4. Comprobar antes de decir que funciona

- **Nunca «ya está» sin haberlo comprobado.** Si no se ha podido verificar, se
  dice.
- Cómo verificar: `npm run build` + `npm run preview` para peso e imágenes;
  peticiones directas al servidor para contenido; en el navegador, geometría
  con `getBoundingClientRect`/`getComputedStyle`. Las capturas y los colores
  del preview no siempre son fiables → [trampas · verificación](trampas-y-lecciones.md#verificación-y-herramientas).
- El formulario y las cookies se prueban en la URL temporal, no solo en local.

## 5. Git y publicación (decidido 14-09-2026)

1. **Commit**: al terminar una pieza (sección aprobada, flujo funcionando,
   arreglo cerrado). Claude avisa, propone el mensaje y **espera el sí**.
   Nunca un commit por cada ronda de feedback.
2. **Mensajes**: en español, en imperativo, sin prefijos, explicando el porqué
   cuando no sea obvio (`Añade…`, `Corrige…`). (XPM-front tiene su propia
   convención; ver [fuera de la guía](anexos/fuera-de-la-guia.md).)
3. **Solo los ficheros propios**: nada de `git add -A` ni `git add .`; se
   añade fichero a fichero lo que ha tocado Claude.
4. **Push**: agrupado y con permiso. Cada push obliga a repetir el paso manual
   de publicación.
5. **Publicación en Vercel: siempre Belén, en todos los proyectos**, con un
   commit manual desde la web de GitHub con la cuenta `xanasystem`, que es lo
   que lanza el despliegue (Vercel rechaza los commits de
   `belen@xanasystem.com`).
6. **Claude nunca publica** (ni `vercel deploy` ni `vercel --prod`) **ni dice
   «publicado»** tras un push. Dice: «subido a GitHub, pendiente de tu paso en
   GitHub».
7. **Ramas**: por defecto se trabaja y se publica en `main`, también para
   revisar en la URL temporal. Ramas solo si se pide.
8. **Cada permiso vale para ese cambio**, no para los siguientes
   (xanatechnologies).
9. **Clave SSH**: remoto con el alias `github-belenxana`
   ([arranque de proyecto](flujos/arranque-proyecto.md)).

## 6. Documentación del proyecto

- `CLAUDE.md` del proyecto al día: decisiones, cuentas, enlace a Figma,
  cuestiones abiertas.
- `CHECKLIST.md`: el estado real, tarea a tarea, con lo pendiente y de quién
  depende.
- Lo aprendido que valga para otros proyectos se **propone** para la guía; no
  se añade sin permiso.

## 7. Con el cliente

- Lo que se le pide va con las [plantillas](plantillas/cliente/README.md); lo
  que no llegue se marca y se reclama, no se rellena.
- Las revisiones se hacen sobre la URL temporal.
- Condiciones comerciales, plazos y mantenimiento: los fija Belén.

## Fuentes

Memorias de xanasystem-web (cambios mínimos, aislar componentes, `git add`
selectivo, Figma exacto), xanatechnologies (`feedback-no-commits-without-permission`),
clays-of-the-world (`CLAUDE.md`: push no publica, varias sesiones, commit por
pieza), APC (proponer antes de extender) · transcripts de grupomerhi y
festival-viento · decisiones de la guía (14-09-2026).
