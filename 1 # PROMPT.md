# PROMPT

Ayudame a armar un prompt para usar en un chat con un LLM de Gemini Pro.

Tengo conocimientos de programación, un poco de automatizaciones y de herramientas de AI, como ser el uso de los GPT, herramientas de flujos como n8n, power automate, un poco de pyhton, se usar VSCode y herramientas como Cline/Roo Code/Git HubCopilot.

Me solicitaron una solución para armar una tabla con datos extraidos de archivos de textos en formatos PDF y Word, que son contratos de locación por arrendamientos de inmuebles de un negocio inmobiliario.

¿Cómo podemos armar un prompt para ayudarme a pensar y armar una solución?

Te daré una serie de preguntas y respuesta para facilitar un poco el contexto. 

1. **Archivos de Origen:**
    * Qué tipos de archivos de texto son exactamente (ej. PDF, DOCX, DOC, TXT)?
      * Son PDF y DOCX.
    * Son PDFs "nativos" (texto seleccionable) o PDFs escaneados (imágenes que requieren OCR)?
      * PDFs nativos.
    * Los archivos Word son `.doc` o `.docx`?
      * .docx
    * ¿El contenido de los archivos sigue una estructura repetitiva (por ejemplo, siempre en el mismo orden) o puede variar?
      * Son contratos de locación que cumple una estructura muy similar.
    * Cuál es el volumen aproximado de archivos que necesitas procesar (ej. diario, semanal, total, número de archivos)?
      * Es un directorio de 18 gb en archivos (también hay otros archivos que debemos omitir). El directorio posee un primer nivel de subdirectorios, para agrupar por cierta naturlaza en común, y un segundo nivel de subdirectorios, que sirve para separar los archivos de distintas propiedades.
    * Cuál es la frecuencia con la que se reciben estos archivos o se debe ejecutar la extracción? ¿Será un proceso manual o automatizado?
      * Los archivos están todos en un directorio en OneDrive. Se debe hacer una primera extracción inicial de forma manual y luego se considera automático el detectar archivos nuevos que cumplan con reglas para dicha extracción.
      * El directorio posee 18 gb en archivos. Para una ejecución incial se puede descargar una copia temporal a un disco local para optimizar el procesamiento. Pero el proceso debe poder reutilizarse también cuando se suban archivos nuevos al repositorio de archivos en la nube OneDrive.

2. **Datos a Extraer:**
    * Qué tipo de información específica necesitas extraer de cada archivo? (Ej. Nombre, Fecha, Número de Factura, Dirección, Monto, Párrafos específicos, etc.)
      * Los archivos son contratos de locación para arrendamientos de inmuebles. La información a extraer incluye fechas de inicio y fin del contrato, monto inicial, nombre del propietario, y datos de contacto (teléfono, WhatsApp, correo electrónico).
    * La información a extraer siempre se encuentra en el mismo lugar o formato dentro de los archivos (ej. siempre después de 'Nombre:', en una tabla específica dentro del documento, o varía mucho la ubicación/formato)?
      * La información se extrae de archivos que contengan específicamente la palabra 'contrato' o 'contratos' (puede ser en mayúscula, minúsculas).
    * Qué tan estructurada está la información dentro de los archivos? (ej. datos en pares clave-valor simples, información contenida en tablas dentro del documento, texto corrido donde hay que identificar patrones complejos, listas)
      * Son archivos de varias páginas con lenguajes legales dado que son contratos de locación de inmuebles.
    * Hay "marcadores" o "etiquetas" consistentes que puedan usarse para identificar los datos (ej. siempre aparece la palabra "Total:" antes del monto a extraer)?
      * Pueden tomarse como marcadores palabras que se usan en la jerga legal de contratos de arrendamiento bajo el código civil/comercial de Argentina.
    * Puede haber variaciones en la forma en que se presentan los datos o la información a extraer (ej. la fecha puede venir en distintos formatos como DD/MM/YYYY, MM-DD-YY, etc.)?
      * El formato de las fechas es bajo la cultura argentina.
    * ¿Qué acción se debe tomar si alguno de los datos requeridos no se encuentra en un archivo particular? (ej. dejar la celda vacía, marcar el archivo como error)
      * Dejar el dato vació.

3. **Tabla de Destino:**
    * En qué formato final necesitas la tabla de datos extraídos? (ej. CSV, hoja de cálculo Excel/Google Sheets, base de datos relacional como PostgreSQL/MySQL, base de datos NoSQL, JSON, etc.)?
      * En json considero que es más versátil en caso de necesitar transformaciones de formatos.
    * Cuáles serían las columnas de esa tabla? (Es decir, qué dato de los archivos iría en cada columna?)
      * Los campos relevantes para la solución son: fechas de inicio y fin del contrato, monto inicial, nombre del propietario, teléfono, WhatsApp, correo electrónico.
      * Los campos metadata del prosesamiento son: el nombre del archivo de origen, la ruta completa del archivo, la fecha de procesamiento, observaciones (en caso de error o algún dato que no pudo procesar).
    * Dónde se debe almacenar o usar esta tabla una vez generada (ej. guardarla localmente, subirla a un servicio en la nube, insertarla en una base de datos, enviarla a otra aplicación a través de una API)?
      * Dependerá del tipo de solución implementada, pero al ser formato json podemos resolver esto después.
    * ¿Cómo se deben reportar los errores o archivos que no pudieron ser procesados (ej. en un archivo de log separado, una columna en la tabla de salida)?
      * Se debe guardar la información del nombre del archivo, la ruta de ubicación, y un mensaje en el campo de observaciones de lo que ocurrió.

4. **Contexto y Requisitos Adicionales:**
    * Cuál es el objetivo final de tener esta tabla? (Para qué se van a usar los datos? Esto puede ayudar a priorizar la fiabilidad o la velocidad).
      * Para hacer un seguimiento de cada contrato, con el fin de contactar a los propietarios de los inmuebles que tengan el contrato finalizado o hacerlo cuando finalicen.
    * Hay requisitos de velocidad o rendimiento para el procesamiento? (ej. debe ser en tiempo real, puede ejecutarse como un batch nocturno).
      * No hay necesidad de que sea un alto rendimiento en velocidad. Puede ser disparado por el trigger del evento de un archivo nuevo agregado, pero si falla debería poder correrse en batch nocturno para detectar aquellos eventos que no fueron capturados o procesados.
    * Hay alguna consideración de seguridad o privacidad con los datos que vas a procesar?
      * No.
    * Tienes alguna restricción o preferencia de herramientas *adicional* a las que mencionaste (Python, n8n, Power Automate, VS Code, Copilot, etc.)? Por ejemplo, "debe ser open source", "debe ejecutarse on-premise", "preferimos herramientas low-code/no-code", "tenemos licencia de [herramienta X]".
      * Debemos usar inteligencia artificial para leer cada archivo que contenga el nombre "contrato" y que extraiga los datos. Será una solución privada, bebe ser con herramientas gratis en su mayoría, solo de pago en caso de que su utilidad simplifique mucho la solución.
    * Qué nivel de precisión se requiere en la extracción de datos? (ej. "una pequeña tasa de error es aceptable" vs "debe ser 100% preciso")
      * Debe ser altamente preciso en la extracción de los datos claves solicitados. Propongo usar una segunda AI a la cual le pasemos los datos extraidos y que confirme si son los correctos que corresponden analizando el mismo archivo.
    * ¿Se necesita una interfaz de usuario (GUI) para seleccionar la carpeta y ejecutar el proceso, o una herramienta de línea de comandos/script es suficiente?
      * Para la detección de proceso automático no se necesita un GUI. Para las ejecuciones manules puede ser la ejecución de un comando por consola.
    * ¿Querés una solución que puedas correr manualmente en tu máquina (por ejemplo, un script en Python) o buscás algo automático (tipo flujo n8n, o serverless)?
      * Quiero explorar las opciones de:
        * Usar docker para n8n, que si la solución me gusta luego puedo contratar un servicio en la nube.
        * Usar algo en python que haga uso de librerías para consumir APIs de AI, puede ser también con langchain.

Haz haz las preguntas adicionales que necesites para ampliar la comprención del problema contexto necesidad, etc.
Te recuerdo que incialmente quiero que me preguntes lo necesario para después armemos un prompt para una nueva charla, con el obejtivo de entender opciones con pros y contras de los distintos enfoques.
Quiero que me respondas en formato markdown.
Empecemos con una serie de preguntas que necesites para entender mejor el requerimiento.
