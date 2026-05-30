# **Taller 1 Pipeline de MLOps**

**Equipo**:

| Nombres                       | Grupo   |
| ----------------------------- | ------- |
| Anderson Daniel Pipicano Ruiz | Grupo 2 |
| Fredy Yamid Alvarez Palechor  | Grupo 2 |

---

# Descripción del problema

Actualmente, el sector salud genera grandes volúmenes de información provenientes de historias clínicas, registros hospitalarios, laboratorios y plataformas epidemiológicas, creando la posibilidad de implementar soluciones inteligentes que apoyen el diagnóstico médico y mejoren la toma de decisiones clínicas. En este contexto, se propone desarrollar una solución basada en Machine Learning y MLOps capaz de predecir la posible presencia de enfermedades comunes y huérfanas a partir de síntomas, antecedentes y datos clínicos de los pacientes, integrando información proveniente de múltiples fuentes médicas. La solución busca reducir diagnósticos tardíos o incorrectos, optimizar la priorización de pacientes y mejorar la asignación de recursos médicos mediante modelos predictivos confiables, monitoreados y capaces de adaptarse continuamente a nuevos datos y cambios epidemiológicos.

El pipeline integra procesos de adquisición de datos, aseguramiento de calidad, ingeniería de características, entrenamiento de modelos, despliegue de servicios inteligentes y monitoreo continuo, permitiendo construir una solución escalable, reproducible y adaptable a nuevos datos clínicos.

---

# 1. Fuentes de datos

La primera etapa corresponde a las fuentes de información utilizadas para alimentar el sistema analítico. La solución utilizará información proveniente de múltiples fuentes del sector salud, como historias clínicas electrónicas, registros hospitalarios, sistemas RIPS, plataformas gubernamentales como SISPRO y SIVIGILA, resultados de laboratorio y bases de datos epidemiológicas. Estas fuentes pueden encontrarse almacenadas en bases de datos relacionales, bases de datos no relacionales y servicios expuestos mediante APIs, archivos planos y repositorios de imágenes médicas, permitiendo integrar información clínica proveniente de diferentes sistemas y plataformas. Los datos contienen información relacionada con síntomas, antecedentes médicos, diagnósticos, signos vitales, variables demográficas y evolución clínica de los pacientes. Debido a la diversidad de orígenes, el sistema debe integrar tanto datos estructurados, almacenados en tablas y bases de datos, como datos no estructurados, por ejemplo, notas médicas en texto libre o reportes clínicos, además de posibles datos temporales asociados al seguimiento y evolución del paciente.

Dentro de las principales restricciones y limitaciones del problema se encuentra el desbalance de información entre enfermedades comunes y enfermedades huérfanas, ya que estas últimas poseen muy pocos registros disponibles para entrenamiento. Asimismo, los datos médicos suelen presentar inconsistencias, duplicidad, valores faltantes y diferentes formatos provenientes de múltiples sistemas clínicos, lo que hace necesario implementar procesos de validación, limpieza y normalización. Adicionalmente, la solución debe considerar aspectos de privacidad y protección de datos sensibles, así como la necesidad de interpretabilidad del modelo, debido a que las predicciones deben ser comprensibles y confiables para el personal médico encargado de la toma de decisiones clínicas.

Estas fuentes contienen información heterogénea relacionada con:

* Síntomas.
* Diagnósticos.
* Variables demográficas.
* Resultados de laboratorio.
* Evolución clínica del paciente.
* Antecedentes médicos.

---

# 2. Ingesta y Calidad del Dato

La etapa de ingesta y calidad del dato tiene como objetivo integrar y preparar la información proveniente de diferentes fuentes clínicas y epidemiológicas, como bases de datos relacionales y no relacionales, APIs, archivos planos e imágenes médicas. El pipeline contempla procesos de ingesta batch y streaming para capturar tanto información histórica como datos en tiempo real, permitiendo consolidar registros asociados a síntomas, antecedentes médicos, diagnósticos y evolución clínica de los pacientes. Posteriormente, los datos son transformados y estandarizados para garantizar compatibilidad entre los diferentes sistemas de origen.

Una vez integrada la información, se ejecutan procesos de validación y aseguramiento de calidad para detectar registros duplicados, valores faltantes, inconsistencias, errores de formato y datos fuera de rango clínico. En el caso de archivos e imágenes, también se verifican aspectos como integridad y estructura. Finalmente, se realizan tareas de limpieza y normalización de variables médicas con el fin de garantizar que la información utilizada por los modelos sea consistente, confiable y adecuada para el entrenamiento y despliegue de soluciones de Machine Learning.

---

# 3. Procesamiento e Ingeniería de Datos

La etapa de procesamiento e ingeniería de datos tiene como propósito transformar la información clínica en variables útiles para el entrenamiento de los modelos de Machine Learning. En esta fase se realizan procesos de limpieza, normalización y codificación de datos, permitiendo convertir variables categóricas, síntomas y registros clínicos en representaciones numéricas interpretables por los algoritmos. Asimismo, se manejan valores faltantes y se aplican transformaciones que permitan unificar escalas y formatos entre diferentes fuentes de información.

Posteriormente, se desarrolla la ingeniería de características, donde se construyen nuevas variables derivadas a partir de síntomas, antecedentes médicos, resultados de laboratorio y evolución clínica de los pacientes. También pueden generarse variables temporales, agrupaciones de síntomas o representaciones semánticas provenientes de texto clínico e imágenes médicas. Esta etapa es fundamental para extraer patrones relevantes y mejorar la capacidad predictiva de los modelos, especialmente en escenarios de enfermedades huérfanas donde la información disponible es limitada y altamente desbalanceada.

---

# 4. Entrenamiento y Modelos

La etapa de entrenamiento y modelos tiene como objetivo desarrollar soluciones predictivas capaces de identificar posibles enfermedades a partir de la información clínica procesada. Dependiendo del tipo de datos disponibles, pueden utilizarse modelos de Machine Learning tradicionales como Random Forest, XGBoost, LightGBM o CatBoost para datos tabulares y estructurados, así como modelos de Deep Learning como CNN para imágenes médicas y Transformers para texto clínico. Adicionalmente, pueden emplearse técnicas no supervisadas como K-Means y HDBSCAN para segmentación de pacientes, identificación de patrones clínicos y detección de agrupamientos asociados a posibles enfermedades. Debido al desbalance existente en enfermedades huérfanas, también pueden incorporarse estrategias como balanceo de clases, transfer learning o modelos híbridos para mejorar la capacidad de generalización.

El entrenamiento del modelo se realiza utilizando conjuntos de datos divididos en entrenamiento, validación y prueba, evitando fuga de información entre pacientes y garantizando una evaluación confiable del desempeño. Durante esta etapa se ejecutan procesos de ajuste de hiperparámetros, validación cruzada y comparación entre diferentes algoritmos con el fin de seleccionar el modelo más robusto y preciso. La evaluación se realiza mediante métricas como precision, recall, F1-score y ROC-AUC, priorizando especialmente la reducción de falsos negativos debido al impacto clínico que puede representar un diagnóstico no detectado. Finalmente, los modelos y resultados obtenidos son versionados y registrados para garantizar trazabilidad, reproducibilidad y control sobre futuras actualizaciones del sistema.

---

# 5. Servicios de Modelos y Consumo

La etapa de servicios de modelos y consumo tiene como objetivo desplegar los modelos de Machine Learning en una infraestructura centralizada que permita su acceso desde diferentes sistemas clínicos y aplicaciones médicas. Los modelos entrenados serán empaquetados en contenedores y desplegados en plataformas cloud como Google Cloud Platform (GCP), Microsoft Azure o AWS, utilizando servicios de orquestación y ejecución que permitan escalar automáticamente según la demanda de consultas médicas. Esto permitirá mantener alta disponibilidad, tolerancia a fallos y capacidad de procesamiento en tiempo real para múltiples instituciones o usuarios concurrentes.

Una vez desplegados, los modelos serán expuestos mediante APIs REST seguras y endpoints específicos para inferencia, autenticados mediante tokens o credenciales institucionales. A través de estos servicios, aplicaciones hospitalarias, dashboards clínicos, historias clínicas electrónicas o plataformas web podrán enviar información de pacientes en formato JSON y recibir como respuesta predicciones asociadas a posibles enfermedades, probabilidades de riesgo y explicaciones generadas por el modelo. Los usuarios clínicos accederán a estas funcionalidades desde interfaces web o sistemas hospitalarios integrados, sin necesidad de interactuar directamente con la infraestructura del modelo.

---

# 6. IA Generativa y RAG

La etapa de IA Generativa y RAG se incorpora como un componente adicional orientado a mejorar la interacción entre el sistema y el usuario clínico, funcionando como un apoyo inteligente para la interpretación y análisis de la información médica. Mediante técnicas de Retrieval-Augmented Generation (RAG), el sistema podrá consultar información relevante desde bases documentales, guías clínicas, antecedentes médicos o resultados generados por los modelos predictivos, permitiendo generar respuestas contextualizadas y más comprensibles para el personal médico.

Este componente también facilitará el procesamiento de texto clínico proveniente de historias médicas, notas de evolución o reportes hospitalarios, ayudando a resumir información relevante y extraer contexto útil para el diagnóstico. Adicionalmente, la IA generativa podrá asistir en la interpretación de las predicciones generadas por los modelos de Machine Learning, proporcionando explicaciones más claras sobre posibles factores de riesgo, síntomas relevantes o patrones detectados, fortaleciendo así la toma de decisiones clínicas y la interacción del usuario con la solución analítica.

---

# 7. Monitoreo y Observabilidad

La etapa de monitoreo y observabilidad es fundamental debido a la criticidad del entorno médico y al impacto que pueden tener las predicciones sobre los pacientes. El sistema debe supervisar continuamente métricas de desempeño del modelo, como precisión, recall, F1-score, tasa de falsos negativos y tiempos de respuesta, con el fin de garantizar que las predicciones mantengan un nivel adecuado de confiabilidad y estabilidad en producción. Asimismo, se monitorea la calidad de los datos de entrada para detectar inconsistencias, valores atípicos, registros incompletos o cambios en la estructura de la información proveniente de las diferentes fuentes clínicas.

Adicionalmente, el pipeline contempla mecanismos de detección de drift de datos y drift de concepto para identificar variaciones en patrones epidemiológicos, aparición de nuevas enfermedades o cambios en el comportamiento de la población que puedan degradar el desempeño del modelo. Debido a que continuamente se generan nuevos registros clínicos y datos médicos, el sistema debe permitir procesos periódicos de reentrenamiento utilizando información más reciente y validada. Antes de desplegar nuevas versiones del modelo, estas deben pasar nuevamente por procesos de evaluación técnica y validación clínica, garantizando trazabilidad, control de versiones y mejora continua de la solución.

---

# Conexión Integral del Pipeline

El pipeline funciona como un ecosistema continuo e integrado donde cada etapa alimenta a la siguiente.

1. Las fuentes clínicas suministran datos heterogéneos.
2. La ingesta consolida y valida la información.
3. El procesamiento transforma los datos en variables analíticas.
4. Los modelos aprenden patrones clínicos y generan predicciones.
5. Los servicios permiten consumir esas predicciones en aplicaciones reales.
6. La IA generativa mejora la interacción y explicabilidad.
7. El monitoreo observa continuamente el comportamiento del sistema y retroalimenta el pipeline con nuevos datos para futuros reentrenamientos.

---

# 8. Diagrama general 

Imagen 1. Diagrama general del proceso
![alt text](img/pipeline_inical.png)

---

Imagen 2. Tecnologías recomendadas 
![alt text](img/tecnologia.png)
