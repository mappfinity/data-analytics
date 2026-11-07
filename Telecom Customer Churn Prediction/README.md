# Telecom Customer Churn: from raw data to a retention decision

An end-to-end data analyst / data scientist notebook that answers one business question: **who is leaving, why, and what is the most profitable way to keep them?**

## Dataset

**Telecom Customer Churn by Maven Analytics**, published on Kaggle:
<https://www.kaggle.com/datasets/shilongzhuang/telecom-customer-churn-by-maven-analytics>

The data is not included in this repository. Download it from the link above (see the dataset page for its terms of use) and place the three files in a `data/` folder:

| File | Content |
|---|---|
| `telecom_customer_churn.csv` | 7,043 California customers, status at the end of Q2 2022 (38 columns) |
| `telecom_zipcode_population.csv` | Population per zip code |
| `telecom_data_dictionary.csv` | Field descriptions |

## What the notebook covers

- **Data work:** safe parsing, validated joins, assertion-based data contract, null and outlier audit, leakage audit
- **Analysis:** churn rates with confidence intervals, chi-square / Mann-Whitney tests ranked by effect size, churn-reason Pareto
- **Survival analysis:** Kaplan-Meier, log-rank test, Cox regression stratified by contract
- **Modelling:** baselines, pipelines, cross-validation, tuning (logistic regression, gradient boosting), PR-AUC, lift, calibration, bootstrap comparison, feature ablation
- **Interpretation:** odds ratios, permutation importance, partial dependence, per-customer explanations, fairness slices
- **Segmentation:** K-Means segments and decision-tree risk rules
- **Decision layer:** expected-value targeting, profit curve, sensitivity analysis, BI-ready scored CSV

## Key results

- **26.5%** of customers churned during the quarter
- Month-to-month customers churn at **46%** (two-year: 3%); month-to-month Fiber customers are 27% of the base but **59% of churners**
- Held-out test set: **ROC-AUC 0.904**, **PR-AUC 0.771**; the top 20% of customers by risk contain **59% of churners**
- A positive-expected-value contact policy nets about **$36k** on the test set versus **$28k** when contacting everyone (campaign cost, save rate and margin are illustrative assumptions)

## Data traps handled

- Dropping `Joined` customers makes "tenure ≤ 3 months" a perfect churn predictor. They are kept as retained customers.
- `pandas` silently converts the text `"None"` in `Offer` to a missing value. The notebook parses it explicitly.

## Run it

```bash
pip install pandas numpy scipy scikit-learn matplotlib seaborn lifelines jupyter
jupyter notebook telecom_churn_portfolio.ipynb
```

Python ≥ 3.10. The notebook runs in about 1.5 minutes and writes `outputs/scored_customers.csv`.

## Limitations

Single snapshot (no history before churn), correlation rather than causation, and illustrative campaign economics. Section 11 of the notebook discusses these and next steps.
