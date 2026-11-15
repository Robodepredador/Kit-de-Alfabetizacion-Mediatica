# Lupita: catálogo de imágenes y especificación de animaciones

Abra `lupita-catalogo.html` en un navegador para ver todos los estados animados.

## Archivos (`lupita/`)

| Estado | Animado | Estático (movimiento reducido) | PNG 512 px |
|---|---|---|---|
| Reposo | `lupita-reposo.svg` | `lupita-reposo-estatica.svg` | `png/lupita-reposo.png` |
| Saludo | `lupita-saludo.svg` | `lupita-saludo-estatica.svg` | `png/lupita-saludo.png` |
| Señala | `lupita-senala.svg` | `lupita-senala-estatica.svg` | `png/lupita-senala.png` |
| Habla | `lupita-habla.svg` | `lupita-habla-estatica.svg` | `png/lupita-habla.png` |
| Celebra | `lupita-celebra.svg` | `lupita-celebra-estatica.svg` | `png/lupita-celebra.png` |
| Logo | — | `lupita-logo-estatica.svg` | `png/lupita-logo-512.png`, `-192.png`, `-32.png` |
| Avatar chat | `lupita-avatar.svg` | `lupita-avatar-estatica.svg` | `png/lupita-avatar.png` |
| Avatar hablando | `lupita-avatar-habla.svg` | `lupita-avatar-habla-estatica.svg` | `png/lupita-avatar-habla.png` |

En los SVG animados del catálogo todas las animaciones se repiten sin fin, para poder verlas. En la app, `lpWave` del saludo se repite solo 6 veces (ver la tabla). Para la app, **recree Lupita como componente React** (`<Lupita arm mouth jump sparkles size still shadow />`). Así se cambia de estado sin cambiar de archivo y la boca se sincroniza con la voz. Use los SVG y PNG para el favicon, el ícono de la app, la presentación y las redes.

## Geometría (viewBox `0 0 220 242`)

```
sombra       ellipse cx116 cy222 rx60 ry8  fill #7DBFB8          (sin sombra en logo/avatar)
mango        path M158 150 L194 194  stroke #07336D 30 round
             path M170 165 L192 192  stroke #E69277 17 round
lente        circle cx110 cy100 r72  fill #E69277 stroke #07336D 6
vidrio       circle cx110 cy100 r53  fill #FFF6E3 stroke #07336D 5
brillo       path M74 84 A40 40 0 0 1 98 62  stroke #FFFFFF 7 round
ojos         ellipse (92,96) y (128,96) rx9 ry12 fill #07336D; brillos circle (95,91) y (131,91) r3.5 #FFFFFF
mejillas     circle (79,116) y (141,116) r7 fill #F2B6A2
boca smile   path M96 116 Q110 130 124 116  stroke #07336D 5 round
boca talk    ellipse cx110 cy122 rx10 ry8 fill #07336D
boca open    path M94 114 Q110 142 126 114 Z fill #07336D
brazo idle   path M46 128 Q22 140 28 164   mano (28,170)
brazo wave   path M46 122 Q16 112 20 82    mano (20,74)
brazo point  path M46 132 Q24 150 36 178   mano (38,184) + dedo M38 192 L38 206 stroke 8
brazo up     path M46 118 Q18 94 28 60     mano (28,52)
             brazo stroke #07336D 10 round; mano circle r12 fill #FFFFFF stroke #07336D 5
estrellas    4 puntas (curvas Q al centro): (196,26) s14 #7DBFB8 · (26,24) s11 #E69277 · (206,104) s10 #7093BF; stroke #07336D 2.5
```

Orden de capas: sombra, luego el grupo del cuerpo (mango, brazo, lente, vidrio, brillo, ojos, mejillas y boca) y al final las estrellas.

## Keyframes (CSS)

```css
@keyframes lpFloat{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
@keyframes lpBlink{0%,92%,100%{transform:scaleY(1)}95%{transform:scaleY(.1)}}
@keyframes lpWave{from{transform:rotate(-14deg)}to{transform:rotate(18deg)}}
@keyframes lpTalk{from{transform:scaleY(.3)}to{transform:scaleY(1.1)}}
@keyframes lpPoint{0%,100%{transform:translateY(0)}50%{transform:translateY(8px)}}
@keyframes lpJump{0%,100%{transform:translateY(0)}40%{transform:translateY(-16px)}}
@keyframes lpTwinkle{0%,100%{transform:scale(.4);opacity:.35}50%{transform:scale(1.1);opacity:1}}
@media (prefers-reduced-motion:reduce){*{animation:none!important}}
```

| Parte | Animación | Configuración | transform-origin |
|---|---|---|---|
| Grupo del cuerpo | lpFloat (o lpJump en Celebra) | 3.2s ease-in-out infinite / .8s ease-in-out infinite | — |
| Grupo de ojos | lpBlink | 4.5s infinite | `transform-box: fill-box; center` |
| Brazo (wave) | lpWave | .45s ease-in-out .2s **6** alternate | `transform-box: view-box; 46px 124px` |
| Brazo (up) | lpWave | .35s ease-in-out infinite alternate | igual |
| Brazo (point) | lpPoint | 1s ease-in-out infinite | — |
| Boca talk | lpTalk | .2s ease-in-out infinite alternate | `fill-box; center` |
| Estrellas | lpTwinkle | 1.1s ease-in-out infinite, retrasos 0 / .35 / .7s | `fill-box; center` |

## Estados y cuándo usarlos

| Estado | Props | Pantallas |
|---|---|---|
| Reposo | `arm=idle mouth=smile` | Tema pasos 1 y 2, actividad ya respondida, retroalimentación |
| Saludo | `arm=wave` | Bienvenida, guía del Menú, cabecera del Chat, escena 1 del video |
| Señala | `arm=point` | Aprender, Tema paso 3, actividad sin responder, Encuesta, escenas 3–7 del video |
| Habla | `mouth=talk arm=idle` | Mientras `speechSynthesis` lee un texto ligado a esa Lupita |
| Celebra | `arm=up mouth=open jump sparkles` | Actividades completadas, Gracias, escena 8 del video |
| Logo | `shadow=false still` | Barra superior, favicon, ícono de la app |
| Avatar | `shadow=false`, sin flotar; `mouth=talk` al leer su mensaje | Burbujas del chat |

**Prioridad**, si varios aplican: Habla > Celebra > Señala > Saludo > Reposo. El cambio es inmediato, sin fundido. Con `utterance.onstart` Lupita pasa a Habla, y con `onend`/`onerror` vuelve al estado de la pantalla.

**Tamaños (ancho):** Bienvenida y Gracias 190 · Video 170 · Guía 104 · Cabecera del chat 96 · Retroalimentación 76 · Avatar 56 · Barra 50. La proporción se mantiene en 220:242.

**Movimiento reducido:** con `prefers-reduced-motion` o con el interruptor «Animaciones: no», se muestra el mismo estado sin animar (`still`). La voz sigue funcionando.

**Accesibilidad:** Lupita es `aria-hidden` salvo en Bienvenida y Gracias (`role="img"` con un `aria-label`). Su estado nunca es la única fuente de información.
