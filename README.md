# Bank Transaction Fraud Detection

An anomaly detection project on bank transaction data, using unsupervised machine learning to flag transactions that behave differently from typical account activity.

## Overview

This project explores a bank transaction dataset and identifies transactions that look unusual enough to warrant review. The dataset does not include a fraud label, so the project is framed as **unsupervised anomaly detection** rather than supervised fraud classification — a transaction flagged as anomalous is not automatically fraud, but it is a reasonable candidate for further investigation.

**Dataset:** [Bank Transaction Dataset for Fraud Detection](https://www.kaggle.com/datasets/valakhorasani/bank-transaction-dataset-for-fraud-detection) (Kaggle) — 2,512 synthetic transactions across 16 original features.

## Project Workflow

1. **Data Preview & Quality Checks** — inspected structure, checked for missing values, duplicates, and outliers (IQR method)
2. **Exploratory Data Analysis** — distribution analysis, skewness checks, and correlation heatmap across numerical and categorical features
3. **Feature Engineering** — derived behavior-based features per account, including:
   - Time-based features (transaction hour, day of week, hours since previous transaction)
   - Amount deviation from the account's own historical average (z-score)
   - New device / new location flags
   - Transaction amount to account balance ratio
4. **Anomaly Detection** — applied and compared two unsupervised models:
   - **Isolation Forest** (primary model)
   - **DBSCAN** (secondary, density-based comparison)
   - PCA used to visualize flagged anomalies in 2D
5. **Synthetic Fraud Validation** — since no real fraud labels exist, synthetic fraud-like transactions were injected (inflated amount, higher login attempts, new device/location) to test whether the model actually catches fraud-shaped behavior
6. **Result Interpretation** — profiled what distinguishes flagged transactions from normal ones, and ranked the top 1% riskiest transactions by anomaly score

## Key Results

| Metric | Value |
|---|---|
| Transactions flagged by Isolation Forest | 76 / 2,512 (~3%) |
| Transactions flagged by DBSCAN | 157 / 2,512 |
| Synthetic fraud recall | 0.68 |
| Synthetic fraud precision | 0.44 |
| ROC-AUC (synthetic fraud validation) | 0.98 |

Transactions flagged as anomalies tend to show a higher deviation from the account's typical spending amount, more login attempts, longer transaction duration, and a higher amount-to-balance ratio compared to normal transactions.

## Limitations

- The dataset has no confirmed fraud labels; validation relies on synthetic fraud injection rather than real fraud cases.
- The `contamination=0.03` parameter used in Isolation Forest is an assumption, not a measured fraud rate.
- The dataset is relatively small (2,512 rows), which may limit how well results generalize.
- Isolation Forest and DBSCAN do not fully agree on which transactions are anomalous, reflecting that "anomaly" is not a single fixed definition.

## Tech Stack

- Python, pandas, NumPy
- scikit-learn (IsolationForest, DBSCAN, PCA, StandardScaler)
- matplotlib, seaborn

## Project Structure

```
Bank Fraud Detection/
├── dataset/
│   └── bank_transactions_data.csv
├── ml/
│   └── fraud_detection.ipynb
└── README.md
```

## How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/your-username/bank-fraud-detection.git
   cd bank-fraud-detection
   ```
2. Install dependencies
   ```bash
   pip install pandas numpy matplotlib seaborn scikit-learn
   ```
3. Open `ml/fraud_detection.ipynb` in Jupyter Notebook or VS Code and run all cells.

## Possible Next Steps

- Validate against a dataset with real, confirmed fraud labels
- Compare additional anomaly detection methods (Local Outlier Factor, autoencoder)
- Test on a larger dataset to check whether patterns hold at scale
- Build a dashboard to present flagged transactions for non-technical review
