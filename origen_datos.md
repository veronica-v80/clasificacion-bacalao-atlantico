# Origen y adquisición de los datos

## 1. Fuente de información

Los datos utilizados en este proyecto provienen de **ICES-DATRAS (Database of Trawl Surveys)**, una plataforma científica del Consejo Internacional para la Exploración del Mar (*International Council for the Exploration of the Sea, ICES*).

Se seleccionaron registros de la campaña **North Sea International Bottom Trawl Survey (NS-IBTS)**, correspondientes al primer trimestre de los años **2020 a 2025**, desarrollada en el mar del Norte.

La especie estudiada es el bacalao del Atlántico (*Gadus morhua*).

**Fecha de adquisición del conjunto principal:** 12 de octubre de 2026.

**Fuente oficial:** [ICES-DATRAS](https://www.ices.dk/data/data-portals/Pages/DATRAS.aspx).

## 2. Características de los datos originales

El archivo original de ICES-DATRAS contiene dos tipos de registros:

| Tipo | Cantidad | Descripción |
|---|---:|---|
| HH (*Haul Header*) | 2.034 | Información general de los lances científicos, como ubicación, profundidad y duración |
| HL (*Haul Length*) | 134.934 | Información biológica de las especies registradas durante los muestreos |

Ambos tipos de registros se analizaron por separado y posteriormente se relacionaron mediante los identificadores de los lances.

## 3. Selección y preparación de los datos

Para construir el dataset de clasificación supervisada se realizaron las siguientes operaciones:

1. Identificación y separación de los registros HH y HL.
2. Selección de los lances válidos mediante el criterio `HaulVal = V`.
3. Identificación de registros biológicos correspondientes a *Gadus morhua*.
4. Construcción de la variable objetivo binaria `presencia_bacalao`.
5. Integración de la información, conservando una observación por lance.
6. Comprobación de duplicados, valores faltantes y tipos de datos.

El conjunto procesado contiene **1.970 lances válidos y 13 columnas**.

Se identificaron 1.335 lances con registros válidos de bacalao (clase 1) y 635 lances sin registros válidos de la especie (clase 0).

La clase 0 representa una **no detección documentada durante el lance**, no una afirmación de ausencia real del bacalao en la zona estudiada.

Los criterios de construcción de esta variable se desarrollan en la documentación de decisiones metodológicas y en la notebook principal.

## 4. Datos adicionales de 2026

Se incorporó un archivo adicional de la campaña NS-IBTS de 2026 para realizar una evaluación preliminar de la capacidad de generalización temporal del modelo.

Este conjunto permitió evaluar **324 lances válidos**, sin volver a entrenar el clasificador.

Los datos de 2026 no se utilizaron para seleccionar el modelo ni ajustar sus hiperparámetros.

## 5. Ubicación de los archivos

Los archivos están organizados dentro del repositorio:

- `data/raw/datras_ns_ibts_2020_2025_original.csv`: datos originales del período principal.
- `data/raw/datras_ns_ibts_2026_original.csv`: datos adicionales para evaluación.
- `data/processed/lances_bacalao_procesado.csv`: dataset procesado utilizado para el desarrollo del modelo.

Debido a su tamaño, GitHub no permite mostrar una vista previa completa de los archivos originales, aunque se encuentran disponibles para su descarga. El dataset procesado puede visualizarse directamente en el repositorio.

La descripción detallada de las columnas, sus tipos de datos y características se encuentra en el [Diccionario de datos](diccionario_datos.md).
