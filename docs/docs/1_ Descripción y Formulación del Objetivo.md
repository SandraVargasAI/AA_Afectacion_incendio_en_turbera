  
**APRENDIZAJE AUTOMÁTICO — ENTREGA 1: DESCRIPCIÓN Y FORMULACIÓN DE OBJETIVOS**

**Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo**

**1\. Descripción del problema, contexto y relevancia**

Las turberas de Sphagnum de Tierra del Fuego acumulan carbono en forma de materia orgánica, y los incendios pueden alterar sus propiedades fisicoquímicas y comprometer esa función. Estimar la severidad del daño suele requerir muestreo de campo y análisis de laboratorio, que son lentos y costosos. Como todos los predictores considerados requieren ensayos de laboratorio, se evaluará la posibilidad de reducir tiempos y costos analíticos al identificar si es factible prescindir de determinaciones específicas de mayor complejidad o costo sin comprometer la precisión de la clasificación de la variable objetivo. 

El problema es determinar si la severidad de afectación de incendios forestales de una muestra de turbera (baja, moderada o alta) puede clasificarse a partir de propiedades del suelo medidas en laboratorio (pH, conductividad eléctrica, materia orgánica, densidad aparente, humedad). Si el modelo funciona, permitiría identificar qué indicadores sirven como señal rápida de daño y priorizar análisis y acciones de restauración.

La relevancia del proyecto radica en evaluar qué indicadores aportan mayor información para caracterizar la condición después de producido el incendio y si un conjunto reducido de mediciones permite obtener resultados comparables a los del conjunto completo. Se plantea como una herramienta experimental de apoyo al análisis ambiental, no como un reemplazo de la evaluación de campo. 

El alcance inicial será tabular y no se interpretará la categoría de severidad baja como ausencia de daño, ya que no existe una clase explícita de muestras no quemadas o incendiadas.

**2\. Objetivos del trabajo**

**Objetivo general**

Desarrollar y evaluar modelos de Aprendizaje Automático que clasifiquen la severidad de quemado producida por el incendio forestal (baja, moderada o alta) de muestras de turberas de Tierra del Fuego a partir de sus propiedades fisicoquímicas.

**Objetivos específicos**

1. Explorar el dataset, analizando distribuciones, valores atípicos y valores faltantes.  
2. Preprocesar los datos: verificar y corregir faltantes, codificar y estandarizar las variables según el modelo.  
3. Entrenar y comparar modelos de clasificación supervisada multiclase (Regresión Logística Multinomial y K-NN) mediante validación cruzada agrupada por turbera.  
4. Evaluar el rendimiento con F1 macro, matriz de confusión, precisión y recall por clase, analizando las confusiones entre categorías.  
5. Identificar las variables fisicoquímicas de mayor importancia para la discriminación de la severidad del daño provocado por el incendio.  
6. Comparar formalmente el desempeño de los modelos entrenados con el conjunto completo de predictores frente a un conjunto reducido seleccionado por importancia, determinando la factibilidad técnica y económica  de simplificar el panel de ensayos analíticos.

**3\. Tipo de problema y variable objetivo**

Es un problema de clasificación supervisada multiclase. La variable objetivo es Burn\_Severity, con tres categorías: Low, Moderate y High. 

**4\. Modelos propuestos**

* Regresión Logística Multinomial: modelo de referencia (*baseline*), interpretable.  
* K-Nearest Neighbors (KNN): se requiere estandarizar las variables.  
* Validación: validación cruzada agrupada por turbera, con métricas F1 macro, precisión y recall por clase.

**5\. Fuente de datos**

*Variables fisico-quimicas de turberas después de un incendio forestal*. Aravena Acuña, M. C. A. (2026). CONICET Digital, CADIC. Licencia Creative Commons Reconocimiento 2.5 Argentina. Muestreo en turberas dominadas por Sphagnum cerca de Tolhuin, Tierra del Fuego.

Link: [https\://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253)

**\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_Materia:** Aprendizaje Automático (2C 2026\) \| **Institución:** Centro Politécnico Superior Malvinas Argentinas       **Docente:** Lic. Nicolas Caballero \| **Alumna:** Sandra Vargas 