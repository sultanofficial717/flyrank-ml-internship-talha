# First Assignment — FlyRank ML Internship

**Applied Search Intelligence: Google Search Ranking & Discoverability**  
*Submitted by: Talha ([sultanofficial717](https://github.com/sultanofficial717))*

---

## 📌 Executive Summary

This repository contains my end-to-end Machine Learning work for the **FlyRank ML Internship 2026**.

The core objective of this project is solving an essential search infrastructure problem: **out of thousands of content pages, which decaying page should a human team refresh FIRST?**

By building, testing, and refining ML models on real anonymized Google Search Console (GSC) and Google Analytics 4 (GA4) search data (~30,000 pages), I developed a learned ranking model that outperforms hand-written heuristic rules by **> 3.9x** on `Precision@50` (improving from ~0.240 baseline precision to **0.940** with Gradient Boosting).

---

## 🎯 Completed Task Accomplishments & Key Wins

### 1. Week 1 — Exploratory Data Analysis & Truth Discovery ([`01_first_look_and_discovery.ipynb`](notebooks/01_first_look_and_discovery.ipynb))
- **Executed Full Baseline Pipeline**: Ran end-to-end features prep, baseline scoring, and model training.
- **Data Discovery A (Search Volume vs Traffic)**: Re-evaluated search volume against impressions for active pages (`impressions_90d > 0`). Found near-zero correlation ($r \approx 0.001$), confirming that target keyword volume is a poor predictor of actual organic page traffic.
- **Data Discovery B (CTR Cliff)**: Analyzed mean Click-Through Rate (CTR) across position tiers and content types, revealing steep CTR drop-offs past position tier 1–3 and identifying specific content types with performance gaps.
- **Data Discovery C (Content Length Myth)**: Verified that median word counts between growing (`up`) and declining (`down`) pages are nearly identical, proving content length alone is not the primary ranking lever.

### 2. Week 2 — Readable Models & Leakage Prevention ([`02_your_first_readable_model.ipynb`](notebooks/02_your_first_readable_model.ipynb))
- **Hand-Written Baseline vs. Learned Decision Tree**: Compared a transparent 2-rule condition (`stale x visible`) against a shallow depth-2 `DecisionTreeClassifier`.
- **Feature Leakage Prevention**: Demonstrated why outcome-derived metrics (like `trend_pct` and `trend_direction`) must **never** be used as model inputs, preventing circular training traps.
- **Depth & Holdout Analysis**: Evaluated tree depths ($d=2, 3, 4$) and validated models on strict out-of-sample client-holdout splits (`GroupShuffleSplit` on `client_id`).

### 3. Pipeline Engineering & Model Enhancements ([`scripts/`](scripts/))
- **Feature Engineering ([`01_prepare_features.py`](scripts/01_prepare_features.py))**:
  - Engineered log-transformed traffic signals (`log_pageviews_90d`, `log_users_90d`, `log_engaged_sessions_90d`).
  - Added non-leaky interaction ratios: `update_age_ratio` (`days_since_last_update / (content_age_days + 1)`) and `clicks_per_session` (`clicks_90d / (sessions_90d + 1)`).
  - Included binary indicators (`has_search_volume`, `has_word_count`, `has_clicks`, `has_ai_sessions`, `measurable_opportunity`).
- **Model Addition & Optimization ([`03_train_model.py`](scripts/03_train_model.py))**:
  - Integrated `gradient_boosting` (`HistGradientBoostingClassifier`) alongside Random Forest, Logistic Regression, and Decision Trees.
  - Added $R^2$ score (`r2_score`) and permutation importance evaluation to metric payloads.

---

## 📊 Comprehensive Model Performance & Evaluation

All metrics are evaluated on an honest **client-holdout split** (~20% of clients held out, ensuring no page from the same client appears in both training and test sets).

| Model / Baseline | Precision@50 | Precision@20 | Precision@100 | $R^2$ Score | ROC AUC | Avg Precision | Split Strategy |
|---|---|---|---|---|---|---|---|
| **Hand-Written Rule Baseline** | 0.240 | 0.150 | 0.360 | -0.0648 | 0.6269 | 0.4676 | Client-Holdout |
| **Logistic Regression** | 0.480 | 0.400 | 0.540 | 0.1066 | 0.7074 | 0.5367 | Client-Holdout |
| **Decision Tree ($d=5$)** | 0.500 | 0.450 | 0.600 | 0.1322 | 0.7347 | 0.5724 | Client-Holdout |
| **Random Forest** | 0.840 | 0.850 | 0.810 | 0.1587 | 0.7437 | 0.6243 | Client-Holdout |
| **Gradient Boosting (Best)** | **0.940** | **0.950** | **0.880** | **0.1758** | **0.7773** | **0.6879** | Client-Holdout |

### Key Findings & Insights:
1. **Precision@50 Lift**: Gradient Boosting achieves **0.940 Precision@50** (47 out of top 50 flagged pages are confirmed declining pages), outperforming the fixed baseline by **> 3.9x**.
2. **Predictive Drivers**: Permutation importance identifies `days_with_impressions`, `avg_position`, `log_impressions_90d`, `content_age_days`, `scroll_rate`, `ctr`, and `update_age_ratio` as top signals.
3. **Honest Claims**: Results are framed strictly as *observed / measured / directional decision-support* without circular leakage.

---

## 📁 Repository Structure & Assignment Notebooks

```text
├── notebooks/                   # Executed guided notebooks
│   ├── 01_first_look_and_discovery.ipynb
│   ├── 02_your_first_readable_model.ipynb
│   └── 03_working_with_the_full_release.ipynb
├── scripts/                     # Reference ML pipeline
│   ├── 01_prepare_features.py   # Feature vector creation & engineering
│   ├── 02_baseline_score.py     # Baseline rule scoring
│   ├── 03_train_model.py        # Model training & metrics evaluation
│   ├── 04_evaluate_and_export.py# Final ranked queue & report generation
│   ├── 05_build_pdf_report.py   # PDF report generator
│   └── run_all.py               # One-command pipeline execution script
├── data/                        # Datasets (gitignored via CI guard)
│   └── raw/content_refresh_anonymized.csv
├── outputs/                     # Generated results, charts, and reports
│   ├── model_report.md
│   ├── refresh_queue.csv
│   └── charts/
└── work/                        # Personal workspace & capstone notebooks
```

### Quick Execution (Local)

```bash
git clone https://github.com/sultanofficial717/flyrank-ml-internship-talha.git
cd flyrank-ml-internship-talha
pip install -r requirements.txt
python scripts/run_all.py
```

---

## 🔒 Data Ethics & Governance (`DATA_USE.md`)
- All datasets are pseudonymized and anonymized.
- Dataset files under `data/` are blocked by `.gitignore` and enforced by CI workflows.
- No private client info or un-anonymized search queries are stored or committed.

---

*Author: Talha ([sultanofficial717](https://github.com/sultanofficial717))*  
*FlyRank ML Internship 2026 | Code under MIT License*
