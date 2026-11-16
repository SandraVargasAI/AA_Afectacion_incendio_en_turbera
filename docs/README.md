# Breve descripción del contenido de cada documento

**1-Descripción y Formulación del Objetivo.md**

Define el problema, el contexto y la relevancia del proyecto. Presenta el objetivo general y los seis objetivos específicos (explorar, preprocesar, entrenar/comparar modelos, evaluar rendimiento, analizar importancia de variables y comparar escenarios completo vs reducido). Describe el tipo de problema (clasificación supervisada multiclase), la variable objetivo Burn_Severity con sus tres categorías y su distribución por turbera, los modelos propuestos (Regresión Logística Multinomial y KNN) con validación agrupada por turbera, el impacto económico/operativo esperado y las fuentes de datos y referencias bibliográficas (Keeley 2009; Fulazzaky et al. 2022).

**2-Descripción del dataset y origen.md**

Documenta la procedencia de los datos (autora, institución CONICET-CADIC, ubicación en Tolhuin, Tierra del Fuego, fecha de muestreo 2023–2024, repositorio y licencia CC BY-NC-SA 2.5), la estructura del dataset (231 filas × 15 columnas), el diccionario de datos con la descripción de cada variable, la distribución de la variable objetivo (Low 71, Moderate 60, High 100), la calidad de los datos (solo Ntotal con 73,2 % de faltantes) y las consideraciones para el modelado (exclusión de identificadores, transformación log1p, RobustScaler, no eliminar outliers). Incluye la organización del proyecto según Cookiecutter y el repositorio GitHub.

**3-Presentación del Modelo y Análisis de Resultados.md**

Presenta los resultados del modelado: origen de los datos, análisis exploratorio (estadísticas descriptivas, faltantes, duplicados, distribución por turbera y conclusiones del EDA), preprocesamiento aplicado, arquitectura de los modelos (LogReg y KNN con ajuste de hiperparámetros por fold), estrategia de validación (StratifiedGroupKFold principal y GroupKFold de respaldo), métricas de evaluación (accuracy y F1 macro con media ± sd, matrices de confusión, precisión y recall por clase), importancia de variables por consenso ({BD, DOB, MO, VWC}), comparación de escenarios completo vs reducido, interpretación de resultados, conclusiones finales, limitaciones metodológicas, recomendaciones, entregables con enlaces al repositorio Git y cierre.


