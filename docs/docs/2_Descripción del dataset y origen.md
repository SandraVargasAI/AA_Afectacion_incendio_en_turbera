**Descripción del dataset y origen**

**Proyecto**: Análisis de la afectación de incendios en turberas mediante aprendizaje automático.

**Fuente**: Aravena Acuña (2026), CONICET Digital – CADIC.

**Dimensiones**: 231 registros y 15 variables.

## **Descripción de las variables**

| Variable | Tipo de dato | Descripción e interpretación |
| ----- | ----- | ----- |
| `N` | Entero (`int64`) | Número o índice de registro de la muestra. Se utiliza para identificar cada observación dentro del conjunto de datos. |
| `Turbera` | Categórica (`object`) | Identificación de la turbera donde se realizó el muestreo. Se registran siete turberas: B1, B3, B8, B9, B11, B21 e I2. |
| `Transecta` | Categórica (`object`) | Identificación de la transecta correspondiente al sector donde se recolectó la muestra. |
| `ID_SOBRE` | Entero (`int64`) | Identificador asociado al sobre o recipiente de recolección de la muestra. |
| `Microtopography` | Categórica (`object`) | Característica de la microtopografía de la turbera. Presenta las categorías HUM (montículo) y HOL (depresión). |
| `Burn_Severity` | Categórica (`object`) | Nivel de severidad del incendio registrado para cada muestra. Presenta tres categorías: Low (baja), Moderate (moderada) y High (alta). Es la variable objetivo del modelo de clasificación. |
| `pH` | Decimal (`float64`) | Medida del nivel de acidez o alcalinidad del material de la turbera. |
| `CE` | Decimal (`float64`) | Conductividad eléctrica. Representa la capacidad del material para conducir corriente eléctrica y aporta información sobre su composición química. |
| `DOB` | Decimal (`float64`) | Variable fisicoquímica cuyo significado y unidad deben confirmarse con la documentación original o la autora del dataset. |
| `MO` | Decimal (`float64`) | Contenido de materia orgánica presente en la muestra. |
| `HD` | Decimal (`float64`) | Variable cuantitativa asociada a una propiedad fisicoquímica del material.  |
| `BD` | Decimal (`float64`) | Densidad aparente (Bulk Density). Representa la masa del material seco por unidad de volumen. |
| `VWC` | Decimal (`float64`) | Contenido volumétrico de agua (Volumetric Water Content). Expresa la proporción de agua respecto del volumen total de la muestra. |
| `Ntotal` | Decimal (`float64`) | Contenido de nitrógeno total de la muestra. Presenta únicamente 62 registros válidos de los 231 disponibles. |
| `GWC` | Decimal (`float64`) | Contenido gravimétrico de agua (Gravimetric Water Content). Representa la cantidad de agua en relación con la masa del material seco. |

## **Categorías de la variable objetivo**

La variable `Burn_Severity` será utilizada para entrenar y evaluar un modelo de clasificación multiclase.

| Categoría | Interpretación | Cantidad | Porcentaje |
| ----- | ----- | ----- | ----- |
| Low | Severidad baja | 71 | 30,7 % |
| Moderate | Severidad moderada | 60 | 26,0 % |
| High | Severidad alta | 100 | 43,3 % |
| Total |  | 231 | 100 % |

Las categorías presentan un orden natural de severidad. Sin embargo, inicialmente se abordará el problema mediante clasificadores multiclase convencionales.

Es importante aclarar que la categoría Low representa una severidad baja y no necesariamente una turbera sin afectación por el incendio.

## **Calidad y disponibilidad de los datos**

A continuación se detalla la variable que presenta valores faltantes, el resto están completas..

| Variable | Registros disponibles | Valores faltantes | Completitud |
| ----- | ----- | ----- | ----- |
| Ntotal | 62 | 169 | 26,8 % |

Se observa que `Ntotal` presenta una proporción elevada de valores faltantes (73,2 %), por lo que se eliminó del dataset del análisis principal.

## **Consideraciones para el modelado**

Para evitar problemas metodológicos durante el entrenamiento del modelo se establecen los siguientes criterios:

* Excluir `N` e `ID_SOBRE`, porque funcionan como identificadores.  
* No utilizar `Turbera` y `Transecta` como predictoras del modelo principal, aunque pueden servir para analizar la distribución de los errores.  
* Eliminar `Ntotal` por su elevada cantidad de valores faltantes.  
* Usar RobustScaler para tratar los valores outlaiers.

## **Fuente y procedencia de los datos**

**Nombre del dataset:** Variables fisico-químicas de turberas después de un incendio forestal.

Autora: Aravena Acuña, Marie Claire Alejandra (2026).

**Institución**: Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET), CADIC.

**Ubicación del estudio:** Turberas dominadas por Sphagnum, cercanas a Tolhuin, Tierra del Fuego, Argentina.

**Repositorio institucional:** [CONICET Digital – Dataset original](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253?utm_source=chatgpt.com).

**Licencia:** Creative Commons Atribución–NoComercial–CompartirIgual 2.5 Argentina (CC BY-NC-SA 2.5), según la información que compartiste del registro institucional.

**Citación bibliográfica:**

Aravena Acuña, Marie Claire Alejandra (2026). Variables fisico-químicas de turberas después de un incendio forestal. Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET). Dataset. [http\://hdl.handle.net/11336/284253](http://hdl.handle.net/11336/284253?utm_source=chatgpt.com)

## **Organización del proyecto – Cookiecutter**

Para mantener una estructura organizada y reproducible, se utilizará la estructura de directorios generada mediante Cookiecutter en el repositorio de GitHub.

Repositorio del proyecto: [AA\_Afectacion\_incendio\_en\_turbera – GitHub](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera?utm_source=chatgpt.com).

| Directorio | Contenido |
| ----- | ----- |
| `data/raw/` | Archivo original descargado del repositorio institucional, sin modificaciones. |
| `data/interim/` | Datos intermedios, luego de las primeras tareas de limpieza y transformación. |
| `data/processed/` | Dataset final preparado para el entrenamiento y evaluación de los modelos. |
| `docs/` | Documentación del proyecto, diccionario de datos, descripción de variables y metodología. |

