# 🤠 Texas State Salary Prediction

**Capstone Project 4**: comparing regression models to predict annual salary for Texas state government employees.

![Python](https://img.shields.io/badge/Python-3.x-blue) ![Scikit-learn](https://img.shields.io/badge/scikit--learn-ML-orange) ![XGBoost](https://img.shields.io/badge/XGBoost-boosting-green)

---

## 📌 Overview

This project uses salary records from all **113 agencies** of the Texas state government (**149,481 employee records**) to predict **Annual Salary**. Four regression models are built, tuned and compared using **R²** and **MSE**.

## 📂 Dataset

- **Source:** Texas Tribune's state salary database, obtained from the Texas State Comptroller under the **Texas Public Information Act**.
- **Size:** 149,481 rows, 14 columns after cleaning.
- **Key columns:** Agency, Class Title, Ethnicity, Gender, Status, Employ Date, Hourly Rate, Hours per Week, Monthly, Annual.
- **Note:** The dataset is not included in this repository. Download it separately and place it as `salary.csv` (the notebook reads `/content/salary.csv`, a Google Colab path, so update it if running locally).

## 🔍 Methodology

1. **Data preparation:** dropped irrelevant / duplicate-flag columns, checked for nulls, split data 80/20 into train and test sets.
2. **Exploratory Data Analysis:** gender, ethnicity, employment status, agency payroll, correlation heatmap, salary distribution.
3. **Model building:** Linear Regression, Decision Tree, Random Forest, XGBoost.
4. **Hyperparameter tuning:** `GridSearchCV` on XGBoost (learning rate, max depth, n_estimators, subsample, colsample_bytree).
5. **Evaluation:** R² (fit quality) and MSE (prediction error) on unseen test data.

## 📊 EDA Highlights

- **Gender:** 57.1% female, 42.9% male.
- **Ethnicity:** White (44.9%), Hispanic (27.2%), Black (24.0%), Asian (2.9%), Other (0.6%), American Indian (0.5%).
- **Status:** about 95% are *Classified Regular Full-Time* (142,502 employees).
- **Salary distribution:** heavily right-skewed (skewness 2.70, kurtosis 14.03), meaning a few high earners stretch the upper range.
- **Correlation:** Monthly vs Annual salary forms a near-perfect straight line.
- **Payroll:** Health and Human Services Commission and Texas Dept. of Criminal Justice have the highest total payroll.

## 🏆 Results

Models trained on `MONTHLY` → `ANNUAL` (80/20 split, `random_state=42`):

| Model | Train R² | Test R² | Test MSE |
|---|---|---|---|
| **Linear Regression** | 1.000 | **1.000** | **~2.5e-22** |
| Random Forest (depth 5) | 0.998 | 0.995 | ~3.0 million |
| Decision Tree (depth 5) | 0.997 | 0.995 | ~3.1 million |
| XGBoost (tuned, GridSearchCV) | 0.992 | 0.982 | ~11.4 million |

**Best XGBoost parameters:** `learning_rate=0.2`, `max_depth=7`, `n_estimators=200`, `subsample=0.8`, `colsample_bytree=1.0`

## 💡 Key Takeaways

- **The simplest model won.** Annual salary is essentially monthly salary × 12, so the relationship is perfectly linear, and tree-based models only added error.
- **Complexity ≠ accuracy.** Even a tuned XGBoost could not beat a plain linear baseline on this data.
- **A perfect score is a red flag.** An R² of 1.0 signals that the input feature effectively contains the target (data leakage), so the result says more about the data than about the model.

## 🚀 Future Work

- Predict salary **without** the `MONTHLY` column, using job class, agency, tenure and status, for a more realistic problem.
- Feature engineering on employment date (years of service).
- Try regularized models and cross-validated comparison across all features.

## 🛠️ Tech Stack

Python · Pandas · NumPy · Scikit-learn · XGBoost · Matplotlib · Seaborn · SciPy · Google Colab


