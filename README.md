
# Análisis Exploratorio de Datos: Detención de Fraude en Transacciones

## 1. 🧾 Introducción

Este repositorio contiene un proyecto de análisis exploratorio de datos, que tiene como finalidad analizar de forma estructurada las caracteristicas y el comportamiento de las transacciones financieras y los factores asociados a la detección del fraude. El proposito principal del trabajo es construir un dataset final unificado a partir de la integración de dos fuentes distintas, un dataset de transacciones etiquetadas que contiene la variable objetivo `is_fraud` y un dataset que contiene un conjunto de indicadores socioeconómicos agregados por código postal, lo que permite contextualizar el entorno económico y fiscal donde se realiza cada operación.

El proyecto abarca todas las etapas de un análisis exploratorio completo, lo que asegura un análisis fiable. En primer lugar, se realiza una revisión preliminar para comprender la estructura de ambos dataset, además se examina la calidad de los datos para detectar posibles problemas en su formato. A continuación, se aplica un proceso de limpieza y transformación orientado a estandarizar el formato de las columnas, asegurando la coherencia entre variables clave y eliminando columnas que no aportan valor al análisis, para seguidamente realizar la unión de ambas fuentes de datos mediante el zip.

Posteriormente, se crean nuevas variables derivadas que aportar información adicional y enriquecen el dataset final, incluyendo componentes temporales, variables demográficas y métricas relativas que permiten comparar el importe de la transacción con el nivel de renta medio. Estas transformaciones completan y facilitan un análisis más interpretable, ayudando a contextualizar el riesgo.

Con el dataset final preparado, se desarrolla un EDA detallado que incluye un análisis univariante y bivariante, comparaciones frente a la variable objetivo, visualizaciones, evaluación de correlaciones y un bloque de análisis temporal para identificar patrones y observar cómo varía el fraude a lo largo del tiempo. El objetivo es detectar señales descriptivas consistentes, comprender cómo varía el fraude según el contexto económico y temporal, y establecer una base sólida para posibles análisis posteriores.

## 2. 🎯 Objetivos del Análisis

### Objetivo General.

Realizar un análisis exploratorio de datos que permita comprender la estructura, la distribución y las relaciones entre las variables del conjunto de transacciones, incorporando información sobre el contexto socioeconómico por código postal, con el objetivo de identificar patrones asociados al fraude y determinar los factores con mayor relevancia analítica.

### Objetivos específicos

- ***Análisis preliminar*** : Realizar una revisión inicial de ambas bases de datos, transacciones y variables socioeconómicas por ZIP para examinar su estructura y composición, identificando tipos de variables, rangos, posibles incoherencias y aspectos relacionados con calidad de los datos que puedan afectar al análisis posterior.

- ***Limpieza de datos*** : Depurar el conjunto de datos mediante la estandarización de formatos y tipos de dato, corrección de inconsistencias y eliminación de variables irrelevantes para el objetivo del análisis. Además, se verifica la coherencia de las variables clave y se aplican transformaciones necesarias para garantizar la calidad del dataset final.

    Al final de esta fase, se realiza la unión de ambas fuentes de datos a través del código postal **ZIP**, construyendo un dataset integrado y consistente, y verificando la coherencia de la información.

- ***Análisis exploratorio de datos (EDA)*** : Analizar de forma detallada el dataset final para describir la distribución de las variables mediante un análisis univariante y estudiar su relación con la variable objetivo **is_fraud** a traves de análisis bivariante, incorporando visualizaciones y medidas robustas. Asimismo, se generan variables derivadas de tipo temporal, demográfico y económico que facilitan el estudio, lo que permiten profundizar en la identificación de diferencias asociadas a transacciones fraudulentas.

## 3. 📂 Estructura del Proyecto

```text 
|------ data # Conjunto de datos utilizados en el proyecto.
  |---- 1.raw # Datos originales sin procesar.
    |--- fraudTrain.csv # Dataset inicial de transaciones.
    |--- IRSIncomeByZipCode.xlsx # Archivo original con información socieconomica por ZIP.
  |---- 2.processed # Datos transformados y listos para el análisis.
    |-- fraudTrain_limpio.parquet # Versión depurada del dataset de transacciones.
    |-- IRSIncomeByZipCode_limpio.parquet # Datos socioeconómicos por ZIP depurados y estandarizados.
    |-- df_final.parquet # Dataset unificado y preparado para el análisis final.
    |-- df_final_eda.parquet # Dataset final con las variables creadas en el EDA.
    |-- df_final_eda_powerbi.xlsx # Versión en Excel de df_final_eda, generada para facilitar la importación y compatibilidad con Power BI. 
    |-- df_final_eda_powerbi_terminado.xlsx # Versión final en Excel preparada para Power BI, con trans_date formateada como texto (YYYY-MM-DD) para evitar problemas de lectura de fechas.
|------ notebook # Notebooks con el desarrollo del análisis.
    |--- 0.1_analisis_preliminar.ipynb # Exploración inicial y revisión del formato de los datos.
    |--- 0.2_limpieza_y_transformación.ipynb # Procesos de depuración y transformación del dataset.
    |--- 0.3_analis_exploratorio_de_datos.ipynb # Análisis exploratorio completo y visualizaciones.
|------ README.md # Documento principal con la descripción general del proyecto.
|------ requirements.txt # Lista de dependencias del proyecto, librerías necesarias para ejecutar notebooks, scripts y reproducir el análisis.
|------ Informe_EDA.pdf # Informe final con el resumen del proceso, resultados del EDA e interpretaciones principales.
|------ Dashboard.pbix # 
```

## 4. 📋 Descripción del Conjunto de Datos

Este conjunto de datos integra transacciones bancarias y variables socioeconómicas agregadas por código postal, con el fin de identificar patrones y factores asociados al fraude.

Los conjuntos de datos y las variables empleadas en el análisis exploratorio son los siguientes:

`fraudTrain.csv` (Dataset transaciones con tarjeta de credito)

- **index**: Identificador único de cada fila

- **trans_date_trans_time**: Fecha y hora de la transacción

- **cc_num**: Número de tarjeta de crédito del cliente

- **merchant**: Nombre del comercio

- **category**: Categoría del comercio

- **amt**: Importe de la transacción

- **first**: Nombre del titular de la tarjeta

- **last**: Apellidos del titular de la tarjeta

- **gender**: Género del titular de la tarjeta. (F = Female, M = Male)

- **street**: Dirección del titular de la tarjeta.

- **city**: Ciudad del titular de la tarjeta.

- **state**: Estado del titular de la tarjeta.

- **zip**: Código postal del titular de la tarjeta.

- **lat**: Latitud de la ubicación del titular de la tarjeta.

- **long**: Longitud de la ubicación del titular de la tarjeta.

- **city_pop**: Población de la ciudad del titular.

- **job**: Profesión/ocupación del titular.

- **dob**: Fecha de nacimiento del titular.

- **trans_num** : Número/identificador de la transacción.

- **unix_time**: Marca de tiempo UNIX de la transacción.

- **merch_lat**: Latitud de la ubicación del comercio.

- **merch_long**: Longitud de la ubicación del comercio.

- **is_fraud**: Indicador de fraude, es la **variable objetivo**. (1 = fraude, 0 = no fraude)

`IRSIncomeByZipCode.xlsx` (Dataset datos socioeconómicos por código postal)

- **STATE**: Abreviatura de dos letras del estado en el que se encuentra el código postal.

- **ZIPCODE**: Código postal de EE. UU. de cinco dígitos. 

- **Number of returns**: Número total de declaraciones de impuestos presentadas en el código postal.

- **Adjusted gross income (AGI)**: Importe total del ingreso bruto ajustado declarado en el código postal.

- **Avg AGI**: Promedio del ingreso bruto ajustado (AGI) declarado en el código postal.

- **Number of returns with total income**: Total de declaraciones con ingresos totales informados en el código postal.

- **Total income amount**: Importe total de ingresos declarado en el código postal.

- **Avg total income**: Promedio del ingreso total declarado en el código postal. 

- **Number of returns with taxable income**: Total de declaraciones con ingreso imponible informado en el código postal.

- **Taxable income amount**: Importe total del ingreso imponible declarado en el código postal.

- **Avg taxable income**: Promedio del ingreso imponible declarado en el código postal.

`df_final` (Dataset Integrado)

Además de las variables ya descritas en los apartados anteriores, se incluyen las siguientes variables adicionales:

- **avg_agi**: Ingreso bruto ajustado medio por declaración en el ZIP de la transacción.

- **avg_total_income**: Ingreso total medio por declaración en el ZIP de la transacción.

- **avg_taxable_income**: Ingreso imponible medio por declaración en el ZIP de la transacción.

- **trans_date**: Fecha de la transacción (sin hora), en formato datetime.

- **day_of_week**: Día de la semana en que ocurre la transacción (Monday–Sunday).

- **hour**: Hora del día en la que se realiza la transacción (0–23).

- **moment_of_day**: Franja horaria categorizada según la hora de la transacción. (night, early morning, morning, afternoon, evening y late night.)

- **age**: Edad del titular en el momento de la transacción.

- **age_group**: Tramo de edad, segmentación de age en intervalos definidos.

- **taxable_share**: Proporción de ingreso imponible sobre ingreso total en el ZIP.

- **amt_vs_avg_agi**: Ratio entre el importe de la transacción y el AGI medio del ZIP, como medida relativa del importe.

- **income_level**: Nivel de renta del ZIP segmentado por cuartiles a partir de avg_agi (low, lower-middle, upper-middle, high).

- **month**: Mes de la transacción (January-December)

- **is_weekend**: Indicador de fin de semana (0 = laborable, 1 = sábado o domingo).

## 5. 💾 Instalación, requisitos y reproducción del proyecto

### 5.1 Requisitos

- Versión de Python recomendada 3.12.

- Probado en Windows

- Gestor de paquetes: pip

### 5.2 Dependencias principales

- Pandas y NumPy: manipulación, limpieza y transformación de datos.

- Matplotlib y Seaborn: visualizaciones descriptivas (distribuciones, comparativas y relaciones).

- pyarrow: lectura/escritura de ficheros **.parquet.**

- openpyxl: lectura del fichero **.xlsx.**

### 5.3 Instalación

``` bash

pip install -r requirements.txt

```

### 5.4 Reproducir el proyecto

#### 1. Datos originales:

- `data/0.1_raw/fraudTrain.csv`

- `data/0.1_raw/IRSIncomeByZipCode.xlsx`

#### 2. Ejecuta los notebooks en este orden:

1. `notebook/0.1_analisis_preliminar.ipynb`

- Revisión inicial de estructura y calidad de ambas fuentes.

2. `notebook/0.2_limpieza_y_transformación.ipynb`

- Limpieza, normalización y generación de datasets “procesados”.

3. `notebook/0.3_analis_exploratorio_de_datos.ipynb`

- EDA completo, univariante, bivariante, correlaciones y análisis temporal.

- Generación del dataset final.

#### 3. Archivos generados:

Como resultado de ejecutar todo el proceso, se generan:

- `data/0.2_processed/fraudTrain_limpio.parquet`

- `data/0.2_processed/IRSIncomeByZipCode_limpio.parquet`

- `data/0.2_processed/df_final.parquet`

- `data/0.2_processed/df_final_eda.parquet`

- `data/0.2_processed/df_final_eda_powerbi.xlsx`

- `data/0.2_processed/df_final_eda_powerbi_terminado.xlsx`

## 6. ✍️ Recap Sesiones

**Sesión 1**

- Se configura el repositorio en GitHub y se inicializa el proyecto.

- Se define la estructura de carpetas para organizar datos y notebooks.

- Se redacta una primera versión del README con la descripción general del trabajo.

- Se incorporan los datos en bruto, `fraudTrain.csv` y `IRSIncomeByZipCode.xlsx`.

**Sesión 2**

- Se crea el notebook `0.1-Analisis_preliminar.ipynb`.

- Exploración inicial de ambos datasets para entender su estructura y variables, revisar tipos de datos y detectar posibles incoherencias.

- Comprobación de calidad básica valores nulos, duplicados, rangos anómalos y documentación de los ajustes necesarios antes del análisis.

**Sesión 3**

- Desarrollo del notebook `0.2_limpieza_y_transformación.ipynb`.

- Limpieza y estandarización de `fraudTrain.csv` conversión de fechas, ajuste de tipos, homogeneización de categorías y eliminación de variables no relevantes para el EDA.

- Limpieza y estandarización de `IRSIncomeByZipCode.xlsx`, normalización de nombres de columnas, conversión del ZIP a texto, eliminación de códigos postales no válidos y coherencia de formato con el dataset de transacciones.

- Guardado de los datasets depurados en formato Parquet: `fraudTrain_limpio.parquet` y `IRSIncomeByZipCode_limpio.parquet`.

**Sesión 4**

- Integración de ambos datasets mediante **zip**, generando un dataset unificado `df_final` con información de transacciones y variables socioeconómicas.

- Verificación posterior al merge para confirmar consistencia ausencia de duplicados, control de nulos y coherencia de campos comunes.

- Exportación del dataset final unificado como `df_final.parquet` para facilitar el trabajo en etapas posteriores.

**Sesión 5**

- Creación del notebook `0.3_analisis_exploratorio_de_datos.ipynb`.

- Ejecución del análisis exploratorio completo, revisión de calidad, análisis univariante y bivariante, visualizaciones y matriz de correlación.

- Construcción de variables derivadas temporales, demográficas y ratios para enriquecer el análisis y mejorar la interpretación.

- Desarrollo del análisis temporal mes, hora, día de la semana, fin de semana vs laborable para identificar patrones de riesgo.

- Guardado del dataset final con variables generadas como `df_final_eda.parquet`.

**Sesión 6**

- Consolidación de resultados y redacción de observaciones principales de cada bloque del EDA.

- Elaboración del informe final `Informe_EDA.pdf` con el proceso completo, hallazgos relevantes y conclusiones generales.

**Sesión 7**

- Desarrollo del dashboard en Power BI.

- Elaboración del informe final.

**Sesión 8**

- Finalización y revisión del README.

- Finalización y revisión del informe final. 

## 7. 🧷 Resultados y Conclusiones

El análisis exploratorio de datos ha permitido obtener una visión detallada del comportamiento de las transacciones y de los factores asociados a la detección de fraude, integrando las transacciones con información socioeconómica agregada por código postal. Tras la depuración e integración, se trabaja con un dataset final de **325.090** registros y **23** variables, sin evidencias de problemas básicos de calidad, lo que confirma que el conjunto está preparado para el análisis.

Se confirma un fuerte desbalance en la variable objetivo `is_fraud`, ya que aproximadamente el **99,42%** de las transacciones corresponden a no fraude frente a solo un **0,58%** de fraude, algo habitual en escenarios de fraude. Este desbalance condiciona la interpretación, ya que los recuentos absolutos pueden ser engañosos. Por ello, en los apartados comparativos se prioriza el uso de tasas y proporciones para evaluar diferencias entre grupos.

Entre las variables numéricas, el importe de la transacción `amt` destaca por su alta dispersión y presencia de valores extremos, aunque la mediana es **47,10**, el máximo alcanza **25.086,94**, reflejando una distribución claramente asimétrica. En el análisis bivariante, esta variable muestra una separación marcada entre clases, la mediana de `amt` pasa de **46,85** en no fraude a **362,28** en fraude, lo que sugiere que las transacciones fraudulentas tienden a ser de importe notablemente superior. Además, el ratio `amt_vs_avg_agi` es especialmente relevante, su mediana pasa de **0,86** en no fraude a **8,58** en fraude, indicando que los fraudes suelen ser operaciones altas en términos relativos respecto a la renta media del ZIP.

Por el contrario, los indicadores socioeconómicos medios por ZIP como `avg_agi`, `avg_total_income` y `avg_taxable_income` presentan valores centrales muy similares entre clases, en el caso de `avg_agi` la mediana es **48,57** en no fraude frente a **47,71** en fraude, de la misma manera que `avg_total_income`, donde la mediana es **49,59** en no fraude frente a **48,72** en fraude, por último `avg_taxable_income`, la mediana es **40,44** en no fraude frente a **39,20** en fraude, lo que sugiere una capacidad discriminante limitada cuando se analizan de forma aislada, ya que no diferencian claramente entre ambas clases. Este comportamiento también se refleja en la correlación, donde estas variables muestran relaciones casi perfectas entre sí de entre **0,99** y **1,00**, indicando que ambas columnas presentan un comportamiento prácticamente idéntico.

En el análisis de variables categóricas, las diferencias en la tasa de fraude son, en general, moderadas y se concentran sobre todo en algunas categorías concretas. En `category` se observan contrastes más visibles, por ejemplo, **Shopping_net** con un **1,64%** y **Misc_net** con un **1,53%** presentan tasas relativamente más altas que otras categorías como **Health_fitness** con un **0,11%** o **Food_dining** con un **0,19%**. Esto sugiere que el tipo de comercio está asociado a distintos niveles de riesgo, aunque la mayoría de categorías se mantiene en valores bajos. En cambio, variables como `gender` apenas muestran diferencias, ya que **Female** presenta el **0,56%** vs **Male** que presenta el **0,61%**, por lo que su capacidad descriptiva para separar las operaciones fraudulentas de las que no los son, es limitada. 

En el análisis temporal, el patrón más claro aparece en el componente horario. Por franja horaria `moment_of_day`, destacan tasas más elevadas en **late night** con un **1,92%** y **night** con un **1,27%**, frente a franjas diurnas como **morning** que apenas alcanza el **0,10%** o **afternoon** con un **0,13%**. Esta tendencia se refuerza al bajar al detalle por hora, en la tabla por hora aparecen picos especialmente marcados en **22h** y **23h** con un **2,92%** y un **2,71%** respectivamente, mientras que muchas horas diurnas se mantienen en niveles bajos. Por el contrario, el patrón semanal es más suave, las tasas por `day_of_week` se mueven en un rango estrecho aproximadamente entre el **0,48%** y el **0,71%**, lo que indica variaciones pequeñas entre días. En conjunto, el tiempo aporta información útil, pero principalmente a nivel intradía horas y franjas horarias, es donde las diferencias son más consistentes.

En conclusión, el análisis exploratorio confirma que el fraude no responde a un único factor, sino a la combinación de de características económicas, del importe relativo, y del contexto temporal, especialmente a nivel horario. El proyecto deja un dataset final enriquecido que permite identificar los patrones más consistentes y las variables con mayor capacidad descriptiva, dejando una base sólida para fases posteriores de análisis orientados a explicar el fenómeno con mayor detalle o plantear modelos predictivos con métricas que tengan en cuenta el fuerte desbalance de la clase positiva.

## 8. 🤝 Contribuciones

Cualquier contribucion es bien venida, si quiere colaborar en el proyecto, abre un pull request.

## 9. ✍️ Autores

Carlos Hernando

https://github.com/C4rl0s1515