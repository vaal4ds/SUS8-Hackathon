# Anti-Money Laundering (AML) Detection — Hackathon Project

<img width="1337" height="596" alt="image" src="https://github.com/user-attachments/assets/cccd4225-0886-44d2-b430-d348cede68f9" />

## Overview

This repository contains the solution my team and I developed during the $8^{th}$ edition of [Stat Under the Stars](https://sis2025.sis-statistica.it/sus-2025/) hackathon, which focused on **Anti-Money Laundering (AML)** — the process of detecting and preventing illicit financial activities where criminals disguise illegally obtained funds as legitimate income.

Money laundering is a **major challenge in modern banking and finance**, as institutions must comply with strict regulations and develop robust detection systems. The dataset provided simulates **transactional banking data** containing thousands of legitimate transactions and a small fraction of suspicious ones potentially linked to money laundering.

The main task:

> **Build a binary classification model to identify laundering transactions among legitimate ones.**


---

## Dataset

The dataset contains **anonymized banking transactions** aimed at detecting suspicious behavior possibly linked to money laundering.

**Files:**

* **`sus8_train.csv`** — 55,307 labeled transactions (training set)
* **`sus8_test.csv`** — 23,743 unlabeled transactions (test set)
* **`submission.txt`** — Team LVRS Submission 

**Key Features:**

* **From Account / To Account** — Anonymized sender/receiver IDs
* **Payment Type** — Transaction type (e.g., `"Credit"`, `"Cheque"`)
* **Amount Paid** — Transaction amount
* **Type Account From / To** — Account types (`A`–`F`)
* **Avg Stock From / To** — Avg. account balance in last 30 days
* **Is Laundering** — Target label (1 = laundering, 0 = legitimate) — *train set only*

The dataset is highly **imbalanced** and may contain noise, missing values, and complex interdependencies, reflecting real-world AML detection challenges.


---

## Problem Statement

We are given a dataset containing banking transaction records.
Among a **heavily imbalanced dataset**, only a **small proportion** of transactions are related to money laundering.

**Objective:** Develop a machine learning model that can **accurately distinguish** between normal and laundering transactions while considering the severe class imbalance.

---

##  Evaluation Metrics

To assess model performance fairly and effectively, the hackathon defined a **custom evaluation metric** combining three components:

### 1. **AUC (Area Under the ROC Curve)**

* Measures the model’s ability to distinguish between fraudulent and non-fraudulent transactions.

### 2. **Balanced Accuracy**

Ensures good performance on both classes, even with imbalance:

$$
\text{Balanced Accuracy} = \frac{TPR + TNR}{2}
$$

Where:

* **TPR (True Positive Rate)** = TP / (TP + FN)
* **TNR (True Negative Rate)** = TN / (TN + FP)
* **TP**: Fraud correctly classified
* **FN**: Fraud missed
* **TN**: Legitimate correctly classified
* **FP**: Legitimate misclassified as fraud


### 3. **Fraud Capture Rate (Top-N Predictions)**

Focuses on **catching the most suspicious cases**:

$$
\text{Fraud Capture Rate} = \frac{\sum_{i \in T_{485}} y_i}{\sum_{i \in T} y_i}
$$

Where:

* **N = 485**: Number of transactions selected for manual review
* $T_{485}$: Indices of the top-485 highest fraud probability predictions
* $y_i$: True label of transaction *i* (1 = fraud, 0 = legitimate)
* $T$: Total set of fraudulent transactions

### **Final Score**

The overall score is the **arithmetic mean** of:

* **AUC**
* **Balanced Accuracy**
* **Fraud Capture Rate (Top 485)**

$$
\frac{AUC + BA + FCR}{3}
$$

---

##  Approach

Our approach followed these steps:

### 1. Data Exploration & Preprocessing

* Checked class imbalance → Fraud cases were a small minority.
* Encoded categorical features (`Payment Type`, account types) using **target encoding** to preserve ordinal relationships without expanding dimensionality excessively.
* Scaled numeric features (`Amount Paid`, `Avg Stock From/To`) using **RobustScaler** to reduce the impact of extreme values.
* Addressed imbalance with **class weighting** rather than oversampling to avoid synthetic noise.

### 2. Model Selection & Justification

* **Gradient Boosting (LightGBM, CatBoost, XGBoost)**

  * Tree-based methods handle **mixed categorical/numeric data** well.
  * Resistant to scaling issues and can capture **non-linear interactions** between features.
  * CatBoost was particularly useful as it handles categorical features natively and reduces preprocessing overhead.

* **Logistic Regression (with regularization)**

  * Provided a **baseline linear model** for interpretability.
  * Useful for understanding feature importance and correlation patterns.

* **Random Forest**

  * Added as a **robust ensemble baseline**, though slower than boosting methods.
  * Good for checking consistency of feature importance across models.


### 3. Model Tuning & Evaluation

* Used **stratified cross-validation** to preserve class ratios in splits.
* Hyperparameter tuning was done with **Optuna** — a fast, flexible optimization framework that efficiently explores parameter space using **Tree-structured Parzen Estimators (TPE)**.

  * Chosen over grid/random search because it **finds better parameters with fewer trials**, which is critical for faster iterations on large datasets.
  * Search objective focused on **maximizing AUC** while monitoring Balanced Accuracy and Top-485 Capture Rate as secondary metrics.
* Evaluated all models not only on AUC but also **Balanced Accuracy** and **Fraud Capture Rate (Top-485)** to ensure rare fraud cases were ranked at the top.

### 4. Final Submission Strategy

* **Model ensembling** of CatBoost, LightGBM, and Logistic Regression improved stability and reduced variance in predictions.
* Final predictions were ranked by fraud probability to optimize the **Fraud Capture Rate** metric.

---

## Results

| Metric                 | Score |
| ---------------------- | ----- |
| **AUC**                | ?  |
| **Balanced Accuracy**  | ?  |
| **Fraud Capture Rate** | ?  |
| **Final Score**        | 0.76594 |

---

##  Hackathon Insights

* **Imbalanced datasets** require careful metric choice; accuracy alone is misleading.
* **Top-N prioritization** aligns well with real-world AML processes where only a subset of transactions can be manually reviewed.
* Feature engineering from **transactional patterns** is often more valuable than raw model complexity.

## Team Members 

* **L**eonardo Rocci   
* **V**aleria Avino  
* **R**iccardo Soleo  


*( before the hackathon )*

<img width="228" height="105" alt="image" src="https://github.com/user-attachments/assets/fb0005be-238a-4a30-a6ee-3fdbbf7c88aa" />


*( ... during the hackathon )*

<img width="228" height="105" alt="image" src="https://github.com/user-attachments/assets/4cdff526-b9f4-4bdf-8ef3-5335014bf447" />

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---
