# Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo

---

# Tabla de contenidos

[Descripción del proyecto](#descripción-del-proyecto)

[Objetivos](#objetivos)

[Objetivo general](#objetivo-general)

[Objetivos específicos](#objetivos-específicos)

[Contexto del problema y relevancia](#contexto-del-problema-y-relevancia)

[Tipo de problema y variable objetivo](#tipo-de-problema-y-variable-objetivo)

[Distribución de muestras por turbera y severidad](#distribución-de-muestras-por-turbera-y-severidad)

[Distribución general de las clases](#distribución-general-de-las-clases)

[Modelos de Aprendizaje Automático y Validación](#modelos-de-aprendizaje-automático-y-validación)

[Impacto económico u operativo](#impacto-económico-u-operativo)

[Fuente de datos](#fuente-de-datos)

[Estructura del repositorio y Link](#estructura-del-repositorio)

[Notebooks del proyecto](#notebooks-del-proyecto)

[Preprocesamiento aplicado](#preprocesamiento-aplicado)

[Decisiones de limpieza y transformación](#decisiones-de-limpieza-y-transformación)

[Limpieza de columnas y nombres](#limpieza-de-columnas-y-nombres)

[Tratamiento de nulos y faltantes](#tratamiento-de-nulos-y-faltantes)

[Tratamiento de valores atípicos (Outliers)](#tratamiento-de-valores-atípicos-\(outliers\))

[Transformación de la variable objetivo](#transformación-de-la-variable-objetivo)

[Variable objetivo y features seleccionadas](#variable-objetivo-y-features-seleccionadas)

[Variable objetivo (Target)](#variable-objetivo-\(target\))

[Features (variables predictoras) utilizadas](#features-\(variables-predictoras\)-utilizadas)

[Modelos probados y evaluación](#modelos-probados-y-evaluación)

[Modelos y evaluación](#modelos-y-evaluación)

[Comparación y métricas](#comparación-y-métricas)

[Resultados y salidas generadas](#resultados-y-salidas-generadas)

[Video de presentación](#video-de-presentación)

[Recomendación](#recomendación)

[Limitaciones](#limitaciones)

[Referencia bibliográfica](#referencia-bibliográfica)

[Autoría](#autoría)

[Formato Original de Organización de Proyecto \- Cookiecutter](#formato-original-de-organización-de-proyecto---cookiecutter)

---

## Descripción del proyecto

Este proyecto, desarrollado en el marco de la materia Aprendizaje Automático del Politécnico Superior Malvinas Argentinas, busca clasificar la severidad de los incendios en turberas de Sphagnum de Tierra del Fuego (baja, moderada o alta) a partir de sus propiedades fisicoquímicas.

Mediante modelos de aprendizaje automático, se evaluará qué variables aportan mayor información y si es posible reducir la cantidad de análisis de laboratorio sin afectar significativamente la precisión de las predicciones. El objetivo es generar una herramienta experimental de apoyo al diagnóstico ambiental y a la planificación de acciones de restauración, sin reemplazar las evaluaciones de campo.

---

## Objetivos 

### **Objetivo general** 

Desarrollar y evaluar modelos de Aprendizaje Automático que clasifiquen la severidad del fuego (Low / Moderate / High) en turberas de Sphagnum de Tierra del Fuego a partir de propiedades fisicoquímicas del suelo, comparando Regresión Logística Multinomial y KNN bajo validación agrupada por turbera, e identificar un subconjunto reducido de variables predictoras que preserve la capacidad de discriminación.

### **Objetivos específicos** 

* Explorar el dataset, analizando las distribuciones de las variables, los valores atípicos y los valores faltantes.  
* Preprocesar los datos mediante la identificación y tratamiento de valores faltantes, la codificación de variables categóricas y la estandarización de las variables numéricas, según los requerimientos de cada modelo.  
* Entrenar y comparar modelos de clasificación supervisada multiclase (Regresión Logística Multinomial y K-NN), utilizando validación cruzada estratificada y agrupada por turbera, para evaluar su capacidad de generalización a nuevas unidades de muestreo.  
* Evaluar el rendimiento de los modelos mediante F1-score macro, matrices de confusión, precisión y recall por clase, analizando las confusiones entre las categorías de severidad.  
* Identificar y analizar la importancia relativa de las variables fisicoquímicas para la discriminación de las categorías de severidad del daño provocado por el incendio, con el propósito de seleccionar un subconjunto reducido de predictores.  
* Comparar cuantitativamente el desempeño de los modelos de clasificación entrenados con el conjunto completo de variables predictoras y con un subconjunto reducido seleccionado según su importancia, utilizando los mismos criterios de evaluación y validación cruzada agrupada por turbera, para determinar cuánto puede reducirse el número de ensayos analíticos sin comprometer significativamente la capacidad predictiva de los modelos y evaluar la factibilidad técnica y económica de dicha reducción.

---

## Contexto del problema y relevancia 

Las turberas de Sphagnum de Tierra del Fuego acumulan carbono en forma de materia orgánica, y los incendios pueden alterar sus propiedades fisicoquímicas y comprometer esa función. Estimar la severidad del daño suele requerir muestreo de campo y análisis de laboratorio, que son lentos y costosos. Como todos los predictores considerados requieren ensayos de laboratorio, se evaluará la posibilidad de reducir tiempos y costos analíticos al identificar si es factible prescindir de determinaciones específicas de mayor complejidad o costo sin comprometer la precisión de la clasificación de la variable objetivo.

El problema es determinar si la severidad de afectación de incendios forestales de una muestra de turbera (baja, moderada o alta) puede clasificarse a partir de propiedades del suelo medidas en laboratorio (pH, conductividad eléctrica, materia orgánica, densidad aparente, humedad). Si el modelo funciona, permitiría identificar qué indicadores sirven como señal rápida de daño y priorizar análisis y acciones de restauración.

La relevancia del proyecto radica en evaluar qué indicadores aportan mayor información para caracterizar la condición después de producido el incendio y si un conjunto reducido de mediciones permite obtener resultados comparables a los del conjunto completo. Se plantea como una herramienta experimental de apoyo al análisis ambiental, no como un reemplazo de la evaluación de campo.

---

## Tipo de problema y variable objetivo 

Este proyecto se encuadra dentro del aprendizaje automático supervisado. Se trata de un problema de clasificación supervisada multiclase, la variable objetivo se denomina Burn\_Severity y tiene tres categorías a predecir: Low, Moderate y High.

La unidad de análisis está constituida por cada muestra individual de suelo de turbera, representada por una fila del dataset. El conjunto está compuesto por 231 observaciones procedentes de siete turberas.

Cada registro contiene información sobre la procedencia de la muestra, su clasificación microtopográfica, la categoría de severidad del incendio (Burn\_Severity) y un conjunto de mediciones fisicoquímicas del suelo.

La variable objetivo Burn\_Severity clasifica la severidad del fuego en tres categorías: baja (Low), moderada (Moderate) y alta (High), según el grado de alteración de la vegetación y las propiedades del suelo. La clasificación se fundamenta en los criterios de Keeley (2009) [\[1\]](#referencia-bibliográfica) y Fulazzaky et al. (2022) [\[2\]](#referencia-bibliográfica).

* Baja**:** efectos reducidos del fuego y escasa alteración de la materia orgánica.  
* Moderada**:** pérdida parcial de materia orgánica y modificaciones intermedias en la vegetación y el suelo.  
* Alta**:** pérdida importante de materia orgánica y alteraciones significativas del suelo y la vegetación.

La severidad fue determinada mediante evaluación visual en campo, como parte de un muestreo estratificado según la severidad del incendio y la posición microtopográfica. Por lo tanto, no se calculó a partir de las variables fisicoquímicas utilizadas como predictoras. Esta independencia evita la circularidad y reduce el riesgo de fuga de información (*data leakage*) durante el entrenamiento de los modelos.

### **Distribución de muestras por turbera y severidad** 

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

### **Distribución general de las clases** 

High       100 muestras · 43.29%

Low          71 muestras · 30.74%

Moderate  60 muestras · 25.97%

La distribución de las categorías de severidad del incendio presenta diferencias entre las unidades de muestreo. En particular, la turbera B1 no registra observaciones de severidad moderada, mientras que B3 presenta una menor cantidad de muestras de severidad baja. Las restantes cinco turberas presentan una distribución equilibrada de las tres categorías (Low, Moderate y High).

En cuanto a la identificación de posibles mediciones repetidas, se verificó que los 231 registros poseen identificadores únicos al considerar la combinación de turbera e ID\_SOBRE. Tampoco se encontraron filas completamente duplicadas. Sin embargo, debido a que no se dispone de coordenadas ni de un identificador específico del punto físico de muestreo, no es posible descartar completamente la existencia de réplicas espaciales.

Considerando la estructura jerárquica del muestreo, donde múltiples observaciones pertenecen a una misma turbera, se propone utilizar validación cruzada agrupada por turbera  y estratificada mediante *StratifiedGroupKFold*, utilizando la turbera como variable de agrupamiento.

Este procedimiento permite mantener todas las muestras de una misma turbera dentro de una única partición, evitando que observaciones pertenecientes a una misma unidad de muestreo se distribuyan simultáneamente entre los conjuntos de entrenamiento y prueba.

---

## Modelos de Aprendizaje Automático y Validación 

| Modelo | Descripción |
| :---- | :---- |
| Regresión Logística Multinomial | Modelo de referencia (baseline), de fácil interpretación. |
| K-Nearest Neighbors (KNN) | Modelo basado en la proximidad entre muestras. Requiere estandarizar las variables. |
| Validación | Validación cruzada agrupada por turbera, utilizando F1 macro, precisión y recall por clase como métricas de evaluación. |

---

## Impacto económico u operativo {#impacto-económico-u-operativo}

Este proyecto propone una solución tecnológica de apoyo al diagnóstico ambiental frente al impacto de los incendios forestales en las turberas de Sphagnum de Tierra del Fuego, mediante la clasificación de la severidad del fuego en tres categorías: baja (Low), moderada (Moderate) y alta (High).

Desde el punto de vista económico y operativo, se busca evaluar si los modelos de aprendizaje automático permiten identificar las variables fisicoquímicas que aportan mayor información para predecir la severidad del fuego y, eventualmente, reducir la cantidad de análisis de laboratorio necesarios.

Si los resultados lo permiten, esto podría representar un ahorro potencial en costos y tiempos de procesamiento, al evitar algunas determinaciones de laboratorio sin afectar significativamente la calidad de las predicciones. Las mediciones que podrían omitirse se definirán a partir de la importancia de las variables y del desempeño de los modelos; por el momento, no se dispone de información suficiente para cuantificar ese ahorro.

Además, la herramienta contibuye  a:

* Optimizar los recursos destinados a los análisis de laboratorio.  
* Agilizar la identificación de áreas con distintos niveles de afectación.  
* Mejorar la planificación de tareas de monitoreo y restauración.  
* Facilitar la toma de decisiones informadas por parte de organismos públicos y actores privados.  
* Aportar información para el desarrollo de políticas de gestión ambiental sostenible.

---

## Fuente de datos 

**Nombre del dataset**: Variables fisico-químicas de turberas después de un incendio forestal.

**Autora**: Aravena Acuña, Marie Claire Alejandra (2026).

**Institución**: Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET), CADIC.

**Ubicación del estudio**: Turberas dominadas por Sphagnum, cercanas a Tolhuin, Tierra del Fuego, Argentina.

**Repositorio institucional**: [CONICET Digital – Dataset original](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253?utm_source=chatgpt.com).

**Licencia**: Creative Commons Atribución–NoComercial–CompartirIgual 2.5 Argentina (CC BY-NC-SA 2.5).

**Citación bibliográfica**:

Aravena Acuña, Marie Claire Alejandra (2026). Variables fisico-químicas de turberas después de un incendio forestal. Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET). Dataset. [http\://hdl.handle.net/11336/284253](http://hdl.handle.net/11336/284253?utm_source=chatgpt.com)

Link: [https\://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253)

---

## Estructura del repositorio 

Organización de carpetas y archivos. Basado en el esquema Cookiecutter Data Science:

```
proyecto/
├── data/
│   ├── raw/          # Datos originales sin modificar
│   ├── interim/      # Datos intermedios
│   └── processed/    # Datos limpios listos para modelar
├── docs/             # Documentación, descripción del dataset
├── models/           # Modelos entrenados (.pkl)
├── notebooks/        # Notebooks numerados por etapa
├── reports/
│   ├── figures/      # Gráficos y visualizaciones
│   └── video/        # Video de presentación
├── src/              # Código reutilizable (opcional)
├── requirements.txt
└── README.md
```

### **Link al repositorio del proyecto:** [AA\_Afectacion\_incendio\_en\_turbera – GitHub](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera?utm_source=chatgpt.com). 

---

## Notebooks del proyecto 

**Notebook Preprocesamiento**  
El notebook desarrolla la etapa de Análisis Exploratorio de Datos (EDA) y Preprocesamiento para clasificar la severidad de incendios en turberas de Tierra del Fuego (Low, Moderate, High) a partir de variables fisicoquímicas del suelo. El flujo abarca la carga del dataset, la exploración de su estructura y estadísticas descriptivas, el diagnóstico y tratamiento de datos faltantes, la detección de valores atípicos (outliers), el análisis visual de distribuciones y correlaciones (pairplot, heatmap), y finalmente la codificación ordinal de la variable objetivo, dejando los datos listos para el modelado. 

**Notebook Modelado**  
Inicia con la preparación, limpiando y transformando los datos para dividirlos en entrenamiento y prueba. Luego, avanzamos al entrenamiento y evaluación, donde ajustamos el algoritmo y validamos su rendimiento mediante métricas clave. A continuación, analizamos la importancia de las variables para identificar qué factores tienen mayor impacto en las predicciones. Finalmente, realizamos una comparación de escenarios para contrastar distintas configuraciones o supuestos, permitiéndonos seleccionar la solución más robusta para tomar decisiones.   
---

## Preprocesamiento aplicado

## **Decisiones de limpieza y transformación** 

### **Limpieza de columnas y nombres** 

* Eliminación de columna: Se elimina la columna `Ntotal` del dataset.  
* Nombres: Las columnas restantes conservan sus nombres originales (pH, CE, DOB, MO, HD, BD, VWC, GWC, etc.). Se genera una nueva columna llamada `target` para almacenar la variable objetivo codificada.

### **Tratamiento de nulos y faltantes** 

* Nulos en `Ntotal`: Se detectó que esta variable presentaba 169 valores nulos (73,16%). Se decidió no imputarlos para evitar sesgos y eliminar la columna completa, reduciendo el dataset de 15 a 14 columnas.  
* Duplicados: Se verificó que no existen registros duplicados en el DataFrame.  
* Nulos en el resto de variables: Tras eliminar `Ntotal`, el dataset queda 100% completo (231 filas x 14 columnas) sin valores faltantes en las variables predictoras ni en la variable objetivo.

### **Tratamiento de valores atípicos (Outliers)** 

* Diagnóstico: Se utilizó el método del rango intercuartílico (IQR) con límites de Tukey. Se identificó que CE (Conductividad Eléctrica) tiene un 22,08% de outliers, seguida por BD (9,09%) y VWC (5,63%).  
* Decisión: No se eliminaron los valores atípicos. Se argumentó que representan variaciones reales de las turberas y efectos genuinos del fuego. En su lugar, se decidió tratarlos durante el escalado aplicando `RobustScaler` (que usa la mediana y el IQR) y una transformación logarítmica (`np.log1p`) para reducir la asimetría, especialmente en CE.

### **Transformación de la variable objetivo** 

* Codificación: La variable categórica ordinal `Burn_Severity` (Low, Moderate, High) se transformó a valores numéricos respetando su jerarquía: Low \= 0, Moderate \= 1, High \= 2, almacenados en la columna `target`. Se verificó que no quedaran valores nulos tras el mapeo.

---

## Variable objetivo y features seleccionadas  

### **Variable objetivo (Target)** 

\`Burn\_Severity\` (codificada como \*\*\`target\`\*\* para el modelo): Categoría ordinal de la severidad del incendio, con tres niveles: Low (0), Moderate (1) y High (2).

### **Features (variables predictoras) utilizadas** 

Se seleccionaron 7 variables fisicoquímicas continuas del suelo, excluyendo identificadores, la microtopografía y la variable con datos faltantes \`Ntotal\`:

* pH (nivel de alcalinidad)  
* CE (conductividad eléctrica)  
* DOB (profundidad de quema)  
* MO (materia orgánica, %)  
* HD (grado de humificación)  
* BD (densidad aparente)  
* VWC (contenido volumétrico de agua)

Tras el análisis de importancia, se identificó un subconjunto reducido óptimo de 4 variables: \`BD\`, \`DOB\`, \`MO\` y \`VWC\`, que mantuvo el rendimiento predictivo sin necesidad de medir \`pH\`, \`CE\` ni \`HD\`.  
---

## Modelos probados y evaluación 

### **Modelos y evaluación** 

Se compararon dos clasificadores multiclase para predecir la severidad del fuego (Low, Moderate, High): Regresión Logística Multinomial (modelo base, interpretable) y K-Nearest Neighbors (KNN) (basado en proximidad, con ajuste de hiperparámetros). Ambos utilizaron \`RobustScaler\` y transformación \`log1p\` en la variable CE dentro de un \`Pipeline\`, evaluados bajo \*\*validación cruzada agrupada por turbera\*\* (\`StratifiedGroupKFold\` con 4 particiones, usando \`GroupKFold\` como respaldo) para medir la capacidad de generalización a sitios no vistos.

### **Comparación y métricas** 

El rendimiento se midió principalmente con F1-score macro (por el desbalance moderado de clases), complementado con accuracy, precisión y recall por clase, y matrices de confusión acumuladas. Los resultados muestran un rendimiento prácticamente indistinguible entre ambos modelos (F1 macro ≈ 0,73 ± 0,11–0,14); la Regresión Logística obtuvo una leve ventaja en la estrategia principal (+0,006), mientras que KNN fue más sensible al cambiar la estrategia de partición. Las confusiones se concentran entre clases contiguas (Moderate–High), indicando que ambos modelos capturan el gradiente ordinal pero tienen dificultad con la categoría intermedia.

---

## Resultados y salidas generadas 

El flujo de modelado confirmó la viabilidad predictiva mediante validación agrupada. Los modelos lograron una exactitud (accuracy) general del 73-75% bajo la estrategia principal (\`StratifiedGroupKFold\`), destacando la Regresión Logística (LogReg) con un F1-macro de 0.737 ± 0.136, ligeramente superior a KNN (0.731 ± 0.106). Al analizar el rendimiento por clase, el modelo demostró una gran capacidad para identificar las severidades extremas: la clase Low obtuvo un excelente F1-score (0.89) y la clase High un rendimiento sólido (0.65 a 0.73), mientras que la clase Moderate representó el mayor desafío (F1 ≈ 0.61), actuando como una categoría de transición difusa.

Las matrices de confusión revelaron que los errores se concentran en las categorías adyacentes (confundir Moderate con High y viceversa), sin que se observen confusiones graves entre los extremos (Low vs. High). Finalmente, el análisis de importancia de variables mostró un consenso contundente: la Profundidad de quema (DOB) y la Materia Orgánica (MO) son los predictores más fuertes para ambos modelos. Al probar un escenario reducido con solo 4 variables (DOB, MO, BD y VWC), el rendimiento se mantuvo prácticamente idéntico al modelo completo (F1-macro de 0.729 para LogReg), demostrando que es posible reducir los análisis de laboratorio en futuros muestreos sin perder capacidad predictiva.

---

## Video de presentación 

Link al Video

https://drive.google.com/file/d/1tLYFyQ6wbdhIYS0Mv7ZgmnLRGeCkbwu5/view?usp=sharing


---

## Recomendación 

1. Para el modelado. Se recomienda adoptar LogReg como modelo principal, no por una ventaja significativa de F1 macro (que no la hay: \+0.006, dentro de la sd), sino por su interpretabilidad mediante coeficientes y su menor costo computacional (no requiere GridSearchCV por fold). KNN ofrece un rendimiento equivalente pero añade complejidad de ajuste sin ventaja clara de generalización en este dataset. Si se opta por KNN, se recomienda reportar los hiperparámetros seleccionados por fold (n\_neighbors, metric, weights) para documentar la estabilidad de la elección.  
2. Para futuros muestreos. Se recomienda priorizar la medición de BD, DOB, MO y VWC y reducir la determinación de pH, CE y HD en muestreos exploratorios o de bajo presupuesto. No obstante, se sugiere mantener las variables excluidas en una submuestra de control para verificar en nuevos sitios que su exclusión no degrada el rendimiento. Esta estrategia permite reducir determinaciones de laboratorio sin cerrar la puerta a recuperar información si aparece una turbera con condiciones atípicas.  
3. Para fortalecer la validación. La principal limitación es el número reducido de turberas (7), que restringe la cantidad de folds independientes y aumenta la varianza de las estimaciones (sd ≈ 0.10–0.14 en F1 macro). Se recomienda incorporar nuevas turberas en futuras campañas para: (i) aumentar el número de grupos en la validación agrupada; (ii) permitir una estimación más estable de la generalización; (iii) evaluar si el subconjunto reducido se sostiene en sitios con condiciones edáficas distintas. Mientras no se disponga de más turberas, los resultados deben presentarse como exploratorios.  
4. Para el análisis de importancia. Se recomienda replicar la importancia por permutación de KNN sobre predicciones out-of-fold (en lugar de sobre el modelo entrenado con todos los datos) en una próxima iteración, para reducir el sesgo optimista de la estimación exploratoria actual. Asimismo, se sugiere reportar la matriz de correlación entre predictores (por ejemplo, Spearman) para contextualizar la posible repartición de importancia entre variables colineales.  
5. Para la interpretación de las etiquetas. Se recomienda documentar explícitamente que Burn\_Severity se determinó por evaluación visual en campo, de forma independiente de las variables fisicoquímicas usadas como predictores, siguiendo los criterios de Keeley (2009) y Fulazzaky et al. (2022). Esto evita la circularidad metodológica y aclara que el modelo aprende a predecir una etiqueta preexistente, no a construirla a partir de los predictores.  
6. Para la comunicación de resultados. Se recomienda presentar siempre la media junto con la desviación estándar entre folds y evitar reportar un único valor de F1 macro como estimación puntual robusta. Dado el reducido número de grupos, un F1 macro de 0.737 ± 0.136 es más informativo —y más honesto— que un 0.737 sin contexto. Las matrices de confusión acumuladas y los gráficos por fold del notebook permiten esta presentación; conviene conservarlos en el informe final.

---

## Limitaciones 

1. Solo 7 turberas. La validación agrupada con 4 folds deja 1–2 turberas por test; la estimación de generalización tiene alta varianza (sd ≈ 0.10–0.14 en F1 macro). No se puede afirmar robustez estadística; los resultados son indicativos y deben replicarse con más turberas.  
2. Estratificación limitada. Con 7 grupos y 3 clases, StratifiedGroupKFold no siempre puede garantizar equilibrio perfecto de clases entre folds. La comparación con GroupKFold mostró que el orden de los modelos puede invertirse según la partición, lo que evidencia esta limitación.  
3. Importancia por permutación exploratoria. La permutación de KNN se calculó sobre el modelo full evaluado sobre los mismos datos, por lo que puede sobreestimar la importancia. Se usó solo para consensuar con los coeficientes, no para validar el subconjunto. La validación real es la comparación A vs B.  
4. Variables correlacionadas. La importancia observada puede repartirse entre variables colineales, alterando la selección. El consenso entre métodos mitiga pero no elimina este riesgo.  
5. Ausencia de costos de laboratorio. No se dispone de costos ni tiempos de cada análisis, por lo que la reducción de variables se plantea como posibilidad técnica y operativa, no como ahorro económico real.

---

## Referencia bibliográfica 

\[1\] \- Keeley, J. E. (2009). Fire intensity, fire severity and burn severity: A brief review and suggested usage. International Journal of Wildland Fire, 18(1), 116–126. [https\://doi.org/10.1071/WF07049](https://doi.org/10.1071/WF07049?utm_source=chatgpt.com)

\[2\] \- Fulazzaky, M. A., Ismail, I., Harlen, H., Sukendi, Roestamy, M., & Siregar, Y. I. (2022). Evaluation of change in the peat soil properties affected by different fire severities. Environmental Monitoring and Assessment, 194, 783. [https\://doi.org/10.1007/s10661-022-10430-z](https://doi.org/10.1007/s10661-022-10430-z)

---

## Autoría 

Espacio académico correspondiente a la alumna Sandra Vargas en la asignatura Aprendizaje Automático (dictada por el Lic. Nicolás Caballero), perteneciente al 2do cuatrimestre 2026 de la Tecnicatura Superior en Ciencia de Datos e Inteligencia Artificial en la institución Centro Politécnico Superior Malvinas Argentinas.

---

## Formato Original de Organización de Proyecto \- Cookiecutter 
```
├── LICENSE            <- Open-source license if one is chosen
├── Makefile           <- Makefile with convenience commands like `make data` or `make train`
├── README.md          <- The top-level README for developers using this project.
├── data
│   ├── external       <- Data from third party sources.
│   ├── interim        <- Intermediate data that has been transformed.
│   ├── processed      <- The final, canonical data sets for modeling.
│   └── raw            <- The original, immutable data dump.
│
├── docs               <- A default mkdocs project; see www.mkdocs.org for details
│
├── models             <- Trained and serialized models, model predictions, or model summaries
│
├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
│                         the creator's initials, and a short `-` delimited description, e.g.
│                         `1.0-jqp-initial-data-exploration`.
│
├── pyproject.toml     <- Project configuration file with package metadata for 
│                         AA_Afectacion_incendio_en_turbera and configuration for tools like black
│
├── references         <- Data dictionaries, manuals, and all other explanatory materials.
│
├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
│   └── figures        <- Generated graphics and figures to be used in reporting
│
├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
│                         generated with `pip freeze > requirements.txt`
│
├── setup.cfg          <- Configuration file for flake8
│
└── AA_Afectacion_incendio_en_turbera   <- Source code for use in this project.
    │
    ├── __init__.py             <- Makes AA_Afectacion_incendio_en_turbera a Python module
    │
    ├── config.py               <- Store useful variables and configuration
    │
    ├── dataset.py              <- Scripts to download or generate data
    │
    ├── features.py             <- Code to create features for modeling
    │
    ├── modeling                
    │   ├── __init__.py 
    │   ├── predict.py          <- Code to run model inference with trained models          
    │   └── train.py            <- Code to train models
    │
    └── plots.py                <- Code to create visualizations
```

