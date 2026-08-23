# AI / ML / DL / GenAI — Complete Interview Study Notes
# PART 5 — ML Algorithms: Regression

---

> **Dependency:** Parts 3 and 4 must be read first.
> Part 3 defined supervised learning and regression. Part 4 covered preprocessing.
> Now we build regression models on clean, prepared data.

---

## Table of Contents — PART 5

- [5.1 What is Regression?](#51-what-is-regression)
- [5.2 Linear Regression](#52-linear-regression)
- [5.3 Multiple Linear Regression](#53-multiple-linear-regression)
- [5.4 Polynomial Regression](#54-polynomial-regression)
- [5.5 Overfitting & Underfitting — First Encounter](#55-overfitting--underfitting--first-encounter)
- [5.6 Ridge Regression (L2 Regularization)](#56-ridge-regression-l2-regularization)
- [5.7 Lasso Regression (L1 Regularization)](#57-lasso-regression-l1-regularization)
- [5.8 Elastic Net](#58-elastic-net)
- [5.9 Model Evaluation for Regression](#59-model-evaluation-for-regression)
- [5.10 Regression Algorithm Comparison](#510-regression-algorithm-comparison)
- [5.11 Interview Questions — Part 5](#511-interview-questions--part-5)

---

## 5.1 What is Regression?

Regression is a supervised learning task where the goal is to predict a **continuous numerical value**.

```
Input features (X) → Model → Continuous output (ŷ)

Examples:
  House size, location, age  →  House price ($425,000)
  Temperature, humidity       →  Ice cream sales (350 units)
  Years of experience         →  Salary ($85,000)
```

All regression algorithms try to answer the same question:
**"Given these inputs, what number should I predict?"**

They differ in how they define the relationship between inputs and outputs.

---

## 5.2 Linear Regression

### Definition

Linear Regression models the relationship between input features and output as a **straight line** (in 2D) or a **hyperplane** (in higher dimensions).

It assumes that the output is a **linear combination** of the input features.

### Why is it needed?

It is the simplest and most interpretable regression model. Many real relationships are approximately linear over a certain range, and linear regression is the natural starting point.

### Basic Idea

Find the best-fitting straight line through the data points that minimizes prediction error.

```
      y (salary)
      |              × × ×
      |          × × /
      |       × × / ← best fit line
      |    × × /
      | × × /
      |──────────────── x (years experience)
```

### The Formula

```
ŷ = w × x + b

Where:
  ŷ  = predicted output
  x  = input feature
  w  = weight (slope) — how much y changes per unit change in x
  b  = bias (intercept) — value of y when x = 0
```

**Example:**
```
Predicting salary from years of experience:

ŷ = 5000 × x + 30000

If x = 5 years:
  ŷ = 5000 × 5 + 30000 = $55,000
```

The model learned that each additional year of experience adds $5,000 to salary, and the base salary is $30,000.

### How It Works — Training

Training finds the values of `w` and `b` that minimize the **Mean Squared Error (MSE)** on the training data.

```
Step 1: Initialize w and b to random values

Step 2: Compute predictions:
        ŷᵢ = w × xᵢ + b   for each training sample

Step 3: Compute loss (MSE):
        L = (1/n) × Σ(yᵢ - ŷᵢ)²

Step 4: Compute gradients (how much w and b contributed to error):
        ∂L/∂w = (-2/n) × Σ xᵢ(yᵢ - ŷᵢ)
        ∂L/∂b = (-2/n) × Σ (yᵢ - ŷᵢ)

Step 5: Update parameters using gradient descent:
        w = w - α × ∂L/∂w
        b = b - α × ∂L/∂b

Step 6: Repeat Steps 2–5 until loss stops decreasing
```

**Alternatively:** For linear regression, there is a closed-form (analytical) solution called the **Normal Equation**:

```
w = (XᵀX)⁻¹ Xᵀy
```

This gives the exact optimal solution without iterating — but it is computationally expensive for large datasets because of the matrix inverse `(XᵀX)⁻¹`.

### Assumptions of Linear Regression

These are important for interviews:

| Assumption | Meaning |
|---|---|
| **Linearity** | Relationship between X and y is linear |
| **Independence** | Samples are independent of each other |
| **Homoscedasticity** | Variance of errors is constant across all X values |
| **Normality** | Errors are normally distributed |
| **No multicollinearity** | Input features are not highly correlated with each other |

### Code

```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)          # train (finds optimal w and b)

print(model.coef_)       # weights (w) — one per feature
print(model.intercept_)  # bias (b)

predictions = model.predict(X_test)
```

### Advantages

- Simple and interpretable — you can read the coefficients and understand the model
- Fast to train
- Works well when the true relationship is approximately linear
- No hyperparameters to tune

### Limitations

- Assumes a strictly linear relationship — fails on curved or complex patterns
- Sensitive to outliers (MSE squares errors, so outliers are heavily penalized)
- Assumes no multicollinearity — breaks down when input features are correlated
- Underfits complex datasets

---

## 5.3 Multiple Linear Regression

### Definition

An extension of simple linear regression to **multiple input features**.

### Formula

```
ŷ = w₁x₁ + w₂x₂ + w₃x₃ + ... + wₙxₙ + b

In vector form:
ŷ = Xw + b

Where:
  X = matrix of input features (n_samples × n_features)
  w = vector of weights (one per feature)
  b = bias term
```

### Example

Predicting house price from multiple features:

```
ŷ = 200×(size_sqft) + 50000×(num_bedrooms) + 1500×(age_years) + 20000

A house: size=1500 sqft, bedrooms=3, age=10 years

ŷ = 200×1500 + 50000×3 + 1500×10 + 20000
  = 300,000 + 150,000 + 15,000 + 20,000
  = $485,000
```

### Why Coefficients Matter

Each coefficient (`w`) tells you:
- The direction of the relationship (positive weight → feature increases prediction)
- The magnitude (how much the prediction changes per unit increase in that feature)

**Important:** Coefficients are only directly comparable if the features are on the same scale. This is why standardization is important for interpretable linear models.

### Multicollinearity Problem

If two input features are highly correlated (e.g., "house size in sq ft" and "house size in sq meters"), the model has trouble assigning individual weights — the coefficients become unstable and unreliable.

```
Fix: Remove one of the correlated features, or use Ridge/Lasso regression.
```

---

## 5.4 Polynomial Regression

### Definition

Polynomial regression fits a **curved** relationship between input and output by adding polynomial (squared, cubed, etc.) terms of the features.

### Why is it needed?

Linear regression can only fit straight lines. Real data often has curved relationships.

```
Linear:     ŷ = wx + b          (straight line)
Polynomial: ŷ = w₁x + w₂x² + b  (parabola / curve)
```

**Example:**

```
House price vs. distance from city center:
  Very close → expensive (city premium)
  Medium distance → cheaper
  Far but good suburb → more expensive again

This U-shape CANNOT be captured by a straight line.
Polynomial regression can model it.
```

### How it works

You transform the input features by adding squared, cubed, or cross-product terms, then apply linear regression on the extended feature set.

```python
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

poly_model = Pipeline([
    ("poly",  PolynomialFeatures(degree=2, include_bias=False)),
    ("model", LinearRegression())
])

poly_model.fit(X_train, y_train)
```

**What `PolynomialFeatures(degree=2)` does with one feature x:**
```
Input:  [x]
Output: [x, x²]
```

**With two features x₁, x₂ and degree=2:**
```
Input:  [x₁, x₂]
Output: [x₁, x₂, x₁², x₁x₂, x₂²]
```

The model is still **linear in the parameters** — it is linear regression on transformed features. Only the feature space is non-linear.

### The Degree Tradeoff

```
Degree 1 → straight line → may underfit
Degree 2 → parabola      → often a good fit
Degree 3 → cubic          → more flexible
Degree 10+ → extremely wiggly → almost certainly overfits
```

```
         Underfit             Good Fit            Overfit
           │                    │                   │
degree=1   │        degree=2-3  │      degree=10+   │
  × ×      │             ×      │         × × ×     │
× × ─────  │           × × ×── │       ×/×\×/×\×   │
      × ×  │         ×         │     ×/           × │
            │                   │                   │
```

---

## 5.5 Overfitting & Underfitting — First Encounter

This concept is so important that it deserves its own introduction here before we continue to regularization. We'll cover it fully in Part 7.

### Underfitting

The model is **too simple** — it cannot capture the pattern in the training data.

- Training error is high
- Validation error is high
- The model fails on both seen and unseen data

**Cause:** Model is not complex enough (e.g., linear model on non-linear data).

### Overfitting

The model is **too complex** — it memorizes the training data, including the noise.

- Training error is very low
- Validation error is high
- The model fails on unseen data

**Cause:** Model has too many parameters / too flexible for the amount of data.

### Good Fit

- Training error is low
- Validation error is also low (close to training error)

```
Error
  │
  │  ←── underfitting region ──→ ← good fit → ←── overfitting region ──→
  │
  │  Training Error ──────────────────────────────────────────────────
  │
  │  Validation Error ────────────────────────────╲____╱‾‾‾‾‾‾‾‾‾‾
  │
  └─────────────────────────────────────────────────────────────────────
                                         Model Complexity →
```

**Key insight:** We want a model complex enough to learn real patterns, but not so complex that it learns noise.

**Solution:** Regularization — adding a penalty for model complexity. This is exactly what Ridge, Lasso, and Elastic Net do.

---

## 5.6 Ridge Regression (L2 Regularization)

### Definition

Ridge Regression is Linear Regression with an added **L2 penalty** that discourages the model from assigning very large weights to features.

### Why is it needed?

Linear regression with polynomial features or many correlated features tends to overfit — it assigns extreme weights to capture every data point, including noise.

Ridge penalizes large weights, forcing the model to stay simple.

### How It Works

The loss function now has two parts:

```
Ridge Loss = MSE + λ × Σwᵢ²

         ↑              ↑
  Original   L2 Penalty (sum of squared weights)
   MSE loss

Where:
  λ (lambda) = regularization strength (hyperparameter)
              Large λ → strong penalty → smaller weights → simpler model
              Small λ → weak penalty  → similar to regular linear regression
              λ = 0   → exactly regular linear regression
```

### What Ridge Does to Weights

Ridge does NOT set weights to exactly zero. It **shrinks** all weights toward zero.

```
Without Ridge: w = [5.2, -8.1, 12.4, -3.0, 9.8]
With Ridge:    w = [1.1, -2.3,  3.8, -0.8, 2.5]  ← all shrunk, none zeroed
```

### Visual Intuition

Imagine the loss landscape as a valley. Ridge adds a "gravity" that pulls all weights toward the origin — the center.

The optimal solution is a trade-off between fitting the data (MSE) and keeping weights small (L2 penalty).

### Code

```python
from sklearn.linear_model import Ridge

model = Ridge(alpha=1.0)   # alpha = λ (regularization strength)
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

### Advantages

- Handles multicollinearity well — when features are correlated, Ridge distributes the weight across them instead of assigning extreme weights
- Prevents overfitting
- More stable than ordinary linear regression when features are correlated

### Limitations

- Does not perform feature selection — all features remain in the model (just with smaller weights)
- Requires tuning λ (use cross-validation to find the best value)

---

## 5.7 Lasso Regression (L1 Regularization)

### Definition

Lasso (Least Absolute Shrinkage and Selection Operator) is Linear Regression with an **L1 penalty** — the sum of the absolute values of the weights.

### Why is it different from Ridge?

Lasso can **set weights exactly to zero** — it performs automatic feature selection.

### Loss Function

```
Lasso Loss = MSE + λ × Σ|wᵢ|

         ↑              ↑
  Original   L1 Penalty (sum of absolute weights)
   MSE loss
```

### What Lasso Does to Weights

Lasso pushes some weights all the way to zero — effectively removing those features from the model.

```
Without Lasso: w = [5.2, -8.1, 12.4, -3.0, 9.8]
With Lasso:    w = [0.0,  0.0,  4.1,  0.0, 1.3]  ← some zeroed out!
```

Features with zeroed weights are effectively **eliminated from the model**.

### Why Does L1 Create Sparsity?

The mathematical reason involves the geometry of the penalty regions:

- **L2 (Ridge)** penalty region is a circle — the optimal point usually touches the circle on a smooth curve (non-zero weights)
- **L1 (Lasso)** penalty region is a diamond — the optimal point often hits a **corner** of the diamond, where one or more weights are exactly zero

```
L2 (Ridge) — Circle penalty:       L1 (Lasso) — Diamond penalty:

    w₂                                  w₂
     │    ╭──────╮                        │    /\
     │   /        \                       │   /  \
     │  │     ●   │                       │  /    \
     │   \       /                        │ /  ●   \
     │    ╰──────╯                        │/        \
     └─────────────── w₁                 └─────────── w₁
                                          ↑
                            Optimal point hits corner
                            → w₁ = 0 (feature eliminated)
```

### Code

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=0.1)   # alpha = λ
model.fit(X_train, y_train)

# See which features were eliminated
print(model.coef_)         # zero coefficients = eliminated features
```

### Advantages

- Automatic feature selection — great for high-dimensional data with many irrelevant features
- Produces sparse models — easier to interpret
- Reduces model complexity

### Limitations

- When features are highly correlated, Lasso arbitrarily picks one and zeros out the others (unstable selection)
- May not perform as well as Ridge on multicollinear data

---

## 5.8 Elastic Net

### Definition

Elastic Net combines both L1 (Lasso) and L2 (Ridge) penalties.

### Why is it needed?

- Lasso is better at feature selection but unstable with correlated features
- Ridge is stable with correlated features but doesn't do selection
- Elastic Net gets the **best of both**

### Loss Function

```
Elastic Net Loss = MSE + λ₁ × Σ|wᵢ| + λ₂ × Σwᵢ²

                              ↑L1             ↑L2

Or equivalently (using a mixing ratio r):
  Loss = MSE + λ × [r × Σ|wᵢ| + (1-r) × Σwᵢ²]

  r = 1.0 → pure Lasso
  r = 0.0 → pure Ridge
  r = 0.5 → equal mix of both
```

### Code

```python
from sklearn.linear_model import ElasticNet

model = ElasticNet(alpha=0.1, l1_ratio=0.5)  # l1_ratio = r (mixing ratio)
model.fit(X_train, y_train)
```

### When to Use

- Large number of features with many being irrelevant (benefits from Lasso's selection)
- Some features are correlated (benefits from Ridge's stability)
- You are unsure whether Lasso or Ridge is better → try Elastic Net

---

## 5.9 Model Evaluation for Regression

After training any regression model, you need to measure how well it predicts.

---

### Mean Absolute Error (MAE)

Average of the absolute differences between predictions and true values.

```
MAE = (1/n) × Σ|yᵢ - ŷᵢ|

Where:
  yᵢ  = true value
  ŷᵢ  = predicted value
  n   = number of samples
```

**Example:**

```
True:      [100, 200, 300]
Predicted: [110, 190, 280]
Errors:    [ 10,  10,  20]

MAE = (10 + 10 + 20) / 3 = 13.33
```

**Interpretation:** On average, predictions are off by 13.33 units.

**Properties:**
- Same units as the target variable (easy to interpret)
- Robust to outliers (errors are not squared, so large errors don't dominate)

---

### Mean Squared Error (MSE)

Average of the **squared** differences between predictions and true values.

```
MSE = (1/n) × Σ(yᵢ - ŷᵢ)²
```

**Properties:**
- Penalizes large errors heavily (because errors are squared)
- Not in the same units as the target (it's in squared units — harder to interpret directly)
- Differentiable everywhere — mathematically convenient for gradient descent

---

### Root Mean Squared Error (RMSE)

Square root of MSE — brings the error back to the original units.

```
RMSE = √MSE = √[(1/n) × Σ(yᵢ - ŷᵢ)²]
```

**Properties:**
- Same units as the target (easier to interpret than MSE)
- Still penalizes large errors more than MAE
- The most commonly reported regression metric

**Example:**
```
Predicting house prices in dollars:
  RMSE = $25,000 → on average, predictions are off by $25,000
  MAE  = $18,000 → the "typical" error is $18,000
  (RMSE > MAE always, because of the squaring)
```

---

### R² (R-squared / Coefficient of Determination)

Measures how much of the variance in the target is explained by the model.

```
R² = 1 - (SS_res / SS_tot)

Where:
  SS_res = Σ(yᵢ - ŷᵢ)²   ← residual sum of squares (model error)
  SS_tot = Σ(yᵢ - ȳ)²    ← total sum of squares (variance in y)
  ȳ      = mean of y
```

**Interpretation:**

| R² Value | Meaning |
|---|---|
| R² = 1.0 | Model explains all variance — perfect predictions |
| R² = 0.8 | Model explains 80% of the variance |
| R² = 0.0 | Model is no better than predicting the mean every time |
| R² < 0   | Model is worse than predicting the mean — very bad |

**Example:**
```
House price mean = $300,000
Model RMSE = $25,000
R² = 0.85 → model explains 85% of price variation

A simple baseline (always predict $300,000) has R² = 0.
Your model (R² = 0.85) is much better than the baseline.
```

---

### Adjusted R²

A modified version of R² that penalizes adding unnecessary features.

```
Adjusted R² = 1 - [(1 - R²) × (n - 1) / (n - k - 1)]

Where:
  n = number of samples
  k = number of features
```

**Why it's needed:**

Adding any new feature, even a useless one, always increases R² slightly (by random chance). Adjusted R² penalizes for each additional feature. It increases only if the new feature genuinely improves the model more than would be expected by chance.

---

### When to Use Which Metric

| Metric | Use When |
|---|---|
| **MAE** | You want a simple, interpretable error; outliers should not dominate |
| **RMSE** | Large errors are especially bad and should be penalized more |
| **R²** | You want to understand how much of the target's variance your model explains |
| **Adjusted R²** | Comparing models with different numbers of features |

---

## 5.10 Regression Algorithm Comparison

| Algorithm | Handles Non-linearity | Feature Selection | Regularization | Best For |
|---|---|---|---|---|
| Linear Regression | No | No | No | Simple linear problems |
| Multiple Linear Regression | No | No | No | Multiple linear features |
| Polynomial Regression | Yes (via transformation) | No | No | Curved relationships |
| Ridge | No | No | L2 | Multicollinearity, overfitting |
| Lasso | No | Yes (zeros weights) | L1 | High-dimensional, sparse features |
| Elastic Net | No | Partial | L1 + L2 | Correlated + many irrelevant features |

---

## 5.11 Interview Questions — Part 5

---

**Q: What is the difference between Linear Regression and Logistic Regression?**

A: Linear Regression predicts a continuous numerical output (house price, temperature). Logistic Regression predicts a probability for a binary classification task (spam/not spam). Linear Regression outputs any real number; Logistic Regression applies a sigmoid function to map the output to [0, 1]. Despite the name, Logistic Regression is a classification algorithm, not a regression algorithm.

---

**Q: What is the difference between Ridge and Lasso?**

A: Both add a regularization penalty to Linear Regression. Ridge uses L2 penalty (sum of squared weights) — it shrinks all weights but never to exactly zero. Lasso uses L1 penalty (sum of absolute weights) — it can shrink weights to exactly zero, performing automatic feature selection. Ridge is better for multicollinear features. Lasso is better when many features are irrelevant.

---

**Q: Why does Lasso perform feature selection but Ridge does not?**

A: The geometry of the L1 constraint region (a diamond shape) means the optimal solution often falls at a corner, where one or more weights are exactly zero. The L2 constraint region (a sphere) has no corners — the optimal solution hits it on a smooth curve, giving non-zero weights to all features.

---

**Q: When would you choose Elastic Net over Ridge or Lasso?**

A: When your dataset has many features, some of which are irrelevant (like Lasso situation) AND some of the remaining features are correlated with each other (like Ridge situation). Elastic Net gives you feature selection (from L1) while being more stable than pure Lasso on correlated features (from L2).

---

**Q: What is R² and what does R² = 0 mean?**

A: R² (coefficient of determination) measures the proportion of variance in the target variable explained by the model. R² = 1 means perfect predictions. R² = 0 means the model is no better than simply predicting the mean of the target every time. R² < 0 means the model is worse than that naive baseline.

---

**Q: Why is RMSE preferred over MSE for reporting?**

A: MSE is in squared units of the target variable, which is hard to interpret. If predicting house prices in dollars, MSE would be in dollars-squared — meaningless. RMSE is the square root of MSE, bringing the error back to the original units (dollars), making it directly interpretable.

---

**Q: What is multicollinearity and how does Ridge help?**

A: Multicollinearity occurs when two or more input features are highly correlated. This makes it hard for linear regression to assign stable individual weights — small changes in data cause large changes in coefficients. Ridge regression distributes the weight across correlated features by penalizing large weights, resulting in more stable, smaller coefficients.

---

**Q: What are the assumptions of Linear Regression?**

A: (1) Linearity — the relationship between X and y must be linear. (2) Independence — observations are independent. (3) Homoscedasticity — constant variance of residuals across all X values. (4) Normality — residuals are normally distributed. (5) No multicollinearity — features should not be highly correlated with each other.

---

**Q: What is the difference between MAE and RMSE?**

A: MAE (Mean Absolute Error) averages the absolute errors — it treats all errors equally and is robust to outliers. RMSE (Root Mean Squared Error) averages squared errors before taking the root — it penalizes large errors much more heavily. Use MAE when outliers should not dominate the metric. Use RMSE when large errors are especially costly (e.g., predicting a bridge's load capacity).

---

**Q: Why does adding polynomial features risk overfitting?**

A: High-degree polynomial features create an extremely flexible model that can bend and twist to fit every training point — including noise. A degree-10 polynomial with enough flexibility can pass through every training point perfectly (training error = 0) while performing terribly on unseen data. The fix is to add regularization (Ridge/Lasso) or use cross-validation to choose the right degree.

---

**Q: What is the Normal Equation in Linear Regression?**

A: The Normal Equation is an analytical formula that directly computes the optimal weights: `w = (XᵀX)⁻¹ Xᵀy`. It gives the exact solution in one step without iteration. However, computing `(XᵀX)⁻¹` involves inverting an (n_features × n_features) matrix, which is O(n³) — computationally infeasible when the number of features is large. For large datasets, gradient descent is preferred.

---

## Summary — What You Learned in Part 5

```
REGRESSION ALGORITHMS

Linear Regression
  ↓ (add more features)
Multiple Linear Regression
  ↓ (add polynomial terms for curves)
Polynomial Regression
  ↓ (overfits? → add L2 penalty)
Ridge Regression ← good for correlated features
  ↓ (need feature selection? → switch to L1 penalty)
Lasso Regression ← zeroes out irrelevant features
  ↓ (both situations? → combine)
Elastic Net ← best of both

EVALUATION METRICS
  MAE  → average absolute error (robust, interpretable)
  MSE  → average squared error (penalizes large errors)
  RMSE → √MSE (same units as target, most used)
  R²   → % of variance explained (0 = baseline, 1 = perfect)
  Adjusted R² → penalizes unnecessary features

KEY CONCEPTS
  Regularization → penalty on weight magnitude → prevents overfitting
  L1 penalty → sparsity (feature selection)
  L2 penalty → weight shrinkage (no sparsity)
  λ (alpha) → regularization strength (hyperparameter to tune)
```

---

**Next:** PART 6 — ML Algorithms: Classification

> Say **NEXT** to continue to Part 6.
