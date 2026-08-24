# AI / ML Compact Study Notes

A high-signal reference for AI/ML fundamentals, preprocessing, classical algorithms, evaluation, and tuning.

## 1. Core Map

- **AI:** Broad field of systems performing tasks requiring intelligence.
- **ML:** AI that learns patterns from data instead of hand-written rules.
- **Deep Learning:** ML using multi-layer neural networks.
- **Generative AI:** Models that create text, images, audio, video, or code.
- **LLM:** Large generative model trained mainly on text.
- **Narrow AI:** Built for one task; all current practical AI is narrow.
- **AGI:** Hypothetical human-level general intelligence; does not yet exist.

### Learning Types

| Type | Data | Goal | Examples |
|---|---|---|---|
| Supervised | Features + labels | Predict output | Classification, regression |
| Unsupervised | Features only | Find structure | Clustering, PCA |
| Semi-supervised | Few labels + much unlabeled data | Improve learning with limited labels | Image labeling |
| Reinforcement | States, actions, rewards | Learn a policy | Games, robotics, RLHF |

**Classification** predicts categories. **Regression** predicts continuous numbers.  
**Parameters** are learned during training; **hyperparameters** are chosen before training.

### Intelligent Agent

`Environment -> perceive (sensors) -> decide (goal/policy) -> act (actuators) -> Environment`

A rational agent chooses the action with the highest expected utility given its information.

## 2. Data and Mathematics

### Data

- **Structured:** Tables, CSV, SQL.
- **Semi-structured:** JSON, XML, logs.
- **Unstructured:** Text, images, audio, video; convert to numeric vectors, pixels, or embeddings.
- **Feature (X):** Input variable.
- **Label/target (y):** Value to predict.
- **Sample:** One row/observation.
- **Time series:** Order matters; use time-aware splits and features.

### Dataset Rules

- Split into **train** (learn), **validation** (select/tune), and **test** (final honest estimate).
- Never fit preprocessing on validation/test data.
- Prevent **data leakage**: no future information, target-derived features, or test statistics in training.
- Use a baseline, such as predicting the majority class or target mean.

### Linear Algebra

- Scalar: one number; vector: ordered numbers; matrix: 2D array; tensor: N-dimensional array.
- Dataset shape is usually `(samples, features)`.
- Matrix multiplication drives neural network layers: `output = XW + b`.
- Dot product measures weighted interaction/similarity and appears in attention.
- PCA uses covariance, eigenvectors, and eigenvalues to find high-variance directions.

### Probability and Statistics

- Conditional probability: `P(A|B) = P(A and B) / P(B)`.
- Bayes: `P(A|B) = P(B|A)P(A) / P(B)`.
- Mean is outlier-sensitive; median is robust; mode is most frequent.
- Variance measures squared spread; standard deviation is `sqrt(variance)`.
- Covariance shows joint change; correlation normalizes it to `[-1, 1]`.
- Correlation is not causation; confounders may explain both variables.
- Normal distribution is described by mean `mu` and standard deviation `sigma`.

### Calculus and Optimization

- Derivative: rate of change; gradient: vector of partial derivatives.
- Chain rule enables backpropagation through composed layers.
- Loss measures prediction error; training minimizes loss.
- Gradient descent update: `w := w - learning_rate * dL/dw`.
- Learning rate too high can overshoot; too low can converge slowly.
- Regression commonly uses MSE; classification commonly uses cross-entropy/log loss.

## 3. End-to-End ML Workflow

1. Define the business problem, prediction time, constraints, and success metric.
2. Collect representative, correctly labeled data.
3. Split data early; use stratification for classification and time splits for time series.
4. Explore: shape, types, nulls, duplicates, distributions, outliers, class balance, correlations.
5. Clean and engineer features using training data only.
6. Build preprocessing and model in one pipeline.
7. Train a simple baseline, then compare suitable models.
8. Tune hyperparameters using validation or cross-validation.
9. Evaluate once on the untouched test set.
10. Deploy with versioned preprocessing and model artifacts.
11. Monitor latency, errors, data drift, concept drift, quality, fairness, and cost.
12. Retrain when fresh data or performance monitoring requires it.

## 4. Preprocessing and EDA

### EDA Checklist

```python
print(df.shape)
print(df.info())
print(df.describe())
print(df.isna().sum())
print(df.duplicated().sum())
print(df.nunique())
```

Use histograms for distributions, box plots for spread/outliers, bar/count plots for categories and class balance, scatter plots for relationships, and a correlation heatmap for linear relationships.

### Missing Values

- **MCAR:** Missing independently of all data.
- **MAR:** Missingness depends on observed variables.
- **MNAR:** Missingness depends on the missing value itself; most difficult.

Choices:

- Drop rows only when missingness is small and reasonably random.
- Drop a column when it is mostly missing and not valuable.
- Median imputation for skewed numeric data or outliers; mean for roughly symmetric data; mode for categories.
- Forward/backward fill for suitable time series.
- Model-based imputation for complex cases.
- Add a missing indicator when missingness may carry signal.

Fit imputers on training data only.

### Categorical Features

| Method | Use | Main Risk |
|---|---|---|
| One-hot | Nominal, low-cardinality categories | Many columns |
| Ordinal encoding | Ordered categories | Must define correct order |
| Label encoding | Usually ordinal/tree workflows | False numeric order for nominal data |
| Target encoding | High-cardinality categories | Leakage; fit on train only |

### Scaling

- **Min-max:** `x' = (x - min) / (max - min)`; maps to a range, but very outlier-sensitive.
- **Standardization:** `x' = (x - mean) / std`; mean 0, standard deviation 1.
- **Robust scaling:** `(x - median) / IQR`; useful with heavy outliers.

Scale features for KNN, SVM, K-Means, PCA, and often neural networks. Tree models generally do not need scaling. Always `fit` the scaler on train, then `transform` train/validation/test.

### Outliers

IQR rule: `IQR = Q3 - Q1`; flag values below `Q1 - 1.5*IQR` or above `Q3 + 1.5*IQR`.  
Z-score rule: flag roughly `|z| > 3` when the feature is approximately normal.

First decide whether an outlier is an error, a valid extreme, or the signal. Then remove, clip, log-transform (`log1p`), keep, or use robust methods.

### Imbalanced Classes

Accuracy can be useless when one class dominates. Prefer precision, recall, F1, PR-AUC, ROC-AUC, and a confusion matrix according to the business cost.

Options: class weights, SMOTE/oversampling, undersampling, and decision-threshold adjustment. Resample the training set only, never validation or test sets.

### Pipeline Pattern

```python
num = Pipeline([("impute", SimpleImputer(strategy="median")),
                ("scale", StandardScaler())])
cat = Pipeline([("impute", SimpleImputer(strategy="most_frequent")),
                ("encode", OneHotEncoder(handle_unknown="ignore"))])
preprocess = ColumnTransformer([("num", num, numeric_cols),
                                ("cat", cat, categorical_cols)])
model = Pipeline([("prep", preprocess), ("model", estimator)])
model.fit(X_train, y_train)
```

A pipeline keeps training and inference transformations identical and reduces leakage.

## 5. Regression

### Linear Regression

`y_hat = w1*x1 + ... + wn*xn + b`

Finds coefficients minimizing squared error. It is fast and interpretable but assumes a suitable linear relationship and is sensitive to outliers and multicollinearity.

Important assumptions: linearity, independent observations, constant residual variance (homoscedasticity), approximately normal residuals for inference, and limited multicollinearity.

### Polynomial Regression

Adds terms such as `x^2`, `x^3`, and interactions, then fits linear regression. It can model curves but high degree increases overfitting; use regularization and cross-validation.

### Regularized Linear Models

- **Ridge/L2:** `MSE + lambda * sum(w^2)`; shrinks coefficients, handles correlated features, does not usually make coefficients zero.
- **Lasso/L1:** `MSE + lambda * sum(|w|)`; can set coefficients to zero and select features, but selection can be unstable with correlated features.
- **Elastic Net:** combines L1 and L2; useful when there are many irrelevant and correlated features.

Larger regularization means a simpler model. Tune it using cross-validation.

### Regression Metrics

- **MAE:** `mean(|y - y_hat|)`; interpretable and less sensitive to outliers.
- **MSE:** `mean((y - y_hat)^2)`; strongly penalizes large errors.
- **RMSE:** `sqrt(MSE)`; target units and emphasizes large errors.
- **R2:** `1 - SS_res/SS_tot`; `1` perfect, `0` mean baseline, negative worse than baseline.
- **Adjusted R2:** R2 with a penalty for unnecessary features.

Choose MAE when typical error matters; RMSE when large mistakes are especially costly.

## 6. Classification

### Confusion Matrix

| | Predicted positive | Predicted negative |
|---|---:|---:|
| Actual positive | TP | FN |
| Actual negative | FP | TN |

- **Accuracy:** `(TP + TN) / all`; use cautiously with imbalance.
- **Precision:** `TP / (TP + FP)`; controls false alarms.
- **Recall/sensitivity:** `TP / (TP + FN)`; controls missed positives.
- **Specificity:** `TN / (TN + FP)`; true-negative rate.
- **F1:** `2 * precision * recall / (precision + recall)`; balances precision and recall.
- **FPR:** `FP / (FP + TN)`.
- **ROC-AUC:** ranking ability across thresholds; `0.5` random, `1.0` perfect.
- **PR-AUC:** often more informative for rare positive classes.
- **Log loss:** evaluates probability quality and heavily punishes confident mistakes.

Lowering a probability threshold usually increases recall and decreases precision. Choose the threshold from business costs, not habit.

### Algorithms

| Algorithm | Core idea | Strengths | Watch-outs |
|---|---|---|---|
| Logistic regression | Sigmoid of linear score | Fast, interpretable probabilities, strong baseline | Linear boundary; scale features |
| KNN | Vote among nearest points | Simple, nonlinear, no distribution assumption | Scale required; slow prediction; high-dimensional weakness |
| Naive Bayes | Bayes + conditional independence | Very fast, strong for text/small data | Independence assumption; weak interactions |
| Decision tree | Recursive feature thresholds | Interpretable, nonlinear, no scaling | Overfits; unstable without limits |
| Random forest | Bagged random trees + vote | Robust, strong default, feature importance | Memory and inference cost; less interpretable |
| Gradient boosting | Sequentially correct residuals | High accuracy on tabular data | Tuning and overfitting risk |
| XGBoost | Optimized regularized boosting | Fast, regularization, missing-value handling, early stopping | More hyperparameters; can overfit |
| SVM | Maximum-margin boundary; kernels for nonlinear data | Strong in high-dimensional, smaller datasets | Scale required; expensive for large data |

**Bagging** trains diverse models in parallel and mainly reduces variance.  
**Boosting** trains models sequentially and mainly reduces bias.

## 7. Unsupervised Learning

### Clustering

- **K-Means:** Assign to nearest centroid, recompute means, repeat. Fast and scalable; needs `K`, assumes roughly spherical similar-sized clusters, sensitive to scale and outliers. Objective is WCSS/inertia.
- **Hierarchical agglomerative:** Start with one cluster per point and repeatedly merge; inspect a dendrogram. No initial `K`, but expensive and early merges cannot be undone.
- **DBSCAN:** Groups dense regions using `eps` and `min_samples`; finds arbitrary shapes and noise (`-1`), but struggles with varying density and high dimensions.
- **GMM:** Mixture of Gaussian distributions; gives soft membership probabilities and handles overlapping/elliptical clusters.

Choose `K` with the elbow method (inertia) and/or silhouette score.  
Silhouette is near `+1` for well-separated points, `0` at boundaries, and negative for poor assignments.

### Dimensionality Reduction

- **PCA:** Unsupervised linear projection maximizing variance; standardize first. Fast and useful for compression, visualization, and preprocessing, but components are less interpretable and labels are ignored.
- **LDA:** Supervised projection maximizing class separation; useful before classification.
- **t-SNE:** Nonlinear visualization preserving local neighborhoods; computationally expensive, unstable across runs, and generally not for downstream features or new-point transformation.
- **UMAP:** Faster nonlinear visualization with better global structure and support for transforming new points.

## 8. Generalization and Tuning

### Underfitting vs Overfitting

- **Underfitting/high bias:** Training and validation errors both high. Fix with a richer model, better features, less regularization, or more training.
- **Overfitting/high variance:** Training error low but validation error high. Fix with simpler models, more data, regularization, feature selection, augmentation, or early stopping.

`Total expected error = bias^2 + variance + irreducible noise`.

### Regularization

- L1/L2 penalties constrain weights.
- Dropout randomly disables neurons during neural-network training; disable it at inference.
- Early stopping keeps the checkpoint with the best validation score.
- Data augmentation creates varied training examples.
- Batch normalization can stabilize deep-network training.

### Cross-Validation

K-fold CV trains K times, each time validating on a different fold, and reports mean plus variation. Use **StratifiedKFold** for classification, especially with imbalance. Use time-aware CV for time series. Keep the final test set untouched.

### Hyperparameter Search

- **Grid search:** exhaustive; practical only for small grids.
- **Random search:** explores more useful ranges with fewer trials.
- **Bayesian optimization:** uses previous results to choose promising trials.

Tune only on training data through CV or a validation set. Common hyperparameters: tree depth, number of trees, learning rate, K, SVM `C/gamma`, regularization strength, batch size, and network depth.

## 9. Interview Quick Answers

**Why not evaluate on training data?** It measures memorization, not generalization.  
**Why is accuracy misleading?** A majority-class model can score highly while missing every minority case.  
**Why fit preprocessing only on train?** Using test statistics leaks information and inflates results.  
**Why do trees not need scaling?** Threshold comparisons are unchanged by monotonic rescaling.  
**Why use a pipeline?** It applies identical, leakage-safe transformations during training and inference.  
**Ridge vs Lasso?** Ridge shrinks correlated features; Lasso can remove features.  
**Precision vs recall?** Precision limits false alarms; recall limits missed positives.  
**Random forest vs boosting?** Forests average independent diverse trees; boosting sequentially fixes errors.  
**PCA vs t-SNE?** PCA is linear and reusable for preprocessing; t-SNE is nonlinear visualization focused on local structure.  
**What is concept drift?** The relationship between inputs and target changes over time.  
**What is data drift?** The input distribution changes over time.  
**What is the first step in an ML problem?** Define the prediction, availability of features at prediction time, costs, constraints, and success metric.

## 10. Minimal Tool Map

- Python: language and scripting.
- NumPy: arrays and numerical operations.
- Pandas: tabular data loading, cleaning, and analysis.
- Matplotlib/Seaborn: visualization and EDA.
- scikit-learn: preprocessing, classical models, metrics, pipelines, and CV.
- PyTorch/TensorFlow: deep learning tensors and training.
- SQL: data extraction and aggregation.
- FastAPI: model-serving API.
- Docker: reproducible packaging.
- Git: version control and experiment history.
