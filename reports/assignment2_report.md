# Weekend Assignment 2 Report: Loan Data Wrangling and Exploratory Analysis

## Question Explored

This analysis investigated the question:

**What applicant and financial characteristics are associated with successful loan approval?**

I used the Loan Prediction dataset, which contains 614 loan applications and 13 original variables covering applicant demographics, income, loan information, credit history, property area, and loan approval status.

## Data Cleaning

The dataset initially contained missing values in several categorical and numerical columns. Missing categorical values were filled using the most frequent category, while missing numerical values were filled using the median. The median was selected because financial variables can contain extreme values that may influence the mean.

The `Dependents` column contained the value `3+`, which was converted to `3` so that the variable could be represented numerically. Duplicate rows were also checked, and no duplicate records remained after cleaning.

The final cleaned dataset contained **614 rows and 16 columns**, including the new features created during the analysis.

## Feature Engineering

A GroupBy aggregation was used to calculate the loan approval rate and average loan amount for each credit-history group. These group-level statistics were then merged back into the main applicant dataset.

A pivot table was also created to compare loan application outcomes across education levels, property areas, and loan-status categories.

For the NumPy computation, the `LoanAmount` column was converted into a NumPy array and standardized using vectorized operations and broadcasting. The standardized values were added back to the dataset as `LoanAmount_Standardized`.

## Key Findings

The strongest finding was the relationship between credit history and loan approval.

Applicants with a credit-history value of `1` had an approval rate of approximately **79.05%**, while applicants with a credit-history value of `0` had an approval rate of approximately **7.87%**.

This shows a very strong association between positive credit history and loan approval in this dataset. However, this relationship should not be interpreted as proof that credit history alone causes approval, because other applicant and financial characteristics may also influence lending decisions.

The first visualization, `assignment2_chart1.png`, shows the large difference in approval rates between the two credit-history groups.

The second visualization, `assignment2_chart2.png`, explores the relationship between applicant income, requested loan amount, and loan approval status.

The third visualization, `assignment2_chart3.png`, compares average requested loan amounts between graduates and non-graduates across loan approval outcomes.

The fourth visualization, `assignment2_chart4.png`, explores the relationship between applicant income and requested loan amount using a trendline.


## Limitation

One limitation of this analysis is that the dataset represents historical loan decisions from a particular lending context. The patterns found in the dataset may reflect existing lending policies or characteristics of the applicants represented in the data.

Therefore, the results should not automatically be generalized to other banks, countries, or populations. In addition, the strong association between credit history and approval should not be interpreted as a causal relationship.


## Conclusion

The analysis demonstrated how NumPy and Pandas can be used to transform a raw banking dataset into a structured dataset suitable for exploratory analysis.

The results indicate that credit history has a strong association with loan approval in this dataset. The analysis also demonstrated the importance of examining data quality, performing deliberate cleaning, engineering useful features, and using visualizations to communicate findings.
