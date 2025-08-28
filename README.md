📌 Project Overview

This project demonstrates a fraud detection system built on real-world financial transaction data. It covers the complete workflow from data cleaning → feature engineering → model training → evaluation → interpretability.

The model is trained using LightGBM, a gradient boosting algorithm well-suited for highly imbalanced datasets like fraud detection, where fraudulent cases make up less than 0.2% of all transactions.

📊 Dataset

Source: [Kaggle Dataset – Fraud.csv]

Size: ~6 million rows, 11 features

Key Features:

step → transaction day/time

type → transaction type (TRANSFER, CASH_OUT, etc.)

amount → transaction value

oldbalanceOrg, newbalanceOrg, oldbalanceDest, newbalanceDest → account balances before/after transactions

isFraud → target variable (1 = Fraud, 0 = Legit)

isFlaggedFraud → pre-existing rule-based flag (excluded to prevent leakage)

🔧 Data Preprocessing
1. Missing Values

Checked with .isnull().sum()

✅ No missing values found

2. Outliers:

-Large transaction amounts were observed.

-These are valid fraud indicators, not errors → kept in the dataset.

3. Multicollinearity

   -Strong correlation between:

   -oldbalanceOrg ↔ newbalanceOrg

   -oldbalanceDest ↔ newbalanceDest

Action Taken:

Engineered new features:

#orig_diff = oldbalanceOrg - newbalanceOrg - amount\
#dest_diff = oldbalanceDest - newbalanceDest + amount\
Dropped redundant balance columns to reduce leakage.\

4. Feature Engineering

  Dropped identifiers: nameOrig, nameDest

  Dropped low-signal features: step, isFlaggedFraud

 Encoded categorical feature type into numeric codes

