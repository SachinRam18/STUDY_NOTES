# Machine Learning — Study Notes

---

# UNIT I — INTRODUCTION TO MACHINE LEARNING

---

## 1. Machine Learning Fundamentals

### Definition
Machine Learning (ML) is a subset of Artificial Intelligence where systems learn patterns from data and improve their performance on a task without being explicitly programmed for every rule.

### Traditional Programming vs Machine Learning

| Traditional Programming | Machine Learning |
|---|---|
| Rules + Data → Output | Data + Output → Rules (Model) |
| Rules are hand-coded by programmers | Rules are learned from data |
| Fails on unseen patterns | Generalizes to unseen data |

### Types of Machine Learning

**Supervised Learning:**
- Model learns from labeled data (input-output pairs).
- Task: Predict the output for new inputs.
- Examples: Classification, Regression.

**Unsupervised Learning:**
- Model learns from unlabeled data.
- Task: Find hidden structure or patterns.
- Examples: Clustering, Dimensionality Reduction.

**Reinforcement Learning:**
- An agent learns by interacting with an environment.
- Receives rewards for good actions, penalties for bad ones.
- Task: Learn a policy that maximizes cumulative reward.
- Examples: Game playing, Robotics.

### Basic ML Workflow

```
Data Collection → Data Preprocessing → Feature Engineering
→ Model Selection → Training → Evaluation → Deployment
```

### Train / Validation / Test Split

- **Training set:** Used to train (fit) the model.
- **Validation set:** Used to tune hyperparameters and select the best model.
- **Test set:** Used only once at the end to evaluate final performance on unseen data.

A common split: 70% train / 15% validation / 15% test.

### Key Terms

| Term | Meaning |
|---|---|
| **Feature** | An input variable (column) used for prediction |
| **Label** | The output/target variable the model predicts |
| **Parameter** | Internal model values learned from data (e.g., weights in Linear Regression) |
| **Hyperparameter** | Settings chosen before training that control learning (e.g., learning rate, number of trees) |

---

## 2. Applications of Machine Learning

| Application | Description | Example |
|---|---|---|
| **Classification** | Predict a category | Spam detection, Disease diagnosis |
| **Regression** | Predict a continuous value | House price prediction |
| **Clustering** | Group similar data points | Customer segmentation |
| **Recommendation** | Suggest relevant items | Netflix, Amazon |
| **Anomaly Detection** | Identify unusual patterns | Fraud detection |
| **NLP** | Understand/generate text | Chatbots, Translation |
| **Computer Vision** | Understand images/video | Face recognition, Object detection |

---

## 3. Linear Algebra for Machine Learning

### Core Concepts

- **Scalar:** A single number. Example: `5`
- **Vector:** An ordered list of numbers. Example: `[1, 2, 3]` (represents a point or direction in space)
- **Matrix:** A 2D array of numbers (rows × columns). Used to represent datasets and transformations.
- **Tensor:** A generalization of scalars, vectors, and matrices to N dimensions. Images are 3D tensors (height × width × channels).

### Important Operations

**Dot Product:** Multiplies two vectors element-wise and sums the result.
```
[1, 2] · [3, 4] = 1×3 + 2×4 = 11
```
Used to compute weighted sums (e.g., neuron output = weights · inputs).

**Matrix Multiplication:** Row of first matrix × column of second matrix.
- Shapes: (m × n) × (n × p) → (m × p)
- Used in neural network forward pass, transformations.

**Vector Representation:** Data points are represented as vectors. A dataset of m samples with n features is an (m × n) matrix where each row is one data sample.

---

## 4. Hypothesis Space

- **Hypothesis (h):** A single candidate function that the model uses to map inputs to outputs.
- **Hypothesis Space (H):** The set of all possible hypotheses a learning algorithm can consider.
- **Candidate Models:** Different configurations or functions within the hypothesis space.
- **Model Selection:** Choosing the best hypothesis from H based on performance on validation data.

> **Example:** In linear regression, the hypothesis space is all possible straight lines. The algorithm searches this space to find the best-fitting line.

The goal of learning is to find the hypothesis in H that best generalizes to unseen data.

---

## 5. Inductive Bias

**Inductive bias** is the set of assumptions a learning algorithm makes to generalize beyond the training data. Without bias, a model cannot make predictions on unseen data — it would just memorize the training set.

**Why it's needed:** The training data alone is never enough to uniquely determine the correct model. The algorithm needs prior assumptions to choose between equally fitting hypotheses.

**Examples:**
- **Linear Regression** assumes the relationship between inputs and output is linear.
- **Nearest Neighbor (KNN)** assumes nearby points have similar labels.
- **Decision Trees** assume simpler (shorter) trees generalize better.

Every ML model has some inductive bias — choosing a model means choosing its biases.

---

## 6. Generalization

**Generalization** is the ability of a model to perform well on new, unseen data (not just training data).

- **Training error:** Error on the training set (what the model saw during training).
- **Test error (Generalization error):** Error on new, unseen data.

### Overfitting
- Model learns the training data too well, including its noise.
- Training error is low, but test error is high.
- Model is too complex.

### Underfitting
- Model is too simple to capture the underlying pattern.
- Both training error and test error are high.

```
Underfitting         Good Fit          Overfitting
(High Bias)                          (High Variance)
    |_____           ~~~~               /\/\/\/\
```

### Ways to Improve Generalization
- Use more training data.
- Simplify the model (reduce complexity).
- Apply regularization (L1, L2).
- Use dropout (for neural networks).
- Apply cross-validation.

---

## 7. Bias-Variance Trade-off

**Bias:** Error from wrong assumptions in the model. A high-bias model underfits — it is too simple.

**Variance:** Error from sensitivity to small fluctuations in training data. A high-variance model overfits — it fits noise.

```
Total Error = Bias² + Variance + Irreducible Noise
```

| | High Bias | High Variance |
|---|---|---|
| Training Error | High | Low |
| Test Error | High | High |
| Problem | Underfitting | Overfitting |
| Fix | More complex model | Regularization, more data |

**The Trade-off:** Reducing bias tends to increase variance and vice versa. The goal is to find the sweet spot that minimizes total test error.

---

## 8. VC Dimension

**VC Dimension (Vapnik–Chervonenkis Dimension)** is a measure of the capacity (complexity) of a hypothesis space — how many points a model can correctly classify in the worst case.

- A higher VC dimension → more complex model → can fit more patterns → but also higher risk of overfitting.
- A lower VC dimension → simpler model → better generalization but may underfit.

**Key idea:** A model that can "shatter" (correctly classify in all possible ways) a set of N points has VC dimension ≥ N.

**Relation to Generalization:** A model with a large VC dimension needs more training data to generalize well. VC dimension helps theoretically bound the generalization error.

---

## 9. PAC Learning

**PAC (Probably Approximately Correct) Learning** is a theoretical framework that defines when a model can be said to have "learned" a concept from data.

**Core idea:** An algorithm PAC-learns a concept if, given enough training samples, it can output a hypothesis that is:
- **Approximately correct:** Error is at most ε (epsilon — a small allowed error).
- **Probably:** With probability at least 1 − δ (delta — allowed failure probability).

**Key takeaway:** PAC learning provides a formal bound on how many training examples are needed to guarantee learning with a certain accuracy and confidence.

**Relation to Generalization:** PAC learning theoretically justifies why a model trained on enough data should generalize to unseen data.

---
---

# UNIT II — SUPERVISED LEARNING

---

## 1. Linear Regression

### What it is
Linear Regression models the relationship between one or more input features and a continuous output by fitting a straight line (or hyperplane in multiple dimensions).

**Simple Linear Regression (one feature):**
```
ŷ = w·x + b
```
where `w` = weight (slope), `b` = bias (intercept), `ŷ` = predicted output.

**Multiple Linear Regression (multiple features):**
```
ŷ = w₁x₁ + w₂x₂ + ... + wₙxₙ + b
```

### Regression vs Classification

| Regression | Classification |
|---|---|
| Predicts a continuous value | Predicts a discrete category |
| Example: House price | Example: Spam or Not Spam |

### Cost/Loss Function
Measures how wrong the predictions are. Linear Regression uses **Mean Squared Error (MSE)**:
```
MSE = (1/n) × Σ(ŷᵢ - yᵢ)²
```
Goal: Minimize MSE by adjusting `w` and `b`.

### Least Squares
The **Ordinary Least Squares (OLS)** method finds the values of `w` and `b` that minimize the sum of squared differences between predicted and actual values. It gives the analytical (closed-form) solution for Linear Regression without needing iterative optimization.

### Gradient Descent

An iterative optimization algorithm used to minimize the cost function by updating parameters in the direction that reduces the error.

**Update Rule:**
```
w = w − α × (∂Loss/∂w)
b = b − α × (∂Loss/∂b)
```
where `α` = learning rate.

**Learning Rate:**
- Too small → slow convergence.
- Too large → may overshoot or diverge.

**Types:**

| Type | Data Used Per Update | Speed | Stability |
|---|---|---|---|
| Batch GD | Entire dataset | Slow | Stable |
| Stochastic GD (SGD) | 1 sample | Fast | Noisy |
| Mini-Batch GD | Small batch (e.g. 32) | Balanced | Balanced |

### Bayesian Linear Regression

Instead of finding a single best-fit line, Bayesian Linear Regression treats the model parameters as probability distributions.

- **Prior:** Initial belief about the parameters before seeing data.
- **Likelihood:** How well the parameters explain the observed data.
- **Posterior:** Updated belief about parameters after seeing data (Prior × Likelihood).

**Key difference:**

| Ordinary Linear Regression | Bayesian Linear Regression |
|---|---|
| Gives a single set of weights | Gives a distribution over weights |
| Point estimate | Uncertainty estimate included |
| Can overfit on small data | More robust with small data |

---

## 2. Linear Classification

### Discriminant Function
A **discriminant function** is a function of the input features that produces a score used to assign a class label. The **decision boundary** is the surface where the discriminant function output is zero — points on either side belong to different classes.

For a linear discriminant:
```
f(x) = wᵀx + b
```
- `f(x) > 0` → Class 1
- `f(x) < 0` → Class 2

### Perceptron

The Perceptron is the simplest linear classifier — a single artificial neuron.

**Structure:** Inputs → Weights → Weighted Sum → Step Function → Output (0 or 1)

**Working:**
1. Compute weighted sum: `z = wᵀx + b`
2. Apply step function: output = 1 if `z ≥ 0`, else 0.
3. If output is wrong, update weights.

**Weight Update Rule:**
```
w = w + α × (y - ŷ) × x
```
where `y` = true label, `ŷ` = predicted label, `α` = learning rate.

**Convergence:** The Perceptron converges (finds a solution) only if the data is **linearly separable**.

**Limitations:**
- Cannot solve problems that are not linearly separable (e.g., XOR problem).
- Only produces a hard 0/1 output — no probability.

---

## 3. Logistic Regression

### What it is
Despite its name, Logistic Regression is a **classification** algorithm — not regression. It predicts the probability that an input belongs to a class.

### Sigmoid Function
Converts any real number to a value between 0 and 1:
```
σ(z) = 1 / (1 + e⁻ᶻ)
```
where `z = wᵀx + b`.

- Output ≥ 0.5 → Class 1
- Output < 0.5 → Class 0

The decision boundary is where `z = 0` (i.e., `σ(z) = 0.5`).

### Loss Function
Uses **Binary Cross-Entropy Loss** (not MSE):
```
Loss = −[y·log(ŷ) + (1−y)·log(1−ŷ)]
```

### Multiclass Classification
Extended using **Softmax** (One-vs-Rest or multinomial logistic regression). Each class gets a probability, and the class with the highest probability wins.

### Logistic Regression vs Linear Regression

| | Linear Regression | Logistic Regression |
|---|---|---|
| Output | Continuous value | Probability (0 to 1) |
| Task | Regression | Classification |
| Activation | None (identity) | Sigmoid |
| Loss Function | MSE | Binary Cross-Entropy |
| Decision Boundary | N/A | Linear boundary |

---

## 4. Naive Bayes

### What it is
A probabilistic classifier based on **Bayes' Theorem**, with the "naive" assumption that all features are **conditionally independent** given the class.

### Bayes' Theorem
```
P(class | features) = P(features | class) × P(class) / P(features)
```
- **P(class | features):** Posterior — what we want to find.
- **P(features | class):** Likelihood — probability of features given the class.
- **P(class):** Prior — how common the class is in training data.
- **P(features):** Evidence — constant for comparison, can be ignored.

### Working
**Training:** Estimate P(class) and P(featureᵢ | class) from the training data.

**Prediction:** For a new input, calculate the posterior for each class and pick the class with the highest value.

```
class = argmax [ P(class) × ∏ P(featureᵢ | class) ]
```

### Common Variants

| Variant | Assumption on Features |
|---|---|
| **Gaussian NB** | Features follow a Gaussian (normal) distribution — for continuous data |
| **Multinomial NB** | Features are counts/frequencies — for text classification |
| **Bernoulli NB** | Features are binary (0 or 1) — for binary feature data |

### Applications
- Email spam filtering
- Sentiment analysis
- Document classification

### Advantages and Limitations

**Advantages:**
- Very fast to train and predict.
- Works well with small data.
- Handles high-dimensional data (text) effectively.

**Limitations:**
- The independence assumption is rarely true in practice.
- Poor probability estimates if the assumption is strongly violated.
- **Zero-probability problem:** If a feature value never appeared with a class in training, it zeroes out the whole probability. Fixed by **Laplace Smoothing** (add 1 to all counts).

---

## 5. Support Vector Machine (SVM)

### What it is
SVM is a supervised classification algorithm that finds the **optimal hyperplane** that maximally separates classes.

### Core Concepts

- **Hyperplane:** A decision boundary (line in 2D, plane in 3D, hyperplane in N-D) that separates classes.
- **Support Vectors:** The data points that are closest to the hyperplane. Only these points define the margin.
- **Margin:** The distance between the hyperplane and the nearest support vectors from each class. SVM maximizes this margin.

```
Class A  |  Class B
  o o    |    x x
  o [SV] | [SV] x
         |
     Hyperplane (max margin)
```

### Hard Margin vs Soft Margin

| Hard Margin | Soft Margin |
|---|---|
| No misclassification allowed | Allows some misclassification |
| Only works on linearly separable data | Works on non-linearly separable data |
| Sensitive to outliers | More robust |

Soft margin introduces a **penalty parameter C**:
- Large C → small margin, fewer errors (can overfit).
- Small C → large margin, more errors (may underfit).

### Kernel Trick
When data is not linearly separable, SVM maps it to a higher dimension where it becomes separable — without actually computing the transformation (using kernel functions).

| Kernel | Use Case |
|---|---|
| **Linear** | Linearly separable data |
| **Polynomial** | Non-linear data with polynomial relationships |
| **RBF (Radial Basis Function)** | General-purpose, most commonly used |

### Advantages and Limitations

**Advantages:** Works well in high dimensions, effective with small datasets, robust to outliers (soft margin).

**Limitations:** Slow on large datasets, sensitive to feature scaling, hard to interpret.

---

## 6. Decision Tree

### What it is
A tree-shaped model that makes decisions by splitting data into subsets based on feature values. Each internal node tests a feature, each branch is a decision rule, and each leaf is a class label or value.

```
        [Outlook?]
       /    |     \
   Sunny  Overcast  Rainy
    [Humidity?] YES [Wind?]
    /     \         /    \
  High   Normal  Strong  Weak
   NO     YES     NO     YES
```

### Splitting Criteria
To decide which feature to split on, we measure "impurity":

**Entropy:** Measures disorder in a node.
```
Entropy = -Σ pᵢ × log₂(pᵢ)
```
Pure node (all same class) → Entropy = 0. Mixed node → Entropy is high.

**Information Gain:** How much entropy decreases after a split.
```
IG = Entropy(parent) − weighted average Entropy(children)
```
Choose the split that maximizes Information Gain.

**Gini Impurity:** Alternative to entropy (used in CART algorithm).
```
Gini = 1 − Σ pᵢ²
```
Lower Gini → purer node. Slightly faster to compute than entropy.

### Tree Construction
1. Start with all data at root.
2. Find the best feature to split on (highest IG or lowest Gini).
3. Split data into subsets, create child nodes.
4. Repeat recursively for each child.
5. Stop when a stopping criterion is met (e.g., max depth, min samples per leaf, pure node).

### Overfitting and Pruning
Decision Trees easily overfit by growing too deep. **Pruning** removes branches that provide little predictive power:
- **Pre-pruning:** Stop growing early (max depth, min samples).
- **Post-pruning:** Grow full tree, then remove unnecessary branches.

### Advantages and Limitations
**Advantages:** Easy to interpret and visualize, no feature scaling required, handles mixed data types.

**Limitations:** Prone to overfitting, sensitive to small data changes, biased toward features with more categories.

---

## 7. Random Forest

### What it is
Random Forest is an ensemble of Decision Trees trained using **Bagging** and **random feature selection**. The final prediction is made by aggregating predictions from all trees.

### How it Works

1. **Bootstrap Sampling (Bagging):** Create multiple random subsets of the training data (with replacement). Train one Decision Tree on each subset.
2. **Random Feature Selection:** At each split, only consider a random subset of features (not all). This decorrelates the trees.
3. **Aggregation:**
   - **Classification:** Majority vote across all trees.
   - **Regression:** Average of all tree outputs.

### Random Forest vs Decision Tree

| | Decision Tree | Random Forest |
|---|---|---|
| Model | Single tree | Multiple trees |
| Overfitting | High | Low (averaging reduces variance) |
| Interpretability | High | Low (black box) |
| Performance | Moderate | Generally better |
| Speed | Fast | Slower (many trees) |

### Advantages and Limitations
**Advantages:** Reduces overfitting vs single tree, handles missing values, works well out-of-the-box, provides feature importance.

**Limitations:** Harder to interpret, slower than a single tree, needs more memory.

---
---

# UNIT III — ENSEMBLE TECHNIQUES AND UNSUPERVISED LEARNING

---

## 1. Ensemble Learning

### What it is
Ensemble learning combines predictions from multiple models to produce a better overall prediction than any single model alone.

**Weak learner:** A model that performs only slightly better than random guessing (e.g., a shallow decision tree / decision stump).

**Strong learner:** A model with high accuracy. Ensemble methods combine weak learners to build a strong learner.

---

### Voting

Used to combine predictions of multiple trained classifiers.

**Hard Voting:** Each model votes for a class, and the majority class wins.

**Soft Voting:** Each model outputs a probability for each class. Probabilities are averaged, and the class with the highest average probability wins. Soft voting generally performs better.

---

### Bagging (Bootstrap Aggregating)

- Train multiple models **in parallel**, each on a different random bootstrap sample (sampling with replacement from training data).
- Aggregate predictions (majority vote for classification, average for regression).
- Reduces **variance** (helps with overfitting).
- **Example:** Random Forest.

---

### Boosting

- Train models **sequentially**. Each new model focuses more on the examples that the previous model got wrong.
- Reduces **bias** (helps with underfitting).
- Final prediction is a weighted combination of all models.

**AdaBoost (Adaptive Boosting):**
- Assigns higher weights to misclassified examples.
- Each weak learner's weight in the final vote depends on its accuracy.

**Gradient Boosting:**
- Each new tree is trained to predict the residual errors (mistakes) of the previous model.
- Powerful and accurate.

**XGBoost:**
- An optimized, regularized implementation of Gradient Boosting.
- Very fast, handles missing values, widely used in competitions and industry.

---

### Stacking

- Train multiple **diverse base models** (e.g., Decision Tree, SVM, KNN) on the training data.
- Their predictions are used as features for a **meta-model** (e.g., Logistic Regression) that learns how to best combine them.
- More flexible than voting — the meta-model learns the optimal combination.

---

### Bagging vs Boosting vs Stacking

| | Bagging | Boosting | Stacking |
|---|---|---|---|
| **Training** | Parallel | Sequential | Two levels |
| **Focus** | Reduces variance | Reduces bias | Combines diverse models |
| **Models** | Same type | Same type | Different types |
| **Example** | Random Forest | AdaBoost, XGBoost | Blending models |
| **Risk** | May underfit | Can overfit if too many estimators | More complex to implement |

---

## 2. Unsupervised Learning

Unsupervised learning finds patterns in data without labeled outputs. The model has no "right answers" to learn from.

**Applications:**
- **Clustering:** Group similar data points (customer segmentation).
- **Dimensionality Reduction:** Reduce number of features while preserving structure (PCA).
- **Anomaly Detection:** Identify unusual points.
- **Generative Models:** Learn the data distribution (GANs, GMMs).

---

## 3. K-Means Clustering

### What it is
An unsupervised algorithm that partitions data into **K clusters**, where each data point belongs to the cluster with the nearest **centroid** (cluster center).

### Working

1. Choose K (number of clusters).
2. Randomly initialize K centroids.
3. **Assignment step:** Assign each point to the nearest centroid (using Euclidean distance).
4. **Update step:** Recompute each centroid as the mean of all points assigned to it.
5. Repeat steps 3–4 until centroids no longer change (convergence).

### Choosing K — Elbow Method
Run K-Means for different values of K. Plot the **Within-Cluster Sum of Squares (WCSS)** vs K. The "elbow" point (where WCSS stops decreasing sharply) is the optimal K.

### Advantages and Limitations
**Advantages:** Simple, fast, scales to large datasets.

**Limitations:**
- Must specify K in advance.
- Sensitive to initial centroid placement (use K-Means++ for better initialization).
- Assumes spherical (round), equally-sized clusters.
- Sensitive to outliers.
- Only assigns hard cluster membership (not probabilistic).

---

## 4. Instance-Based Learning — K-Nearest Neighbors (KNN)

### What it is
KNN is a non-parametric, lazy learning algorithm. It makes predictions based on the K most similar (nearest) training examples to the new input.

**Lazy learning:** No explicit training phase. The model just stores the training data and computes at prediction time.

### Working

1. Store all training data.
2. For a new input, compute the distance to all training points.
3. Find the K nearest neighbors.
4. **Classification:** Majority vote among K neighbors.
5. **Regression:** Average of K neighbors' values.

### Choosing K
- Small K → noisy boundary, high variance (overfitting).
- Large K → smooth boundary, high bias (underfitting).
- Use cross-validation to find optimal K.

### Distance Measures
- **Euclidean Distance:** Most common. Straight-line distance.
- **Manhattan Distance:** Sum of absolute differences.

### Feature Scaling
KNN is distance-based, so features must be scaled (standardized or normalized) to prevent features with large ranges from dominating.

### Advantages and Limitations
**Advantages:** Simple, no training time, naturally handles multi-class, adapts to new data easily.

**Limitations:** Slow prediction on large datasets (computes distance to all points), sensitive to irrelevant features and feature scale, high memory usage.

---

### KNN vs K-Means

| | KNN | K-Means |
|---|---|---|
| **Type** | Supervised (classification/regression) | Unsupervised (clustering) |
| **K means** | Number of nearest neighbors | Number of clusters |
| **Training** | No training (lazy) | Iterative training |
| **Output** | Class label or value | Cluster assignment |

---

## 5. Gaussian Mixture Model (GMM)

### What it is
GMM is a probabilistic unsupervised model that assumes data is generated from a **mixture of K Gaussian (normal) distributions**, each with its own mean and covariance.

**Soft clustering:** Unlike K-Means (hard assignment), GMM gives each point a **probability of belonging to each cluster**.

### GMM vs K-Means

| | K-Means | GMM |
|---|---|---|
| Cluster boundary | Hard (each point → one cluster) | Soft (probability per cluster) |
| Cluster shape | Spherical (equal-size) | Elliptical (flexible shape) |
| Output | Cluster label | Probability distribution |
| Underlying model | Distance-based | Probabilistic |

### Expectation-Maximization (EM) for GMM

EM is the algorithm used to fit GMM parameters (means, covariances, and mixing weights).

**E-step (Expectation):** For each data point, compute the probability that it belongs to each Gaussian component (soft assignment).

**M-step (Maximization):** Update the Gaussian parameters (mean, covariance, weight) using the soft assignments from the E-step to maximize the likelihood of the data.

**Iterative:** Repeat E and M steps until convergence (log-likelihood stops improving).

**Key relationship:** EM is to GMM what Lloyd's algorithm is to K-Means — it's the optimization procedure used to learn the model parameters. EM is more general and can be applied to other latent variable models.

### Applications
- Density estimation
- Soft clustering
- Anomaly detection
- Generative modeling

---
---

# UNIT IV — NEURAL NETWORKS

---

## 1. Neural Network Fundamentals

### What is a Neural Network?
A Neural Network is a computational model inspired by the human brain. It consists of layers of interconnected **neurons** (nodes) that learn to transform inputs into outputs.

### Neuron (Node)
Each neuron:
1. Receives inputs (features or outputs of previous layer).
2. Computes a weighted sum: `z = wᵀx + b`
3. Applies an **activation function** to introduce non-linearity.
4. Passes output to the next layer.

- **Weights (w):** Learned parameters that control the strength of connections.
- **Bias (b):** A learned offset that shifts the activation.

### Layers

```
Input Layer → Hidden Layer(s) → Output Layer
```

- **Input Layer:** Receives raw features.
- **Hidden Layer(s):** Learn intermediate representations.
- **Output Layer:** Produces the final prediction.

### Forward Propagation
Data flows from input → through hidden layers → to output. Each layer computes `activation(Wx + b)`. The final output is the prediction.

---

### Multilayer Perceptron (MLP)

An MLP is a feedforward neural network with one or more hidden layers.

**Architecture:** Input → [Hidden Layer 1] → [Hidden Layer 2] → ... → Output

**Working:** Input is passed forward through each layer. Each layer applies a linear transformation followed by an activation function.

**Perceptron vs MLP:**

| Perceptron | MLP |
|---|---|
| Single layer, single neuron | Multiple layers and neurons |
| Step function activation | Non-linear activations (ReLU, Sigmoid) |
| Only linearly separable | Can learn non-linear patterns |

---

## 2. Activation Functions

**Why needed:** Without activation functions, multiple layers would collapse into a single linear transformation. Non-linear activations allow neural networks to learn complex patterns.

| Function | Formula | Range | Use Case |
|---|---|---|---|
| **Sigmoid** | `1 / (1 + e⁻ˣ)` | (0, 1) | Binary output, logistic regression |
| **Tanh** | `(eˣ − e⁻ˣ) / (eˣ + e⁻ˣ)` | (−1, 1) | Hidden layers (zero-centered) |
| **ReLU** | `max(0, x)` | [0, ∞) | Default for hidden layers |
| **Leaky ReLU** | `max(αx, x)` where α ≈ 0.01 | (−∞, ∞) | Fix for dying ReLU |
| **Softmax** | `eˣⁱ / Σeˣʲ` | (0, 1), sums to 1 | Multi-class output layer |

**Key notes:**
- Sigmoid and Tanh cause vanishing gradient problems in deep networks.
- ReLU is fast and avoids this in most cases.
- Softmax converts a vector of scores to class probabilities.

---

## 3. Neural Network Training

### Steps:
1. **Forward Propagation:** Pass input through the network, compute prediction.
2. **Calculate Loss:** Compare prediction to true label using a loss function.
   - Regression: MSE
   - Binary classification: Binary Cross-Entropy
   - Multi-class: Categorical Cross-Entropy
3. **Backward Propagation (Backpropagation):** Calculate how much each weight contributed to the error (gradients).
4. **Weight Update (Gradient Descent):** Adjust weights in the direction that reduces loss.
5. Repeat for all batches and epochs.

---

## 4. Backpropagation

### What it is
Backpropagation is the algorithm used to calculate gradients of the loss with respect to every weight in the network, enabling gradient descent to update them.

### Process

**Forward Pass:** Input flows through layers → prediction is computed → loss is computed.

**Backward Pass:** Starting from the output, compute ∂Loss/∂w for each weight using the **chain rule** of calculus, moving backward layer by layer.

**Weight Update:**
```
w = w − α × (∂Loss/∂w)
```

**Relationship with Gradient Descent:** Backpropagation **calculates the gradients**. Gradient Descent **uses those gradients to update the weights**. They work together — neither alone is sufficient.

---

## 5. Shallow vs Deep Networks

| | Shallow Network | Deep Network |
|---|---|---|
| Hidden Layers | 0 or 1 | 2+ (often many more) |
| Complexity | Simple | High |
| Patterns Learned | Simple, low-level | Hierarchical, abstract |
| Parameters | Fewer | Many more |

**Representation Learning:** Deep networks automatically learn hierarchical representations of data:
- Early layers → edges, textures.
- Middle layers → shapes, parts.
- Final layers → objects, concepts.

**Advantage of depth:** Can approximate complex functions with fewer neurons than a wide shallow network. Better performance on complex tasks (images, speech, text).

---

## 6. Vanishing Gradient Problem

### Definition
During backpropagation in deep networks, gradients get multiplied across many layers. If the gradients are small (< 1), they shrink exponentially as they travel backward → weights in early layers barely update → network stops learning.

### Cause
Sigmoid and Tanh activations squeeze outputs into a small range (0 to 1 or −1 to 1). Gradients of these functions are always ≤ 0.25, so multiplying them across many layers makes gradients near zero.

### Effect
Early layers learn very slowly or not at all. The network effectively becomes shallow.

### Solution: ReLU

**ReLU (Rectified Linear Unit):** `f(x) = max(0, x)`

- Gradient is 1 for positive inputs → no shrinking.
- Simple and fast to compute.
- Greatly alleviates vanishing gradient in deep networks.

**Dying ReLU Problem:** If a neuron always gets negative input, it always outputs 0. Its gradient is also 0, so it never updates — the neuron "dies."

**Leaky ReLU:** Fixes dying ReLU by allowing a small negative slope:
```
f(x) = max(αx, x)   where α ≈ 0.01
```
Gradient is never exactly zero, so neurons don't die.

---

## 7. Hyperparameter Tuning

### Parameters vs Hyperparameters

| Parameters | Hyperparameters |
|---|---|
| Learned from data during training | Set manually before training |
| Weights, biases | Learning rate, batch size, epochs |

### Important Neural Network Hyperparameters

| Hyperparameter | Effect |
|---|---|
| **Learning Rate** | Controls step size in gradient descent; most critical HP |
| **Batch Size** | Samples per weight update; affects speed and stability |
| **Epochs** | Number of full passes through the training data |
| **Number of Layers** | Controls model depth and capacity |
| **Number of Neurons** | Controls model width per layer |
| **Optimizer** | Algorithm for weight update (SGD, Adam, RMSprop) |
| **Dropout Rate** | Fraction of neurons dropped; controls regularization |

### Tuning Strategies

**Grid Search:** Try every combination of a predefined set of hyperparameter values. Exhaustive but slow.

**Random Search:** Randomly sample hyperparameter combinations. Faster and often finds better results than grid search.

**Automated Tuning (AutoML/Bayesian Optimization):** Uses algorithms to intelligently explore the hyperparameter space based on past results.

---

## 8. Batch Normalization

### What it is
Batch Normalization (BatchNorm) normalizes the output of each layer during training so that it has zero mean and unit variance, then applies learnable scale and shift parameters.

### Why it's needed
As training progresses, the distribution of inputs to each layer shifts (called **Internal Covariate Shift**), making training slow and unstable.

### Working (Conceptual)
For each mini-batch:
1. Compute the mean and variance of the batch's activations.
2. Normalize the activations.
3. Apply learnable parameters γ (scale) and β (shift) to restore expressiveness.

### Benefits
- Faster and more stable training.
- Allows higher learning rates.
- Acts as a mild regularizer (reduces need for Dropout).

### Training vs Inference
- **Training:** Uses the mean and variance of the current mini-batch.
- **Inference:** Uses a running average of mean and variance computed during training (since there may be no batch).

---

## 9. Regularization

### Definition
Regularization adds a penalty to the loss function to discourage overly complex models, reducing overfitting.

### L1 Regularization (Lasso)
Adds sum of absolute values of weights to the loss:
```
Loss_new = Loss + λ × Σ|wᵢ|
```
- Drives many weights to exactly **zero** → sparse model → automatic feature selection.

### L2 Regularization (Ridge)
Adds sum of squared weights to the loss:
```
Loss_new = Loss + λ × Σwᵢ²
```
- Drives weights toward zero but rarely exactly zero → all features are kept but shrunk.
- Most commonly used.

**λ (lambda):** Regularization strength. Larger λ → stronger penalty → simpler model.

---

### Dropout

**What it is:** During each training step, randomly "drop out" (set to zero) a fraction of neurons. Each training step uses a different random subset of neurons.

**Why it reduces overfitting:**
- Neurons cannot rely on specific other neurons → learn more robust, independent features.
- Acts as training many different subnetworks simultaneously and averaging them (ensemble effect).

**Training vs Inference:**
- **Training:** Neurons are randomly dropped with probability `p` (dropout rate).
- **Inference:** All neurons are active, but their outputs are scaled by `(1 − p)` to account for the higher number of active neurons.

---
---

# UNIT V — DESIGN AND ANALYSIS OF ML EXPERIMENTS

---

## 1. ML Experiment Design

A structured ML experiment follows these steps:

1. **Define the problem:** Classification, regression, clustering? What is the target variable?
2. **Dataset preparation:** Collect, clean, and explore data. Handle missing values and outliers.
3. **Train/Validation/Test split:** Partition data before any preprocessing to avoid data leakage.
4. **Data preprocessing:** Scaling, encoding categorical variables, handling imbalance.
5. **Feature selection:** Remove irrelevant or redundant features.
6. **Model training:** Fit the model on the training set.
7. **Evaluation:** Evaluate on the validation set, tune hyperparameters.
8. **Model comparison:** Compare multiple models using consistent metrics.
9. **Final test:** Evaluate the chosen model once on the test set.

> **Data leakage warning:** Never use test data for any decision (preprocessing, feature selection, tuning) before the final evaluation.

---

## 2. Cross-Validation

### What it is
A technique to evaluate model performance more reliably by training and validating on multiple different splits of the data, rather than a single fixed split.

### K-Fold Cross-Validation
1. Split the dataset into **K equal-sized folds**.
2. For each fold: train on the other K−1 folds, validate on this fold.
3. Repeat K times. Average the K validation scores.

This ensures every sample is used for both training and validation.

**Common choice:** K = 5 or K = 10.

### Stratified K-Fold
Ensures that each fold has roughly the same proportion of class labels as the full dataset. Important for imbalanced datasets.

### Advantages and Limitations
**Advantages:** More reliable estimate of model performance, uses all data for both training and validation.

**Limitations:** Computationally expensive (trains K models), slow for large datasets or complex models.

---

### Resampling

**Resampling** means repeatedly drawing samples from a dataset to estimate statistics or validate models.

**Bootstrapping:**
- Draw N samples **with replacement** from a dataset of size N.
- Some samples appear multiple times; some not at all (~37% are left out — the "out-of-bag" samples).
- The out-of-bag samples can be used as a validation set.
- Used in Random Forest to create diverse training sets for each tree.
- Also used to estimate confidence intervals for model performance metrics.

---

## 3. Classification Performance Metrics

### Confusion Matrix

For a binary classifier:

```
                  Predicted Positive   Predicted Negative
Actual Positive        TP                   FN
Actual Negative        FP                   TN
```

- **TP (True Positive):** Correctly predicted positive.
- **TN (True Negative):** Correctly predicted negative.
- **FP (False Positive):** Predicted positive, but actually negative. (Type I Error)
- **FN (False Negative):** Predicted negative, but actually positive. (Type II Error)

### Evaluation Metrics

| Metric | Formula | Meaning |
|---|---|---|
| **Accuracy** | (TP + TN) / Total | Overall correctness |
| **Precision** | TP / (TP + FP) | Of all predicted positives, how many are correct? |
| **Recall (Sensitivity)** | TP / (TP + FN) | Of all actual positives, how many were found? |
| **F1 Score** | 2 × (Precision × Recall) / (Precision + Recall) | Harmonic mean of Precision and Recall |
| **Specificity** | TN / (TN + FP) | Of all actual negatives, how many were correctly identified? |

### ROC Curve and AUC

- **ROC Curve (Receiver Operating Characteristic):** Plots **True Positive Rate (Recall)** vs **False Positive Rate** at different classification thresholds.
- **AUC (Area Under the Curve):** A single number summarizing the ROC curve.
  - AUC = 1.0 → perfect classifier.
  - AUC = 0.5 → random classifier (no skill).

### Choosing Metrics

| Situation | Preferred Metric |
|---|---|
| Balanced dataset | Accuracy |
| Imbalanced dataset | F1 Score, AUC-ROC |
| Minimizing false positives is critical (e.g., spam) | Precision |
| Minimizing false negatives is critical (e.g., cancer detection) | Recall |

---

## 4. Model Evaluation and Comparison

### Single Model Evaluation
- Evaluate on the **test set** only after all tuning is complete.
- Report relevant metrics (accuracy, F1, AUC) based on the problem type.
- Analyze training vs validation vs test performance to diagnose overfitting/underfitting.

### Comparing Multiple Models
- Use the same train/test splits and cross-validation procedure for all models.
- Compare using the same evaluation metric.
- Do not choose the model based on test set performance alone — use validation/CV performance.

### Statistical Significance
A difference in model performance on a single test set might be due to chance. Statistical tests determine if the difference is **statistically significant** (i.e., real, not random).

### Statistical Tests for Model Comparison

**t-Test (Independent):**
- Compares means of two independent groups.
- Use when comparing models on completely different test sets.
- Assumes normal distribution.

**McNemar's Test:**
- Specifically designed for comparing two classifiers on the **same test set**.
- Uses the confusion of one model vs the other (which model got which samples wrong).
- Preferred for comparing classifiers in practice.

**K-Fold Cross-Validation Paired t-Test:**
- Run K-Fold CV for both models.
- Perform a paired t-test on the K validation scores.
- "Paired" because both models are evaluated on the same folds.
- More reliable than a single test set comparison.

| Test | When to Use |
|---|---|
| **Independent t-Test** | Two models on different datasets |
| **McNemar's Test** | Two classifiers on the same test set |
| **Paired t-Test (K-Fold)** | Two models evaluated with K-Fold CV |

---

*End of ML Study Notes*
