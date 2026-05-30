# **Taller Pipeline de MLOps**

**Equipo**:

| Nombres                       | Grupo   |
| ----------------------------- | ------- |
| Anderson Daniel Pipicano Ruiz | Grupo 2 |
| Fredy Yamid Alvarez Palechor  | Grupo 2 |

## **Descripción del problema**

Actualmente, el sector salud genera grandes volúmenes de información provenientes de **historias clínicas, registros hospitalarios, laboratorios y plataformas epidemiológicas**, creando la posibilidad de implementar soluciones inteligentes que apoyen el diagnóstico médico y mejoren la toma de decisiones clínicas. En este contexto, se propone desarrollar una solución basada en Machine Learning y MLOps capaz de predecir la posible presencia de enfermedades comunes y huérfanas a partir de síntomas, antecedentes y datos clínicos de los pacientes, integrando información proveniente de múltiples fuentes médicas. La solución busca **reducir diagnósticos tardíos o incorrectos, optimizar la priorización de pacientes y mejorar la asignación de recursos médicos mediante modelos predictivos confiables, monitoreados y capaces de adaptarse continuamente a nuevos datos y cambios epidemiológicos**.

El pipeline integra procesos de **adquisición de datos, aseguramiento de calidad, ingeniería de características, entrenamiento de modelos, despliegue de servicios inteligentes y monitoreo continuo, permitiendo construir una solución escalable, reproducible y adaptable a nuevos datos clínicos**.

![alt text](img/pipeline_mlops.png)

## **1. Fuentes de Datos**

La primera etapa del pipeline corresponde a las **fuentes de información utilizadas para alimentar el sistema analítico y los modelos de Machine Learning**. La solución propuesta estará enfocada **exclusivamente en el procesamiento de datos tabulares provenientes de diferentes entidades y sistemas del sector salud** permitiendo consolidar información clínica estructurada para el entrenamiento y despliegue de modelos predictivos orientados a la detección de enfermedades comunes y enfermedades huérfanas.

Las **principales fuentes de información** consideradas incluyen **historias clínicas electrónicas (HCE), registros hospitalarios, sistemas RIPS**, plataformas gubernamentales como **SISPRO y SIVIGILA, resultados de laboratorio clínico y bases de datos epidemiológicas**. Estas fuentes contienen información relevante relacionada con síntomas, antecedentes médicos, **diagnósticos previos, signos vitales, tratamientos, variables demográficas y evolución clínica de los pacientes**.

Debido a la heterogeneidad de los sistemas clínicos, los datos pueden **encontrarse almacenados en múltiples tecnologías y formatos**. Entre las principales fuentes se contemplan **bases de datos relacionales como PostgreSQL y MySQL, archivos planos en formatos CSV y JSON** así como servicios **expuestos mediante APIs REST** interoperables bajo estándares médicos como **HL7 y FHIR**. La integración de estas fuentes permitirá consolidar información proveniente de múltiples instituciones médicas y plataformas hospitalarias.

La arquitectura **cloud-native propuesta utilizará Google Cloud Platform (GCP)** como plataforma **centralizada de almacenamiento y administración de datos clínicos**. Los datos provenientes de las diferentes fuentes externas serán **integrados y almacenados inicialmente en Google Cloud Storage** el cual funcionará como Data Lake centralizado para conservar tanto información histórica como nuevos registros clínicos provenientes de **hospitales, laboratorios y entidades gubernamentales**.

Posteriormente, la información estructurada será **consolidada en BigQuery, permitiendo disponer de un entorno analítico escalable para consultas, integración y consumo por parte de las etapas posteriores del pipeline MLOps**. Esta centralización facilitará la trazabilidad de los datos, el acceso controlado a la información clínica y la interoperabilidad entre múltiples sistemas médicos.

La **adquisición de información podrá realizarse tanto en modalidad batch como streaming**. Los procesos batch estarán orientados a la carga periódica de **registros clínicos históricos, resultados de laboratorio y bases epidemiológicas** mientras que los procesos streaming permitirán incorporar actualizaciones clínicas en tiempo real, como nuevos diagnósticos o eventos hospitalarios recientes.

Dentro de las principales restricciones del **problema se encuentra el alto desbalance existente entre enfermedades comunes y enfermedades huérfanas** debido a la **baja disponibilidad de registros asociados a enfermedades raras**. Esta limitación representa un reto importante para el entrenamiento de modelos predictivos, ya que puede generar **sesgos y dificultades de generalización sobre clases minoritarias**.

Adicionalmente, la información médica proveniente de múltiples instituciones puede **presentar diferencias semánticas, formatos heterogéneos y distintos estándares de codificación clínica** lo que hace necesaria la implementación posterior de procesos de **validación, homologación y normalización de datos**.

Las principales variables clínicas consideradas dentro de las fuentes de información incluyen:

- Síntomas reportados por el paciente.
- Diagnósticos clínicos previos.
- Variables demográficas.
- Resultados de laboratorio.
- Antecedentes médicos.
- Evolución clínica y temporal del paciente.
- Tratamientos y medicamentos registrados.

![alt text](img/fuente_datos.png)

## **2. Ingesta y Calidad del Dato**

Una vez centralizada la **información clínica en Google Cloud Storage y BigQuery**, el pipeline continúa con la etapa de **ingesta y calidad del dato**, cuyo **objetivo principal es consumir, validar, transformar y estandarizar** la información médica que será utilizada posteriormente en los procesos de analítica avanzada y entrenamiento de modelos de Machine Learning.

Esta etapa constituye uno de los componentes más críticos dentro de la arquitectura MLOps, ya que garantiza que los **modelos predictivos sean entrenados utilizando información consistente, confiable y clínicamente válida**. Debido a la naturaleza heterogénea de las fuentes médicas, es necesario **implementar procesos automatizados de integración y control de calidad** que permitan reducir inconsistencias y problemas asociados a errores de codificación, registros incompletos o diferencias semánticas entre instituciones hospitalarias.

La arquitectura propuesta utilizará **Google Cloud Dataflow** como servicio principal para la **construcción de pipelines ETL/ELT distribuidos y escalables**. **Dataflow** permitirá procesar grandes volúmenes de información clínica tanto en modalidad batch como streaming, integrándose de manera nativa con los servicios centralizados definidos en la etapa anterior.

Los **procesos batch estarán orientados al consumo periódico de datasets históricos almacenados en Cloud Storage y BigQuery**, permitiendo ejecutar cargas masivas relacionadas con historias clínicas, registros epidemiológicos y resultados de laboratorio. Por otra parte, los procesos streaming permitirán procesar eventos clínicos en tiempo real provenientes de Pub/Sub, como nuevos diagnósticos, actualizaciones de pacientes o resultados médicos recientes.

Dentro del flujo de integración, Dataflow realizará consultas y extracción de información desde las capas centralizadas de almacenamiento definidas en BigQuery y Cloud Storage, consolidando los datos requeridos para las siguientes etapas analíticas del pipeline. Esta arquitectura desacoplada permitirá mantener independencia entre los sistemas de origen y los procesos de procesamiento analítico, facilitando la escalabilidad y mantenimiento de la solución.

Una vez consumida la información, los pipelines de Dataflow ejecutarán procesos automatizados de validación y transformación de datos clínicos, incluyendo tareas como:

- validación estructural de registros,
- identificación de datos faltantes,
- eliminación de duplicados,
- normalización de variables clínicas,
- homologación de formatos médicos,
- validación de tipos de datos,
- verificación de rangos clínicamente válidos,
- y estandarización semántica entre instituciones de salud.

Por ejemplo, durante esta etapa se **podrán detectarse inconsistencias relacionadas con diagnósticos mal codificados, síntomas registrados con nomenclaturas diferentes entre hospitales, valores fisiológicos fuera de rangos clínicamente posibles o registros incompletos asociados a pacientes**.

Adicionalmente, se implementarán reglas automáticas de calidad del dato sobre atributos críticos relacionados con síntomas, antecedentes médicos, resultados de laboratorio y variables demográficas. Estas validaciones permitirán medir indicadores asociados a:

- completitud,
- unicidad,
- consistencia,
- integridad,
- exactitud,
- y distribución estadística de los datos clínicos.

Como parte del **proceso de normalización**, la solución **incorporará un diccionario centralizado** de variables **médicas y catálogos** estandarizados de **diagnósticos y síntomas**, permitiendo homologar información proveniente de múltiples instituciones hospitalarias y reducir problemas derivados de diferencias semánticas o variaciones de codificación clínica.

Posteriormente, los datos procesados y validados serán almacenados nuevamente en BigQuery dentro de capas analíticas optimizadas para consumo por parte de las etapas de procesamiento, ingeniería de características y entrenamiento de modelos. Esta **estructura permitirá mantener separación entre datos crudos, datos procesados y datasets analíticos listos para Machine Learning**.

Debido a la **sensibilidad de la información médica, toda la etapa de ingestión y procesamiento** mantendrá mecanismos de **seguridad y control** de acceso mediante **Cloud IAM, Secret Manager y Cloud KMS** garantizando protección de la información clínica tanto en tránsito como en reposo.

Finalmente, esta etapa permite asegurar que las decisiones clínicas soportadas por los modelos de **Machine Learning se construyan sobre información validada, estandarizada y confiable** reduciendo riesgos asociados a errores de calidad del dato, sesgos analíticos y degradación del desempeño predictivo.

![alt text](img/ingesta_calidad_dato.png)

## **3. Procesamiento e Ingeniería de Datos**

La etapa de **procesamiento e ingeniería de datos** tiene como propósito transformar la información clínica en variables útiles para el entrenamiento de los modelos de Machine Learning. En esta fase se realizan **procesos de limpieza, normalización y codificación de datos**, permitiendo **convertir variables categóricas, síntomas y registros clínicos en representaciones numéricas interpretables por los algoritmos**. Asimismo, se manejan valores faltantes y se aplican transformaciones que permitan unificar escalas y formatos entre diferentes fuentes de información.

Posteriormente, se desarrolla la **ingeniería de características**, donde se construyen **nuevas variables derivadas a partir de síntomas, antecedentes médicos, resultados de laboratorio y evolución clínica de los pacientes**. También pueden **generarse variables temporales, agrupaciones de síntomas o representaciones semánticas provenientes de texto clínico e imágenes médicas**. Esta etapa es **fundamental para extraer patrones relevantes y mejorar la capacidad predictiva de los modelos** especialmente en escenarios de enfermedades huérfanas donde la información disponible es limitada y altamente desbalanceada.

![alt text](img/procesamiento_ingenieria.png)

## **4. Entrenamiento y Modelos**

La etapa de **entrenamiento y modelos** tiene como **objetivo desarrollar soluciones predictivas capaces de identificar posibles enfermedades a partir de la información clínica procesada**. Dependiendo del tipo de datos disponibles pueden utilizarse **modelos de Machine Learning tradicionales como Random Forest, XGBoost, LightGBM o CatBoost** para **datos tabulares y estructurados**, así como **modelos de Deep Learning como CNN para imágenes médicas y Transformers para texto clínico**. Adicionalmente, pueden emplearse técnicas no **supervisadas como K-Means y HDBSCAN** para **segmentación de pacientes, identificación de patrones clínicos y detección de agrupamientos asociados a posibles enfermedades**. Debido al desbalance existente en enfermedades huérfanas, también pueden incorporarse estrategias como balanceo de clases, transfer learning o modelos híbridos para mejorar la capacidad de generalización.

El **entrenamiento del modelo se realiza utilizando conjuntos de datos divididos en entrenamiento, validación y prueba**, evitando fuga de información entre pacientes y garantizando una evaluación confiable del desempeño. Durante esta etapa se ejecutan procesos de **ajuste de hiperparámetros, validación cruzada y comparación entre diferentes algoritmos** con el fin de **seleccionar el modelo más robusto y preciso**. La evaluación se realiza mediante **métricas como precision, recall, F1-score y ROC-AUC**, priorizando especialmente la **reducción de falsos negativos** debido al impacto clínico que puede representar un diagnóstico no detectado. Finalmente los **modelos y resultados obtenidos son versionados y registrados para garantizar trazabilidad, reproducibilidad y control sobre futuras actualizaciones del sistema**.

![alt text](img/entrenamiento_modelo.png)

## **5. Automatizacion y despliegue**

![alt text](img/automatizacion_despliegue.png)

## **6. Servicios de Modelos y Consumo**

La etapa de servicios de **modelos y consumo** tiene como **objetivo desplegar los modelos de Machine Learning en una infraestructura centralizada que permita su acceso desde diferentes sistemas clínicos y aplicaciones médicas**. Los modelos entrenados serán empaquetados en **contenedores y desplegados en plataformas cloud como Google Cloud Platform (GCP), Microsoft Azure o AWS** utilizando servicios de orquestación y ejecución que permitan escalar automáticamente según la demanda de consultas médicas. Esto permitirá mantener alta disponibilidad, tolerancia a fallos y capacidad de procesamiento en tiempo real para múltiples instituciones o usuarios concurrentes.

Una vez desplegados, los modelos serán **expuestos mediante APIs REST seguras y endpoints específicos para inferencia** autenticados mediante tokens o credenciales institucionales. A través de estos **servicios, aplicaciones hospitalarias, dashboards clínicos, historias clínicas electrónicas o plataformas web** podrán enviar información de pacientes en **formato JSON** para recibir respuesta predicciones asociadas a **posibles enfermedades, probabilidades de riesgo y explicaciones generadas por el modelo**. Los usuarios clínicos accederán a estas funcionalidades desde interfaces web o sistemas hospitalarios integrados, sin necesidad de interactuar directamente con la infraestructura del modelo.

![alt text](img/nodelo_consumo.png)

## **7. IA Generativa y RAG**

La etapa de **IA Generativa y RAG** se incorpora como un componente adicional orientado a **mejorar la interacción entre el sistema y el usuario clínico** funcionando como un **apoyo inteligente para la interpretación y análisis de la información médica**. Mediante **técnicas de Retrieval-Augmented Generation (RAG)** el sistema podrá consultar información relevante desde bases documentales, guías clínicas, antecedentes médicos o resultados generados por los modelos predictivos, permitiendo generar respuestas contextualizadas y más comprensibles para el personal médico.

Este componente también **facilitará el procesamiento de texto clínico proveniente de historias médicas, notas de evolución o reportes hospitalarios** ayudando a resumir información relevante y extraer contexto útil para el diagnóstico. Adicionalmente, la **IA generativa podrá asistir en la interpretación de las predicciones generadas** por los modelos de Machine Learning, proporcionando explicaciones más claras sobre posibles **factores de riesgo, síntomas relevantes o patrones detectados, fortaleciendo así la toma de decisiones clínicas y la interacción del usuario con la solución analítica**.

![alt text](img/ia.png)

## **8. Monitoreo y Observabilidad**

La etapa de **monitoreo y observabilidad** es fundamental debido a la criticidad del entorno médico y al impacto que pueden tener las predicciones sobre los pacientes. **El sistema debe supervisar continuamente métricas de desempeño del modelo a trevez de las metricas como precisión, recall, F1-score, tasa de falsos negativos y tiempos de respuesta** con el fin de garantizar que las predicciones mantengan un nivel adecuado de confiabilidad y estabilidad en producción. Asimismo se **monitorea la calidad de los datos de entrada para detectar inconsistencias, valores atípicos, registros incompletos o cambios en la estructura de la información proveniente de las diferentes fuentes clínicas**.

Adicionalmente, el pipeline contempla **mecanismos de detección de drift de datos y drift de concepto** para identificar variaciones en **patrones epidemiológicos, aparición de nuevas enfermedades o cambios en el comportamiento de la población que puedan degradar el desempeño del modelo**. Debido a que continuamente se generan nuevos registros clínicos y datos médicos, el sistema debe permitir procesos periódicos de reentrenamiento utilizando información más reciente y validada. Antes de desplegar **nuevas versiones del modelo, estas deben pasar nuevamente por procesos de evaluación técnica y validación clínica, garantizando trazabilidad, control de versiones y mejora continua de la solución**.

![alt text](img/monitoreo.png)

## **Conexión Integral del Pipeline**

El pipeline funciona como un ecosistema continuo e integrado donde cada etapa alimenta a la siguiente.

1. Las fuentes clínicas suministran datos heterogéneos.
2. La ingesta consolida y valida la información.
3. El procesamiento transforma los datos en variables analíticas.
4. Los modelos aprenden patrones clínicos y generan predicciones.
5. Los servicios permiten consumir esas predicciones en aplicaciones reales.
6. La IA generativa mejora la interacción y explicabilidad.
7. El monitoreo observa continuamente el comportamiento del sistema y retroalimenta el pipeline con nuevos datos para futuros reentrenamientos.
8. Diagrama general

### **1. Diagrama general del proceso**

![alt text](img/pipeline_mlops.png)

### **2. Otras tecnologías recomendadas**

![alt text](img/tecnologia.png)
