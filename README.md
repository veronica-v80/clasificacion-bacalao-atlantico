# Clasificación de registros de bacalao del Atlántico mediante Aprendizaje Automático

## 1. Descripción del proyecto

El bacalao del Atlántico (*Gadus morhua*) es una especie de interés para la investigación pesquera en el mar del Norte. El análisis de los datos obtenidos durante las campañas científicas permite estudiar cómo se relacionan las características de los lugares muestreados con los registros de esta especie.

Este proyecto tiene como finalidad desarrollar un modelo de Aprendizaje Automático capaz de **estimar, antes de realizar un lance de investigación pesquera, la probabilidad de obtener registros de bacalao del Atlántico**, a partir de las características geográficas y de profundidad del lugar donde se prevé efectuar el muestreo.

Para ello, se utilizaron datos científicos de ICES-DATRAS, correspondientes a las campañas *North Sea International Bottom Trawl Survey* (NS-IBTS), realizadas durante el primer trimestre de los años 2020 a 2025.

A partir de estos datos, se construyó un conjunto de 1.970 lances válidos y se desarrolló un modelo de clasificación supervisada basado en **regresión logística**, utilizando como variables predictoras la latitud, la longitud y la profundidad del lance.

El trabajo comprende la exploración y preparación de los datos, la comparación de algoritmos, el entrenamiento y la evaluación del modelo seleccionado, así como la interpretación de sus resultados. Para evaluar su capacidad de generalización, se utilizaron los registros de 2025 como conjunto de prueba y se realizó una evaluación adicional, de carácter preliminar, con datos de 2026.

El propósito de esta herramienta es explorar su posible utilidad como apoyo para la planificación de futuras campañas de investigación pesquera, aportando una estimación anticipada basada en patrones identificados en los registros históricos.

Es importante aclarar que el modelo predice la **probabilidad de obtener registros de bacalao durante un lance**, y no la presencia o ausencia real de la especie en una zona geográfica. Para utilizarlo antes de realizar el lance, es necesario conocer previamente la ubicación y la profundidad aproximada del lugar donde se realizará el muestreo.

## 2. Objetivo general

Desarrollar y evaluar un modelo de Aprendizaje Automático supervisado que permita estimar, antes de realizar un lance de investigación pesquera, la probabilidad de obtener registros de bacalao del Atlántico (*Gadus morhua*) en el mar del Norte, utilizando información geográfica y de profundidad del lugar de muestreo, a partir de datos científicos de ICES-DATRAS correspondientes al primer trimestre de las campañas NS-IBTS del período 2020–2025, con el propósito de explorar su utilidad como herramienta de apoyo para la planificación de futuras campañas de investigación.

## 3. Objetivos específicos

1. Explorar y preparar los datos científicos de ICES-DATRAS correspondientes a las campañas NS-IBTS del primer trimestre de 2020 a 2025, identificando los lances válidos y construyendo la variable objetivo que representa la presencia o ausencia de registros de bacalao.

2. Analizar las características geográficas y de profundidad de los lances, seleccionando variables predictoras que puedan conocerse antes de realizar un nuevo muestreo.

3. Entrenar y comparar modelos de clasificación supervisada, como Regresión Logística, K-Nearest Neighbors (KNN) y Árbol de Decisión, para seleccionar una alternativa adecuada al problema planteado.

4. Evaluar el desempeño del modelo seleccionado mediante validación temporal y métricas de clasificación (accuracy, precision, recall, F1-score y matriz de confusión), utilizando datos de 2025 como conjunto de prueba y realizando una evaluación adicional con registros de 2026.

5. Interpretar los resultados obtenidos, identificar las principales limitaciones del modelo y analizar su posible utilidad como herramienta de apoyo para la planificación de futuras campañas de investigación pesquera.

## 4. Fuente de datos

Los datos utilizados en este proyecto provienen de **ICES-DATRAS**, una plataforma científica que reúne información de campañas internacionales de investigación pesquera.

Para el desarrollo y entrenamiento del modelo se utilizaron registros de la campaña *North Sea International Bottom Trawl Survey* (NS-IBTS), correspondientes al primer trimestre de los años 2020 a 2025.

**Características de la fuente:**

- **Organismo:** International Council for the Exploration of the Sea (ICES).
- **Base de datos:** Database of Trawl Surveys (DATRAS).
- **Campaña:** North Sea International Bottom Trawl Survey (NS-IBTS).
- **Área geográfica:** Mar del Norte.
- **Período principal:** Primer trimestre de 2020 a 2025.
- **Período adicional:** Primer trimestre de 2026.
- **Especie estudiada:** Bacalao del Atlántico (*Gadus morhua*).

**Fuente oficial:** https://www.ices.dk/data/data-portals/Pages/DATRAS.aspx

### Archivos utilizados

Los datos se encuentran organizados en las siguientes carpetas del repositorio:

| Archivo | Descripción |
|---|---|
| `data/raw/datras_ns_ibts_2020_2025_original.csv` | Datos originales utilizados para explorar, preparar, entrenar y evaluar el modelo con registros del período 2020–2025. |
| `data/processed/lances_bacalao_procesado.csv` | Dataset procesado de 1.970 lances válidos, utilizado para desarrollar el modelo de clasificación. |
| `data/raw/datras_ns_ibts_2026_original.csv` | Datos adicionales de 2026 utilizados para realizar una evaluación preliminar del modelo ya entrenado. |

### Uso de los datos de 2026

Los registros de 2026 se utilizaron para comprobar cómo se desempeña el modelo frente a datos de un año posterior al período principal de estudio.

Estos datos no se utilizaron para entrenar ni ajustar el modelo. Su incorporación permitió realizar una evaluación adicional de su capacidad para identificar registros de bacalao en nuevos lances de investigación.

Esta evaluación se considera **preliminar**, debido a que corresponde a los registros disponibles de 2026 y no necesariamente representa la totalidad de las campañas de ese año.

Los datasets incluidos en el repositorio permiten consultar los datos utilizados y facilitar la reproducción del análisis. **Se reconoce a ICES-DATRAS como fuente original de la información.**

## 5. Descripción y características del dataset

El conjunto de datos original de ICES-DATRAS contiene información sobre los lances realizados durante las campañas científicas y los registros biológicos de las especies identificadas.

Los datos se organizan en dos tipos de registros:

- **HH (Haul Header):** contiene información general de cada lance, como su ubicación, profundidad, duración y características del muestreo. Se identificaron 2.034 registros y 71 columnas.
- **HL (Haul Length):** contiene información biológica de las especies registradas durante los lances. Se identificaron 134.934 registros y 30 columnas.

### Preparación del conjunto de datos

Para construir el dataset utilizado en el proyecto, se seleccionaron los lances considerados válidos por ICES-DATRAS, identificados mediante el código `HaulVal = V`.

A partir de esta selección se obtuvieron **1.970 lances válidos**. Posteriormente, se relacionó la información general de cada lance con sus registros biológicos para identificar cuáles presentaban registros válidos de bacalao del Atlántico.

Como resultado, se construyó un conjunto de datos procesado de **1.970 observaciones y 13 columnas**, con una única fila por lance.

### Variable objetivo

La variable `presencia_bacalao` permite clasificar los lances en dos categorías:

- **Clase 1:** 1.335 lances con registros de bacalao (67,8 %).
- **Clase 0:** 635 lances sin registros de bacalao (32,2 %).

Es importante señalar que la clase 0 representa la ausencia de registros de la especie durante el lance y no demuestra que el bacalao esté ausente de esa zona.

### Variables predictoras del modelo final

Aunque durante la exploración se analizaron diferentes características de los lances, para entrenar el modelo definitivo se seleccionaron tres variables:

| Variable | Descripción |
|---|---|
| `ShootLat` | Latitud del lugar donde se realiza el lance. |
| `ShootLong` | Longitud del lugar donde se realiza el lance. |
| `Depth` | Profundidad del lugar de muestreo. |

Estas variables fueron seleccionadas porque permiten estudiar la relación entre la ubicación, la profundidad y los registros de bacalao.

La duración del lance (`HaulDur`) también se examinó durante la exploración, pero no se utilizó en el modelo final, dado que su valor real se conoce después de realizar el muestreo. Esta decisión mantiene la coherencia con el objetivo de realizar predicciones anticipadas.

### Otras variables del dataset

El conjunto procesado también conserva ocho columnas identificadoras: `Survey`, `Quarter`, `Year`, `Country`, `Ship`, `Gear`, `HaulNo` y `StNo`.

Estas variables permiten identificar cada lance y su contexto, pero no se utilizaron como entradas del modelo definitivo.

### Calidad de los datos

Durante el procesamiento se verificó la correspondencia entre los registros de lances y la información biológica, se revisaron los tipos de datos y se comprobó la ausencia de valores faltantes en las variables seleccionadas.

El dataset procesado contiene una única observación por lance válido, sin duplicados según los identificadores empleados.

El procedimiento de preparación y análisis se encuentra documentado en el notebook principal `notebooks/modelo_final_bacalao.ipynb`. La descripción detallada de las columnas está disponible en `docs/diccionario_datos.md`.

## 6. Metodología

Para desarrollar el modelo de clasificación se siguieron diferentes etapas, desde la exploración de los registros científicos hasta la evaluación de su capacidad para realizar predicciones sobre datos de años posteriores.

### Exploración y preparación de los datos

Inicialmente, se analizaron los registros de ICES-DATRAS para conocer su estructura, identificar las variables disponibles y verificar la calidad de los datos.

Se seleccionaron los lances válidos, se relacionaron sus características generales con los registros biológicos y se construyó la variable objetivo `presencia_bacalao`.

Posteriormente, se preparó un conjunto de 1.970 lances válidos y se seleccionaron tres variables predictoras: latitud, longitud y profundidad.

### División temporal de los datos

Para evaluar el modelo de manera más realista, se realizó una división cronológica de los registros:

- **Entrenamiento:** 1.609 lances correspondientes al período 2020–2024.
- **Prueba:** 361 lances correspondientes al año 2025.

Esta separación permitió entrenar el modelo con información histórica y evaluar su desempeño sobre registros de un año posterior, sin utilizar los datos de prueba durante el entrenamiento.

### Entrenamiento y comparación de modelos

Durante el desarrollo se exploraron tres algoritmos de clasificación supervisada:

- **Regresión Logística:** estima la probabilidad de pertenecer a cada clase a partir de las variables predictoras.
- **K-Nearest Neighbors (KNN):** clasifica los registros considerando la semejanza con otros ejemplos.
- **Árbol de Decisión:** realiza clasificaciones mediante reglas basadas en las características de los datos.

Para comparar las alternativas se utilizaron métricas de clasificación y validación temporal.

Finalmente, se seleccionó la **regresión logística** por su desempeño competitivo y por permitir una interpretación relativamente sencilla de los resultados.

### Validación temporal y ajuste del modelo

Además de separar los datos de entrenamiento y prueba, se aplicó validación cruzada temporal, respetando el orden cronológico de los registros.

Este procedimiento consistió en entrenar el modelo con datos de años anteriores y evaluarlo sobre un año posterior, ampliando progresivamente el período de entrenamiento.

También se exploraron diferentes valores del parámetro de regularización `C` mediante `GridSearchCV`.

Aunque se observaron pequeñas diferencias entre las configuraciones, se conservó el modelo de regresión logística con `C = 1`, debido a que presentó un rendimiento muy cercano al mejor valor encontrado y permitió mantener una configuración sencilla.

El modelo se implementó mediante un `Pipeline` que integra la estandarización de las variables con `StandardScaler` y el algoritmo `LogisticRegression`.

### Evaluación del desempeño

El rendimiento del modelo se analizó mediante cuatro métricas principales:

- **Accuracy (exactitud):** proporción total de predicciones correctas.
- **Precision (precisión):** proporción de predicciones positivas que realmente corresponden a lances con registros de bacalao.
- **Recall (sensibilidad):** proporción de lances con registros de bacalao que el modelo logra identificar.
- **F1-score:** medida que combina precision y recall para evaluar su equilibrio.

También se utilizaron matrices de confusión para observar los aciertos y errores correspondientes a ambas clases.

La evaluación principal se realizó sobre los 361 lances del año 2025, que no participaron en el entrenamiento.

### Evaluación adicional con registros de 2026

Para ampliar el análisis, se utilizó un conjunto adicional de datos correspondientes a 2026.

Después de aplicar los criterios de preparación, se obtuvieron **324 lances válidos**, sobre los cuales se evaluó el modelo previamente entrenado, sin modificar sus parámetros.

Esta evaluación permitió observar cómo se comportaba el modelo frente a registros posteriores a los utilizados durante su desarrollo.

Los resultados de 2026 se consideran preliminares y se interpretan como una evaluación complementaria, no como una confirmación definitiva de su desempeño futuro.

### Guardado del modelo

Una vez finalizado el entrenamiento, el modelo seleccionado se guardó en el archivo `models/modelo_logistico_bacalao.joblib`.

Este archivo conserva el procesamiento de las variables y el clasificador entrenado, permitiendo reutilizarlos posteriormente sin necesidad de repetir todo el entrenamiento.

El procedimiento completo se encuentra documentado en `notebooks/modelo_final_bacalao.ipynb`.

## 7. Resultados y métricas del modelo

El modelo de regresión logística se evaluó utilizando registros de años posteriores a los empleados durante su entrenamiento. Esta estrategia permitió analizar su capacidad para realizar predicciones sobre nuevos datos.

### Resultados de la evaluación de 2025

La evaluación principal se realizó sobre **361 lances correspondientes a 2025**, que no fueron utilizados durante el entrenamiento.

| Métrica | Resultado |
|---|---:|
| Accuracy (exactitud) | 0,759 (75,9 %) |
| Precision (precisión) | 0,752 (75,2 %) |
| Recall (sensibilidad) | 0,950 (95,0 %) |
| F1-score | 0,839 (83,9 %) |

Los resultados indican que el modelo clasificó correctamente aproximadamente el **75,9 % de los lances evaluados**.

El valor de recall fue especialmente elevado: el modelo identificó correctamente el **95 % de los lances que presentaban registros de bacalao**.

Sin embargo, la precisión del 75,2 % indica que algunas predicciones de presencia correspondieron a lances en los que no se registró la especie.

La matriz de confusión mostró 227 lances con registros de bacalao correctamente identificados y 47 lances sin registros correctamente clasificados. También se produjeron 75 falsas predicciones de presencia y 12 casos en los que el modelo no identificó registros de bacalao existentes.

Esto evidencia que el modelo presenta una mayor capacidad para reconocer los lances con registros de bacalao que aquellos sin registros.

### Evaluación adicional con datos de 2026

Para comprobar el comportamiento del modelo frente a datos posteriores, se realizó una evaluación complementaria con **324 lances válidos correspondientes a 2026**.

| Métrica | Resultado |
|---|---:|
| Accuracy (exactitud) | 0,738 (73,8 %) |
| Precision (precisión) | 0,752 (75,2 %) |
| Recall (sensibilidad) | 0,913 (91,3 %) |
| F1-score | 0,825 (82,5 %) |

En esta evaluación, el modelo clasificó correctamente el 73,8 % de los lances y logró identificar aproximadamente el 91,3 % de aquellos con registros de bacalao.

La matriz de confusión mostró 200 lances con registros correctamente identificados y 39 lances sin registros correctamente clasificados. Se observaron también 66 falsas predicciones de presencia y 19 casos de registros de bacalao no identificados.

Aunque los resultados fueron ligeramente inferiores a los obtenidos en 2025, el modelo mantuvo una sensibilidad elevada para identificar los lances con registros de la especie.

Estos resultados deben interpretarse como preliminares, dado que corresponden al conjunto de registros disponibles de 2026 utilizado en el análisis.

### Comparación de resultados

| Métrica | Año 2025 | Año 2026 |
|---|---:|---:|
| Accuracy | 75,9 % | 73,8 % |
| Precision | 75,2 % | 75,2 % |
| Recall | 95,0 % | 91,3 % |
| F1-score | 83,9 % | 82,5 % |

La comparación muestra que el rendimiento del modelo se mantuvo relativamente estable entre ambas evaluaciones, con una disminución moderada en accuracy, recall y F1-score durante 2026.

El principal resultado es su **alta capacidad para identificar lances con registros de bacalao**. No obstante, también se observa una dificultad importante para distinguir los lances sin registros, lo que genera predicciones positivas incorrectas.

Por lo tanto, el modelo presenta resultados prometedores como ejercicio de clasificación predictiva, aunque requiere mejoras antes de considerarse una herramienta confiable para apoyar decisiones operativas en campañas científicas.

## 8. Conclusiones del proyecto

El desarrollo de este proyecto permitió aplicar técnicas de Aprendizaje Automático a datos científicos de investigación pesquera para estudiar la posibilidad de anticipar registros de bacalao del Atlántico en futuros lances del mar del Norte.

A partir de la exploración y preparación de los datos de ICES-DATRAS, se construyó un conjunto de 1.970 lances válidos y se entrenaron y compararon diferentes algoritmos de clasificación supervisada. Finalmente, se seleccionó la regresión logística por su rendimiento competitivo y su relativa facilidad de interpretación.

Los resultados obtenidos en las evaluaciones de 2025 y 2026 muestran que el modelo presenta una **alta capacidad para identificar lances en los que se registró bacalao**. Este comportamiento es relevante para el objetivo del proyecto, ya que permite explorar la posibilidad de realizar estimaciones antes de efectuar un muestreo, utilizando la ubicación y la profundidad aproximada del lugar.

Sin embargo, también se identificó una dificultad importante: el modelo tiende a clasificar como positivos algunos lances en los que no se registró la especie. Esto significa que una predicción favorable no garantiza que se obtengan registros de bacalao durante el muestreo.

La evaluación adicional de 2026 permitió observar que el modelo mantuvo un comportamiento similar al obtenido en 2025, aunque con una disminución moderada en algunas métricas. Este resultado aporta evidencia preliminar sobre su capacidad de generalización, pero no resulta suficiente para asegurar que mantendrá el mismo desempeño en otras campañas o condiciones.

En conclusión, el proyecto demuestra que es posible identificar patrones predictivos a partir de información geográfica y de profundidad de los lances. No obstante, **el modelo debe considerarse una herramienta experimental**, cuyo potencial para apoyar la planificación de campañas científicas requiere nuevas evaluaciones y mejoras.

El trabajo también permitió reconocer la importancia de preparar adecuadamente los datos, respetar el orden temporal durante la evaluación e interpretar las métricas considerando tanto los aciertos como los errores del modelo.


## 9. Alcances y limitaciones del proyecto

El modelo desarrollado permite estimar la probabilidad de obtener registros de bacalao del Atlántico en un lance de investigación pesquera, utilizando información sobre la ubicación geográfica y la profundidad del lugar donde se prevé realizar el muestreo.

Su principal alcance consiste en explorar la posibilidad de utilizar datos históricos para anticipar resultados de futuros lances. Sin embargo, es necesario considerar algunas limitaciones.

### Principales limitaciones

**1. Diferencia entre ausencia de registros y ausencia de la especie**

Un lance sin registros de bacalao no significa necesariamente que la especie esté ausente de esa zona. La falta de detección puede depender de las condiciones del muestreo y de otros factores que no fueron incluidos en el análisis.

**2. Dificultad para identificar lances sin registros**

Aunque el modelo logró reconocer una proporción elevada de lances con registros de bacalao, presentó mayores dificultades para clasificar aquellos sin registros. Esto produjo predicciones positivas incorrectas, que deben tenerse en cuenta al interpretar sus resultados.

**3. Cantidad limitada de variables predictoras**

El modelo utiliza solamente tres variables: latitud, longitud y profundidad. Existen otros factores ambientales y biológicos, como la temperatura del agua, que podrían influir en los registros de la especie y contribuir a mejorar las predicciones.

**4. Alcance geográfico y temporal**

Los datos corresponden a campañas científicas realizadas en el mar del Norte durante el primer trimestre de los años estudiados. Por lo tanto, los resultados no pueden generalizarse automáticamente a otras regiones, estaciones del año o modalidades de muestreo.

**5. Aplicación anticipada del modelo**

Para realizar una predicción antes de un lance, se necesita conocer previamente la ubicación y la profundidad aproximada del lugar de muestreo. Esta información debe obtenerse antes de utilizar el modelo, ya que no estaría disponible como resultado del propio lance.

### Posibles mejoras futuras

Como continuidad del proyecto, se podrían incorporar nuevas variables ambientales, ampliar la cantidad de campañas analizadas y evaluar otros algoritmos de clasificación.

También sería conveniente mejorar la identificación de los lances sin registros de bacalao y realizar nuevas evaluaciones con datos posteriores, para comprobar si el modelo mantiene su desempeño en diferentes condiciones.

En consecuencia, el modelo constituye una primera aproximación al problema predictivo y debe considerarse una herramienta experimental, cuyo uso en la planificación real de campañas requeriría evaluaciones adicionales.

## 10. Organización del proyecto 

El proyecto se organizó utilizando **Cookiecutter Data Science**, una plantilla que proporciona una estructura de carpetas para desarrollar proyectos de análisis de datos y Aprendizaje Automático.

Esta organización permite separar los datos originales de los procesados, conservar el código de análisis, almacenar el modelo entrenado y reunir la documentación. De esta manera, se facilita la comprensión del trabajo, el seguimiento de sus etapas y la reproducción de los resultados.

### Estructura del repositorio

```text
clasificacion-bacalao-atlantico/
│
├── data/
│   ├── raw/
│   │   ├── datras_ns_ibts_2020_2025_original.csv
│   │   └── datras_ns_ibts_2026_original.csv
│   │
│   ├── processed/
│   │   └── lances_bacalao_procesado.csv
│   │
│   ├── external/
│   └── interim/
│
├── notebooks/
│   └── modelo_final_bacalao.ipynb
│
├── models/
│   └── modelo_logistico_bacalao.joblib
│
├── docs/
│   └── diccionario_datos.md
│
├── reports/
│   └── figures/
│
├── references/
│
├── clasificacion_bacalao_atlantico/
│
├── README.md
├── .gitignore
├── environment.yml
├── pyproject.toml
└── Makefile
```

### Descripción de las carpetas principales

| Carpeta o archivo | Función |
|---|---|
| `data/raw/` | Contiene los datos originales obtenidos de ICES-DATRAS, correspondientes a los períodos 2020–2025 y 2026. |
| `data/processed/` | Almacena el dataset preparado para entrenar y evaluar el modelo. |
| `data/external/` y `data/interim/` | Carpetas previstas por Cookiecutter para datos externos o resultados intermedios. |
| `notebooks/` | Contiene el notebook principal con la exploración, preparación de datos, entrenamiento, evaluación e interpretación de resultados. |
| `models/` | Guarda el modelo de regresión logística entrenado, para su posterior reutilización. |
| `docs/` | Contiene el diccionario de datos y la documentación complementaria. |
| `reports/figures/` | Carpeta destinada a guardar gráficos y figuras cuando se requieran archivos independientes. |
| `references/` | Espacio reservado para materiales de consulta y documentación adicional. |
| `clasificacion_bacalao_atlantico/` | Carpeta de código Python creada por la plantilla Cookiecutter. |
| `README.md` | Presenta el proyecto, su metodología, resultados, conclusiones e instrucciones de reproducción. |
| `.gitignore` | Evita incorporar al repositorio determinados archivos temporales y de configuración privada. |

La estructura permite distinguir los archivos utilizados en el análisis de los resultados obtenidos y del modelo entrenado.

El desarrollo principal se encuentra en `notebooks/modelo_final_bacalao.ipynb`, mientras que el modelo guardado está disponible en `models/modelo_logistico_bacalao.joblib`.

## 11. Reproducción del análisis

El proyecto fue desarrollado en Python, utilizando Jupyter Notebook para la exploración, preparación de los datos, entrenamiento y evaluación del modelo de Aprendizaje Automático.

### Herramientas y bibliotecas utilizadas

- **Python:** lenguaje de programación utilizado para desarrollar el proyecto.
- **Jupyter Notebook:** entorno donde se documentó y ejecutó el análisis.
- **pandas y NumPy:** manipulación y procesamiento de los datos.
- **Matplotlib:** elaboración de gráficos para explorar e interpretar los resultados.
- **scikit-learn:** entrenamiento, validación y evaluación de los modelos de clasificación.
- **joblib:** almacenamiento del modelo entrenado.
- **Git y GitHub:** control de versiones y publicación del proyecto.

### Pasos para reproducir el análisis

1. Descargar o clonar el repositorio de GitHub.
2. Preparar un entorno de Python con las bibliotecas necesarias.
3. Abrir el archivo `notebooks/modelo_final_bacalao.ipynb` en Jupyter Notebook.
4. Verificar que los datasets originales se encuentren en `data/raw/`.
5. Ejecutar las celdas del notebook en el orden establecido para reproducir la preparación de datos, el entrenamiento y las evaluaciones.

El notebook contiene las explicaciones, los procedimientos y los resultados del análisis.

Además, el repositorio incluye el dataset procesado en `data/processed/`, el diccionario de datos en `docs/` y el modelo entrenado en `models/`.

La reproducción completa requiere contar con las bibliotecas utilizadas y un entorno de Python compatible.

## 12. Video de presentación del proyecto

En el siguiente video se presenta una síntesis del proyecto, incluyendo el problema abordado, los datos utilizados, la metodología de entrenamiento, los resultados de las evaluaciones y las principales conclusiones.

**Video de presentación:** [Ver presentación del proyecto](PEGAR_AQUI_ENLACE_DEL_VIDEO)