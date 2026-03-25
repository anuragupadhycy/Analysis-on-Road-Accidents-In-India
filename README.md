# 📊 ECOTRIX — Road Accidents in India: Econometric Analysis

**Authors:** Jai Ram Suri · Anurag Upadhyay · Raghav Vaidya · Shreyansh Goel · Jatin Sharma

---

## 📌 Research Question

> *What is the impact of **vehicle density** and **road length** on the number of road accidents in India?*

India reports over 4,61,000 road accidents annually — 53 accidents and 19 deaths every hour. This project models the structural relationship between road infrastructure, vehicle density, and accident counts using time-series data from **1990–2009**.

---

## 📁 Project Structure

```
ecotrix_project/
│
├── analysis.py          # Full econometric analysis (all models + tests)
├── visualizations.py    # All charts and plots
├── plots/               # Generated figures (auto-created on run)
├── requirements.txt     # Python dependencies
└── README.md
```

---

## 🔬 Methodology & Models

The analysis progresses through the following steps:

### 1. Full OLS Model (Model 1)
```
Road_Accidents = β0 + β1·Road_Length + β2·Density + β3·Reg_Vehicles + ε
```
- **R² = 0.971** — very high explanatory power
- Problem: **multicollinearity** detected via VIF

### 2. Multicollinearity Detection (VIF)
| Variable | VIF | Status |
|---|---|---|
| Road Length | 4.563 | ✅ OK |
| Vehicle Density | 4.563 | ✅ OK |
| Registered Vehicles | 134.565 | ❌ Dropped |

`Registered_Vehicles` is dropped due to extreme collinearity with Density.

### 3. Reduced OLS + Ramsey RESET Test (Model 2)
```
Road_Accidents = β0 + β1·Road_Length + β2·Density + ε
```
- Ramsey RESET: **F = 4.41, p = 0.031** → Specification error → model rejected

### 4. Log-Log (Double Log) Model (Model 3)
```
ln(Accidents) = β0 + β1·ln(Road_Length) + β2·ln(Density) + ε
```
- **R² = 0.955**
- Coefficients are now **elasticities**
- Problem: **Positive autocorrelation** (Durbin-Watson = 0.955, BG p = 0.033)

### 5. Prais-Winsten GLS — Final Model (Model 4)
```
ln(Accidents) = 6.17 + 0.256·ln(Density) + 0.399·ln(Road_Length)
```
| Variable | Coefficient | Interpretation |
|---|---|---|
| ln(Density) | **0.256** | 1% ↑ density → 0.256% ↑ accidents |
| ln(Road_Length) | **0.399** | 1% ↑ road length → 0.399% ↑ accidents |

- **R² = 0.968** — 96.8% variation explained
- Durbin-Watson = 1.318 → autocorrelation resolved ✅
- Jarque-Bera normality: p = 0.104 → residuals normal ✅

---

## 📦 Setup & Installation

### Prerequisites
```bash
pip install numpy pandas matplotlib scipy
```

### Run the Analysis
```bash
# Full econometric analysis (all models + hypothesis tests)
python analysis.py

# Generate all charts
python visualizations.py
```

---

## 📈 Output Plots

| File | Description |
|---|---|
| `plots/01_raw_trends.png` | Raw data trends (1990–2009) |
| `plots/02_log_transforms.png` | Log-transformed variables |
| `plots/03_residuals_ols.png` | OLS residuals + ACF |
| `plots/04_gls_fit.png` | GLS actual vs predicted |
| `plots/05_gls_normality.png` | Residual histogram + Q-Q plot |
| `plots/06_vif.png` | VIF bar chart |

---

## 📚 Data Sources

| Variable | Source |
|---|---|
| Road Accidents | Ministry of Road Transport & Highways (MoRTH), India |
| Road Length | MoRTH Annual Reports |
| Vehicle Density | MoRTH / National Transport Statistics |

**Time period:** 1990–2009 (T = 20 observations)

---

## 🧪 Tests Performed

| Test | Purpose | Result |
|---|---|---|
| VIF | Multicollinearity | Dropped Reg_Vehicles (VIF=134) |
| Ramsey RESET | Specification error | Rejected linear form |
| Durbin-Watson | Autocorrelation | Positive AC detected (DW=0.955) |
| Breusch-Godfrey | Autocorrelation (general) | AC confirmed (p=0.033) |
| Prais-Winsten | AC remedy | DW improved to 1.318 |
| Jarque-Bera | Normality of residuals | Normal (p=0.104) |

---

## 📖 Key Findings

1. **Vehicle density** and **road length** are both statistically significant predictors of road accidents
2. **Road length has a larger elasticity (0.399)** than density (0.256) — infrastructure expansion poses greater accident risk than density alone
3. The model explains **96.8%** of variation in road accidents
4. The log-log specification confirms that the relationship is **multiplicative**, not linear

---

*Econometrics Project — Department of Economics*
