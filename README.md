# 📉 Customer Churn Prediction & Retention Prioritization

Predict customer churn using interpretable machine learning and prioritize high-risk accounts for targeted, resource-efficient retention campaigns.

[![Python 3.11](https://img.shields.io/badge/Python-3.11-blue?logo=python)](https://www.python.org/)
[![XGBoost](https://img.shields.io/badge/XGBoost-1.7+-green?logo=xgboost)](https://xgboost.ai/)
[![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-yellow?logo=powerbi)](https://powerbi.microsoft.com/)
[![Status](https://img.shields.io/badge/Status-Production--Ready-brightgreen)]()
[![License](https://img.shields.io/badge/License-MIT-orange)](LICENSE)

> 💡 *Business Context: Simulates American Express AIM Team workflow — proactive retention & save-a-card analytics for SME cardholders.*

---

## 🏢 Problem Statement & Business Impact

Customer churn costs **5–25x more** than retention. This project builds a predictive system to:
1. **Identify** customers likely to churn within the next billing cycle
2. **Explain** why using interpretable ML (SHAP)
3. **Prioritize** retention efforts using a risk × value tiering framework

**Business Impact:** Enables proactive, resource-efficient retention campaigns focused on high-value, high-risk customers, reducing wasted marketing spend and improving customer lifetime value (CLV).

---

## 👤 My Contribution

This was a solo end-to-end portfolio project. Key engineering & analytical work I completed:
- ✅ Engineered behavioral features beyond standard RFM (`txn_velocity`, `engagement_decay`, `credit_utilisation`)
- ✅ Built the 3-tier prioritization framework from scratch (churn probability × customer value)
- ✅ Designed Power BI dashboard layout, DAX measures, and interactive filters
- ✅ Implemented SHAP explainability to translate model outputs into actionable business recommendations
- ✅ Structured repo for reproducibility: `.venv`, dual requirements, experiment tracking, data versioning, pre-commit hooks

---

## 📊 Dataset

| Source | Records | Features | Target |
|--------|---------|----------|--------|
| [Credit Card Customer Churn (Kaggle)](https://www.kaggle.com/datasets/sakshigoyal7/credit-card-customers) | ~10,127 | 20+ (demographic, transactional, behavioral) | `Attrition_Flag` (Binary) |

🔁 **Reproducibility: Get the Data**
```bash
# Option 1: Automated download (requires Kaggle API setup)
python scripts/download_data.py

# Option 2: Generate synthetic data for quick testing
python scripts/generate_synthetic_data.py --n_samples 10000
```

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Language** | Python 3.11 |
| **Core ML** | `scikit-learn`, `xgboost`, `shap`, `imbalanced-learn` |
| **Data/EDA** | `pandas`, `numpy`, `matplotlib`, `seaborn`, `plotly` |
| **BI/Reporting** | Power BI Desktop |
| **DevOps/Repro** | `venv`, `pre-commit`, `pytest`, Conventional Commits, Experiment Tracking |

---

## 🧭 Methodology & Architecture

```mermaid
graph LR
    A[Raw Data] --> B[Preprocessing & Validation]
    B --> C[Feature Engineering]
    C --> D[Model Training & CV]
    D --> E[Evaluation + SHAP Explainability]
    E --> F[Prioritization Tiers]
    F --> G[Power BI Dashboard]
    G --> H[Business Recommendations]
```

### 1. Data Preprocessing & Validation
- Dropped ID columns & Kaggle dataset artifacts
- Label-encoded categorical variables
- Handled class imbalance via `class_weight='balanced'` (SMOTE tested, excluded for production simplicity)
- Stratified 80/20 train-test split with feature scaling
- Added data validation gates (target presence, empty string checks, null audits)

### 2. Model Development
| Model | AUC | F1-Score | Precision | Recall |
|-------|-----|----------|-----------|--------|
| Logistic Regression (Baseline) | 0.78 | 0.65 | 0.68 | 0.62 |
| Random Forest | 0.84 | 0.73 | 0.75 | 0.71 |
| **XGBoost (Final)** | **0.87** | **0.79** | **0.81** | **0.77** |

- Hyperparameter tuning via `RandomizedSearchCV` (5-fold CV)
- Final params: `max_depth=5, learning_rate=0.1, n_estimators=150, scale_pos_weight=auto`

### 3. Model Explainability (SHAP)
- Global feature importance + local prediction explanations
- **Top 5 churn drivers identified:**
  1. `Total_Trans_Ct` ↓ → churn risk ↑
  2. `Total_Relationship_Count` ↓ → churn risk ↑
  3. `Months_Inactive_12_mon` ↑ → churn risk ↑
  4. `Contacts_Count_12_mon` ↑ → churn risk ↑
  5. `Credit_Limit` ↓ → churn risk ↑ (for high-spenders)

### 4. Prioritization Framework
Customers segmented by churn probability × business value:
| Tier | Churn Probability | Customer Value | Recommended Action |
|------|-------------------|----------------|---------------------|
| 🔴 **High Risk** | > 0.70 | Top 30% by spend | Personalized retention offer + dedicated outreach |
| 🟡 **Medium Risk** | 0.40–0.70 | Mid-tier | Automated nurture campaign + loyalty incentives |
| 🟢 **Low Risk** | < 0.40 | All | Monitor; no active intervention needed |

---

## 📈 Power BI Dashboard

<!-- ![Dashboard Preview](./assets/dashboard_preview.png) -->
*Screenshot placeholder: Replace with your actual `assets/dashboard_preview.png`*

**Key Visualizations:**
- Churn rate by demographic/behavioral segments
- Risk tier distribution (pie + bar charts)
- SHAP-based feature importance waterfall chart
- Interactive customer risk heatmap (filter by age, tenure, spend)
- `"Top 100 At-Risk Customers"` table for sales team export

---

## 🚀 How to Run

⏱️ **Expected Runtime:** ~6 mins on 8GB RAM | ~3 mins on Colab GPU

### Prerequisites
- Python **3.11** (recommended for XGBoost/SHAP compatibility)
- Windows 10 / macOS / Linux
- Google Colab (for experimentation) or local Jupyter

### Step-by-Step Setup
```bash
# 1. Clone & navigate
git clone https://github.com/beinganujchoudhary/Customer-Churn-Prediction-Retention-Prioritization.git
cd Customer-Churn-Prediction-Retention-Prioritization

# 2. Create & activate virtual environment
python -m venv .venv
.venv\Scripts\activate   # Windows
# source .venv/bin/activate  # macOS/Linux

# 3. Install dependencies
pip install -r requirements.in   # Human-readable spec
pip freeze > requirements.txt    # Machine-readable lock

# 4. Verify environment
jupyter notebook notebooks/00_environment_check.ipynb

# 5. Download or generate data
python scripts/download_data.py  # or generate_synthetic_data.py

# 6. Run notebooks end-to-end (recommended order)
jupyter notebook notebooks/
# → 01_eda.ipynb → 02_modeling.ipynb → 03_explainability.ipynb

# 7. (Optional) Export predictions for Power BI
python src/export_predictions.py --input data/processed/customers_v1.csv --output output/predictions.csv
```

> 💡 **Colab Workflow Tip:** Create notebooks locally, commit empty scaffolds to Git, then open via `File → Open notebook → GitHub` in Colab. Never use `files.upload()` for production reproducibility.

---

## 📁 Project Structure

```
Customer-Churn-Prediction-Retention-Prioritization/
├── .venv/                          # Virtual environment (gitignored)
├── data/
│   ├── raw/                        # Original datasets (gitignored)
│   └── processed/                  # Cleaned & versioned (e.g., customers_v1.csv)
├── notebooks/
│   ├── 00_environment_check.ipynb  # Dependency & version validation
│   ├── 01_eda.ipynb                # Exploratory Data Analysis
│   ├── 02_modeling.ipynb           # Training, CV, & hyperparameter tuning
│   └── 03_explainability.ipynb     # SHAP analysis & business translation
├── src/                            # Reusable Python modules
├── scripts/                        # Data download & synthetic generation
├── output/
│   ├── models/                     # Saved .pkl models
│   ├── plots/                      # High-DPI visualizations for README
│   └── predictions.csv             # Final risk scores & tiers
├── experiments/                    # Experiment tracking & iteration logs
├── assets/                         # Dashboard previews, architecture diagrams
├── tests/                          # Basic pipeline validation
├── requirements.in                 # Human-readable dependency spec
├── requirements.txt                # Machine-readable frozen dependencies
├── requirements-dev.txt            # Dev tooling (pytest, black, pre-commit)
├── .pre-commit-config.yaml         # Auto-clean notebook outputs
├── .gitignore
└── README.md
```

---

## 🧪 Experiment Tracking & Reproducibility

This project follows production MLOps practices:
- 📦 **Dependency Management:** `requirements.in` (flexible ranges) + `requirements.txt` (exact pins)
- 📊 **Data Versioning:** `data/processed/customers_v1.csv`, `v2`, etc. (never overwrite raw)
- 📝 **Experiment Logs:** Track iterations in `experiments/experiment_XX.md`
- 🔧 **Pre-commit Hooks:** Automatically strip notebook outputs before commits
- 📌 **Conventional Commits:** `feat(core):`, `fix(data):`, `docs(readme):`

---

## 🔑 Key Insights

1. **Behavioral > Demographic:** Transaction frequency and product engagement are stronger churn predictors than age/gender.
2. **Inactivity is a Leading Indicator:** Customers with 2+ months of inactivity have **3.2x higher churn probability**.
3. **High-Value ≠ Low-Risk:** Top spenders with declining engagement need proactive retention—don't assume loyalty.
4. **Explainability Drives Action:** SHAP visualizations helped non-technical stakeholders trust and act on model outputs.

---

## ⚠️ Limitations & Known Issues

| Area | Limitation | Mitigation |
|------|------------|------------|
| **Churn Definition** | Based on 90+ days inactivity; business definition may vary | Align with CRM event logging in production |
| **Class Imbalance** | Handled via `class_weight`; real-world may need cost-sensitive learning | Test SMOTE/ADASYN or threshold tuning |
| **Temporal Features** | No seasonality/campaign exposure included | Add time-series features for v2 |
| **Deployment** | Dashboard assumes weekly refresh; real-time scoring not implemented | Wrap in FastAPI + schedule Airflow jobs |

---

## 🔐 Data Privacy & Compliance
- All preprocessing includes PII obfuscation steps
- Pipeline designed with GDPR-compliant anonymisation principles
- No real customer data is stored or shared; synthetic data available for testing

---

## 🤝 Contributing

Contributions welcome! Please follow:
1. Fork the repo & create a feature branch (`git checkout -b feat/amazing-feature`)
2. Install dev tools: `pip install -r requirements-dev.txt`
3. Run tests: `pytest tests/`
4. Format code: `black src/ notebooks/`
5. Commit using Conventional Commits
6. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for details.

> 💡 **Pro Tip:** For best results, pair this model with a CRM integration to auto-trigger retention workflows for Tier 1 customers. Start with a simple CSV export before building APIs.

[⬆ Back to Top](#-customer-churn-prediction--retention-prioritization)
