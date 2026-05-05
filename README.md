# 🏙️ Egypt Housing Analysis

**Data Decoders Team**  
Marwan Kandil · Hana Hashish · Badr Ouda

---

## Overview

A data science project analysing the Cairo residential property market. Using a scraped listings dataset (`properties.csv`), the project walks through the full pipeline — from raw data cleaning to statistical hypothesis testing and multiple linear regression — to uncover what drives property prices across the city.

---

## Table of Contents

1. [Project Structure](#project-structure)
2. [Dependencies](#dependencies)
3. [Dataset](#dataset)
4. [Methodology](#methodology)
5. [Key Findings](#key-findings)
6. [How to Run](#how-to-run)
7. [Team](#team)

---

## Project Structure

```
.
├── Final_PythonNotebook_Data_Decoders.ipynb   # Main analysis notebook
├── properties.csv                              # Raw listings dataset
└── README.md
```

---

## Dependencies

Install all required packages with:

```bash
pip install numpy pandas matplotlib seaborn scipy statsmodels scikit-learn
```

| Package | Purpose |
|---|---|
| `numpy` | Numerical operations & log transforms |
| `pandas` | Data loading, cleaning, and manipulation |
| `matplotlib` | Base plotting |
| `seaborn` | Statistical visualisations |
| `scipy` | ANOVA and linear regression tests |
| `statsmodels` | OLS regression with full summary output |
| `scikit-learn` | Feature standardisation (`StandardScaler`) |

---

## Dataset

The raw dataset (`properties.csv`) contains Cairo property listings with the following fields:

- `price` — listing price in EGP (some entries marked `"Ask"`)
- `size_sqm` — property size in square metres
- `bedroom` / `bathroom` — room counts
- `type` — property type (apartment, villa, duplex, etc.)
- `location` — compound, district, and city concatenated

**Cleaning steps applied:**
- Removed `"Ask"` price entries
- Converted price, size, bedrooms, and bathrooms to numeric types
- Dropped rows with nulls in key columns
- Removed duplicates
- Applied IQR-based outlier removal on both `size_sqm` and `price`
- Parsed `location` into separate `compound`, `district`, and `city` columns

---

## Methodology

### 1. Feature Engineering
- `price_per_sqm` = price ÷ size_sqm
- `total_rooms` = bedrooms + bathrooms

### 2. Exploratory Data Analysis (EDA)
- Price distribution (raw and log-transformed)
- Listing counts by property type
- Average price by type and district
- Scatter plot of size vs. price coloured by type
- Correlation heatmap across numeric features
- Price per sqm by bedroom count

### 3. Hypothesis Testing
**Test 1 — Property type & location vs. price/sqm**
- H₀: Location and property type have no significant effect on price per sqm
- Method: One-way ANOVA across type groups and district groups separately
- Result: Both p-values < 0.05 → **H₀ rejected**

**Test 2 — Property size vs. price**
- H₀: No linear relationship between size (sqm) and price (slope = 0)
- Method: Simple linear regression via `scipy.stats.linregress`
- Result: p-value < 0.05 → **H₀ rejected**

### 4. Multiple Linear Regression
Two OLS models predicting **log(price)** from `size_sqm`, `bedroom`, `bathroom`, and `total_rooms`:

- **Unscaled model** — for interpretable coefficients
- **Scaled model** — features standardised for relative importance comparison

Diagnostics include residuals vs. fitted plot and a Q-Q plot.

---

## Key Findings

- **Property type and district are both statistically significant** predictors of price per sqm (ANOVA, p < 0.05).
- **Property size significantly predicts price** via a linear relationship.
- Premium districts in Cairo command measurably higher per-sqm rates.
- The scaled regression model reveals relative feature importance — larger size and more bathrooms are the strongest positive predictors in the log-price model.

---

## How to Run

1. Clone the repository and ensure `properties.csv` is in the project root.
2. Install dependencies (see above).
3. Open the notebook:

```bash
jupyter notebook Final_PythonNotebook_Data_Decoders.ipynb
```

4. Run all cells from top to bottom (`Kernel → Restart & Run All`).

---

## Team

| Name | Student ID |
|---|---|
| Marwan Kandil | 24-101070 |
| Hana Hashish | 24-101336 |
| Badr Ouda | 24-101338 |
