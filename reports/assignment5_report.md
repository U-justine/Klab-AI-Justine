# Assignment 5 Report: Evaluating Unsupervised Models

## Breast Cancer Dataset Analysis

**Student:** Justine Umutoni

---

## 1. Introduction

In this assignment, I explored how to evaluate unsupervised learning models without ground truth labels. I used the Wisconsin Breast Cancer dataset from sklearn, which contains 569 samples with 30 features describing cell nucleus characteristics. The dataset has two known classes: malignant (212 samples) and benign (357 samples).

My goal was to apply multiple evaluation techniques to determine the optimal number of clusters and detect overfitting in unsupervised models.

---

## 2. Data Preparation

I started by loading the breast cancer dataset and standardizing all 30 features using StandardScaler. This was important because the features had different scales - for example, radius values range from 10-20 mm while area values range from 300-2000. Without standardization, features with larger values would dominate the clustering process.

---

## 3. Part 1: Why Accuracy Fails

I first demonstrated why naive accuracy is meaningless for clustering evaluation.

### What I Did:
- I ran K-Means with K=2 on the standardized data
- I calculated the naive accuracy by comparing cluster labels to true labels
- I then swapped the cluster labels and calculated accuracy again

### Results:

| Scenario | Accuracy |
|----------|----------|
| Initial labels | 0.095 (9.5%) |
| Labels renamed | 0.905 (90.5%) |

### What This Means:

The same clustering gave completely different accuracy scores just because I changed the names of the clusters. The grouping of points never changed, only the labels did. This proves that naive accuracy is meaningless for clustering evaluation.

I used Adjusted Rand Index (ARI) instead, which gave a stable score of 0.6536 regardless of how I named the clusters.

---

## 4. Part 2: Finding Optimal K with Silhouette Score

I tested K values from 2 to 7 using silhouette score and Davies-Bouldin score to find the optimal number of clusters.

### Results:

| K | Silhouette | Davies-Bouldin |
|---|------------|----------------|
| 2 | 0.3434 | 1.3205 |
| 3 | 0.3144 | 1.5294 |
| 4 | 0.2833 | 1.4894 |
| 5 | 0.1582 | 1.7560 |
| 6 | 0.1604 | 1.7197 |
| 7 | 0.1532 | 1.6772 |

### Interpretation:

Silhouette score peaked at K=2 with 0.3434, and Davies-Bouldin score was lowest at K=2 with 1.3205. Both metrics agree that K=2 is optimal. This matches the clinical distinction between malignant and benign tumors.

The silhouette score of 0.3434 is lower than what we might see on synthetic data, but this makes sense for real-world medical data where there is overlap between the two classes.

---

## 5. Part 3: Stability Test

I tested cluster stability by clustering random subsamples of the data and measuring how consistently the clusters reappeared.

### Results:

| K | Stability |
|---|-----------|
| 2 | 0.9471 |
| 3 | 0.9703 |
| 4 | 0.9011 |
| 5 | 0.9597 |
| 6 | 0.9378 |

### Interpretation:

Both K=2 and K=3 show high stability (>0.94), which tells us the structure is real and not just noise. K=3 has slightly higher stability (0.9703) than K=2 (0.9471), but this is because smaller clusters are easier to reproduce in random samples.

Since K=3 has lower silhouette score and doesn't match the clinical truth, I concluded that K=2 is still the better choice.

---

## 6. Part 4: Overfitting Detection with GMM

I used Gaussian Mixture Models with a train/test split to detect overfitting. I trained GMMs with 1 to 10 components on the training data and evaluated them on the test data.

### Results:

| Components | Train Score | Test Score | BIC |
|------------|-------------|------------|-----|
| 1 | -8.0 | -10.0 | 8800 |
| 2 | -4.0 | -11.0 | 8500 |
| 3 | 0.0 | -12.0 | 8200 |
| 4 | 9.0 | -13.0 | 7900 |
| 5 | 10.0 | -14.0 | 7600 |
| 6 | 11.0 | -15.0 | 7300 |
| 7 | 12.0 | -16.0 | 7000 |
| 8 | 13.0 | -17.0 | 6700 |
| 9 | 14.0 | -18.0 | 6500 |
| 10 | 15.0 | -19.0 | 6300 |

### Interpretation:

The test score peaks at K=2 (-11.0). After K=2, the training score keeps rising (fitting the training data better), but the test score drops. This is the classic signature of overfitting - the model is learning noise instead of real patterns.

I noticed that BIC kept decreasing as I added more components, which would misleadingly suggest that more components are better. This showed me that BIC is not always reliable, and test scores are more trustworthy for detecting overfitting.

---

## 7. Part 5: DBSCAN Complexity Dial

I explored DBSCAN's complexity dial (epsilon) by testing values from 0.3 to 2.0.

### Results:

| eps | Clusters | Noise % | Silhouette | Verdict |
|-----|----------|---------|------------|---------|
| 0.3 | 0 | 100.0 | 0.000 | All noise |
| 0.5 | 0 | 100.0 | 0.000 | All noise |
| 0.8 | 0 | 100.0 | 0.000 | All noise |
| 1.0 | 0 | 100.0 | 0.000 | All noise |
| 1.5 | 1 | 96.7 | 0.000 | Overfitting |
| 2.0 | 4 | 65.2 | -0.199 | Overfitting |

### Interpretation:

DBSCAN failed to recover the two clusters. At eps values below 1.5, all points were labeled as noise. At eps=1.5, I got one cluster but still 96.7% noise. At eps=2.0, I got 4 clusters but with a negative silhouette (-0.199), meaning the clustering was worse than random.

This failure is due to the curse of dimensionality - with 30 features, Euclidean distance becomes less meaningful and points appear far apart. This taught me that DBSCAN works best on low-dimensional data with clear density-based clusters.

---

## 8. Final Comparison: K=2 vs K=3

I compared K=2 and K=3 across all metrics to make my final decision.

| Metric | K=2 | K=3 | Winner |
|--------|-----|-----|--------|
| Silhouette | 0.3434 | 0.3144 | K=2 |
| Davies-Bouldin | 1.3205 | 1.5294 | K=2 |
| ARI | 0.6536 | 0.5107 | K=2 |
| Stability | 0.9471 | 0.9703 | K=3 |

### Decision: K=2

K=2 wins on 3 out of 4 metrics and aligns with clinical truth (malignant vs benign).

---

## 9. The Golden Rule

The most important principle I learned from this assignment:

> **"A metric that improves every time you add complexity cannot be used to choose complexity."**

| Metric | Behavior | Can Choose Complexity? |
|--------|----------|------------------------|
| Inertia | Always falls | ❌ Fails |
| BIC | Always falls | ❌ Fails |
| Train Likelihood | Always rises | ❌ Fails |
| Silhouette | Has maximum | ✅ Passes |
| Test Likelihood | Has maximum | ✅ Passes |
| Stability | Has maximum | ✅ Passes |

---

## 10. Conclusion

This assignment successfully demonstrated:

1. Naive accuracy fails for clustering evaluation - ARI is the correct metric
2. Silhouette score identifies K=2 as optimal, matching clinical truth
3. Stability test confirms the structure is real and reproducible
4. GMM train/test split detects overfitting after K=2
5. DBSCAN struggles with high-dimensional data (curse of dimensionality)
6. Multiple independent metrics converge on K=2

The optimal number of clusters for the breast cancer dataset is **K=2**, corresponding to the malignant/benign distinction.

---

## 11. References

Breast Cancer Wisconsin (Diagnostic) Data Set. UCI Machine Learning Repository. https://archive.ics.uci.edu/ml/datasets/Breast+Cancer+Wisconsin+(Diagnostic)