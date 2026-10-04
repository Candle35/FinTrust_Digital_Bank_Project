# FinTrust Digital Bank — AnalystLab Africa Experience Lab

**Intern:** Emmanuella Esi Arhin
**Track:** Data Science
**Programme:** AnalystLab Africa Experience Lab — 4-Week FinTrust Project

## Project Overview

FinTrust Digital Bank is a fictional case used by the AnalystLab Africa Experience Lab to simulate a real multidisciplinary fintech project. Working within the Data Science track, this repo documents my process across four weeks: understanding the business problem, exploring the data, building and evaluating a predictive model for transaction risk review, and presenting findings.

All data used (customer records, transaction records) is **synthetic and for educational purposes only** — nothing here reflects a real bank's operations, customers, or fraud outcomes.

## Repository Structure

```
├── week-1/   Business understanding, resource review, predictive problem definition,
│             target/feature assessment, hypotheses, initial modelling plan
├── week-2/   Data preparation, feature engineering, exploratory analysis, baseline model
├── week-3/   Model development, tuning, validation, error analysis
├── week-4/   Final evaluation, write-up, and presentation materials
└── README.md
```

Each week's folder contains a submission write-up (`weekN-submission.md`) plus any supporting code, notebooks, or artifacts produced that week.

## Week 2 — What's Inside          

Data preparation and quality assessment on the customer and transaction datasets, exploratory data analysis (visualisations against `Risk_Review_Flag`), 8+ engineered features, a baseline Logistic Regression model with evaluation (Accuracy, Precision, Recall, F1, ROC-AUC), model interpretation, and a Week 2 Project Documentation write-up.

## Track Objectives

1. Define a supervised classification problem predicting the likelihood a transaction needs risk review.
2. Conduct thorough EDA and data-quality assessment across the customer and transaction datasets.
3. Identify and justify a candidate feature set.
4. Build, tune and validate a baseline and candidate model, evaluated with metrics appropriate to an imbalanced target.
5. Document assumptions, limitations and results clearly enough for portfolio-ready reporting.

## What Week 3 builds on
The notebook starts from the Week 2 prepared dataset and adds features on top of it. Section 2 reproduces the submitted Week 2 baseline exactly (balanced Logistic Regression, 12 features, random 80/20 split, threshold 0.50: accuracy 0.635, precision 0.289, recall 0.594, F1 0.389, ROC-AUC 0.668) and asserts that the reproduced numbers match.

## Headline results (chronological test set, n = 2,400)
- **Ranking quality is unchanged from Week 2:** ROC-AUC 0.672 for the candidate vs 0.676 for the Week 2 recipe (95% CI of the difference −0.022 to +0.015).
- **At a fixed 20% review budget** the candidate reaches precision 0.360 / recall 0.366 vs 0.326 / 0.331 for Week 2 (bootstrap intervals exclude 0).
- **Simpler and safer:** 5 inputs instead of 12; no look-ahead customer counts; no customer attributes.
- `Transaction_Status` and `Account_Status` stay excluded (Week 2 decision); adding them would change test ROC-AUC by only about +0.002 and +0.001.
- The F1-optimal threshold does not beat the Week 2 baseline on F1 (0.395 vs 0.406); the 20%-budget operating point is the recommended default.
- Remaining errors are mostly label noise: every false negative has at most one risk signal.
- The candidate was chosen by a pre-stated rule (simplest model within 0.01 of the best validation scores). Logistic Regression sits right on that boundary, so the choice is stated explicitly.
## Status

- [x] Week 1 — Understand & Plan
- [x] Week 2 — Analyse & Prepare
- [x] Week 3 — Develop & Integrate
- [ ] Week 4 — Test, Refine & Present

## Disclaimer

This project uses synthetic, educational data provided by AnalystLab Africa. `Risk_Review_Flag` is a synthetic label, not a verified fraud or AML determination. Nothing in this repository should be interpreted as reflecting real FinTrust (or any real bank's) operations, policies, or customer data.
