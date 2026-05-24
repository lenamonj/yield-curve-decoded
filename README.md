<p align="center">
  <img src="https://img.shields.io/badge/Python-3.8%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/PCA-Unsupervised%20ML-7c3aed?style=for-the-badge" alt="PCA">
  <img src="https://img.shields.io/badge/XGBoost-Classifier-EC6C37?style=for-the-badge&logo=xgboost&logoColor=white" alt="XGBoost">
  <img src="https://img.shields.io/badge/Data-FRED%20API-0A6E2D?style=for-the-badge" alt="FRED">
  <img src="https://img.shields.io/badge/45%20Years-Daily%20Data-0EA5E9?style=for-the-badge" alt="45 Years">
  <img src="https://img.shields.io/badge/SHAP-Explainability-4B0082?style=for-the-badge" alt="SHAP">
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="MIT License">
</p>

<h1 align="center">The Curve Knows</h1>

<p align="center">
  <strong>45 years of daily US Treasury yield curve data. 11,000+ trading days. 7 maturities. Decomposed into three latent factors using PCA - then fed into a recession prediction model that flags downturns before they hit.</strong>
</p>

<p align="center">
  <em>The yield curve has inverted before every US recession since 1981. This notebook proves it, quantifies it, and gives you a live probability reading.</em>
</p>

---

## Results

| Metric | Value |
|--------|-------|
| **PCA: Variance Explained (3 factors)** | **100.0%** - three factors capture the entire curve |
| **Recession Model ROC-AUC** | **0.72** on a fully out-of-sample test (2011-2026) |
| **Current 12-Month Recession Probability** | **31.6%** (CAUTIOUS) |

Three unsupervised factors - level, slope, and curvature - explain virtually all variation in the US Treasury curve across 45 years of daily data. The slope factor (PC2) inverts before every single recession in the dataset. The XGBoost model, trained on 4 recessions (1981-2009) and tested on a period it has never seen (2011-2026), produces a calibrated recession probability that updates live every time you run the notebook.

No Bloomberg terminal. No paid data. Just the FRED API and 11,000 days of history.

---

## What This Actually Does

Most people know "the yield curve inverts before recessions." This notebook goes far beyond that talking point:

1. **Pulls 45 years of daily Treasury yields** (3M through 30Y) directly from the FRED API - 11,180 trading days of data back to 1981
2. **Decomposes the curve using PCA** into the three classical factors that fixed income professionals use daily: level (parallel shifts), slope (steepening/flattening), and curvature (butterfly). These three factors explain 100% of curve variation.
3. **Visualizes every recession** with spread time series, factor trajectories, and a year-by-maturity yield heatmap spanning four decades
4. **Builds a recession classifier** using PCA factors + engineered spread features (2s10s, 3m10y, butterfly, rate of change, lagged slope) - trained on the 1981-82, 1990-91, 2001, and 2007-09 recessions
5. **Outputs a live signal** - today's curve projected onto the PCA factors with historical percentile ranks and a current 12-month recession probability

| Section | What It Covers |
|---------|---------------|
| **Data** | 7 Treasury maturities (3M-30Y) from FRED, daily since 1981 |
| **EDA** | Curve visualization, 2s10s/3m10y spreads with recession shading, yield surface heatmap, distributional analysis |
| **PCA** | Level/slope/curvature decomposition. Scree plot, loadings, factor time series, perturbation analysis ("what does each factor do to the curve?"), 3-factor reconstruction quality |
| **Recession Model** | XGBoost classifier predicting 12-month-ahead recessions from 14 curve shape features. Hyperparameter tuned, SHAP explained. |
| **Current Signal** | Live curve, PCA factor percentile ranks, recession probability as of today |

---

## Quick Start

1. Get a free [FRED API key](https://fred.stlouisfed.org/docs/api/api_key.html) (30 seconds)

2. Clone and install:
```bash
git clone https://github.com/lenamonj/the-curve-knows.git
cd the-curve-knows
pip install -r requirements.txt
export FRED_API_KEY="your_key_here"
```

3. Run:
```bash
jupyter notebook yield_curve_decoded.ipynb
```

Every time you run it, it pulls the latest curve from FRED and gives you an updated recession probability.

---

## Design Decisions

- **PCA before prediction.** Decomposing the curve into interpretable factors before feeding into XGBoost gives the model structured features that match how fixed income professionals actually think about the curve. Raw yields would work, but the factors are more interpretable and more stable.
- **12-month forward target.** Predicting "recession within the next year" is the useful question. Predicting the current month's NBER status is useless - NBER announces recessions 6-12 months after they start.
- **No external data.** The curve alone contains the signal. GDP, payrolls, and other macro data would improve accuracy, but would obscure the point: the yield curve is a sufficient statistic for recession risk. The bond market prices in what the economy is about to do.
- **Temporal split.** Train on 1981-2010 (4 recessions), test on 2011-2026 (1 recession). No random shuffling. The model must predict forward, not backward.
- **Live signal.** Not a historical backtest. The notebook gives you a number you can act on today.

---

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
  <sub>Built with Python, the FRED API, and 45 years of Treasury data. No proprietary tools required.</sub>
</p>
