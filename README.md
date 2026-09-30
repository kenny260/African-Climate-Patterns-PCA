# Formative Assignment: Principal Component Analysis (PCA)

---

## Overview

This repository contains a from-scratch implementation of Principal Component Analysis (PCA) applied to Rwanda rainfall data sourced from the Humanitarian Data Exchange (HDX) HAPI platform. The goal is to reduce the dimensionality of a 7-feature dataset while retaining as much variance as possible, using only `numpy` and `matplotlib` — no `sklearn` or other ML libraries.

---

## Repository Structure

```
.
├── PCA_Formative_2_Peer_Pair_16.ipynb  
├── hdx_hapi_rainfall_rwa.csv             
├── task_sheet.pdf                        
└── README.md                             
```

---

## Dataset

**Source:** [HDX HAPI — Rainfall, Rwanda](https://data.humdata.org/)  
**File:** `hdx_hapi_rainfall_rwa.csv`

**Why this dataset?** Rwanda's economy is heavily agriculture-dependent, and rainfall patterns directly affect food security, crop yield, and water resource management. This data captures both *economic activity* signals (seasonal rainfall driving agricultural cycles) and *population pressure* dynamics (water availability across administrative regions).

**Data quality issues handled:**
- 119 missing values in `rainfall` and `aggregation_period` — rows dropped after encoding
- Several admin columns (`admin1_code`, `admin2_code`, etc.) are entirely NaN — excluded
- Non-numeric columns (`aggregation_period`, `version`, `location_code`) — label-encoded

---

## Features Used (7 columns)

| Feature | Type | Notes |
|---|---|---|
| `rainfall` | Numeric | Observed rainfall (mm) |
| `rainfall_long_term_average` | Numeric | Historical baseline (mm) |
| `rainfall_anomaly_pct` | Numeric | % deviation from baseline |
| `number_pixels` | Numeric | Spatial coverage proxy |
| `provider_admin1_code` | Numeric | Sub-region identifier (900110–900114) |
| `aggregation_period_enc` | Encoded | dekad=0, 1-month=1, 3-month=2 |
| `version_enc` | Encoded | final=0, forecast=1, preliminary=2 |

---

## PCA Pipeline

The notebook implements PCA in the following steps — **all using numpy only**:

1. **Load & encode** — raw CSV loaded with `numpy.genfromtxt`; non-numeric columns label-encoded via dictionary mapping; NaN rows dropped
2. **Standardize** — each feature transformed to mean=0, std=1 using $z = \frac{x - \mu}{\sigma}$
3. **Covariance matrix** — computed as $\Sigma = \frac{X^T X}{n-1}$
4. **Eigendecomposition** — `numpy.linalg.eigh` applied to the symmetric covariance matrix
5. **Sort components** — eigenvalues and eigenvectors sorted in descending order
6. **Select components** — 4 PCs selected (cumulative variance ≥ 85%)
7. **Project data** — original 7D data projected onto 4D principal component space
8. **Visualize** — scatter plots before/after PCA and a scree plot

---

## Results

| PC | Explained Variance | Cumulative |
|---|---|---|
| PC1 | 38.9% | 38.9% |
| PC2 | 18.7% | 57.6% |
| PC3 | 15.6% | 73.2% |
| PC4 | 13.2% | **86.4%** ✓ |
| PC5 | 9.9% | 96.3% |
| PC6 | 3.4% | 99.7% |
| PC7 | 0.3% | 100% |

**4 principal components** were selected, reducing the dataset from 7 dimensions to 4 while retaining **86.4%** of total variance.

---

## How to Run

1. Clone this repository
2. Open `PCA_Formative_Rwanda_Rainfall.ipynb` in [Google Colab](https://colab.research.google.com/) or Jupyter
3. Upload `hdx_hapi_rainfall_rwa.csv` to the same working directory (or the Colab session storage)
4. Run all cells in order — outputs are displayed inline

**Requirements:** Python 3.8+, `numpy`, `matplotlib` (both available by default in Colab)

---

## Libraries Used

| Library | Purpose |
|---|---|
| `numpy` | All PCA computation (standardization, covariance, eigendecomposition, projection) |
| `matplotlib` | Visualizations (before/after scatter plots, scree plot) |

No `sklearn`, `scipy`, `pandas`, or other ML/data libraries were used for any PCA step.
