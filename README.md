# IoT-Based Water-Quality Monitoring and XGBoost Classification of Physicochemical Suitability for Rainbow Trout Aquaculture in the Molino River, Peru

## Overview

The study combines a low-cost Internet of Things (IoT) monitoring prototype with an Extreme Gradient Boosting (XGBoost) classifier. Together they assess whether the physicochemical conditions of the Molino River (Amazonas, Peru) are suitable for rainbow trout (*Oncorhynchus mykiss*) aquaculture.

- **Study area:** Molino River, Amazonas, Peru
- **Monitoring sites:** 5 (P1–P5)
- **Monitoring period:** 428 days, from April 15, 2025 to June 17, 2026
- **Records:** 40,817, at about 15-minute intervals
- **Predictor variables:** water temperature, pH, and dissolved oxygen (DO)

## Repository contents

| File | Description |
|---|---|
| `Data.csv` | Water-quality records from the five monitoring sites (P1–P5) |
| `Data_Analysis.py` | Python code for descriptive analysis, suitability classification, and XGBoost modelling |
| `README.md` | This file |

## Dataset description (`Data.csv`)

The fields are separated by semicolons (`;`). The dataset has no missing values.

| Column | Description | Unit |
|---|---|---|
| `created_at` | Date and time of measurement (UTC−05:00) | ISO 8601 |
| `punto` | Monitoring site ID (P1–P5) | — |
| `entry_id` | Record ID assigned by the IoT platform | — |
| `Temperature` | Water temperature | °C |
| `pH` | pH | — |
| `Oxigeno Disuelto` | Dissolved oxygen | mg/L |

## Methods summary

### Suitability criteria

A record is classified as **suitable** (`APTO`) only when it meets all three thresholds. Otherwise it is classified as **unsuitable** (`NO APTO`).

| Variable | Threshold |
|---|---|
| Temperature | 10 – 17 °C |
| pH | 6.5 – 9.0 |
| Dissolved oxygen | > 5.5 mg/L |

### Model

- **Algorithm:** XGBoost (`XGBClassifier`; 100 boosting iterations, `max_depth = 5`, `learning_rate = 0.05`, `subsample = 0.8`, `colsample_bytree = 0.8`, `random_state = 42`)
- **Validation:** Leave-One-Group-Out cross-validation, with each monitoring site held out once as an independent test set
- **Metrics:** accuracy, precision, sensitivity (recall), specificity, F1-score, and ROC-AUC
- **Interpretation:** gain-based feature importance

## Reproducing the analysis

### Requirements

- Python 3.10 or later
- `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `xgboost`, `joblib`

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost joblib
```

### Running in Google Colab

`Data_Analysis.py` was exported from a Google Colab notebook. To run it in Colab, open the script, upload `Data.csv` when prompted, and run all cells. Outputs (tables, figures at 600 dpi, and the trained model) are saved to Google Drive.

### Running locally

Make these changes first:

1. Replace the file-upload cell with `df = pd.read_csv("Data.csv", sep=";")`.
2. Remove the `google.colab` imports and the `drive.mount(...)` calls.
3. Set `OUTPUT_DIR` and `ruta_salida` to a local folder, such as `"results"`.
4. Run the script in Jupyter, because it uses `display()`.



The dataset and code are released under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

## Contact

For questions about the data or the code, please open an issue in this repository or contact the corresponding author.
