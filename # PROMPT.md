# PROMPT

¡Excelente! Me parece un plan muy bueno. Primero, aclaramos bien el alcance y los detalles, y luego construimos un prompt específico para explorar las soluciones con Gemini Pro.

Basado en la información que me proporcionaste y para asegurarme de entender todos los matices antes de pasar a la fase de diseño de la solución, aquí tienes algunas preguntas adicionales:

Preguntas de Clarificación Adicional:

Filtrado de Archivos:

* Mencionaste filtrar archivos por nombre que contengan "contrato" o "contratos". ¿Es esta regla suficiente para identificar solo los contratos relevantes dentro del directorio de 18GB? ¿Puede haber archivos no contractuales que contengan esa palabra en el nombre? ¿O puede haber contratos relevantes que no la contengan?
    * La regla es que diga contrato en el nombre.
* ¿Hay algún otro criterio (aparte del nombre) que pueda usarse para identificar si un archivo es un contrato de locación que debe ser procesado (por ejemplo, el tipo de archivo, una palabra clave específica en el contenido al inicio del documento, o la ubicación en una subcarpeta específica dentro del segundo nivel de subdirectorios)?
    * El nombre y la extensión debe ser .pdf o .docx

Variabilidad de Estructura y Lenguaje:

* Si bien la estructura es "similar", ¿qué tan común es encontrar cláusulas o formatos inusuales en algunos contratos? ¿Podría esto dificultar la extracción automática de datos clave?
    * Están bien estructurados los contratos.
* La jerga legal argentina mencionada como "marcadores": ¿Son frases muy específicas y repetitivas (ej. "EL ARRENDADOR arrienda AL ARRENDATARIO...") o son más bien conceptos que la AI deberá interpretar dentro de oraciones más variadas?
    * El LOCADOR es el dueño del inmueble, sus datos son los que me interesan.
* ¿Qué pasa con los contratos con anexos o addendums? ¿Deben ser procesados también, o la información clave siempre está en el cuerpo principal del contrato original?
    * La información clave siempre está en el contrato original

Detalles de los Datos a Extraer:

Para asegurar la precisión, ¿podrías describir o dar ejemplos (incluso si son ficticios) de cómo se presentan típicamente los datos clave (fechas, monto, nombre, teléfono, WhatsApp, correo electrónico) en el texto? Por ejemplo:

Fecha inicio: 01/01/2023 vs. Comienza el primer día de enero de 2023.

Monto: $150.000.- vs. El alquiler mensual es de CIENTO CINCUENTA MIL PESOS ARGENTINOS ($150.000).

Propietario: Juan Pérez DNI 12.345.678 vs. El señor Juan Pérez (en adelante, "EL PROPIETARIO") con domicilio en...

Contacto: Tel: +54 9 11 5555-1234, Mail: jperez@mail.com vs. Información de contacto dispersa.

Mencionaste "nombre del propietario". ¿Hay otros roles importantes (ej. inquilino, garante) cuyos nombres también podrían ser útiles, o solo necesitas el propietario?

¿Cómo se manejan los montos con ajuste o indexación en los contratos? ¿Se necesita extraer solo el monto inicial o también la fórmula de ajuste (si es relevante y extraíble de forma estructurada)? Por ahora solo pides el monto inicial, lo cual simplifica, pero quiero confirmar si esa es la única necesidad.

Procesamiento y Escalabilidad:

Con 18GB, ¿cuál es la cantidad estimada de archivos individuales a procesar en la carga inicial? (Saber si son miles de archivos pequeños o menos archivos muy grandes ayuda a pensar en estrategias de procesamiento).

La descarga temporal a un disco local para el batch inicial, ¿hay espacio suficiente para esto? (Parece que sí, pero es bueno confirmarlo).

Para la automatización de archivos nuevos en OneDrive, ¿cómo se planifica detectar esos archivos? ¿N8n o un script Python estarían continuamente "vigilando" la carpeta (polling) o se podría configurar algún tipo de webhook o trigger si OneDrive lo permite (esto suele ser más complejo de configurar)?

La validación con una segunda AI: ¿La idea es pasarle todo el texto del contrato y los datos extraídos a una segunda llamada a un LLM pidiendo confirmación, o es una validación más "ligera"? ¿Esto duplica o triplica el costo de las llamadas a la AI por archivo?

Consideraciones de Herramientas/Costo:

Reiterando el punto del costo de las APIs de AI: Aunque sean "gratuitas en su mayoría", procesar 18GB de documentos, más la posible validación secundaria, podría acumular un costo no trivial. ¿Hay un presupuesto estimado (ej. "menos de $X dólares al mes") o un umbral a partir del cual el costo de la AI se vuelve prohibitivo y justificaría buscar alternativas (como modelos locales más pequeños o técnicas de extracción no basadas completamente en LLMs generativos)?

¿Hay alguna preferencia entre n8n (visual, bajo-código, orquestación) y Python (flexibilidad, control granular) para la lógica principal de extracción y procesamiento? O queremos evaluar ambas opciones de forma neutral?

Agradezco tu paciencia con estas preguntas. Entender estos puntos me permitirá ayudarte a estructurar un prompt mucho más efectivo para que Gemini Pro te ofrezca las opciones de solución más adecuadas y detalladas, considerando tus herramientas, contexto y limitaciones.

Una vez que respondas, pasaremos a armar ese prompt enfocándonos en las pros y contras de los enfoques con n8n y Python + AI.