# AI / ML / DL / GenAI — Complete Interview Study Notes
# PART 4 — Data Preprocessing & Exploratory Data Analysis

---

> **Dependency:** Part 3 introduced the ML Workflow. This part dives deep into
> Steps 3–5 of that workflow: Data Cleaning, EDA, and Feature Preparation.
> These steps happen before any model is trained.

---

## Table of Contents — PART 4

- [4.1 Why Preprocessing Matters](#41-why-preprocessing-matters)
- [4.2 Exploratory Data Analysis (EDA)](#42-exploratory-data-analysis-eda)
- [4.3 Handling Missing Values](#43-handling-missing-values)
- [4.4 Handling Duplicates](#44-handling-duplicates)
- [4.5 Encoding Categorical Variables](#45-encoding-categorical-variables)
- [4.6 Feature Scaling](#46-feature-scaling)
- [4.7 Handling Outliers](#47-handling-outliers)
- [4.8 Handling Imbalanced Data](#48-handling-imbalanced-data)
- [4.9 Preprocessing Pipeline — Putting It Together](#49-preprocessing-pipeline--putting-it-together)
- [4.10 Interview Questions — Part 4](#410-interview-questions--part-4)

---

## 4.1 Why Preprocessing Matters

Raw data is almost never ready for a machine learning model.

Think of it like cooking: you don't throw a whole raw chicken into an oven in its packaging. You clean it, cut it, season it, and prepare it. Data preprocessing is that preparation step.

**What happens if you skip it?**

| Problem | Consequence |
|---|---|
| Missing values | Most ML models crash or give wrong results |
| Un-encoded text | Models only understand numbers — text breaks them |
| Un-scaled features | Models with distances (KNN, SVM) are biased toward large-value features |
| Outliers | Linear models are heavily distorted |
| Imbalanced classes | Models predict the majority class and ignore the minority |

**Rule:** The quality of your preprocessing directly determines the ceiling of your model's performance.

---

## 4.2 Exploratory Data Analysis (EDA)

### Definition

EDA is the process of **understanding your dataset** before building any model. It is part detective work, part statistics, and part visualization.

### Why is it needed?

Before modeling, you need to answer:
- What does the data look like?
- Are there missing values?
- What is the distribution of each feature?
- Are any features correlated?
- Are there obvious patterns or anomalies?
- Is the target variable balanced?

Skipping EDA is like driving to an unknown city without looking at a map first.

---

### 4.2.1 First Look at the Data

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data.csv")

# Shape
print(df.shape)           # (rows, columns)

# First few rows
print(df.head())

# Data types and nulls
print(df.info())

# Statistical summary
print(df.describe())

# Missing values per column
print(df.isnull().sum())

# Count of unique values per column
print(df.nunique())
```

**`describe()` output explanation:**

| Stat | Meaning |
|---|---|
| count | Number of non-null values |
| mean | Average |
| std | Standard deviation (spread) |
| min | Minimum value |
| 25% | First quartile — 25% of data is below this |
| 50% | Median |
| 75% | Third quartile — 75% of data is below this |
| max | Maximum value |

---

### 4.2.2 Distribution Analysis

Check how values are spread in each feature.

**Histogram** — shows the frequency distribution of a numerical feature:

```python
sns.histplot(df["age"], bins=30, kde=True)
plt.title("Age Distribution")
plt.show()
```

**Box Plot** — shows median, quartiles, and outliers:

```
     |       ┌──────┬───────┐         •
     |───────┤  IQR │       ├─────────
     |       └──────┴───────┘
          Q1  median      Q3      outlier

IQR = Q3 - Q1 (Interquartile Range)
Whiskers extend to Q1 - 1.5×IQR and Q3 + 1.5×IQR
Points beyond whiskers = outliers
```

```python
sns.boxplot(x=df["salary"])
```

**What to look for:**
- **Normal distribution** — symmetric bell curve, common in many ML assumptions
- **Right skew** — long tail on the right (e.g., income data)
- **Left skew** — long tail on the left
- **Bimodal** — two peaks, might indicate two distinct sub-populations

---

### 4.2.3 Correlation Analysis

Correlation measures how linearly related two features are.

```python
# Correlation matrix
corr = df.corr()

# Visualize as heatmap
sns.heatmap(corr, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Matrix")
```

**How to read it:**
- Value close to +1 → strongly positively correlated
- Value close to -1 → strongly negatively correlated
- Value close to 0 → no linear correlation

**What to do with high correlation:**
- If two features are highly correlated (e.g., 0.95), they carry almost the same information
- Keep one, drop the other — reduces multicollinearity
- Or use PCA to combine them

---

### 4.2.4 Target Variable Analysis

Always understand what you're predicting:

```python
# For classification — check class balance
print(df["churn"].value_counts())
sns.countplot(x="churn", data=df)

# For regression — check distribution
sns.histplot(df["house_price"])
```

**If the target is heavily imbalanced** → you need to address this before training (covered in Section 4.8).

---

### 4.2.5 Scatter Plots — Feature vs Target

Check which features relate to the target:

```python
# How does age relate to salary?
sns.scatterplot(x="age", y="salary", data=df)

# Pairplot — all features vs each other
sns.pairplot(df, hue="churn")
```

---

### 4.2.6 EDA Summary Checklist

```
EDA Checklist:
  ✓ Shape of dataset (rows, columns)
  ✓ Data types of each column
  ✓ Missing values per column
  ✓ Duplicate rows
  ✓ Distribution of numerical features (histograms)
  ✓ Distribution of categorical features (bar charts)
  ✓ Outliers (box plots)
  ✓ Correlation matrix (heatmap)
  ✓ Target variable distribution (balanced?)
  ✓ Feature vs target relationships (scatter plots)
```

---

## 4.3 Handling Missing Values

### Types of Missingness

Before choosing a strategy, understand WHY data is missing:

| Type | Meaning | Example |
|---|---|---|
| **MCAR** — Missing Completely At Random | Missingness is unrelated to any data | Survey response accidentally skipped |
| **MAR** — Missing At Random | Missingness relates to other observed features | Older patients less likely to report income |
| **MNAR** — Missing Not At Random | Missingness relates to the missing value itself | People with very high income skip income field |

**Why it matters:** MCAR is the easiest to handle. MNAR is the most problematic — imputing incorrectly can introduce bias.

---

### Strategy 1 — Drop Rows

Remove all rows that have missing values.

```python
df_clean = df.dropna()
```

**When to use:**
- Very few rows are missing (< 5% of data)
- The missingness is MCAR

**Risk:** Lose valuable data. If 30% of rows have at least one missing value, dropping them loses too much.

---

### Strategy 2 — Drop Columns

Remove the entire column if it has too many missing values.

```python
# Drop columns where more than 40% of values are missing
threshold = 0.4
df_clean = df.loc[:, df.isnull().mean() < threshold]
```

**When to use:** Column has more missing values than it has data — it carries little information.

---

### Strategy 3 — Mean / Median / Mode Imputation

Fill missing values with the mean (numerical), median (numerical with outliers), or mode (categorical).

```python
from sklearn.impute import SimpleImputer

# For numerical columns — fill with median
num_imputer = SimpleImputer(strategy="median")
df["salary"] = num_imputer.fit_transform(df[["salary"]])

# For categorical columns — fill with most frequent value
cat_imputer = SimpleImputer(strategy="most_frequent")
df["city"] = cat_imputer.fit_transform(df[["city"]])
```

**When to use:**
- Mean: data is normally distributed, no extreme outliers
- Median: data is skewed or has outliers
- Mode: categorical data

**Risk:** Imputation reduces variance and can distort relationships between features.

---

### Strategy 4 — Forward Fill / Backward Fill

Used for **time-series data**. Fill missing value with the previous (forward fill) or next (backward fill) known value.

```python
df["temperature"].fillna(method="ffill", inplace=True)  # forward fill
df["temperature"].fillna(method="bfill", inplace=True)  # backward fill
```

**When to use:** Sensor data, stock prices, daily metrics — where yesterday's value is a reasonable estimate of today's.

---

### Strategy 5 — Model-Based Imputation

Use a ML model to predict the missing value from other features.

```python
# Example: predict missing "salary" using "age", "experience", "education"
from sklearn.experimental import enable_iterative_imputer
from sklearn.impute import IterativeImputer

imputer = IterativeImputer(random_state=42)
df_imputed = imputer.fit_transform(df)
```

**When to use:** When missingness is complex (MAR/MNAR) and accuracy is critical.
**Risk:** More complex, computationally expensive.

---

### Strategy 6 — Missing Indicator

Instead of imputing, create a new binary column that marks whether a value was missing.

```python
df["salary_was_missing"] = df["salary"].isnull().astype(int)
df["salary"].fillna(df["salary"].median(), inplace=True)
```

**Why:** The fact that a value is missing might itself be informative — e.g., people who don't fill in income might have a different churn rate. The model can learn this pattern.

---

### Summary — Choosing a Missing Value Strategy

```
How much data is missing?
  │
  ├── < 5%  → Drop rows (if MCAR) or simple imputation
  │
  ├── 5-40% → Impute (mean/median/mode or model-based)
  │            + Add missing indicator column
  │
  └── > 40% → Drop the entire column
```

---

## 4.4 Handling Duplicates

```python
# Check for duplicates
print(df.duplicated().sum())

# Remove duplicates
df = df.drop_duplicates()
```

**Why:** Duplicates give the model extra "weight" toward certain examples, distorting training. Especially problematic if the same sample appears in both training and test sets.

---

## 4.5 Encoding Categorical Variables

ML models require numerical input. Categorical text values must be converted to numbers.

---

### Label Encoding

Assign each category a unique integer.

```
City:    Delhi → 0,  Mumbai → 1,  Pune → 2
```

```python
from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
df["city_encoded"] = le.fit_transform(df["city"])
```

**Problem:** Implies an ordering — the model might think Pune (2) > Mumbai (1) > Delhi (0), which is meaningless. This is incorrect for nominal categories.

**When to use:** Only for **ordinal** data where order is meaningful (Small=0, Medium=1, Large=2) or for tree-based models that handle this correctly.

---

### One-Hot Encoding

Create a new binary column for each category.

```
City:  Delhi  Mumbai  Pune
         1      0       0   ← Delhi
         0      1       0   ← Mumbai
         0      0       1   ← Pune
```

```python
df_encoded = pd.get_dummies(df, columns=["city"], drop_first=True)
# drop_first=True avoids the "dummy variable trap" (multicollinearity)
```

**When to use:** Nominal categorical data with no meaningful order.

**Problem:** If a feature has 1000 unique categories, you get 1000 new columns — very high dimensionality. This is called the **curse of dimensionality**.

---

### Ordinal Encoding

Manually assign ordered integer values that respect the category's natural order.

```python
from sklearn.preprocessing import OrdinalEncoder

# Define the order
order = [["High School", "Bachelor", "Master", "PhD"]]
enc = OrdinalEncoder(categories=order)
df["edu_encoded"] = enc.fit_transform(df[["education"]])
```

**When to use:** Ordinal data (size, rating, education level).

---

### Target Encoding (Mean Encoding)

Replace each category with the mean of the target variable for that category.

```
City:   Avg Churn Rate
Delhi → 0.35
Mumbai → 0.48
Pune   → 0.22
```

```python
target_mean = df.groupby("city")["churn"].mean()
df["city_encoded"] = df["city"].map(target_mean)
```

**Advantage:** Handles high-cardinality categorical features without exploding dimensions.

**Risk:** **Data leakage** — if you compute target means using the full dataset (including test data). Always compute from training data only and apply to validation/test.

---

### Comparison Table

| Encoding | When to Use | Problem |
|---|---|---|
| Label Encoding | Ordinal features, tree models | Implies false order for nominal data |
| One-Hot Encoding | Nominal features, low cardinality | High dimensions with many categories |
| Ordinal Encoding | Ordinal features | Must specify correct order manually |
| Target Encoding | High-cardinality nominal features | Risk of data leakage |

---

## 4.6 Feature Scaling

### Why Scaling is Needed

Many ML algorithms calculate **distances** between data points or depend on the **magnitude** of feature values. If one feature is in thousands (salary: 50,000) and another is in single digits (age: 30), the algorithm pays too much attention to salary and almost ignores age.

```
Distance-based algorithms affected by scale:
  - KNN (K-Nearest Neighbors)
  - SVM (Support Vector Machine)
  - K-Means clustering
  - PCA
  - Neural Networks (converge faster with scaled data)

Tree-based algorithms NOT affected by scale:
  - Decision Tree
  - Random Forest
  - XGBoost / LightGBM / CatBoost
```

---

### Normalization (Min-Max Scaling)

Scale all values to a specific range, usually [0, 1].

```
x_scaled = (x - x_min) / (x_max - x_min)
```

**Example:**
```
Salary: [20000, 50000, 80000, 100000]
Min = 20000, Max = 100000

Scaled: [0.0, 0.375, 0.75, 1.0]
```

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()
df[["salary", "age"]] = scaler.fit_transform(df[["salary", "age"]])
```

**When to use:**
- Data has a known fixed range
- Algorithms expect input in [0, 1] (e.g., neural networks with sigmoid output)
- No significant outliers

**Problem:** Very sensitive to outliers. If one value is 1,000,000 in a salary column, all other values get compressed near 0.

---

### Standardization (Z-Score Normalization)

Transform data to have **mean = 0** and **standard deviation = 1**.

```
x_scaled = (x - μ) / σ

Where:
  μ = mean of the feature
  σ = standard deviation of the feature
```

**Example:**
```
Salary: [20000, 50000, 80000, 100000]
Mean μ = 62500, Std σ = 33541

Scaled: [-1.27, -0.37, 0.52, 1.12]
```

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
df[["salary", "age"]] = scaler.fit_transform(df[["salary", "age"]])
```

**When to use:**
- Data is approximately normally distributed
- Algorithms assume zero-mean input (linear models, neural networks, PCA, SVM)
- Data has significant outliers (standardization is more robust than min-max)

---

### Robust Scaling

Uses the **median and IQR** instead of mean and standard deviation — very robust to outliers.

```
x_scaled = (x - median) / IQR
```

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()
df[["salary"]] = scaler.fit_transform(df[["salary"]])
```

**When to use:** Data with many outliers that you can't remove.

---

### Critical Rule: Fit on Train, Transform on All

**Always** fit the scaler only on training data. Apply the same scaler to validation and test data.

```python
scaler = StandardScaler()

# Fit ONLY on training data
scaler.fit(X_train)

# Transform all sets using the training statistics
X_train_scaled = scaler.transform(X_train)
X_val_scaled   = scaler.transform(X_val)
X_test_scaled  = scaler.transform(X_test)
```

**Why:** Fitting on the full dataset (including test) would leak test set statistics into training — a form of data leakage.

---

### Comparison

| Method | Formula | Sensitive to Outliers | When to Use |
|---|---|---|---|
| Min-Max | (x - min) / (x - max) | Very | Fixed range, no outliers |
| Standardization | (x - μ) / σ | Moderately | Normal distribution |
| Robust | (x - median) / IQR | No | Heavy outliers |

---

## 4.7 Handling Outliers

### What is an Outlier?

A data point that is significantly different from most other points.

```
Ages: [22, 24, 25, 23, 26, 24, 137]
                                 ↑
                           Clear outlier
```

**Outliers can be:**
- **Errors** → data entry mistake (age = 137)
- **Valid extremes** → a genuine high-income earner in salary data
- **Signal** → in fraud detection, the outlier IS what you're looking for

---

### Detection Method 1 — IQR (Interquartile Range)

```
Q1 = 25th percentile
Q3 = 75th percentile
IQR = Q3 - Q1

Lower bound = Q1 - 1.5 × IQR
Upper bound = Q3 + 1.5 × IQR

Any point outside these bounds = outlier
```

```python
Q1 = df["salary"].quantile(0.25)
Q3 = df["salary"].quantile(0.75)
IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

# Identify outliers
outliers = df[(df["salary"] < lower) | (df["salary"] > upper)]
```

---

### Detection Method 2 — Z-Score

```
z = (x - μ) / σ

If |z| > 3 → outlier
```

Points more than 3 standard deviations from the mean are flagged.

```python
from scipy import stats

z_scores = np.abs(stats.zscore(df["salary"]))
outliers = df[z_scores > 3]
```

**When to use:** Data is approximately normally distributed.

---

### What to Do With Outliers

| Strategy | How | When |
|---|---|---|
| **Remove** | Drop the outlier rows | Error confirmed; small number of outliers |
| **Cap / Clip** | Replace with boundary value | Valid but extreme; keep in dataset |
| **Log transform** | Apply log to compress the scale | Right-skewed data (income, population) |
| **Keep** | Leave unchanged | Outliers are meaningful signal (fraud) |
| **Robust methods** | Use median-based algorithms | Can't remove but don't want to be distorted |

**Clipping example:**
```python
# Cap salaries at the 99th percentile
upper_cap = df["salary"].quantile(0.99)
df["salary"] = df["salary"].clip(upper=upper_cap)
```

**Log transform example:**
```python
# Compress right-skewed salary distribution
df["log_salary"] = np.log1p(df["salary"])  # log(1 + x) handles zero values
```

---

## 4.8 Handling Imbalanced Data

### What is Class Imbalance?

When one class in your target variable has far more samples than another.

```
Fraud Detection Dataset:
  Not Fraud: 990,000 samples  (99%)
  Fraud:       10,000 samples  (1%)
```

**The problem:** A model that always predicts "Not Fraud" achieves 99% accuracy. But it has zero ability to detect actual fraud — which is the entire point.

**Why accuracy is misleading here:** Accuracy measures overall correctness, not per-class performance. For imbalanced data, you must use Precision, Recall, and F1-score (covered in Part 5).

---

### Strategy 1 — Oversampling (Increase the minority)

Duplicate samples from the minority class to balance the dataset.

**Simple random oversampling:**
```python
from imblearn.over_sampling import RandomOverSampler

ros = RandomOverSampler(random_state=42)
X_resampled, y_resampled = ros.fit_resample(X_train, y_train)
```

**Problem:** Just duplicating exact copies adds no new information — the model may overfit to those exact samples.

---

### Strategy 2 — SMOTE (Synthetic Minority Over-sampling Technique)

SMOTE creates **synthetic** (artificial) minority class samples rather than exact duplicates.

**How SMOTE works:**
```
Pick a minority class sample
      ↓
Find its K nearest minority class neighbors
      ↓
Randomly interpolate between the sample and one neighbor
      ↓
New synthetic sample is created along that line
```

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(random_state=42)
X_resampled, y_resampled = smote.fit_resample(X_train, y_train)
```

**Advantage:** Generates new, plausible minority examples instead of just copying.
**Risk:** Can generate unrealistic samples if the feature space is complex.

---

### Strategy 3 — Undersampling (Reduce the majority)

Randomly remove samples from the majority class.

```python
from imblearn.under_sampling import RandomUnderSampler

rus = RandomUnderSampler(random_state=42)
X_resampled, y_resampled = rus.fit_resample(X_train, y_train)
```

**Problem:** Throws away potentially useful data.

**When to use:** Dataset is very large and computation is a concern. You have so much majority class data that removing some doesn't matter.

---

### Strategy 4 — Class Weights

Instead of resampling, tell the model to **pay more attention to the minority class** by assigning it a higher weight in the loss function.

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(class_weight="balanced", random_state=42)
# "balanced" automatically computes weights inversely proportional to class frequencies
```

**This is often the simplest and most effective approach.** No data manipulation needed.

---

### Strategy 5 — Threshold Adjustment

ML classifiers output a probability (e.g., 0.7 = 70% likely to be fraud). The default decision threshold is 0.5.

For imbalanced data, lowering the threshold catches more minority class instances at the cost of more false positives.

```python
probs = model.predict_proba(X_test)[:, 1]  # probability of fraud

# Default threshold
predictions_default = (probs >= 0.5).astype(int)

# Lowered threshold — catches more fraud, but also more false alarms
predictions_adjusted = (probs >= 0.3).astype(int)
```

---

### Summary — Choosing an Imbalance Strategy

```
Imbalance ratio mild (e.g., 80:20)?
  → Class weights usually sufficient

Imbalance extreme (e.g., 99:1)?
  → SMOTE + class weights
  → Or SMOTE + undersampling combination

Dataset very large?
  → Undersampling is computationally cheap

Can't modify data?
  → Threshold adjustment after training
```

---

## 4.9 Preprocessing Pipeline — Putting It Together

In production, preprocessing steps must be applied consistently every time — during training AND during inference. **Pipelines** ensure this.

```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.impute import SimpleImputer
from sklearn.ensemble import RandomForestClassifier

# Define which columns are numerical vs categorical
num_features = ["age", "salary", "experience"]
cat_features = ["city", "department"]

# Numerical pipeline: impute missing → scale
num_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler",  StandardScaler())
])

# Categorical pipeline: impute missing → encode
cat_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("encoder", OneHotEncoder(handle_unknown="ignore"))
])

# Combine both
preprocessor = ColumnTransformer([
    ("num", num_pipeline, num_features),
    ("cat", cat_pipeline, cat_features)
])

# Full pipeline: preprocessing + model
full_pipeline = Pipeline([
    ("preprocessor", preprocessor),
    ("model", RandomForestClassifier(n_estimators=100))
])

# Train
full_pipeline.fit(X_train, y_train)

# Predict on new data — preprocessing is applied automatically
predictions = full_pipeline.predict(X_test)
```

**Why pipelines matter:**
- Prevents data leakage (scaler fit only on training data)
- Makes deployment clean — one `.predict()` call handles everything
- Easy to save and load as a single object

---

## 4.10 Interview Questions — Part 4

---

**Q: What is the difference between normalization and standardization?**

A: Normalization (Min-Max scaling) scales data to a fixed range, typically [0, 1], using `(x - min) / (max - min)`. Standardization (Z-score) transforms data to have mean 0 and standard deviation 1, using `(x - mean) / std`. Normalization is sensitive to outliers; standardization is more robust. Use normalization when data has no outliers and a known range. Use standardization when data follows a normal distribution or has outliers.

---

**Q: Why do tree-based models like Random Forest not need feature scaling?**

A: Decision trees split features based on threshold comparisons (e.g., "age > 30"), not on distances or magnitudes. Scaling doesn't change which threshold is optimal — just the numbers. But for KNN, SVM, or neural networks, the absolute magnitude of features directly affects computation, so scaling is essential.

---

**Q: What is one-hot encoding and what is the dummy variable trap?**

A: One-hot encoding creates a binary column for each category. The dummy variable trap occurs when you include all one-hot columns — they are perfectly collinear (one column can be derived from all the others). For example, if "city" has Delhi and Mumbai columns, knowing both are 0 means it must be Pune. This causes multicollinearity in linear models. The fix is `drop_first=True` — drop one category column.

---

**Q: What is SMOTE and why is it better than random oversampling?**

A: SMOTE (Synthetic Minority Over-sampling Technique) creates new synthetic minority class examples by interpolating between existing minority examples and their nearest neighbors. Random oversampling just duplicates existing samples — this gives the model no new information and can cause overfitting to those specific points. SMOTE generates plausible new variations, giving the model a richer view of the minority class.

---

**Q: What is the difference between MCAR, MAR, and MNAR?**

A: MCAR (Missing Completely At Random) — missingness is unrelated to any data, purely random. MAR (Missing At Random) — missingness is related to other observed variables but not to the missing value itself. MNAR (Missing Not At Random) — missingness is related to the value that is missing (e.g., very sick patients skip health surveys). MNAR is the most problematic because simple imputation introduces bias.

---

**Q: Why must you fit the scaler on training data only?**

A: If you fit the scaler on the full dataset (including test data), the scaler's mean and standard deviation are computed using test data statistics. This leaks information from the test set into training — an indirect form of data leakage. The model benefits from "knowing" test set statistics, giving artificially inflated performance.

---

**Q: When would you use median imputation over mean imputation?**

A: Use median imputation when the feature has a skewed distribution or significant outliers. The mean is pulled toward extreme values (e.g., average salary of 10 people jumps dramatically if one person earns $10 million). The median is resistant to outliers — it represents the "middle" value regardless of extremes.

---

**Q: Why is accuracy misleading for imbalanced datasets?**

A: With 99% of samples in one class, a model that always predicts that class achieves 99% accuracy without learning anything useful. Accuracy treats all errors equally. For imbalanced data, use Precision, Recall, and F1-score, which evaluate performance on each class separately, or AUC-ROC which evaluates across all decision thresholds.

---

**Q: What is target encoding and what is its biggest risk?**

A: Target encoding replaces each categorical value with the mean of the target variable for that category. Its biggest risk is data leakage — if you compute the target means using the full dataset before splitting, the model indirectly sees test set labels during training. Always compute target encoding means from training data only.

---

**Q: What is a preprocessing pipeline and why is it important?**

A: A pipeline is a sequential chain of data transformations applied automatically. It ensures that: (1) the scaler is fit only on training data, (2) the same transformations are applied consistently during both training and inference, and (3) the model and preprocessing can be saved and deployed as a single unit. Without a pipeline, it is easy to accidentally apply different preprocessing at training vs. deployment.

---

**Q: What does EDA tell you and what are the key plots to know?**

A: EDA (Exploratory Data Analysis) helps you understand the data before modeling. Key plots:
- **Histogram** — distribution of a numerical feature
- **Box plot** — spread, quartiles, and outliers
- **Scatter plot** — relationship between two numerical features
- **Bar chart** — frequency of categorical values
- **Correlation heatmap** — linear relationships between all feature pairs
- **Count plot** — distribution of the target variable (class balance)

---

## Summary — What You Learned in Part 4

```
DATA PREPROCESSING FLOW

Raw Dataset
     │
     ├── EDA
     │   ├── Shape, types, null counts
     │   ├── Distribution of each feature
     │   ├── Correlation matrix
     │   └── Target variable balance
     │
     ├── Handle Missing Values
     │   ├── Drop (< 5% missing, MCAR)
     │   ├── Mean/Median/Mode imputation
     │   ├── Forward/backward fill (time-series)
     │   ├── Model-based imputation
     │   └── Missing indicator column
     │
     ├── Handle Duplicates → drop_duplicates()
     │
     ├── Encode Categorical Features
     │   ├── Label encoding (ordinal, trees)
     │   ├── One-hot encoding (nominal)
     │   ├── Ordinal encoding (ordered categories)
     │   └── Target encoding (high cardinality)
     │
     ├── Scale Numerical Features
     │   ├── Min-Max (no outliers, fixed range)
     │   ├── Standardization (normal dist, outliers ok)
     │   └── Robust scaling (heavy outliers)
     │
     ├── Handle Outliers
     │   ├── Detect: IQR or Z-score
     │   └── Treat: remove, clip, log-transform, keep
     │
     ├── Handle Imbalanced Data
     │   ├── SMOTE (synthetic oversampling)
     │   ├── Random undersampling
     │   ├── Class weights
     │   └── Threshold adjustment
     │
     └── Bundle into a Pipeline
         → fit on train only
         → transform train + val + test
         → deploy as single object
```

---

**Next:** PART 5 — ML Algorithms: Regression

> Say **NEXT** to continue to Part 5.
