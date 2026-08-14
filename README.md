# Team Rho Capstone Project
### Predicting State-Level Inpatient Admissions with Google Trends

**Team:** Casey Brookshier and Brendan Collari

**Repo:** https://github.com/brendancollari/Team-Rho-Capstone-Project


## Research Question

Which suicide and crisisrelated Google search terms provide the most accurate leading indicators of state-level inpatient psychiatric admissions one year later?

Mental health care in the U.S. is strained and care is shifting toward outpatient models due to workforce shortages and psychiatric bed deficits. When the system falls short, the burden often fall  onto correctional facilities and emergency rooms rather than just disappearing. If reliable online search patterns can be identified early, policymakers and healthcare organizations could respond to demand before strain occurs rather than reacting to it after.

**Hypothesis:** An increase in Google searches related to suicide and crisis intervention will predict higher state-level inpatient psychiatric admissions one year later.

**Prediction:** A focused set of suicide- and crisis-related Google search terms (e.g., suicidal thoughts, suicide hotline, self-harm) will improve one year prediction of state-level inpatient psychiatric admissions compared with a baseline model that excludes Google Trends data.

**Stakeholders:** State/municipal healthcare organizations, public health researchers, and workforce planners who could benefit from a leading indicator that isn't going to lag like  traditional utilization data.

---

## Data Sources

| Dataset | Source | Role |
|---|---|---|
| Google Trends | trends.google.com via Pytrends API | Leading indicator variable so the state-level search interest for crisis/suicide-related terms |
| HCUP Fast Stats | AHRQ HCUP | Outcome avriable which is state-level Mental Health/Substance Use inpatient admissions (quarterly, summed to annual) |
| Population Estimates Program (PEP) | U.S. Census Bureau | Annual state population, will be used to compute per-capita admission rate |

**Coverage Timeframe:** 2013–2023 

**Target variable:**
```
Inpatient Admission Rate = (Mental Health/Substance Use inpatient admissions) / (state population) × 100,000
```

Raw Google Trends data has been size-reduced (not substantially cleaned) to stay under GitHub's 100MB file limit; codebooks documenting what was retained/removed are included alongside the reduced files in `data/raw/`.

---

## Repo Structure

```

Team-Rho-Capstone-Project/
├── data/
│   ├── raw/          # Source data, the Google Trends, the HCUP, and the Census Bureau)
│   └── processed/     # Merged/aggregated state-year dataset
├── notebooks/ # Data collection/merge notebook + EDA report notebook
│   ├── raw/
│   └── processed/
├── requirements.txt
└── README.md

```

---

## Data Dictionary

*To be finalized after EDA and feature selection.*

| Variable | Description | Notes |
|---|---|---|
| `State` | U.S. state | 46 states with sufficient HCUP coverage, 2013–2023 |
| `Year` | Calendar year | 2013–2023 |
| `state_population` | Annual state population estimate | U.S. Census PEP |
| `mental_health_admissions` | Mental Health/Substance Use inpatient admissions, annual state total | Aggregated from HCUP quarterly counts |
| `admission_rate_per_100k` | Target variable admissions per 100,000 population | `(mental_health_admissions / state_population) × 100,000` |
| `[search term]_lag1` | Prior-year (year t) Google Trends search interest for a crisis/suicide related term | Predicts year t+1 admission rate |

---

## Tools Used

Python 3, pandas, NumPy, matplotlib, seaborn, statsmodels, scikit-learn, XGBoost, LightGBM, SHAP, MLflow, Jupyter Notebook / Google Colab, GitHub Desktop.

---

## How to Reproduce

```bash
git clone https://github.com/brendancollari/Team-Rho-Capstone-Project.git
cd Team-Rho-Capstone-Project
```

Open the notebooks in `notebooks/` — the data collection/merge notebook first, followed by the EDA notebook. Raw and processed datasets are included in `data/`, so notebooks can be run without re-pulling from source APIs.

---

## How to Run data collection notebook

Clone the repository and navigate to the project directory:

```bash
git clone https://github.com/brendancollari/Team-Rho-Capstone-Project.git
cd Team-Rho-Capstone-Project
python3 -m venv venv
source venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt

```
After running the commands above, Jupyter Notebook will open the project notebook. Open notebooks/processed/build_prediction_dataset.ipynb and select Run All. The processed dataset will be generated automatically and saved to data/processed/.
---
## References

- Google. (n.d.). Google Trends. https://trends.google.com/trends
- Agency for Healthcare Research and Quality. (2026). HCUP Fast Stats: State trends in hospital utilization by payer. Healthcare Cost and Utilization Project. https://datatools.ahrq.gov/hcup-fast-stats
- U.S. Census Bureau. (n.d.). State population totals and components of change. https://www.census.gov/programs-surveys/popest/data/tables.html
- Țîbîrnă et al. (2026)
- Alabi et al. (2026)

---

## Methods

**EDA:** Visualize each search term over time and by state; correlation checks between terms and lagged admission rate; multicollinearity checks across terms; summary statistics on the admissions target; in-state vs. between-state variation within Google search interest.

**Preprocessing:** TBD

**Modeling:**
- **Baseline:** Multiple linear regression with state fixed effects (lagged predictors)
- **Supervised:** Random Forest, XGBoost that will be tuned via cross-validation, interpreted with SHAP values
- **Unsupervised:** PCA (dimensionality/redundancy across search terms), hierarchical clustering (state similarity in search patterns)
- **Evaluation:** RMSE, MAE, R²

**Stack:** Python (pandas, NumPy, statsmodels, scikit-learn, XGBoost, LightGBM), Jupyter/Colab, matplotlib/seaborn/Plotly, SHAP, MLflow.

---

## Timeline (8 weeks)

1. Preliminary Project Proposal
2. Finalized Project Proposal and Data Collection
3. EDA
4. Data cleaning & preprocessing
5. Baseline model training/testing
6. Unsupervised models (PCA, hierarchical clustering)
7. Tuning & model refinement
8. Interpretation & final reporting

---

## Limitations

- Google Trends reflects search interest, which may not mirror actual mental health need. For example, news events or school assignments can inflate search volume that does not have to do with personal google search trends.
- The model does not account for differences in state mental health policy, funding, or Medicaid eligibility rules, which affect psychiatric bed capacity and admissions independent of search interest.
- The HCUP admissions target captures all Mental Health/Substance Use inpatient stays, not admissions specifically driven by suicide or crisis events.
