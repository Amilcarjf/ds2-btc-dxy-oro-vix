# GUÍA ESTRUCTURADA — DS2 BTC / DXY / Oro / VIX

> Documentación técnica completa del proyecto.  
> Audiencia: el propio desarrollador, revisores técnicos, y cualquier persona que clone el repositorio.

---

## Índice

1. [Descripción del proyecto](#1-descripción-del-proyecto)
2. [Estado actual](#2-estado-actual)
3. [Arquitectura del pipeline](#3-arquitectura-del-pipeline)
4. [Estructura de notebooks](#4-estructura-de-notebooks)
5. [Estructura de carpetas](#5-estructura-de-carpetas)
6. [Decisiones clave de diseño](#6-decisiones-clave-de-diseño)
7. [Convenciones del proyecto](#7-convenciones-del-proyecto)
8. [Cómo ejecutar el pipeline](#8-cómo-ejecutar-el-pipeline)
9. [Hipótesis de trabajo](#9-hipótesis-de-trabajo)
10. [Documentación de referencia](#10-documentación-de-referencia)

---

## 1. Descripción del proyecto

### Qué es

Pipeline analítico modular orientado al estudio del comportamiento de **Bitcoin (BTC)** en relación con variables macro-financieras: el índice del dólar estadounidense (**DXY**), el precio del **Oro**, y el índice de volatilidad del mercado (**VIX**).

El proyecto construye un dataset reproducible, aplica feature engineering sobre series temporales financieras, y desarrolla modelos de Machine Learning para predecir el retorno diario de BTC.

### Problema analítico

> ¿Cómo modelar el retorno diario de Bitcoin usando información histórica propia y variables externas vinculadas al dólar, al oro y al miedo del mercado?

### Objetivo general

Modelar el retorno diario de Bitcoin a partir de variables históricas y macro-financieras, bajo un enfoque reproducible, cuantitativo y sin leakage temporal.

### Declaración metodológica

Este proyecto no genera señales de trading ni recomendaciones de inversión. Su finalidad es exclusivamente descriptiva, explicativa y predictiva en términos estadísticos.

### Stack tecnológico

| Herramienta | Rol |
|---|---|
| Python | Lenguaje principal del pipeline |
| pandas | Manipulación y transformación de DataFrames |
| yfinance | Adquisición de datos financieros históricos |
| matplotlib | Visualización exploratoria |
| scikit-learn | Modelado y evaluación |
| XGBoost | Modelo de gradient boosting |
| Git / GitHub | Control de versiones y portfolio |

---

## 2. Estado actual

| Fase | Notebook | Estado |
|---|---|---|
| Exploración inicial BTC (CoinGecko) | `btc_exploracion_inicial_coingecko.ipynb` | ✅ Completo |
| Adquisición BTC rango fijo | `01_btc_adquisicion_rango_fijo.ipynb` | ✅ Completo |
| Adquisición DXY rango fijo | `02_dxy_adquisicion_rango_fijo.ipynb` | ✅ Completo |
| Adquisición Gold | `03_gold_adquisicion_rango_fijo.ipynb` | ✅ Completo |
| Adquisición VIX | `04_vix_adquisicion_rango_fijo.ipynb` | ✅ Completo |
| Integración multi-activo | `05_integracion_multi_activo.ipynb` | ✅ Completo |
| Feature Engineering | `06_feature_engineering.ipynb` | ⏳ Pendiente |
| EDA financiero multi-activo | `07_eda_financiero.ipynb` | ⏳ Pendiente |
| Modelado baseline | `08_modelado_baseline.ipynb` | ⏳ Pendiente |
| Modelado Fase 2 | `09_modelado_fase2.ipynb` | ⏳ Pendiente |

---

## 3. Arquitectura del pipeline

El pipeline tiene **9 notebooks operativos** organizados en dos fases. La numeración es fija y no varía.

```
01 — Adquisición BTC
02 — Adquisición DXY
03 — Adquisición Gold
04 — Adquisición VIX
        │
        ▼
05 — Integración multi-activo
        │
        ▼
06 — Feature Engineering
        │
        ▼
07 — EDA financiero
        │
        ▼
08 — Modelado baseline       ← Fase 1
09 — Modelado Fase 2         ← Fase 2
```

### Rango temporal oficial

| Parámetro | Valor |
|---|---|
| Inicio | 2023-05-06 |
| Fin | 2026-05-06 |
| Frecuencia | Diaria |
| Duración aprox. | ~1096 días (~3 años) |

### Fuentes de datos

| Activo | Ticker | Fuente | Forma de uso |
|---|---|---|---|
| BTC | BTC-USD | Yahoo Finance (`yfinance`) | Precio de cierre → retorno diario |
| DXY | DX-Y.NYB | Yahoo Finance (`yfinance`) | Nivel → retorno diario |
| Oro | GC=F | Yahoo Finance (`yfinance`) | Precio de cierre → retorno diario |
| VIX | ^VIX | Yahoo Finance (`yfinance`) | Nivel (no retorno) |

### Variable objetivo (target)

```
Target = retorno_BTC(t+1)
```

Retorno porcentual diario de Bitcoin. El modelo estima el retorno del día siguiente.

### Regla temporal crítica

```
Features ≤ t-1  |  Target = t+1
```

Ninguna feature puede usar información del día actual ni de días futuros respecto del target. Esta regla se aplica en todos los notebooks del pipeline y se valida explícitamente en el notebook de Feature Engineering.

---

## 4. Estructura de notebooks

### Exploración

Notebooks utilizados durante la fase inicial de exploración conceptual y validación metodológica. No forman parte del pipeline operativo. Se conservan como evidencia del proceso analítico.

| Notebook | Descripción |
|---|---|
| `btc_exploracion_inicial_coingecko.ipynb` | Exploración BTC con CoinGecko y ventana dinámica `days=365`. Documenta la fase exploratoria previa al pipeline definitivo. |
| `btc_exploracion_inicial_coingecko_RAW.ipynb` | Versión de trabajo sin edición del notebook exploratorio. |

### Pipeline

Notebooks modulares del pipeline operativo. Cada notebook tiene responsabilidad única, logging estructurado, validaciones explícitas y export a `data/processed/`.

| Notebook | Responsabilidad | Output |
|---|---|---|
| `01_btc_adquisicion_rango_fijo.ipynb` | Adquirir, limpiar, validar y construir features iniciales de BTC | `btc_processed.csv` |
| `02_dxy_adquisicion_rango_fijo.ipynb` | Adquirir, limpiar, validar y construir features iniciales de DXY | `dxy_processed.csv` |
| `03_gold_adquisicion_rango_fijo.ipynb` | Adquirir, limpiar, validar y construir features iniciales de Gold | `gold_processed.csv` |
| `04_vix_adquisicion_rango_fijo.ipynb` | Adquirir, limpiar, validar y construir features iniciales de VIX | `vix_processed.csv` |
| `05_integracion_multi_activo.ipynb` | Unificar los cuatro datasets en un DataFrame único alineado temporalmente | `dataset_integrado.csv` |
| `06_feature_engineering.ipynb` | Construir features de Fase 1 y Fase 2 con notación temporal correcta | `dataset_features.csv` |
| `07_eda_financiero.ipynb` | EDA analítico multi-activo, correlaciones, regímenes, hipótesis descriptivas | — |
| `08_modelado_baseline.ipynb` | Baseline naive y media histórica. Modelado lineal base. Evaluación Fase 1 | — |
| `09_modelado_fase2.ipynb` | Features extendidas, modelos avanzados, validación temporal, feature importance | — |

### Estructura interna de cada notebook de adquisición

Todos los notebooks 01–04 siguen la misma estructura canónica:

```
0. Header y contexto metodológico
1. Adquisición desde Yahoo Finance
2. Limpieza estructural (MultiIndex → columnas planas)
3. Validación estructural (shape, rango, orden, duplicados, gaps)
4. Construcción de features iniciales (return, volatility_7d, ma_7d)
5. Validación de NaN en features construidas
6. EDA de validación (precio, retornos, volatilidad, MA)
7. Export a data/processed/
```

---

## 5. Estructura de carpetas

```
DS2_BTC_DXY_ORO_VIX/
│
├── data/
│   ├── raw/                    ← Datos originales sin transformación (en .gitignore)
│   ├── interim/                ← Datasets intermedios si se requieren
│   └── processed/              ← Datasets procesados por activo y dataset integrado (en .gitignore)
│
├── notebooks/
│   ├── exploracion/            ← Notebooks de exploración conceptual (no pipeline)
│   ├── laboratorio/            ← RAW, pruebas y experimentos (en .gitignore, solo local)
│   │   ├── btc_exploracion_inicial_coingecko.ipynb
│   │   └── btc_exploracion_inicial_coingecko_RAW.ipynb
│   │
│   └── pipeline/               ← Notebooks operativos del pipeline
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
│   └── DS2_Proyecto_Final_Diseño_Definitivo.docx
│
├── src/
│   └── utils.py                ← Funciones reutilizables (se activa si una función aparece en 2+ notebooks)
│
├── GUIA_ESTRUCTURADA.md        ← Este documento
├── README.md                   ← Descripción pública del proyecto
└── .gitignore
```

### Qué no está en el repositorio

Por diseño, no se suben a GitHub:

- `data/raw/` — datos originales descargados
- `data/processed/` — datasets generados por el pipeline, reproducibles
- `data/interim/` — estados intermedios
- `notebooks/laboratorio/` — notebooks RAW y de experimentación, solo locales

Los datasets se regeneran ejecutando el pipeline desde el notebook 01.

---

## 6. Decisiones clave de diseño

### Rangos fijos en lugar de `days=365`

El parámetro `days=365` define una ventana flotante relativa a la fecha de ejecución. Esto hace que el dataset cambie entre ejecuciones distintas, rompiendo la reproducibilidad.

Se adoptó rango fijo `2023-05-06 → 2026-05-06` para garantizar que el dataset sea idéntico en cualquier ejecución, en cualquier entorno y en cualquier momento.

### Yahoo Finance como fuente principal (no CoinGecko)

Durante la adquisición inicial se intentó usar la API pública de CoinGecko con rangos fijos. La API devolvió error 401 al consultar datos históricos fuera del último año. Se migró a Yahoo Finance mediante `yfinance`, que cubre el rango completo sin restricciones de acceso.

CoinGecko queda documentada como fuente utilizada en la fase exploratoria inicial.

### VIX como nivel, no como retorno

El VIX se incorpora al modelo como nivel (`VIX(t-1)`) y no como retorno porcentual. Su valor interpretativo es como medida de aversión al riesgo: `VIX=30` significa algo específico en el contexto financiero. Su variación porcentual no tiene la misma carga semántica.

### EDA de validación vs. EDA analítico

Los notebooks 01–04 contienen EDA de **validación**: confirmar que el dato llegó correctamente y tiene coherencia visual. El notebook 07 contiene EDA **analítico**: comparación multi-activo, correlaciones, regímenes, hipótesis descriptivas. Son etapas distintas con propósitos distintos.

### Drawdown como variable de análisis, no feature del modelo base

El drawdown es una medida acumulativa y estructural que puede introducir problemas de interpretación dinámica en el modelo base. Se usa como variable de análisis en el EDA pero no como feature predictiva en los modelos de Fase 1.

### Modularización artesanal, no técnica

La modularización de este proyecto es por **notebooks separados con responsabilidad clara**, no por scripts `.py`. El archivo `src/utils.py` existe en la estructura pero solo se activa si una función se repite en dos o más notebooks. No se modulariza por anticipación.

### Doble baseline

Todo modelo se compara contra dos baselines antes de evaluar su valor real:

| Baseline | Fórmula |
|---|---|
| Naive lag-1 | `retorno_predicho(t+1) = retorno_BTC(t-1)` |
| Media histórica | `retorno_predicho(t+1) = media(retornos históricos train)` |

Si el modelo no supera al baseline, ese resultado se documenta honestamente. Un modelo que apenas supera al baseline sobre retornos de BTC es un resultado profesionalmente válido dado el nivel de ruido del activo.

---

## 7. Convenciones del proyecto

### Nombres de variables financieras

```
return          — retorno porcentual diario (pct_change)
volatility_7d   — desviación estándar rolling 7 días sobre return
ma_7d           — media móvil 7 días sobre Close
```

### Notación temporal

```
(t-1)   — dato del día anterior al de predicción
(t+1)   — target: dato del día siguiente
```

Toda feature usa subscript `(t-1)` o anterior. El target es siempre `(t+1)`.

### Nombres de archivos procesados

```
[activo]_processed.csv

Ejemplos:
btc_processed.csv
dxy_processed.csv
gold_processed.csv
vix_processed.csv
dataset_integrado.csv
dataset_features.csv
```

### Estructura de `PROJECT_PATH`

Todos los notebooks definen la ruta base del proyecto como:

```python
PROJECT_PATH = r"C:\DS2_BTC_DXY_ORO_VIX"
```

La raíz de todos los proyectos vive directamente en `C:\`. Y construyen rutas mediante `os.path.join()`, nunca con concatenación de strings ni rutas hardcodeadas en las funciones de export.

### Logging estructurado

Cada notebook usa el siguiente patrón de validación:

```python
print("=== VALIDACIÓN ESTRUCTURAL [ACTIVO] ===")
print(f"Shape del dataset: {df.shape}")
print(f"Rango temporal: {df.index.min().date()} → {df.index.max().date()}")
print(f"Orden cronológico correcto: {df.index.is_monotonic_increasing}")
print(f"Duplicados temporales: {df.index.duplicated().sum()}")
print(f"Gaps temporales detectados: {(df.index.to_series().diff().dropna() != pd.Timedelta(days=1)).sum()}")
```

Y para NaN:

```python
print("=== VALIDACIÓN DE NaN ===")
print(f"NaN en return: {df['return'].isna().sum()}")
print(f"NaN en volatility_7d: {df['volatility_7d'].isna().sum()}")
print(f"NaN en ma_7d: {df['ma_7d'].isna().sum()}")
```

### Gaps temporales en activos tradicionales

BTC opera 24/7. DXY, Oro y VIX pertenecen a mercados financieros tradicionales y no operan fines de semana ni feriados. Los gaps detectados en estos activos son comportamiento esperado del calendario financiero, no errores de datos.

### Commits de Git

```
init:      estructura base y configuración inicial
feat:      nueva funcionalidad o notebook completado
fix:       corrección de un error
refactor:  reorganización sin cambio de comportamiento
docs:      cambios en documentación
```

Ejemplos:
```
init: estructura base del proyecto
feat: 01_btc_adquisicion_rango_fijo — adquisición y validación completa
feat: 02_dxy_adquisicion_rango_fijo — adquisición y validación completa
docs: actualizar GUIA_ESTRUCTURADA con estado actual
```

---

## 8. Cómo ejecutar el pipeline

### Requisitos

```bash
pip install pandas yfinance matplotlib scikit-learn xgboost
```

### Configuración de rutas

Cada notebook define `PROJECT_PATH` en la celda de configuración inicial. Actualizar esta variable con la ruta local antes de ejecutar.

### Orden de ejecución

```
01 → 02 → 03 → 04 → 05 → 06 → 07 → 08 → 09
```

Los notebooks 01–04 son independientes entre sí y pueden ejecutarse en cualquier orden. A partir del 05, cada notebook depende del output del anterior.

### Outputs esperados en `data/processed/`

```
btc_processed.csv        ← generado por 01
dxy_processed.csv        ← generado por 02
gold_processed.csv        ← generado por 03
vix_processed.csv        ← generado por 04
dataset_integrado.csv    ← generado por 05
dataset_features.csv     ← generado por 06
```

---

## 9. Hipótesis de trabajo

| ID | Hipótesis |
|---|---|
| H-1 | Existe una relación inversa entre la fortaleza del dólar (DXY) y el retorno de Bitcoin. |
| H-2 | La relación entre Bitcoin y el dólar varía según el nivel de volatilidad del mercado, mostrando mayor intensidad en períodos de alta volatilidad. |
| H-3 | Bitcoin reacciona con mayor sensibilidad que el oro ante cambios en las condiciones de mercado vinculadas al dólar y al riesgo global. |
| H-4 | La incorporación de variables macro-financieras (DXY, Oro y VIX) mejora la capacidad predictiva del modelo respecto de un enfoque basado únicamente en el historial de BTC. |

Las hipótesis se validan en los notebooks 07, 08 y 09. El cierre formal ocurre en el notebook 09, que conecta explícitamente los resultados del modelo con cada hipótesis.

---

## 10. Documentación de referencia

| Documento | Ubicación | Descripción |
|---|---|---|
| Diseño técnico definitivo | `reports/DS2_Proyecto_Final_Diseño_Definitivo.docx` | Diseño completo del proyecto: target, features, hipótesis, pipeline, fuentes, baselines |
| README | `README.md` | Descripción pública del proyecto, estructura y estado actual |
| GUÍA ESTRUCTURADA | `GUIA_ESTRUCTURADA.md` | Este documento |

---

*DS2 BTC / DXY / Oro / VIX — Coderhouse 2025–2026*
