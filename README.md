# Lumina HealthPath — Automated Patient Risk Classifier

**Microsoft Elevate AI Developers Program | Capstone Project**
**Author:** Abubakar Jibrin Gunda (Sadiq) | **Brand:** Gunda LobyAI | **Student ID:** MSDEV-2026-3799

---

## Business Context

Lumina HealthPath is a 150-person HealthTech scale-up specialising in non-invasive diagnostic insights from wearable device data and longitudinal health records. Their clinical teams currently categorise patients into **High Risk** or **Stable** metabolic groups using fragmented Excel macros and hard-coded if-else thresholds — a process that is slow, expensive, and prone to human subjectivity.

This project replaces that legacy system with an interpretable, recall-optimised **Logistic Regression** binary classifier trained on 44,000+ anonymised patient records.

---

## Problem Statement

| Issue | Impact |
|---|---|
| Manual clinical reviews cannot scale | $1.2M annual operational deficit projected |
| No baseline performance metric exists | No way to measure or improve accuracy |
| 500% patient data growth expected next quarter | Current process will collapse under volume |
| Fragmented Excel logic misses feature interactions | High-risk patients are being missed |

---

## Solution Overview

A three-phase pipeline following the **Discovery-to-Action (DTA)** framework:

```
PostgreSQL Data Warehouse
        ↓
Phase 1: Data Engineering & EDA
  → Median Imputation (train-only fit)
  → StandardScaler (train-only fit)
  → Feature Correlation Heatmap
        ↓
Phase 2: Model Development
  → Baseline Logistic Regression (class_weight='balanced')
  → GridSearchCV (5-fold stratified CV, optimising Recall)
  → Decision Threshold Optimisation
        ↓
Phase 3: Clinical Validation
  → Confusion Matrix (raw + normalised)
  → ROC-AUC Curve
  → Coefficient Interpretability Table
  → Executive Stakeholder Dashboard
        ↓
AWS SageMaker (Inference) → Tableau (Reporting)
```

---

## Dataset

| Property | Value |
|---|---|
| Records | 44,108 anonymised patient records |
| Features | 12 metabolic markers |
| Target | Binary — `1` = High Risk, `0` = Stable |
| Class balance | ~38% High Risk / ~62% Stable |
| Missing values | ~4% (wearable drop-out pattern, median-imputed) |

**Features used:**

| Feature | Type | Clinical Role |
|---|---|---|
| Age | Continuous | Cumulative metabolic burden |
| BMI | Continuous | Adiposity / insulin resistance proxy |
| Glucose | Continuous | Fasting blood sugar (hyperglycaemia marker) |
| HbA1c | Continuous | Chronic glycaemic control (gold standard) |
| Systolic_BP | Continuous | Cardiovascular / metabolic syndrome marker |
| Diastolic_BP | Continuous | Hypertensive risk contribution |
| Cholesterol | Continuous | Dyslipidaemia indicator |
| Triglycerides | Continuous | Metabolic syndrome core component |
| Activity_Steps | Continuous | Physical activity (protective factor) |
| Sleep_Hours | Continuous | Restorative health proxy |
| Smoking | Binary | Oxidative stress / atherosclerosis accelerator |
| Family_History | Binary | Heritable metabolic vulnerability |

---

## Model Performance

> All constraints set by Lumina HealthPath's clinical auditing team are met.

| Metric | Constraint | Result | Status |
|---|---|---|---|
| Recall (High Risk) | ≥ 0.85 | **0.8508** | ✅ |
| False Negative Rate | < 15% | **14.92%** | ✅ |
| ROC-AUC | — | **0.9331** | ✅ |
| F1-Score | — | **0.8260** | ✅ |
| Coefficients documented | All 12 features | **12 / 12** | ✅ |
| NaN post-imputation | 0 | **0** | ✅ |

**Best hyperparameters (GridSearchCV):** `C=0.001`, `solver=liblinear`, `max_iter=500`, `class_weight=balanced`
**Optimal decision threshold:** `0.489` (lowered from default 0.50 to meet Recall constraint)

---

## Key Clinical Findings — Coefficient Interpretability

| Feature | Direction | Odds Ratio | Clinical Note |
|---|---|---|---|
| HbA1c | ↑ Risk | Highest | Gold-standard chronic glycaemia marker |
| Glucose | ↑ Risk | High | Direct insulin resistance indicator |
| Family_History | ↑ Risk | High | Non-modifiable heritable risk |
| Smoking | ↑ Risk | Moderate | Accelerates atherosclerosis |
| Age | ↑ Risk | Moderate | Cumulative metabolic burden |
| Activity_Steps | ↓ Risk | Protective | Improves insulin sensitivity |

---

## Repository Structure

```
lumina-healthpath-risk-classifier/
│
├── lumina_healthpath_capstone.ipynb   # Main notebook (all 3 phases)
├── README.md                          # This file
└── requirements.txt                   # Python dependencies
```

---

## How to Run

```bash
# 1. Clone the repository
git clone https://github.com/Sadfat/lumina-healthpath-risk-classifier.git
cd lumina-healthpath-risk-classifier

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch the notebook
jupyter notebook lumina_healthpath_capstone.ipynb
```

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.x | Core language |
| Scikit-Learn | Logistic Regression, GridSearchCV, StandardScaler, metrics |
| Pandas / NumPy | Data manipulation |
| Matplotlib / Seaborn | Visualisation |
| Jupyter Notebook | Development environment |
| AWS SageMaker | Production inference (target deployment) |
| Tableau | Stakeholder risk distribution reporting |
| PostgreSQL | Source data warehouse |

---

## Constraints Satisfied

- ✅ Logistic Regression used exclusively (interpretable for medical auditors)
- ✅ Recall ≥ 0.85 for the High Risk class
- ✅ Missing values handled via median imputation (no NaN in training matrix)
- ✅ All features standardised using StandardScaler
- ✅ Confusion matrix included (raw counts + normalised rates)
- ✅ No black-box models used

---

*Built with the Discovery-to-Action (DTA) framework | Gunda LobyAI | Microsoft Elevate AI Developers Program*
