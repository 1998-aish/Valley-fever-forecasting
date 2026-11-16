# Valley Fever Outbreak Forecasting — FSE 570 Capstone

**Team Debaters:** Aishwarya Srivastava · Fasbeer Mohammad Haque · Harshavardhan Balraj Kagithoju · Sultan Mohiuddin Shaik

## Project Overview

Valley Fever (coccidioidomycosis) incidence in Arizona has nearly doubled over the past two decades, with more than 14,000 cases reported in 2024. This project forecasts **next-month reported Valley Fever cases for Maricopa, Pinal and Pima counties** by fusing four public datasets into one county-month table, then comparing simple baselines against count-data and tree-based models.

The research question is whether adding dust and weather measurements improves forecasts beyond case history alone.

## Status at a Glance

| Stage | What was delivered |
|---|---|
| Proposal | Problem, data plan, tiered methodology, timeline (slides in `docs/proposal/`) |
| Checkpoint 1 | Four sources integrated into 396 county-month records, data-quality checks, first Maricopa forecast (Ridge vs. seasonal baseline) |
| Checkpoint 2 / Status Update 2 | Three-county Tier 1 + Tier 2 modeling, 2024 held-out test, sensitivity checks, SHAP, reporting-delay simulation, weather ablation, lag tests, quad chart |

## Data

| Dataset | Source | Role |
|---|---|---|
| ADHS case reports (event-date, monthly) | [azdhs.gov](https://azdhs.gov) | Target variable |
| EPA PM10 air quality | [EPA AQS](https://aqs.epa.gov/aqsweb/airdata/) | Dust proxy (not a spore measurement) |
| AZMet station weather and soil | [azmet.arizona.edu](https://azmet.arizona.edu/past-data) | Soil temperature, humidity, wind, VPD |
| PRISM climate | [prism.oregonstate.edu](https://prism.oregonstate.edu) | Rainfall and temperature (county-centroid samples) |

- **396 county-month records** (3 counties × 132 months, Jan 2015 – Dec 2025).
- **357 forecast-ready pairs** (119 per county): one month's inputs paired with the next month's case count, so no future information is used.
- Cleaning included correcting a mislabeled AZMet station, masking invalid `999` sensor placeholders (these had pushed one month's Maricopa dew-point average to 41 °C), and resolving duplicate station-day readings. The audit trail is in the QC sheets of `data/processed/by_county/Maricopa_2015_2025_QC_Corrected_Model.xlsx`.
- ADHS case counts were never edited.

## Methodology: Plan vs. What Was Done

| Proposal plan | What was actually run |
|---|---|
| Tier 1: seasonal-naive, SARIMA, Poisson / Negative Binomial | Seasonal baseline (same month last year), Ridge, HistGradientBoosting. SARIMA was not run. |
| Tier 2: XGBoost / LightGBM on lagged features | Poisson, Negative Binomial (NB2) and XGBoost (small / medium / regularized), chosen by nested chronological validation on 2020–2023 only. LightGBM was not used. |
| Tier 3: spatio-temporal graph network (STGCN) | Not built. ADHS data is county-level, and a ZIP-level graph needs ZIP-level case labels. |

Validation is chronological (walk-forward, never a random split). Selected models were frozen before being scored on 2024. Pinal and Pima use one XGBoost model trained on all three counties with county indicators; Maricopa uses a county-only NB2.

## Verified Results

Mean absolute error (MAE, cases per month; lower is better), 2024 retrospective test:

| County | Selected model | Seasonal baseline | Selected model | Change |
|---|---|---|---|---|
| Maricopa | NB2 | 240.17 | 132.14 | −45% |
| Pinal | XGBoost (medium, pooled) | 40.83 | 28.43 | −30% |
| Pima | XGBoost (medium, pooled) | 32.33 | 29.80 | −8% |

**April–December 2024 only** (excluding the January–March 2024 testing concern):

| County | Seasonal baseline | Selected model | Change |
|---|---|---|---|
| Maricopa | 135.00 | 91.29 | −32% |
| Pinal | 23.78 | 20.22 | −15% |
| Pima | 14.78 | 17.15 | **+16% (worse)** |

2025 results are provisional (ADHS counts may still be revised): Maricopa 273.00 → 134.87, Pinal 36.75 → 13.72, Pima 35.33 → 17.96.

Source files: `results/checkpoint2/` (all numbers come from the verified rerun in `notebooks/checkpoint2/VF_Checkpoint2_Tier1_Tier2_One_Colab.ipynb`).

## Key Findings and Limitations

- **Improvement is uneven.** Strong in Maricopa, moderate in Pinal, small in Pima.
- **Simpler comparisons sometimes win.** On full-year 2024, Pinal's Tier 1 boosting (24.27) and a plain "same as last month" guess (24.50) both beat the selected XGBoost (28.43); Pima's Tier 1 Ridge (27.54) beat its selected model (29.80). Beating the seasonal baseline alone does not make a model best.
- **Weather and dust help inconsistently.** In the ablation study they improved April–December 2024 for all three counties, but hurt all three in January–March 2024, and results for the full year and for 2025 are mixed. Same-month rainfall was the strongest single signal in the independent lag test, but also inconsistent.
- **Case history dominates.** In SHAP, case-history features rank first in every county, followed by relative humidity (PM10 also appears in Maricopa's top five). The Maricopa SHAP values come from an exploratory XGBoost challenger, not the selected NB2 model.
- **Reporting delay matters.** In the simulation, case counts that are one month stale raise Maricopa's 2024 MAE from 132 to about 148 (about 191 with a two-month delay). Real ADHS publication dates have not yet been checked.
- **January–March 2024** coincides with a documented ADHS lab-testing concern. Counts were left unchanged, and results are reported with and without those months.
- 2024 was examined in earlier experiments, so it is not a completely untouched test year.
- PM10 is a dust proxy, PRISM values are county-centroid samples, and 2025 cases are provisional.
- These are retrospective research results, **not an operational warning system** and not evidence of causation.

## Next Tasks (proposed owners, Status Update 2)

| Task | Proposed owner |
|---|---|
| Check PM10 / rainfall lag timing and observation dates | Harshavardhan |
| Audit ADHS release timing and Jan–Mar 2024 counts | Aishwarya |
| Re-test case-history baselines and later-year scoring | Sultan |
| Review NB2 / XGBoost interpretation and rerun consistency | Fasbeer |

## Repository Structure

```
├── docs/
│   ├── proposal/              # Proposal slides
│   ├── checkpoint1/           # Checkpoint 1 slides, presenter guide, Excel + Colab explainer
│   ├── status_update2/        # Quad chart (PDF + editable PPTX), 5- and 9-minute scripts, run guide
│   │   └── earlier_version/   # First Checkpoint 2 quad chart, script and guide (superseded)
│   └── background/            # Early research summary
├── data/
│   ├── raw/                   # Original ADHS case workbook (see data/raw/README.md)
│   └── processed/             # Three-county input tables used by the notebooks
│       └── by_county/         # Per-county workbooks with QC sheets
├── notebooks/
│   ├── checkpoint1/           # Maricopa demo and three-county demo notebooks
│   └── checkpoint2/           # Main Tier 1 + Tier 2 run, weather ablation, lag tests
├── results/
│   ├── checkpoint1/           # Checkpoint 1 evidence and sensitivity files
│   ├── checkpoint2/           # Verified 2024 metrics, sensitivity, full results workbook, chart
│   ├── ablation/              # Weather vs. case-history-only comparison
│   └── lag_tests/             # Rainfall / PM10 lag investigation
└── dashboard/                 # Interactive county map and results explorer (open in a browser)
```

## Reproducing the Main Results

1. Open `notebooks/checkpoint2/VF_Checkpoint2_Tier1_Tier2_One_Colab.ipynb` in Google Colab.
2. Choose Runtime → Run all, and upload `data/processed/VF_Checkpoint2_Input_Three_Counties.xlsx` when asked.
3. Compare the new `2024_MAIN` and `2024_SENSITIVITY` sheets with `results/checkpoint2/`.

XGBoost scores shifted between earlier saved runs and the verified rerun because package versions were not pinned. Keep each run's results with its settings and do not mix them. The versions used in the verified rerun are in `requirements.txt`.

## Local Setup

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
