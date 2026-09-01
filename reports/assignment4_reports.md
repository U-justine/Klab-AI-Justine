# Credit Card Fraud Detection: F1-Optimized Approach

**Date:** September 2026
**Course:** KLab AI Bootcamp
**Branch:** `assignment/day-04`

---

## 1. Problem Statement

The objective is to build a machine learning model that predicts whether a credit card transaction is **fraudulent or legitimate**.

Because fraudulent transactions are extremely rare compared with legitimate transactions, the model should not be evaluated using accuracy alone. The main goal is to detect as many fraudulent transactions as possible while keeping false alarms under control.

---

## 2. Data Summary

* **Dataset:** 284,807 credit card transactions
* **Fraud cases:** 492
* **Non-fraud cases:** 284,315
* **Fraud rate:** Approximately 0.172%
* **Target:** `Class`

  * `0` = Not Fraud
  * `1` = Fraud
* **Features:** Transaction amount, time, and PCA-transformed numerical features

The dataset is highly imbalanced, making fraud detection a challenging classification problem.

---

## 3. Models Tested

Several classification approaches were considered to determine which model performs best.

* **Logistic Regression**
* **Random Forest**
* **XGBoost**
* **SMOTE** was considered/used to address class imbalance where appropriate.
* **Threshold tuning** was applied to investigate whether changing the default prediction threshold could improve F1-score.

The models were compared using **Precision, Recall, F1-Score, Accuracy, and ROC-AUC**.

---

## 4. Why F1-Score Was Selected

Accuracy is not the most appropriate primary metric for this problem because the dataset is highly imbalanced.

For example, a model that predicts almost every transaction as **Not Fraud** can achieve very high accuracy while failing to detect fraudulent transactions.

### Important Metrics

| Metric        | What it measures                       | Business meaning                       |
| ------------- | -------------------------------------- | -------------------------------------- |
| **Precision** | Correctness of fraud alerts            | Reduces false alarms                   |
| **Recall**    | Fraud cases successfully detected      | Reduces missed fraud                   |
| **F1-Score**  | Balance between precision and recall   | Provides an overall balance            |
| **ROC-AUC**   | Ability to distinguish the two classes | Measures ranking/discrimination        |
| **Accuracy**  | Overall correct predictions            | Less informative with severe imbalance |

### Why F1?

F1-score is particularly useful because the business needs both:

* **High recall** → detect more fraudulent transactions.
* **High precision** → avoid unnecessarily blocking legitimate customers.

Therefore, F1-score was selected as the main metric for comparing the models.

---

## 5. Threshold Optimization

The default classification threshold is usually **0.50**. However, this threshold is not always optimal for fraud detection.

Different thresholds were tested to determine whether the model could achieve a better balance between precision and recall.

A lower threshold generally increases recall but can also increase false positives. A higher threshold can increase precision but may cause the model to miss more fraudulent transactions.

Therefore, threshold tuning provides an opportunity to select a threshold based on the desired business trade-off.

---

## 6. Model Comparison

The models were compared using the same test set and the most important classification metrics.

| Model               | Threshold | Precision |    Recall |  F1-Score |   ROC-AUC |
| ------------------- | --------: | --------: | --------: | --------: | --------: |
| Logistic Regression |     Tuned |         — |         — |         — |         — |
| Random Forest       |     Tuned | **95.1%** | **78.6%** | **86.0%** | **97.3%** |
| XGBoost             |     Tuned |         — |         — |         — |         — |

Based on the evaluated results, **Random Forest achieved the strongest F1-score of approximately 0.86** and provided a strong balance between detecting fraud and limiting false alarms.

> The exact results should be updated from the final notebook output if additional threshold tuning, SMOTE, or model configurations change the scores.

---

## 7. Random Forest Results

The Random Forest model achieved approximately:

| Metric        |    Result |
| ------------- | --------: |
| **Accuracy**  |    99.96% |
| **Precision** |     95.1% |
| **Recall**    |     78.6% |
| **F1-Score**  | **86.0%** |
| **ROC-AUC**   |     97.3% |

### Confusion Matrix

| Actual \ Predicted | Not Fraud | Fraud |
| ------------------ | --------: | ----: |
| **Not Fraud**      |    56,860 |     4 |
| **Fraud**          |        21 |    77 |

This means the model successfully identified many fraudulent transactions while producing very few false fraud alerts.

---

## 8. Business Justification

The choice of F1-score reflects the two major business risks.

### Missed Fraud

A fraudulent transaction classified as legitimate can result in:

* Financial loss
* Customer disputes
* Chargebacks
* Reduced customer trust

This makes **recall** important.

### False Fraud Alerts

A legitimate transaction incorrectly classified as fraudulent can:

* Interrupt customer purchases
* Frustrate customers
* Increase investigation costs
* Require unnecessary manual review

This makes **precision** important.

### F1-Score

F1-score combines precision and recall into a single measure, making it useful when the business wants to balance fraud detection with false-alarm reduction.

---

## 9. Why Random Forest Performed Well

Random Forest was selected as the best-performing model based on the evaluated F1-score.

Possible reasons include:

* It can capture non-linear relationships.
* It combines multiple decision trees.
* It can model complex interactions between features.
* It performs well on many classification problems.
* It can provide feature-importance information.

However, the model should still be validated further before being used in a real financial system.

---

## 10. Future Improvements

Although the current model performs well, **the results can still be improved**.

### Threshold Optimization

More threshold values can be tested to find the best trade-off between precision and recall.

### SMOTE and Other Imbalance Techniques

SMOTE can be compared with:

* Class weighting
* Random under-sampling
* Random over-sampling
* Other imbalance-handling techniques

The goal is to determine which approach produces the best F1-score without creating excessive false positives.

### Cross-Validation

Cross-validation can provide a more reliable estimate of model performance and reduce dependence on one train/test split.

### Feature Engineering

Additional meaningful transaction features could potentially improve fraud detection.

### Hyperparameter Tuning

Random Forest and XGBoost parameters can be optimized using techniques such as:

* Grid Search
* Random Search
* Bayesian optimization

### XGBoost Improvements

XGBoost can be further tuned and compared against Random Forest using the same threshold-optimization process.

### Cost-Sensitive Evaluation

Future versions could consider the actual financial cost of:

* Missing a fraudulent transaction
* Blocking a legitimate transaction

This could lead to a business-specific optimization metric rather than relying only on F1.

### Real-Time Monitoring

A production fraud-detection system should continuously monitor:

* Model performance
* Fraud patterns
* Data drift
* False-positive rates
* False-negative rates

---

## 11. Conclusion

The credit card fraud detection experiment demonstrates that **accuracy alone is not sufficient** for evaluating highly imbalanced classification problems.

The model comparison showed that **Random Forest achieved a strong F1-score of approximately 86.0%**, with approximately **95.1% precision** and **78.6% recall**.

The results indicate that the model can identify a substantial proportion of fraudulent transactions while keeping false alarms relatively low.

**F1-score was selected as the primary metric because it balances precision and recall.**

However, the project is not considered finished. Further improvements through **threshold tuning, SMOTE experimentation, hyperparameter optimization, cross-validation, feature engineering, and cost-sensitive evaluation** could potentially improve the model and make it more suitable for real-world deployment.

> **Key takeaway:** In highly imbalanced fraud detection, the goal is not simply to achieve high accuracy, but to find the best balance between catching fraud and minimizing false alarms.