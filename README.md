# Kit de Alfabetización Mediática para Adultos Mayores

> **Qué es este repositorio.** El prototipo navegable, publicado como sitio estático.
> `index.html` es la aplicación: ábralo en el navegador o visite la URL de Vercel.
> No hay build, ni dependencias que instalar, ni backend.
>
> - **Prototipo:** [`index.html`](./index.html)
> - **Catálogo de Lupita:** [`lupita-catalogo.html`](./lupita-catalogo.html)
> - **Especificación de diseño:** el resto de este documento.
>
---

## Handoff: especificación de diseño

## Overview
Web prototype (mobile first, also desktop) of an educational assistant that helps older adults in Peru critically analyze how media, social networks and AI represent old age. UI language: Spanish (Peru), formal "usted", warm and never childish. The guide character is **Lupita**, an animated magnifying glass that is also the kit's logo.

## About the Design Files
`index.html` is a **design reference built in HTML**: a working prototype showing the intended look, copy and behavior. It is not production code to ship. Recreate it in a real framework (recommended: Next.js + TypeScript + Tailwind, deployed on Vercel). All copy and content data live in the `<script data-dc-script>` block (the `LESSONS`, `ACTS`, `SCENES`, `SURVEY` and `FACES` constants, the `initialMsgs()` and `quick()` methods, and the `mascot()` SVG builder). The markup lives inside `<x-dc>…</x-dc>`. `{{ name }}` holes are filled by `renderVals()`.

## Fidelity
**High-fidelity.** Final colors, type, copy, sizes and animations. Reproduce them faithfully.

## Design Tokens
**Colors**
- `#FFF6E3`: page background (cream)
- `#FFFFFF`: cards, secondary buttons
- `#07336D`: primary (titles, primary buttons, top bar, borders, icon strokes). Hover `#052752`; button shadow `#041F44`.
- `#1E1816`: body text
- `#5A4E48`: secondary text (7.5:1 on cream)
- `#EADCC0`: neutral card borders and placeholder bars
- Accents, never used for text: `#7093BF` (mid blue), `#E69277` (peach), `#7DBFB8` (teal)
- Light tints for icon tiles: `#DDE6F1` (blue; also the selected and hover background), `#F9E1D8` (peach), `#DCEFEC` (teal)
- Lupita's cheeks: `#F2B6A2`

**Typography**
- Titles: Merriweather 900. H1 `clamp(30–32px, 5–6vw, 40–50px)`, line-height 1.2.
- Body: Atkinson Hyperlegible 400/700. Base 24px, line-height 1.5. Lesson text 26px. Buttons 24–28px, weight 700. Small labels 18–22px (minimum 18px).

**Shape and spacing**
- Radius: buttons 10px, cards 12px, chat bubbles 14px with one corner at 4px, progress bars 3px.
- Borders 3px. Primary buttons have `box-shadow: 0 4px 0 #041F44` (5px on the hero button).
- Clickable cards have a `3px solid #07336D` border and `box-shadow: 0 4px 0 #07336D`.
- Content max-width is 800px (820px for chat, 880px for the video), side padding 16–20px, gaps 12–22px.
- Focus: `outline: 4px solid #07336D; outline-offset: 3–4px`.
- Button heights: primary 68–72px, secondary 64px, chat-inline 56px (minimum).

## Lupita (mascot / logo)
An SVG with a 220×242 viewBox, built by `mascot()` in the HTML. Recreate it as a React component:
- **Lens:** circle r72 filled peach `#E69277` with a 6px navy stroke; an inner cream circle r53 with a 5px navy stroke; a white highlight arc.
- **Face:** navy oval eyes with white glints, peach cheeks, and a mouth that is `smile`, `talk` (animated ellipse) or `open` (celebrate).
- **Handle:** navy, 30px stroke, with a peach inner stroke, going down-right.
- **Arm:** a left arm ending in a white hand. Poses: `idle`, `wave`, `point` (bobs down toward the button), `up` (celebrate).
- **Optional:** teal ground shadow and three twinkling 4-point stars (`sparkles`).

Animations (keyframes in the HTML `<style>`):
- `lpFloat` 3.2s: body bob of −6px
- `lpBlink` 4.5s: eyes
- `lpWave` 0.45s × 6 alternate: hand wave at the start
- `lpTalk` 0.2s alternate: mouth, while text is being read aloud
- `lpPoint` 1s: pointing arm
- `lpJump` 0.8s: celebrate jump
- `lpTwinkle` 1.1s: stars

All animations are disabled under `prefers-reduced-motion` and by the `animations` toggle.

**Where Lupita appears:**
- Top bar, 50px, static.
- Welcome screen, 190px, waving.
- Guide bubble on Menú, Aprender, Tema, Practicar and Encuesta, 104px.
- Video, 170px.
- Chat header (96px) and each bot message (56px). She talks while that message is read aloud.
- Activity feedback, 76px.
- Thanks screen, 190px, celebrating.

## Screens
1. **Bienvenida** `/`: Lupita with a "Hola, soy Lupita" tag. H1 with the kit name, a one-line tagline, the "Empezar" button (opens the video) with the note "Primero verá un video corto de 1 minuto.", and an "Escuchar" button. Decorative teal and peach circles in the corners.
2. **¿Cómo se usa?** (video): 8 scenes (text and visuals in `SCENES` and the `vs1`–`vs8` blocks), 8 progress bars, "Parte N de 8", and a large navy caption bar (24–30px, white). Lupita poses per scene; the buttons being explained are highlighted with a 6px `#E69277` outline. Controls: Pausar / Seguir viendo, Ver desde el principio, Saltar video. The final button is "Ir al menú". Opened from the Welcome screen and from the Menu.
3. **Menú:** guide bubble with a greeting based on the hour (Buenos días / Buenas tardes / Buenas noches). Three full-width cards: Conversar con Lupita, Aprender, Practicar. Below them, a dashed button "¿Cómo se usa? Ver video de 1 minuto".
4. **Aprender:** 5 full-width cards, each with a number tile and a title.
5. **Tema:** "Tema N de 5", the H1, then "Paso N de 3: La idea / Un ejemplo / Para pensar" with 3 progress bars. One idea per step. Step 3 is a navy card with the question. Buttons: Anterior / Siguiente, and on step 3 "Practicar este tema" (goes to the matching activity) and "Ver otro tema".
6. **Conversar:** header, a "Conversación de ejemplo" notice, and the scripted conversation (user's WhatsApp chain about a diabetes "cure", then 3 bot messages). Each bot message has an Escuchar button. Three stacked quick replies: "¿Cómo verifico una noticia?" (4-step answer), "Quiero practicar" (answer with an "Ir a Practicar" button), and "Volver al menú". A text input plus Hablar (mic) and Enviar buttons; free text gets a polite scripted fallback.
7. **Practicar:** "Actividad N de 3" with bars, the H1, and an "Ejemplo inventado para practicar" chip. Example per activity: a fake headline, a WhatsApp message, or an AI image placeholder plus its description. The question, then 3 option buttons (A/B/C). Choosing one shows feedback with Lupita, a title and an explanation, plus Escuchar and "Siguiente actividad" / "Terminar". The page auto-scrolls to the feedback. Feedback always recognizes what is right and explains; it never says "Incorrecto".
8. **Completado:** "¡Completó las actividades!" with buttons "Ir a la encuesta" and "Volver al menú". Lupita celebrates in the guide bubble.
9. **¿Qué le pareció?:** 3 questions, each with 3 face buttons (labels in `SURVEY`: Fácil/Regular/Difícil, Sí, todo/Algunas cosas/Me costó, Sí/Tal vez/No). An optional comment textarea with a Hablar button, and Enviar.
10. **¡Muchas gracias!:** celebrating Lupita and a "Volver al inicio" button.

## Navigation ("Volver" in the top bar)
- Menú goes to Bienvenida.
- Video goes back to where it was opened from.
- Aprender goes to Menú.
- Tema goes to the previous step, or to Aprender from step 1.
- Conversar goes to Menú.
- Practicar goes to its origin (Tema, Conversar or Menú).
- Encuesta goes to Menú.
- Gracias goes to Bienvenida.

## Behavior and state
- **Speech:** one global utterance at a time (`es-PE`, rate 0.9). The button toggles between Escuchar and Detener (filled navy while active). Speech stops when the screen changes. If speech isn't supported, show a toast.
- **Dictation:** SpeechRecognition `es-PE` appends text to the field. If it isn't supported or fails, show a toast suggesting the keyboard.
- **Practice:** `answers {actIndex: optionIndex}`. "Siguiente" goes to the next unanswered activity; when all 3 are done, show Completado.
- **Survey:** `votes {q: face}` and `comment`. Submitting is allowed with blanks.
- **Toast:** fixed at the bottom, dark `#1E1816`, with an "Entendido" button.

## Assets
- `lupita/`: full Lupita image catalog (animated SVG, static SVG and 512px PNG per state, plus logo PNGs at 512/192/32). Specs in `LUPITA.md`; visual catalog in `lupita-catalogo.html`.
- `assets/logo.png`: the original 3D logo (reference only; replaced by Lupita).
- Icons are inline stroke SVG paths (the `IC` map in the HTML), Feather-style.
- The activity 3 image is a placeholder; a real AI-generated image is pending.

## Files
- `index.html`: the full prototype (open it in a browser).
- `lupita-catalogo.html`: visual catalog of every Lupita state with animation rules.
- `LUPITA.md`: Lupita geometry, keyframes, state rules, sizes, reduced motion.
- `docs/resumen-proyecto.md`: project document (research and assistant prompt).
- `PROMPT_CLAUDE_CODE.md`: the ready-to-paste prompt for Claude Code.
