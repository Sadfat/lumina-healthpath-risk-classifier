# Lumina HealthPath — Predicting User Verification Status Using Logistic Regression

**Microsoft Elevate AI Developers Program | Capstone Project**  
**Author:** Abubakar Jibrin Gunda (Sadiq) | **Brand:** Gunda LobyAI | **Student ID:** MSDEV-2026-3799

---

## Business Context

Lumina HealthPath's digital health platform allows clinicians and patients to submit video-based health reports and claim statuses. As the platform scales to 200+ partner hospitals, a growing backlog of unverified user accounts is creating bottlenecks in the report review pipeline. The data science team has been tasked with building an automated binary classifier to predict `verified_status` — reducing manual review time by 75%.

---

## Dataset

| Property | Value |
|---|---|
| Records | 19,382 platform user accounts |
| Target | `verified_status` — `verified` / `not verified` |
| Class balance | ~94% not verified / ~6% verified (severe imbalance) |
| Key features | `claim_status`, `author_ban_status`, `video_transcription_text`, engagement metrics |

**Features used:**

| Feature | Type | Role |
|---|---|---|
| `claim_status` | Categorical | Claim vs opinion submission behaviour |
| `author_ban_status` | Categorical | Account standing on platform |
| `video_view_count` | Numeric | Content reach indicator |
| `video_like_count` | Numeric | Audience engagement signal |
| `video_share_count` | Numeric | Content amplification signal |
| `video_download_count` | Numeric | Content utility indicator |
| `video_comment_count` | Numeric | Authentic interaction signal |
| `video_duration_sec` | Numeric | Content depth proxy |
| `text_length` | Engineered | Character count from `video_transcription_text` |

---

## Pipeline Overview

```
Raw Platform Data (PostgreSQL)
        ↓
Phase 1: Discovery & Preprocessing
  → Descriptive statistics, class imbalance analysis
  → Median imputation (train-only fit)
        ↓
Phase 2: Feature Engineering & Encoding
  → text_length extracted from video_transcription_text
  → Label encoding: claim_status, author_ban_status
  → Feature correlation heatmap
        ↓
Phase 3: Model Construction
  → Stratified train/test split (80/20)
  → Oversampling minority class (verified) on training set
  → StandardScaler (train-only fit)
  → Baseline Logistic Regression
  → GridSearchCV (5-fold, optimising Recall)
  → Decision threshold optimisation
        ↓
Phase 4: Performance Evaluation
  → Confusion matrix (raw + normalised)
  → ROC-AUC curve
  → Coefficient interpretability table
        ↓
Phase 5: Actionable Insights & Business Recommendations
```

---

## Model Performance

| Metric | Constraint | Result | Status |
|---|---|---|---|
| Recall (verified) | ≥ 0.85 | **0.8755** | ✅ |
| False Negative Rate | < 15% | **12.45%** | ✅ |
| ROC-AUC | — | **0.5929** | ✅ |
| Coefficients documented | 9 / 9 | **9 / 9** | ✅ |
| NaN post-imputation | 0 | **0** | ✅ |
| Class imbalance handled | Required | **Oversampling** | ✅ |

**Best hyperparameters:** `C=5.0`, `solver=lbfgs`, `max_iter=500`  
**Optimal decision threshold:** `0.436`

---

## Key Business Recommendations

1. **Automate backlog triage** — model scores all accounts; only those above threshold go to manual review, cutting workload by ~75%
2. **Fast-track `claim_status` accounts** — strongest predictor of verified status; route to priority queue
3. **Use `text_length` as a pre-filter** — transcriptions > 150 characters signal substantive, legitimate reporting
4. **Auto-exclude banned accounts** — `author_ban_status` is strongly negatively associated with verified status
5. **Quarterly model refresh** — retrain when feature distributions shift > 2σ from training baseline

---

## Repository Structure

```
lumina-healthpath-risk-classifier/
├── lumina_healthpath_capstone.ipynb   # Main notebook (all 5 phases)
├── README.md                          # This file
└── requirements.txt                   # Python dependencies
```

---

## How to Run

```bash
git clone https://github.com/Sadfat/lumina-healthpath-risk-classifier.git
cd lumina-healthpath-risk-classifier
pip install -r requirements.txt
jupyter notebook lumina_healthpath_capstone.ipynb
```

---

*Built with the Discovery-to-Action (DTA) framework | Gunda LobyAI | Microsoft Elevate AI Developers Program*
