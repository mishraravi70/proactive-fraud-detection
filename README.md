📌 Project Overview

This project demonstrates a fraud detection system built on real-world financial transaction data. It covers the complete workflow from data cleaning → feature engineering → model training → evaluation → interpretability.

The model is trained using LightGBM, a gradient boosting algorithm well-suited for highly imbalanced datasets like fraud detection, where fraudulent cases make up less than 0.2% of all transactions.

📊 Dataset

Source: [Kaggle Dataset – Fraud.csv]

Size: ~6 million rows, 11 features

# Key Features:

- step → transaction day/time

* type → transaction type (TRANSFER, CASH_OUT, etc.)

+ amount → transaction value

+ oldbalanceOrg, newbalanceOrg, oldbalanceDest, newbalanceDest → account balances before/after transactions

+ isFraud → target variable (1 = Fraud, 0 = Legit)

+ isFlaggedFraud → pre-existing rule-based flag (excluded to prevent leakage)

# Data Preprocessing
## 1. Missing Values

- Checked with .isnull().sum()

✅ No missing values found

## 2.Outliers:

- Large transaction amounts were observed.

- These are valid fraud indicators, not errors → kept in the dataset.

## 3.Multicollinearity

- Strong correlation between:

- oldbalanceOrg ↔ newbalanceOrg

- oldbalanceDest ↔ newbalanceDest

Action Taken:

Engineered new features:

#orig_diff = oldbalanceOrg - newbalanceOrg - amount\
#dest_diff = oldbalanceDest - newbalanceDest + amount\
Dropped redundant balance columns to reduce leakage.\

## 4.Feature Engineering

- Dropped identifiers: nameOrig, nameDest

- Dropped low-signal features: step, isFlaggedFraud

- Encoded categorical feature type into numeric codes\

# 🤖 Model: LightGBM
## Why LightGBM?

- Handles large-scale data efficiently

- Robust to class imbalance (using scale_pos_weight)

- Captures non-linear interactions

- Provides feature importance & SHAP explainability

## Training Setup

- Split: 80/20 (Stratified for fraud ratio)

- Objective: Binary classification (isFraud)

Metrics:

- ROC-AUC (ranking ability)

- Precision, Recall, F1-score (business interpretability)

- Confusion Matrix

- Precision-Recall Curve\

 #  📈 Results

- ROC-AUC: ~0.99 (excellent separability)

- Fraud Recall: ~71% (captures majority of fraud cases)

- Fraud Precision: ~47% (around half of flagged cases are actual fraud)

- False Positives reduced drastically (~1,290 vs ~27,000 initial)\

# Key Predictive Features

1. Transaction Type (TRANSFER, CASH_OUT dominate fraud cases)

2.Balance Consistency (orig_diff, dest_diff)

3.Transaction Amount (extreme/high amounts often fraud)

4.New Destinations (fresh accounts often suspicious)\

# Explainability

- Feature importance plots

- SHAP summary & force plots\

# 🛡️ Fraud Prevention Recommendations

## 1.Transaction Controls

 - Risk-based authentication (OTP, biometric for high-risk transfers)

 - Dynamic limits & cooling-off for new payees

 - Recipient reputation scoring

## 2.Behavioral profiling

- Model & Infrastructure

- Two-stage scoring (fast pre-screen + LightGBM for risky cases)

- Real-time features & velocity checks

- Continuous retraining with feedback loops
