# Clasificación de presencia del bacalao del Atlántico mediante Aprendizaje Automático

## 1. Descripción del proyecto

Este proyecto tiene como finalidad aplicar técnicas de Aprendizaje Automático para analizar patrones asociados a la distribución espacial del bacalao del Atlántico (*Gadus morhua*) en el mar del Norte.

Se utilizan datos científicos provenientes de ICES-DATRAS, correspondientes a las campañas North Sea International Bottom Trawl Survey (NS-IBTS), durante el primer trimestre de los años 2020 a 2025.

El problema se aborda mediante **clasificación supervisada**, buscando predecir la presencia o ausencia registrada de bacalao en los lances de investigación pesquera a partir de sus características geográficas y operativas.

## 2. Objetivo general

Desarrollar y evaluar modelos de Aprendizaje Automático supervisado para clasificar la presencia o ausencia registrada de bacalao del Atlántico (*Gadus morhua*) en lances de investigación pesquera del mar del Norte, utilizando variables como latitud, longitud, profundidad y duración del lance, a partir de datos de las campañas NS-IBTS correspondientes al primer trimestre del período 2020–2025, con el propósito de identificar patrones asociados a su distribución espacial.

## 3. Objetivos específicos

1. Explorar y preparar los datos de ICES-DATRAS correspondientes al período seleccionado.
2. Identificar las variables geográficas y operativas relacionadas con la presencia o ausencia registrada de bacalao.
3. Entrenar y comparar modelos de clasificación supervisada, como Regresión Logística, KNN y Árbol de Decisión.
4. Evaluar su desempeño mediante métricas como accuracy, precision, recall, F1-score y matriz de confusión.
5. Analizar los patrones espaciales identificados y las limitaciones de las predicciones.

## 4. Fuente de datos
El dataset fue descargado el 12 de octubre de 2026 desde la base científica ICES-DATRAS, seleccionando los registros de la campaña NS-IBTS correspondientes al primer trimestre de los años 2020 a 2025.
- **Organismo:** International Council for the Exploration of the Sea (ICES).
- **Base de datos:** Database of Trawl Surveys (DATRAS).
- **Campaña:** North Sea International Bottom Trawl Survey (NS-IBTS).
- **Área geográfica:** Mar del Norte.
- **Período:** Primer trimestre de 2020 a 2025.
- **Especie:** Bacalao del Atlántico (*Gadus morhua*).

**Fuente oficial:** https://www.ices.dk/data/data-portals/Pages/DATRAS.aspx

Los archivos se organizan en dos conjuntos:

- `data/raw/datras_ns_ibts_2020_2025_original.csv`: datos originales descargados.
- `data/processed/lances_bacalao_procesado.csv`: datos preparados para el análisis y modelado.

Los datasets original y procesado se incluyen en el proyecto para facilitar la consulta y reproducción del análisis. Los datos provienen de ICES-DATRAS, cuya fuente se reconoce en esta documentación.

## 5. Descripción y características del dataset

El conjunto de datos original de ICES-DATRAS contiene dos tipos de registros:

- **HH (Haul Header):** 2.034 registros y 71 columnas con información de los lances de investigación pesquera.
- **HL (Haul Length):** 134.934 registros y 30 columnas con información biológica de las especies registradas.

Para el proyecto se seleccionaron 1.970 lances válidos, de acuerdo con el código de validez `HaulVal = V`.

Después de verificar la información biológica y realizar el procesamiento, se obtuvo un dataset final de **1.970 observaciones y 13 columnas**, organizado en ocho identificadores, cuatro variables predictoras y una variable objetivo.

### Variables identificadoras

Las ocho columnas identificadoras del conjunto procesado son Survey, Quarter, Year, Country, Ship, Gear, HaulNo y StNo. Estas variables se conservan como texto (str) y permiten identificar cada lance y su contexto. No se utilizarán inicialmente como variables predictoras.

### Variables predictoras

| Variable | Descripción | Tipo de dato |
|---|---|---|
| `Depth` | Profundidad del lance | Entero (`int64`) |
| `ShootLat` | Latitud del lance | Decimal (`float64`) |
| `ShootLong` | Longitud del lance | Decimal (`float64`) |
| `HaulDur` | Duración del lance | Entero (`int64`) |

### Variable objetivo

presencia_bacalao: variable binaria de tipo entero (int64) que representa la presencia o ausencia de registros válidos de bacalao del Atlántico mediante los valores 0 y 1.

- **Clase 1:** 1.335 lances con registros de bacalao (67,8 %).
- **Clase 0:** 635 lances sin registros de bacalao (32,2 %).

La clase 0 representa ausencia de detección documentada durante el lance y no implica necesariamente que la especie esté ausente de la zona.

### Calidad de los datos

Durante el preprocesamiento se verificó la correspondencia entre los registros HH y HL, se seleccionaron los lances válidos y se transformaron las variables predictoras a formato numérico.

El conjunto procesado no presenta valores faltantes y contiene una única observación por lance, sin duplicados según los identificadores utilizados.

El procedimiento completo se encuentra documentado en `notebooks/exploracion_datos_datras.ipynb`.

## 6. Metodología

El proyecto contempla cinco etapas:

**Etapa 1. Exploración de datos:** análisis de la estructura del dataset, tipos de variables, valores faltantes y distribución de los registros.

**Etapa 2. Preprocesamiento:** limpieza de datos, construcción y verificación de la variable objetivo, selección de variables predictoras y preparación del conjunto de datos.

### Procedimiento de preprocesamiento realizado

Para construir el conjunto de datos de clasificación se realizaron las siguientes operaciones:

1. Separación de los registros HH (lances) y HL (información biológica).
2. Selección de los 1.970 lances válidos mediante el código `HaulVal = V`.
3. Verificación de la correspondencia entre los lances y sus registros biológicos.
4. Identificación de los registros válidos de *Gadus morhua*.
5. Construcción de la variable binaria `presencia_bacalao`, diferenciando los lances con y sin registros válidos de la especie.
6. Conversión de las variables predictoras a formato numérico y tratamiento de códigos de datos faltantes.
7. Verificación de valores ausentes y duplicados.

Como resultado, se obtuvo un dataset procesado de 1.970 observaciones y 13 columnas, con una única fila por lance válido y sin valores faltantes.

**Etapa 3. Entrenamiento:** implementación de algoritmos de clasificación supervisada, como Regresión Logística, KNN y Árbol de Decisión.

**Etapa 4. Evaluación:** comparación del rendimiento de los modelos mediante métricas de clasificación y matrices de confusión.

**Etapa 5. Interpretación:** análisis de los resultados para identificar posibles patrones espaciales asociados a la presencia registrada de bacalao y evaluar las limitaciones de los modelos.

## 7. Organización del proyecto

## 7. Organización del proyecto

El proyecto se organiza mediante la plantilla **Cookiecutter Data Science**, que permite separar los datos originales y procesados, los notebooks, el código fuente, los modelos y la documentación, facilitando la organización y reproducibilidad del trabajo.

```text
clasificacion-bacalao-atlantico/
│
├── data/
│   ├── raw/                           # Dataset original ICES-DATRAS
│   │   └── datras_ns_ibts_2020_2025_original.csv
│   └── processed/                     # Dataset preparado para clasificación
│       └── lances_bacalao_procesado.csv
│
├── models/                            # Modelos entrenados (pendiente)
│
├── notebooks/                         # Exploración y procesamiento
│   └── exploracion_datos_datras.ipynb
│
├── reports/
│   └── figures/                       # Gráficos e imágenes del proyecto
│
├── references/                        # Documentación complementaria
├── docs/                              # Documentación del proyecto
│
├── clasificacion_bacalao_atlantico/   # Código Python
│   ├── config.py
│   ├── dataset.py
│   ├── features.py
│   ├── plots.py
│   └── modeling/
│       ├── train.py
│       └── predict.py
│
├── README.md                          # Descripción y documentación
├── environment.yml                    # Entorno de trabajo
├── pyproject.toml                     # Configuración del proyecto
└── Makefile                           # Comandos auxiliares
```
## 8. Estado actual

El proyecto se encuentra en desarrollo.

Se completaron la adquisición, exploración y preparación inicial de los datos de ICES-DATRAS. Se dispone del dataset original, un conjunto procesado de 1.970 observaciones y un notebook de exploración denominado `exploracion_datos_datras.ipynb`.

Las próximas etapas comprenden la preparación de los conjuntos de entrenamiento y prueba, la implementación de los modelos de clasificación supervisada y la evaluación de su desempeño.

## 9. Alcances y limitaciones

Las predicciones se refieren a la presencia o ausencia registrada de bacalao en los lances de investigación pesquera analizados. La ausencia de capturas no implica necesariamente la ausencia real de la especie en una zona determinada.

Los resultados permitirán estudiar asociaciones entre las características geográficas y operativas de los lances y los registros de presencia, pero no estimar directamente la abundancia total de bacalao ni establecer relaciones causales.

La interpretación de los resultados estará limitada al área geográfica, período y características de los datos utilizados.
