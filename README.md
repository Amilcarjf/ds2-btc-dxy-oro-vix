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
- Desarrollar modelos baseline y modelos posteriores de predicción/análisis.
- Incorporar buenas prácticas de trazabilidad y modularización sin sobreingeniería.

---

## Rango temporal oficial

| Inicio | Fin |
|---|---|
| 2023-05-06 | 2026-05-06 |

Frecuencia:
- diaria

Duración aproximada:
- ~1096 días (~3 años)

---

## Estructura del proyecto

```text
DS2_BTC_DXY_ORO_VIX/

├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
│
├── notebooks/
│   ├── exploracion/
│   │   └── btc_exploracion_inicial_coingecko.ipynb
│   │
│   ├── laboratorio/              ← solo local, no incluido en el repo
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
│   └── DS2_Proyecto_Final_Diseño_Definitivo.docx
│
├── src/
│   └── utils.py
│
├── GUIA_ESTRUCTURADA.md
└── README.md
```

---

## Estructura de datos

### `data/raw/`
Datos originales descargados desde APIs o fuentes externas sin transformación.

### `data/interim/`
Datasets intermedios utilizados durante integración o procesamiento parcial.

### `data/processed/`
Datasets finales limpios y validados listos para integración y modelado.

---

## Estructura de notebooks

### Exploración
Notebooks utilizados durante la fase inicial de exploración conceptual y validación metodológica.

No forman parte directa del pipeline modular definitivo, pero se conservan como evidencia del proceso analítico y evolución metodológica del proyecto.

### Pipeline

Notebooks modulares responsables de:

- adquisición,
- limpieza,
- validación,
- integración,
- feature engineering,
- EDA,
- y modelado.

Cada notebook mantiene responsabilidad clara, trazabilidad y outputs reutilizables.

---

## Principios metodológicos

El proyecto prioriza:

- reproducibilidad,
- modularización razonable,
- validación explícita,
- trazabilidad,
- y claridad narrativa.

No se busca construir una arquitectura enterprise ni un sistema de trading en tiempo real.

---

## Reglas de modularización

- Si el mismo código se repite 3 veces → pasa a función.
- Si una función se utiliza en 2 notebooks → pasa a `src/`.

---

## Estado actual

Completado:
- BTC adquisición definitiva completada.
- DXY adquisición definitiva completada.
- Gold adquisición definitiva completada.
- VIX adquisición definitiva completada.
- Validaciones estructurales implementadas en todos los activos.
- Features iniciales construidas.
- Export modular funcionando.
- Arquitectura base estabilizada.

Siguientes etapas:
- Integración multi-activo.
- Feature Engineering.
- EDA financiero.
- Modelado.
