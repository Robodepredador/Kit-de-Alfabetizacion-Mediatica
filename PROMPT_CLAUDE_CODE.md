# Prompt para Claude Code

Copie todo lo que está debajo de la línea y péguelo en Claude Code, dentro de una carpeta vacía que contenga esta carpeta `design_handoff_kit_alfabetizacion/`.

---

Quiero que construyas y despliegues en Vercel un prototipo web navegable llamado **"Kit de Alfabetización Mediática para Adultos Mayores"**. Es un asistente educativo que ayuda a personas mayores en Perú a analizar críticamente cómo los medios, las redes sociales y la inteligencia artificial representan la vejez. Lo voy a presentar a mi compañera de proyecto, así que debe verse terminado y funcionar sin errores en celular y en computadora.

## Material de referencia (léelo todo antes de escribir código)

- `design_handoff_kit_alfabetizacion/README.md`: especificación completa con pantallas, colores, tipografías, medidas, textos exactos, comportamiento y reglas de accesibilidad.
- `design_handoff_kit_alfabetizacion/index.html`: el prototipo de diseño en HTML. Es la **referencia visual y de comportamiento**, no código para copiar tal cual. Ahí están todos los textos, los datos (`LESSONS`, `ACTS`, `SCENES`, `SURVEY`), la mascota SVG `mascot()` y la lógica de navegación. Lee la clase `Component` y el template completo.
- `design_handoff_kit_alfabetizacion/LUPITA.md` y `lupita-catalogo.html`: catálogo completo de Lupita (todos los estados, geometría exacta, keyframes, reglas de prioridad, tamaños y movimiento reducido). Implementa el componente `Lupita` siguiendo este documento al pie de la letra.
- `design_handoff_kit_alfabetizacion/lupita/`: imágenes listas (SVG animados, SVG estáticos y PNG). Usa `png/lupita-logo-512.png`, `-192.png` y `-32.png` para el favicon, el ícono de la app y la imagen Open Graph.
- `design_handoff_kit_alfabetizacion/docs/resumen-proyecto.md`: el documento del proyecto (marco teórico y prompt del asistente).
- `design_handoff_kit_alfabetizacion/assets/logo.png`: logo original (ya no se usa en la interfaz; Lupita lo reemplaza).

## Stack

- **Next.js (App Router) + TypeScript + Tailwind CSS**, listo para Vercel sin configuración extra.
- Fuentes con `next/font/google`: **Merriweather** (700, 900) para títulos y **Atkinson Hyperlegible** (400, 700) para el texto.
- Sin backend ni base de datos. Todo el contenido vive en archivos de datos tipados (`/data/lessons.ts`, `/data/activities.ts`, `/data/video.ts`, `/data/survey.ts`, `/data/chat.ts`).
- Las respuestas del chat siguen guionadas, igual que en el prototipo. **No** conectes ninguna API de IA.
- La voz usa la Web Speech API del navegador (`speechSynthesis` con `lang="es-PE"` y `rate=0.9`; `SpeechRecognition` / `webkitSpeechRecognition` para el micrófono), con mensajes amables si el navegador no la soporta.

## Estructura sugerida

- Una ruta por pantalla, para que el botón "atrás" del navegador y los enlaces directos funcionen:
  `/` (Bienvenida), `/como-se-usa` (video), `/menu`, `/aprender`, `/aprender/[tema]?paso=1|2|3`, `/conversar`, `/practicar?actividad=1|2|3`, `/practicar/completado`, `/encuesta`, `/gracias`.
- Componentes: `Lupita` (la mascota SVG animada, con las props `arm`, `mouth`, `jump`, `sparkles`, `size` y `still`), `TopBar` (con "Volver" y la mini Lupita), `GuideBubble` (Lupita más una frase y el botón Escuchar), `ListenButton`, `MicButton`, `BigButton` (variantes primaria y secundaria), `ProgressBars`, `ChatBubble`, `OptionButton`, `FaceRating`, `Toast`.
- Hooks: `useSpeech()` (hablar y detener, con `speakingId` global para que solo suene un texto a la vez y Lupita mueva la boca) y `useDictation()`.
- Estado: React Context para las respuestas de práctica, los votos de la encuesta y el origen de la práctica (menú, tema o chat), que define a dónde lleva "Volver". Guárdalo en `sessionStorage` para que sobreviva a recargas durante la demo.

## Requisitos que no se pueden romper (accesibilidad para adultos mayores)

- Texto base de **24px**, títulos de 30px o más, ningún texto de menos de 18px.
- Botones de **mínimo 64px de alto** (56px los secundarios dentro del chat), siempre con ícono + texto.
- Contraste WCAG AA. Los colores de acento (#7093BF, #E69277, #7DBFB8) **nunca** se usan para texto, solo para formas, fondos de íconos y detalles. El texto va en #07336D o #1E1816 y el secundario en #5A4E48.
- Máximo 3 o 4 opciones por pantalla. "Volver" siempre visible en la barra fija superior (en todas las pantallas menos la Bienvenida). "Escuchar" en todos los textos importantes.
- Trato de "usted", tono cálido y nunca infantil. En la práctica nunca se dice "¡Incorrecto!".
- Foco visible (`outline: 4px solid #07336D`), `lang="es"`, `aria-live` en la retroalimentación y en el chat, `aria-pressed` en las opciones elegidas.
- Respeta `prefers-reduced-motion` y agrega un interruptor "Animaciones sí/no" accesible desde el menú.
- Ningún flujo puede quedar sin salida. Revisa cada pantalla.

## Tareas para terminar el prototipo

1. Recrea **todas** las pantallas del prototipo con los mismos textos, colores, medidas y animaciones (los keyframes `lpFloat`, `lpBlink`, `lpWave`, `lpTalk`, `lpPoint`, `lpJump` y `lpTwinkle` están en el `<style>` del HTML).
2. El **video tutorial** (`/como-se-usa`) tiene 8 escenas: cada una avanza cuando se cumplen a la vez su duración mínima y el fin de la voz. Incluye Pausar, Ver desde el principio y Saltar video. Si el navegador bloquea el audio antes de una interacción, la reproducción empieza con el toque en "Empezar".
3. **Actividad 3:** crea un `public/ia-mayores.jpg` provisional (un recuadro neutro con texto alternativo) y deja un comentario `TODO` claro para reemplazarlo por la imagen real hecha con IA. Mantén la descripción escrita debajo de la imagen.
4. **Encuesta:** guarda las respuestas en `localStorage` con fecha y hora, y agrega una ruta oculta `/resultados` que liste las respuestas y permita descargarlas como CSV. Es para la sesión de validación.
5. Agrega `metadata` (título, descripción en español, `themeColor: #07336D`), un favicon y un ícono de app creados a partir de Lupita, y un `manifest.webmanifest` para que se pueda "agregar a la pantalla de inicio" en el celular.
6. Revisa en anchos de 360px, 768px y 1280px. En computadora el contenido se centra con un ancho máximo de unos 800px.
7. Ejecuta `npm run build` y `npm run lint` sin errores. Luego haz una revisión manual del flujo completo: Bienvenida → Video → Menú → cada tema (3 pasos) → Practicar este tema → 3 actividades → Encuesta → Gracias → Inicio, y también Menú → Conversar → las 3 respuestas rápidas.
8. Inicializa git, crea el repositorio y despliega con `npx vercel --prod`, o dame los pasos exactos para conectarlo desde vercel.com si necesitas que yo inicie sesión.
9. Al terminar, dame: la URL pública, un resumen corto de lo que hiciste, la lista de cosas pendientes (por ejemplo, la imagen real o el nombre definitivo del asistente) y un guion de demo de 3 minutos para presentárselo a mi compañera.

Antes de empezar, muéstrame tu plan en una lista corta y pregúntame solo si algo es realmente ambiguo.
