<h1 align="center">🏦 Bank Transaction Fraud Detection</h1>

<p align="center">
  Unsupervised anomaly detection on bank transaction data, engineering account-level behavioral features and validating results through synthetic fraud injection.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas" />
  <img src="https://img.shields.io/badge/scikit--learn-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white" alt="scikit-learn" />
  <img src="https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white" alt="Matplotlib" />
</p>

<br>

## 📑 Table of Contents

- [✨ Features](#-features)
- [📊 Model Performance](#-model-performance)
- [🛠️ Tech Stack](#️-tech-stack)
- [📂 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
- [🗄️ Dataset](#️-dataset)
- [🤝 Contributing](#-contributing)
- [🐛 Issues](#-issues)

---

## ✨ Features

### 🔎 Exploratory Data Analysis

- **Data Quality Checks** — missing value, duplicate, and outlier detection (IQR method) across the full dataset
- **Distribution Analysis** — numerical and categorical feature distributions, with skewness scoring to flag heavily imbalanced features
- **Correlation Heatmap** — pairwise correlation across numerical features to catch redundancy before modeling

### 🧠 Behavioral Feature Engineering

- **Account-relative signals** — amount deviation from each account's own historical average (z-score), not a flat dataset-wide threshold
- **New device / new location flags** — first-seen detection per account, a common real-world fraud signal
- **Amount-to-balance ratio** — flags transactions that consume most or all of an account's available balance

### 🚨 Anomaly Detection

- **Isolation Forest** — primary anomaly detection model, scoring every transaction by how easily it separates from the rest of the data
- **DBSCAN** — secondary, density-based model used for comparison against Isolation Forest
- **PCA Visualization** — 2D projection of flagged anomalies for visual inspection
- **Risk-Ranked Output** — transactions ranked by anomaly score rather than a flat yes/no label, surfacing the top 1% highest-risk cases

---

## 📊 Model Performance

Isolation Forest and DBSCAN applied to 2,512 transactions, validated using 50 injected synthetic fraud transactions (since the source dataset has no real fraud labels).

| Metric | Score |
|---|---|
| Transactions flagged (Isolation Forest) | 76 / 2,512 (~3%) |
| Transactions flagged (DBSCAN) | 157 / 2,512 |
| Synthetic fraud recall | 0.68 |
| Synthetic fraud precision | 0.44 |
| ROC-AUC (synthetic fraud validation) | 0.98 |

| Class | Precision | Recall | F1-score |
|---|---|---|---|
| Normal | 0.99 | 0.98 | 0.99 |
| Synthetic Fraud | 0.44 | 0.68 | 0.54 |

Flagged anomalies show a higher amount z-score, more login attempts, longer transaction duration, and a higher amount-to-balance ratio compared to normal transactions. Full breakdown available in `ml/fraud_detection.ipynb`.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Modeling | scikit-learn (IsolationForest, DBSCAN, PCA, StandardScaler) |
| Data handling | Pandas, NumPy |
| Visualization | Matplotlib, Seaborn |
| Notebook | Jupyter (`fraud_detection.ipynb`) |
| Key packages | `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn` |

---

## 📂 Project Structure

```
Bank Fraud Detection/
├── ml/
│   └── fraud_detection.ipynb       # Main analysis and modeling notebook
│
├── dataset/
│   └── bank_transactions_data.csv  # Source transaction dataset
│
├── image/
│   └── notebook_screenshot.png     # Notebook preview image
│
├── README.md
└── requirements.txt
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- `pip`

### 1. Clone the repo

```bash
git clone https://github.com/<your-username>/bank-fraud-detection.git
cd bank-fraud-detection
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the notebook

```bash
jupyter notebook ml/fraud_detection.ipynb
```

Run all cells from top to bottom. The notebook covers data quality checks, EDA, feature engineering, anomaly detection modeling, synthetic fraud validation, and result interpretation, in that order.

---

## 🗄️ Dataset

The project uses the [Bank Transaction Dataset for Fraud Detection](https://www.kaggle.com/datasets/valakhorasani/bank-transaction-dataset-for-fraud-detection) from Kaggle — 2,512 synthetic transactions across 16 features, including transaction amount, type, channel, device, location, and login attempts. The dataset does not include a confirmed fraud label, which is why this project is framed as anomaly detection rather than supervised fraud classification, with synthetic fraud injection used for validation instead.

---

## 🤝 Contributing

This started as a solo student portfolio project, but improvements are always welcome. Here's how to contribute:

1. **Fork** the repository.
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes.** Analysis and modeling logic lives in `ml/fraud_detection.ipynb`.
4. **Commit your changes**
   ```bash
   git commit -m "Add: short description of your change"
   ```
5. **Push to your branch**
   ```bash
   git push origin feature/your-feature-name
   ```
6. **Open a pull request** describing what you changed and why.

Since this project doesn't have an automated test suite yet, please manually verify your change by re-running the relevant notebook section before opening a PR, and mention what you tested in the PR description.

---

## 🐛 Issues

Found a bug, or something not working as expected? Check the [Issues](https://github.com/<your-username>/bank-fraud-detection/issues) page first to see if it's already been reported.

If not, feel free to open a new issue. To help track it down quickly, please include:

- A clear, descriptive title (e.g. "IsolationForest fails on missing feature column")
- Steps to reproduce the issue
- What you expected to happen vs. what actually happened
- Relevant error output/traceback from the notebook
- Which section of the notebook it occurs in (EDA, feature engineering, modeling, or validation)
