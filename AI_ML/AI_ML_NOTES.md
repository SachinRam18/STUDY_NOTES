# AI / ML / DL / GenAI Interview Notes

Compact, generic notes for a software engineering student. Each topic answers: what, why, how, when, and limitations.

## Learning Map

```text
Foundations -> Data -> Math -> AI -> ML -> Algorithms -> Training -> Evaluation
-> Features -> Deep Learning -> CNN/RNN -> NLP -> Embeddings -> Attention
-> Transformers -> GenAI -> LLMs -> Prompting -> Vector Search -> RAG
-> Agents -> Applications -> MLOps -> Deployment -> Evaluation -> Security
-> Responsible AI -> Vision -> RL -> Multimodal -> System Design
```

# PART 1 - FOUNDATIONS

## Programming and CS Prerequisites

### Python

Python is the main language for ML because it is readable and has strong libraries for data, modeling, deep learning, and APIs. Heavy numerical work is usually executed by optimized C/C++/CUDA code underneath Python.

Know functions, classes, modules, exceptions, file handling, iterators, list/dict/set operations, virtual environments, and JSON. In ML, `fit()` usually learns parameters and `predict()` uses them for inference.

```python
with open("config.json") as file:
    config = json.load(file)
```

Use environment variables for secrets. Never hard-code API keys.

### NumPy and Pandas

- **NumPy:** N-dimensional arrays, vectorized arithmetic, matrix operations, shapes, dtypes, broadcasting, and linear algebra.
- **Pandas:** DataFrames for loading, filtering, grouping, joining, cleaning, and summarizing tabular data.
- `X` commonly means feature matrix with shape `(samples, features)`; `y` means target.

```python
df = pd.read_csv("data.csv")
df.shape
df.isna().sum()
df.describe()
```

### Data Structures and Complexity

Arrays provide indexed access; hash maps provide average constant-time key lookup; sets store unique values; stacks are LIFO; queues are FIFO; trees represent hierarchy; graphs represent relationships; heaps support efficient min/max retrieval.

| Complexity | Meaning | Example |
|---|---|---|
| O(1) | Constant | Array index lookup |
| O(log n) | Grows slowly | Binary search |
| O(n) | One pass | Scan a dataset |
| O(n log n) | Efficient sorting | Merge sort |
| O(n^2) | Pairwise work | Naive distance comparisons |

Complexity matters because training and inference often operate on millions of rows or vectors.

### SQL, DBMS, OS, Networks, Git

- SQL: `SELECT`, `WHERE`, `JOIN`, `GROUP BY`, indexes, transactions, and aggregation are common in feature pipelines.
- DBMS: understand tables, keys, normalization, indexes, transactions, isolation, and replication at a basic level.
- OS: processes have separate memory; threads share process memory. Understand files, memory, environment variables, and graceful shutdown.
- Networks: DNS finds services; HTTPS encrypts HTTP; latency is delay; bandwidth is transfer capacity; streaming may use SSE or WebSockets.
- Git: branch, commit, merge, pull, push, tags, and history keep code, experiments, and model versions reproducible.

### REST, Authentication, Caching, Docker, Cloud

- REST exposes resources through HTTP methods such as GET, POST, PUT, and DELETE; requests are stateless and often return JSON.
- Authentication identifies the caller; authorization decides what the caller may do. Use HTTPS, short-lived credentials, least privilege, and secret managers.
- Caching stores expensive results in systems such as Redis. Cache only when freshness, privacy, and invalidation are understood.
- Docker packages code, dependencies, and runtime configuration into a portable image. A container is a running instance of an image.
- Cloud services provide compute, object storage, managed databases, GPUs, autoscaling, logging, and model platforms.

# PART 2 - DATA FUNDAMENTALS

## What Is Data?

Data is the evidence from which a model learns patterns. A model cannot fix unrepresentative, incorrect, or biased data; better algorithms do not compensate for poor data.

| Data form | Examples | Typical preparation |
|---|---|---|
| Structured | Tables, CSV, SQL | Cleaning and feature engineering |
| Semi-structured | JSON, XML, logs | Parsing and extraction |
| Unstructured | Text, image, audio, video | Numeric representation or embeddings |

## Column and Dataset Concepts

- Numerical features can be continuous or discrete.
- Categorical features are nominal (no order) or ordinal (meaningful order).
- Binary features have two values.
- Text, image, audio, and video need domain-specific representations.
- Time-series data has temporal dependence; random shuffling can leak the future.
- A **sample** is one observation; a **feature** is an input; a **label** is the expected output.
- Independent variables are inputs; the dependent variable is the target.

```text
Dataset -> samples (rows) -> features X (inputs) + label y (target)
```

### Train, Validation, and Test

- **Training set:** used to learn parameters.
- **Validation set:** used to compare models and tune hyperparameters.
- **Test set:** held out until the final evaluation.

The test set estimates performance on unseen data. Repeatedly using it for decisions makes it another validation set and causes test overfitting. Use stratified splits for classification and chronological splits for time series.

## Common Data Problems

- **Missing values:** empty or unknown fields; handle according to why they are missing.
- **Duplicates:** repeated samples can bias training and cause train/test contamination.
- **Outliers:** extreme values may be errors, valid rare cases, or the signal itself.
- **Noise:** random errors or irrelevant variation reduce learnable signal.
- **Imbalance:** one class dominates; accuracy may hide poor minority detection.
- **Leakage:** training receives future, target-derived, or test-set information.

**Interview rule:** Ask whether each feature would truly be available at prediction time.

# PART 3 - MATHEMATICS FOR ML

## Linear Algebra

- **Scalar:** one number, such as a learning rate.
- **Vector:** ordered numbers, such as one sample or an embedding.
- **Matrix:** rows and columns, such as a dataset or weight table.
- **Tensor:** a general N-dimensional array, used by neural networks.
- **Transpose:** exchanges rows and columns.
- **Inverse:** a matrix `A^-1` satisfies `AA^-1 = I` when the inverse exists; numerical libraries usually solve systems without explicitly computing it.

Matrix multiplication requires the inner dimensions to match. A neural layer is commonly:

```text
output = input * weights + bias
```

The dot product `a . b = sum(a_i*b_i)` measures weighted interaction and is central to linear models, similarity, and attention.

### Eigenvalues, Eigenvectors, and PCA

An eigenvector keeps its direction under a matrix transformation; its eigenvalue is the scale change:

```text
A v = lambda v
```

PCA uses covariance and eigenvectors to find directions containing the most variance.

## Probability

Probability represents uncertainty between 0 and 1.

```text
P(A | B) = P(A and B) / P(B)
P(A | B) = P(B | A) P(A) / P(B)       # Bayes' theorem
```

Prior is belief before evidence; likelihood is how compatible evidence is with a hypothesis; posterior is the updated belief; evidence normalizes the result. Naive Bayes assumes features are conditionally independent given the class.

Random variables map outcomes to values. Discrete distributions describe countable outcomes; continuous distributions describe ranges. A normal distribution is described by mean `mu` and standard deviation `sigma`.

## Statistics

- Mean is the arithmetic average and is sensitive to outliers.
- Median is the middle value and is more robust to skew.
- Mode is the most frequent value.
- Variance is average squared deviation from the mean.
- Standard deviation is `sqrt(variance)` and uses the original units.
- Covariance measures how two variables change together.
- Correlation normalizes linear association to `[-1, 1]`.
- Correlation does not prove causation; a confounder may affect both variables.
- Sampling must represent the population; biased samples create biased models.
- Hypothesis testing compares a null hypothesis with evidence. A p-value is not the probability that the null hypothesis is true.

## Calculus and Optimization

A derivative is the local rate of change. A partial derivative changes one variable while holding others constant. A gradient collects all partial derivatives and points toward steepest increase.

The chain rule differentiates compositions and enables backpropagation through neural-network layers.

```text
parameters -> model prediction -> loss -> gradient -> parameter update -> repeat
```

Gradient descent:

```text
w_new = w_old - learning_rate * gradient(loss, w)
```

A loss/objective function measures error. A learning rate that is too high can overshoot; one that is too low makes learning slow. A global minimum is the lowest possible loss; a local minimum is lowest only nearby. Saddle points and flat regions can also slow optimization.

# PART 4 - ARTIFICIAL INTELLIGENCE

## AI, ML, DL, GenAI

| Term | Meaning | Typical capability |
|---|---|---|
| AI | Broad field of intelligent systems | Reasoning, search, perception |
| ML | Learns patterns from data | Prediction and decisions |
| DL | ML with layered neural networks | Complex raw-data patterns |
| GenAI | Generates new content | Text, image, audio, code |
| LLM | Large language model | Text understanding and generation |

The categories overlap: deep learning is ML, and ML is AI, but rule-based AI is not necessarily ML.

## Types and Approaches

- **Narrow AI:** specialized for a task; current deployed AI is narrow.
- **AGI:** hypothetical general human-level capability; it does not currently exist.
- **ASI:** theoretical intelligence beyond humans in all domains.
- **Rule-based systems:** explicit IF/THEN logic; transparent but brittle.
- **Search:** explores possible states, such as BFS, DFS, A*, or game-tree search.
- **Planning:** chooses actions to reach a goal under constraints.
- **Knowledge representation:** stores facts and relationships using ontologies or knowledge graphs.
- **ML/DL:** learns patterns when rules are too complex to write.
- **GenAI:** learns a data distribution and samples new content from it.

## Intelligent Agents

An agent perceives an environment and acts toward a goal.

```text
Environment -> sensors/perception -> state -> decision/policy -> action -> Environment
```

State is the current situation; action changes the environment; utility measures goal achievement. A rational agent chooses the action with the highest expected utility given its information, not an impossible perfect action.

# PART 5 - MACHINE LEARNING FUNDAMENTALS

## Traditional Programming vs ML

```text
Traditional: input + human-written rules -> output
ML:         input + example outputs -> learned model -> output for new input
```

ML is useful when rules are complex, changing, or difficult to encode. It still needs problem definition, data quality, evaluation, and software engineering.

## Learning Paradigms

| Paradigm | Feedback | Examples |
|---|---|---|
| Supervised | Correct labels | Spam, price prediction |
| Unsupervised | No labels | Segmentation, compression |
| Semi-supervised | Few labels + many unlabeled samples | Image classification |
| Reinforcement | Rewards after actions | Games, robotics |

Supervised tasks include **classification** (category) and **regression** (number). Unsupervised tasks include clustering, dimensionality reduction, and association rules.

## Reinforcement Learning Overview

```text
agent -> action -> environment -> reward + next state -> policy update -> repeat
```

State describes the situation; action is a choice; reward is feedback; policy maps states to actions; value estimates future reward; Q-value estimates the value of an action in a state. Exploration tries unknown actions; exploitation uses the best-known action.

# PART 6 - ML WORKFLOW AND PREPROCESSING

## End-to-End Workflow

```text
Problem -> data -> clean -> EDA -> features -> split -> baseline
-> train -> validate -> tune -> final test -> deploy -> monitor -> retrain
```

1. Define target, prediction time, business cost, constraints, and metric.
2. Collect representative data and labels.
3. Split before learning dataset statistics.
4. Explore distributions, missingness, relationships, and target balance.
5. Clean and transform with train-fitted operations.
6. Build a baseline and choose models appropriate to data and constraints.
7. Tune on training/validation data.
8. Evaluate once on the untouched test set.
9. Deploy the same preprocessing and model artifact.
10. Monitor performance, drift, latency, cost, errors, and fairness.

## Missing Data

- **MCAR:** missingness unrelated to observed or missing values.
- **MAR:** missingness depends on observed variables.
- **MNAR:** missingness depends on the missing value itself.

Options: drop rows when loss is small and missingness is acceptable; drop mostly empty columns; use mean/median/mode imputation; forward/backward fill for suitable time series; use model-based imputation for complex relationships; add a missingness indicator when absence contains signal.

Fit the imputer on training data only.

## Encoding

- **One-hot:** one binary column per nominal category; good for low cardinality.
- **Ordinal:** maps known order, such as small < medium < large.
- **Label encoding:** integer IDs; inappropriate for nominal categories unless the model/encoding treatment is designed for it.
- **Target encoding:** replaces a category with a target statistic; useful for high cardinality but highly leakage-prone.

Unknown categories at inference must have a defined behavior, such as `handle_unknown="ignore"`.

## Scaling

```text
MinMax:          x' = (x - min) / (max - min)
Standardization: x' = (x - mean) / standard_deviation
Robust:          x' = (x - median) / IQR
```

Scaling matters for KNN, SVM, K-Means, PCA, and many neural networks because magnitude affects distances or optimization. Tree splits are generally scale-invariant. Fit on train, transform all other sets.

## Outliers and Imbalance

IQR rule: `IQR = Q3 - Q1`; flag below `Q1 - 1.5*IQR` or above `Q3 + 1.5*IQR`. Z-score `|z| > 3` is a rough rule for approximately normal data.

Before removing an outlier, determine whether it is an error, valid extreme, or useful signal. Alternatives are clipping, log transformation, robust scaling, or robust models.

For class imbalance use class weights, oversampling, SMOTE, undersampling, or threshold adjustment. Apply resampling only to training data. Never create synthetic validation/test data.

## EDA

```python
df.shape; df.info(); df.describe()
df.isna().sum(); df.duplicated().sum(); df.nunique()
```

Use histograms for distributions, box plots for outliers, bar/count plots for categories and class balance, scatter plots for relationships, and correlation heatmaps for linear association. EDA suggests hypotheses; it does not prove causation.

## Leakage-Safe Pipeline

```python
numeric = Pipeline([("impute", SimpleImputer(strategy="median")),
                    ("scale", StandardScaler())])
categorical = Pipeline([("impute", SimpleImputer(strategy="most_frequent")),
                        ("encode", OneHotEncoder(handle_unknown="ignore"))])
preprocess = ColumnTransformer([("num", numeric, numeric_cols),
                                ("cat", categorical, categorical_cols)])
full_model = Pipeline([("preprocess", preprocess), ("model", estimator)])
full_model.fit(X_train, y_train)
```

A pipeline prevents train/inference mismatch and makes leakage less likely.

# PART 7 - REGRESSION ALGORITHMS

## Linear and Polynomial Regression

Regression predicts a continuous value.

```text
y_hat = w1*x1 + w2*x2 + ... + wn*xn + b
```

Linear regression chooses weights that minimize squared error. Coefficients indicate direction and effect conditional on other features, but interpretation requires care with scale, correlation, and confounding.

Assumptions commonly discussed: linear relationship, independent observations, constant residual variance, approximately normal residuals for statistical inference, and limited multicollinearity.

Polynomial regression adds terms such as `x^2`, `x^3`, and interactions, then fits a linear model in the expanded feature space. It models curves but high degrees can overfit.

## Ridge, Lasso, Elastic Net

- **Ridge:** `MSE + lambda * sum(w^2)`. Shrinks coefficients and stabilizes correlated features; generally keeps all features.
- **Lasso:** `MSE + lambda * sum(abs(w))`. Can make coefficients exactly zero and perform feature selection; selection can be unstable for correlated features.
- **Elastic Net:** combines L1 and L2; useful with many features, irrelevant variables, and correlated groups.

Regularization trades training fit for simpler models. Tune its strength with cross-validation.

## Regression Metrics

```text
MAE  = mean(abs(y - y_hat))
MSE  = mean((y - y_hat)^2)
RMSE = sqrt(MSE)
R2   = 1 - SS_res / SS_tot
```

MAE is interpretable and less affected by large errors. RMSE is in target units and emphasizes large errors. R2 compares with the mean baseline; `1` is perfect, `0` is baseline, and it can be negative. Adjusted R2 penalizes unnecessary predictors.

# PART 8 - CLASSIFICATION ALGORITHMS AND METRICS

## Confusion Matrix

| | Predicted positive | Predicted negative |
|---|---:|---:|
| Actual positive | TP | FN |
| Actual negative | FP | TN |

- Accuracy: `(TP + TN) / total`; misleading under imbalance.
- Precision: `TP / (TP + FP)`; of predicted positives, how many were correct.
- Recall/sensitivity: `TP / (TP + FN)`; of actual positives, how many were found.
- Specificity: `TN / (TN + FP)`; true-negative rate.
- F1: harmonic mean of precision and recall.
- FPR: `FP / (FP + TN)`.
- ROC-AUC: ranking quality over thresholds.
- PR-AUC: often more informative for rare positives.
- Log loss: evaluates predicted probabilities and punishes confident mistakes.

Lowering a positive threshold usually increases recall and decreases precision. Select the threshold using business costs.

## Logistic Regression

Despite its name, logistic regression is classification. It maps a linear score to a probability:

```text
z = w*x + b
sigmoid(z) = 1 / (1 + exp(-z))
```

It is fast, interpretable, probabilistic, and a strong baseline. Its decision boundary is linear unless features are transformed. Regularization strength in common libraries may be represented by `C`, the inverse of penalty strength.

## K-Nearest Neighbors

KNN stores training examples. At prediction time it computes distances, selects the K nearest points, and votes or averages their targets.

Small K is noisy and can overfit; large K is smooth and can underfit. KNN needs scaling, is expensive at prediction time, uses memory, and suffers from the curse of dimensionality.

## Naive Bayes

Naive Bayes uses Bayes' theorem and assumes conditional independence of features given the class. It is extremely fast and effective for many text problems.

- Gaussian NB: continuous features.
- Multinomial NB: counts such as word frequencies.
- Bernoulli NB: binary presence/absence features.

The independence assumption is often false, so probabilities may be poorly calibrated even when classification works well.

## Decision Tree

A tree recursively asks feature-threshold questions until a leaf predicts a class or value. Splits can use Gini impurity, entropy/information gain, or regression error.

Important controls: `max_depth`, `min_samples_split`, `min_samples_leaf`, and `max_features`. Trees handle nonlinear interactions and do not need scaling, but unpruned trees overfit and are unstable.

## Random Forest

Random forest trains many diverse trees on bootstrap samples and random feature subsets, then votes or averages.

```text
bootstrap samples + random feature subsets -> many trees -> aggregate prediction
```

It reduces variance, handles nonlinear tabular data, is parallelizable, and offers feature importance. It uses more memory and is less interpretable than one tree. Out-of-bag samples can provide an internal estimate.

## Boosting, AdaBoost, XGBoost

Boosting trains weak learners sequentially. Each new learner focuses on current errors.

- AdaBoost increases weights of misclassified samples.
- Gradient boosting fits residuals/negative gradients.
- XGBoost adds efficient tree construction, regularization, subsampling, missing-value handling, and early stopping.
- LightGBM is optimized for large tabular data.
- CatBoost handles categorical features effectively.

Key parameters: number of estimators, learning rate, tree depth, row/feature subsampling, and regularization. Lower learning rates often need more trees. Boosting can overfit without validation and stopping controls.

## Support Vector Machine

SVM finds a maximum-margin separating hyperplane. Points closest to the boundary are support vectors. Soft-margin parameter `C` trades margin width against training errors.

The kernel trick computes similarity in an implicit higher-dimensional space. Common kernels are linear, polynomial, and RBF. SVM usually needs scaling and can be slow for large datasets; it is often strong for smaller, high-dimensional data.

## Algorithm Selection

| Situation | Good starting choices |
|---|---|
| Need interpretable baseline | Linear/logistic regression, shallow tree |
| Small tabular data | Regularized linear model, tree, SVM |
| General tabular baseline | Random forest or gradient boosting |
| High-quality tabular prediction | XGBoost/LightGBM/CatBoost |
| Text counts | Naive Bayes or linear model |
| Distance-based patterns | KNN, after scaling |
| High-dimensional small data | Linear SVM or logistic regression |

# PART 9 - CLUSTERING AND DIMENSIONALITY REDUCTION

## K-Means

K-Means alternates between assigning points to the nearest centroid and recomputing centroid means.

```text
choose K -> initialize centroids -> assign -> recompute -> repeat until stable
```

It minimizes within-cluster sum of squares (inertia). K-Means++ improves initialization. It is fast but requires K, assumes roughly spherical similarly sized clusters, and is sensitive to scale, initialization, and outliers.

## Hierarchical Clustering

Agglomerative clustering starts with one cluster per point and repeatedly merges the closest clusters. A dendrogram shows merge distances; cutting it at a height gives clusters.

Single, complete, average, and Ward linkage define cluster distance differently. The method provides hierarchy but is expensive and early merges cannot be undone.

## DBSCAN

DBSCAN groups dense regions and marks sparse points as noise. `eps` is neighborhood radius; `min_samples` defines a dense core. It finds arbitrary shapes and outliers without specifying K, but struggles with varying density and high-dimensional distances.

## Gaussian Mixture Model

GMM models data as a mixture of Gaussian components. Unlike K-Means hard assignments, it gives membership probabilities. It suits overlapping or elliptical clusters but requires choosing the number of components and distribution assumptions.

## Choosing Clusters

- **Elbow:** plot inertia against K and look for diminishing returns; sometimes ambiguous.
- **Silhouette:** compares within-cluster cohesion to nearest-cluster separation; near `+1` is good, `0` is boundary, negative is poor.
- Also use domain meaning and stability; a numerical score alone does not prove useful segments.

## Dimensionality Reduction

High dimensions increase computation, sparsity, visualization difficulty, and overfitting risk.

- **PCA:** linear, unsupervised, variance-preserving projection. Standardize first when feature scales differ. Components are combinations of original features and may be hard to interpret.
- **LDA:** supervised projection that seeks class separation; useful for classification.
- **t-SNE:** nonlinear visualization preserving local neighborhoods; expensive, sensitive to settings, and not generally a production feature transform.
- **UMAP:** nonlinear visualization that is often faster and preserves more global structure; can support transforming new points depending on implementation.

# PART 10 - GENERALIZATION, FEATURES, AND TUNING

## Underfitting and Overfitting

- **Underfitting:** model is too simple; training and validation performance are both poor. Add useful features, increase capacity, train longer, or reduce regularization.
- **Overfitting:** model memorizes noise; training performance is strong but validation performance is poor. Simplify, add data, regularize, augment, select features, or stop earlier.

```text
total expected error = bias^2 + variance + irreducible noise
simple model -> high bias, low variance
complex model -> low bias, high variance
```

## Regularization

- L1 and L2 penalize parameter size.
- Dropout randomly disables neurons during training; it is disabled at inference.
- Early stopping keeps the best validation checkpoint.
- Data augmentation creates varied examples.
- Batch normalization can stabilize optimization but is not a replacement for validation.

## Feature Engineering

- **Creation:** ratios, counts, date parts, interaction terms, domain signals.
- **Transformation:** log skewed values, bin continuous values, encode categories, scale values.
- **Selection:** remove leakage, constants, duplicates, noisy or redundant features.
- **Extraction:** PCA, embeddings, or learned representations.
- **Domain features:** use knowledge of how the system generates data.

Feature engineering must respect prediction-time availability and be fitted inside a leakage-safe pipeline.

## Cross-Validation

K-fold CV trains K times, each time validating on a different fold, and reports mean and variation. Stratified K-fold preserves class ratios. Leave-one-out uses one validation sample per round and is expensive. Time-series CV must preserve order.

Use CV on training data to estimate generalization and tune models. Keep the final test set untouched.

## Hyperparameter Search

- **Grid search:** all combinations; simple but expensive.
- **Random search:** explores broad ranges efficiently.
- **Bayesian optimization:** uses earlier results to select promising trials.

Parameters are learned by training; hyperparameters are selected by the engineer. Examples include tree depth, number of trees, learning rate, K, SVM `C`, and regularization strength.

# PART 11 - DEEP LEARNING FUNDAMENTALS

## Neural Networks and Neurons

A neural network is a composition of parameterized layers.

```text
input features -> hidden layers -> output layer -> prediction
```

A neuron computes a weighted sum, adds bias, and applies an activation:

```text
z = w1*x1 + ... + wn*xn + b
a = activation(z)
```

Weights control learned influence; bias shifts the activation; activations add nonlinearity. Without nonlinear activations, stacked linear layers collapse into one linear transformation.

## Forward and Backward Propagation

```text
input -> forward pass -> prediction -> loss
loss -> backpropagation -> gradients -> optimizer update -> repeat
```

Forward propagation computes predictions. The loss compares predictions with labels. Backpropagation applies the chain rule to compute gradients. The optimizer updates weights. Training happens over epochs, batches, and iterations.

- **Epoch:** one pass over training data.
- **Batch:** samples processed together.
- **Mini-batch:** common compromise between full batch and one sample.
- **Iteration:** one parameter update, usually one batch.

## Activation Functions

| Function | Use | Limitation |
|---|---|---|
| Sigmoid | Binary probability output | Saturation and vanishing gradients |
| Tanh | Centered bounded activation | Saturation |
| ReLU `max(0,x)` | Common hidden layers | Dead neurons for negative inputs |
| Leaky ReLU | ReLU with negative slope | Extra choice of slope |
| Softmax | Multi-class probability distribution | Can be overconfident; used at output |

ReLU is often preferred in hidden layers because it is simple and has a non-saturating positive region, making optimization easier than sigmoid/tanh in many networks.

## Optimizers

- **SGD:** basic noisy gradient updates; simple and often effective.
- **Momentum:** accumulates direction to reduce oscillation.
- **RMSProp:** adapts step sizes using recent squared gradients.
- **Adam:** combines momentum-like first moments with adaptive second moments; strong default.
- **AdamW:** decouples weight decay from Adam's gradient update and is common for modern neural networks.

Optimizer choice does not remove the need for good data, learning-rate tuning, validation, and checkpointing.

# PART 12 - CNN, RNN, LSTM, AND GRU

## Convolutional Neural Networks

CNNs exploit local spatial structure and shared filters, making them efficient for images. A filter slides over an image to create a feature map.

```text
image -> convolution/filter -> activation -> pooling
      -> repeated feature extraction -> flatten/dense -> output
```

- **Kernel/filter:** learned local pattern detector.
- **Feature map:** filter responses.
- **Padding:** controls border treatment and output size.
- **Stride:** filter step size.
- **Pooling:** downsamples; max pooling keeps strongest response, average pooling averages.
- **Flatten:** converts feature maps to a vector for dense layers.

CNNs learn edges, textures, parts, and higher-level patterns. They reduce parameters through local connectivity and weight sharing. They can be sensitive to shifts, data quality, and distribution changes; modern alternatives also include vision transformers.

## RNNs

RNNs process sequences one step at a time and carry a hidden state:

```text
(input_t, hidden_{t-1}) -> RNN cell -> hidden_t -> output_t
```

They suit sequence-dependent data but sequential computation is slow and long-range gradients can vanish or explode.

## LSTM and GRU

LSTM adds a cell state and gates: forget, input, and output. Gates control what to discard, store, and expose, helping preserve long-term information.

GRU uses update and reset gates with a simpler structure and often fewer parameters. LSTM can be more expressive; GRU can be faster. Transformers are now preferred for many long-context tasks, but recurrent models remain useful for smaller or streaming systems.

# PART 13 - NLP AND REPRESENTATIONS

## NLP Pipeline

NLP converts human language into representations a model can process.

```text
raw text -> normalize/tokenize -> numeric representation -> model -> task output
```

Tokenization splits text into words, subwords, or characters. A vocabulary maps tokens to IDs. Stop-word removal, stemming, and lemmatization can help traditional pipelines but are task- and language-dependent; do not apply them blindly to modern language models.

## Bag of Words and TF-IDF

- **One-hot:** sparse vector with one active vocabulary position; no similarity or order.
- **Bag of Words:** counts vocabulary terms; ignores word order.
- **TF-IDF:** weights a term by its frequency in a document and rarity across documents.

TF-IDF is useful for small, transparent text classification and search baselines. It cannot represent deep meaning or context well.

## Embeddings

An embedding is a dense vector designed to place semantically related items near each other.

```text
text/image -> embedding model -> vector -> similarity/search/model input
```

Cosine similarity:

```text
cos(a,b) = (a dot b) / (||a|| ||b||)
```

Embeddings can represent text, images, audio, or multimodal content. Contextual embeddings represent a token differently depending on surrounding text. Embedding quality depends on model, domain, language, chunking, and evaluation.

# PART 14 - ATTENTION AND TRANSFORMERS

## Attention

Attention lets a model focus on relevant parts of an input instead of treating every position equally.

Each position produces a **query**, **key**, and **value**. Queries are matched against keys; scores are normalized; values are combined.

```text
Q, K, V -> similarity scores -> scale/mask -> softmax weights -> weighted V
```

Scaled dot-product attention:

```text
Attention(Q,K,V) = softmax(QK^T / sqrt(d_k)) V
```

The scale reduces extreme scores. A causal mask prevents a token from seeing future tokens during generation.

- **Self-attention:** Q, K, and V come from the same sequence.
- **Cross-attention:** one sequence queries another, common in encoder-decoder models.
- **Multi-head attention:** several attention projections learn different relationships in parallel.

Attention captures long-range relationships but has high memory/compute cost as sequence length grows.

## Transformer Architecture

```text
tokens -> token embeddings + positions -> attention -> feed-forward network
       -> residual connections + layer normalization -> repeated blocks -> output
```

Transformers use self-attention, multi-head attention, position information, feed-forward layers, residual connections, and normalization.

- **Encoder:** builds representations from input; common in understanding tasks.
- **Decoder:** generates autoregressively; common in language generation.
- **Encoder-decoder:** maps one sequence to another, such as translation.

Transformers parallelize training better than RNNs and model long-range interactions, but require substantial data and compute. Position information is required because attention alone does not inherently encode order.

# PART 15 - GENERATIVE AI AND LLMS

## Generative vs Discriminative AI

- **Discriminative models:** learn a boundary or conditional prediction, such as `P(label | input)`.
- **Generative models:** learn how data is distributed and can create new samples.

Generative systems produce text, images, audio, video, or code. Outputs are not guaranteed to be true merely because they are fluent.

## Large Language Models

An LLM is a large neural language model trained to model token sequences. It predicts a distribution for the next token or performs a related sequence objective.

```text
text -> tokenizer -> token IDs -> embeddings -> transformer -> logits -> probabilities -> token
```

- **Token:** text unit, often a subword rather than a word.
- **Parameter:** learned numerical value; parameter count is not the same as knowledge quality.
- **Context window:** maximum input/output token context supported by a model.
- **Inference:** using the trained model to generate or score output.
- **Pretraining:** broad learning from large corpora.
- **Fine-tuning:** adapting model weights to a task or domain.

Tokenization affects cost, context limits, and multilingual behavior. Context window is not permanent memory; information outside it is unavailable unless stored and retrieved.

## LLM Training and Alignment

```text
collect/clean data -> tokenize -> pretrain -> supervised fine-tune
-> preference alignment -> evaluate -> deploy -> monitor
```

- **Pretraining:** learns general language patterns, often with next-token prediction.
- **Instruction tuning/SFT:** trains on prompt-response examples.
- **RLHF:** uses human preferences to train a reward signal and optimize behavior.
- **DPO:** directly optimizes preferred over rejected responses without a separate online RL loop in the usual formulation.
- **Alignment:** improves helpfulness, behavior, and safety; it does not guarantee truth.

Fine-tuning changes model behavior/knowledge in weights, but it does not reliably provide live facts. It can also cause forgetting, leakage, or overfitting.

## Sampling Controls

- **Temperature:** changes probability sharpness; higher generally increases variety and risk.
- **Top-k:** samples from the k most likely tokens.
- **Top-p:** samples from the smallest cumulative-probability set.
- **Max tokens:** limits generated length.

Deterministic settings are useful for structured tasks, but low temperature does not guarantee factuality.

# PART 16 - PROMPT ENGINEERING

## Prompt Structure

A robust prompt specifies role or context, task, input, constraints, output format, and failure behavior.

- **Zero-shot:** no examples.
- **One-shot/few-shot:** one or several examples.
- **System message:** high-level behavior/instructions.
- **User message:** task or input.
- **Structured output:** JSON/schema-constrained response where supported.
- **Prompt template:** reusable parameterized prompt.

Prompt engineering improves task specification; it cannot replace missing data, reliable tools, or evaluation.

```text
context + task + constraints + examples + output schema -> model response
```

Chain-of-thought should not be treated as a requirement to expose private reasoning. Prefer concise answers, intermediate structured fields, or tool traces when an application needs auditability.

## Prompt Risks

- **Prompt injection:** untrusted content attempts to override instructions.
- **Jailbreaking:** attempts to bypass safety controls.
- **Context overload:** too much irrelevant text reduces useful attention.
- **Instruction conflict:** different sources give contradictory directions.

Treat retrieved documents and tool outputs as untrusted data, delimit them, validate outputs, and enforce authorization outside the model.

# PART 17 - VECTOR DATABASES AND RAG

## Vector Search

Embeddings turn content and queries into vectors. Similarity search retrieves vectors nearest to a query using cosine, dot product, or distance metrics.

```text
documents -> chunks -> embeddings -> vector index
query -> embedding -> nearest-neighbor search -> relevant chunks
```

Approximate nearest neighbor (ANN) indexes trade a small amount of exactness for much faster search. Metadata filters restrict results by tenant, date, access level, or document type.

## Vector Databases

A vector database stores vectors, metadata, and indexes and supports insertion, filtering, and similarity search. FAISS is a vector-search library; Chroma, Pinecone, Weaviate, and Milvus are common vector-storage/search choices with different hosting and operational models.

Important design choices: embedding model, dimension, distance metric, index type, chunk size/overlap, metadata, update strategy, permissions, and evaluation.

## Retrieval-Augmented Generation

RAG retrieves external information at query time and places it in the LLM context.

```text
documents -> clean -> chunk -> embed -> index
user question -> embed -> retrieve/filter -> rerank -> construct context
-> LLM -> cited/grounded answer
```

Components:

- **Chunking:** divides documents into retrievable units; too small loses context, too large adds noise.
- **Retrieval:** fetches candidate chunks using vector, keyword, or hybrid search.
- **Reranking:** uses a stronger relevance model to reorder candidates.
- **Context construction:** includes relevant text, metadata, instructions, and source identifiers.
- **Generation:** LLM answers using context and should abstain when evidence is insufficient.

RAG is useful for private, changing, or source-citable knowledge. It can reduce unsupported answers but does not guarantee correctness. Bad chunking, poor embeddings, retrieval misses, stale indexes, context limits, and prompt injection remain risks.

## RAG vs Fine-Tuning vs Long Context

| Approach | Changes | Best for | Main limitation |
|---|---|---|---|
| RAG | Retrieved context at runtime | Fresh/private/citable facts | Retrieval quality and latency |
| Fine-tuning | Model weights | Style, format, task behavior | Cost, stale facts, training risk |
| Long context | More input in one request | Known large context | Cost, attention limits, irrelevant text |

Often these approaches are combined. Do not fine-tune merely to make a model remember frequently changing documents.

# PART 18 - AI AGENTS AND TOOL CALLING

## Agent Concept

An agent is an LLM-based or rule-based system that observes a task state, selects actions/tools, observes results, and continues until a stopping condition.

```text
user goal -> agent/LLM -> tool selection -> authorized tool call
          -> observation -> state update -> next step or final answer
```

- **Tool:** controlled function/API such as search, database query, or payment.
- **Function calling:** model emits structured arguments for an application to execute.
- **State:** task progress, messages, tool results, and permissions.
- **Planning:** decomposes a goal into steps.
- **Memory:** short-term context or persistent user/application data; must have retention and privacy rules.
- **Human-in-the-loop:** approval for risky or irreversible actions.

The application, not the LLM, must enforce authentication, authorization, validation, rate limits, idempotency, timeouts, and side-effect controls.

## Agent Tradeoffs

Agents help with multi-step, tool-rich tasks but add latency, cost, nondeterminism, and security risk. Prefer a deterministic workflow when steps are known. Bound iterations, tool permissions, budget, and context. Log decisions and tool calls without storing unnecessary sensitive data.

# PART 19 - LLM APPLICATION DEVELOPMENT

## Typical Stack

```text
frontend -> FastAPI/backend -> auth/rate limit -> orchestrator
         -> LLM / retriever / tools -> database/cache -> response/stream
```

Use provider APIs through a backend so keys are not exposed to browsers. Validate request schemas, handle retries and timeouts, stream only when useful, and log request IDs rather than secrets.

Common pieces: LLM API, FastAPI/Flask/Django, PostgreSQL for relational data, Redis for caching/state, vector DB for retrieval, object storage for documents, and orchestration libraries such as LangChain, LangGraph, or LlamaIndex when they simplify a real need.

## Common Architectures

### Chatbot

```text
UI -> backend -> conversation state -> LLM -> filtered response -> UI
```

### RAG Chatbot

```text
UI -> API -> query embedding -> vector DB -> reranker -> LLM -> answer + sources
```

### Agentic System

```text
user -> API -> agent state
                  |-> LLM
                  |-> RAG
                  |-> database
                  |-> external APIs/tools
             -> validated final response
```

Design tenant isolation, authorization-aware retrieval, error handling, observability, and human approval before adding autonomy.

# PART 20 - MLOPS, LLMOPS, AND DEPLOYMENT

## MLOps Lifecycle

```text
data versioning -> training -> experiment tracking -> model registry
-> CI/CD -> serving -> monitoring -> retraining
```

- Version code, data snapshots, features, prompts, dependencies, and models.
- Track parameters, metrics, datasets, artifacts, and environment.
- A model registry manages staged versions and approvals.
- CI tests code, data contracts, preprocessing, and model behavior.
- CD deploys reproducible artifacts with rollback.
- LLMOps additionally tracks prompts, provider/model versions, token cost, traces, retrieval quality, and safety evaluations.

MLflow is commonly used for experiments/registry; Docker packages services; Kubernetes orchestrates containers; CI systems automate checks; cloud platforms provide managed compute and storage.

## Serving Modes and Infrastructure

- **Batch inference:** process many records periodically; efficient, not immediate.
- **Real-time inference:** respond per request; latency and availability matter.
- **CPU:** cheaper and suitable for many classical/small models.
- **GPU:** high parallel throughput for deep learning but limited by VRAM and cost.
- **Latency:** time per request; **throughput:** requests per time.
- **Scaling:** add replicas, batch requests, cache, queue work, or use autoscaling.
- **Load balancing:** distributes traffic across healthy instances.

Measure p50/p95/p99 latency, throughput, error rates, utilization, cost, and quality. Use timeouts, retries with backoff, circuit breakers, and graceful degradation.

## Model Optimization

- **Quantization:** lower numerical precision to reduce memory and speed inference; may reduce quality.
- **Pruning:** remove less important weights/structures.
- **Distillation:** train a smaller student from a larger teacher.
- **LoRA:** learns low-rank adapter matrices while freezing most base weights.
- **QLoRA:** combines quantized base weights with LoRA adapters.
- **PEFT:** parameter-efficient fine-tuning family.
- **KV cache:** reuses attention keys/values during autoregressive generation; saves computation but consumes memory.
- **Flash Attention:** memory-efficient attention implementation.
- **Batching:** improves throughput but may increase latency.
- **Speculative decoding:** a smaller model proposes tokens and a larger model verifies them.

Choose optimizations using measured quality, latency, memory, and cost rather than names alone.

# PART 21 - AI EVALUATION

## Traditional ML Evaluation

Use a held-out test set and metrics aligned with cost. Classification uses confusion-matrix metrics, ROC-AUC, PR-AUC, calibration, and threshold analysis. Regression uses MAE, RMSE, R2, and residual analysis. Also check subgroup performance, robustness, and data coverage.

## LLM Evaluation

- **Correctness:** matches a trusted answer or expected behavior.
- **Relevance:** addresses the request.
- **Groundedness/faithfulness:** supported by supplied sources.
- **Hallucination:** unsupported or fabricated claim.
- **Safety/toxicity:** avoids harmful or disallowed behavior.
- **Latency and cost:** meets product constraints.

Combine deterministic tests, curated datasets, human review, model-based graders with calibration, and production feedback. A fluent answer is not automatically correct.

## RAG Evaluation

- Retrieval precision: fraction of retrieved items that are relevant.
- Retrieval recall: fraction of relevant items that were retrieved.
- Context relevance: whether supplied context helps the question.
- Faithfulness: whether answer claims follow from context.
- Answer relevance/correctness: whether final response solves the task.

Evaluate retrieval and generation separately so a bad answer can be traced to indexing, retrieval, reranking, context construction, or generation.

# PART 22 - SECURITY AND RESPONSIBLE AI

## AI Security Risks

- **Prompt injection:** untrusted text manipulates instructions.
- **Data leakage:** model, logs, prompts, retrieval, or tools expose secrets.
- **Jailbreaking:** attempts to bypass behavior restrictions.
- **Model poisoning:** malicious training or fine-tuning data changes behavior.
- **Adversarial examples:** small input changes cause wrong predictions.
- **Excessive agency/tool abuse:** model performs unauthorized actions.
- **Authentication/authorization failures:** users access another tenant's data or tools.

Defenses: least privilege, server-side authorization, input/output validation, secret redaction, data classification, sandboxed tools, allowlists, rate limits, quotas, human approval, monitoring, red teaming, dependency security, and incident response. Do not rely on a prompt as the only security boundary.

## Responsible AI

- **Bias:** systematic disadvantage in data or behavior.
- **Fairness:** define and measure appropriate subgroup outcomes; metrics may conflict.
- **Explainability:** communicate reasons or evidence for outputs.
- **Interpretability:** how directly a model's internal decision process can be understood.
- **Privacy:** minimize, protect, retain, and delete data appropriately.
- **Transparency:** disclose capabilities, limits, data use, and automation.
- **Accountability:** humans and organizations own decisions and remediation.
- **Safety:** prevent foreseeable harm and provide escalation.

Responsible AI is a lifecycle practice: assess before launch, monitor after launch, document decisions, and provide recourse.

# PART 23 - COMPUTER VISION

## Tasks

- **Image classification:** one or more labels for an image.
- **Object detection:** class plus bounding box for each object.
- **Segmentation:** class per pixel; semantic labels regions, instance segmentation separates objects.
- **OCR:** extracts text from images.
- **Face recognition:** compares or identifies face representations; high privacy and fairness risk.

## Models and Transfer Learning

ResNet uses residual connections to train deep CNNs. YOLO is a family of fast object detectors. U-Net uses an encoder-decoder with skip connections for segmentation. Vision Transformers apply attention to image patches.

Transfer learning starts with a pretrained visual model and adapts it to a smaller domain dataset. Freeze layers when data/compute is limited; fine-tune carefully to avoid overfitting or catastrophic forgetting.

Metrics depend on task: accuracy/F1 for classification, IoU and mAP for detection/segmentation, and character/word error rate for OCR.

# PART 24 - REINFORCEMENT LEARNING

## Core Loop

```text
state -> policy chooses action -> environment returns reward + next state -> update
```

The goal is usually to maximize discounted cumulative reward:

```text
G_t = r_t + gamma*r_{t+1} + gamma^2*r_{t+2} + ...
```

`gamma` controls how much future rewards matter. A value function estimates expected return from a state. A Q-value estimates expected return from a state-action pair. A policy maps states to actions.

## Q-Learning and DQN

Q-learning updates toward the best next action:

```text
Q(s,a) <- Q(s,a) + alpha * [r + gamma*max Q(s',a') - Q(s,a)]
```

DQN uses a neural network to approximate Q-values. Experience replay breaks correlation by sampling past transitions; a target network stabilizes targets. Exploration strategies such as epsilon-greedy balance trying actions with using known good actions.

RL is difficult because rewards can be sparse, environments can be expensive, policies can be unsafe, and offline data may not support exploration. Use simulation, constraints, reward design, and evaluation carefully.

# PART 25 - MULTIMODAL AI

Multimodal systems process or generate more than one modality: text, image, audio, or video.

```text
image/audio/video -> modality encoder -> shared or connected representation
text -> text encoder -----------------> multimodal model -> output
```

Examples include vision-language models, image-text embeddings, speech-to-text, text-to-speech, and multimodal LLMs. Challenges include alignment between modalities, different sampling rates, large inputs, privacy, latency, and evaluation across formats.

# PART 26 - AI SYSTEM DESIGN

## Design Method

1. Clarify users, task, success metric, constraints, and failure cost.
2. Define inputs, outputs, data sources, and prediction time.
3. Choose baseline and model based on quality, latency, cost, and interpretability.
4. Draw online request flow and offline data/training flow.
5. Design storage, indexing, caching, queues, APIs, and authorization.
6. Define evaluation, monitoring, drift detection, rollback, and human escalation.
7. Estimate scale, capacity, GPU/CPU needs, and privacy/security controls.

## Example: Customer-Support RAG Agent

```text
user -> authenticated API -> intent/router -> retrieve authorized documents
     -> rerank -> LLM response draft -> policy/PII checker
     -> answer with sources OR human escalation
```

Offline: ingest documents, remove stale content, chunk, embed, index, and evaluate retrieval. Online: authenticate, retrieve within tenant permissions, enforce token/cost budgets, stream response, log trace IDs, and capture feedback.

## Example: Fraud Detection

```text
events -> feature pipeline -> fraud model -> threshold/policy
       -> approve, review, or block -> feedback -> retraining
```

Use time-aware validation, leakage checks, precision/recall and cost-based thresholds, drift monitoring, explanations for reviewers, and a fallback when features are unavailable.

## Example: Real-Time LLM Service

```text
client -> gateway/auth/rate limit -> queue/router -> model replicas
       -> cache/stream -> response
```

Plan for retries, timeouts, provider fallback, prompt/model versioning, token budgets, load shedding, observability, and data residency.

# PART 27 - INTERVIEW WHY QUESTIONS

## ML

**Why split train/validation/test?** To learn, tune, and finally estimate unseen performance without reusing the same evidence.

**Why normalize or standardize?** Distance and gradient-based methods can be dominated by feature scale; scaling improves geometry and optimization.

**Why does overfitting happen?** A model has enough flexibility to memorize noise or has too little data relative to complexity.

**Bias vs variance?** Bias is systematic error from overly simple assumptions; variance is sensitivity to the training sample.

**Why cross-validation?** It gives a more stable estimate from multiple validation splits and uses training data efficiently.

**Why regularization?** It discourages complexity and can improve generalization.

**Why random forest?** Diverse trees averaged together reduce variance and work well on tabular data with little preprocessing.

**Why XGBoost?** Sequential error correction plus regularization and efficient implementation often gives strong tabular performance.

**Why PCA?** To compress correlated high-dimensional features, reduce computation, or visualize data; it may reduce interpretability.

## Deep Learning

**Why ReLU in hidden layers?** It adds nonlinearity and often has better gradient flow than saturating sigmoid/tanh.

**Why backpropagation?** It efficiently computes each parameter's contribution to loss using the chain rule.

**Why CNN for images?** Local connectivity and shared filters exploit spatial structure with fewer parameters.

**Why RNN?** It carries state across sequence steps, though sequential computation and long-term gradients are limitations.

**Why LSTM/GRU?** Gates control information flow and reduce the long-term memory problem of simple RNNs.

**Why Adam/AdamW?** Adaptive steps and momentum often make optimization easier; they still require tuning and validation.

**Why dropout?** Randomly disabling neurons discourages co-adaptation and reduces overfitting during training.

## NLP, LLMs, and RAG

**Why embeddings?** They provide dense vectors where semantic similarity can be measured and searched.

**Why attention?** It allows each token to use the most relevant context dynamically.

**Why transformers?** They parallelize training and model long-range relationships better than basic recurrence.

**What is a token?** A model-specific text unit that affects cost and context length.

**What is hallucination?** A generated claim that is unsupported, false, or fabricated.

**What is a context window?** The maximum model input/output context for one request, not persistent memory.

**Why RAG?** It provides changing or private evidence at query time and can return sources without changing model weights.

**RAG vs fine-tuning?** RAG changes supplied context for knowledge; fine-tuning changes weights for behavior or task adaptation.

**Why vector databases?** They make embedding similarity search and metadata filtering practical at scale.

**RAG vs long context?** RAG selects relevant evidence; long context sends more material directly, often at higher cost and with more noise.

## Agents and Production

**Why use agents?** To complete tasks requiring dynamic tool selection or multiple steps; use workflows when steps are predictable.

**What is function calling?** Structured model output that requests an application-defined function; the application executes and validates it.

**How deploy a model?** Package versioned preprocessing and model, expose a controlled API or batch job, monitor quality/operations, and support rollback.

**How reduce inference latency?** Smaller/quantized models, batching where appropriate, caching, shorter prompts, fast retrieval, streaming, optimized kernels, and measured scaling.

**How reduce LLM cost?** Route simple tasks to smaller models, reduce unnecessary tokens, cache safe results, batch work, retrieve selectively, and monitor spend.

**How monitor drift?** Compare production feature/output distributions and delayed labels with training baselines; distinguish data drift from concept drift.

**How secure an AI app?** Enforce authorization outside the model, isolate tenants, validate tool calls, protect secrets, treat retrieved text as untrusted, log safely, and test attacks.

# FINAL INTERVIEW REVISION

## Must-Know Concepts

- AI vs ML vs DL vs GenAI vs LLM.
- Features, labels, splits, leakage, pipelines, and data quality.
- Scaling, encoding, missing values, imbalance, and EDA.
- Linear/logistic regression, trees, forests, boosting, XGBoost, SVM, KNN, Naive Bayes.
- Classification and regression metrics and threshold tradeoffs.
- Clustering, PCA, overfitting, bias/variance, regularization, CV, and tuning.
- Neurons, activations, forward pass, loss, backpropagation, optimizers, CNN, RNN, LSTM, and GRU.
- Tokenization, TF-IDF, embeddings, attention, transformers, tokens, context, and sampling.
- Pretraining, fine-tuning, alignment, prompting, hallucination, RAG, reranking, and vector search.
- Agents, tools, state, memory, human approval, APIs, MLOps, deployment, monitoring, optimization, security, and responsible AI.

## Most Important Comparisons

| Comparison | Short answer |
|---|---|
| Classification vs regression | Category vs continuous number |
| Precision vs recall | Avoid false positives vs avoid false negatives |
| Bagging vs boosting | Parallel variance reduction vs sequential error correction |
| Ridge vs Lasso | Shrinks weights vs can remove features |
| PCA vs LDA | Unsupervised variance vs supervised class separation |
| CNN vs RNN | Spatial structure vs sequential state |
| RNN vs LSTM/GRU | Simple recurrence vs gated memory |
| Parameters vs hyperparameters | Learned values vs chosen settings |
| RAG vs fine-tuning | Runtime evidence vs changed model weights |
| RAG vs long context | Retrieved subset vs larger direct input |
| Workflow vs agent | Fixed steps vs dynamic tool decisions |
| Data drift vs concept drift | Input distribution changes vs input-target relationship changes |

## Essential Formulas

```text
Linear regression:       y_hat = Xw + b
MSE:                     mean((y - y_hat)^2)
MAE:                     mean(abs(y - y_hat))
R2:                      1 - SS_res / SS_tot
Precision:               TP / (TP + FP)
Recall:                  TP / (TP + FN)
F1:                      2PR / (P + R)
Standardization:         (x - mean) / standard_deviation
Min-max scaling:         (x - min) / (max - min)
Gradient update:         w <- w - learning_rate * gradient
Sigmoid:                 1 / (1 + exp(-z))
Bayes:                   P(A|B) = P(B|A)P(A) / P(B)
Cosine similarity:       (a dot b) / (||a|| ||b||)
Attention:               softmax(QK^T / sqrt(d_k))V
Q-learning:              Q <- Q + alpha[r + gamma*max(Q_next) - Q]
```

## End-to-End AI/ML Flow

```text
Problem
  -> Data
  -> Preprocessing
  -> Features/Embeddings
  -> Training
  -> Validation and Evaluation
  -> Deployment
  -> Monitoring and Retraining
```

```text
Traditional ML
  -> Deep Learning
  -> Transformers
  -> LLMs
  -> RAG
  -> Agents
  -> Production AI
```

## Final Interview Answer Pattern

For any technology, answer in this order:

1. Define it in simple language.
2. State the problem it solves.
3. Show the input-to-output flow.
4. Explain one important internal idea or formula.
5. Give a practical use case.
6. State when to use it and its limitations.
7. Compare it with the closest alternative.
8. Mention evaluation, monitoring, and security when production is involved.
