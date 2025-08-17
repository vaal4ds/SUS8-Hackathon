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
\text{Final Score } =\frac{AUC + BA + FCR}{3}
$$

---

# AML Detection Pipeline
We designed a stacked ensemble pipeline to detect rare fraudulent transactions by combining graph-based features with powerful gradient boosting methods.

## Approach

Our approach followed these steps:

### Pipeline Overview

```mermaid
flowchart LR
    A[Raw Transaction Data] --> B[Graph Construction & Feature Engineering]
    B --> C[Base Models: LightGBM & CatBoost]
    C --> D[Meta Model: Logistic Regression]
    D --> E[Probability Calibration - Isotonic Regression]
    E --> F[Top-N Ranking for Fraud Detection]
```

### 1. Graph Construction & Feature Engineering

* Constructed **graph features** from transaction relationships to capture interactions between accounts.
* Extracted key **transactional patterns** that help differentiate normal vs. suspicious behavior.

### 2. Base Models

* **LightGBM & CatBoost**:

  * Tree-based models capturing **non-linear interactions** in the graph-derived features.
  * CatBoost handles categorical features natively.

### 3. Meta Model

* **Logistic Regression**:

  * Combines predictions of base models to produce a more **stable and calibrated score**.
  * Acts as the stacking layer for ensemble predictions.

### 4. Probability Calibration

* **Isotonic Regression** used to **improve the ranking of fraud probabilities** for downstream Top-N evaluation.

### 5. Top-N Ranking for Fraud Detection

* Ranked transactions by calibrated probability.
* Optimized **fraud detection efficiency**, focusing on the subset of transactions most likely to be fraudulent.

## Hackathon Insights

* **Stacked ensembles improve stability and predictive performance** compared to single models.
* **Graph-based features provide significant value** in detecting suspicious transactions.
* **Top-N ranking aligns with real-world AML constraints**, where only a fraction of transactions can be manually reviewed.
* Probability calibration further enhances the accuracy of ranking the highest-risk transactions.

---

## Results

**Final Score** : 0.76594

---

## Team Members 

* **L**eonardo Rocci   
* **V**aleria Avino  
* **R**iccardo Soleo  


<img width="228" height="105" alt="image" src="https://github.com/user-attachments/assets/fb0005be-238a-4a30-a6ee-3fdbbf7c88aa" />

*( before the hackathon )*



<img width="228" height="105" alt="image" src="https://github.com/user-attachments/assets/4cdff526-b9f4-4bdf-8ef3-5335014bf447" />

*( ... at 4 a.m., during the hackathon )*


---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---
