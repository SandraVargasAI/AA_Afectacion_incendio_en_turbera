# Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo

<a target="_blank" href="https://cookiecutter-data-science.drivendata.org/">
    <img src="https://img.shields.io/badge/CCDS-Project%20template-328F97?logo=cookiecutter" />
</a>

Proyecto para parcial de AA
---

## 1. Tabla de contenidos.. ...... link a cada tema
---

## 2. Descripción del proyecto

Las turberas de Sphagnum de Tierra del Fuego acumulan carbono en forma de materia orgánica, y los
incendios pueden alterar sus propiedades fisicoquímicas y comprometer esa función. Estimar la
severidad del daño suele requerir muestreo de campo y análisis de laboratorio, que son lentos y
costosos. Como todos los predictores considerados requieren ensayos de laboratorio, se evaluará la
posibilidad de reducir tiempos y costos analíticos al identificar si es factible prescindir de determinaciones
específicas de mayor complejidad o costo sin comprometer la precisión de la clasificación de la variable
objetivo.

El problema es determinar si la severidad de afectación de incendios forestales de una muestra de
turbera (baja, moderada o alta) puede clasificarse a partir de propiedades del suelo medidas en
laboratorio (pH, conductividad eléctrica, materia orgánica, densidad aparente, humedad). Si el modelo
funciona, permitiría identificar qué indicadores sirven como señal rápida de daño y priorizar análisis y
acciones de restauración.

La relevancia del proyecto radica en evaluar qué indicadores aportan mayor información para
caracterizar la condición después de producido el incendio y si un conjunto reducido de mediciones
permite obtener resultados comparables a los del conjunto completo. Se plantea como una herramienta
experimental de apoyo al análisis ambiental, no como un reemplazo de la evaluación de campo.

El alcance inicial será tabular y no se interpretará la categoría de severidad baja como ausencia de daño,
ya que no existe una clase explícita de muestras no quemadas o incendiadas.

**Contexto y propósito general.** 

Explicar:
- Qué problema se aborda y en qué dominio (atmosférico).
- Qué se busca lograr (clasificar, predecir, segmentar, descubrir patrones).
- Por qué es relevante (impacto esperado, utilidad práctica, contexto local si aplica).
- Aclarar ej.: "Proyecto desarrollado para la materia Aprendizaje Automático…"

---

## 3. Objetivos

### Objetivo general
Desarrollar y evaluar modelos de Aprendizaje Automático que clasifiquen la severidad de quemado producida por el incendio forestal (baja, moderada o alta) de muestras de turberas de Tierra del Fuego a partir de sus propiedades fisicoquímicas.

### Objetivos específicos
- Explorar el dataset, analizando distribuciones, valores atípicos y valores faltantes.
- Preprocesar los datos: verificar y corregir faltantes, codificar y estandarizar las variables según el modelo.
- Entrenar y comparar modelos de clasificación supervisada multiclase (Regresión Logística Multinomial y K-NN) mediante validación cruzada agrupada por turbera.
- Evaluar el rendimiento con F1 macro, matriz de confusión, precisión y recall por clase, analizando las confusiones entre categorías.
- Identificar las variables fisicoquímicas de mayor importancia para la discriminación de la severidad del daño provocado por el incendio.
- Comparar formalmente el desempeño de los modelos entrenados con el conjunto completo de predictores frente a un conjunto reducido seleccionado por importancia, determinando la factibilidad técnica y económica de simplificar el panel de ensayos analíticos.

---

## 4. Contexto del problema y relevancia

---

## 5. Tipo de problema
Este proyecto se encuadra dentro del aprendizaje automático supervisado. Se trata de un problema de clasificación multiclase, la variable objetivo se denomina  Burn_Severity y tiene tres categorías a predecir: Low, Moderate y High.

---

## 6. Modelo(s) de aprendizaje automático propuesto(s)
● Regresión Logística Multinomial: modelo de referencia (baseline), interpretable.
● K-Nearest Neighbors (KNN): se requiere estandarizar las variables.
● Validación: validación cruzada agrupada por turbera, con métricas F1 macro, precisión y recall
por clase

... dar formato de tabla???

---

## 7. Dataset utilizado
Variables fisico-quimicas de turberas después de un incendio forestal. Aravena Acuña, M. C. A. (2026).CONICET Digital, CADIC. Licencia Creative Commons Reconocimiento 2.5 Argentina. Muestreo en turberas dominadas por Sphagnum cerca de Tolhuin, Tierra del Fuego.

Link: https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253

---

## 8. Estructura del repositorio

**Organización de carpetas y archivos.** Basado en el esquema Cookiecutter Data Science:

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

---

## 9. Notebooks del proyecto

---

## 10. Preprocesamiento aplicado

**Decisiones de limpieza y transformación.** 
Detallar:
- Limpieza de columnas y nombres.
- Tratamiento de nulos, faltantes etc

---

## 11. Features seleccionadas y variables objetivo

---

## 12. Modelos probados y evaluación

**Comparación de algoritmos y métricas.**

---

## 13. Resultados y salidas generadas

lalalla


---

## Video de presentación

**Enlace al video explicativo.** 
URL..
Describir como se recorr eel video

---
## Autoría

Espacio académico correspondiente a la alumna Sandra Vargas en la asignatura Aprendizaje Automático (dictada por el Lic. Nicolás Caballero), perteneciente al 2do cuatrimestre 2026 de la Tecnicatura Superior en Ciencia de Datos e Inteligencia Artificial en la institución Centro Politécnico Superior Malvinas Argentinas.

- Fin del README.

---

## Project Organization

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

--------

