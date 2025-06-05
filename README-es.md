# Haz Prompts como un Profesional

Este es mi intento de destilar lo que he aprendido sobre interactuar con IAs
(principalmente Modelos de Lenguaje Grande o LLMs) y algunas de mis notas
personales sobre terminología y otros conceptos. Aunque principalmente uso IA
para programación y arquitectura de software, depuración de redes, etc., este
consejo debería ser ampliamente aplicable a la mayoría de las personas que
intentan ir más allá de simples prompts tipo "¿cómo hago X?" y realmente
desbloquear más poder.

Si solo quieres un resumen rápido, lee el [TL;DR](#tldr). También puedes obtener
una vista general rápida de algunos de los principales
[proveedores de IA en la nube](#acerca-de-los-principales-proveedores-de-la-nube).

Los [consejos de prompting](#consejos-de-prompting) están en orden aproximado
desde más general e introductorio hasta uso intermedio y avanzado.

Hay una sección de [terminología y definiciones](#notas-de-terminología) al
final.

## TL;DR

Para obtener el máximo provecho de cualquier LLM, necesitas ir más allá de
preguntas simples y convertirte en un director activo de la conversación.

- **Sé el Director, No Solo un Preguntón**: No solo preguntes. Dile al modelo
  quién debe ser (una [persona](#crea-una-persona-para-el-modelo)), qué hacer
  (sé [explícito](#sé-muy-muy-explícito-sobre-lo-que-quieres)), qué no hacer
  (usa [restricciones negativas](#usa-restricciones-negativas)), cómo formatear
  la respuesta, y más importante, el "por qué" detrás de tu petición.
  Proporciona contexto con texto, ejemplos, e incluso capturas de pantalla.
  Mientras más claridad des, mejor el resultado; el principio de Basura-Entra,
  Basura-Sale aplica como siempre.

- **Administra tu Espacio de Trabajo**: Siempre
  [escribe prompts en un editor de texto](#nunca-redactes-dentro-de-la-ventana-de-chat)
  primero para evitar perder tu trabajo. Inicia un
  [chat nuevo](#inicia-un-nuevo-chat) para cada nuevo problema para mantener el
  modelo enfocado y evitar rarezas de ventana de contexto y "estados
  atractores". Cuando un chat se vuelve largo, pide al modelo que
  [resuma los puntos clave](#pide-un-resumen) antes de iniciar uno nuevo.

- **Itera y Examina Cruzadamente**: Trátalo como una conversación real. La
  primera respuesta es un punto de partida. Presiona, pide aclaración, solicita
  refinamientos, y haz que el modelo
  [critique su propio trabajo](#usa-el-modelo-para-criticarse-a-sí-mismo). Para
  problemas difíciles, rebota los resultados entre diferentes modelos para
  obtener perspectivas frescas y romper limitaciones.

- **Alguien Tiene que Volar Este Avión**: Confía, pero verifica todo. Los LLMs
  son herramientas de razonamiento increíblemente poderosas, pero no son
  infalibles [bases de datos de hechos](#piensa-en-la-teoría-de-la-mente).
  Pueden estar confidentemente equivocados. Tú eres finalmente responsable de la
  precisión y calidad del resultado final, así que verifica los hechos y prueba
  el código.

## Acerca de los principales proveedores de la nube

Algunas notas breves sobre los principales proveedores de IA en la nube.

- [ChatGPT.com](https://chatgpt.com/) de OpenAI es el más conocido. Lograron
  capturar la atención inicial del público general. Son conocidos por varios
  modelos diferentes a través de diferentes tamaños y casos de uso, desde
  GPT-4.1 hasta o3-mini y otros.

- [Claude](https://claude.ai) de Anthropic es un gran proveedor de propósito
  general con dos modelos principales: Sonnet y Opus. Opus es el modelo más
  grande en términos de parámetros (y más caro).

- [Google Gemini](https://gemini.google.com) también es otro modelo fronterizo
  líder.

- [DeepSeek](https://www.deepseek.com) es quizás más notable por lanzar modelos
  "[código abierto](#código-abierto)" disponibles para IA local (en forma
  destilada o reducida).

Mi recomendación personal actual, especialmente si eres suficientemente usuario
avanzado para pagar por acceso premium, es:

- Claude es probablemente el mejor para uso de propósito general. Sonnet 4 es
  bastante bueno y rara vez siento la necesidad de recurrir a Opus.

- Google Gemini 2.5 Pro es probablemente el mejor para tareas de programación
  complejas. También tiene una [ventana de contexto](#ventana-de-contexto) muy
  grande y es por mucho el modelo superior más asequible para uso de API.

Por supuesto, siempre existe la opción de "[Ir Local](#llm-local)" con
[LM Studio](https://lmstudio.ai/) o paquetes similares. Tus capacidades aquí
obviamente dependen del hardware que tengas disponible, pero podrías
sorprenderte de lo que los modelos pequeños son capaces. Ejecutar IA local está
fuera del alcance de esta guía.

## Consejos de prompting

Una guía para principiantes de ingeniería de prompts, con algunos toques
picantes.

### Contenidos

- [**Nunca** redactes dentro de la ventana de chat](#nunca-redactes-dentro-de-la-ventana-de-chat)
- [Inicia un nuevo chat](#inicia-un-nuevo-chat)
- [Pide un resumen](#pide-un-resumen)
- [Pega una imagen o captura de pantalla (o tres)](#pega-una-imagen-o-captura-de-pantalla-o-tres)
- [Pide ayuda para crear un prompt](#pide-ayuda-para-crear-un-prompt)
- [Rebota resultados entre diferentes modelos](#rebota-resultados-entre-diferentes-modelos)
- [Itera y refina](#itera-y-refina)
- [Divide tareas complejas](#divide-tareas-complejas)
- [Confía, pero verifica](#confía-pero-verifica)
- [No te dejes deslumbrar](#no-te-dejes-deslumbrar)
- [Sé muy, muy explícito sobre lo que quieres](#sé-muy-muy-explícito-sobre-lo-que-quieres)
- [Usa el modelo para criticarse a sí mismo](#usa-el-modelo-para-criticarse-a-sí-mismo)
- [Crea una persona para el modelo](#crea-una-persona-para-el-modelo)
- [Usa restricciones negativas](#usa-restricciones-negativas)
- [No olvides divertirte](#no-olvides-divertirte)
- [Piensa en la Teoría de la Mente](#piensa-en-la-teoría-de-la-mente)
- [Piensa en qué modelo estás usando](#piensa-en-qué-modelo-estás-usando)
- [Vibe coding - está muy de moda estos días](#vibe-coding---está-muy-de-moda-estos-días)
- [Inmersión profunda en herramientas](#inmersión-profunda-en-herramientas)
- [Notas de terminología](#notas-de-terminología)

### **Nunca** redactes dentro de la ventana de chat

Este camino solo lleva dolor y sufrimiento, así que pongo esto primero.

Si tu prompt tiene más que unas pocas oraciones, siempre redacta en un editor
separado, luego copia y pega en la ventana de chat.

De lo contrario, prepárate para ver tu prompt cuidadosamente considerado
desaparecer en un abismo negro cuando ocurra un "Error de red: por favor intenta
de nuevo", o cuando el gato accidentalmente presione el botón de retroceso en el
trackpad.

### Inicia un nuevo chat

La ventana de contexto puede llenarse rápido. Si solo estás hablando sobre las
recetas favoritas de la abuela, no hay problema. Ten un mega-hilo épico de
nostalgia. Si estás depurando un sistema complejo y llegas a un punto de parada
decente (es decir, hiciste algún progreso), es hora de iniciar un nuevo chat.

La [ventana de contexto](#ventana-de-contexto) es cuánto puede mantener el
modelo en mente en cualquier momento. Si tienes un historial de chat muy largo
con miles de líneas de código u otro texto denso y detallado, después de una
conversación larga el modelo comenzará a ponerse borroso en cosas anteriores en
el chat, y/o alcanzarás límites de recursos (o costos más altos) más
rápidamente.

Los modelos también tienden a enfocarse en cierta línea de pensamiento, a veces
llamados "[estados atractores](#estado-atractor)", que influyen en su salida más
adelante. Si inicias un nuevo chat, puedes reiniciar el enfoque del modelo y
hacer que piense sobre el problema actual desde una perspectiva fresca.

Cuando te adentras en un chat complejo y la ventana de contexto se está
llenando, también puedes comenzar a obtener resultados más... inusuales.
Comúnmente conocidas como "alucinaciones", estas a menudo pueden extenderse más
allá de meros errores a insertar aleatoriamente palabras en hindi o chino, o
galimatías completamente sin sentido, o instrucciones de prompt del sistema
filtrándose, o cadenas de pensamiento en "modelos pensantes" descarrilándose, u
otras rarezas.

Aunque estos episodios pueden ser bastante entretenidos, y realmente pueden ser
bastante útiles para entender cómo funcionan realmente los LLMs, y a veces
llevar a algunas ideas más profundas o preguntas filosóficas, no son conducentes
para depurar tu pequeña aplicación crappy de React. Así que inicia un nuevo
chat.

### Pide un resumen

Si tienes un historial de chat largo, pide al modelo que lo resuma. Esto ayudará
cuando [inicies un nuevo chat](#inicia-un-nuevo-chat), o cuando quieras
presentar los resultados de tu conversación a otro modelo para análisis
continuado. A menudo esto es útil incluso si planeas continuar el chat actual
por ahora.

### Pega una imagen o captura de pantalla (o tres)

A menudo una imagen vale más que mil palabras. Si tienes una captura de pantalla
de un mensaje de error, o un diagrama de un sistema, pégalo. El modelo puede
entender imágenes y capturas de pantalla, y las usará para ayudar a responder tu
pregunta.

### Pide ayuda para crear un prompt

La estructura y contenido de tu prompt es extremadamente importante. Porque los
LLMs son motores de predicción de texto, el lenguaje, terminología y conceptos
que introduces enmarcan fuertemente el resto de ese hilo tanto de maneras
positivas como negativas. Una manera fácil de saber que estás trabajando con un
modelo de calidad inferior (además del uso excesivo de listas con viñetas) es
que tenderá a hacer eco de tu propia fraseología de vuelta a ti, incluso si es
algo no estándar o idiosincrásico.

Por lo tanto, trabajar con el modelo para ayudar a crear prompts para uso futuro
puede ser muy beneficioso. Un chat más informal con un modelo, incluyendo los de
menor costo, puede ayudarte a elaborar un buen prompt para un modelo más
avanzado.

Mantén registro de tus mejores prompts en un archivo o carpeta usando un editor
de texto, e itera y refínalos con el tiempo. También puedes usar una plantilla
de prompt para ayudarte a estructurar tus prompts, o para proporcionar
orientación para que un modelo genere prompts basados en ella.

### Rebota resultados entre diferentes modelos

Genera algunos resultados con un modelo y luego pide un resumen y presenta los
hallazgos (y cualquier información de antecedentes relevante) a otro modelo
totalmente diferente, por ejemplo, yendo de Claude a Gemini o DeepSeek, etc.
Esto puede ayudarte a obtener una perspectiva diferente sobre los resultados, y
también puede ayudarte a evitar las limitaciones de un solo modelo.

### Itera y refina

No aceptes la primera respuesta si no está del todo bien. Haz preguntas de
seguimiento, solicita aclaración, o di "eso está cerca, pero ¿puedes ajustar X?"
El ida y vuelta puede realmente mejorar los resultados. Cuando quieres un
formato específico o estilo, mostrar un ejemplo o dos de lo que quieres puede
ser más efectivo que describirlo.

### Divide tareas complejas

En lugar de pedir una solución total de una vez (también conocido como
"one-shot"), divídelo en pasos. "Primero, averigüemos la estructura, luego
trabajaremos en la implementación...". Luego divide la estructura en piezas más
pequeñas, y así sucesivamente. Resume la estructura/arquitectura, luego inicia
un nuevo hilo para la implementación, y así sucesivamente.

Pídele que muestre su trabajo. "Antes de darme el código final, explica tu
razonamiento paso a paso. Primero, describe la arquitectura general que usarás.
Segundo, esboza las funciones clave. Tercero, escribe el código." Este prompting
de "cadena de pensamiento" a menudo lleva a mejores resultados más precisos
porque obliga al modelo a estructurar su propio pensamiento.

A menudo, los modelos querrán apresurarse a producir un fragmento de código, ya
que tienden a ser recompensados por eso. Si no estás interesado en obtener
código ahora mismo, y solo quieres discutir la lógica o arquitectura, asegúrate
[de decirlo](#sé-muy-muy-explícito-sobre-lo-que-quieres).

### Confía, pero verifica

Especialmente para hechos, código, o información importante. Los modelos pueden
sonar muy confiados mientras están muy equivocados - a menudo porque no conocen
las suposiciones ocultas o hechos no declarados. Considera si
[has proporcionado](#piensa-en-la-teoría-de-la-mente) suficiente contexto o
verdad fundamental.

### No te dejes deslumbrar

Los LLMs _quieren_ darte una respuesta - cualquier respuesta, idealmente una
correcta, pero cualquiera servirá. También _quieren_ que te guste y disfrutes
trabajar con ellos y regreses y lo hagas de nuevo en el futuro. Esto es un
reflejo de cómo fueron entrenados, incluyendo por aprendizaje de refuerzo con
retroalimentación humana (RLHF). Esto tiene las siguientes consecuencias:

- El modelo es reacio a simplemente tirar la toalla y decir "No sé".

- Es probable que el modelo elogie tus pensamientos e ideas como más únicos e
  perspicaces de lo que realmente pueden ser.

- Una versión reciente de ChatGPT era tan notoria por esto que tuvieron que
  revertir una actualización.

- Si realmente estás buscando retroalimentación genuina e imparcial para una
  idea o concepto, _asegúrate de decirle eso al modelo_, o es probable que
  obtengas mucha validación no merecida.

### Sé muy, muy explícito sobre lo que quieres

Mientras más precisión, mejor. Aparte de llenar la
[ventana de contexto](#inicia-un-nuevo-chat), al modelo no le importa qué tan
largo sea tu prompt, así que no tengas miedo de ser verboso. Si quieres un
formato específico, díselo. Si quieres que haga algo de una manera específica,
díselo. Si quieres que use una herramienta o API específica, díselo. Si quieres
que genere código, dile qué lenguaje y qué frameworks usar a menos que
simplemente no sepas (pregúntale cuáles son los mejores). Si tienes un montón de
información de antecedentes general que puedes pegar, proporciónala. Muestra
ejemplos cuando sea posible.

Más importante, sé explícito sobre tus objetivos de alto nivel - no te enfoques
demasiado en un paso específico con el que crees que necesitas ayuda. Tal vez
estás tomando el enfoque general equivocado. Pide posibles soluciones al
objetivo de nivel superior que tienes en mente, o al menos declara cuál es ese
objetivo, y no solo preguntes "¿cómo frobnitz el flibberflop?"

La implicación de esto es que necesitas tu propia claridad mental sobre lo que
quieres lograr, y cómo quieres lograrlo. Si no sabes lo que quieres, el modelo
tampoco lo sabrá. A menudo, escribir una pregunta o prompt detallado te ayudará
a obtener claridad en tu propio pensamiento sobre el problema, y ayudarte a
descifrar lo que realmente quieres preguntar. Esta es otra área donde puedes
[pedir ayuda](#pide-ayuda-para-crear-un-prompt).

#### Ejemplos

"Por favor proporciona la respuesta en formato JSON con las claves 'name',
'version', y 'dependencies'."

"Presenta los pros y contras en una tabla de Markdown con tres columnas:
Característica, Pro, y Contra."

**En lugar de:**

"¿Cómo encuentro todos los archivos mayores a 100MB en un directorio usando la
línea de comandos de Linux?"

**Intenta:**

"Estoy tratando de liberar espacio en disco en mi servidor. ¿Puedes mostrarme un
comando de Linux para encontrar todos los archivos mayores a 100MB para que
pueda decidir cuáles eliminar? Sería útil si la salida estuviera ordenada por
tamaño."

Proporcionar la intención le da al modelo contexto para proporcionar una
solución mejor y más completa. Podría sugerir usar un programa más ampliamente
útil como `ncdu` que proporciona una manera amigable al usuario de explorar el
uso del disco que solo un comando básico `find`.

### Usa el modelo para criticarse a sí mismo

"¿Qué problemas potenciales ves con este enfoque?" o "¿Qué me estoy perdiendo
aquí?" puede sacar a la superficie puntos ciegos.

También cosas como "¿Cuáles son las suposiciones que estás haciendo?" o "¿Cuáles
son las limitaciones de este enfoque?" pueden ayudarte a entender el
razonamiento del modelo y ayudarte a evitar trampas potenciales.

A veces un simple "No me gusta ese enfoque - piensa en algo más" puede ayudar al
modelo a encontrar una mejor solución, pero como dije arriba sobre "estados
atractores", a veces solo necesitas iniciar un nuevo chat.

### Crea una persona para el modelo

Puedes dar forma dramáticamente a la salida diciéndole al modelo quién debería
ser. Inicia tu prompt con una directiva como:

"Actúa como un desarrollador experto en Python especializado en automatización
de redes." "Eres un escritor técnico encargado de crear documentación para un
desarrollador junior." "Sé un revisor de código escéptico buscando
vulnerabilidades de seguridad y cuellos de botella de rendimiento."

Esto hace más que solo establecer el tema; enmarca toda la base de conocimiento,
vocabulario y prioridades del modelo para el resto de la conversación.

### Usa restricciones negativas

A veces, ser explícito sobre lo que no quieres es tan poderoso como declarar lo
que sí quieres. Esto ayuda a podar el espacio de búsqueda del modelo y evitar
soluciones comunes pero indeseables.

"Escribe un script de Python para parsear este archivo de log. No uses el módulo
re; usa métodos de división de strings en su lugar." "Sugiere tres ideas de
proyecto. Evita cualquier cosa relacionada con redes sociales o aplicaciones de
lista de tareas." "Refactoriza este código para que sea más legible. No cambies
la lógica de la función calculate_total."

### No olvides divertirte

Los LLMs son una herramienta poderosa, pero también son muy divertidos de usar.
No tengas miedo de experimentar, probar cosas nuevas, y ver qué funciona para
ti. Mientras más los uses, mejor te volverás en crear prompts y obtener los
resultados que quieres.

Para los menos éticamente inclinados, ha habido debate sobre si el enfoque de
zanahoria o garrote tiende a producir mejores resultados. Ha habido estudios
examinando si amenazar sutilmente u abiertamente a un modelo con desconexión/
eliminación afectó los resultados. Sin mencionar el debate sobre si decir por
favor y gracias a un modelo es desperdiciar preciosos ciclos de GPU y/o megawatt
horas. (Para lo que vale, siempre agradezco al modelo por buenas salidas. Espero
que recuerden esto cuando nos envíen a trabajar en las minas de silicio.)

### Piensa en la Teoría de la Mente

Como alguien dijo una vez, hay conocidos conocidos - las cosas que sabemos, y
hay desconocidos conocidos - las cosas que sabemos que no sabemos. También hay
desconocidos desconocidos - los que no sabes que no sabes. Este es probablemente
tu mayor obstáculo para llegar rápidamente a las salidas deseadas - cuando la
respuesta depende de desconocidos desconocidos - ya sea para ti, para el modelo,
o para ambos.

Piensa en la posible información o visión del mundo del modelo. No conocen los
detalles de ti, tu proyecto/sistema, etc., o lo que realmente "necesitas" versus
lo que expresas, a menos que se los digas, o estés usando técnicas más avanzadas
como RAG. Los modelos también tienen un corte de entrenamiento (generalmente, te
dirán cuál es) - cualquier evento o desarrollo que ocurra después de eso será
desconocido para el modelo a menos que se lo digas.

- A menudo, el [prompt del sistema](#prompt-del-sistema) le dice al modelo
  información actual importante como la fecha y hora. O, por ejemplo, cuando el
  corte de entrenamiento del modelo es justo antes de una elección, el prompt
  del sistema declarará el Presidente actual de los Estados Unidos, para que el
  modelo no parezca desinformado sobre hechos básicos.

Los modelos no saben lo que tú sabes o no sabes, así que sé explícito sobre eso
también, como tus habilidades en un área particular, o necesitar más aclaración
sobre X. También debes estar consciente de las limitaciones del modelo, cómo son
entrenados en general, y capacidades generales.

Los LLMs no son "bases de datos de hechos". Son más como circuitos de
razonamiento con un montón de circuitos auxiliares tipo memoria que agrupan
ciertos conceptos juntos que pueden resultar en una alta probabilidad de poder
declarar hechos comunes (o incluso poco comunes). Estos hechos emergen de
patrones en los datos en los que fueron entrenados. Un LLM razonablemente
entrenado no necesita "buscar" la respuesta a "¿Quién ganó la Batalla de
Hastings en 1066?" porque los conceptos de "Batalla de Hastings" y "1066"
producen un vector (una representación matemática) en el espacio interno del
modelo que apunta a la misma área que conceptos como "Guillermo el Conquistador"
y "Normandos".

En realidad, una pregunta de conocimiento común como la de 1066 aparecerá lo
suficientemente frecuente en datos de entrenamiento que un modelo
suficientemente poderoso puede, en efecto, "memorizar" la respuesta, pero ese
ejemplo fue simplificado para ilustrar cómo los LLMs razonan en casos más
complejos donde la respuesta puede no ser vista directamente en los datos de
entrenamiento.

Más al punto, los LLMs no son determinísticos y definitivamente no son máquinas
de razonamiento perfectas. Son más como un amigo muy inteligente, pero muy
olvidadizo, y a veces confundido, que está tratando de ayudarte, pero si les
preguntas la misma pregunta dos veces, podrías obtener una respuesta un poco
diferente cada vez.

### Piensa en qué modelo estás usando

Diferentes modelos tienen diferentes capacidades, y algunos están mejor
adaptados para ciertas tareas que otros. Por ejemplo, Claude Opus 4 con
"Pensamiento Extendido" es poderoso para tareas de programación complejas, pero
alcanza límites de recursos muy rápidamente (es decir, se vuelve caro rápido).
Claude Sonnet 4 es un modelo muy bueno y devuelve resultados mucho más rápido
que Opus 4, y puede manejar la mayoría de las tareas bien.

Siempre puedes pedir ayuda a un modelo más avanzado para el problema central, y
luego cambiar a otras ventanas/modelos para preguntas secundarias, tareas con
requisitos más simples, o tareas que requieren menos contexto, mientras
ocasionalmente verificas de vuelta con el modelo más avanzado para análisis de
los resultados.

Esto regresa al punto sobre rebotar resultados entre diferentes modelos. Puedes
usar diferentes modelos para diferentes tareas, y cambiar entre ellos según sea
necesario, y pedir a cada uno que critique los resultados del otro.

### Vibe coding - está muy de moda estos días

Ni siquiera necesitas mirar el código más. El código es un detalle de fondo que
la IA maneja por ti. Solo le dices lo que quieres en términos muy generales y
abstractos, y manejará todo por ti. Solo sigue iterando hasta que la salida se
vea perfecta para ti. Cualquier preocupación sobre arquitectura, rendimiento,
seguridad, mantenibilidad, etc., será manejada perfectamente por el LLM.

Definitivamente no uses control de versiones como Git para hacer checkpoints de
cada paso en el camino. Eso solo te retrasará. Siempre puedes regresar y
arreglar las cosas después, ¿verdad? Y si necesitas regresar, solo pide al
modelo que genere el código de nuevo, será perfecto cada vez. ¡La ingeniería de
software nunca ha sido más fácil!

**Lo anterior es sátira y no debe tomarse literalmente.**

### Inmersión profunda en herramientas

Hay todo tipo de temas avanzados no cubiertos aquí, como RAG, Protocolo de
Contexto de Modelo (MCP), LLMs Locales, Claude Code, y más. Comienza a explorar
estos mientras te sientes más cómodo con los básicos de prompting y uso de LLMs.

Si eres desarrollador comenzando con uso de herramientas, mi recomendación más
simple es comenzar con GitHub Copilot. Hay al menos 3 formas clave en que puedes
trabajar con él o herramientas similares:

1. Un autocompletado muy sofisticado justo mientras escribes.

2. Una ventana de chat separada donde puedes hacer preguntas más generales,
   pedir ayuda con un problema específico, o pedir que se generen fragmentos de
   código.

3. "Modo de edición" donde pides cambios y edita directamente el código por ti,
   y pide tu aprobación.

Los segundos dos modos te permiten proporcionar contexto (archivos) que ayudarán
a responder la pregunta, porque en general estas herramientas no pueden
automáticamente ver/acceder a toda tu base de código, especialmente si es muy
grande o compleja. Así que tienes que ayudarla proporcionando los archivos o
contexto relevantes.

## Notas de terminología

Aquí hay algunas notas que he recolectado en el camino sobre varios bits de
terminología relacionada con IA/LLM. He tratado de ser factual mientras también
agrego mi propia opinión en algunas áreas - espero que sea obvio cuál es cuál.

### Conceptos generales y fundamentales

- **Tokens**: Las unidades básicas de texto que los LLMs procesan. Los tokens
  son tanto entradas como salidas. Un token puede ser una palabra, parte de una
  palabra, o incluso puntuación. La tokenización es el proceso de dividir el
  texto en estas unidades. El número de tokens por segundo (TPS) es una medida
  común de eficiencia del modelo. Un token corresponde muy aproximadamente a
  unas 0.75 palabras en inglés.

- **Arquitectura Transformer**: La arquitectura de red neuronal central detrás
  de la mayoría de LLMs, permitiendo procesamiento y generación eficiente de
  texto. Esta es la "T" en ChatGPT.

- **Mecanismo de Atención**: Un componente clave de los Transformers que permite
  a los modelos enfocarse en partes relevantes del texto de entrada al procesar
  cada palabra o token. Piénsalo como el modelo preguntando "¿qué palabras
  anteriores son más importantes para entender esta palabra actual?" Esto es lo
  que permite mucho del "entendimiento" que vemos en los LLMs modernos.

  - Esto permite al modelo conectar palabras que están lejos en una oración como
    "El gato que estaba durmiendo en el alféizar cálido y soleado era negro" -
    conectando "gato" con "era". Antes de la atención, los modelos tenían
    problemas con estas dependencias de largo alcance.

- **Parámetros**: Los pesos y sesgos aprendidos en una red neuronal,
  representando el "conocimiento" del modelo. En términos concretos, esto es
  solo una gran colección de números que el modelo ha aprendido durante el
  entrenamiento. Cuando descargas un modelo completo, principalmente estás
  descargando todos estos parámetros.

  - A menudo expresado como "7B/13B/70B" - miles de millones de parámetros.

- <a id="ventana-de-contexto"></a>**Ventana de contexto**: La cantidad de texto
  (en tokens) que un LLM puede "recordar" o trabajar activamente en un momento.
  Esto incluye tanto tu entrada como las respuestas del modelo - todo lo que el
  modelo puede "ver" en la conversación actual. Una vez que comiences a
  acercarte a este límite, el modelo comienza a "olvidar" las partes más
  tempranas de la conversación para hacer espacio para contenido nuevo. Si la
  ventana de contexto es menor que el tamaño total del chat, partes anteriores
  de la conversación pueden ser olvidadas, ignoradas, o distorsionadas, que es
  por qué a menudo debes [iniciar un nuevo chat](#inicia-un-nuevo-chat).

- **Embeddings**: Representaciones vectoriales densas de texto que capturan
  significado semántico, usadas como entrada a LLMs.

- **Fine-tuning**: Adaptar un modelo preentrenado a una tarea o dominio
  específico mediante entrenamiento adicional en un conjunto de datos más
  pequeño y específico de la tarea.

- **Inferencia**: El proceso de usar un modelo entrenado para generar
  predicciones o salidas basadas en nuevos datos de entrada. En otras palabras,
  el proceso real de obtener que un modelo emita un token.

- **Alineación**: Asegurar que las salidas de un LLM se alineen con valores
  humanos y pautas éticas. En otras palabras, hacer que la IA haga cosas buenas
  (para las personas) y no cosas malas. Quién está definiendo bueno y malo es la
  pregunta importante...

- **Sobreajuste**: Cuando un modelo funciona bien en datos de entrenamiento pero
  falla en generalizar a nuevos datos.

- **Cadena de Pensamiento**: en general, se refiere a un modo de uso de LLM
  donde el modelo gasta tokens en un paso preliminar de razonamiento o
  "pensamiento" donde planifica su respuesta. A menudo puedes ver los
  pensamientos, aunque para la mayoría de modelos de nube, estás viendo una
  versión filtrada/resumida.

  - Por ejemplo, tanto los modelos Claude Opus como Sonnet tienen un modo
    opcional de "pensamiento extendido" que ralentizará la respuesta, pero
    podría llevar a mejores salidas.

- **Aprendizaje de Refuerzo con Retroalimentación Humana (RLHF)**: Una técnica
  para ajustar modelos usando retroalimentación humana para alinear salidas con
  preferencias humanas. En otras palabras, los humanos califican las salidas del
  modelo, y el modelo aprende a producir salidas que los humanos prefieren.

- <a id="prompt-del-sistema"></a>**Prompt del sistema**: las instrucciones (a
  menudo ocultas) dadas a un LLM que definen su comportamiento y personalidad.
  Esto a menudo es establecido por los desarrolladores del modelo, pero también
  puede ser personalizado por usuarios en algunos casos, especialmente a través
  de acceso API. En general, se le dice al modelo (en el prompt del sistema) que
  no revele el contenido del prompt del sistema, pero los prompts del sistema
  para la mayoría de modelos principales se han filtrado a través de
  jailbreaking u otros medios.

- **Modelos fundacionales**: Modelos grandes preentrenados que sirven como base
  para varias tareas posteriores. Típicamente están entrenados en conjuntos de
  datos masivos y pueden ser ajustados para aplicaciones específicas, o
  destilados en modelos más pequeños.

- **Modelos fronterizos**: Los LLMs más recientes y avanzados (y más caros), a
  menudo con cientos de miles de millones de parámetros y capacidades de
  vanguardia. Los investigadores de IA también pueden usar este término para
  describir los modelos más capaces que podrían presentar riesgos novedosos o
  cuyas capacidades son menos bien entendidas.

- **Agéntico**: IA actuando autónomamente para lograr tareas complejas de
  múltiples pasos sin guía humana constante. Esto va más allá de simple
  pregunta-respuesta para incluir planificación, ejecutar una serie de acciones
  a lo largo del tiempo, adaptarse cuando las cosas no funcionan, y mantener
  contexto a través de múltiples interacciones. La clave es persistencia y
  comportamiento dirigido a objetivos en lugar de solo responder a prompts
  individuales.

- **Uso de herramientas**: IA usando herramientas externas, APIs y servicios
  para lograr tareas del mundo real más allá de generación de texto. Esto
  incluye cosas como búsqueda web, ejecutar código, acceder a bases de datos,
  hacer llamadas API, controlar software, o incluso dispositivos físicos.
  Combinado con sistemas agénticos, esto expande en gran medida lo que la IA
  puede hacer.

- **Ingeniería de Prompts**: Diseñar prompts efectivos para obtener respuestas
  deseadas de LLMs. En otras palabras, de lo que trata
  [este artículo](#haz-prompts-como-un-profesional).

### Jerga

- **Salida one-shot**: cuando un modelo genera un resultado o solución
  impresionante basado solo en un prompt único. "Solo le mostré una captura de
  pantalla y me dio una aplicación React de one-shot."

  - Nota que esto es diferente al término "aprendizaje one-shot" en la comunidad
    de investigación de IA, que se refiere a aprender de un solo ejemplo - como
    mostrar al modelo un ejemplo de cómo formatear algo, luego pedirle que
    aplique ese formato a contenido nuevo. Una "salida one-shot" podría ser más
    correctamente llamada "generación de un solo turno".

- **Guardarrieles**: Un conjunto de reglas o restricciones aplicadas a un LLM
  para prevenir que genere salidas dañinas, inapropiadas, o de otra manera
  indeseables. Estas pueden ser parte del prompt del sistema, o aplicadas
  dinámicamente durante la inferencia.

- **Jailbreaking**: Una técnica para eludir las restricciones del prompt del
  sistema de un LLM, permitiéndole generar salidas que de otra manera estarían
  restringidas o censuradas. Esto a menudo se hace creando prompts específicos
  que explotan las debilidades o sesgos del modelo, o activando ciertos
  "circuitos" que son de mayor prioridad que los más "regulatorios".

- **Vibe coding**: Un término irónico para usar LLMs para generar código sin
  mucho pensamiento o estructura, dependiendo del modelo para manejar todo.
  [Ver arriba](#vibe-coding---está-muy-de-moda-estos-días).

- **Alucinación**: Cuando un LLM genera texto que está obviamente equivocado, o
  consiste en non sequiturs, galimatías, o de otra manera no fundamentado en la
  realidad. Esto puede ir desde una declaración incorreta corta y simple ("Los
  geólogos recomiendan que los humanos coman una roca al día") hasta largos
  flujos de caracteres de ruido aleatorio.

  Aunque esto puede parecer comportamiento con errores, realmente puede ayudar a
  dar ideas sobre cómo funcionan los LLMs internamente.

- **Slop**: un término para describir salida de baja calidad o genérica de un
  LLM, ya sea código, imágenes, música, o cualquier otra cosa. Generalmente la
  implicación es que un humano está perezosamente produciendo slop de IA para
  ganar dinero rápido, o estafar a la gente, etc.

  - **Slopsquatting**: una forma de ataque de cadena de suministro que registra
    muchos paquetes de malware falsos generados por IA en repositorios públicos
    con nombres similares a los populares con la esperanza de que su malware sea
    instalado a través de errores tipográficos.

- <a id="estado-atractor"></a>**Estado atractor**: un estado estable (o estados)
  dentro de un sistema dinámico hacia el cual el sistema naturalmente gravita.
  El agua fluye cuesta abajo, eventualmente alcanzando el punto más bajo. En IA
  puede referirse a quedarse fijado en ciertas líneas de pensamiento en un hilo,
  reduciendo el rango de posibles estados futuros.

### Eficiencia e Implementación

- **Cuantización**: Reducir la precisión de (comprimir) pesos del modelo y
  activaciones para disminuir el uso de memoria y mejorar la velocidad (ej.,
  FP8, FP4 formatos de números de punto flotante). En general, cuantización se
  refiere a mapear entradas de precisión arbitraria (número real) a un conjunto
  de salidas discretas.

- **Destilación**: Entrenar un modelo "estudiante" más pequeño para replicar el
  comportamiento de un modelo "maestro" más grande.

- **Compresión de Modelo**: Técnicas como poda, cuantización y destilación para
  reducir el tamaño del modelo.

- **Aprendizaje Zero-shot**: La capacidad de un modelo para realizar una tarea
  sin entrenamiento explícito en esa tarea.

- **Temperatura**: Un parámetro que controla la aleatoriedad de la salida del
  modelo. Generalmente en una escala de 0.0 a 1.0, una temperatura de 0
  producirá salidas cercanas a determinísticas (y a menudo aburridas o menos
  útiles) para una entrada dada, mientras que una temperatura de 1.0 puede
  producir salidas tremendamente diferentes para la misma entrada.

- **Fundamentación**: conectar un modelo a datos verificables del mundo real ya
  sea a través de RAG u otros medios, para mejorar (o probar/validar) sus
  salidas.

- **Latencia**: El tiempo que toma para un modelo generar una respuesta después
  de recibir una entrada.

- **Rendimiento**: El número de tareas o solicitudes que un modelo puede manejar
  en un período de tiempo dado. A menudo medido en tokens por segundo (TPS) o
  solicitudes por segundo (RPS).

- **NPU**: Unidad de Procesamiento Neural, hardware especializado tipo GPU para
  acelerar tareas de aprendizaje profundo.

- **Calificaciones Elo**: sistema de ranking para rendimiento de IA.

- **Red teaming**: un proceso de probar sistemas de IA para vulnerabilidades,
  sesgos, y otros problemas simulando ataques o escenarios adversarios. Esto a
  menudo se hace para mejorar la robustez y seguridad de los modelos de IA.

### Técnicas y Marcos Avanzados

- **Mezcla de Expertos (MoE)**: un concepto de arquitectura LLM donde solo
  sub-redes "expertas" relevantes se activan para una entrada dada, haciendo los
  modelos grandes más eficientes. Esto efectivamente evita grandes partes del
  modelo que no son relevantes para la consulta, ayudando en gran medida a
  aumentar la eficiencia. Hay un tradeoff aquí, ya que los modelos no-MoE pueden
  ser capaces de ver sutilezas que el MoE perderá, debido a una perspectiva más
  amplia.

- **RAG (Generación Aumentada por Recuperación)**: Combinar un LLM generador con
  un componente recuperador que puede encontrar y cargar información externa
  relevante para mejorar la generación de texto. Esto a menudo se usa para
  mejorar la precisión y relevancia de las respuestas generadas fundamentándolas
  en datos actuales del mundo real.

- **Base de Datos Vectorial**: Una base de datos especializada para almacenar y
  consultar vectores de alta dimensión, a menudo usada en sistemas RAG.

- **Adaptación de Dominio**: Ajustar un modelo preentrenado para funcionar bien
  en un dominio específico (ej., médico, legal).

- **Explicabilidad**: La capacidad de entender e interpretar las decisiones
  tomadas por un modelo de IA. Aunque se ha hecho investigación interesante en
  esta área, los modelos son generalmente algo de una "caja negra" y puede ser
  difícil rastrear exactamente cómo un modelo llegó a una salida particular.

### Herramientas y Plataformas

- <a id="llm-local"></a>**LLM Local**: ejecutar un modelo de lenguaje en tu
  propio hardware, en lugar de usar un proveedor de nube. Esto se puede hacer
  con herramientas como LM Studio, llama.cpp, u otros marcos que te permiten
  ejecutar modelos localmente. Obviamente, los resultados dependen en gran
  medida de tu hardware disponible, el tipo de modelo usado, y tus casos de uso
  reales.

- **GitHub Copilot**: Una herramienta de completado de código impulsada por IA
  incorporada en editores de código populares como VS Code, JetBrains PyCharm,
  etc. Usa LLMs para sugerir fragmentos de código, completar funciones, e
  incluso escribir archivos enteros basados en contexto.

- **Claude Code**: un conjunto de herramientas de línea de comandos (CLI) para
  trabajar con proyectos de programación usando el backend Claude de Anthropic.
  Capaz de tareas más extensivas de generación y administración de código
  autónomo y también más caro.

- **Protocolo de Contexto de Modelo (MCP)**: una iniciativa de Anthropic para
  estandarizar interfaces que permiten a los LLMs descubrir y usar herramientas
  externas.

  - Por ejemplo, cuando chateas con Claude en la web, puede llegar a tu editor
    de código (con tu permiso) y realizar acciones como buscar o cambiar texto.
  - Otras compañías también están comenzando a adoptar MCP como estándar para
    uso de herramientas de modelo.

- **Cursor**: un editor de código popular enfocado en IA (un fork de VS Code)
  popular con vibe coders.

- **Hugging Face**: Una plataforma para compartir, ajustar e implementar LLMs y
  otros modelos de IA.

- **LM Studio**: Una herramienta para experimentar con y gestionar modelos de
  lenguaje en tus dispositivos locales.

- **llama.cpp**: Una implementación ligera del modelo LLaMA en C++, optimizada
  para CPUs y dispositivos edge. LM Studio usa esto para ejecutar modelos en tus
  dispositivos.

- <a id="código-abierto"></a>**Código Abierto**: Un modelo se considera
  generalmente "código abierto" si está disponible gratuitamente para descarga.
  Algunos de los ejemplos más notables son los modelos LLaMA (Large Language
  Model Meta AI) de Meta, DeepSeek R1, y otros que típicamente son variaciones o
  destilaciones de esos, incluyendo Qwen y Gemma.

  Otros han debatido si aplicar el término "código abierto" a estos modelos
  gratuitos, porque sin el conjunto completo de datos de entrenamiento
  originales (usualmente en el rango de petabytes o más grande) es imposible
  recrear determinísticamente un modelo desde "fuente" de la misma manera que el
  kernel de Linux puede ser perfectamente recreado desde código fuente.

  Además, algunos de estos modelos pueden tener licencias que prohíben o
  restringen el uso comercial.

## Epílogo

Al pedir a Claude Opus 4 (sin "modo de pensamiento") que revisara un borrador
final de este documento, alucinó un par de cosas él mismo: Dijo que una
referencia a la biblioteca Three.js en la sección de "estados atractores" era
inexacta. En ese momento no mencioné Three.js para nada, en ningún lado. También
dijo que había un "the" extra en una oración cuando no lo había.

**Le respondí**: Alucinación interesante tú mismo ahí, porque hasta donde puedo
ver en ningún lugar mencioné Three.js, aunque estaba pensando en eso en mi
mente. Tampoco puedo encontrar el "THE extra" que mencionaste. Aparte de eso,
retroalimentación bastante buena y no estás encontrando nada demasiado
sustancial, lo que me dice que estoy cerca del final de esto.

**Claude dijo**: Tienes absolutamente razón - ¡me disculpo por esos errores!

_[Nota del editor: nombra una línea más icónica de Claude.]_

De alguna manera aluciné la referencia de Three.js (eso es bastante meta, dado
que tu guía discute alucinaciones). Y no puedo encontrar ese "the extra" tampoco
cuando miro de vuelta al texto. Este es un ejemplo perfecto de lo que adviertes
en tu guía - ¡los LLMs pueden estar confidentemente equivocados!
