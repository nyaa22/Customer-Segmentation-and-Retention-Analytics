# Customer Segmentation & Retention Analytics

Retention budgets are wasted when every customer gets the same offer. This project answers two questions a business needs before spending on retention: **which customers are most likely to leave**, and **which of them are actually worth saving**.

It combines churn prediction (machine learning) with RFM-based customer segmentation to find the overlap: high-value customers who are showing signs of disengagement.

## Key Results

| Metric | Value |
| --- | --- |
| Churn model ROC-AUC | **0.83** |
| Churn model accuracy | **79%** |
| Customers in churn dataset | 7,043 |
| Transactions analysed (after cleaning) | 397,884 (from 541,909 raw) |
| Customers segmented | 4,338 |
| Behavioural segments (K-Means) | 4 |
| High-value at-risk customers identified | **223** |

**Business takeaway:** instead of sending retention offers to every customer, focus spend on the 223 customers who are both high-value and at risk. That improves retention ROI and protects the revenue that matters most.

## Datasets

| Part | Dataset | Source |
| --- | --- | --- |
| Churn prediction | Telco Customer Churn (7,043 customers, 21 features: demographics, contract, services, billing) | [Kaggle – IBM sample data](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) |
| Segmentation | Online Retail (541,909 transactions from a UK online retailer) | [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/352/online+retail) |

The Telco CSV is included in `data/`. The Online Retail file (~45 MB) is not committed, so download it from UCI and save it as `OnlineRetail.csv` before running Part 2.

## Methodology

### Part 1: Churn Prediction
1. **Cleaning:** converted `TotalCharges` to numeric, imputed missing values with the median, dropped the ID column.
2. **Encoding:** label-encoded categorical variables (contract type, payment method, services, etc.).
3. **Modelling:** trained a Random Forest classifier on an 80/20 train-test split.
4. **Evaluation:** ROC-AUC, precision, recall and F1 on the held-out set.
5. **Drivers:** feature importance analysis, with tenure, contract type and monthly charges among the strongest churn signals.

| Class | Precision | Recall | F1 |
| --- | --- | --- | --- |
| Retained (0) | 0.83 | 0.90 | 0.86 |
| Churned (1) | 0.63 | 0.49 | 0.55 |

### Part 2: RFM Segmentation
1. **Cleaning:** removed cancelled orders, missing customer IDs and non-positive quantities/prices (541,909 → 397,884 rows).
2. **RFM features:** for each customer, calculated **Recency** (days since last purchase), **Frequency** (number of orders) and **Monetary** (total spend).
3. **Clustering:** standardised RFM values and applied K-Means (k = 4).
4. **Labelling:** mapped customers to business-readable segments: *High Value Loyal*, *Frequent Buyers*, *At Risk* and *Low Engagement*.
5. **Targeting:** flagged *At Risk* customers with above-median spend as the priority retention list (223 customers).

## Recommendations

- **Prioritise high-value at-risk customers** with personalised win-back offers instead of blanket discounts.
- **Push month-to-month customers toward longer contracts**, since contract type is one of the strongest churn drivers.
- **Intervene early in the customer lifecycle**, because low-tenure customers churn most.
- **Reward High Value Loyal customers** with loyalty benefits to protect the revenue base.

## Limitations & Next Steps

- **Churn recall is 0.49:** the model catches about half of actual churners because the classes are imbalanced (~27% churn). Next step: class weights, SMOTE or threshold tuning to improve recall.
- **Two separate datasets:** churn and segmentation use different customer bases, so the at-risk list comes from RFM rules, not the ML model. In a real business, both would run on the same customer data.
- **Validation:** add silhouette scores / the elbow method to justify k = 4.

## Tools

Python · pandas · NumPy · scikit-learn (Random Forest, K-Means, StandardScaler) · matplotlib · seaborn · Google Colab

## Repository Structure

```
Customer-Segmentation-and-Retention-Analytics/
├── data/
│   └── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── notebook/
│   └── Customer_Segmentation_&_Retention_Analytics.ipynb
└── README.md
```

## How to Run

1. Clone the repository.
2. Download the Online Retail dataset from UCI and save it as `OnlineRetail.csv`.
3. Open the notebook in Jupyter or Google Colab and run all cells top to bottom.

## Author

**Ananya Jare**: [GitHub](https://github.com/nyaa22) · [LinkedIn](https://www.linkedin.com/in/ananya-jare-875433289)
