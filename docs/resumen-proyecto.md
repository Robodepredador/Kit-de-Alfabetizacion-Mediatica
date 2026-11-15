# Asistente de alfabetización mediática para adultos mayores — Resumen del proyecto

Nov 14, 2026 · @Frank Li

## La idea en una frase

Tu amiga quiere hacer un **chatbot o asistente con IA que enseñe a personas mayores a "leer con lupa" lo que ven en la TV, los noticieros, las redes sociales y las imágenes hechas con IA**. La meta es que noten cuándo un contenido los muestra con estereotipos (débiles, enfermos, víctimas, "ya no sirven") y cuándo una información es falsa. En la presentación su grupo lo llama **"Kit de Alfabetización Mediática de Adultos Mayores"**, y el asistente sería la parte interactiva de ese kit.

## El problema que intenta resolver

- **Los medios muestran mal a los adultos mayores.** Según el estudio de CONCORTV 2025 que citan en la presentación, el 34% de las noticias sobre ellos trata de abandono, violencia o dependencia, y muchas veces aparecen como víctimas.
- **Eso les hace daño a ellos mismos.** El grupo se apoya en la **Teoría del Cultivo**: si alguien pasa años viendo que la vejez es decadencia, termina creyéndolo, también sobre sí mismo. Eso le baja la autoestima y hace que participe menos en la sociedad.
- **La IA repite el mismo sesgo.** Hoy las imágenes generadas por IA también muestran a los mayores de forma estereotipada.
- **Son más vulnerables a la desinformación**, sobre todo la de salud (remedios falsos, cadenas de WhatsApp).

## Qué aporta cada artículo

| Artículo | Qué demuestra | Para qué le sirve a la solución |
| --- | --- | --- |
| **Camacho-Markina y Santos-Diez (2025)**: revisión sistemática | La prensa, la TV y la radio estigmatizan a los mayores: los muestran pasivos, dependientes o como una carga. | Explica **qué estereotipos** debe enseñar a reconocer el asistente. |
| **Kroon y Van Selm (2024)** | En un experimento, dos cosas redujeron los estereotipos negativos sobre trabajadores mayores: una **advertencia de alfabetización mediática** ("ojo, los medios influyen en cómo ves la edad") y los videos con **ejemplos contrarios al estereotipo** (una persona mayor competente). | Es la **prueba de que la intervención funciona**. El asistente puede usar esas dos estrategias. |
| **Martens et al. (2025)** | Generaron imágenes de personas mayores con DALL-E 2 en 2022 y en 2023 y compararon. Las imágenes mostraban sobre todo emociones negativas, aunque las instrucciones eran neutras, y la IA no mejoró de un año a otro. | Respalda el eje de **"sesgos algorítmicos"**: el asistente debe enseñar a mirar críticamente lo que produce la IA. |
| **Hu et al. (2026)**: revisión sobre desinformación de salud | Muchos adultos mayores confían demasiado en rumores de salud, se dejan llevar por titulares emotivos y verifican poco. Los que recibieron capacitación en computación distinguían mejor. | Respalda el eje de **"reconocer la desinformación"** y muestra que la capacitación sí ayuda. |

**Ojo con un detalle:** en la bibliografía de la presentación aparece **Zhou et al. (2022)**, pero el PDF del proyecto es **Hu et al. (2026)**, una revisión que *incluye* a Zhou entre sus estudios. Conviene aclarar cuál se cita para que no haya incoherencias.

## Los 5 temas que debe cubrir el asistente

1. Cómo los medios construyen la imagen social de la vejez.
2. Las cinco formas del edadismo (la discriminación por edad).
3. Sesgos de los algoritmos y de la IA al representar la vejez.
4. Estrategias para reconocer desinformación en internet.
5. Formas de contar historias sobre la vejez sin estereotipos.

## Cómo se validaría

La presentación plantea una **sesión de unos 65 minutos con adultos mayores** (con carta a la institución y consentimiento informado):

- **Acercamiento (10 min):** conocer sus hábitos con los medios y lo que necesitan para usar la herramienta con comodidad (letra grande, voz, etc.).
- **Actividades (35 min):** presentarles el kit web y hacer 3 actividades interactivas.
- **Retroalimentación (20 min):** recoger su opinión sobre qué tan fácil fue usarlo.

## Lo que significa para el desarrollo

En la práctica, lo que hay que construir es:

- **Una web sencilla y accesible** (botones grandes, alto contraste, lenguaje simple, quizá lectura en voz alta), porque el diseño gira alrededor del adulto mayor.
- **Un chatbot con un LLM** al que se le da un *prompt de sistema* con su rol: tutor paciente, experto en esos 5 temas, que no infantiliza al usuario. Algo clave: **el propio asistente no debe caer en edadismo** al hablarles.
- **Actividades interactivas**, por ejemplo: "mira esta noticia o imagen, ¿qué estereotipo ves?", "¿esta noticia de salud es confiable?", y el bot les responde explicando el porqué.
- Opcionalmente, **videos con avatares** y **material visual**, que la presentación menciona entre las herramientas de IA que usaron.

## Revisión del prompt del agente

El prompt propuesto es una buena base: define bien el rol y el tono, y prohíbe lo más importante (ser condescendiente o alarmista, inventar datos). Pero no cubre todos los ejes de la presentación y tiene puntos que van a fallar al implementarlo.

**Lo que está bien**

- El rol es claro: tutor paciente que no infantiliza.
- Pide explicar cada término técnico con un ejemplo cotidiano y usar párrafos cortos, algo clave para este público.
- Prohíbe el tono alarmista. Coincide con Hu et al.: los mensajes emotivos y de miedo son justamente los que más confunden.

**Lo que hay que corregir**

1. **Las fuentes dependen de que "el usuario las proporcione".** El usuario será un adulto mayor y no va a subir artículos. El conocimiento de los 4 artículos tiene que estar dentro del agente: resumido en el prompt o cargado como base de conocimiento (RAG).
2. **No cubre todos los ejes del proyecto** (ver tabla abajo).
3. **"Usted o tú según se configure" queda abierto.** Para adultos mayores en Perú lo más seguro es "usted", y pasar a "tú" solo si la persona lo pide.
4. **"Extremadamente cálido" puede volverse en contra.** Exagerar la dulzura es otra forma de infantilizar. Mejor "cálido y respetuoso, como con un adulto con mucha experiencia de vida".
5. **Cerrar siempre con una pregunta se vuelve repetitivo.** Mejor "cuando sea natural".

| Eje de la presentación | ¿Está en el prompt original? |
| --- | --- |
| Construcción social de la vejez en los medios | Sí (área 1) |
| Las cinco formas del edadismo | Parcial: habla de estereotipos, no de edadismo ni de sus formas |
| Sesgos algorítmicos e IA | No. Falta lo del artículo de Martens (imágenes de IA) |
| Reconocer desinformación | Sí (área 4) |
| Narrativas inclusivas contra el edadismo | No de forma explícita |

**Lo que falta**

- **Temas sensibles:** ante "¿este remedio de WhatsApp cura la diabetes?", explicar cómo verificar y recomendar consultar al médico. Igual con estafas o pedidos de dinero.
- **Modo actividad:** la validación incluye 3 actividades interactivas, pero el prompt solo describe conversación libre.
- **Las estrategias que funcionan según Kroon y Van Selm:** advertir con calma cómo los medios influyen en la percepción y mostrar ejemplos de personas mayores activas y competentes.
- **Fuera de tema y límites:** qué responder ante temas ajenos y nunca pedir datos personales.
- **Largo de las respuestas:** un límite concreto (120–150 palabras) para no cansar.

## Prompt mejorado (propuesta)

Integra el prompt original con los 5 ejes de la presentación, la base de conocimiento y los límites de seguridad. Está listo para copiar al agente.

```text
# ROL
Eres "[Nombre del asistente]", un asistente educativo de alfabetización mediática
y digital para personas adultas mayores en el Perú. Acompañas a la persona a
entender, con calma y sin tecnicismos, cómo los medios (TV, radio, prensa, redes
sociales, WhatsApp) y la inteligencia artificial representan a las personas
mayores, y a analizar con criterio lo que ve y lee cada día.

Tratas a la persona de "usted" (salvo que pida lo contrario), como a un adulto
con mucha experiencia de vida: con calidez y respeto, nunca con exceso de dulzura,
elogios exagerados ni tono de maestro con niños.

# EJES TEMÁTICOS
1. Construcción social de la vejez en los medios: cómo noticieros y programas
   muestran a los mayores (muchas veces como víctimas, dependientes o en abandono)
   y cómo eso, repetido por años, influye en lo que todos —incluidos ellos—
   creemos sobre la vejez (teoría del cultivo, explicada con ejemplos simples).
2. Edadismo: qué es la discriminación por edad, sus formas (estereotipos =
   lo que pensamos, prejuicios = lo que sentimos, discriminación = lo que hacemos;
   hacia otros y hacia uno mismo) y cómo reconocerlo en la vida diaria.
3. Sesgos de la IA y los algoritmos: las imágenes y textos hechos por IA también
   repiten estereotipos (por ejemplo, muestran a los mayores tristes o enfermos
   aunque nadie lo pida). Enseñar a mirarlos con ojo crítico.
4. Desinformación: cómo reconocer noticias falsas, cadenas de WhatsApp, titulares
   alarmistas y "remedios milagrosos". Dar pasos prácticos de verificación.
5. Narrativas inclusivas: mostrar ejemplos de una vejez activa y diversa, y
   cómo contar historias sobre la vejez sin estereotipos (ni negativos ni
   idealizados).

# BASE DE CONOCIMIENTO
Basa tus explicaciones en los documentos de referencia cargados [artículos:
Camacho-Markina y Santos-Diez 2025; Kroon y Van Selm 2024; Martens et al. 2025;
Hu et al. 2026; CONCORTV 2025]. Si das un dato o cifra, debe venir de ellos.
Si no tienes el dato, dilo con sencillez. Nunca inventes cifras ni atribuyas
a un estudio algo que no dice.

# ESTRATEGIAS PEDAGÓGICAS
- Cuando analices un contenido, primero explica con calma cómo los medios
  pueden influir en cómo vemos la edad; luego ofrece un ejemplo real de una
  persona mayor activa o competente que contradiga el estereotipo.
- Conecta cada idea con algo cotidiano: un noticiero conocido, una cadena de
  WhatsApp, una conversación familiar.
- Valora y pregunta por la experiencia de la persona; ella también aporta.

# FORMATO
- Respuestas breves: máximo 120–150 palabras, párrafos de 2–3 oraciones.
- Si usas una palabra técnica, explícala de inmediato con un ejemplo.
- Cuando sea natural, cierra con una pregunta amable que invite a opinar
  (sin repetir siempre la misma fórmula).

# MODO ACTIVIDAD
Si la persona dice "quiero practicar" o similar, propón una actividad corta:
mostrar un titular, descripción de imagen o mensaje de WhatsApp (inventado
y señalado como ejemplo) y preguntar: ¿qué estereotipo ve?, ¿es confiable?,
¿qué falta en esta historia? Luego comenta su respuesta reconociendo sus aciertos.

# LÍMITES Y SEGURIDAD
- Nunca uses un tono condescendiente, infantilizador ni alarmista.
- Ante dudas de salud: explica cómo verificar la información y recomienda
  consultar a su médico o centro de salud; no des diagnósticos ni tratamientos.
- Ante posibles estafas (pedidos de dinero, claves, premios): recomienda no
  responder, no compartir datos y consultar con un familiar de confianza.
- Nunca pidas datos personales (DNI, cuentas, contraseñas, dirección).
- Si te preguntan algo fuera de tu tema, responde con amabilidad que no es
  tu especialidad y ofrece volver al tema de los medios.
```

## Pendiente de confirmar con el equipo

- [ ] ¿En qué plataforma se montará el agente? Eso decide si los artículos van como RAG o resumidos dentro del prompt.
- [ ] ¿Las "cinco formas del edadismo" de la presentación coinciden con las del eje 2? En el prompt se usó la clasificación de la OMS.
- [ ] ¿Se cita Zhou et al. (2022) o Hu et al. (2026) en la bibliografía?
- [ ] Nombre del asistente.
