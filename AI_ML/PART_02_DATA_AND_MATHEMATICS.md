# AI / ML / DL / GenAI — Complete Interview Study Notes
# PART 2 — Data Fundamentals & Mathematics for ML

---

> **Dependency:** PART 1 — Foundations must be read first.

---

## Table of Contents — PART 2

- [2.1 What is Data?](#21-what-is-data)
- [2.2 Types of Data](#22-types-of-data)
- [2.3 Data Types (Column-Level)](#23-data-types-column-level)
- [2.4 Dataset Concepts](#24-dataset-concepts)
- [2.5 Data Problems](#25-data-problems)
- [2.6 Linear Algebra for ML](#26-linear-algebra-for-ml)
- [2.7 Probability for ML](#27-probability-for-ml)
- [2.8 Statistics for ML](#28-statistics-for-ml)
- [2.9 Calculus for ML](#29-calculus-for-ml)
- [2.10 Optimization — The Bridge to Training](#210-optimization--the-bridge-to-training)
- [2.11 Interview Questions — Part 2](#211-interview-questions--part-2)

---

## 2.1 What is Data?

Data is the raw material of AI/ML.

Without data, there is no learning. A model learns by finding **patterns in data**. The quality, quantity, and structure of your data directly determine how good your model can become.

> "Garbage in, garbage out" — a model trained on bad data will make bad predictions, no matter how sophisticated the algorithm.

---

## 2.2 Types of Data

### Structured Data

Organized in rows and columns, like a spreadsheet or SQL table.

```
| Age | Salary | Department | Churn |
|-----|--------|------------|-------|
| 25  | 50000  | Sales      | No    |
| 40  | 90000  | Tech       | Yes   |
```

- Easy to store, query, and process
- Traditional ML algorithms (Linear Regression, Decision Trees) work directly on this
- Most corporate data lives here (CRMs, ERP systems, databases)

---

### Unstructured Data

No predefined format. The meaning is not immediately machine-readable.

Examples:
- **Text** — emails, articles, social media posts
- **Images** — photos, medical scans, satellite imagery
- **Audio** — speech, music, recordings
- **Video** — movies, surveillance footage

**Challenge:** You cannot feed raw text or images directly into most ML models. You first need to convert them into numbers (vectors/embeddings).

---

### Semi-Structured Data

Has some organization but does not fit neatly into rows and columns.

Examples:
- **JSON** — API responses, config files
- **XML** — legacy data formats
- **HTML** — web pages
- **Log files** — server logs with timestamps and messages

```json
{
  "user": "alice",
  "age": 30,
  "orders": [
    {"item": "laptop", "price": 1200},
    {"item": "mouse", "price": 25}
  ]
}
```

---

### Summary

| Type | Format | Examples | ML Challenge |
|---|---|---|---|
| Structured | Rows & columns | SQL tables, CSV | Minimal — ready to use |
| Unstructured | No fixed format | Text, images, audio | Must convert to numbers |
| Semi-structured | Partial format | JSON, XML, logs | Needs parsing/extraction |

---

## 2.3 Data Types (Column-Level)

Within a dataset, each column (feature) has a type. Knowing the type tells you how to handle it.

---

### Numerical (Quantitative)

Numbers that represent measurable quantities.

- **Continuous** — can take any value in a range
  - Age: 25.5, Salary: 72345.67, Temperature: 36.8°C
- **Discrete** — countable whole numbers
  - Number of children: 0, 1, 2, 3

**How to handle:** Use directly in most algorithms. May need scaling (normalization/standardization).

---

### Categorical (Qualitative)

Values that represent categories or groups.

- City: Delhi, Mumbai, Bangalore
- Color: Red, Blue, Green
- Gender: Male, Female, Other

**Important:** ML models work with numbers, not strings. You must **encode** categorical columns into numbers before training.

---

### Ordinal

A special type of categorical data where the categories have a meaningful order, but the spacing between them is not necessarily equal.

- Education: High School < Bachelor < Master < PhD
- Rating: Poor < Fair < Good < Excellent
- Size: Small < Medium < Large < XL

**Why it's different from regular categorical:** Order matters. "PhD > Bachelor" is meaningful.

---

### Binary

Only two possible values.

- Spam / Not Spam
- True / False
- 1 / 0

---

### Text

Free-form text data. Requires NLP preprocessing before it can be used in models.

---

### Image

Grids of pixel values. A grayscale image of size 28×28 is a 784-dimensional vector.

```
A 28x28 grayscale image:
Each pixel = a number from 0 (black) to 255 (white)
Total values = 28 × 28 = 784 numbers
```

---

### Time-Series

Data collected over time, where the order matters.

- Stock prices over days
- Sensor readings every second
- Website traffic per hour

**Key property:** The timestamp order is meaningful. You cannot shuffle rows randomly the way you can with tabular data.

---

### Summary Table

| Type | Example | Encoding Needed |
|---|---|---|
| Numerical | Age, Salary | Scale if needed |
| Categorical | City, Color | One-hot / Label encoding |
| Ordinal | Rating, Education | Ordinal encoding |
| Binary | Spam flag | Already 0/1 |
| Text | Reviews | Tokenization + Embeddings |
| Image | Photos | Pixel normalization + CNN |
| Time-series | Stock prices | Specialized models (RNN/LSTM) |

---

## 2.4 Dataset Concepts

These are the building blocks you will use in every ML project.

---

### Features

A **feature** is an input variable — a column in your dataset that the model uses to make predictions.

Also called: attributes, variables, predictors, inputs, X.

```
| Age | Salary | Department | ← These are features
```

---

### Labels

A **label** is the output variable — the thing the model is trying to predict.

Also called: target, output, y, dependent variable.

```
| Churn | ← This is the label (what we want to predict)
```

---

### Samples

A **sample** is one row in your dataset — one observation, one data point.

```
| 25 | 50000 | Sales | No | ← This is one sample
```

---

### Training / Validation / Test Split

This is one of the most important concepts in ML.

```
Full Dataset
     |
     ├── Training Set   (~70-80%) ← Model learns from this
     ├── Validation Set (~10-15%) ← Tune model during development
     └── Test Set       (~10-15%) ← Final honest evaluation
```

**Why split?**

- If you train and test on the same data, the model will "memorize" the answers — it will look great on paper but fail on new data.
- The test set simulates "data the model has never seen before."
- **The test set must never be used during training or tuning.** Treat it as a sealed envelope you open only at the very end.

---

### Independent vs Dependent Variables

| Term | Same As | Meaning |
|---|---|---|
| Independent variable | Feature, X | The input — what you control or observe |
| Dependent variable | Label, y | The output — what depends on the input |

---

## 2.5 Data Problems

Real-world data is almost never clean. These are the problems you will encounter in every project.

---

### Missing Values

Some cells in your dataset are empty.

```
| Age | Salary | City    |
| 25  | 50000  | Delhi   |
| 30  | NaN    | Mumbai  |  ← Salary is missing
| NaN | 70000  | Pune    |  ← Age is missing
```

**Why it happens:** Data entry errors, system failures, optional survey questions, merging from different sources.

**Impact:** Most ML models cannot handle NaN (Not a Number). You must address missing values before training.

---

### Duplicate Data

Rows that appear more than once.

**Why it matters:** Duplicates distort training — the model sees the same example more than once, which biases it toward those samples.

---

### Outliers

Data points that are far from the rest of the data.

```
Salaries: [45000, 48000, 50000, 52000, 1000000]
                                         ↑ outlier
```

**Impact:**
- Mean is heavily distorted by outliers
- Many models are sensitive to outliers (e.g., Linear Regression)
- Sometimes outliers are errors; sometimes they are the most interesting data points (fraud detection!)

---

### Noisy Data

Data that contains random errors or irrelevant information.

- A blurry photo
- A mistyped survey response ("agee" instead of "age")
- Sensor readings with random spikes

---

### Imbalanced Data

One class in the label has far more examples than others.

```
Spam Detection Dataset:
  Not Spam: 95,000 samples
  Spam:      5,000 samples
```

**Impact:** A model that always predicts "Not Spam" would be 95% accurate but completely useless. This is why accuracy alone is a misleading metric for imbalanced data.

---

### Data Leakage

Data leakage happens when information from the **future** or from the **test set** accidentally gets into the training data.

**Example:** You are predicting whether a loan will default. You include the column "loan_recovery_amount" in your features — but this column only has a value **after** the loan has already defaulted. So you are training on future information.

**Impact:** The model performs perfectly during training but completely fails in production.

**Rule:** Features must only contain information that would realistically be available at the time of making the prediction.

---

## 2.6 Linear Algebra for ML

Linear algebra is the mathematical language of ML. Every input, weight, and output is a number, vector, or matrix.

**Intuition first:** Think of linear algebra as the math of organizing and transforming groups of numbers efficiently.

---

### 2.6.1 Scalars

A single number.

```
Temperature = 36.8
Learning rate = 0.001
```

---

### 2.6.2 Vectors

An ordered list of numbers. Represents a point or direction in space.

```
v = [2, 5, 1, 8]  — a vector with 4 elements
```

**In ML context:**
- A single data sample's features = a vector
- A word embedding = a vector (e.g., 300 dimensions)
- Model weights for a layer = a vector

```python
import numpy as np
v = np.array([2, 5, 1, 8])
print(v.shape)  # (4,) — a vector with 4 elements
```

---

### 2.6.3 Matrices

A 2D grid of numbers (rows × columns).

```
M = [[1, 2, 3],
     [4, 5, 6],
     [7, 8, 9]]

Shape: (3, 3) — 3 rows, 3 columns
```

**In ML context:**
- Your entire training dataset = a matrix (samples × features)
- Weight matrix of a neural network layer = a matrix
- Image = a 2D matrix of pixel values

```python
M = np.array([[1, 2, 3], [4, 5, 6]])
print(M.shape)  # (2, 3) — 2 rows, 3 columns
```

---

### 2.6.4 Tensors

A generalization of scalars, vectors, and matrices to N dimensions.

```
Scalar     → 0D tensor  (single number)
Vector     → 1D tensor  [1, 2, 3]
Matrix     → 2D tensor  [[1,2],[3,4]]
3D tensor  → e.g., an image batch: (batch_size, height, width)
4D tensor  → e.g., video: (batch, time, height, width)
```

**In ML context:** PyTorch and TensorFlow work with tensors. An RGB image is a 3D tensor: `(height, width, channels)` where channels = 3 (Red, Green, Blue).

```python
# A batch of 32 images, each 28x28 grayscale
batch = np.zeros((32, 28, 28))
print(batch.shape)  # (32, 28, 28) — a 3D tensor
```

---

### 2.6.5 Matrix Multiplication

This is the most important linear algebra operation in deep learning.

**Rule:** To multiply matrices A (m×n) and B (n×p), the number of columns in A must equal the number of rows in B. The result is a matrix of shape (m×p).

```
A = [[1, 2],    B = [[5, 6],
     [3, 4]]         [7, 8]]

A × B = [[1×5 + 2×7,  1×6 + 2×8],   =  [[19, 22],
          [3×5 + 4×7,  3×6 + 4×8]]       [43, 50]]
```

**Why it matters:** A neural network layer is fundamentally:

```
Output = Input_matrix × Weight_matrix + Bias
```

One matrix multiplication = one layer's computation.

---

### 2.6.6 Dot Product

The dot product of two vectors gives a single number.

```
a = [1, 2, 3]
b = [4, 5, 6]

dot(a, b) = 1×4 + 2×5 + 3×6 = 4 + 10 + 18 = 32
```

**Why it matters:**
- Measures similarity between two vectors
- Used in attention mechanisms: "how similar is query to key?"
- Cosine similarity (used in embeddings) is based on dot product

---

### 2.6.7 Transpose

Flipping a matrix over its diagonal — rows become columns, columns become rows.

```
A = [[1, 2, 3],     A^T = [[1, 4],
     [4, 5, 6]]            [2, 5],
                            [3, 6]]

Shape: (2, 3)         Shape: (3, 2)
```

---

### 2.6.8 Eigenvalues and Eigenvectors

This is used in **PCA** (Principal Component Analysis) for dimensionality reduction.

**Intuition:**
- An eigenvector is a special direction such that when you multiply it by a matrix, it only scales — it doesn't rotate.
- The eigenvalue is how much it scales.

```
A × v = λ × v
         ↑     ↑
      matrix  eigenvector   λ = eigenvalue
```

**Why it matters in ML:** PCA uses eigenvectors to find the directions in your data that capture the most variance. More on this in Part 5.

---

## 2.7 Probability for ML

Probability is the language of uncertainty — and ML is all about making predictions under uncertainty.

---

### 2.7.1 What is Probability?

Probability is a number between 0 and 1 that measures how likely an event is.

```
P(event) = 0   → impossible
P(event) = 0.5 → 50/50 chance
P(event) = 1   → certain
```

---

### 2.7.2 Conditional Probability

The probability of event A **given** that event B has already happened.

```
P(A | B) = probability of A given B
```

**Example:**
- P(Spam) = 0.1 (10% of emails are spam)
- P(Contains "FREE" | Spam) = 0.8 (80% of spam emails contain "FREE")
- P(Contains "FREE" | Not Spam) = 0.05

This is the foundation of Naive Bayes classifiers.

---

### 2.7.3 Bayes' Theorem

One of the most important formulas in ML.

```
P(A | B) = P(B | A) × P(A)
           ─────────────────
                 P(B)
```

**Variables:**
- `P(A | B)` → posterior: what we want to know (probability of A, given we observed B)
- `P(B | A)` → likelihood: how likely is B if A is true
- `P(A)` → prior: our belief about A before seeing B
- `P(B)` → evidence: total probability of observing B

**Simple Example — Spam Filter:**
- You receive an email with the word "FREE"
- What is the probability it's spam?

```
P(Spam | "FREE") = P("FREE" | Spam) × P(Spam)
                   ────────────────────────────
                           P("FREE")

= 0.8 × 0.1 / 0.12 ≈ 0.67
```

Even with the word "FREE", there's a 67% chance it's spam.

---

### 2.7.4 Random Variables and Distributions

A **random variable** is a variable whose value is determined by a random event.

A **probability distribution** tells you how likely each value is.

**Discrete distribution** — finite or countable values:
- Die roll: P(1) = P(2) = ... = P(6) = 1/6

**Continuous distribution** — any value in a range:
- **Normal (Gaussian) distribution** — the famous bell curve

```
Normal Distribution:

      ┌───╮
     /     \
    /       \
───/─────────\───
  μ-3σ  μ  μ+3σ

μ = mean (center)
σ = standard deviation (spread)
```

**Why normal distribution matters in ML:**
- Model errors are often assumed to be normally distributed
- Weight initialization in neural networks often uses normal distribution
- Central Limit Theorem: averages of large samples tend to be normally distributed

---

### 2.7.5 Independent Events

Two events are independent if knowing one gives you no information about the other.

```
P(A and B) = P(A) × P(B)   (if A and B are independent)
```

**Naive Bayes assumes all features are independent** — that's why it's called "Naive."

---

## 2.8 Statistics for ML

Statistics helps you understand your data before building any model.

---

### 2.8.1 Mean, Median, Mode

Given data: `[2, 4, 4, 6, 8, 100]`

| Measure | Formula | Value | When to Use |
|---|---|---|---|
| Mean | Sum / Count | 20.67 | Average — sensitive to outliers |
| Median | Middle value | 5 | Better for skewed data |
| Mode | Most frequent | 4 | Categorical data |

**Important:** When data has outliers (like the 100 above), **median** is more representative than mean.

---

### 2.8.2 Variance and Standard Deviation

These measure how **spread out** the data is.

**Variance:**
```
σ² = (1/n) × Σ(xᵢ - μ)²

Where:
  xᵢ = each data point
  μ  = mean
  n  = number of points
```

**Standard Deviation:**
```
σ = √σ²
```

**Example:**
Data: `[2, 4, 6, 8, 10]`, Mean = 6

```
σ² = ((2-6)² + (4-6)² + (6-6)² + (8-6)² + (10-6)²) / 5
   = (16 + 4 + 0 + 4 + 16) / 5
   = 40 / 5 = 8

σ = √8 ≈ 2.83
```

**Why it matters in ML:**
- Standardization uses mean and standard deviation
- Variance tells you which features have more information
- PCA maximizes variance to find important directions

---

### 2.8.3 Covariance

Covariance measures how two variables change together.

```
Cov(X, Y) = (1/n) × Σ(xᵢ - μₓ)(yᵢ - μᵧ)
```

- **Positive covariance** → when X increases, Y tends to increase
- **Negative covariance** → when X increases, Y tends to decrease
- **Zero covariance** → X and Y are not linearly related

**Example:**
- Height and Weight → positive covariance (taller people tend to be heavier)
- Temperature and Heating bills → negative covariance

---

### 2.8.4 Correlation

Correlation is a **normalized** version of covariance — always between -1 and +1.

```
Pearson's r = Cov(X, Y) / (σₓ × σᵧ)

r = +1 → perfect positive linear relationship
r = 0  → no linear relationship
r = -1 → perfect negative linear relationship
```

**Why this matters in ML:**
- Highly correlated features carry redundant information
- If two features are 99% correlated, you only need one of them
- Correlation matrices reveal feature relationships in EDA

```
Important: Correlation ≠ Causation

Ice cream sales and drowning deaths are correlated.
(Both increase in summer.)
But ice cream does not cause drowning.
The hidden variable is "hot weather."
```

---

### 2.8.5 Sampling

Taking a subset of data from a larger population.

**Why:** It's impossible to collect data about every person, every transaction, or every event. You sample.

**Key concern:** Is your sample representative of the population?
- Biased sample → biased model → unfair predictions
- Example: Training a face recognition model only on images of one ethnicity will fail on others.

---

### 2.8.6 Hypothesis Testing (Basic Idea)

Used to determine if an observed pattern is statistically real or just random chance.

Concepts:
- **Null Hypothesis (H₀):** There is no effect / no difference
- **Alternative Hypothesis (H₁):** There is a real effect
- **p-value:** If p < 0.05, the result is "statistically significant" (the pattern is unlikely to be random)

In ML, you'll encounter this when comparing models: "Is Model A truly better than Model B, or did it just get lucky on this test set?"

---

## 2.9 Calculus for ML

Calculus is used to train neural networks. The key concept is: **how do we adjust model parameters to reduce error?** Calculus tells us the direction and size of those adjustments.

---

### 2.9.1 Functions

A function takes an input and returns an output.

```
f(x) = x²

f(3) = 9
f(-2) = 4
```

In ML: the loss function takes model parameters as input and returns an error value.

---

### 2.9.2 Derivatives

A **derivative** tells you the rate of change of a function at a specific point. Geometrically, it is the slope of the tangent line at that point.

```
f(x) = x²

f'(x) = 2x   ← derivative

At x = 3: slope = 2×3 = 6 (steeply going up)
At x = 0: slope = 0 (flat, at the minimum)
```

**Intuition:** If you are at a point on a hill:
- A positive slope → you're going uphill to the right
- A negative slope → you're going downhill to the right
- Zero slope → you're at a peak or valley

**In ML:** We use derivatives to find which direction to move the model's parameters to reduce error.

---

### 2.9.3 Partial Derivatives

When a function has multiple inputs, the partial derivative measures how the output changes when you change **one input at a time**, keeping the others fixed.

```
f(x, y) = x² + 3xy + y²

∂f/∂x = 2x + 3y   (derivative with respect to x, treating y as constant)
∂f/∂y = 3x + 2y   (derivative with respect to y, treating x as constant)
```

**In ML:** A neural network has millions of parameters. We compute the partial derivative of the loss with respect to each parameter. This tells us how much each parameter contributed to the error.

---

### 2.9.4 Gradient

The **gradient** is the collection of all partial derivatives of a function, organized as a vector.

```
∇f = [∂f/∂x₁, ∂f/∂x₂, ∂f/∂x₃, ...]
```

**Intuition:** The gradient points in the direction of steepest increase. To minimize a function (reduce error), you move in the **opposite** direction of the gradient.

---

### 2.9.5 Chain Rule

The chain rule lets you compute the derivative of a **composed function** — a function inside a function.

```
If y = f(g(x)), then:

dy/dx = (dy/dg) × (dg/dx)
      = f'(g(x)) × g'(x)
```

**Example:**
```
f(x) = (3x + 2)²

Let g(x) = 3x + 2    →    g'(x) = 3
Let f(u) = u²         →    f'(u) = 2u

dy/dx = f'(g(x)) × g'(x) = 2(3x+2) × 3 = 6(3x+2)
```

**Why chain rule matters in ML:** Backpropagation in neural networks is literally the chain rule applied repeatedly through layers. Each layer's gradient is computed by chaining together the gradients from all subsequent layers.

---

## 2.10 Optimization — The Bridge to Training

This connects all the math above to actual model training.

---

### 2.10.1 The Objective

ML training is fundamentally an **optimization problem.**

You have:
- A model with parameters (weights and biases)
- A loss function that measures how wrong the model's predictions are
- The goal: find the parameters that minimize the loss function

```
Parameters
    ↓
Model makes prediction
    ↓
Loss function measures error
    ↓
Use calculus to find direction to reduce error
    ↓
Update parameters
    ↓
Repeat until loss is small enough
```

---

### 2.10.2 Loss Function

A **loss function** (also called cost function) measures the difference between the model's prediction and the true answer.

**For regression (predicting numbers):**
```
Mean Squared Error (MSE):

L = (1/n) × Σ(yᵢ - ŷᵢ)²

Where:
  yᵢ  = true value
  ŷᵢ  = predicted value
  n   = number of samples
```

**For classification (predicting categories):**
```
Cross-Entropy Loss:

L = -(1/n) × Σ[yᵢ × log(ŷᵢ) + (1-yᵢ) × log(1-ŷᵢ)]
```

**Intuition:** When the model predicts correctly, the loss is low. When it predicts poorly, the loss is high. Training is the process of making this loss as small as possible.

---

### 2.10.3 Gradient Descent

Gradient Descent is the algorithm that adjusts model parameters to minimize the loss.

**Intuition — The Hiker Analogy:**

Imagine you are on a hilly landscape in the dark. Your goal is to reach the lowest point (minimum loss). You can't see far, but you can feel the slope under your feet. You take a step in the direction that goes downhill. Repeat until you can't go lower.

```
Landscape = Loss function
Your position = Model parameters
Downhill direction = Negative gradient
Step size = Learning rate
```

**The update rule:**

```
New Weight = Old Weight - Learning Rate × Gradient

w = w - α × ∂L/∂w

Where:
  w  = weight parameter
  α  = learning rate (step size)
  ∂L/∂w = gradient of loss with respect to w
```

**Step-by-step:**

```
Initialize weights randomly
      ↓
Forward pass: compute predictions
      ↓
Compute loss
      ↓
Backward pass: compute gradient (chain rule)
      ↓
Update weights: w = w - α × gradient
      ↓
Repeat
```

---

### 2.10.4 Learning Rate

The learning rate controls **how big each step is** during gradient descent.

```
High learning rate → big steps → may overshoot minimum → unstable
Low learning rate  → tiny steps → may take forever → too slow
Good learning rate → converges smoothly to the minimum
```

```
         Loss
          |
          |  ↘
          |    ↘
          |      ↘__________
          |              ← minimum
          └──────────────────
                    Iterations

Smooth convergence = good learning rate
```

**In practice:** Learning rate is one of the most important hyperparameters to tune. Values like 0.001, 0.01, and 0.0001 are common starting points.

---

### 2.10.5 Local vs Global Minima

A **global minimum** is the absolute lowest point of the loss function.

A **local minimum** is a point that is lower than nearby points, but not the lowest overall.

```
Loss
  |    
  |  \      /\      /
  |   \    /  \    /
  |    \  /    \__/
  |     \/      ↑        ↑
  |      ↑    local    global
  |   another  min      min
  |   local min
  └─────────────────────────
```

**In practice:** For most modern deep learning, local minima are not a major problem. The loss landscape is so high-dimensional that most local minima have similar loss values to the global minimum. **Saddle points** (flat regions) are a bigger concern.

---

## 2.11 Interview Questions — Part 2

---

**Q: What is the difference between structured and unstructured data?**

A: Structured data is organized in rows and columns (like SQL tables or CSVs) and is directly machine-readable. Unstructured data has no predefined format (like text, images, and audio) and must be converted into numbers (via embeddings or pixel arrays) before feeding into ML models.

---

**Q: What is data leakage?**

A: Data leakage occurs when information from the future or from the test set accidentally gets into the training data. For example, including a feature that is only available after the event you're trying to predict. It causes the model to appear highly accurate during training but fail completely in production.

---

**Q: Why do we split data into train, validation, and test sets?**

A: We split data because a model that is evaluated on the same data it was trained on will appear to perform perfectly — but it has simply memorized the answers. The test set acts as "unseen data" to give an honest estimate of real-world performance. The validation set is used to tune the model during development without contaminating the test set.

---

**Q: What is the difference between mean and median? When would you prefer median?**

A: Mean is the average of all values — it is sensitive to outliers. Median is the middle value — it is robust to outliers. Prefer median when your data has extreme values that would distort the average. For example, in salary data where a few executives earn 100x the typical salary, median is more representative.

---

**Q: What is a gradient?**

A: A gradient is a vector of partial derivatives of a function with respect to each of its inputs. In ML, it tells us how much the loss function changes when we change each parameter. It points in the direction of steepest increase, so we move in the opposite direction (negative gradient) to minimize the loss.

---

**Q: What is gradient descent?**

A: Gradient descent is the optimization algorithm used to train ML models. It iteratively adjusts model parameters in the direction of the negative gradient of the loss function. The learning rate controls the step size. It is the fundamental algorithm behind training neural networks.

---

**Q: What is the chain rule and why does it matter in deep learning?**

A: The chain rule is a calculus rule for differentiating composed functions: if y = f(g(x)), then dy/dx = f'(g(x)) × g'(x). In deep learning, backpropagation uses the chain rule to compute gradients through multiple layers — the gradient of the loss flows backward through each layer by multiplying the local gradients together.

---

**Q: What is the difference between variance and standard deviation?**

A: Variance is the average of the squared differences from the mean. Standard deviation is the square root of variance — it is in the same units as the original data, making it easier to interpret. Both measure spread. Standard deviation of 10 in a salary dataset means typical salaries deviate by about $10 from the mean.

---

**Q: What does Bayes' Theorem mean in simple terms?**

A: Bayes' Theorem lets us update our belief about an event based on new evidence. It says: posterior probability = likelihood × prior / evidence. In spam filtering, we start with a belief about how common spam is (prior), observe a new email's words (evidence), and update our estimate of whether it's spam (posterior).

---

**Q: Why does correlation not imply causation?**

A: Two variables can be correlated because they are both caused by a third variable (a confounding variable), not because one causes the other. Ice cream sales and drowning deaths are positively correlated — both increase in summer due to hot weather. Understanding this prevents building models that make causal claims from correlational data.

---

**Q: What is a tensor?**

A: A tensor is a generalization of scalars, vectors, and matrices to N dimensions. A scalar is a 0D tensor, a vector is 1D, a matrix is 2D, and beyond 2D are higher-order tensors. In deep learning, all data and model parameters are represented as tensors. An RGB image is a 3D tensor of shape (height, width, 3).

---

## Summary — What You Learned in Part 2

```
DATA                          MATHEMATICS
├── Structured                ├── Linear Algebra
│   ├── Tabular (rows/cols)   │   ├── Scalars, Vectors, Matrices, Tensors
│   └── SQL databases         │   ├── Matrix multiplication (core of DL)
├── Unstructured              │   └── Dot product (core of attention)
│   ├── Text → embeddings     │
│   ├── Images → pixel arrays ├── Probability
│   └── Audio → spectrograms  │   ├── Conditional probability
└── Semi-structured           │   └── Bayes Theorem (Naive Bayes)
    └── JSON, XML             │
                              ├── Statistics
DATASET CONCEPTS              │   ├── Mean/Median/Mode
├── Features (X)              │   ├── Variance/Std Dev
├── Labels (y)                │   ├── Covariance/Correlation
├── Samples (rows)            │   └── Sampling
└── Train/Val/Test split      │
                              ├── Calculus
DATA PROBLEMS                 │   ├── Derivatives (slope)
├── Missing values            │   ├── Partial derivatives
├── Duplicates                │   ├── Gradient (direction of change)
├── Outliers                  │   └── Chain rule (backpropagation)
├── Noise                     │
├── Imbalanced classes        └── Optimization
└── Data leakage                  ├── Loss function
                                  ├── Gradient descent
                                  └── Learning rate
```

These mathematics concepts are **not abstract theory** — they are used every day inside every ML model:
- Matrix multiplication happens 100s of times per second in a neural network
- Gradients are computed after every training step
- Probability is used in every classifier's output

---

**Next:** PART 3 — Artificial Intelligence Fundamentals & Machine Learning

> Say **NEXT** to continue to Part 3.
