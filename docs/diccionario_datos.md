
# Diccionario de datos

## Proyecto: Clasificación de registros de bacalao del Atlántico

### 1. Descripción del conjunto de datos

El dataset lances_bacalao_procesado.csv fue elaborado a partir de información de ICES-DATRAS, correspondiente a la campaña NS-IBTS del primer trimestre de los años 2020–2025, en el mar del Norte.

Contiene **1.970 observaciones y 13 columnas**. Cada fila representa un lance válido de investigación pesquera.

El conjunto de datos incluye identificadores de los lances, características geográficas y operativas, y la variable objetivo utilizada para la clasificación supervisada.

**Fuente:** International Council for the Exploration of the Sea (ICES), base de datos DATRAS.

### 2. Descripción de las variables

| Variable | Tipo de dato | Descripción | Unidad / Valores | Función |
|---|---|---|---|---|
| Survey | Texto | Campaña de investigación pesquera | NS-IBTS | Identificador |
| Quarter | Texto | Trimestre en el que se realizó el muestreo | 1 | Identificador |
| Year | Texto | Año de realización del lance | 2020–2025 | Identificador temporal |
| Country | Texto | País responsable del muestreo | Código de país | Identificador |
| Ship | Texto | Código del buque de investigación | Código de buque | Identificador |
| Gear | Texto | Código del arte de pesca utilizado | Código de arte | Identificador |
| HaulNo | Texto | Número identificador del lance | Código / número | Identificador |
| StNo | Texto | Identificador de la estación de muestreo | Código de estación | Identificador |
| Depth | Entero | Profundidad registrada durante el lance | Metros (m) | Predictora |
| ShootLat | Decimal | Latitud geográfica del inicio del lance | Grados decimales | Predictora |
| ShootLong | Decimal | Longitud geográfica del inicio del lance | Grados decimales | Predictora |
| HaulDur | Entero | Duración efectiva del lance | Minutos | Análisis exploratorio |
| presencia_bacalao | Entero binario | Indica si el lance presenta registros válidos de bacalao del Atlántico | 0 / 1 | Variable objetivo |

### 3. Variable objetivo

La variable presencia_bacalao se construyó relacionando los registros de cabecera de lance (HH) con los registros biológicos (HL) de la especie *Gadus morhua*.

- **Clase 1:** lance válido con al menos un registro biológico válido de bacalao del Atlántico.
- **Clase 0:** lance válido sin registros de bacalao, con información biológica disponible y controles de registro verificados.

La clase 0 representa la falta de detección documentada durante el muestreo y no demuestra la ausencia real de la especie en la zona.

**Distribución de clases:**

| Clase | Cantidad | Porcentaje |
|---|---:|---:|
| 0 – Sin registro | 635 | 32,2 % |
| 1 – Con registro | 1.335 | 67,8 % |
| **Total** | **1.970** | **100 %** |

### 4. Variables utilizadas en el modelo

El modelo final de regresión logística utiliza únicamente:

- Depth: profundidad.
- ShootLat: latitud.
- ShootLong: longitud.

La variable HaulDur se conserva para el análisis exploratorio, pero se excluye del entrenamiento porque su valor efectivo no se conoce antes de realizar el lance.

Para utilizar el modelo en una etapa previa al muestreo, será necesario disponer de estimaciones de profundidad obtenidas independientemente y comparables con los datos de entrenamiento.

### 5. Criterios de preparación y calidad

Para construir el dataset procesado:

1. Se seleccionaron los lances válidos (HaulVal = V).
2. Se verificó la correspondencia entre los registros HH y HL.
3. Se comprobaron los códigos de registro de especies (StdSpecRecCode = 1 y BySpecRecCode = 1).
4. Se identificaron los registros válidos de *Gadus morhua* (SpecVal = 1).
5. Se construyó la variable objetivo binaria.
6. Se convirtieron las variables numéricas y se verificó la ausencia de valores faltantes.
7. Se comprobó que cada lance estuviera representado una sola vez.

### 6. Uso del conjunto de datos

Para el desarrollo del modelo se realizó una separación temporal:

- **2020–2024:** entrenamiento y validación temporal.
- **2025:** evaluación principal del modelo.

Adicionalmente, se utilizaron datos preliminares de la campaña de 2026 para evaluar el comportamiento del modelo frente a una campaña posterior. Estos datos se prepararon por separado y no forman parte del archivo lances_bacalao_procesado.csv.

### 7. Fuente y documentación

**Fuente de datos:** ICES-DATRAS, North Sea International Bottom Trawl Survey (NS-IBTS), primer trimestre.

Sitio oficial: https://datras.ices.dk/

**Archivo original:** data/raw/datras_ns_ibts_2020_2025_original.csv

**Archivo procesado:** data/processed/lances_bacalao_procesado.csv

**Notebook principal:** notebooks/modelo_final_bacalao.ipynb
