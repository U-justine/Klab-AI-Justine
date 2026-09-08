# Assignment 5 Reflection

## Justine Umutoni

---

## 1. What I Learned

This assignment was my first deep dive into evaluating unsupervised learning models. The most important lesson I learned is that **accuracy is meaningless for clustering evaluation**. When I saw the naive accuracy change from 9.5% to 90.5% just by swapping cluster labels, I finally understood why ARI is the correct metric.

I also learned that **inertia always falls with more clusters**, which means it cannot be used to choose the optimal number of clusters. Silhouette score, on the other hand, has a genuine maximum, making it useful for model selection.

The GMM overfitting analysis was particularly valuable. Seeing the test score drop while the training score rose gave me a clear understanding of overfitting in an unsupervised context. This showed me that I can detect overfitting even without labels by using a train/test split.

I was surprised that DBSCAN failed on this dataset, but I understood why after learning about the curse of dimensionality. With 30 features, distance-based clustering becomes difficult because all points appear far apart. This taught me to choose algorithms based on data characteristics.

---

## 2. Challenges I Faced

**Visualization Errors:** I had issues with seaborn boxplots that took me some time to resolve. Converting target labels to categorical type fixed the problem. This taught me the importance of data type handling in visualization.

**DBSCAN Parameter Tuning:** Finding the right epsilon value was challenging because all points were initially labeled as noise. I had to increase epsilon significantly to get any clusters, and even then the results were poor with negative silhouette. I spent a lot of time trying different values before accepting that DBSCAN just doesn't work well on this dataset.

**Interpreting Overfitting:** I initially thought BIC was a reliable metric for model selection. When I saw BIC kept decreasing with more components, I almost concluded that more components were better. It was only when I looked at the test scores that I realized the model was overfitting. This was a valuable lesson about not trusting a single metric.

**Stability Interpretation:** I was confused why K=3 had slightly higher stability than K=2. After thinking about it, I realized that smaller, more consistent clusters are easier to reproduce in random samples, even if they don't represent the true structure as well.

---

## 3. Key Takeaways

1. **Accuracy fails** for clustering evaluation - use ARI instead
2. **Silhouette score** is useful for finding optimal K (peaks at K=2)
3. **Stability test** confirms clusters are real structure, not noise
4. **GMM train/test split** detects overfitting (test score drops after K=2)
5. **DBSCAN** struggles with high-dimensional data (curse of dimensionality)
6. **Metrics that always improve** (inertia, BIC, train score) **cannot be used to choose complexity**

---

## 4. What I Would Do Differently

If I were to do this assignment again, I would:

1. **Try dimensionality reduction** like PCA before applying DBSCAN to help with the curse of dimensionality
2. **Use cross-validation** for GMM to get more robust test scores
3. **Try different min_samples** values for DBSCAN to see if that improves results
4. **Add silhouette diagrams** for a more detailed view of cluster quality
5. **Include BIC plots** alongside test scores to compare model selection criteria

---

## 5. Real-World Applications

The techniques I learned in this assignment have real-world applications:

| Application | How This Applies |
|-------------|------------------|
| **Medical Diagnosis** | Validating that clustering matches clinical categories |
| **Customer Segmentation** | Ensuring segments are stable and reproducible |
| **Anomaly Detection** | Detecting when clustering fails (like DBSCAN) |
| **Feature Engineering** | Identifying which algorithms work best for different data types |

---

## 6. My Thoughts on Each Part

**Part 1 (Accuracy Fails):** This was eye-opening. I always thought accuracy was a universal metric, but now I understand it only works when labels have meaning. In clustering, labels are just names.

**Part 2 (Silhouette Score):** This was straightforward to implement and gave clear results. I appreciate that both silhouette and Davies-Bouldin agreed on K=2.

**Part 3 (Stability Test):** This was the most interesting part because it's a concept I hadn't encountered before. Testing if clusters reappear in random samples makes a lot of sense for validating structure.

**Part 4 (GMM Overfitting):** This was the most valuable part because it showed me how to detect overfitting without labels. The train/test split is a supervised learning concept that works well in unsupervised contexts too.

**Part 5 (DBSCAN):** This was frustrating at first because it didn't work, but I learned a lot from the failure. The curse of dimensionality is real, and not every algorithm works on every dataset.

---

## 7. Final Thoughts

This assignment was a practical introduction to the challenges of evaluating unsupervised learning. The most important insight was understanding that **I need to use multiple metrics to validate my results** - no single metric tells the full story.

The convergence of multiple independent metrics (Silhouette, ARI, GMM Test Score) on K=2 gave me confidence that this was the right choice. This is the key principle I will carry forward: **always use multiple evaluation metrics and look for convergence.**

---