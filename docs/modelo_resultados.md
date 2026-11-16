# Desarrollo del modelo y análisis de resultados

## 1. Modelos de clasificación evaluados

Para predecir los registros de bacalao del Atlántico se compararon tres algoritmos de clasificación supervisada:

- **Regresión logística:** estima la probabilidad de pertenecer a la clase positiva mediante una función logística.
- **K-Nearest Neighbors (KNN):** clasifica cada observación considerando las clases de sus vecinos más cercanos.
- **Árbol de decisión:** genera predicciones mediante reglas de decisión basadas en las variables predictoras.

Se utilizaron `Depth`, `ShootLat` y `ShootLong` como variables predictoras y `presencia_bacalao` como variable objetivo.

## 2. Entrenamiento y validación temporal

Se utilizó una división temporal para evaluar el desempeño frente a campañas posteriores:

| Conjunto | Período | Lances |
|---|---|---:|
| Entrenamiento y validación | 2020–2024 | 1.609 |
| Prueba independiente | 2025 | 361 |

Durante el entrenamiento se aplicaron cuatro divisiones de validación temporal.

La regresión logística se implementó mediante un `Pipeline` que integra `StandardScaler` y `LogisticRegression`, evitando que los datos de validación intervengan en el ajuste de las escalas.

## 3. Comparación y selección del modelo

Se compararon los algoritmos mediante métricas de clasificación, utilizando el F1-score como uno de los criterios principales.

Se seleccionó la regresión logística porque obtuvo buenos resultados al evaluar su capacidad de realizar predicciones con datos de distintos años. Además, permite comprender con mayor facilidad cómo las variables utilizadas influyen en las predicciones, lo que facilita la interpretación de los resultados.

Se realizó una búsqueda de hiperparámetros con `GridSearchCV`, evaluando los valores:

`C = [0.01, 0.1, 1, 10, 100]`

El mejor valor evaluado fue `C = 0.01`, con un F1-score promedio aproximado de 0,834. Sin embargo, se conservó `C = 1`, cuyo F1-score promedio fue aproximadamente 0,832.

La diferencia fue pequeña, es por eso que, se mantuvo la configuración seleccionada para el modelo final.

También se incorporó un modelo de referencia (*baseline*), cuyo F1-score fue aproximadamente 0,815. La mejora respecto del baseline fue moderada.

## 4. Evaluación independiente de 2025

La evaluación principal se realizó sobre 361 lances válidos de la campaña 2025, que no participaron del entrenamiento.

| Métrica | Resultado |
|---|---:|
| Accuracy | 75,9 % |
| Precision (clase 1) | 75,2 % |
| Recall (clase 1) | 95,0 % |
| F1-score (clase 1) | 83,9 % |

### Matriz de confusión

| Clase real | Predicción 0 | Predicción 1 |
|---|---:|---:|
| Sin registros de bacalao (0) | 47 | 75 |
| Con registros de bacalao (1) | 12 | 227 |

### Interpretación

El accuracy indica que el modelo clasificó correctamente aproximadamente tres de cada cuatro lances.

La precision muestra que alrededor del 75 % de las predicciones positivas correspondieron a lances con registros de bacalao.

El recall del 95 % constituye una de las principales fortalezas del modelo: logró identificar 227 de los 239 lances con registros.

Sin embargo, la matriz de confusión evidencia **75 falsos positivos**, es decir, lances sin registros que fueron clasificados como positivos.

El recall de la clase negativa fue aproximadamente del 39 %. Por lo tanto, el modelo presentó un desempeño considerablemente mejor para reconocer la clase positiva que para identificar los lances sin registros.

Este resultado demuestra por qué es necesario interpretar el F1-score junto con la matriz de confusión y las métricas por clase.

## 5. Evaluación adicional de 2026

Se utilizó el modelo entrenado para evaluar 324 lances válidos de 2026, sin realizar un nuevo entrenamiento.

| Métrica | 2025 | 2026 |
|---|---:|---:|
| Accuracy | 75,9 % | 73,8 % |
| Precision | 75,2 % | 75,2 % |
| Recall | 95,0 % | 91,3 % |
| F1-score | 83,9 % | 82,5 % |

### Matriz de confusión de 2026

| Clase real | Predicción 0 | Predicción 1 |
|---|---:|---:|
| Sin registros de bacalao (0) | 39 | 66 |
| Con registros de bacalao (1) | 19 | 200 |

### Interpretación comparativa

El desempeño fue relativamente similar entre las evaluaciones de 2025 y 2026, aunque disminuyeron el accuracy, el recall y el F1-score.

La precision se mantuvo aproximadamente en el 75 %, mientras que el recall permaneció por encima del 91 %.

Esto indica que el modelo conservó una alta sensibilidad para identificar lances con registros de bacalao.

No obstante, los falsos positivos continuaron siendo numerosos y persistió la dificultad para reconocer la clase negativa.

La evaluación de 2026 se considera **preliminar** y no demuestra por sí sola que el modelo sea adecuado para aplicaciones operativas.

## 6. Conclusiones y limitaciones

Los resultados indican que las variables geográficas y de profundidad contienen información útil para clasificar los registros de bacalao durante los lances científicos estudiados.

La principal fortaleza del modelo fue su alta sensibilidad para identificar lances con registros de la especie.

Sin embargo, su capacidad para reconocer lances sin registros fue limitada, lo que se refleja en los falsos positivos observados.

Las principales limitaciones son:

- Uso de solamente tres variables predictoras.
- Dificultad para identificar correctamente la clase negativa.
- Alcance restringido a las campañas NS-IBTS del primer trimestre.
- Necesidad de contar con información previa de profundidad para su aplicación antes del muestreo.

Como mejoras futuras, se propone incorporar variables ambientales, ampliar los períodos de estudio y explorar estrategias para reducir los falsos positivos.

El modelo constituye una **primera aproximación experimental** que podría contribuir al análisis y la planificación científica, pero no reemplaza la evaluación especializada ni permite afirmar la presencia o ausencia real de bacalao en una zona.

## 7. Documentación complementaria

El desarrollo completo, los gráficos, el entrenamiento y las evaluaciones se encuentran en la [notebook principal](../notebooks/modelo_final_bacalao.ipynb).

Las justificaciones técnicas se presentan en [Decisiones metodológicas](decisiones_metodologicas.md).

La descripción de las variables está disponible en el [Diccionario de datos](diccionario_datos.md).
