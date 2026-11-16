# Decisiones metodológicas del proyecto

## 1. Enfoque del problema

El proyecto aborda un problema de **clasificación supervisada binaria**, cuyo objetivo es estimar la probabilidad de obtener registros de bacalao del Atlántico (*Gadus morhua*) durante un lance científico.

Cada observación representa un lance válido de la campaña NS-IBTS. La predicción se plantea para la etapa previa al muestreo, utilizando información que podría estar disponible durante la planificación.

## 2. Construcción de la variable objetivo

Se construyó la variable binaria `presencia_bacalao`:

- **Clase 1:** lance con al menos un registro biológico válido de bacalao.
- **Clase 0:** lance sin registros válidos de bacalao.

Se seleccionaron únicamente los lances considerados válidos por ICES-DATRAS (`HaulVal = V`).

Para fundamentar la clase 0, se comprobó que los 635 lances correspondientes contienen registros biológicos de otras especies y presentan los códigos `StdSpecRecCode = 1` y `BySpecRecCode = 1`.

Según el vocabulario oficial de ICES, estos códigos corresponden al registro de todas las especies estándar y a una lista abierta de especies de captura incidental, respectivamente.

Por ello, la clase 0 se interpreta como una **no detección documentada durante el lance**, bajo los criterios de registro utilizados. Esta clasificación no demuestra la ausencia real de bacalao en la zona.

## 3. Selección de variables predictoras

Para el modelo final se seleccionaron:

- `Depth`: profundidad del lance.
- `ShootLat`: latitud.
- `ShootLong`: longitud.

Estas variables permiten estudiar la relación entre la ubicación geográfica, la profundidad y los registros de bacalao.

La duración efectiva del lance (`HaulDur`) se analizó durante el EDA, pero se excluyó del entrenamiento porque no se conoce antes de realizar el muestreo.

Para una aplicación predictiva previa al lance, también sería necesario disponer de una estimación anticipada de profundidad comparable con la registrada en los datos históricos.

## 4. Tratamiento de datos y codificación

Durante el preprocesamiento se verificaron los tipos de datos, la correspondencia entre los registros HH y HL, los valores faltantes y los duplicados.

El dataset procesado contiene 1.970 observaciones y 13 columnas, con una única fila por lance válido.

Las variables numéricas seleccionadas no presentan valores faltantes, por lo que no fue necesario aplicar imputación.

Como las variables predictoras finales son numéricas, no fue necesario utilizar técnicas de codificación de variables categóricas. Los identificadores se conservaron para documentar los lances, pero no se incorporaron al modelo.

La variable objetivo se codificó mediante los valores binarios 0 y 1.

## 5. Distribución de clases y métricas

El dataset contiene:

- 1.335 lances con registros de bacalao (67,8 %).
- 635 lances sin registros de bacalao (32,2 %).

Se identificó un desequilibrio moderado entre las clases.

Por este motivo, la evaluación no se basó únicamente en el accuracy. Se utilizaron también precision, recall, F1-score y matrices de confusión para analizar el rendimiento por clase.

Se incorporó un modelo de referencia (*baseline*) para comparar el desempeño de los clasificadores.

Se conservó la distribución original de las clases, sin aplicar técnicas de balanceo. Como mejora futura, podría evaluarse su aplicación durante el entrenamiento.

## 6. División temporal y prevención de fugas de información

Se utilizó una división temporal para evaluar la capacidad del modelo de generalizar a campañas posteriores:

- **Entrenamiento y validación:** 2020–2024, con 1.609 lances.
- **Prueba independiente:** 2025, con 361 lances.

Durante el entrenamiento se utilizaron cuatro divisiones de validación cruzada temporal, respetando el orden cronológico de las campañas.

La campaña de 2025 permaneció excluida de la selección de hiperparámetros y del ajuste del modelo.

Para la regresión logística se utilizó un `Pipeline` que incorpora `StandardScaler` y el clasificador. Esto permite calcular los parámetros de estandarización exclusivamente con los datos de entrenamiento de cada división, evitando fugas de información durante la validación.

## 7. Comparación y selección del modelo

Se compararon tres algoritmos de clasificación supervisada:

- Regresión logística.
- K-Nearest Neighbors (KNN).
- Árbol de decisión.

Se seleccionó la regresión logística por su rendimiento en validación temporal y su facilidad de interpretación.

Se evaluaron distintos valores del hiperparámetro de regularización `C` mediante `GridSearchCV`, utilizando el F1-score como criterio principal.

Aunque el mejor valor evaluado fue `C = 0.01`, se conservó `C = 1` por presentar un rendimiento muy cercano.

## 8. Evaluación adicional y limitaciones

El modelo seleccionado se evaluó también con 324 lances válidos de la campaña 2026, sin volver a entrenarlo.

Esta evaluación permitió explorar su comportamiento ante datos posteriores, pero se considera preliminar.

Las principales limitaciones identificadas son:

- Mayor dificultad para clasificar correctamente los lances sin registros de bacalao.
- Cantidad relevante de falsos positivos.
- Utilización de solamente tres variables predictoras.
- Alcance restringido a las campañas y períodos analizados.
- Necesidad de contar con información previa sobre la profundidad para realizar predicciones antes del lance.

Por estas razones, el modelo constituye una aproximación experimental de apoyo al análisis científico y no una herramienta operativa validada.

## 9. Documentación complementaria

El proceso de preparación, entrenamiento y evaluación está desarrollado en la notebook principal:

[Notebook del modelo final](../notebooks/modelo_final_bacalao.ipynb).

Para consultar las características de las variables, ver el [Diccionario de datos](diccionario_datos.md).

Los códigos utilizados para fundamentar la variable objetivo pueden consultarse en el [vocabulario oficial de ICES](https://vocab.ices.dk/).
