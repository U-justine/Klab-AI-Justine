# Assignment 3: Reflection

**Author:** Justine Umutoni  
**Date:** August 2026  
**Course:** KLab AI Bootcamp  

---

## Challenges I Faced

### 1. Understanding the Dataset

One challenge I faced was identifying the correct columns to use for the machine learning models. The column names in my actual dataset were different from the examples, so I had to inspect the dataset and select the appropriate features and target.

### 2. Selecting Features and Target

I selected `ApplicantIncome`, `CoapplicantIncome`, and `Credit_History` as the features and `LoanAmount` as the target.

This helped me understand the importance of choosing suitable variables when building a machine learning model.

### 3. Comparing Different Models

Another challenge was understanding why the more complex Random Forest model did not perform better.

I initially expected Random Forest to perform better because it uses multiple decision trees. However, the results showed that Linear Regression performed better on my dataset.

### 4. Understanding Evaluation Metrics

I learned how to use **MSE** and **R² Score** to evaluate regression models.

I learned that:

- A lower MSE indicates better performance.
- A higher R² Score indicates better performance.

These metrics helped me make an objective comparison between the two models.

---

## What I Learned

Through this assignment, I learned:

- How to prepare data for a regression problem.
- How to split data into training and testing sets.
- How to train a Linear Regression model.
- How to train a Random Forest Regression model.
- How to make predictions using trained models.
- How to evaluate regression models using MSE and R² Score.
- How to compare different machine learning models.
- How visualizations can help explain model performance.

---

## Key Takeaways

The main lessons I learned from this assignment are:

- A more complex model is not always the best model.
- Model performance depends on the dataset and selected features.
- MSE and R² Score are useful for comparing regression models.
- Visualizations can make model results easier to understand.
- Simple models can perform well when they match the characteristics of the data.

---

## What I Would Do Differently

If I repeated this assignment, I would:

- Try additional features that may help predict `LoanAmount`.
- Use cross-validation to get more reliable performance results.
- Experiment with different Random Forest parameters.
- Try other regression algorithms.
- Explore feature engineering to improve the models.
- Spend more time analyzing the relationship between the features and target.

---

## Conclusion

This assignment improved my understanding of regression and machine learning model evaluation.

I learned how to build two different models, make predictions, evaluate their performance, and compare their results.

The results showed that **Linear Regression performed better than Random Forest** on my dataset, with a lower MSE and a higher R² Score.

Most importantly, I learned that choosing a machine learning model should be based on its actual performance on the data rather than assuming that a more complex model will always perform better.

Overall, this assignment gave me practical experience with building and evaluating machine learning regression models.