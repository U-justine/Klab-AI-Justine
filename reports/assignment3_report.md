# Assignment 3: Report

**Author:** Justine Umutoni  
**Date:** August 2026  
**Course:** KLab AI Bootcamp  

---

## Objective

The objective of this assignment was to build and compare two machine learning regression models: **Linear Regression** and **Random Forest Regression**.

The models were used to predict `LoanAmount` using `ApplicantIncome`, `CoapplicantIncome`, and `Credit_History`.

The models were evaluated using **Mean Squared Error (MSE)** and **R² Score**.

---

## Dataset

The dataset used in this assignment is the cleaned loan dataset from Assignment 2.

- **Rows:** 614
- **Columns:** 16
- **Features:** `ApplicantIncome`, `CoapplicantIncome`, `Credit_History`
- **Target:** `LoanAmount`

The target variable, `LoanAmount`, is numeric, making it suitable for a regression problem.

---

## Methodology

### 1. Data Preparation

I loaded the cleaned loan dataset from Assignment 2 and explored its structure.

I also checked for missing values in the selected features and target.

The dataset was divided into:

- 80% training data
- 20% testing data

### 2. Linear Regression

Linear Regression was used as the first model.

The model used:

- `ApplicantIncome`
- `CoapplicantIncome`
- `Credit_History`

to predict `LoanAmount`.

The model was evaluated using MSE and R² Score.

### 3. Random Forest Regression

Random Forest Regression was used as the second model.

The model was created using **100 decision trees** with `random_state=42`.

It was trained using the same features and target as Linear Regression.

The same evaluation metrics were used to compare both models.

---

## Results

### Performance Comparison

| Model | MSE | R² Score |
|---|---:|---:|
| Linear Regression | 2636.03 | 0.516 |
| Random Forest | 4548.84 | 0.164 |

### Interpretation

Linear Regression performed better than Random Forest.

Linear Regression had a lower MSE of **2636.03**, compared with **4548.84** for Random Forest.

It also achieved a higher R² Score of **0.516**, compared with **0.164** for Random Forest.

A lower MSE means smaller prediction errors, while a higher R² Score means better performance in explaining the variation in the target variable.

The R² Score of 0.516 means that Linear Regression explained about **52% of the variation in LoanAmount** on the test data.

---

## Actual vs Predicted Results

The Actual vs Predicted scatter plot also supported the evaluation results.

The Linear Regression predictions were closer to the actual values, while the Random Forest predictions were more scattered.

This indicates that Linear Regression produced better predictions for this dataset.

---

## Discussion

### Why Linear Regression Performed Better

Linear Regression performed better because it was able to capture the relationship between the selected features and `LoanAmount` effectively.

The results also show that a simpler model can sometimes perform better than a more complex model, depending on the dataset.

### Why Random Forest Performed Worse

Random Forest produced a higher MSE and a lower R² Score.

Although Random Forest can capture complex relationships, it did not perform as well as Linear Regression on this dataset.

---

## Conclusion

In this assignment, I built and compared Linear Regression and Random Forest Regression models to predict `LoanAmount`.

Based on the evaluation results, **Linear Regression performed better than Random Forest**.

It achieved an MSE of **2636.03** and an R² Score of **0.516**, while Random Forest achieved an MSE of **4548.84** and an R² Score of **0.164**.

Therefore, Linear Regression was the better-performing model for predicting `LoanAmount` in this dataset.

---

## Recommendations

For future work, I would:

- Try additional relevant features.
- Use cross-validation for more reliable evaluation.
- Experiment with different Random Forest parameters.
- Try other regression algorithms.
- Explore feature engineering to improve model performance.

---

## References

- [Scikit-learn Linear Regression Documentation](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html)
- [Scikit-learn Random Forest Regression Documentation](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestRegressor.html)