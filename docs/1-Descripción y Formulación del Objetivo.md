**APRENDIZAJE AUTOMÁTICO — DESCRIPCIÓN Y FORMULACIÓN DE OBJETIVOS**

**Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo**

### **1\. Descripción del problema, contexto y relevancia**

Las turberas de Sphagnum de Tierra del Fuego acumulan carbono en forma de materia orgánica, y los incendios pueden alterar sus propiedades fisicoquímicas y comprometer esa función. Estimar la severidad del daño suele requerir muestreo de campo y análisis de laboratorio, que son lentos y costosos. Como todos los predictores considerados requieren ensayos de laboratorio, se evaluará la posibilidad de reducir tiempos y costos analíticos al identificar si es factible prescindir de determinaciones específicas de mayor complejidad o costo sin comprometer la precisión de la clasificación de la variable objetivo. 

El problema es determinar si la severidad de afectación de incendios forestales de una muestra de turbera (baja, moderada o alta) puede clasificarse a partir de propiedades del suelo medidas en laboratorio (pH, conductividad eléctrica, materia orgánica, densidad aparente, humedad). Si el modelo funciona, permitiría identificar qué indicadores sirven como señal rápida de daño y priorizar análisis y acciones de restauración.

La relevancia del proyecto radica en evaluar qué indicadores aportan mayor información para caracterizar la condición después de producido el incendio y si un conjunto reducido de mediciones permite obtener resultados comparables a los del conjunto completo. Se plantea como una herramienta experimental de apoyo al análisis ambiental, no como un reemplazo de la evaluación de campo. 

El alcance inicial será tabular y no se interpretará la categoría de severidad baja como ausencia de daño, ya que no existe una clase explícita de muestras no quemadas o incendiadas.

### **2\. Objetivos del trabajo**

**Objetivo general**

Desarrollar y evaluar modelos de Aprendizaje Automático que clasifiquen la severidad del fuego (Low / Moderate / High) en turberas de Sphagnum de Tierra del Fuego a partir de propiedades fisicoquímicas del suelo, comparando Regresión Logística Multinomial y KNN bajo validación agrupada por turbera, e identificar un subconjunto reducido de variables predictoras que preserve la capacidad de discriminación.

**Objetivos específicos**

1. Explorar el dataset, analizando las distribuciones de las variables, los valores atípicos y los valores faltantes.  
2. Preprocesar los datos mediante la identificación y tratamiento de valores faltantes, la codificación de variables categóricas y la estandarización de las variables numéricas, según los requerimientos de cada modelo.  
3. Entrenar y comparar modelos de clasificación supervisada multiclase (Regresión Logística Multinomial y K-NN), utilizando validación cruzada estratificada y agrupada por turbera, para evaluar su capacidad de generalización a nuevas unidades de muestreo.  
4. Evaluar el rendimiento de los modelos mediante F1-score macro, matrices de confusión, precisión y recall por clase, analizando las confusiones entre las categorías de severidad.  
5. Identificar y analizar la importancia relativa de las variables fisicoquímicas para la discriminación de las categorías de severidad del daño provocado por el incendio, con el propósito de seleccionar un subconjunto reducido de predictores.  
6. Comparar cuantitativamente el desempeño de los modelos de clasificación entrenados con el conjunto completo de variables predictoras y con un subconjunto reducido seleccionado según su importancia, utilizando los mismos criterios de evaluación y validación cruzada agrupada por turbera, para determinar cuánto puede reducirse el número de ensayos analíticos sin comprometer significativamente la capacidad predictiva de los modelos y evaluar la factibilidad técnica y económica de dicha reducción.

### **3\. Tipo de problema y variable objetivo**

Es un problema de clasificación supervisada multiclase. 

La unidad de análisis está constituida por cada muestra individual de suelo de turbera, representada por una fila del dataset. El conjunto está compuesto por 231 observaciones procedentes de siete turberas.

Cada registro contiene información sobre la procedencia de la muestra, su clasificación microtopográfica, la categoría de severidad del incendio (Burn\_Severity) y un conjunto de mediciones fisicoquímicas del suelo.

La variable objetivo Burn\_Severity clasifica la severidad del fuego en tres categorías: baja (Low), moderada (Moderate) y alta (High), según el grado de alteración de la vegetación y las propiedades del suelo. La clasificación se fundamenta en los criterios de Keeley (2009) \[1\] y Fulazzaky et al. (2022) \[2\].

* Baja**:** efectos reducidos del fuego y escasa alteración de la materia orgánica.  
* Moderada**:** pérdida parcial de materia orgánica y modificaciones intermedias en la vegetación y el suelo.  
* Alta**:** pérdida importante de materia orgánica y alteraciones significativas del suelo y la vegetación.

La severidad fue determinada mediante evaluación visual en campo, como parte de un muestreo estratificado según la severidad del incendio y la posición microtopográfica. Por lo tanto, no se calculó a partir de las variables fisicoquímicas utilizadas como predictoras. Esta independencia evita la circularidad y reduce el riesgo de fuga de información (*data leakage*) durante el entrenamiento de los modelos.

#### **Distribución de muestras por turbera y severidad**

| Turbera | Low | Moderate | High | Total |
| :---- | :---- | :---- | :---- | :---- |
| B1 | 15 | 0 | 40 | 55 |
| B11 | 10 | 10 | 10 | 30 |
| B21 | 10 | 10 | 10 | 30 |
| B3 | 6 | 10 | 10 | 26 |
| B8 | 10 | 10 | 10 | 30 |
| B9 | 10 | 10 | 10 | 30 |
| I2 | 10 | 10 | 10 | 30 |
| **Total** | **71** | **60** | **100** | **231** |

#### **Distribución general de las clases**

High       100 muestras · 43.29%

Low          71 muestras · 30.74%

Moderate  60 muestras · 25.97%

La distribución de las categorías de severidad del incendio presenta diferencias entre las unidades de muestreo. En particular, la turbera B1 no registra observaciones de severidad moderada, mientras que B3 presenta una menor cantidad de muestras de severidad baja. Las restantes cinco turberas presentan una distribución equilibrada de las tres categorías (Low, Moderate y High).

En cuanto a la identificación de posibles mediciones repetidas, se verificó que los 231 registros poseen identificadores únicos al considerar la combinación de turbera e ID\_SOBRE. Tampoco se encontraron filas completamente duplicadas. Sin embargo, debido a que no se dispone de coordenadas ni de un identificador específico del punto físico de muestreo, no es posible descartar completamente la existencia de réplicas espaciales.

Considerando la estructura jerárquica del muestreo, donde múltiples observaciones pertenecen a una misma turbera, se propone utilizar validación cruzada agrupada por turbera  y estratificada mediante *StratifiedGroupKFold*, utilizando la turbera como variable de agrupamiento.

Este procedimiento permite mantener todas las muestras de una misma turbera dentro de una única partición, evitando que observaciones pertenecientes a una misma unidad de muestreo se distribuyan simultáneamente entre los conjuntos de entrenamiento y prueba.

### **4\. Modelos propuestos**

* Regresión Logística Multinomial: modelo de referencia (*baseline*), interpretable.  
* K-Nearest Neighbors (KNN): se requiere estandarizar las variables.  
* Validación: validación cruzada agrupada por turbera, con métricas F1 macro, precisión y recall por clase.

### **5\. Impacto económico u operativo**

Este proyecto propone una solución tecnológica de apoyo al diagnóstico ambiental frente al impacto de los incendios forestales en las turberas de Sphagnum de Tierra del Fuego, mediante la clasificación de la severidad del fuego en tres categorías: baja (Low), moderada (Moderate) y alta (High).

Desde el punto de vista económico y operativo, se busca evaluar si los modelos de aprendizaje automático permiten identificar las variables fisicoquímicas que aportan mayor información para predecir la severidad del fuego y, eventualmente, reducir la cantidad de análisis de laboratorio necesarios.

Si los resultados lo permiten, esto podría representar un ahorro potencial en costos y tiempos de procesamiento, al evitar algunas determinaciones de laboratorio sin afectar significativamente la calidad de las predicciones. Las mediciones que podrían omitirse se definirán a partir de la importancia de las variables y del desempeño de los modelos; por el momento, no se dispone de información suficiente para cuantificar ese ahorro.

Además, la herramienta contibuye a:

* Optimizar los recursos destinados a los análisis de laboratorio.  
* Agilizar la identificación de áreas con distintos niveles de afectación.  
* Mejorar la planificación de tareas de monitoreo y restauración.  
* Facilitar la toma de decisiones informadas por parte de organismos públicos y actores privados.  
* Aportar información para el desarrollo de políticas de gestión ambiental sostenible.

### **6\. Fuente de datos**

Variables fisico-quimicas de turberas después de un incendio forestal. 

Aravena Acuña, M. C. A. (2026). CONICET Digital, CADIC. 

Licencia Creative Commons Atribución–NoComercial–CompartirIgual 2.5 Argentina (CC BY-NC-SA 2.5). 

Muestreo en turberas dominadas por Sphagnum cerca de Tolhuin, Tierra del Fuego.

Link: [https\://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253)

### **7\. Referencia bibliográfica**

\[1\] \- Keeley, J. E. (2009). Fire intensity, fire severity and burn severity: A brief review and suggested usage. International Journal of Wildland Fire, 18(1), 116–126. [https\://doi.org/10.1071/WF07049](https://doi.org/10.1071/WF07049?utm_source=chatgpt.com)

\[2\] \- Fulazzaky, M. A., Ismail, I., Harlen, H., Sukendi, Roestamy, M., & Siregar, Y. I. (2022). Evaluation of change in the peat soil properties affected by different fire severities. Environmental Monitoring and Assessment, 194, 783\. [https\://doi.org/10.1007/s10661-022-10430-z](https://doi.org/10.1007/s10661-022-10430-z)

**___________________________________________________________________________________________________________________________________________**

**Materia:** Aprendizaje Automático (2C 2026\) \| **Institución:** Centro Politécnico Superior Malvinas Argentinas       
**Docente:** Lic. Nicolas Caballero \| **Alumna:** Sandra Vargas 
