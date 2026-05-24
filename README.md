<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/PCA-Unsupervised-7c3aed?style=for-the-badge" alt="PCA">
  <img src="https://img.shields.io/badge/XGBoost-Classifier-EC6C37?style=for-the-badge&logo=xgboost&logoColor=white" alt="XGBoost">
  <img src="https://img.shields.io/badge/Data-FRED%20API-0A6E2D?style=for-the-badge" alt="FRED">
  <img src="https://img.shields.io/badge/SHAP-Explainability-4B0082?style=for-the-badge" alt="SHAP">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="MIT License">
</p>

<h1 align="center">Yield Curve Decoded</h1>

<p align="center">
  <strong>PCA decomposition of the US Treasury yield curve and recession prediction from curve shape features. Pulls live data from FRED, decomposes the curve into level/slope/curvature factors, and outputs a current 12-month recession probability.</strong>
</p>

<p align="center">
  <em>The curve has been right before every recession since 1976. This notebook shows you how to read it.</em>
</p>

---

## Results

| Metric | Value |
|--------|-------|
| **PCA: Variance Explained (3 factors)** | 100.0% |
| **Recession Model ROC-AUC** | 0.72 (test set, 2011-2026) |
| **Recession Model PR-AUC** | 0.11 (vs 0.07 baseline) |
| **Current 12-Month Recession Probability** | 31.6% (CAUTIOUS) |

Three PCA factors capture virtually all yield curve variation across 45 years. The slope factor (PC2) visibly inverts before every recession in the dataset. The XGBoost recession model, trained on 4 recessions (1981-2009) and tested out-of-sample on 2011-2026, produces a calibrated probability that updates every time the notebook runs.

---

## What It Does

| Section | What It Covers |
|---------|---------------|
| **Data** | 8 Treasury maturities (1M-30Y) from FRED, daily since 1976 |
| **EDA** | Curve visualization, 2s10s/3m10y spreads with recession shading, 3D surface, yield heatmaps |
| **PCA** | Decompose the curve into level, slope, and curvature factors. Scree plot, loadings, factor time series, perturbation analysis, reconstruction quality |
| **Recession Model** | XGBoost classifier predicting 12-month-ahead recessions from PCA factors + spread features. Compared against simple 2s10s inversion rule |
| **Current Signal** | Today's curve projected onto PCA factors with percentile ranks and live recession probability |

## Quick Start

1. Get a free [FRED API key](https://fred.stlouisfed.org/docs/api/api_key.html)

2. Clone and install:
```bash
git clone https://github.com/lenamonj/yield-curve-decoded.git
cd yield-curve-decoded
pip install -r requirements.txt
export FRED_API_KEY="your_key_here"
```

3. Run:
```bash
jupyter notebook yield_curve_decoded.ipynb
```

## Design Decisions

- **PCA before prediction.** Decomposing the curve into interpretable factors (level/slope/curvature) before feeding into XGBoost gives the model structured features that match how fixed income professionals think about the curve.
- **12-month forward target.** Predicting "recession within the next year" is more useful than predicting the current month's NBER status, which is published with a lag anyway.
- **No external data.** The curve alone contains the signal. Adding GDP, employment, or other macro data would improve accuracy but obscure the point: the yield curve is a sufficient statistic for recession risk.
- **Temporal split.** Train on 1976-2010 (multiple recessions), test on 2011-present. No random shuffling.
- **Live signal.** The notebook pulls today's curve from FRED and outputs a current recession probability every time it runs.

## Project Structure

```
.
├── yield_curve_decoded.ipynb    # Full pipeline - one notebook
├── requirements.txt             # Python dependencies
├── LICENSE                      # MIT License
└── README.md                    # You are here
```

## License

This project is licensed under the [MIT License](LICENSE).

---

<p align="center">
  <sub>Built with Python, FRED API, and 50 years of Treasury data.</sub>
</p>
