# Anti-Money Laundering (AML) Detection — Hackathon Project

<img width="1337" height="596" alt="image" src="https://github.com/user-attachments/assets/cccd4225-0886-44d2-b430-d348cede68f9" />

## Overview

This repository contains the solution I developed during a hackathon focused on **Anti-Money Laundering (AML)** — the process of detecting and preventing illicit financial activities where criminals disguise illegally obtained funds as legitimate income.

Money laundering is a **major challenge in modern banking and finance**, as institutions must comply with strict regulations and develop robust detection systems. The dataset provided simulates **transactional banking data** containing thousands of legitimate transactions and a small fraction of suspicious ones potentially linked to money laundering.

The main task:

> **Build a binary classification model to identify laundering transactions among legitimate ones.**

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

---

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

---

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

---

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

1. **Data Understanding & Cleaning**

   * Explored transaction distribution, missing values, and class imbalance
   * Preprocessed numerical and categorical features

2. **Feature Engineering**

   * Created meaningful ratios, time-based features, and aggregations
   * Encoded categorical variables

3. **Handling Class Imbalance**

   * Applied techniques such as class weights and oversampling

4. **Modeling**

   * Tested tree-based models (e.g., XGBoost, LightGBM, CatBoost)
   * Tuned hyperparameters with cross-validation

5. **Evaluation**

   * Implemented custom metric to match hackathon scoring
   * Focused on improving **Fraud Capture Rate** without sacrificing overall AUC

---

##  Repository Structure

```
 AML-Detection
 ┣  README.md               # Project documentation
 ┣  requirements.txt        # Python dependencies
 ┣  data_preprocessing.py   # Data cleaning & feature engineering
 ┣  model_training.py       # Model building & evaluation
 ┣  utils.py                 # Helper functions (metrics, loaders)
 ┗  notebooks               # Exploratory data analysis & experiments
```

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

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---
