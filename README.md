# Mobiles Dataset Analysis (2025)

> **Just want to see the results?** [Click here to preview the notebook](https://htmlpreview.github.io/?https://github.com/aaehub/mobile_analysis/blob/main/main.html) — no Python, no setup, no code to run.

A data analysis and machine learning project that explores mobile phone specifications and pricing across multiple global markets, culminating in a price prediction model.

## Project Summary

### What Was Done

**1. Dataset Exploration**
- Loaded and inspected `Mobiles Dataset (2025).csv` — 930 rows × 15 columns covering specs and launch prices across 5 countries (Pakistan, India, China, USA, Dubai)
- All spec columns were raw strings with units (e.g. `"174g"`, `"6GB"`, `"3,600mAh"`)
- All price columns were strings with currency symbols (e.g. `"USD 799"`, `"PKR 224,999"`)

**2. Data**

| File | Rows | Columns | Numeric Cols | Processing |
|---|---|---|---|---|
| `Mobiles Dataset (2025).csv` | 930 | 15 | 1 | None — raw original data |
| `normlized_transformed.csv` | 930 | 19 | 16 | Fully processed — units stripped, currencies converted, cameras split, `device_age` engineered |

**3. Transformations applied in `normlized_transformed.csv`**
- Units stripped from all spec columns (`g`, `GB`, `MP`, `mAh`, `inches` removed, values parsed as floats)
- All 5 price columns converted from local currency to USD float
- Multi-lens camera specs (e.g. `"50MP + 12MP"`) split into `_max`, `_sum`, and `_count` columns
- `device_age` feature engineered as `2026 - Launched Year`
- 1 null introduced: Samsung Galaxy Z Fold6 1TB has no Pakistan price

**4. Model — `main.ipynb`**
- Built a **Random Forest Regressor** to predict USA launch price (`Price USD_USA`)
- Used `normlized_transformed.csv` as input (no manual cleaning needed)
- Removed 1 extreme outlier (Nokia T21 at $39,622 — data entry error) using 3×IQR rule
- Features: brand, device age, weight, RAM, screen size, battery, front/back camera specs

**Model Results:**

| Metric | Value |
|---|---|
| MAE | ~$105 (avg prediction error) |
| RMSE | ~$156 |
| R² | ~0.86 (86% of price variance explained) |

**Top predictors:** RAM → Weight → Brand → Front Camera MP

**5. Dependencies fixed**
- Added `scikit-learn>=1.3.0` to `requirements.txt` (was missing, caused import error in Jupyter)

---

## Files

```
mobile_analysis/
├── Mobiles Dataset (2025).csv     # Raw mobile phone dataset (930 phones)
├── normlized_transformed.csv      # Fully processed, ML-ready dataset
├── main.ipynb                     # Price prediction notebook
├── main.html                      # Rendered notebook — preview at htmlpreview.github.io
├── requirements.txt               # Python dependencies
└── README.md
```

## Setup

**Requirements:** Python 3.8+

```bash
# Create and activate virtual environment
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # macOS/Linux

# Install dependencies
pip install -r requirements.txt
```

## Dependencies

```
pandas >= 2.0.0
numpy >= 1.24.0
scikit-learn >= 1.3.0
jupyter >= 1.0.0
ipykernel >= 6.0.0
```

## Usage

```bash
jupyter notebook
```

Open `main.ipynb` and run all cells. Edit the values in **Section 8** to predict the price of any phone.
