# BTC / DXY / Oro / VIX

Proyecto Final — Data Science II  
Coderhouse | 2025–2026

---

## Descripción general

Proyecto de análisis financiero orientado al estudio del comportamiento de **Bitcoin (BTC)** en relación con:

- **DXY** (US Dollar Index),
- **Oro**,
- y **VIX** (Volatility Index).

El objetivo del proyecto es construir un pipeline analítico modular que permita:

- adquirir datos financieros históricos,
- validar consistencia temporal,
- construir features financieras,
- desarrollar análisis exploratorio,
- y aplicar modelos de Machine Learning sobre series temporales financieras.

---

## Objetivos del proyecto

- Construir datasets reproducibles mediante rangos temporales fijos.
- Analizar comportamiento y volatilidad de BTC.
- Evaluar relaciones entre BTC y variables macro-financieras.
- Aplicar Feature Engineering sobre series temporales.
- Desarrollar modelos baseline y modelos de Fase 2 con optimización de hiperparámetros y análisis SHAP.
- Incorporar buenas prácticas de trazabilidad y modularización sin sobreingeniería.

---

## Rango temporal oficial

| Inicio | Fin |
|---|---|
| 2023-05-06 | 2026-05-06 |

Frecuencia: diaria  
Duración aproximada: ~1096 días (~3 años)

---

## Estructura del proyecto

```text
DS2_BTC_DXY_ORO_VIX/

├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/              ← datasets completos incluidos para reproducibilidad
│
├── notebooks/
│   ├── exploracion/
│   │   └── btc_exploracion_inicial_coingecko.ipynb
│   │
│   ├── laboratorio/            ← solo local, no incluido en el repo
│   │
│   └── pipeline/
│       ├── 01_btc_adquisicion_rango_fijo.ipynb
│       ├── 02_dxy_adquisicion_rango_fijo.ipynb
│       ├── 03_gold_adquisicion_rango_fijo.ipynb
│       ├── 04_vix_adquisicion_rango_fijo.ipynb
│       ├── 05_integracion_multi_activo.ipynb
│       ├── 06_feature_engineering.ipynb
│       ├── 07_eda_financiero.ipynb
│       ├── 08_modelado_baseline.ipynb
│       └── 09_modelado_fase2.ipynb
│
├── reports/
│   └── DS2_Pipeline_Analitico_Proyecto_Final.docx
│
├── src/
│   └── utils.py
│
├── GUIA_ESTRUCTURADA.md
└── README.md
```

---

## Datasets incluidos en el repositorio

Todos los datasets generados por el pipeline están versionados en `data/processed/` como artefactos de entrega. Esto garantiza que el proyecto sea completamente reproducible sin depender de la disponibilidad de APIs externas en el momento de la ejecución.

| Archivo | Generado por | Descripción |
|---|---|---|
| `btc_processed.csv` | Notebook 01 | Serie histórica de BTC procesada |
| `dxy_processed.csv` | Notebook 02 | Serie histórica de DXY procesada |
| `gold_processed.csv` | Notebook 03 | Serie histórica de Oro procesada |
| `vix_processed.csv` | Notebook 04 | Serie histórica de VIX procesada |
| `dataset_integrado.csv` | Notebook 05 | Dataset multi-activo integrado (INNER JOIN) |
| `dataset_features_full.csv` | Notebook 06 | Dataset con todas las features Fase 1 |
| `dataset_modeling.csv` | Notebook 06 | Dataset listo para modelado Fase 1 |
| `dataset_phase2.csv` | Notebook 09 | Dataset con features expandidas Fase 2 |
| `model_comparison_phase1.csv` | Notebook 08 | Métricas comparativas Fase 1 |
| `model_comparison_final.csv` | Notebook 09 | Métricas comparativas Fase 1 + Fase 2 |

---

## Cómo reproducir este proyecto

### Requisitos

```bash
pip install pandas yfinance matplotlib scikit-learn xgboost shap
```

### Configuración de rutas

Cada notebook define `PROJECT_PATH` en su celda de configuración inicial. Actualizar esa variable con la ruta local antes de ejecutar.

### Opción A — Reproducción rápida (recomendada para evaluación)

Dado que todos los datasets están incluidos en el repositorio, es posible ejecutar cualquier notebook directamente sin necesidad de correr los anteriores.

Para evaluar el modelado completo, ejecutar directamente:

```
notebooks/pipeline/08_modelado_baseline.ipynb
notebooks/pipeline/09_modelado_fase2.ipynb
```

### Opción B — Pipeline completo desde cero

Para regenerar todos los datos desde las fuentes originales (Yahoo Finance vía `yfinance`), ejecutar en orden estricto:

```
01_btc_adquisicion_rango_fijo.ipynb
02_dxy_adquisicion_rango_fijo.ipynb
03_gold_adquisicion_rango_fijo.ipynb
04_vix_adquisicion_rango_fijo.ipynb
05_integracion_multi_activo.ipynb
06_feature_engineering.ipynb
07_eda_financiero.ipynb
08_modelado_baseline.ipynb
09_modelado_fase2.ipynb
```

Los notebooks 01–04 son independientes entre sí. A partir del 05, cada notebook depende del output del anterior.

---

## Estructura de notebooks

### Exploración

Notebooks utilizados durante la fase inicial de exploración conceptual y validación metodológica. No forman parte del pipeline operativo, pero se conservan como evidencia del proceso analítico.

### Pipeline

Notebooks modulares con responsabilidad clara, validaciones explícitas, logging estructurado y outputs exportados. Cada notebook puede ejecutarse de forma autónoma gracias a los datasets incluidos en el repositorio.

---

## Principios metodológicos

- Reproducibilidad garantizada mediante rangos temporales fijos y datasets versionados.
- Regla temporal estricta: `features ≤ t-1 | target = t+1`. Sin leakage.
- Modularización razonable: un notebook por responsabilidad, sin sobreingeniería.
- Validación explícita en cada etapa del pipeline.
- Resultados reportados con honestidad, sin inflación de métricas.

---

## Reglas de modularización

- Si el mismo código se repite 3 veces → pasa a función.
- Si una función se utiliza en 2 notebooks → pasa a `src/`.

---

## Estado actual

Completado:
- Pipeline completo de 9 notebooks ejecutado y versionado.
- Adquisición de BTC, DXY, Gold y VIX.
- Integración multi-activo (752 observaciones, INNER JOIN).
- EDA multivariado con correlaciones móviles y regímenes de mercado.
- Feature Engineering Fase 1 y Fase 2 sin leakage temporal.
- Modelado baseline (Fase 1): regresión lineal vs. baselines.
- Modelado Fase 2: Random Forest, XGBoost, optimización de hiperparámetros, SHAP.
- Evaluación de 4 hipótesis con cierre formal.
- Todos los datasets exportados y versionados.

---

Documentación técnica completa en `GUIA_ESTRUCTURADA.md` y `reports/DS2_Pipeline_Analitico_Proyecto_Final.docx`.
