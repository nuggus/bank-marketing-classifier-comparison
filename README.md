# Practical Application III: Comparing Classifiers

**Notebook**: [bank-marketing-classifier-comparision.ipynb](bank-marketing-classifier-comparision.ipynb)
**Author**: Viswanath Nuggu 
**Dataset**: [UCI Bank Marketing Dataset](https://archive.ics.uci.edu/ml/datasets/bank+marketing) — 41,188 records, 20 features

---

## Business Problem

A Portuguese bank needs to identify which clients are likely to subscribe to a term deposit product before making an outbound telemarketing call. With only ~11.3% of contacts resulting in a subscription, a predictive model can dramatically reduce wasted calls and improve campaign ROI.

---

## Summary of Findings

### Data
- **41,188 client contacts** from 17 campaigns (May 2008 – Nov 2010)
- **Class imbalance**: 88.7% "No", 11.3% "Yes"
- No missing values (NaN), but several categorical columns contain `"unknown"` entries
- `duration` dropped to prevent data leakage (it's only known after a call ends)

### Models Compared
All four classifiers were evaluated with default settings, then improved via GridSearchCV (5-fold stratified CV, optimizing ROC-AUC):

| Model | Default ROC-AUC | Tuned ROC-AUC | Tuned F1 |
|---|---|---|---|
| Logistic Regression | ~0.77 | **~0.79** | ~0.47 |
| KNN | ~0.71 | ~0.74 | ~0.43 |
| Decision Tree | ~0.60 | ~0.76 | ~0.47 |
| SVM | ~0.77 | **~0.79** | ~0.47 |

> Primary metric: **ROC-AUC** (best for imbalanced classification)

### Top Predictors
1. **euribor3m** (3-month interest rate) — strongest economic signal; lower rates → more subscriptions
2. **poutcome_success** — clients who subscribed in a prior campaign are far more likely to again
3. **nr.employed / emp.var.rate** — macroeconomic climate drives subscription behavior
4. **contact_telephone** — cellular outreach outperforms telephone
5. **month** — March, September, October, and December show highest conversion rates

---

## Key Recommendations (Non-Technical)

1. **Re-target prior subscribers first** — Previous success is the single strongest behavioral predictor
2. **Time campaigns during low interest-rate environments** — Economic conditions significantly affect willingness to subscribe
3. **Prioritize cellular outreach** — Higher conversion rates than landline telephone
4. **Focus on retirees and students** — These job categories show the highest subscription rates
5. **Limit calls per client** — More than 2–3 contacts per campaign rarely converts new subscribers
6. **Intensify campaigns in March, Sept, Oct, Dec** — These months show peak subscription rates

---

## Next Steps

- Test ensemble methods (Random Forest, XGBoost) which typically outperform single classifiers
- Apply SMOTE oversampling to better handle class imbalance
- Optimize classification threshold (move from 0.50) using a business cost function
- Deploy best model as a lead-scoring API to rank clients before each campaign

---

## Project Structure

```
comparing classifiers/
├── README.md                          ← This file
├── bank-marketing-classifier-comparision.ipynb         ← Main analysis notebook
├── data/
│   ├── bank-additional-full.csv       ← Full dataset (41,188 rows)
│   ├── bank-additional.csv            ← Sample dataset
│   └── bank-additional-names.txt      ← Feature descriptions
└── CRISP-DM-BANK.pdf                  ← Accompanying research paper
```
