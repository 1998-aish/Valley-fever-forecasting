# Valley-fever-forecasting

# Valley Fever Outbreak Forecasting — FSE 570 Capstone

**Team Debaters:** Aishwarya Srivastava · Fasbeer Mohammad Haque · Harshavardhan Balraj Kagithoju · Sultan Mohiuddin Shaik

## Project Overview

Valley Fever (coccidioidomycosis) incidence in Arizona has nearly doubled over the past two decades, with more than 14,000 cases reported in 2024. This project frames Valley Fever forecasting as a time-series problem, fusing three heterogeneous public datasets into a tiered modeling pipeline to predict county-level outbreak risk ahead of time.

## Data Sources

| Dataset | Source | Relevance |
|---|---|---|
| ADHS Case Reports | [azdhs.gov](https://azdhs.gov) | Target variable |
| EPA PM10 Air Quality | [EPA AQS](https://aqs.epa.gov/aqsweb/airdata/) | Dust dispersal "blow" mechanism |
| AZMet + PRISM Weather | [azmet.arizona.edu](https://azmet.arizona.edu/past-data), [prism.oregonstate.edu](https://prism.oregonstate.edu) | Rainfall/soil temperature "grow" step |

## Methodology — Tiered Approach

1. **Tier 1 (Baseline):** Seasonal-naive, SARIMA, and Poisson/Negative Binomial regression
2. **Tier 2 (Machine Learning):** XGBoost/LightGBM on lagged environmental features
3. **Tier 3 (Stretch):** Spatio-Temporal Graph Convolutional Network (STGCN) with SHAP interpretability

All models validated using time-series cross-validation (walk-forward, expanding-window).

## Repository Structure

```
├── docs/              # Proposal, report, and presentation slides
├── data/
│   ├── raw/           # Original, unmodified downloaded datasets
│   └── processed/     # Cleaned, merged county-month tables
├── notebooks/         # Exploratory analysis and model development
└── scripts/           # Reusable data pipeline and modeling code
```

## Timeline

| Weeks | Phase |
|---|---|
| 1–4 | Data collection & EDA |
| 5–8 | Tier 1–2 model development (Status Report #1) |
| 9–11 | Stretch goal + full validation (Status Report #2) |
| 12–15 | Final reporting and presentation |

## Setup

```bash
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows
pip install -r requirements.txt
```

## Team Contacts

- Aishwarya Srivastava — asriv144@asu.edu
- Fasbeer Haque — fmhaque@asu.edu
- Harshavardhan Balraj Kagithoju — hkagitho@asu.edu
- Sultan Mohiuddin Shaik — sshaik69@asu.edu
