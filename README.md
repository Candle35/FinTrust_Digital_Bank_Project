# FinTrust Digital Bank — Predictive Risk Review Project (Data Science Track)

**Author:** Emmanuella Esi Arhin · **Programme:** AnalystLab Africa Experience Lab · **Project:** FinTrust Financial Intelligence & Digital Banking Support Solution

> **Responsible use.** FinTrust is a fictional bank and every dataset here is synthetic. `Risk_Review_Flag` is a **synthetic educational target**; it is **not** evidence of fraud or financial crime. This is an educational prototype, not a production banking system, and it gives no personalised financial, investment or legal advice.

## The project
FinTrust has growing volumes of customer and transaction data but no data-driven way to decide which transactions deserve a closer manual risk review. This repository follows my Data Science track over four weeks: define the problem, prepare and explore the data, build and compare models, and finalise a validated, documented model that **ranks transactions by how likely they were marked for review**, so a limited review team can spend its time where it helps most.

## Final result (Week 4)
| | |
|---|---|
| **Final model** | Logistic Regression (balanced class weights) + Platt calibration layer |
| **Four inputs** | `Transaction_Type` · `high_amount` (Amount_NGN ≥ NGN 160,000) · `is_international` · `is_night` (00:00–05:59) |
| **Main evidence** | Rolling-origin evaluation on 7,200 predictions made on later data: ROC-AUC 0.674 [0.658, 0.688] |
| **Hold-out test (20% review budget)** | precision 0.357 · recall 0.368 · F1 0.362 · ROC-AUC 0.679 · lift 1.84× |
| **Honest headline** | Ranking quality is about 0.67 ROC-AUC, the same as Week 2. A flexible model given *every* field reaches only 0.68, so the ceiling is a limit of the data. The gain is a **simpler, calibrated, safer, tested** model, not a more accurate fraud detector. |

## Project journey (Weeks 1–4)
| Week | Focus | What was done | Key result |
|---|---|---|---|
| **1** | Understand and plan | Defined the predictive problem; reviewed the project resources; assessed the target (about 80% No / 20% Yes); listed candidate features and hypotheses; documented data-quality checks (no duplicates; 96 missing `Device_Type`/`Location` values) | A clear problem statement and modelling plan |
| **2** | Analyse and prepare | Joined customer and transaction data; filled missing values with "Unknown"; 9 EDA charts; 8 engineered features; balanced Logistic Regression baseline (12 features) | Baseline ROC-AUC 0.668, recall 0.594, precision 0.289 |
| **3** | Develop and validate | Moved to a chronological 60/20/20 split; cut features from 12 to 5 (night flag, log and high-amount flag, international, type); tuned four model families; bootstrap intervals; error analysis; chose a Logistic Regression candidate | Test ROC-AUC 0.672, precision 0.360 and recall 0.366 at a 20% review budget |
| **4** | Test, refine, present | Continued from the Week 3 dataset (reproduced exactly); 20 feature variants; nested rolling-origin evaluation; 8 models compared with a pre-stated selection rule; calibration; safe scoring function with 15 tests; comparison with the Data Analytics intern's findings | Final four-feature model above; calibration error 0.281 → 0.021; scoring tests 7/15 → 15/15 |

## What the model says
- **Risk rises** with Transfer and Cash Withdrawal transactions, night-time (00:00–05:59), international transactions and amounts of NGN 160,000 or more.
- **The amount effect is a step**, not a slope: a scan picks NGN 160,000 in every rolling window.
- **Customer attributes carry no signal** (age, gender, segment, tenure, engagement), so they are not model inputs.
- **`Transaction_Status` helps slightly** (+0.006 ROC-AUC) but may not be known when a transaction is scored, so it is deliberately excluded.
- **About two thirds of flagged transactions are false alarms** at a 20% review budget. It is a prioritisation aid for a human reviewer.

## Repository structure
```
FinTrust_Digital_Bank_Project/
├── README.md                 ← this file (whole project)
├── data/
│   ├── raw/                  FinTrust customer and transaction CSVs (synthetic)
│   └── processed/            Week 3 modelling dataset (v2); Week 4 final dataset is saved here when the notebook runs
├── week-1/                   Problem definition, resource review, target assessment, plan
├── week-2/                   EDA, data preparation, feature engineering, baseline model, documentation
├── week-3/                   Advanced modelling notebook, documentation and outputs
└── week-4/                   Final model: notebook, models/, reports/, presentation script
    ├── FinTrust_Week4_DS_Final_Model.ipynb
    ├── models/               final model, calibrator and model card (created when the notebook runs)
    └── reports/figures/      5 key charts (created when the notebook runs)
```

## How to reproduce the final results
**1. Data files.** Three files are needed, all under `data/`:

| File | What it is |
|---|---|
| Customer data CSV | Approved synthetic customer data (1,500 rows) |
| Transaction data CSV | Approved synthetic transaction data (12,000 rows, 1 Jan – 31 Mar 2026) |
| `FinTrust_Modelling_Dataset_v2.csv` | The Week 3 modelling dataset, the starting point of Week 4 |

**2. Install the packages.**
```bash
pip install pandas numpy scikit-learn scipy matplotlib seaborn joblib jupyter nbformat
```
Results were produced with Python 3.12, scikit-learn 1.8, pandas 3.0 and NumPy 2.4.

**3. Run the notebook.** Open `week-4/FinTrust_Week4_DS_Final_Model.ipynb`, check that the three file paths in the setup cells point to the files above, and choose *Run All* (about 6–8 minutes; the seed is fixed at 42). Running it creates `week-4/models/` and `week-4/reports/figures/`, and saves the final dataset (`FinTrust_Modelling_Dataset_Final.csv`) in `data/processed/`.

**4. Check it worked.** The notebook first confirms that its preparation reproduces the Week 3 dataset exactly (all 12,000 rows) and that the Week 3 candidate's metrics match the Week 3 submission (cut-off NGN 142,058; test ROC-AUC 0.672; PR-AUC 0.321). If either check fails, it stops with an error.

## Scoring new transactions
The notebook defines `score_transactions(df)` (Section 10.1). It takes raw transaction fields (`Transaction_Type`, `Amount_NGN`, `International_Transaction`, `Transaction_DateTime`) and returns one row per transaction with:

| Column | Meaning |
|---|---|
| `status` / `reason` | `scored`, or `rejected` with the reason (invalid records are never silently scored) |
| `n_signals` | How many of: Transfer/Cash Withdrawal, night-time, international, high amount |
| `risk_score` | Raw model score, for ranking only |
| `flag_probability` | Calibrated probability ("roughly x in 100 similar transactions are flagged") |
| `review_flag` | True if the score falls in the default top-20% review budget |
| `needs_manual_check` | True for rejected records, to be routed to a human |

The saved files in `week-4/models/` (`final_model.joblib`, `final_calibrator.joblib`, `final_model_card.json`) hold the model, its calibration layer and the input/output contract.

## Collaboration and integration
- **Data Analytics track:** I received the Data Analytics intern's SQL script and three-page Power BI dashboard (screenshots; no SQL result tables). All 74 values printed on the dashboard reproduce from the raw data, and their findings were compared with my predictive tests. They prompted two extra tests but did not change the model.
- **Not integrated:** no ML Engineering or Generative AI output was received, so no integration with those tracks is claimed. The model card documents the input and output contract for ML Engineering.

## Limitations
Synthetic label; a three-month window (no seasonality or drift can be assessed); a modest ceiling; the 20% review budget and the NGN 160,000 cut-off are assumptions to re-estimate; the test period was viewed in Week 3, so the Week 4 conclusions rest mainly on the rolling evaluation.

## Tools
Python 3.12, Jupyter / VS Code, pandas, NumPy, scikit-learn, SciPy, Matplotlib, Seaborn, joblib, Git and GitHub.

## Acknowledgements
Built as part of the **AnalystLab Africa Experience Lab Internship Programme**. Thanks to the Data Analytics intern whose dashboard and SQL work I used for the comparison.

#AnalystLabAfrica
