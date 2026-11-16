
# Clasificación de registros de bacalao del Atlántico mediante Aprendizaje Automático

## 1. Descripción del proyecto

El bacalao del Atlántico (*Gadus morhua*) es una especie de interés para la investigación pesquera en el mar del Norte. El análisis de los datos obtenidos durante las campañas científicas permite estudiar cómo se relacionan las características de los lugares muestreados con los registros de esta especie.

Este proyecto tiene como finalidad desarrollar un modelo de Aprendizaje Automático capaz de **estimar, antes de realizar un lance de investigación pesquera, la probabilidad de obtener registros de bacalao del Atlántico**, a partir de las características geográficas y de profundidad del lugar donde se prevé efectuar el muestreo.

Para ello, se utilizaron datos científicos de ICES-DATRAS, correspondientes a las campañas *North Sea International Bottom Trawl Survey* (NS-IBTS), realizadas durante el primer trimestre de los años 2020 a 2025.

A partir de estos datos, se construyó un conjunto de 1.970 lances válidos y se desarrolló un modelo de clasificación supervisada basado en **regresión logística**, utilizando como variables predictoras la latitud, la longitud y la profundidad del lance.

El propósito es explorar su posible utilidad como herramienta de apoyo para la planificación de futuras campañas de investigación pesquera.

**Alcance:** el modelo estima la probabilidad de obtener registros de bacalao durante un lance, no la presencia o ausencia real de la especie en una zona geográfica.

## 2. Objetivo general

Desarrollar y evaluar un modelo de Aprendizaje Automático supervisado que permita estimar, antes de realizar un lance de investigación pesquera, la probabilidad de obtener registros de bacalao del Atlántico (*Gadus morhua*) en el mar del Norte, utilizando información geográfica y de profundidad del lugar de muestreo, a partir de datos científicos de ICES-DATRAS correspondientes al primer trimestre de las campañas NS-IBTS del período 2020–2025, con el propósito de explorar su utilidad como herramienta de apoyo para la planificación de futuras campañas de investigación.

## 3. Objetivos específicos

1. Explorar y preparar los datos científicos de ICES-DATRAS correspondientes a las campañas NS-IBTS del primer trimestre de 2020 a 2025, identificando los lances válidos y construyendo la variable objetivo que representa la presencia o ausencia de registros de bacalao.

2. Analizar las características geográficas y de profundidad de los lances, seleccionando variables predictoras que puedan conocerse antes de realizar un nuevo muestreo.

3. Entrenar y comparar modelos de clasificación supervisada, como Regresión Logística, K-Nearest Neighbors (KNN) y Árbol de Decisión, para seleccionar una alternativa adecuada al problema planteado.

4. Evaluar el desempeño del modelo seleccionado mediante validación temporal y métricas de clasificación (accuracy, precision, recall, F1-score y matriz de confusión), utilizando datos de 2025 como conjunto de prueba y realizando una evaluación adicional con registros de 2026.

5. Interpretar los resultados obtenidos, identificar las principales limitaciones del modelo y analizar su posible utilidad como herramienta de apoyo para la planificación de futuras campañas de investigación pesquera.

## 4. Desarrollo del proyecto en Jupyter Notebook

El proceso completo de exploración y preparación de datos, construcción de la variable objetivo, entrenamiento, validación y evaluación del modelo está documentado en la notebook principal.

**[Ver notebook del proyecto en Jupyter](notebooks/modelo_final_bacalao.ipynb)**

La notebook contiene el código, las visualizaciones, las métricas y los resultados obtenidos durante el desarrollo del proyecto.

## 5. Fuente de datos y características del dataset

Los datos provienen de **ICES-DATRAS (Database of Trawl Surveys)**, una base de datos científica del Consejo Internacional para la Exploración del Mar (ICES).

- **Campaña:** North Sea International Bottom Trawl Survey (NS-IBTS).
- **Área geográfica:** Mar del Norte.
- **Período principal:** Primer trimestre de 2020 a 2025.
- **Fecha de adquisición del conjunto principal:** 12 de octubre de 2026.
- **Especie estudiada:** Bacalao del Atlántico (*Gadus morhua*).

**Fuente oficial:** [ICES-DATRAS](https://www.ices.dk/data/data-portals/Pages/DATRAS.aspx).

### Dataset procesado

Se seleccionaron 1.970 lances válidos (`HaulVal = V`) y se construyó la variable objetivo `presencia_bacalao`:

| Clase | Descripción | Lances | Porcentaje |
|---|---|---:|---:|
| 1 | Con registros de bacalao | 1.335 | 67,8 % |
| 0 | Sin registros de bacalao | 635 | 32,2 % |

La clase 0 representa una **no detección documentada durante el lance** y no demuestra la ausencia real de bacalao en la zona.

El modelo final utiliza tres variables predictoras:

| Variable | Descripción |
|---|---|
| `ShootLat` | Latitud del lance. |
| `ShootLong` | Longitud del lance. |
| `Depth` | Profundidad del lugar de muestreo. |

Para utilizar el modelo antes de un nuevo lance, es necesario disponer previamente de la ubicación y una estimación adecuada de la profundidad del lugar.

**Archivos disponibles:**

- [Datos originales de 2020–2025](data/raw/datras_ns_ibts_2020_2025_original.csv).
- [Dataset procesado](data/processed/lances_bacalao_procesado.csv).
- [Datos adicionales de 2026](data/raw/datras_ns_ibts_2026_original.csv).

Los archivos originales pueden descargarse desde GitHub, aunque su tamaño impide visualizarlos directamente en la vista previa.

**Documentación complementaria:**

- [Origen y adquisición de los datos](docs/origen_datos.md).
- [Diccionario de datos](docs/diccionario_datos.md).

## 6. Metodología

El proyecto comprendió las siguientes etapas:

1. **Exploración y preparación:** análisis de los registros HH y HL, selección de lances válidos, verificación de la calidad de los datos y construcción de la variable objetivo.
2. **Selección de variables:** identificación de las características geográficas y de profundidad que pueden utilizarse para realizar predicciones anticipadas.
3. **Entrenamiento:** implementación y comparación de Regresión Logística, K-Nearest Neighbors (KNN) y Árbol de Decisión.
4. **Validación:** división temporal de los datos y evaluación del rendimiento mediante métricas de clasificación y matrices de confusión.
5. **Interpretación:** análisis de los resultados, las limitaciones y las posibilidades de mejora.

### División temporal

| Conjunto | Período | Lances |
|---|---|---:|
| Entrenamiento y validación | 2020–2024 | 1.609 |
| Prueba independiente | 2025 | 361 |
| Evaluación adicional preliminar | 2026 | 324 |

Se aplicó validación cruzada temporal y se utilizó un `Pipeline` que integra `StandardScaler` y `LogisticRegression`, para evitar fugas de información durante el ajuste de las variables.

Se seleccionó la **regresión logística** por su rendimiento competitivo y su relativa facilidad de interpretación.

El modelo final se conservó con `C = 1`, una configuración cuyo rendimiento fue cercano al mejor resultado obtenido mediante `GridSearchCV`.

Los datos de 2026 se utilizaron para evaluar el modelo previamente entrenado, sin modificar sus parámetros.

**Justificación de los procedimientos:** [Decisiones metodológicas](docs/decisiones_metodologicas.md).

## 7. Resultados principales

El modelo de regresión logística se evaluó con datos de 2025 y, de manera complementaria, con registros disponibles de 2026.

| Métrica | Evaluación 2025 | Evaluación 2026 |
|---|---:|---:|
| Accuracy | 75,9 % | 73,8 % |
| Precision | 75,2 % | 75,2 % |
| Recall | 95,0 % | 91,3 % |
| F1-score | 83,9 % | 82,5 % |

### Interpretación

Los resultados muestran una **alta capacidad para identificar lances con registros de bacalao**, con valores de recall del 95,0 % en 2025 y del 91,3 % en 2026.

Sin embargo, el modelo presentó mayores dificultades para reconocer los lances sin registros de la especie, lo que se reflejó en una cantidad importante de falsos positivos.

La evaluación adicional de 2026 mostró un comportamiento relativamente similar al de 2025, aunque con una disminución moderada en algunas métricas.

### Conclusiones y limitaciones

El proyecto permitió comprobar que las características geográficas y de profundidad contienen información útil para clasificar los registros de bacalao durante los lances científicos estudiados.

No obstante, los resultados deben interpretarse considerando las siguientes limitaciones:

- Dificultad para reconocer correctamente los lances sin registros de bacalao.
- Uso de solamente tres variables predictoras.
- Alcance restringido al área, las campañas y los períodos analizados.
- Necesidad de conocer previamente la ubicación y la profundidad aproximada del lugar para realizar una predicción anticipada.

Como posibles mejoras futuras, se propone incorporar variables ambientales, explorar estrategias para reducir los falsos positivos y ampliar las evaluaciones con nuevos datos.

Por lo tanto, el modelo constituye una **aproximación experimental** y todavía no puede considerarse una herramienta operativa validada para la planificación de campañas científicas.

**Análisis completo:** [Desarrollo del modelo y resultados](docs/modelo_resultados.md).


## 8. Organización del proyecto

El proyecto se organizó utilizando **Cookiecutter Data Science**, una plantilla que proporciona una estructura de carpetas para desarrollar proyectos de análisis de datos y Aprendizaje Automático. Esta permite separar los datos originales de los procesados, conservar el código de análisis, almacenar el modelo entrenado y reunir la documentación técnica, facilitando la comprensión del trabajo y la reproducción de los resultados.

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
│   ├── origen_datos.md
│   ├── diccionario_datos.md
│   ├── decisiones_metodologicas.md
│   └── modelo_resultados.md
│
├── reports/
│   └── figures/
│
├── references/
│   └── fuentes_datos.md
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
| `data/raw/` | Contiene los datos originales obtenidos de ICES-DATRAS. |
| `data/processed/` | Almacena el dataset preparado para entrenar y evaluar el modelo. |
| `data/external/` y `data/interim/` | Carpetas previstas por Cookiecutter para datos externos e intermedios. |
| `notebooks/` | Contiene el desarrollo completo del proyecto en Jupyter Notebook. |
| `models/` | Guarda el modelo de regresión logística entrenado. |
| `docs/` | Contiene la documentación del origen de los datos, el diccionario, las decisiones metodológicas y el análisis de resultados. |
| `reports/figures/` | Carpeta destinada a gráficos y figuras. |
| `references/` | Contiene las fuentes y referencias utilizadas. |
| `clasificacion_bacalao_atlantico/` | Carpeta de código Python creada por Cookiecutter. |
| `README.md` | Presenta los objetivos, la metodología, los resultados y el acceso a la documentación. |
| `.gitignore` | Excluye archivos temporales y otros elementos que no deben incorporarse al repositorio. |

El desarrollo completo se encuentra en la [notebook principal](notebooks/modelo_final_bacalao.ipynb), mientras que el modelo entrenado está disponible en [models/](models/).

La documentación técnica se encuentra organizada en la carpeta [docs/](docs/).

## 9. Reproducción del análisis

El proyecto fue desarrollado en **Python y Jupyter Notebook**, utilizando principalmente pandas, NumPy, Matplotlib, scikit-learn y joblib.

Para reproducir el análisis:

1. Descargar o clonar el repositorio.
2. Preparar un entorno de Python con las dependencias indicadas en `environment.yml`.
3. Verificar que los datasets originales se encuentren en `data/raw/`.
4. Abrir [modelo_final_bacalao.ipynb](notebooks/modelo_final_bacalao.ipynb) en Jupyter Notebook.
5. Ejecutar las celdas en orden para reproducir el procesamiento, entrenamiento y evaluación.

El repositorio también incluye el dataset procesado y el modelo entrenado, lo que facilita la consulta de los resultados obtenidos.

## 10. Video de presentación del proyecto

En el siguiente video se presenta una síntesis del proyecto, incluyendo el problema abordado, la fuente de datos, la metodología, los resultados de las evaluaciones y las principales conclusiones.

**[Ver video de presentación del proyecto](https://drive.google.com/file/d/1WXjzQ8kyhO8HOWAbmn2vG2v30yrvNo4e/view?usp=drive_link)** 
