# Plan de Ejecución para AI - Sistema de Extracción de Contratos

## 1. Arquitectura Abstracta del Sistema

### 1.1 Componentes Principales (Abstractos)

#### A. Procesador de Documentos
- **Función**: Leer y extraer texto de archivos Word/PDF
- **Entrada**: Archivos de documentos
- **Salida**: Texto plano estructurado
- **Intercambiabilidad**: Módulo independiente con interfaz estándar

#### B. Motor de Análisis de Texto
- **Función**: Procesar texto y extraer información específica
- **Entrada**: Texto plano
- **Salida**: Datos estructurados (JSON)
- **Intercambiabilidad**: API REST o biblioteca con interfaz común

#### C. Generador de Embeddings
- **Función**: Convertir texto a representaciones vectoriales
- **Entrada**: Texto procesado
- **Salida**: Vectores numéricos
- **Intercambiabilidad**: Servicio independiente con API estándar

#### D. Base de Datos Vectorial
- **Función**: Almacenar y consultar embeddings
- **Entrada**: Vectores + metadatos
- **Salida**: Resultados de búsqueda similitud
- **Intercambiabilidad**: Driver de conexión intercambiable

#### E. Exportador de Datos
- **Función**: Generar archivos Excel con registros
- **Entrada**: Datos estructurados
- **Salida**: Archivo Excel
- **Intercambiabilidad**: Módulo con interfaz de escritura común

#### F. Motor de Consultas (Fase 2)
- **Función**: Procesar consultas en lenguaje natural
- **Entrada**: Query del usuario
- **Salida**: Respuesta contextual
- **Intercambiabilidad**: LLM intercambiable vía API

#### G. Orquestador Principal
- **Función**: Coordinar todos los componentes
- **Entrada**: Configuración + parámetros de ejecución
- **Salida**: Logs de ejecución + resultados
- **Intercambiabilidad**: No aplicable (core del sistema)

## 2. Opciones de Implementación por Componente

### 2.1 Procesador de Documentos

#### Opción A: Bibliotecas Python Locales
- **Word**: python-docx, docx2txt
- **PDF**: PyMuPDF, pdfplumber, PyPDF2
- **Ventajas**: Completamente gratuito, control total
- **Desventajas**: Limitaciones con documentos complejos

#### Opción B: APIs de Procesamiento
- **Adobe PDF Services API**: Cuota gratuita 1000 docs/mes
- **Microsoft Graph API**: Cuota gratuita limitada
- **Google Drive API**: Cuota gratuita 100 req/100s
- **Ventajas**: Mayor precisión, OCR incluido
- **Desventajas**: Dependencia externa, límites de cuota

#### Opción C: Servicios Open Source
- **Apache Tika**: Vía REST API local con Docker
- **Pandoc**: Conversión local
- **Ventajas**: Potente, sin límites de cuota
- **Desventajas**: Configuración más compleja

### 2.2 Motor de Análisis de Texto

#### Opción A: Bibliotecas NLP Python
- **spaCy**: Modelos pre-entrenados gratuitos
- **NLTK**: Toolkit completo gratuito
- **Transformers (Hugging Face)**: Modelos gratuitos
- **Ventajas**: Completamente gratuito, offline
- **Desventajas**: Requiere configuración de modelos

#### Opción B: APIs de NLP
- **Google Cloud Natural Language**: $1-2 per 1K docs
- **AWS Comprehend**: Cuota gratuita 50K docs/mes
- **Azure Text Analytics**: Cuota gratuita 5K docs/mes
- **Ventajas**: Alta precisión, sin configuración
- **Desventajas**: Costos después de cuotas gratuitas

#### Opción C: Modelos LLM Locales
- **Ollama + Llama/Mistral**: Completamente gratuito
- **GPT4All**: Modelos locales gratuitos
- **Ventajas**: Sin límites, privacidad total
- **Desventajas**: Requiere recursos computacionales

### 2.3 Generador de Embeddings

#### Opción A: Modelos Locales
- **sentence-transformers**: Modelos gratuitos
- **OpenAI Ada (local via oobabooga)**: Gratuito
- **all-MiniLM-L6-v2**: Modelo liviano gratuito
- **Ventajas**: Sin costos, offline
- **Desventajas**: Calidad variable según modelo

#### Opción B: APIs de Embeddings
- **OpenAI Embeddings**: $0.0001 per 1K tokens
- **Cohere Embed**: Cuota gratuita 100 calls/min
- **Google Vertex AI**: Cuota gratuita limitada
- **Ventajas**: Alta calidad, sin configuración
- **Desventajas**: Costos escalables

#### Opción C: Servicios Open Source
- **Sentence-BERT Server**: Docker local
- **Universal Sentence Encoder**: TensorFlow local
- **Ventajas**: Balance calidad/costo
- **Desventajas**: Configuración intermedia

### 2.4 Base de Datos Vectorial

#### Opción A: Soluciones Locales
- **ChromaDB**: Completamente gratuito, SQLite backend
- **Faiss (Facebook)**: Biblioteca gratuita, muy rápida
- **Annoy (Spotify)**: Biblioteca gratuita, eficiente
- **Ventajas**: Sin costos, control total
- **Desventajas**: Escalabilidad limitada

#### Opción B: Servicios Cloud Gratuitos
- **Pinecone**: 1M vectores gratis, 1 índice
- **Weaviate Cloud**: Cluster gratuito pequeño
- **Qdrant Cloud**: 1GB gratis
- **Ventajas**: Escalabilidad, sin configuración
- **Desventajas**: Límites de cuota gratuita

#### Opción C: Autohosteado con Docker
- **Weaviate**: Docker local
- **Qdrant**: Docker local
- **Milvus**: Docker local (más complejo)
- **Ventajas**: Sin límites, profesional
- **Desventajas**: Configuración avanzada

### 2.5 Motor de Consultas (Fase 2)

#### Opción A: LLMs Locales
- **Ollama**: Llama2, Mistral, CodeLlama gratuitos
- **GPT4All**: Modelos locales variados
- **LocalAI**: Compatible con OpenAI API
- **Ventajas**: Sin costos, privacidad
- **Desventajas**: Requiere recursos, calidad variable

#### Opción B: APIs de LLM
- **OpenAI GPT-3.5/4**: $0.001-0.03 per 1K tokens
- **Anthropic Claude**: Cuota gratuita limitada
- **Google Bard/Gemini**: Cuota gratuita
- **Ventajas**: Alta calidad, sin configuración
- **Desventajas**: Costos escalables

#### Opción C: Servicios Open Source
- **Hugging Face Inference API**: Cuota gratuita
- **Together AI**: Cuota gratuita inicial
- **Ventajas**: Balance calidad/costo
- **Desventajas**: Límites de cuota

## 3. Esquema de Configuración JSON

```json
{
  "system_config": {
    "version": "1.0",
    "environment": "development|production",
    "log_level": "DEBUG|INFO|WARNING|ERROR"
  },
  "document_processor": {
    "type": "local|api|docker",
    "config": {
      "word_library": "python-docx|docx2txt",
      "pdf_library": "PyMuPDF|pdfplumber|PyPDF2",
      "api_endpoint": "optional_url",
      "api_key": "optional_key",
      "docker_image": "optional_image",
      "timeout_seconds": 30
    }
  },
  "text_analyzer": {
    "type": "local|api|llm",
    "config": {
      "library": "spacy|nltk|transformers",
      "model_name": "es_core_news_sm|en_core_web_sm",
      "api_endpoint": "optional_url",
      "api_key": "optional_key",
      "llm_model": "llama2|mistral|gpt-3.5"
    }
  },
  "embeddings_generator": {
    "type": "local|api|docker",
    "config": {
      "model_name": "all-MiniLM-L6-v2|text-embedding-ada-002",
      "api_endpoint": "optional_url",
      "api_key": "optional_key",
      "batch_size": 32,
      "max_sequence_length": 512
    }
  },
  "vector_database": {
    "type": "local|cloud|docker",
    "config": {
      "db_type": "chromadb|pinecone|weaviate|qdrant",
      "connection_string": "path_or_url",
      "api_key": "optional_key",
      "collection_name": "contracts",
      "dimension": 384,
      "metric": "cosine|euclidean|dot"
    }
  },
  "data_exporter": {
    "config": {
      "output_format": "excel|csv|json",
      "file_path": "./output/",
      "excel_sheet_name": "contract_registry",
      "include_metadata": true
    }
  },
  "query_engine": {
    "type": "local|api",
    "config": {
      "llm_model": "ollama/llama2|openai/gpt-3.5",
      "api_endpoint": "optional_url",
      "api_key": "optional_key",
      "max_tokens": 1000,
      "temperature": 0.1,
      "context_window": 4000
    }
  },
  "contract_template": {
    "reference_file": "./templates/contract_model.docx",
    "field_mappings": {
      "propietario_nombre": ["locador", "propietario", "dueño"],
      "inquilino_nombre": ["locatario", "inquilino", "arrendatario"],
      "propiedad_direccion": ["inmueble", "propiedad", "dirección"],
      "precio_inicial": ["canon", "alquiler", "renta mensual"],
      "fecha_inicio": ["inicio", "vigencia desde"],
      "fecha_fin": ["finalización", "vencimiento"],
      "tipo_incremento": ["incremento", "ajuste", "actualización"],
      "valor_incremento": ["porcentaje", "coeficiente", "%"]
    }
  }
}
```

## 4. Interfaz de Línea de Comandos

### 4.1 Estructura de Comandos

```bash
# Comando principal
python contract_processor.py [SUBCOMMAND] [OPTIONS]

# Subcomandos disponibles:
- process     # Procesar documentos (Fase 1)
- query       # Consultar base vectorial (Fase 2)  
- monitor     # Monitorear carpeta (Fase 3)
- config      # Gestionar configuración
- status      # Ver estado del sistema
```

### 4.2 Parámetros por Subcomando

```bash
# Procesamiento (Fase 1)
python contract_processor.py process \
  --config config.json \
  --input-dir /path/to/contracts \
  --output-file contracts_registry.xlsx \
  --template-file contract_model.docx \
  --batch-size 10 \
  --verbose

# Consultas (Fase 2)
python contract_processor.py query \
  --config config.json \
  --interactive \
  --query "Contratos que vencen en 2024"

# Configuración
python contract_processor.py config \
  --init \
  --template \
  --validate config.json
```

## 5. Archivo de Instrucciones/Plantilla

### 5.1 Viabilidad y Riesgos

**VIABILIDAD: ALTA** ✅

#### Factores Positivos:
- **Separación de configuración**: JSON config independiente del código
- **Validación previa**: Parámetros validados antes de ejecución
- **Rollback capability**: Fácil revertir cambios de configuración
- **Audit trail**: Log de cambios en configuración
- **Flexibilidad**: Múltiples plantillas para diferentes escenarios

#### Riesgos Controlables:
- **Inyección de código**: Mitigado con validación estricta de JSON
- **Exposición de credenciales**: Archivo de configuración con permisos restringidos
- **Configuración inválida**: Validación previa obligatoria
- **Conflicto de versiones**: Versionado de plantillas

### 5.2 Estructura de Plantilla de Instrucciones

```json
{
  "execution_template": {
    "name": "standard_contract_processing",
    "version": "1.0",
    "description": "Plantilla estándar para procesamiento de contratos",
    "created_date": "2024-01-01",
    "author": "system"
  },
  "execution_parameters": {
    "input_directory": "/data/contracts",
    "output_directory": "/data/output",
    "output_filename": "contracts_registry_{timestamp}.xlsx",
    "batch_processing": {
      "enabled": true,
      "batch_size": 10,
      "parallel_processing": false
    },
    "file_filters": {
      "extensions": [".docx", ".pdf"],
      "exclude_patterns": ["~$*", "temp_*"],
      "min_file_size_kb": 10,
      "max_file_size_mb": 50
    },
    "processing_options": {
      "skip_duplicates": true,
      "overwrite_existing": false,
      "backup_original": true,
      "extract_images": false
    },
    "error_handling": {
      "continue_on_error": true,
      "max_retries": 3,
      "error_log_file": "processing_errors.log",
      "quarantine_failed_files": true
    },
    "output_options": {
      "include_raw_text": false,
      "include_confidence_scores": true,
      "generate_summary_report": true,
      "export_format": "excel"
    }
  },
  "validation_rules": {
    "required_fields": [
      "propietario_nombre",
      "inquilino_nombre", 
      "propiedad_direccion",
      "precio_inicial"
    ],
    "optional_fields": [
      "fecha_inicio",
      "fecha_fin",
      "tipo_incremento",
      "valor_incremento"
    ],
    "data_quality": {
      "min_confidence_score": 0.7,
      "require_date_validation": true,
      "require_price_validation": true
    }
  },
  "notification_settings": {
    "enable_notifications": false,
    "email_on_completion": false,
    "slack_webhook": null,
    "progress_updates_interval": 100
  }
}
```

## 6. Recomendaciones de Implementación Inicial

### 6.1 Stack Recomendado para Prototipo (Costo Cero)

1. **Procesador de Documentos**: python-docx + PyMuPDF (Opción A)
2. **Motor de Análisis**: spaCy + modelos gratuitos (Opción A)
3. **Generador de Embeddings**: sentence-transformers (Opción A)
4. **Base de Datos Vectorial**: ChromaDB (Opción A)
5. **Motor de Consultas**: Ollama + Llama2 (Opción A)

### 6.2 Stack Alternativo Escalable (Cuotas Gratuitas)

1. **Procesador de Documentos**: python-docx + PyMuPDF (Opción A)
2. **Motor de Análisis**: AWS Comprehend (Opción B)
3. **Generador de Embeddings**: OpenAI Ada (Opción B)
4. **Base de Datos Vectorial**: Pinecone (Opción B)
5. **Motor de Consultas**: OpenAI GPT-3.5 (Opción B)

## 7. Próximos Pasos

1. **Selección de componentes**: Elegir opciones específicas por categoría
2. **Definición de arquitectura concreta**: Especificar implementaciones
3. **Generación de código base**: Crear estructura de proyecto
4. **Configuración de entorno**: Setup de dependencias y servicios
5. **Implementación por fases**: Desarrollo iterativo