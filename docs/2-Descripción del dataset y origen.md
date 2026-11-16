**APRENDIZAJE AUTOMÁTICO — DESCRIPCIÓN DEL DATASET**

**Clasificación de la afectación de incendios forestales en turberas de Tierra del Fuego mediante indicadores fisicoquímicos del suelo**

### **1\. Procedencia de los datos**

**Nombre del dataset**: Variables fisico-químicas de turberas después de un incendio forestal.

**Autora**: Aravena Acuña, Marie Claire Alejandra (2026).

**Institución**: Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET), CADIC.

**Ubicación del estudio**: Turberas dominadas por Sphagnum, cercanas a Tolhuin, Tierra del Fuego, Argentina.

**Fecha de muestreo**: se realizó en ocho turberas afectadas por el incendio forestal de 2019, durante la temporada de crecimiento austral 2023-2024 (de diciembre a febrero), aproximadamente cinco años después del evento.

**Repositorio institucional**: [CONICET Digital – Dataset original](https://datosdeinvestigacion.conicet.gov.ar/handle/11336/284253?utm_source=chatgpt.com).

**Licencia**: Creative Commons Atribución–NoComercial–CompartirIgual 2.5 Argentina (CC BY-NC-SA 2.5).

**Citación bibliográfica:**

Aravena Acuña, Marie Claire Alejandra (2026). Variables fisico-químicas de turberas después de un incendio forestal. Consejo Nacional de Investigaciones Científicas y Técnicas (CONICET). Dataset. [http\://hdl.handle.net/11336/284253](http://hdl.handle.net/11336/284253?utm_source=chatgpt.com)

### **2\. Descripción del dataset**

**Fecha de adquisición:** octubre de 2026\.

**Dimensiones y estructura:**

* Cantidad de registros (filas): 231 observaciones .  
* Cantidad de variables (columnas): 15 columnas en total.

**Tipos de variables del dataset:**

* Identificadores y agrupamiento: N (índice original), Turbera (7 sitios: B1, B11, B9, B8, B21, B3, I2), Transecta (T1–T5) e ID\_SOBRE (sobre de muestreo).  
* Diseño y contexto: Microtopography (categórica: HUM \= húmedo/hollow, HOL \= alto/hummock) y Burn\_Severity (variable objetivo ordinal: Low, Moderate, High, definida por evaluación visual en campo).  
* Predictoras fisicoquímicas (continuas): pH (nivel de alcalinidad), CE (conductividad eléctrica), DOB (profundidad de quema), MO (materia orgánica, %), HD (grado de humificación), BD (densidad aparente), VWC (contenido volumétrico de agua) y GWC (contenido gravimétrico de agua).  
* Alta tasa de faltantes: Ntotal (nitrógeno total), con \~73,2 % de celdas vacías, por lo que se excluye de los modelos predictivos.

### 

### **3\. Diccionario de datos**

| Variable | Tipo de dato | Descripción e interpretación |
| :---- | :---- | :---- |
| `N` | Entero (`int64`) | Número o índice de registro de la muestra. Se utiliza para identificar cada observación dentro del conjunto de datos. |
| `Turbera` | Categórica (`object`) | Identificación de la turbera donde se realizó el muestreo. Se registran siete turberas: B1, B3, B8, B9, B11, B21 e I2. |
| `Transecta` | Categórica (`object`) | Identificación de la transecta correspondiente al sector donde se recolectó la muestra. |
| `ID_SOBRE` | Entero (`int64`) | Identificador asociado al sobre o recipiente de recolección de la muestra. |
| `Microtopography` | Categórica (`object`) | Característica de la microtopografía de la turbera. Presenta las categorías HUM (montículo) y HOL (depresión). |
| `Burn_Severity` | Categórica (`object`) | Nivel de severidad del incendio registrado para cada muestra. Presenta tres categorías: Low (baja), Moderate (moderada) y High (alta). Es la variable objetivo del modelo de clasificación. |
| `pH` | Decimal (`float64`) | Medida del nivel de acidez o alcalinidad del material de la turbera. |
| `CE` | Decimal (`float64`) | Conductividad eléctrica. Representa la capacidad del material para conducir corriente eléctrica y aporta información sobre su composición química. |
| `DOB` | Decimal (`float64`) | Profundidad de quema. Representa la cantidad de centímetros de espesor de la turba que se perdieron  por el incendio forestal. Se expresa en centímetros (cm). |
| `MO` | Decimal (`float64`) | Contenido de materia orgánica presente en la muestra. |
| `HD` | Decimal (`float64`) | Grado de humificación.  Representa el nivel de descomposición en el que se encuentra la materia orgánica de la turba.  |
| `BD` | Decimal (`float64`) | Densidad aparente (Bulk Density). Representa la masa del material seco por unidad de volumen. |
| `VWC` | Decimal (`float64`) | Contenido volumétrico de agua (Volumetric Water Content). Expresa la proporción de agua respecto del volumen total de la muestra. |
| `Ntotal` | Decimal (`float64`) | Contenido de nitrógeno total de la muestra.  |
| `GWC` | Decimal (`float64`) | Contenido gravimétrico de agua (Gravimetric Water Content). Representa la cantidad de agua en relación con la masa del material seco. |

### **4\. Descripción de la variable objetivo**

La variable `Burn_Severity` será utilizada para entrenar y evaluar un modelo de clasificación multiclase.

| Categoría | Interpretación | Cantidad | Porcentaje |
| :---- | :---- | :---- | :---- |
| Low | Severidad baja | 71 | 30,7 % |
| Moderate | Severidad moderada | 60 | 26,0 % |
| High | Severidad alta | 100 | 43,3 % |
| Total |  | 231 | 100 % |

Las categorías presentan un orden natural de severidad. Sin embargo, inicialmente se abordará el problema mediante clasificadores multiclase convencionales.

Estas categorías ya están presentes en el archivo, por lo que no será necesario construir niveles de afectación mediante umbrales arbitrarios.

Es importante aclarar que la categoría Low representa una severidad baja y no necesariamente una turbera sin afectación por el incendio.

### **5\. Calidad y disponibilidad de los datos**

A continuación se detalla la variable que presenta valores faltantes.

| Variable | Registros disponibles | Valores faltantes | Porcentaje |
| :---- | :---- | :---- | :---- |
| Ntotal | 62 | 169 | 73,2 % |

Se observa que `Ntotal` presenta una proporción elevada de valores faltantes (73,2 %), por lo que se elimina del dataset del análisis principal. De esta forma se descarta el uso de imputación para rellenar los valores faltantes y generar demasiada información artificial, asegurando así un modelo predictivo libre de imputaciones artificiales y fugas de datos (*data leakage*). 

### **6\. Consideraciones para el modelado**

Para evitar problemas metodológicos durante el entrenamiento del modelo se establecen los siguientes criterios:

* Excluir `N` e `ID_SOBRE`, porque funcionan como identificadores.  
* No utilizar `Turbera` y `Transecta` como predictoras del modelo principal, aunque pueden servir para analizar la distribución de los errores.  
* Eliminar `Ntotal` por su elevada cantidad de valores faltantes.  
* No se eliminarán los valores extremos, se conservarán los datos originales, ya que podrían representar variaciones reales de la turba.   
* Se aplicará transformación logarítmica (np.log1p) para reducir la asimetría.  
* Se aplicará RobustScaler para reducir la influencia de los valores extremos durante el escalado. 

### **7\. Organización del proyecto – Cookiecutter**

Para mantener una estructura organizada y reproducible, se utilizará la estructura de directorios generada mediante Cookiecutter en el repositorio de GitHub.

Repositorio del proyecto: [AA\_Afectacion\_incendio\_en\_turbera – GitHub](https://github.com/SandraVargasAI/AA_Afectacion_incendio_en_turbera?utm_source=chatgpt.com).

| Directorio | Contenido |
| :---- | :---- |
| `data/raw/` | Archivo original descargado del repositorio institucional, sin modificaciones. |
| `data/interim/` | Datos intermedios, luego de las primeras tareas de limpieza y transformación. |
| `data/processed/` | Dataset final preparado para el entrenamiento y evaluación de los modelos. |
| `docs/` | Documentación del proyecto, diccionario de datos, descripción de variables y metodología. |

**___________________________________________________________________________________________________________________________________________**

**Materia:** Aprendizaje Automático (2C 2026\) \| **Institución:** Centro Politécnico Superior Malvinas Argentinas       
**Docente:** Lic. Nicolas Caballero \| **Alumna:** Sandra Vargas 
