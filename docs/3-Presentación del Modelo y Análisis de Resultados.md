**APRENDIZAJE AUTOMÁTICO — PRESENTACIÓN DEL MODELO Y ANÁLISIS DE RESULTADOS**

**Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo**

---

## **1\. Origen de los datos**

El dataset original, Muestras\_FyQ.xls, proviene del Repositorio Institucional CONICET Digital. Contiene 231 observaciones de muestras de turberas de *Sphagnum* de Tierra del Fuego, distribuidas en 7 turberas (identificadas por la columna Turbera), con 15 variables originales que incluyen:

* Identificadores: N, Turbera, Transecta, ID\_SOBRE  
* Variables categóricas: Microtopography (HUM, HOL), Burn\_Severity (variable objetivo: Low, Moderate, High)  
* Predictores fisicoquímicos: pH (nivel de alcalinidad), CE (conductividad eléctrica), DOB (profundidad de quema), MO (materia orgánica), HD (grado de humificación), BD (densidad aparente), VWC (contenido volumétrico de agua), Ntotal (nitrógeno total), GWC (contenido gravimétrico de agua).

El archivo procesado Muestras\_FyQ\_procesado.csv surge del notebook de preprocesamiento y es el insumo del modelado. La variable Ntotal fue excluida por presentar 73,16 % de valores faltantes (169 de 231 registros), decisión que evita introducir sesgos por imputación masiva.

---

## **2\. Análisis exploratorio de datos (EDA)**

### **2.1 Estadísticas descriptivas relevantes**

A partir del resumen estadístico del dataset original:

| Variable | Media | Desv. est. | Mín. | Mediana | Máx. | Observación |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| pH | 3,98 | 0,16 | 3,31 | 3,98 | 4,47 | Muy estable y ácido (típico de turberas) |
| CE | 229,24 | 251,52 | 64,70 | 160,80 | 2760,0 | Fuerte asimetría (media ≫ mediana) → outliers |
| DOB | 0,82 | 0,76 | 0,00 | 0,80 | 3,80 | Alta dispersión, valores nulos en mínimo |
| MO | 95,22 | 2,75 | 84,94 | 96,18 | 98,48 | Muy alta y concentrada (turba orgánica) |
| HD | 0,13 | 0,055 | 0,003 | 0,13 | 0,35 | Cola hacia valores altos |
| BD | 0,045 | 0,023 | 0,016 | 0,038 | 0,138 | Baja densidad aparente |
| VWC | 0,445 | 0,134 | 0,166 | 0,403 | 0,906 | Variabilidad apreciable |
| Ntotal | 8,71 | 2,06 | 1,97 | 7,50 | 15,72 | 73 % faltantes → excluida |

### **2.2 Diagnóstico de valores faltantes y duplicados**

* Valores faltantes: solo Ntotal presenta ausencias (169/231 \= 73,16 %). El resto de las variables está completo (231 no nulos).  
* Duplicados: 0 registros duplicados.  
* Balance de clases (target): Low ≈ 71, Moderate ≈ 60, High ≈ 100 → desbalance moderado, con High sobrerrepresentada.

### **2.3 Distribución por turbera y severidad**

El análisis de frecuencias cruzadas (pd.crosstab) muestra que las 7 turberas aportan muestras a las tres clases de severidad, pero con proporciones variables. Esta estructura justifica el uso de validación agrupada por turbera: observaciones de una misma turbera comparten condiciones edáficas y no son independientes.

### **2.4 Conclusiones clave del EDA**

1. pH es prácticamente constante (rango 3,31–4,47) → baja capacidad discriminante esperada.  
2. CE presenta asimetría marcada y outliers → requiere transformación (se aplicó log1p).  
3. MO es muy alta y con poca variabilidad relativa, pero su variación absoluta puede ser informativa tras escalado.  
4. Ntotal se descarta por faltantes masivos.  
5. No hay duplicados ni faltantes en predictores retenidos → no se requiere imputación en el conjunto final.  
6. Desbalance moderado → se justifica F1 macro como métrica principal (penaliza errores en clases minoritarias como Moderate).

Figuras sugeridas para la presentación: boxplots por clase de severidad para BD, MO, DOB y VWC, y gráfico de barras de distribución de clases por turbera.

---

## **3\. Preprocesamiento**

| Etapa | Tratamiento | Justificación |
| :---- | :---- | :---- |
| Variable Ntotal | Eliminada | 73,16 % faltantes |
| CE | Transformación log1p | Reducir asimetría y efecto de outliers |
| Todas las numéricas | RobustScaler | Robusto a outliers (usa mediana e IQR); no se eliminaron atípicos porque representan variación real |
| Pipeline | Pipeline / ColumnTransformer ajustado solo con train en cada fold | Evitar fuga de información |
| Categóricas | Codificación (Ordinal/OneHot según modelo) | Requerimiento de cada algoritmo |

Decisión clave: no se eliminaron valores atípicos, ya que pueden representar variaciones reales de las turberas (p. ej., muestras muy quemadas con CE extrema).

---

## **4\. Modelo de Aprendizaje Automático desarrollado**

### **4.1 Arquitectura general**

Se implementaron dos modelos supervisados multiclase dentro de un pipeline reproducible:

* Regresión Logística Multinomial (baseline)  
  * multi\_class='multinomial', solver lbfgs (o saga), regularización L2.  
  * Hiperparámetro ajustado: C (inverso de regularización) mediante GridSearchCV dentro de cada fold de entrenamiento.  
  * Coeficientes estandarizados interpretables por clase.

* K-Nearest Neighbors (KNN)  
  * Hiperparámetros ajustados por fold: n\_neighbors, metric (euclidiana/manhattan), weights (uniform/distance).  
  * Búsqueda con GridSearchCV sobre el train de cada fold.

### **4.2 Estrategia de validación**

| Estrategia | Característica | Rol |
| :---- | :---- | :---- |
| StratifiedGroupKFold (4 folds, grupo \= Turbera) | Agrupa por turbera y procura equilibrio de clases | Validación principal |
| GroupKFold (4 folds, grupo \= Turbera) | Agrupa por turbera, sin control de clases | Respaldo / sensibilidad |

Se verificó que ninguna turbera aparece simultáneamente en train y test. Con solo 7 turberas, la estratificación es limitada en ambos esquemas; la comparación entre ambos permite evaluar si las conclusiones dependen de la partición.

### **4.3 Ajuste de hiperparámetros**

* LogReg: GridSearchCV por fold sobre C (y eventualmente penalty).  
* KNN: GridSearchCV por fold sobre n\_neighbors, metric, weights.  
* El ajuste se realizó exclusivamente con datos de entrenamiento de cada fold, evitando fuga.

---

## **5\. Métricas de evaluación**

### **5.1 Métricas promedio entre folds**

| Modelo | Validación | Accuracy (media ± sd) | F1 macro (media ± sd) |
| :---- | :---- | :---- | :---- |
| LogReg | SGKF | 0,749 ± 0,128 | 0,737 ± 0,136 |
| KNN | SGKF | 0,735 ± 0,104 | 0,731 ± 0,106 |
| LogReg | GKF | 0,648 ± 0,049 | 0,618 ± 0,091 |
| KNN | GKF | 0,676 ± 0,079 | 0,646 ± 0,129 |

Interpretación:

* Bajo la estrategia principal (SGKF), LogReg obtiene el mayor F1 macro (0,737) frente a KNN (0,731). La diferencia es de apenas \+0,006, muy inferior a la desviación estándar entre folds (≈ 0,13 y ≈ 0,11). Ambos modelos son estadísticamente indistinguibles.  
* Bajo GKF, el orden se invierte (KNN 0,646 \> LogReg 0,618), pero la diferencia (+0,028) también queda dentro de la variabilidad entre folds (sd ≈ 0,09–0,13). La conclusión sobre qué modelo generaliza mejor no es robusta y depende de la partición.  
* La accuracy es consistentemente ≥ F1 macro, reflejando el desbalance moderado: F1 macro penaliza más los errores en la clase minoritaria (Moderate).

### **5.2 Matrices de confusión acumuladas**

| Aspecto | Hallazgo |
| :---- | :---- |
| Low y High | Clases muy bien definidas: \>60 aciertos en diagonal |
| Moderate | Clase más difícil : LogReg la confunde con High (21 casos); KNN la retiene mejor (16 casos hacia High) |
| Confusión ordinal | Errores se concentran en casilleros contiguos (Low–Moderate, Moderate–High); no hay confusión grave Low↔High |
| LogReg vs KNN en High | LogReg detecta más High reales (66) que KNN (59); KNN subestima severidad alta |

### **5.3 Precisión y recall por clase (media entre folds)**

* Low: precisión y recall altos en todos los modelos (\>0,75).  
* Moderate: recall más bajo (≈0,62 en LogReg-SGKF; ≈0,71 en KNN-SGKF), consistente con su carácter intermedio.  
* High: precision ≈0,73–0,75, recall ≈0,62–0,73 según modelo.

### **5.4 Importancia de variables**

Consenso entre métodos:

* Seleccionadas (4): BD, DOB, MO, VWC  
* Excluidas (3): pH, CE, HD

BD y MO aparecen consistentemente en ambos métodos (coeficientes LogReg y permutación KNN) y son las que mayor información aportan, en línea con su papel en la alteración de propiedades físicas del suelo tras el fuego. DOB y VWC completan el subconjunto por su aporte en al menos uno de los criterios.

### **5.5 Comparación de escenarios (conjunto completo vs reducido)**

| Escenario | n\_vars | Variables | F1\_LR (media ± sd) | F1\_KNN (media ± sd) |
| :---- | :---- | :---- | :---- | :---- |
| A completo | 7 | pH, CE, DOB, MO, HD, BD, VWC | 0,737 ± 0,136 | 0,731 ± 0,106 |
| B reducido | 4 | BD, DOB, MO, VWC | 0,729 ± 0,116 | 0,701 ± 0,096 |
| Diferencia (A–B) | –3 vars | — | \+0,008 | \+0,030 |

Ambas diferencias son inferiores a la sd entre folds (≈0,12–0,14 para LogReg; ≈0,10 para KNN). 

El subconjunto reducido preserva la capacidad de discriminación dentro de la variabilidad esperada.

---

## **6\. Interpretación de resultados y conclusiones finales**

### **6.1 Respuesta al objetivo general**

Desarrollar y evaluar modelos de AA que clasifiquen la severidad del fuego en turberas de Sphagnum de Tierra del Fuego a partir de propiedades fisicoquímicas, comparando Regresión Logística Multinomial y KNN bajo validación agrupada por turbera, e identificar un subconjunto reducido de variables predictoras que preserve la capacidad de discriminación.

Se cumplió: se desarrollaron y compararon ambos modelos bajo SGKF/GKF, se evaluaron con F1 macro, matrices de confusión y precisión/recall por clase, y se identificó un subconjunto reducido {BD, DOB, MO, VWC} que preserva el desempeño dentro de la variabilidad entre folds.

### **6.2 Respuesta a los objetivos específicos**

### **6.3 Conclusiones finales**

| Objetivo específico | Resultado |
| :---- | :---- |
| 1\. Explorar el dataset | 231 obs, 7 turberas, 7 predictores; Ntotal excluida (73 % faltantes); CE asimétrica; sin duplicados; desbalance moderado |
| 2\. Preprocesar | log1p sobre CE \+ RobustScaler en todas; pipeline ajustado solo con train; sin eliminación de outliers |
| 3\. Entrenar y comparar | LogReg (baseline) y KNN con GridSearchCV por fold; validación SGKF principal y GKF respaldo; sin fuga de información |
| 4\. Evaluar rendimiento | F1 macro 0,737 (LogReg) vs 0,731 (KNN) en SGKF; confusiones ordinales; Moderate es la clase más difícil |
| 5\. Importancia de variables | Consenso LogReg (coeficientes) \+ KNN (permutación) → {BD, DOB, MO, VWC}; BD y MO más consistentes |
| 6\. Comparar escenarios | A–B: \+0,008 (LogReg), \+0,030 (KNN), ambos \< sd entre folds → reducción técnicamente factible |

1. Generalización de los modelos. Bajo SGKF, LogReg obtuvo el mayor F1 macro (0,737 ± 0,136) frente a KNN (0,731 ± 0,106). La diferencia (+0,006) es muy inferior a la variabilidad entre folds → desempeño estadísticamente indistinguible. La comparación con GKF invierte el orden (KNN 0,646 \> LogReg 0,618), también dentro de la dispersión → la ventaja de un modelo sobre otro no es robusta y depende de la partición. Con solo 7 turberas y 1–2 por fold de test, las estimaciones son indicativas, no garantía de robustez estadística.  
2. Patrón de confusiones. Los errores se concentran en pares contiguos (Low–Moderate y Moderate–High), lo que indica que los modelos capturan el gradiente ordinal pero tienen dificultad con la categoría intermedia. Moderate presenta el recall más bajo, consistente con su carácter de nivel intermedio entre dos extremos fisicoquímicamente más diferenciados.  
3. Variables que sostienen la señal. El consenso retuvo BD, DOB, MO y VWC. BD y MO son las más consistentes y aportan mayor información para separar severidades. pH, CE y HD no mostraron aporte consistente bajo ambos criterios.  
4. Reducción de variables. La comparación A vs B mostró diferencias de F1 macro de \+0,008 (LogReg) y \+0,030 (KNN), ambas inferiores a la sd entre folds. El subconjunto reducido preserva un desempeño predictivo comparable, lo que sugiere que es posible prescindir de pH, CE y HD en muestreos futuros. Esta conclusión se plantea como posibilidad técnica y operativa, no como ahorro económico real (no se dispone de costos ni tiempos de laboratorio).  
5. Recomendación de modelo. Se recomienda adoptar LogReg como modelo principal, no por una ventaja significativa de F1 macro (que no la hay), sino por su interpretabilidad mediante coeficientes y su menor costo computacional (no requiere GridSearchCV por fold). KNN ofrece rendimiento equivalente pero añade complejidad de ajuste sin ventaja clara de generalización.

---

## **7\. Limitaciones metodológicas**

* Solo 7 turberas: la validación agrupada con 4 folds deja 1–2 turberas por test; alta varianza (sd ≈ 0,10–0,14 en F1 macro). Resultados indicativos, no robustez estadística.  
* Estratificación limitada: con 7 grupos y 3 clases, SGKF no siempre garantiza equilibrio perfecto; la inversión de orden entre SGKF y GKF lo evidencia.  
* Importancia por permutación exploratoria: calculada sobre el modelo full evaluado en los mismos datos → puede sobreestimar. Se usó solo para consensuar, no para validar. La validación real es A vs B.  
* Variables correlacionadas: la importancia puede repartirse entre colineales; el consenso mitiga pero no elimina el riesgo.  
* Ausencia de costos de laboratorio: la reducción se plantea como posibilidad técnica, no como ahorro económico real.

---

## **8\. Recomendaciones**

1. Para el modelado: adoptar LogReg como modelo principal por interpretabilidad y menor costo; si se usa KNN, reportar hiperparámetros por fold.  
2. Para futuros muestreos: priorizar BD, DOB, MO y VWC; reducir pH, CE y HD en muestreos exploratorios, manteniendo una submuestra de control.  
3. Para fortalecer la validación: incorporar nuevas turberas para aumentar grupos en la validación agrupada y estabilizar las estimaciones.  
4. Para el análisis de importancia: replicar la permutación de KNN sobre predicciones out-of-fold y reportar matriz de correlación (Spearman) entre predictores.  
5. Para la interpretación de etiquetas: documentar que Burn\_Severity se determinó por evaluación visual en campo (Keeley 2009; Fulazzaky et al. 2022), evitando circularidad metodológica.  
6. Para la comunicación: presentar siempre media ± sd entre folds; evitar un único valor de F1 macro como estimación puntual robusta.

---

## **9\. Entregables y enlaces del repositorio GIT**

| Entregable | Descripción | Enlace (plantilla) |
| :---- | :---- | :---- |
| **Dataset original** | Muestras\_FyQ.xls  (CONICET Digital) | [main/data/raw/Muestras\_FyQ.xlsx](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/blob/main/data/raw/Muestras_FyQ.xlsx) |
| **Dataset procesado** | Muestras\_FyQ\_procesado.csv | [main/data/interim/Muestras\_FyQ\_procesado.csv](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/blob/main/data/interim/Muestras_FyQ_procesado.csv) |
| **Notebook de preprocesamiento** | Preprocesamiento\_Afectación\_de\_incendio\_en\_la\_turba.ipynb | [main/notebooks/Preprocesamiento\_Afectaci%C3%B3n\_de\_incendio\_en\_la\_turba.ipynb](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/blob/main/notebooks/Preprocesamiento_Afectaci%C3%B3n_de_incendio_en_la_turba.ipynb) |
| **Notebook de modelado** | Modelado LogReg \+ KNN, validación, importancia y escenarios | [/main/notebooks/Modelado\_Afectaci%C3%B3n\_de\_incendio\_en\_la\_turba.ipynb](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/blob/main/notebooks/Modelado_Afectaci%C3%B3n_de_incendio_en_la_turba.ipynb) |
| **Documento de presentación** | Entregas de documentación | [main/docs](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/tree/main/docs) |
| **Video de presentación** | Explicación del modelo y resultados | [main/video](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/tree/main/video) |
| **README del repositorio** | Readmme general | [main/README.MD](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera/blob/main/README.MD) |

Estructura del repositorio:

├── data/  
│   ├── raw/Muestras\_FyQ.xlsx  
│   └── processed/Muestras\_FyQ\_procesado.csv  
├── notebooks/  
│   ├── Preprocesamiento\_Afectación\_de\_incendio\_en\_la\_turba.ipynb  
│   └── Modelado\_Afectación\_de\_incendio\_en\_la\_turba.ipynb  
├── docs/  
│   └── 1-Descripción y Formulación del Objetivo  
│   └── 2-Descripción del dataset y origen  
│   └── 3-Presentación del Modelo y Análisis de Resultados  
├── video/  
│   └── Entrega3\_Video.mp4  
├── reports/figures/  
│   ├── confusion\_matrices.png  
│   ├── f1\_por\_fold.png  
│   └── importancia\_consenso.png  
└── README.md  

---

## **10\. Cierre**

El modelo abordó el problema formulado de manera metodológicamente rigurosa: validación agrupada por turbera para evitar fuga entre unidades de muestreo, pipelines ajustados solo con train, comparación de dos algoritmos con ajuste de hiperparámetros por fold, y selección de variables por consenso entre dos criterios independientes. Los resultados, aunque indicativos por el reducido número de turberas (7), permiten concluir que:

* LogReg y KNN son equivalentes en capacidad de generalización dentro de la variabilidad observada.  
* El gradiente ordinal se captura, con Moderate como clase más difícil.  
* Un subconjunto de 4 variables {BD, DOB, MO, VWC} preserva la capacidad predictiva, habilitando una reducción técnicamente factible de determinaciones de laboratorio.  
* La principal limitación es el número de turberas, por lo que se recomienda ampliar el muestreo para consolidar la robustez estadística de las conclusiones.

**___________________________________________________________________________________________________________________________________________**

**Materia:** Aprendizaje Automático (2C 2026\) \| **Institución:** Centro Politécnico Superior Malvinas Argentinas       
**Docente:** Lic. Nicolas Caballero \| **Alumna:** Sandra Vargas 
