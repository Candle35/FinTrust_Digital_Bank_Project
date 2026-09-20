# FinTrust Digital Bank — Week 1 Submission (Data Science Track)

**AnalystLab Africa — Experience Lab Internship Programme**
*Understand & Plan*

- **Prepared by:** Emmanuella Esi Arhin (Nuella)
- **Track:** Data Science
- **Submission Deadline:** End of Week 1 (Sunday, 11:59 PM WAT)
- **Project:** FinTrust Financial Intelligence & Digital Banking Support Solution

> ⚠️ All data referenced below is synthetic and provided for educational purposes by AnalystLab Africa. `Risk_Review_Flag` is not a real fraud/AML determination.

---

## 1. Business Understanding

### 1.1 What problem is FinTrust trying to solve?

FinTrust Digital Bank has growing volumes of customer and transaction data but no integrated way to turn that data into predictive insight. Specifically, the bank currently relies on manual or ad-hoc processes to decide which transactions warrant closer risk review, with no data-driven model to support or speed up that judgement.

### 1.2 Why is the problem important?

As FinTrust's digital transaction volume grows, manual risk review alone will not scale. A reliable, data-informed way to flag transactions that are statistically more likely to require review helps protect customers and the bank, keeps review effort focused on the transactions most worth examining, and creates a foundation for more consistent, less subjective risk-related decisions.

### 1.3 How can the Data Science track contribute?

The Data Science track can explore the relationship between transaction/customer attributes and the existing Risk_Review_Flag label, define a clear predictive problem, engineer a candidate feature set, and design (then build and evaluate, in later weeks) a classification model that estimates the likelihood a given transaction needs risk review. This turns historical review outcomes into a reusable, testable predictive signal rather than a one-off manual judgement.

### 1.4 What type of output could the Data Science track provide?

- A defined predictive problem statement and target variable assessment.

- An exploratory data analysis (EDA) report documenting patterns associated with Risk_Review_Flag.

- A justified candidate feature set with data types and known concerns.

- A trained and evaluated classification model (Weeks 2–3), with performance metrics appropriate to an imbalanced target.

- A documented model limitations statement, to be consumed by the ML Engineering track for operationalisation and by management as decision support — never as a standalone fraud determination.

## 2. Resource Review

Resources relevant to the Data Science track, what each contains, how they will be used, and limitations identified during Week 1 review:

| **Resource**                             | **What It Contains**                                                                                                                                                                                                                                      | **How I Will Use It**                                                                                                                                                                                     | **Limitations Identified**                                                                                                                                                                                                                          |
|------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| FinTrust Transaction Data (CSV)          | 12,000 synthetic transaction records: 11 fields including Amount_NGN, Transaction_Type, Channel, Device_Type, Location, International_Transaction, Transaction_Status and the target field Risk_Review_Flag, spanning 1 Jan – 31 Mar 2026.                | Primary dataset for the predictive problem — source of the target variable and most candidate features.                                                                                                   | Only a 3-month window (limits ability to detect seasonality); 96 rows (0.8%) missing Device_Type/Location; no data dictionary provided for this file, so field meaning was inferred and cross-checked against the Project Brief and Knowledge Base. |
| FinTrust Customer Data (CSV)             | 1,500 synthetic customer records: demographic fields (Age, Gender, City), relationship fields (Tenure_Months, Account_Type, Account_Status), and behavioural fields (Digital_Engagement_Score, Customer_Segment, Preferred_Channel, Monthly_Income_Band). | Joined to transactions via Customer_ID to add customer-level context (segment, tenure, engagement) as candidate features.                                                                                 | One row per customer, but each customer has multiple transactions (avg. 8) — a naive random split at the transaction level risks leaking customer identity across train/test sets.                                                                  |
| FinTrust Data Dictionary (CSV/XLSX)      | Field-level definitions, business use, and "Modelling_Use" flag for the 12 Customer Data fields.                                                                                                                                                          | Used to confirm which customer fields are approved for modelling (e.g., Customer_Name is flagged "No" and excluded) and to correctly interpret field meaning.                                             | Covers only the Customer Data fields; the Transaction Data fields have no equivalent dictionary, so their definitions were inferred from column names, value inspection, and the Knowledge Base/Project Brief.                                      |
| FinTrust Project Brief (PDF)             | Business scenario, problem statement, scope, governance principles, and the multidisciplinary team structure.                                                                                                                                             | Used to ground the predictive problem in FinTrust's actual business objective and to confirm project boundaries (e.g., Risk_Review_Flag is explicitly a synthetic label, not a real fraud determination). | None significant — this is a scoping document, not a data source.                                                                                                                                                                                   |
| FinTrust 4-Week Master Roadmap (PDF)     | Week-by-week phase plan across all four weeks and tracks.                                                                                                                                                                                                 | Used to align this Week 1 plan with what Weeks 2–4 will require, and to anticipate cross-track dependencies.                                                                                              | High-level only — Week 1 assignment instructions provide the operational detail.                                                                                                                                                                    |
| FinTrust Financial Knowledge Base (DOCX) | Approved customer-support policy content for the Generative AI track (account access, transactions, transfers, escalation rules).                                                                                                                         | Reviewed for business context only; not a data source for the Data Science track.                                                                                                                         | Out of scope for modelling — belongs to the GenAI track.                                                                                                                                                                                            |

## 3. Data Science Track Objectives

**1.** Clearly define a supervised classification problem that predicts the likelihood a transaction needs risk review, using the FinTrust transaction and customer datasets.

**2.** Conduct a thorough exploratory data analysis and data-quality assessment across both datasets, documenting how customer- and transaction-level attributes relate to Risk_Review_Flag.

**3.** Identify and justify a candidate feature set, with data types and known concerns for each feature.

**4.** Build, tune and validate a baseline and at least one stronger candidate model in Weeks 2–3, evaluated with metrics appropriate to an imbalanced target rather than accuracy alone.

**5.** Document assumptions, limitations and model behaviour clearly enough to support honest, portfolio-ready reporting and eventual hand-off to the ML Engineering track.

## 4. Success Criteria

The Data Science track's work will be considered successful if:

- A reproducible baseline model and at least one candidate model exist, with Precision, Recall, F1 and ROC-AUC/PR-AUC reported — not accuracy alone, given the ~80/20 class imbalance in Risk_Review_Flag.

- The candidate model meaningfully outperforms a naive majority-class baseline (which would score ~80.4% accuracy by always predicting "No" review, while being useless as a decision tool).

- Feature importance findings are interpretable and can be explained to a non-technical stakeholder (e.g., "international and night-time transactions are more often flagged").

- Key assumptions and limitations — synthetic label, possible leakage from Transaction_Status, small time window, customer-level grouping — are explicitly documented rather than hidden.

- Feature definitions and model outputs are in a form the ML Engineering track can consume in Week 2+ for pipeline integration.

## 5. Initial Plan: Weeks 2–4

### Week 2 — Analyse & Prepare

- Merge customer and transaction data on Customer_ID; engineer time-based features (hour of day, day of week, is-night) from Transaction_DateTime.

- Handle the 96 missing Device_Type/Location values (likely as an explicit "Unknown" category rather than dropping rows).

- Run deeper EDA with visualisations (distribution of Amount_NGN, risk rate by category, correlation checks).

- Encode categorical features and build a majority-class baseline plus a simple logistic regression baseline.

- Begin formal cross-track collaboration: share the finalised feature list/data format expectations with the ML Engineering track.

### Week 3 — Develop & Integrate

- Train and tune candidate models (e.g., Random Forest, Gradient Boosting) using a customer-grouped train/test split to avoid leakage.

- Compare candidates against the baseline using Precision/Recall/F1/AUC; run k-fold cross-validation.

- Conduct error analysis — examine whether false positives/negatives cluster by transaction type, channel or international status.

- Draft a short model card documenting features used, performance and limitations; align with ML Engineering on the model's expected input/output contract.

### Week 4 — Test, Refine & Present

- Finalise the model and run a final evaluation; write up findings, limitations and recommendations.

- Support integration/testing with the ML Engineering track and, where relevant, share risk-related insight that could inform the GenAI track's escalation guidance.

- Package all Data Science deliverables and prepare the individual recorded presentation.

### Relevant Future Dependencies (from Week 2 onward)

| **Dependency**                          | **From → To**                                      | **Why It May Matter**                                                                                                                                                       | **Expected Week** |
|-----------------------------------------|----------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|
| Finalised feature list & model artifact | Data Science → ML Engineering                      | ML Engineering needs a stable feature set and model output format to build the operational pipeline/API.                                                                    | Week 3            |
| Model risk-driver insights              | Data Science → Project Management / Data Analytics | Findings on which transaction attributes associate with risk review can inform KPI and dashboard design.                                                                    | Week 2–3          |
| Escalation-relevant risk patterns       | Data Science → Generative AI                       | If patterns emerging from the model relate to transactions customers flag as unrecognised, this can (optionally) inform how the GenAI assistant frames escalation guidance. | Week 3–4          |

### Relevant Risks

| **Risk**                                                                                                                    | **Probability** | **Impact** | **Mitigation**                                                                                |
|-----------------------------------------------------------------------------------------------------------------------------|-----------------|------------|-----------------------------------------------------------------------------------------------|
| Risk_Review_Flag imbalance (~80/20) causes a naive model to default to predicting the majority class.                       | High            | High       | Use Precision/Recall/F1/PR-AUC instead of accuracy; consider class weighting or resampling.   |
| Transaction_Status may be assigned alongside or after the risk-review decision, creating data leakage if used as a feature. | Medium          | High       | Test model performance with and without Transaction_Status; document the decision either way. |
| Customer-level join means each customer's transactions are not independent, risking leakage across train/test splits.       | Medium          | Medium     | Use a customer-grouped (not random) train/test split.                                         |
| Missing Device_Type/Location (0.8% of transactions) if mishandled could bias or reduce the usable dataset.                  | Low             | Low        | Encode as an explicit "Unknown" category rather than dropping rows.                           |
| Only three months of data (Jan–Mar 2026) limits the ability to detect seasonal or longer-term patterns.                     | Medium          | Medium     | State this explicitly as a model limitation; avoid claims about yearly seasonality.           |

## 6. Data Science Track Task — FinTrust Predictive Intelligence Problem Definition

### Part A — Predictive Problem Statement

| **Question**                                   | **Answer**                                                                                                                                                                                                                                                                                                                   |
|------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| What do we want to predict?                    | Whether a given transaction will be flagged for risk review (Risk_Review_Flag = Yes/No) — a binary classification problem.                                                                                                                                                                                                   |
| Why could prediction be useful?                | It converts a manual, after-the-fact review outcome into a signal that can (in principle) be estimated at or near transaction time, helping focus limited review capacity on the transactions statistically most likely to need it, rather than reviewing every transaction equally or relying only on individual judgement. |
| Who could use the prediction?                  | FinTrust's risk/operations team (to prioritise review queues), the ML Engineering track (to operationalise the model into a workflow), and indirectly management (as a input to decision-making, not a final verdict).                                                                                                       |
| What would a successful model need to achieve? | Meaningfully outperform the ~80.4% majority-class baseline on Precision/Recall/F1/AUC (not accuracy alone, given the imbalance); surface interpretable, defensible risk drivers; and be explicit about its limitations as a synthetic-data, educational model rather than a real fraud-detection system.                     |

### Part B — Target Assessment: Risk_Review_Flag

Investigated directly against the 12,000-row transaction dataset:

| **Aspect**                          | **Finding**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|-------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| What the field represents           | A binary label indicating whether a transaction was flagged for risk review under FinTrust's (synthetic, educational) risk process.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Possible values                     | "Yes" and "No" — no missing values in this field across all 12,000 rows.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Class distribution                  | No = 9,648 transactions (80.4%); Yes = 2,352 transactions (19.6%).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Can it serve as a modelling target? | Yes — it is a clean, complete, binary field with no missing values, suitable as the target for a supervised classification problem.                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Balanced or imbalanced?             | Imbalanced, roughly 80/20. A model optimised purely for accuracy could reach ~80.4% by always predicting "No" while providing no real decision value — this makes accuracy alone a misleading success metric.                                                                                                                                                                                                                                                                                                                                                                       |
| Potential limitations               | As stated in the Project Brief, this is a synthetic educational label, not a real fraud or AML determination — it should never be presented as one. It reflects whatever rule or process generated the training data, which may not match how FinTrust (or any real bank) would define risk in practice. There is also a plausible leakage concern: Transaction_Status shows its highest risk rate for "Failed" transactions (26.9%), which may mean status and risk flag are not fully independent — this needs to be investigated before Transaction_Status is used as a feature. |

### Part C — Candidate Features

Identified from EDA on both datasets (transaction-level unless noted); risk rates below are the observed proportion of Risk_Review_Flag = "Yes" within each category, from the current 12,000-row extract:

| **Feature**                                          | **Why It May Matter**                                                                                                          | **Data Type**               | **Potential Concern**                                                                                                              |
|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|-----------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| Amount_NGN                                           | Flagged transactions average ₦68,786 vs ₦41,324 for non-flagged — a large observed gap.                                        | Numeric (continuous)        | Right-skewed with high-value outliers (max ₦693,454); may need a log transform.                                                    |
| International_Transaction                            | International transactions are flagged 36.9% of the time vs 18.9% for domestic — the largest gap of any feature reviewed.      | Categorical (binary)        | Only 4% of transactions are international — a sparse category, risk of overfitting to few examples.                                |
| Transaction_Type                                     | Transfer (28.5%) and Cash Withdrawal (25.3%) show much higher review rates than Bill Payment (12.8%) or Card Purchase (13.0%). | Categorical (6 levels)      | Needs one-hot/target encoding; some levels are less frequent than others.                                                          |
| Hour of day (derived from Transaction_DateTime)      | Night-time transactions (00:00–05:59) are flagged 28.4% of the time vs 16.7% during the day.                                   | Numeric/derived categorical | Only 3 months of data; unclear if the pattern would hold across a full year or different time zones handling.                      |
| Channel                                              | Web (21.3%) and ATM (21.2%) show slightly higher review rates than POS (18.3%) and USSD (17.3%).                               | Categorical (5 levels)      | Correlated with Device_Type (e.g., ATM channel ~ ATM Terminal device) — possible redundancy/multicollinearity.                     |
| Transaction_Status                                   | "Failed" transactions show the highest review rate (26.9%) vs "Successful" (19.2%).                                            | Categorical (4 levels)      | Possible data leakage if status is set at/after the same review step that sets Risk_Review_Flag — needs investigation before use.  |
| Customer_Segment (joined from Customer Data)         | Premium customers show a marginally higher rate (20.3%) than Everyday (19.3%), SME (19.4%) and Student (19.7%).                | Categorical (4 levels)      | Differences are small — likely a weak standalone signal.                                                                           |
| Digital_Engagement_Score (joined from Customer Data) | Synthetic 10–100 engagement score; a customer-level behavioural signal not yet tested for direct association with risk rate.   | Numeric (continuous)        | One value per customer repeated across ~8 transactions on average — must use a customer-grouped train/test split to avoid leakage. |
| Tenure_Months (joined from Customer Data)            | Captures how established the customer relationship is; newer relationships could plausibly carry different risk patterns.      | Numeric (continuous)        | Same customer-level repetition/leakage concern as Digital_Engagement_Score.                                                        |

### Part D — Hypotheses

Framed as hypotheses to test in Weeks 2–3, not as conclusions:

**1.** International transactions are more likely to be flagged for risk review than domestic transactions, because they typically move funds outside FinTrust's usual monitoring environment and involve less established behavioural history for the bank to compare against.

**2.** Transactions made late at night (00:00–05:59) are more likely to be flagged for risk review than daytime transactions, possibly because off-hours activity deviates more from a customer's typical usage pattern.

**3.** Transfers and cash withdrawals are more likely to be flagged for risk review than bill payments or airtime/data purchases, because they move funds out of the FinTrust ecosystem entirely, whereas bill payments and airtime purchases are lower-value, routine and more predictable.

**4.** Higher-value transactions are more likely to be flagged for risk review than lower-value ones, consistent with the observed gap in average transaction amount between flagged and non-flagged transactions.

### Part E — Initial Modelling Plan

| **Stage**            | **Plan**                                                                                                                                                                                                                                                                                                                                  |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Data preparation     | Merge Customer Data and Transaction Data on Customer_ID; parse Transaction_DateTime into hour/day-of-week/is-night features; encode the 96 missing Device_Type/Location values as an explicit "Unknown" category; confirm no duplicate Transaction_IDs and no zero/negative amounts (both already verified clean in Week 1).              |
| Exploratory analysis | Visualise Amount_NGN distribution (with and without log transform); chart Risk_Review_Flag rate by Transaction_Type, Channel, International_Transaction, Transaction_Status and hour of day; check correlation among numeric features; examine trends across the Jan–Mar 2026 window.                                                     |
| Feature engineering  | One-hot/target-encode categorical features; log-transform Amount_NGN; derive hour-of-day and weekend/weekday flags; join relevant customer-level fields (Customer_Segment, Digital_Engagement_Score, Tenure_Months, Account_Status).                                                                                                      |
| Baseline model       | Majority-class baseline (always predict "No") as the accuracy floor (~80.4%) that is intentionally uninformative; logistic regression as a simple, interpretable modelling baseline.                                                                                                                                                      |
| Candidate algorithms | Random Forest and Gradient Boosting (e.g., XGBoost/LightGBM) to capture non-linear interactions between features such as amount, time of day and transaction type.                                                                                                                                                                        |
| Evaluation metrics   | Precision, Recall, F1-score, ROC-AUC and PR-AUC — prioritised over raw accuracy given the ~80/20 class imbalance.                                                                                                                                                                                                                         |
| Validation           | Customer-grouped train/test split (not a random row-level split) to prevent the same customer's transactions appearing in both sets; k-fold cross-validation for model comparison.                                                                                                                                                        |
| Error analysis       | Review false positives and false negatives by Transaction_Type, Channel and International_Transaction to see whether errors cluster in particular segments.                                                                                                                                                                               |
| Model limitations    | Risk_Review_Flag is a synthetic educational label, not a verified fraud outcome; the dataset covers only three months, limiting seasonal generalisation; Transaction_Status will be tested for leakage before being trusted as a feature; findings should be communicated as decision support, never as an automated fraud determination. |

### Data-Quality Observations

- Customer Data (1,500 rows, 12 columns): no missing values in any field; no duplicate Customer_ID rows.

- Transaction Data (12,000 rows, 11 columns): Device_Type and Location are each missing in 96 rows (0.8%); 92 of these rows are missing both fields together. Missing rows are spread across all channels (Mobile App: 42, POS: 23, Web: 14, ATM: 11, USSD: 6), so the pattern does not appear tied to a single channel.

- No duplicate Transaction_IDs across all 12,000 transactions; every Customer_ID referenced in the transaction data exists in the customer data and vice versa (a clean one-to-many relationship, avg. 8 transactions per customer, range 1–20).

- Amount_NGN has no zero or negative values; the distribution is right-skewed (median ₦10,306 vs mean ₦46,706, max ₦693,454), which is expected for transaction amounts but will need a log transform or robust scaling for some models.

- Transaction_DateTime parses cleanly for all 12,000 rows and spans 1 January – 31 March 2026 (a 3-month window only).

- Risk_Review_Flag has no missing values and is class-imbalanced (~80% No / ~20% Yes), which must be accounted for in modelling and evaluation.

- Gender includes a third category, "Prefer not to say" (51 customers), alongside Male (727) and Female (722) — worth preserving rather than collapsing.

- This is synthetic, educational data (per the Project Brief); no findings from it should be presented as reflecting real FinTrust operations or real fraud outcomes.

*End of Week 1 Submission — Data Science Track.*
