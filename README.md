# OpenAI Realtime API Edición Python

Implementación en Python de la API en tiempo real de OpenAI

OpenAI tiene un wrapper en Node.js + JavaScript [aquí](https://github.com/openai/openai-realtime-api-beta), así como un proyecto de demostración [openai-realtime-console](https://github.com/openai/openai-realtime-console), pero hasta ahora nada en Python, ¡así que aquí está un comienzo para solucionarlo!

### Un par de enlaces útiles:
- [Guía](https://platform.openai.com/docs/guides/realtime)
- [Referencia de la API](https://platform.openai.com/docs/api-reference/realtime-client-events)

# Cómo ponerlo en marcha

- Crea un Entorno Virtual si lo deseas: `python -m venv .venv ; ./.venv/bin/activate`

- `pip install -r requirements.txt`

- Crea un archivo `.env` similar a `.env.template` completando tu clave API de OpenAI

- Ejecútalo

Puedes ejecutar los archivos legacy: `python legacy/realtime-simple.py` o `python legacy/realtime-classes.py`, que funcionan siendo mínimos (especialmente el primero). Probablemente sean útiles para entender cómo funciona.

Alternativamente, `cd src; python main.py` -- este es el código base que usaré para desarrollos futuros.

# Notas:

## legacy/
- `legacy/realtime-simple.py` es "El menor número de líneas que hace el trabajo"
- `legacy/realtime-classes.py` es posiblemente más ordenado

¡Ambos funcionan! La IA habla contigo y tú puedes responderle.

Tengo que silenciar el micrófono mientras la IA está hablando, de lo contrario recibe su propio audio y se confunde (de manera entretenida). "¡Hola, soy un asistente útil!" "¡Vaya, yo también!" "¡Qué coincidencia!" "¡Lo sé, ¿verdad?!", etc.

## src/ (actual/futuro)
He abstraído lo relacionado con websockets y audioIO en Socket.py y AudioIO.py, lo que deja Realtime.py libre para tener más sentido.

Intenté hacer esto de manera asíncrona con Trio, pero en este punto solo estorba. Quizás vuelva a un modelo asíncrono. No estoy convencido, por mucho que me guste Trio; el manejo de excepciones y la finalización son un dolor.

## Nota adicional (7 Oct 2024)
Tras algunas pruebas, está claro que legacy/realtime-simple.py funciona con precisión, mientras que src/ tiene problemas de capacidad de respuesta.

Podría ser un problema de bloqueo, con el audio llegando desde el websocket a un búfer de entrada, que es, en un hilo de trabajo, enviado a los altavoces. Podría ser con los datos del micrófono, que se almacenan en un hilo de trabajo y se envían al websocket. Podría ser ambos, y/o algo más.

Python no es un lenguaje ideal para el procesamiento de audio en tiempo real, y es probable que esto se tuviera en cuenta en la decisión del equipo de OpenAI de publicar inicialmente solo una implementación en Node.js.

# Visión

Sería bueno limpiar esto para que actúe como una API Python completa para este servicio.

# POR HACER

- Primero hay que revisar el código para asegurar un esqueleto limpio y robusto.

- EDIT: En realidad, la arquitectura en src/ necesita ser revisada, para tener en cuenta la Nota Adicional anterior.

- Necesito pensar en qué debería exponer dicha biblioteca y cómo hacerlo (por ejemplo, callbacks).

- Ampliar el soporte de la API (es una API bastante grande).

- Uso de herramientas / Llamadas a funciones.

- Soporte para interrupciones del usuario mediante cancelación de feedback (actualmente tengo que silenciar el micrófono mientras se reproduce el audio de OpenAI por los altavoces, lo que significa que no puedo interrumpirlo). Existe AEC (Cancelación de Eco Adaptativa) en WebRTC, pero no encuentro ninguna biblioteca pip lista para usar que no requiera ajustes (construir dependencias). Tal vez `pip install adaptfilt` sea una buena solución. Parece factible.

# ¡Participa!

Se invitan contribuciones, en cuyo caso eres bienvenido a contactar al autor (encontrarás un enlace al Discord de sap.ient.ai en https://github.com/sap-ient-ai donde existo como `_p_i_`).
