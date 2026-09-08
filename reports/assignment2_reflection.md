# Assignment 2 Reflection

## 1. Which transformation took the longest to get right, and why?

The **GroupBy and merge transformation** required the most attention. The challenge was not the syntax itself, but making sure the transformation preserved the analytical meaning of the dataset. I had to think carefully about the level of aggregation, the join key, and whether the merge was introducing duplicated observations or changing the original dataset structure.

I also learned that a technically correct transformation is not necessarily an analytically useful one. The aggregated features needed to answer a meaningful question about loan applicants rather than simply satisfy the assignment requirement.

The NumPy standardization was more straightforward because it demonstrated how vectorized operations and broadcasting can efficiently apply the same mathematical transformation across an entire array without explicit iteration.


## 2. What would I do differently with another dataset?

With another dataset, I would begin with a stronger data-quality assessment before performing any transformations. In particular, I would investigate missingness patterns, potential outliers, class imbalance, variable distributions, and possible sources of bias before deciding how the data should be cleaned.

I would also spend more time defining the analytical question before selecting transformations. This would help ensure that every feature engineered, aggregation performed, and visualization created contributes directly to answering the research question.

Most importantly, I would distinguish more clearly between **association and causation**. For example, the strong relationship between credit history and loan approval in this dataset is an important finding, but it does not establish that credit history alone causes an applicant to be approved.

This assignment reinforced that good data analysis is not simply about producing clean code or attractive visualizations. It requires understanding the data-generating process, making defensible preprocessing decisions, validating transformations, and interpreting results within their limitations.
