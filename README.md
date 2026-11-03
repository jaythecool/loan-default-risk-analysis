# 💳 Credit Default Risk Analysis

An exploratory data analysis (EDA) of a synthetic loan portfolio of **50,000 loans (~$908M in total exposure)**. The goal is to find out which borrower and loan characteristics drive defaults, then turn those findings into a simple, explainable risk scoring framework.

---

## 📌 Project Overview

Lenders need to know *who* is likely to default and *why*. This project walks through the full analytical workflow:

1. **Data cleaning**: handle missing values, duplicates and invalid ranges
2. **Portfolio KPIs**: size, exposure, and overall default rate
3. **Risk driver analysis**: default rates across credit, debt, employment, income, and loan characteristics
4. **Risk segmentation**: a rule-based points system that groups borrowers into Low, Medium and High risk

---

## 📂 Repository Structure

```
├── credit_risk_analysis.ipynb                # Main analysis notebook
├── credit_risk_loan_portfolio_synthetic.csv  # Dataset (synthetic)
└── README.md
```

---

## 🗃️ Dataset

Synthetic loan portfolio data with **28 columns** per loan, covering:

| Category | Fields |
|---|---|
| **Borrower** | `age`, `gender`, `annual_income`, `employment_type`, `employment_length_years`, `home_ownership`, `region` |
| **Credit profile** | `credit_score`, `existing_loans`, `delinquencies`, `previous_defaults`, `debt_to_income_ratio` |
| **Loan** | `loan_purpose`, `loan_amount`, `loan_term_months`, `interest_rate`, `monthly_payment`, `issue_date` |
| **Outcome** | `loan_status` (Paid/Current, Delinquent, Defaulted), `default_flag` (target) |
| **Pre-built segments** | `age_group`, `income_group`, `credit_score_band`, `dti_band` |

---

## 🧹 Data Cleaning

| Issue | Action |
|---|---|
| 75 duplicate rows (50,075 → 50,000) | Dropped |
| `employment_length_years`: 750 missing (1.5%) | Filled with median |
| `debt_to_income_ratio`: 601 missing (1.2%) | Filled with median |
| `home_ownership`: 501 missing (1.0%) | Filled with `"Unknown"` |
| `issue_date` stored as text | Converted to datetime |
| Range checks (age, credit score 300–850, income, loan amount, interest rate, DTI 0–1) | No invalid values found |

---

## 📊 Portfolio Snapshot

| Metric | Value |
|---|---|
| Total loans | 50,000 |
| Total loan amount | ~$907.9M |
| Average loan amount | ~$18,158 |
| Defaulted loans | 18,087 |
| **Overall default rate** | **36.17%** |
| Average credit score | 686.9 |
| Average interest rate | 10.11% |
| Average DTI | 22.96% |

---

## 🔍 Key Findings

### 1. Credit score is strongly tied to default
| Band | Default rate |
|---|---|
| Excellent | 21.96% |
| Very Good | 26.58% |
| Good | 33.32% |
| Fair | 43.61% |
| Poor | 51.78% |

### 2. Debt-to-income is the sharpest risk signal
Default rates climb from **25.6%** (Low DTI) to **39.8%** (Moderate), **58.6%** (High) and **85.8%** (Very High).

### 3. Past behavior predicts future behavior
- **Previous defaults:** 0 → 33.1% · 1 → 51.9% · 2 → 68.3%
- **Delinquencies:** 0 → 30.7% · 2 → 58.5% · 4 → 74.5%

### 4. Credit score and DTI compound each other
In the credit score × DTI matrix, the lowest default rate is **13.7%** (Excellent credit, Low DTI). Poor credit with Very High DTI reaches **100%**, though some of these cells hold few loans.

### 5. Other drivers
- **Employment:** Unemployed borrowers default at **53.3%**, versus ~35% for Salaried, Contract and Self-Employed.
- **Income:** The lowest income group (<$30K) has the highest default rate at 42.4%.
- **Loan purpose:** Business loans are the riskiest (41.9%). Other purposes sit between 34% and 38%.
- **Loan-to-income:** Default rates rise from 33.0% (<10%) to 43.4% (50–100%).
- **Loan size:** Very large loans (50K+) default at 44.1%, and the smallest (<5K) at 40.3%.

---

## 🎯 Risk Scoring Framework

A simple, transparent points system built from three drivers:

| Component | Points |
|---|---|
| **Credit score band** | Excellent 0 · Very Good 1 · Good 2 · Fair 3 · Poor 4 |
| **DTI band** | Low 0 · Moderate 1 · High 2 · Very High 3 |
| **Previous defaults** | 1 point per default (capped at 3) |

**Segments:** `0–2` → Low Risk · `3–5` → Medium Risk · `6+` → High Risk

| Segment | Loans | Default rate | Exposure | Share of all defaults |
|---|---|---|---|---|
| 🟢 Low | 16,039 | 21.14% | $277.4M | 18.7% |
| 🟡 Medium | 32,985 | 42.31% | $610.4M | 77.2% |
| 🔴 High | 976 | 75.82% | $20.0M | 4.1% |

The segments separate risk clearly (21% → 42% → 76%). However, the Medium segment holds about two-thirds of the loans and 77% of defaults, so it could be split further in future work.

> **Note:** The `risk_category` column that ships with the dataset labels almost every loan (49,742 of 50,000) as High Risk, so it is not useful for segmentation. This is why a custom scoring system was built.

---

## 🛠️ Tech Stack

- **Python 3**
- **pandas**, **NumPy**: data cleaning and aggregation
- **Matplotlib**, **Seaborn**: visualization
- **Jupyter Notebook**

---

## 🚀 Getting Started

```bash
# 1. Clone the repository
git clone https://github.com/jaythecool/loan-default-risk-analysis.git

# 2. Install dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Launch the notebook
jupyter notebook credit_risk_analysis.ipynb
```

Make sure `credit_risk_loan_portfolio_synthetic.csv` is in the same folder as the notebook.

---

## 🔮 Possible Next Steps

- Build a predictive model (logistic regression, gradient boosting) and compare it with the points system
- Evaluate with AUC/KS and a confusion matrix, since the classes are imbalanced (36% defaults)
- Refine the Medium-risk segment into finer tiers
- Analyze default trends over time using `issue_date`
- Add risk-based pricing analysis (interest rate vs. observed default rate)

---

## ⚠️ Disclaimer

This project uses **synthetic data** for learning and demonstration purposes. Findings should not be used for real lending decisions.

---

## 👤 Author

**Jayesh Vengurlekar**
[LinkedIn](https://www.linkedin.com/in/jayesh-vengurlekar-80309214b/) · [GitHub](https://github.com/jaythecool) ·
