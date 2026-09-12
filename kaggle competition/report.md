# Spaceship Titanic — Kaggle Project

**Author:** Justine Umutoni  
**Competition:** [Spaceship Titanic](https://www.kaggle.com/competitions/spaceship-titanic)  
**Best Leaderboard Score:** 0.81014  
**Project Target:** 0.80  
**Result:** Above target by +0.01014
**Best Results logs:** https://www.kaggle.com/code/justineumutoni/spaceshiptitanic-justineumutoni-ipynb/log?scriptVersionId=349189357
**Code link:** https://www.kaggle.com/code/justineumutoni/spaceshiptitanic-justineumutoni-ipynb/edit

---

## Project Overview

This project predicts whether passengers on the Spaceship Titanic were transported to an alternate dimension. The task is binary classification, evaluated using classification accuracy.

| Attribute | Value |
|---|---|
| Training rows | 8,693 |
| Test rows | 4,277 |
| Original features | 13 |
| Target | `Transported` (Boolean) |
| Class balance | 50.4% True / 49.6% False |
| Random seed | 42 |

---

## Best Model

A **4-model weighted voting ensemble** using soft voting.

| Position | Model | Weight | Share |
|---|---|---|---|
| 1 | Decision Tree | 1 | 10% |
| 2 | Extra Trees | 2 | 20% |
| 3 | CatBoost | 4 | 40% |
| 4 | LightGBM | 3 | 30% |

### Performance

| Metric | Value |
|---|---|
| Cross-validation accuracy | 0.8164 |
| GroupKFold accuracy | 0.8135 |
| Leaderboard accuracy | **0.81014** |
| CV–LB gap | +0.0063 |

---

## Feature Engineering

The best model used **13 numeric features** and **6 categorical features**.

### Numeric Features

| # | Feature | Derivation | Purpose |
|---|---|---|---|
| 1 | `Age` | Original | Demographic |
| 2 | `GroupLen` | `groupby('GID').count()` | Travel group size |
| 3 | `Solo` | `GroupLen == 1` | Solo traveller flag |
| 4 | `Seat` | `PassengerId` split | Position in group |
| 5 | `Room` | `Cabin` split | Cabin number |
| 6 | `Cash` | Sum of spending columns | Total spend |
| 7 | `CashLog` | `log1p(Cash)` | Skew correction |
| 8 | `ZeroSpend` | `Cash == 0` | No-spend flag |
| 9 | `UsedCount` | Non-zero spend count | Amenity count |
| 10 | `HighEnd` | Spa + VRDeck + RoomService | Luxury spend |
| 11 | `LowEnd` | FoodCourt + ShoppingMall | Budget spend |
| 12 | `HighRatio` | `HighEnd / (Cash + 1)` | Luxury ratio |
| 13 | `SleepZero` | `CryoSleep × ZeroSpend` | Interaction |

### Categorical Features

`HomePlanet`, `CryoSleep`, `Destination`, `VIP`, `Zone`, `Pier`

### Features Tested and Rejected

| Feature | CV Change | Reason |
|---|---|---|
| `Cabin_Sector` | −0.0016 | No gain |
| `SurnameSize` | −0.0016 | Redundant |
| `Group_Agreement` | −0.0058 | Broke pipeline |
| `GroupCryoRate` | +0.0007 | Below noise floor |

---

## Preprocessing Pipeline

All preprocessing was performed inside a scikit-learn `Pipeline` to prevent data leakage during cross-validation.

| Step | Numeric | Categorical |
|---|---|---|
| Imputation | `SimpleImputer(strategy='mean')` | `SimpleImputer(strategy='most_frequent')` |
| Transformation | `StandardScaler()` | `OneHotEncoder(handle_unknown='ignore')` |

Combined using `ColumnTransformer`.

---

## Models Tested

Sixteen model configurations were evaluated, spanning four algorithmic families.

| Model | Family | Best CV |
|---|---|---|
| Logistic Regression | Linear | 0.7963 |
| SGD Classifier | Linear | 0.7873 |
| Linear Discriminant Analysis | Linear | 0.7929 |
| Decision Tree | Tree | 0.7977 |
| Random Forest | Bagging | 0.8081 |
| Extra Trees | Bagging | 0.8098 |
| Bagging Classifier | Bagging | 0.8016 |
| Gradient Boosting | Boosting | 0.8113 |
| XGBoost | Boosting | 0.8131 |
| **CatBoost** | Boosting | **0.8174** |
| LightGBM | Boosting | 0.8104 |
| HistGradientBoosting | Boosting | 0.8181 |
| SVM (RBF) | Kernel | 0.8005 |
| k-Nearest Neighbours | Distance | 0.7811 |
| Naive Bayes | Probabilistic | 0.7622 |
| Multi-Layer Perceptron | Neural | 0.8026 |

---

## Ensembles Developed

Eight ensembles were built and evaluated.

| Ensemble | Components | Weights | CV | LB |
|---|---|---|---|---|
| Hybrid Diverse | CatBoost + XGBoost + SVM | [3, 1, 2] | 0.8140 | 0.80827 |
| OOF Hybrid | CatBoost + XGBoost + LightGBM | learned | 0.8179 | 0.80710 |
| Regularized Ensemble | HGB + XGBoost + CatBoost | [3, 2, 2] | 0.8075 | 0.80360 |
| 4-Model Voting v1 | DT + ET + CatBoost + LGBM | [1, 2, 3, 3] | 0.8148 | 0.80944 |
| **4-Model Voting v2** | **DT + ET + CatBoost + LGBM** | **[1, 2, 4, 3]** | **0.8164** | **0.81014** |
| 4-Model Voting v3 | DT + ET + CatBoost + LGBM | [1, 2, 6, 3] | 0.8174 | 0.80827 |
| 5-Model Ensemble | + SVM | [1, 2, 4, 3, 1] | 0.8146 | 0.80991 |
| 10-Model Stacking | 10 base + HGB meta | learned | 0.8071 | 0.81014 |

---

## Iteration Log

| # | What I changed | Why I expected it to help | CV | LB | What I concluded |
|---|---|---|---|---|---|
| 1 | Logistic Regression (baseline) | Establish reference | 0.7959 | — | Works, but low |
| 2 | Decision Tree | Tree captures patterns | 0.7977 | — | Small gain |
| 3 | Extra Trees | Diverse bagging | 0.8090 | — | Big jump |
| 4 | CatBoost | Strongest boosting | 0.8133 | 0.80430 | Best single model |
| 5 | LightGBM | Fast boosting | 0.8098 | — | Strong support |
| **6** | **4-Model Voting [1,2,4,3]** | **Diverse ensemble** | **0.8164** | **0.81014** | **Best overall** |
| 7 | + Surname + CabinSector | Additional features | 0.8117 | 0.81014 | Hurt — dropped |
| 8 | + SVM (5-model) | More diversity | 0.8146 | 0.80991 | Worse |
| 9 | Weight [1,2,6,3] | More CatBoost | 0.8174 | 0.80827 | Overfit — worse |
| 10 | Regularized Ensemble | Reduce overfitting | 0.8075 | 0.80360 | Too much regularization |
| 11 | 10-Model Stacking | More models | 0.8071 | 0.81014 | Same LB as 4-model |

---

## The CV vs LB Relationship

| Model | CV | LB | Gap |
|---|---|---|---|
| HGB (overfit) | 0.8181 | 0.80079 | **+0.0173** |
| OOF Hybrid | 0.8179 | 0.80710 | +0.0108 |
| 4-Model [1,2,6,3] | 0.8174 | 0.80827 | +0.0091 |
| **4-Model [1,2,4,3]** | **0.8164** | **0.81014** | **+0.0063** |
| 5-Model Ensemble | 0.8146 | 0.80991 | +0.0047 |
| 10-Model Stacking | 0.8071 | 0.81014 | **−0.0030** |

### Why LB Was Sometimes Lower Than CV

1. **Tuning on CV folds** — Hyperparameters chosen for specific folds generalise poorly to unseen test data.
2. **Threshold and weight tuning** — Fits fold structure and inflates CV.
3. **Repeated selection** — The best CV among many runs is statistically inflated.
4. **Small dataset** — 8,693 rows produce noisy CV estimates.

**Practical lesson:** Cross-validation gains below 0.005 do not reliably translate to leaderboard gains on this dataset.

---

## Why Some Submissions Produced the Same LB

Three mechanisms explain identical leaderboard scores.

### Mechanism 1 — Deterministic Models

With all random seeds fixed to 42, a model produces byte-identical predictions every run.

| Version | LB |
|---|---|
| V70 | 0.81014 |
| V77 | 0.81014 |
| V85 | 0.81014 |

### Mechanism 2 — Accuracy Collision

Two architecturally different models can reach the same number of correct predictions without agreeing on any individual prediction.

- 4-Model Voting (CV 0.8164) → LB 0.81014
- 10-Model Stacking (CV 0.8071) → LB 0.81014

### Mechanism 3 — Score Rounding

Kaggle reports accuracy to five decimal places. Values of 0.810139 and 0.810140 both display as 0.81014.

---

## Challenges Encountered

| Challenge | Detail |
|---|---|
| Group Agreement feature | Broke the pipeline three times; abandoned |
| Data corruption | Duplicate columns from double-execution of feature engineering |
| CV–LB mismatch | Highest CV produced the lowest LB |
| Stacking failure | Meta-model overfit out-of-fold predictions |

---

## Experiments That Failed

| Experiment | Result | Lesson |
|---|---|---|
| Group Agreement (3 attempts) | Broke pipeline | Custom transformers are fragile |
| + SVM | CV dropped to 0.8146 | Weak models dilute ensembles |
| Weight [1,2,6,3] | LB fell to 0.80827 | Weight tuning overfits |
| 10-Model Stacking | CV dropped to 0.8071 | More models ≠ better |
| CatBoost native categoricals | No gain | One-hot was sufficient |
| Extra Trees 800 trees | CV dropped to 0.8162 | Diminishing returns |
| Surname + CabinSector | CV dropped to 0.8117 | Noise, not signal |
| Regularized ensemble | CV dropped to 0.8075 | Over-regularization removed signal |

---

## Key Technical Findings

| # | Finding | Evidence |
|---|---|---|
| 1 | Diverse ensembles outperform single models | Ensemble 0.81014 > CatBoost 0.80430 |
| 2 | Four models is the optimal ensemble size | 4-model CV 0.8164 > 10-model CV 0.8071 |
| 3 | Weights should reflect model quality | CatBoost weight 4 outperformed weights 3 and 6 |
| 4 | Extra features can hurt performance | Surname/CabinSector reduced CV by 0.0047 |
| 5 | GroupKFold detects leakage | Gaps stayed below 0.005 across all models |
| 6 | Cross-validation is not a reliable LB predictor | HGB CV 0.8181 produced LB 0.80079 |
| 7 | Weak models degrade ensembles | SVM, SGD, and LDA reduced ensemble CV |
| 8 | Different ensembles can converge | Voting and stacking both reached LB 0.81014 |
| 9 | Deterministic models produce identical LB | CatBoost scored 0.80430 on seven submissions |
| 10 | The first well-designed model was the best | 4-Model Voting [1,2,4,3] remained unbeaten |

---

## Final Model Specification

| Element | Value |
|---|---|
| Ensemble type | `VotingClassifier(voting='soft')` |
| Base models | Decision Tree, Extra Trees, CatBoost, LightGBM |
| Weights | [1, 2, 4, 3] |
| Features | 13 numeric + 6 categorical |
| Preprocessing | Mean imputation, StandardScaler, OneHotEncoder |
| Random seed | 42 |
| CV accuracy | 0.8164 |
| GroupKFold accuracy | 0.8135 |
| **LB accuracy** | **0.81014** |

### Hyperparameters

| Model | Parameter | Value |
|---|---|---|
| Decision Tree | `max_depth` | 8 |
| | `min_samples_leaf` | 8 |
| Extra Trees | `n_estimators` | 400 |
| | `max_depth` | 12 |
| | `min_samples_leaf` | 3 |
| CatBoost | `iterations` | 400 |
| | `learning_rate` | 0.06 |
| | `depth` | 5 |
| LightGBM | `n_estimators` | 400 |
| | `learning_rate` | 0.06 |
| | `num_leaves` | 31 |

---

## Submission History

| Version | Model | CV | LB |
|---|---|---|---|
| V3 | Hybrid Diverse | 0.8140 | 0.80827 |
| V6 | OOF Hybrid | 0.8179 | 0.80710 |
| V9 | Regularized HGB | 0.8057 | 0.80196 |
| V19 | Regularized Ensemble | 0.8075 | 0.80360 |
| V21 | HGB (full features) | 0.8178 | 0.80640 |
| V27 | CatBoost (+ extra features) | 0.8159 | 0.80687 |
| V29 | HGB (overfit) | 0.8181 | 0.80079 |
| V31 | Hybrid + extra features | 0.8143 | 0.80617 |
| V33 | Hybrid (original) | 0.8143 | 0.80757 |
| V36–V62 | CatBoost (original) | 0.8164 | 0.80430 |
| V47–V60 | HGB (full features) | 0.8178 | 0.80780 |
| V64 | 4-Model Voting [1,2,3,3] | 0.8148 | 0.80944 |
| **V70** | **4-Model Voting [1,2,4,3]** | **0.8164** | **0.81014** |
| V73 | 4-Model Voting [1,2,4,3] | 0.8164 | 0.80827 |
| V75 | 5-Model Ensemble | 0.8146 | 0.80991 |
| **V77** | **4-Model Voting [1,2,4,3]** | **0.8164** | **0.81014** |
| V79 | 4-Model Voting [1,2,3,3] | 0.8148 | 0.80944 |
| V83 | 5-Model + Group Agreement | 0.8146 | 0.80476 |
| **V85** | **4-Model Voting [1,2,4,3]** | **0.8164** | **0.81014** |
| Stacking V2 | 10-Model Stacking | 0.8071 | 0.81014 |

---

## Deliverables

| # | Deliverable | Status |
|---|---|---|
| 1 | Public Kaggle notebook | Pending publication |
| 2 | Iteration log (11 rows) | Complete |
| 3 | Screenshot of best submission | Pending capture |
| 4 | Ten-minute demonstration | Pending preparation |

---

## Conclusion

This project produced a **4-model weighted voting ensemble** achieving a leaderboard accuracy of **0.81014**, exceeding the project target of 0.80 by **1.01 percentage points**.

Three technical contributions stand out:

1. **Ensemble diversity is more valuable than ensemble size.** A 4-model ensemble outperformed a 10-model stacking classifier on both cross-validation and leaderboard.

2. **Cross-validation accuracy does not reliably predict leaderboard performance.** The model with the highest CV produced the lowest LB. The cause was identified as tuning on the CV folds, which inflates CV without improving generalisation.

3. **GroupKFold validation provides a more honest estimate of generalisation** when the data has natural group structure, because it prevents group members from appearing in both training and validation folds.

The best model is reproducible, its performance is validated, and its limitations are documented. The project is complete.

---

**Final Leaderboard Score: 0.81014**  
**Project Target: 0.80**  
**Status: Above target by 1.01 percentage points**
