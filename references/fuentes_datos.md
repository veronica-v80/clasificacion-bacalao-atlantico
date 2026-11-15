# Fuentes de datos del proyecto



## 1. Fuente principal



Los datos utilizados en este proyecto provienen de **ICES-DATRAS**, una plataforma científica del Consejo Internacional para la Exploración del Mar (*International Council for the Exploration of the Sea*, ICES).



DATRAS reúne información de campañas internacionales de investigación pesquera y permite consultar las características de los lances y los registros biológicos de las especies identificadas durante los muestreos.



**Fuente oficial:** https://www.ices.dk/data/data-portals/Pages/DATRAS.aspx



## 2. Campaña y períodos analizados



Se utilizaron registros de la campaña *North Sea International Bottom Trawl Survey* (NS-IBTS), correspondientes al primer trimestre de los siguientes períodos:



- **2020–2025:** datos empleados para preparar, entrenar y evaluar el modelo principal.

- **2026:** datos utilizados para realizar una evaluación adicional y preliminar del modelo entrenado.



El área de estudio corresponde al mar del Norte y la especie analizada es el bacalao del Atlántico (*Gadus morhua*).



## 3. Archivos del repositorio



| Archivo | Descripción |

|---|---|

| `data/raw/datras_ns_ibts_2020_2025_original.csv` | Datos originales del período principal de estudio. |

| `data/raw/datras_ns_ibts_2026_original.csv` | Datos originales utilizados para la evaluación adicional. |

| `data/processed/lances_bacalao_procesado.csv` | Conjunto de datos preparado para entrenar y evaluar el modelo. |



## 4. Consideraciones sobre la interpretación



La variable objetivo identifica si durante un lance científico se registró bacalao del Atlántico.



La falta de registros de la especie en un lance no demuestra necesariamente su ausencia en la zona estudiada. Por este motivo, las predicciones deben interpretarse como estimaciones de la probabilidad de obtener registros durante el muestreo.



Los datos originales se atribuyen a ICES-DATRAS. El procesamiento y el análisis realizados se documentan en `notebooks/modelo_final_bacalao.ipynb`.

