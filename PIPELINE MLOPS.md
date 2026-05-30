# Taller Pipeline de MLOps

**Integrantes:**
* Anderson Daniel Pipicano Ruiz
* Fredy Yamid Alvarez Palechor

**Grupo:** 2

---

## Descripción del problema

Actualmente, el sector salud genera grandes volúmenes de información provenientes de historias clínicas, registros hospitalarios, laboratorios y plataformas epidemiológicas, creando la posibilidad de implementar soluciones inteligentes que apoyen el diagnóstico médico y mejoren la toma de decisiones clínicas. 

En este contexto, se propone desarrollar una solución basada en Machine Learning y MLOps capaz de predecir la posible presencia de enfermedades comunes y huérfanas a partir de síntomas, antecedentes y datos clínicos de los pacientes, integrando información proveniente de múltiples fuentes médicas. La solución busca reducir diagnósticos tardíos o incorrectos, optimizar la priorización de pacientes y mejorar la asignación de recursos médicos mediante modelos predictivos confiables, monitoreados y capaces de adaptarse continuamente a nuevos datos y cambios epidemiológicos.  

El pipeline integra procesos de adquisición de datos, aseguramiento de calidad, ingeniería de características, entrenamiento de modelos, despliegue de servicios inteligentes y monitoreo continuo, permitiendo construir una solución escalable, reproducible y adaptable a nuevos datos clínicos.  

### Fuentes de datos
La primera etapa corresponde a las fuentes de información utilizadas para alimentar el sistema analítico. La solución utilizará información proveniente de múltiples fuentes del sector salud, como:
* Historias clínicas electrónicas.
* Registros hospitalarios.
* Sistemas RIPS.
* Plataformas gubernamentales (SISPRO y SIVIGILA).
* Resultados de laboratorio.
* Bases de datos epidemiológicas.

Estas fuentes pueden encontrarse almacenadas en bases de datos relacionales, bases de datos no relacionales y servicios expuestos mediante APIs, archivos planos y repositorios de imágenes médicas, permitiendo integrar información clínica proveniente de diferentes sistemas y plataformas. Debido a la diversidad de orígenes, el sistema debe integrar tanto datos estructurados (tablas y bases de datos) como datos no estructurados (notas médicas en texto libre o reportes clínicos), además de posibles datos temporales asociados al seguimiento y evolución del paciente.  

Estas fuentes contienen información heterogénea relacionada con:  
* Síntomas.  
* Diagnósticos.  
* Variables demográficas.  
* Resultados de laboratorio.  
* Evolución clínica del paciente.  
* Antecedentes médicos.  

### Restricciones y limitaciones
* Desbalance de información: Gran disparidad entre enfermedades comunes y enfermedades huérfanas, ya que estas últimas poseen muy pocos registros disponibles para entrenamiento. 
* Calidad de los datos: Los datos médicos suelen presentar inconsistencias, duplicidad, valores faltantes y diferentes formatos.
* Privacidad y seguridad: Cumplimiento estricto de normativas sobre la protección de datos sensibles de los pacientes.
* Interpretabilidad: Necesidad de explicabilidad en los modelos, debido a que las predicciones deben ser comprensibles y confiables para el personal médico.

---

## Ingesta y Calidad del Dato

La etapa de ingesta y calidad del dato tiene como objetivo integrar y preparar la información proveniente de las diferentes fuentes clínicas y epidemiológicas. 

* Estrategias de Ingesta: El pipeline contempla procesos de ingesta batch (lotes) y streaming (tiempo real) para capturar tanto información histórica como datos en tiempo real, consolidando los registros médicos. Posteriormente, los datos son transformados y estandarizados para garantizar compatibilidad entre los sistemas de origen.  
* Aseguramiento de Calidad: Una vez integrada la información, se ejecutan procesos de validación para detectar registros duplicados, valores faltantes, inconsistencias, errores de formato y datos fuera de rango clínico. En el caso de archivos e imágenes, se verifica su integridad y estructura.
* Limpieza y Normalización: Se realizan tareas finales de curación de variables médicas con el fin de garantizar que la información utilizada por los modelos sea consistente, confiable y adecuada para el ciclo de Machine Learning.

---

## Procesamiento e Ingeniería de Datos

Esta etapa tiene como propósito transformar la información clínica cruda en variables útiles para el entrenamiento de los modelos.

1. Preprocesamiento: Se realizan procesos de limpieza, normalización y codificación de datos, convirtiendo variables categóricas, síntomas y registros clínicos en representaciones numéricas interpretables por los algoritmos. Asimismo, se manejan los valores faltantes y se unifican las escalas.
2. Ingeniería de Características (Feature Engineering): Se construyen nuevas variables derivadas a partir de síntomas, antecedentes médicos, resultados de laboratorio y evolución clínica. También pueden generarse variables temporales, agrupaciones de síntomas o representaciones semánticas (embeddings) provenientes de texto clínico e imágenes médicas. Esta fase es fundamental para extraer patrones relevantes en escenarios de enfermedades huérfanas donde la información es limitada.

---

## Entrenamiento y Modelos

El objetivo es desarrollar las soluciones predictivas capaces de identificar posibles enfermedades a partir de la información procesada.

### Tipos de Modelos Utilizados
* Datos estructurados/tabulares: Modelos de Machine Learning tradicionales como Random Forest, XGBoost, LightGBM o CatBoost.
* Imágenes médicas: Modelos de Deep Learning basados en Redes Convencionales (CNN).
* Texto clínico (notas médicas): Modelos basados en Arquitectura Transformers.
* Segmentación y Patrones: Técnicas no supervisadas como K-Means y HDBSCAN para identificación de agrupamientos asociados a posibles enfermedades.
* Estrategias para Datos Desbalanceados: Incorporación de balanceo de clases, transfer learning o modelos híbridos.

### Evaluación y Trazabilidad
El entrenamiento se realiza dividiendo los datos en conjuntos de entrenamiento, validación y prueba, evitando la fuga de información (data leakage) entre pacientes. Durante esta etapa se ejecutan procesos de ajuste de hiperparámetros y validación cruzada.

* Métricas de Evaluación: Se evalúa mediante Precision, Recall, F1-score y ROC-AUC. Se prioriza especialmente la reducción de falsos negativos debido al alto impacto clínico que representa un diagnóstico no detectado.
* Versionamiento: Todos los modelos y resultados son registrados bajo un sistema de control de versiones para garantizar la trazabilidad y la reproducibilidad ante futuras actualizaciones.

---

## Servicios de Modelos y Consumo

Esta etapa se enfoca en el despliegue de los modelos en una infraestructura centralizada que permita su acceso desde diferentes sistemas clínicos.

* Infraestructura y Despliegue: Los modelos entrenados son empaquetados en contenedores (ej. Docker) y desplegados en plataformas Cloud (GCP, Microsoft Azure o AWS), utilizando servicios de orquestación que permiten el autoescalado según la demanda de consultas médicas, garantizando alta disponibilidad y tolerancia a fallos.
* Interfaces de Consumo (APIs): Los modelos se exponen mediante APIs REST seguras con autenticación basada en tokens institucionales. 
* Flujo de Información: Las aplicaciones hospitalarias, dashboards o historias clínicas electrónicas envían la información del paciente en formato JSON y reciben de vuelta predicciones asociadas a posibles enfermedades, probabilidades de riesgo y explicaciones del modelo de forma transparente para el personal de salud.

---

## IA Generativa y RAG

Se incorpora como un componente adicional orientado a mejorar la interacción entre el sistema y el usuario clínico, funcionando como un soporte inteligente.

* Tecnología RAG (Retrieval-Augmented Generation): El sistema puede consultar información relevante desde bases documentales, guías clínicas oficiales, antecedentes del paciente o resultados de los modelos predictivos para generar respuestas contextualizadas y comprensibles en lenguaje natural.
* Procesamiento de Texto: Facilita el análisis de texto libre en historias médicas y notas de evolución, ayudando a resumir información y extraer contexto clave.
* Explicabilidad: Asiste en la interpretación de las predicciones de los modelos de Machine Learning, traduciendo métricas complejas en explicaciones claras sobre factores de riesgo y síntomas detectados.

---

## Monitoreo y Observabilidad

Dada la criticidad del entorno médico, el sistema implementa una supervisión continua en producción:

* Métricas de Desempeño: Supervisión en tiempo real de precision, recall, F1-score, tasa de falsos negativos y tiempos de respuesta de la API.
* Calidad de los Datos: Monitoreo de la información de entrada para detectar anomalías, valores atípicos o cambios en las estructuras de los datos.
* Detección de Drift: Implementación de mecanismos para detectar Data Drift (degradación por cambios en los datos de entrada) y Concept Drift (cambios en los patrones epidemiológicos o aparición de nuevas enfermedades).
* Ciclo de Reentrenamiento: El sistema permite procesos periódicos de reentrenamiento con datos nuevos validados. Antes de pasar a producción, toda nueva versión del modelo debe superar una evaluación técnica y una validación clínica.

---

## Conexión Integral del Pipeline

El pipeline funciona como un ecosistema continuo e integrado donde cada etapa alimenta a la siguiente:

```
[Fuentes Clínicas] ──> Suministran datos heterogéneos.
       │
[Ingesta y Calidad] ──> Consolida y valida la información.
       │
[Procesamiento] ──> Transforma los datos en variables analíticas.
       │
[Modelos / ML] ──> Aprenden patrones clínicos y generan predicciones.
       │
[Servicios / API] ──> Permiten consumir las predicciones en aplicaciones reales.
       │
[IA Generativa / RAG] ──> Mejora la interacción, resúmenes y explicabilidad.
       │
[Monitoreo] ──> Observa el comportamiento y retroalimenta con datos para reentrenamiento.
```