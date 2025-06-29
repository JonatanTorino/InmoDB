# Plan de Documentación - Sistema de Extracción de Contratos de Arrendamiento

- [Plan de Documentación - Sistema de Extracción de Contratos de Arrendamiento](#plan-de-documentación---sistema-de-extracción-de-contratos-de-arrendamiento)
  - [1. Resumen Ejecutivo](#1-resumen-ejecutivo)
    - [Objetivo del Proyecto](#objetivo-del-proyecto)
    - [Alcance](#alcance)
  - [2. Arquitectura del Sistema](#2-arquitectura-del-sistema)
    - [2.1 Stack Tecnológico](#21-stack-tecnológico)
    - [2.2 Componentes Principales](#22-componentes-principales)
      - [Módulo de Lectura de Archivos](#módulo-de-lectura-de-archivos)
      - [Módulo de Extracción de Texto](#módulo-de-extracción-de-texto)
      - [Módulo de Procesamiento NLP](#módulo-de-procesamiento-nlp)
      - [Módulo de Embeddings](#módulo-de-embeddings)
      - [Módulo de Base de Datos Vectorial](#módulo-de-base-de-datos-vectorial)
      - [Módulo de Exportación a Excel](#módulo-de-exportación-a-excel)
      - [Módulo de Chatbot (Fase 2)](#módulo-de-chatbot-fase-2)
  - [3. Información a Extraer](#3-información-a-extraer)
    - [3.1 Datos del Propietario](#31-datos-del-propietario)
    - [3.2 Información de la Propiedad](#32-información-de-la-propiedad)
    - [3.3 Detalles del Contrato](#33-detalles-del-contrato)
    - [3.4 Información de Contacto](#34-información-de-contacto)
  - [4. Fases de Desarrollo](#4-fases-de-desarrollo)
    - [4.1 Fase 1: Base de Datos Vectorial Local + Export Excel](#41-fase-1-base-de-datos-vectorial-local--export-excel)
      - [Objetivos Principales](#objetivos-principales)
      - [Componentes a Desarrollar](#componentes-a-desarrollar)
      - [Entregables Fase 1](#entregables-fase-1)
    - [4.2 Fase 2: Chatbot para Consultas Vectoriales](#42-fase-2-chatbot-para-consultas-vectoriales)
      - [Objetivos Principales](#objetivos-principales-1)
      - [Componentes a Desarrollar](#componentes-a-desarrollar-1)
      - [Entregables Fase 2](#entregables-fase-2)
    - [4.3 Fase 3: Monitoreo Automático (A Definir)](#43-fase-3-monitoreo-automático-a-definir)
      - [Objetivos Preliminares](#objetivos-preliminares)
      - [Componentes Potenciales](#componentes-potenciales)
      - [Estado: **Pendiente de definición detallada**](#estado-pendiente-de-definición-detallada)
  - [5. Configuración del Entorno](#5-configuración-del-entorno)
    - [5.1 Dependencias de Python (Todas Gratuitas)](#51-dependencias-de-python-todas-gratuitas)
    - [5.2 Configuración Local (Sin Costos)](#52-configuración-local-sin-costos)
    - [5.3 Estructura de Archivos Excel](#53-estructura-de-archivos-excel)
  - [6. Flujo de Trabajo](#6-flujo-de-trabajo)
    - [6.1 Procesamiento de Archivos](#61-procesamiento-de-archivos)
    - [6.2 Sistema de Consultas](#62-sistema-de-consultas)
  - [7. Consideraciones Técnicas](#7-consideraciones-técnicas)
    - [7.1 Desafíos Identificados](#71-desafíos-identificados)
    - [7.2 Estrategias de Mitigación](#72-estrategias-de-mitigación)
  - [8. Métricas de Éxito](#8-métricas-de-éxito)
    - [8.1 Indicadores de Rendimiento](#81-indicadores-de-rendimiento)
    - [8.2 Criterios de Aceptación](#82-criterios-de-aceptación)
  - [9. Cronograma Estimado](#9-cronograma-estimado)
    - [Fase 1 - Base de Datos Vectorial + Excel (6-8 semanas)](#fase-1---base-de-datos-vectorial--excel-6-8-semanas)
    - [Fase 2 - Chatbot (4-5 semanas)](#fase-2---chatbot-4-5-semanas)
    - [Fase 3 - Monitoreo Automático (A definir)](#fase-3---monitoreo-automático-a-definir)
  - [10. Riesgos y Contingencias](#10-riesgos-y-contingencias)
    - [10.1 Riesgos Identificados](#101-riesgos-identificados)
    - [10.2 Planes de Contingencia](#102-planes-de-contingencia)
  - [11. Documentación Requerida](#11-documentación-requerida)
    - [11.1 Documentación Técnica](#111-documentación-técnica)
    - [11.2 Documentación de Usuario](#112-documentación-de-usuario)
  - [12. Próximos Pasos](#12-próximos-pasos)
    - [Inmediatos (Fase 1)](#inmediatos-fase-1)
    - [Mediano plazo (Fase 2)](#mediano-plazo-fase-2)
    - [Largo plazo (Fase 3)](#largo-plazo-fase-3)

## 1. Resumen Ejecutivo

### Objetivo del Proyecto

Desarrollar un sistema automatizado para extraer información específica de contratos de arrendamiento almacenados en archivos Word y PDF, organizando estos datos en una base de datos vectorial que permita consultas eficientes.

### Alcance

- **Fase 1**: Construcción de base de datos vectorial local y extracción de datos a Excel
- **Fase 2**: Implementación de chatbot para consultas sobre la base de datos vectorial
- **Fase 3**: Sistema de monitoreo automático para archivos nuevos/modificados (a definir)

## 2. Arquitectura del Sistema

### 2.1 Stack Tecnológico

- **Lenguaje**: Python
- **Procesamiento de documentos**:
  - Word: python-docx
  - PDF: PyMuPDF/pdfplumber/PyPDF2
- **NLP**: Técnicas de procesamiento de lenguaje natural (spaCy, NLTK)
- **Embeddings**: sentence-transformers (modelos gratuitos), transformers
- **Base de datos vectorial**: ChromaDB (local y gratuito)
- **Chatbot**: Streamlit + LangChain + Ollama (modelos LLM locales gratuitos)
- **Salida**: Excel (.xlsx) con openpyxl/xlswriter
- **Interfaz**: Streamlit (para chatbot y administración)

### 2.2 Componentes Principales

#### Módulo de Lectura de Archivos

- Procesamiento de documentos Word (.docx)
- Procesamiento de documentos PDF
- Validación de formatos de archivo

#### Módulo de Extracción de Texto

- Limpieza de texto (eliminación de formato, saltos de línea, ruido)
- Normalización de caracteres
- Preservación de estructura relevante

#### Módulo de Procesamiento NLP

- Reconocimiento de entidades nombradas (NER)
- Análisis de dependencias
- Reglas específicas para contratos de arrendamiento
- Extracción de patrones mediante regex

#### Módulo de Embeddings

- Generación de embeddings vectoriales
- Uso de modelos pre-entrenados
- Optimización para texto legal/contractual

#### Módulo de Base de Datos Vectorial

- Almacenamiento local con ChromaDB
- Indexación para búsquedas eficientes
- Gestión de metadatos
- Persistencia local sin costos

#### Módulo de Exportación a Excel

- Generación de registros estructurados
- Tracking de archivos procesados
- Metadatos de almacenamiento vectorial
- Timestamps y estados de procesamiento

#### Módulo de Chatbot (Fase 2)

- Interfaz conversacional con Streamlit
- Integración con LangChain
- Conexión a base de datos vectorial
- Procesamiento de consultas en lenguaje natural

## 3. Información a Extraer

### 3.1 Datos del Propietario

- Nombre completo
- Identificación (DNI/CUIT)
- Dirección
- Información de contacto

### 3.2 Información de la Propiedad

- Dirección completa
- Tipo de propiedad
- Características principales
- Superficie

### 3.3 Detalles del Contrato

- Fechas de inicio y finalización
- Duración del alquiler
- Precio mensual inicial
- Intervalos de incremento de precios
- Porcentajes de aumento
- Fechas de revisión

### 3.4 Información de Contacto

- Datos del inquilino
- Garantes (si aplica)
- Inmobiliaria o intermediario

## 4. Fases de Desarrollo

### 4.1 Fase 1: Base de Datos Vectorial Local + Export Excel

#### Objetivos Principales

- Construir base de datos vectorial local con herramientas gratuitas
- Procesar archivos Word/PDF de contratos de arrendamiento
- Extraer información específica de cada contrato
- Generar archivo Excel con registro de archivos procesados
- Almacenar embeddings localmente sin costos de servicios externos

#### Componentes a Desarrollar

**Sistema de Procesamiento de Documentos:**

- Lector de archivos Word (.docx) y PDF
- Extractor y limpiador de texto
- Validador de formato de contratos

**Motor de Extracción de Datos:**

- Identificación de información clave mediante NLP
- Extracción de datos del propietario, propiedad, fechas, precios
- Validación y normalización de datos extraídos

**Base de Datos Vectorial Local:**

- Configuración de ChromaDB local
- Generación de embeddings con sentence-transformers
- Indexación y almacenamiento de documentos procesados
- Sistema de metadatos para tracking

**Generador de Excel:**

- Creación de registro por cada archivo procesado
- Columnas: nombre_archivo, fecha_procesamiento, estado, datos_extraídos, id_vectorial
- Actualización incremental del archivo Excel
- Manejo de errores y logs

#### Entregables Fase 1

- Scripts Python para procesamiento completo
- Base de datos ChromaDB configurada y poblada
- Archivo Excel con registros de archivos procesados
- Deseables:
  - Documentación de instalación y uso local
  - Logs de procesamiento y manejo de errores

### 4.2 Fase 2: Chatbot para Consultas Vectoriales

#### Objetivos Principales

- Implementar interfaz de chatbot usando Streamlit
- Conectar chatbot con base de datos vectorial local
- Permitir consultas en lenguaje natural sobre contratos
- Responder preguntas específicas usando embeddings almacenados

#### Componentes a Desarrollar

**Interfaz de Chatbot:**

- Aplicación web con Streamlit
- Chat interface intuitiva
- Historial de conversaciones (sesión)

**Motor de Consultas:**

- Integración con LangChain para procesamiento de queries
- Conexión a ChromaDB para búsquedas vectoriales
- Modelo LLM local (Ollama) para generación de respuestas
- Sistema de ranking y relevancia de resultados

**Procesador de Respuestas:**

- Interpretación de consultas del usuario
- Búsqueda semántica en base vectorial
- Generación de respuestas contextuales
- Citado de fuentes (archivos originales)

#### Entregables Fase 2

- Aplicación Streamlit funcional
- Sistema de consultas vectoriales
- Integración LLM local
- Manual de usuario del chatbot
- Ejemplos de consultas y respuestas

### 4.3 Fase 3: Monitoreo Automático (A Definir)

#### Objetivos Preliminares

- Detectar archivos nuevos en carpetas monitoreadas
- Identificar modificaciones en archivos existentes
- Validar si archivos nuevos son contratos potenciales
- Solicitar confirmación del usuario para procesar
- Actualizar automáticamente Excel y base vectorial

#### Componentes Potenciales

- File system watcher
- Clasificador de tipos de documento
- Sistema de notificaciones
- Interfaz de confirmación
- Actualizador automático

#### Estado: **Pendiente de definición detallada**

## 5. Configuración del Entorno

### 5.1 Dependencias de Python (Todas Gratuitas)

```python
# Procesamiento de documentos
python-docx
PyMuPDF
pdfplumber

# NLP y embeddings (gratuitos)
sentence-transformers
transformers
spacy
nltk

# Base de datos vectorial local
chromadb

# Chatbot y LLM local (Fase 2)
streamlit
langchain
ollama-python

# Excel y manipulación de datos
pandas
openpyxl
xlswriter

# Utilidades
pathlib
watchdog  # Para monitoreo de archivos (Fase 3)
```

### 5.2 Configuración Local (Sin Costos)

**Base de Datos Vectorial:**

- ChromaDB: completamente local, sin servicios externos
- Almacenamiento en sistema de archivos local
- No requiere configuración de nube o APIs pagas

**Modelos de Embeddings:**

- sentence-transformers con modelos pre-entrenados gratuitos
- Modelos como 'all-MiniLM-L6-v2' o 'paraphrase-multilingual-MiniLM-L12-v2'
- Descarga automática en primera ejecución

**LLM Local (Fase 2):**

- Ollama para ejecutar modelos localmente
- Modelos como Llama 2, Mistral, o CodeLlama
- Sin dependencia de APIs externas como OpenAI

### 5.3 Estructura de Archivos Excel

**Columns del registro:**

- `archivo_nombre`: Nombre del archivo procesado
- `ruta_completa`: Path completo del archivo
- `fecha_procesamiento`: Timestamp del procesamiento
- `estado`: SUCCESS/ERROR/PENDING
- `hash_archivo`: Hash para detectar cambios
- `id_vectorial`: ID único en ChromaDB
- `propietario_nombre`: Datos extraídos
- `propiedad_direccion`: Dirección de la propiedad
- `precio_inicial`: Precio mensual inicial
- `fecha_inicio`: Fecha de inicio del contrato
- `fecha_fin`: Fecha de finalización
- `observaciones`: Notas o errores de procesamiento

## 6. Flujo de Trabajo

### 6.1 Procesamiento de Archivos

1. **Detección**: Escanear carpeta de entrada
2. **Validación**: Verificar formato y accesibilidad
3. **Extracción**: Leer contenido de documentos
4. **Limpieza**: Procesar y limpiar texto
5. **Análisis**: Aplicar técnicas de NLP
6. **Extracción de datos**: Identificar información específica
7. **Embeddings**: Generar representaciones vectoriales
8. **Almacenamiento**: Guardar en base de datos vectorial

### 6.2 Sistema de Consultas

1. **Input**: Consulta del usuario
2. **Procesamiento**: Generar embedding de consulta
3. **Búsqueda**: Encontrar documentos similares
4. **Extracción**: Recuperar información específica
5. **Formato**: Presentar resultados estructurados

## 7. Consideraciones Técnicas

### 7.1 Desafíos Identificados

- Diversidad de formatos de contratos
- Variaciones en estructura de documentos
- Precisión en extracción de datos
- Manejo de documentos escaneados (OCR)
- Procesamiento de texto legal específico

### 7.2 Estrategias de Mitigación

- Desarrollo de múltiples patrones de extracción
- Implementación de validaciones cruzadas
- Uso de técnicas de NLP robustas
- Manejo de excepciones y errores
- Logging detallado para debugging

## 8. Métricas de Éxito

### 8.1 Indicadores de Rendimiento

- Precisión de extracción de datos (>90%)
- Tiempo de procesamiento por documento
- Cobertura de tipos de contratos
- Tasa de errores y excepciones

### 8.2 Criterios de Aceptación

- Procesamiento exitoso de formatos Word y PDF
- Extracción precisa de campos requeridos
- Base de datos vectorial funcional
- Sistema de consultas operativo
- Documentación completa

## 9. Cronograma Estimado

### Fase 1 - Base de Datos Vectorial + Excel (6-8 semanas)

**Semana 1-2**: Configuración del entorno y procesamiento básico

- Instalación de ChromaDB y dependencias
- Módulo de lectura de archivos Word/PDF
- Extracción básica de texto

**Semana 3-4**: Extracción de datos y NLP

- Implementación de patrones de extracción
- Procesamiento NLP para identificar información clave
- Validación y normalización de datos

**Semana 5-6**: Base de datos vectorial y embeddings

- Configuración de ChromaDB local
- Generación de embeddings con sentence-transformers
- Almacenamiento y indexación

**Semana 7-8**: Excel export y testing

- Generación de archivo Excel con registros
- Testing integral del pipeline
- Documentación y optimización

### Fase 2 - Chatbot (4-5 semanas)

**Semana 9-10**: Configuración de LLM local y Streamlit

- Instalación y configuración de Ollama
- Desarrollo de interfaz básica con Streamlit
- Conexión con ChromaDB

**Semana 11-12**: Integración LangChain y consultas

- Implementación de queries vectoriales
- Integración con LangChain para procesamiento
- Sistema de respuestas contextuales

**Semana 13**: Testing y refinamiento

- Pruebas de consultas complejas
- Optimización de respuestas
- Documentación del chatbot

### Fase 3 - Monitoreo Automático (A definir)

- Cronograma pendiente de especificación detallada

## 10. Riesgos y Contingencias

### 10.1 Riesgos Identificados

- Calidad variable de documentos fuente
- Complejidad de extracción de datos legales
- Rendimiento de base de datos vectorial
- Escalabilidad del sistema

### 10.2 Planes de Contingencia

- Desarrollo de múltiples estrategias de extracción
- Implementación de fallbacks y validaciones
- Optimización iterativa de rendimiento
- Arquitectura escalable desde el diseño

## 11. Documentación Requerida

### 11.1 Documentación Técnica

- Manual de instalación y configuración
- Documentación de API
- Guía de arquitectura del sistema
- Especificaciones de base de datos

### 11.2 Documentación de Usuario

- Manual de usuario final
- Guía de consultas
- Ejemplos de uso
- FAQ y troubleshooting

## 12. Próximos Pasos

### Inmediatos (Fase 1)

1. **Configuración del entorno local**
   - Instalación de Python y dependencias gratuitas
   - Configuración de ChromaDB local
   - Descarga de modelos de sentence-transformers

2. **Desarrollo del pipeline básico**
   - Implementación de lectores de archivos
   - Sistema de extracción de texto y datos
   - Conexión con base de datos vectorial local

3. **Implementación de export a Excel**
   - Diseño de estructura de registro
   - Sistema de tracking de archivos procesados
   - Manejo de errores y logging

### Mediano plazo (Fase 2)

4. **Preparación para chatbot**
   - Investigación de modelos Ollama disponibles
   - Diseño de interfaz Streamlit
   - Planificación de queries vectoriales

5. **Desarrollo del chatbot**
   - Implementación de interfaz conversacional
   - Integración LangChain + ChromaDB
   - Testing de consultas y respuestas

### Largo plazo (Fase 3)

6. **Especificación detallada del monitoreo**
   - Definición de requerimientos específicos
   - Selección de tecnologías de file watching
   - Diseño de flujo de confirmación de usuario
