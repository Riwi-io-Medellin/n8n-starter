# Ejercicio de práctica: Clasificador de correos (Gmail + n8n)

Este ejercicio replica (a nivel de texto y paso a paso) lo mostrado en el video: crear un clasificador de correos que, al llegar un email a Gmail, lo clasifique en una categoría y mueva el mensaje al label correspondiente. Además, incluye una sección para extenderlo con análisis de sentimiento.

Contenido de esta carpeta:
- `workflows/email-classifier.json`: flujo listo para importar en n8n (clasificación por categorías).
- `samples/test-emails.md`: ejemplos de correos para probar la automatización.

Requisitos previos
- Cuenta de Gmail (con acceso a la bandeja que vas a clasificar).
- n8n en ejecución (local o cloud). Si usas Docker, sigue la guía del repositorio principal.
- API Key de OpenAI (porque el nodo de clasificación de texto usa un modelo LLM). Crea tu API key en la plataforma de OpenAI.
- Crear los labels en Gmail: `Reclamo`, `Soporte`, `Venta`, `Otro`. (Configuración en Gmail: Configuración -> Etiquetas -> Crear etiqueta).

Pasos detallados (construcción desde cero en n8n)
1) Crear un nuevo Workflow
- En n8n, pulsa “Create workflow” y asígnale un nombre, por ejemplo: `Email Classifier`.

2) Nodo disparador: Gmail Trigger
- Añade nodo `Gmail Trigger`.
- Autentícate con OAuth2 (recomendado). Si es la primera vez, crea la credencial siguiendo el asistente de n8n. Esto requiere:
  - En Google Cloud Console: crear un proyecto, habilitar la `Gmail API`, configurar la pantalla de consentimiento (externo en pruebas), y crear credenciales OAuth Client ID (tipo "Web") usando la URL de redirección que n8n te muestre.
  - Copia `Client ID` y `Client Secret` en n8n y completa el login de Google.
- Configura la frecuencia en `Poll Times` (p. ej., cada minuto).
- En `Filters`, agrega `labelIds: ["INBOX"]` para procesar solo correos que estén en la bandeja de entrada.
- Desmarca la opción de “simple” (si existe), para obtener el objeto completo del mensaje.
- Ejecuta un `Test` para ver que llegan datos (id, subject, text, body, etc.).

3) Modelo de lenguaje (OpenAI)
- Añade el nodo `OpenAI Chat Model` y configura tu credencial de OpenAI con tu API key.
- Selecciona un modelo adecuado y económico para clasificación (por ejemplo `gpt-4o-mini` o el que prefieras disponible en tu cuenta). Nota: en el JSON incluido verás `gpt-5` como placeholder; cambia al modelo que tengas disponible si hace falta.

4) Clasificador de texto
- Añade el nodo `Text Classifier` de la categoría IA (LangChain). Conecta su entrada al `Gmail Trigger` y su salida a los nodos de acción.
- En `inputText` pon algo como: `={{ $json.subject }}{{ $json.text }}` para usar asunto + cuerpo como entrada del clasificador.
- Define las categorías:
  - `Reclamo`: "El usuario expresa una queja o inconformidad sobre un servicio o producto".
  - `Soporte`: "Solicitud de ayuda técnica o guía para resolver un problema".
  - `Venta`: "Mensaje relacionado con la compra de productos o servicios".
  - `Otro`: "Cuando no está en ninguna de las otras categorías".
- Conecta el `OpenAI Chat Model` al `Text Classifier` en el puerto de `ai_languageModel`.
- Ejecuta el nodo para ver a qué categoría envía el ejemplo.

5) Acciones en Gmail (aplicar etiquetas)
- Crea 4 nodos de `Gmail` (acción: `addLabels`), uno por cada ruta que sale del `Text Classifier`:
  - `gmail - reclamos` (labelId -> tu label de Reclamo)
  - `gmail - soporte` (labelId -> tu label de Soporte)
  - `gmail - ventas` (labelId -> tu label de Venta)
  - `gmail - otros` (labelId -> tu label de Otro)
- En cada uno:
  - `messageId`: usa el id del mensaje entrante: `={{ $json.id }}` (desde el trigger o desde el clasificador según tu cableado).
  - `labelIds`: selecciona el ID de la etiqueta correspondiente (en n8n saldrá un selector con tus labels de Gmail).

6) Quitar el label INBOX para no reprocesar el mismo correo
- Añade un nodo `Gmail` con operación `removeLabels` y conéctalo después de cada uno de los 4 nodos anteriores.
- `messageId`: `={{ $('Gmail Trigger').item.json.id }}` o `={{ $json.id }}` (según tu ruta de datos).
- `labelIds`: `["INBOX"]`.
- Con esto, tras etiquetar, se elimina `INBOX` y la próxima ejecución no tomará el mismo mensaje otra vez.

7) Pruebas
- Envía un correo de prueba a tu bandeja con texto típico de cada categoría (ver `samples/test-emails.md`).
- Ejecuta el workflow manualmente y verifica que el correo se mueve a la etiqueta esperada.

8) Activar el workflow
- Cuando todo funcione, activa el workflow (toggle `Active`). Ten en cuenta que esto consultará Gmail con la periodicidad definida y consumirá tokens del modelo LLM.

Sección opcional: Extender con análisis de sentimiento
- Objetivo: además de clasificar por categoría, obtener el sentimiento (positivo, negativo o neutral) del correo.
- Forma sencilla con los mismos nodos:
  1) Añade un nodo `OpenAI Chat Model` (puedes reutilizar el mismo si tu configuración lo permite, o crear otro para independencia de prompts).
  2) Añade un nodo de plantilla o un `Code`/`Set` para construir un prompt, por ejemplo: "Clasifica el sentimiento (positivo, neutral, negativo) del siguiente texto. Responde únicamente una palabra: <texto>" usando `={{ $json.subject }}\n\n{{ $json.text }}`.
  3) Conecta la respuesta a un `Set` para guardar el resultado en una propiedad JSON, por ejemplo `sentiment`.
  4) Usa ese campo para tomar decisiones adicionales (p. ej., notificar si es `negativo`).
- Nota: Hay nodos de IA especializados que pueden ayudar (p. ej., plantillas o clasificadores). Si en tu instancia tienes un nodo de “Sentiment” específico, puedes usarlo; de lo contrario, el prompt directo funciona muy bien para ejercicios.

Importar el flujo incluido (JSON)
- En n8n: `Workflows` -> `Import from File` y selecciona `practice/workflows/email-classifier.json`.
- Ajusta credenciales:
  - `Gmail account` (OAuth2) 
  - `OpenAi account` (API Key)
- Ajusta el modelo si `gpt-5` no existe en tu cuenta: elige uno disponible.
- Ajusta los `labelIds` a los IDs reales de tus etiquetas.

Notas y buenas prácticas
- Costos: cada correo procesado invoca el modelo LLM. Usa un modelo económico y ajusta la frecuencia del trigger.
- Desarrollo vs Producción: deja el flujo inactivo mientras pruebas manualmente.
- Logs: n8n permite ver la ejecución paso a paso y los datos de entrada/salida de cada nodo.

¡Listo! Con esto tienes un clasificador por categorías funcional y una guía clara para extender con sentimiento.
