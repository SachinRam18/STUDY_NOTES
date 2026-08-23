# AI / ML / DL / GenAI — Complete Interview Study Notes
# PART 3 — Artificial Intelligence & Machine Learning Fundamentals

---

> **Dependency:** Parts 1 and 2 must be read first.
> Part 2 gave you the mathematical tools. Now we use them to understand what AI and ML actually are.

---

## Table of Contents — PART 3

- [3.1 What is Artificial Intelligence?](#31-what-is-artificial-intelligence)
- [3.2 AI vs ML vs DL — The Big Picture](#32-ai-vs-ml-vs-dl--the-big-picture)
- [3.3 Types of AI](#33-types-of-ai)
- [3.4 AI Approaches](#34-ai-approaches)
- [3.5 Intelligent Agents](#35-intelligent-agents)
- [3.6 What is Machine Learning?](#36-what-is-machine-learning)
- [3.7 Traditional Programming vs Machine Learning](#37-traditional-programming-vs-machine-learning)
- [3.8 Types of Machine Learning](#38-types-of-machine-learning)
- [3.9 Supervised Learning](#39-supervised-learning)
- [3.10 Unsupervised Learning](#310-unsupervised-learning)
- [3.11 Semi-Supervised Learning](#311-semi-supervised-learning)
- [3.12 Reinforcement Learning](#312-reinforcement-learning)
- [3.13 The Complete ML Workflow](#313-the-complete-ml-workflow)
- [3.14 Interview Questions — Part 3](#314-interview-questions--part-3)

---

## 3.1 What is Artificial Intelligence?

### Definition

Artificial Intelligence (AI) is the branch of computer science focused on building systems that can perform tasks that normally require human intelligence.

Human intelligence includes:
- Understanding language
- Recognizing faces
- Making decisions
- Learning from experience
- Solving problems

AI tries to replicate these capabilities in software.

### Why Was AI Created?

Humans have limited time and attention. Many tasks that require intelligence are:
- Too slow to do manually (e.g., reading millions of documents)
- Too complex for simple rules (e.g., detecting fraud in millions of transactions)
- Too repetitive (e.g., classifying emails as spam)
- Beyond human capability at scale (e.g., processing hospital images globally)

AI allows these tasks to be automated, scaled, and improved over time.

---

## 3.2 AI vs ML vs DL — The Big Picture

This is one of the most frequently asked conceptual questions in interviews.

```
Artificial Intelligence
│
│  (any technique that makes machines "smart")
│
└── Machine Learning
    │
    │  (machines learn from data, instead of explicit rules)
    │
    └── Deep Learning
        │
        │  (ML using multi-layered neural networks)
        │
        └── Generative AI
            │
            │  (Deep Learning models that can CREATE new content)
            │
            └── Large Language Models (LLMs)
                   (Massive generative models trained on text)
```

### Simple Comparison Table

| Term | What It Is | What Makes It Special |
|---|---|---|
| **AI** | Broad field — any approach to make machines intelligent | Umbrella term |
| **ML** | Subset of AI — systems that learn from data | No explicit rules needed |
| **DL** | Subset of ML — uses layered neural networks | Learns features automatically |
| **GenAI** | Subset of DL — generates new content | Creates text, images, code, audio |
| **LLM** | Subset of GenAI — massive models trained on text | Conversational, instruction-following |

### Concrete Example

Imagine the task: "Identify spam emails."

| Approach | How It Works |
|---|---|
| **Traditional AI** | Write rules: if email contains "WIN PRIZE" → spam |
| **ML** | Feed 100,000 labeled emails (spam/not spam) → model learns patterns |
| **DL** | Use a neural network → learns subtle patterns from raw text |
| **LLM** | Use GPT-4 with a prompt → understands context and nuance |

Each approach is more powerful — and more complex — than the previous.

---

## 3.3 Types of AI

### Narrow AI (Weak AI)

AI that is designed and trained for **one specific task**.

- Current state of the world — everything we have today is Narrow AI
- Excellent at its specific task, but completely useless at anything else
- A chess AI cannot write code. A spam filter cannot drive a car.

Examples:
- Google Search (search ranking)
- Spotify recommendations (music recommendation)
- Face ID (face recognition)
- ChatGPT (text generation)
- Tesla Autopilot (driving assistance)

---

### Artificial General Intelligence (AGI)

A hypothetical AI that can **perform any intellectual task a human can**.

- Does not exist yet
- Would be able to reason, learn, and generalize across domains
- Could learn chess AND write poetry AND drive a car AND do surgery — without being explicitly trained on each
- Major AI labs (OpenAI, DeepMind, Anthropic) are working toward this

---

### Artificial Superintelligence (ASI)

A hypothetical AI that **surpasses human intelligence in every domain**.

- Completely theoretical at this point
- Would exceed the best human experts in science, creativity, social skills, etc.
- Subject of significant ethical debate

```
Current AI → Narrow AI (here today)
Future?    → AGI → ASI
```

---

## 3.4 AI Approaches

There are multiple ways to build intelligent systems. These are not competing — they are often combined.

---

### Rule-Based Systems (Expert Systems)

**Approach:** Experts encode their knowledge as explicit IF-THEN rules.

```
IF temperature > 38.5°C THEN diagnosis = "fever"
IF email contains "WIN PRIZE" AND sender unknown THEN classify = "spam"
```

**When it works:** When the domain is narrow, well-defined, and rules can be written explicitly.

**Limitation:** Rules break when reality is too complex or ambiguous. Writing rules for understanding natural language is practically impossible.

---

### Search and Planning

**Approach:** Explore a space of possible states to find a path to a goal.

Examples:
- Pathfinding (GPS navigation — find shortest route)
- Chess AI (explore possible moves)
- Robot motion planning

**Key algorithms:** BFS, DFS, A*, Monte Carlo Tree Search

---

### Knowledge Representation

**Approach:** Represent the world as structured facts and relationships.

Examples:
- Semantic Web
- Knowledge Graphs (Google's Knowledge Graph behind search results)
- Ontologies

---

### Machine Learning

**Approach:** Instead of writing rules, let the system learn patterns from data.

This is the dominant approach today. We will cover it in depth throughout these notes.

---

### Deep Learning

**Approach:** Use multi-layered neural networks to learn complex patterns.

Best for: images, text, audio, video — anything with complex non-linear patterns.

---

### Generative AI

**Approach:** Models that learn the distribution of training data and can generate new samples that look like they came from that distribution.

Examples: GPT-4, Stable Diffusion, Suno (music generation).

---

## 3.5 Intelligent Agents

An **agent** is anything that perceives its environment and takes actions.

This concept is fundamental to understanding AI systems — and especially modern AI agents (which we'll cover in Part 14).

### Components of an Intelligent Agent

```
Environment
    |
    | (Perception)
    ↓
Sensors/Inputs
    |
    ↓
Agent (has a GOAL)
    |
    | (Decision Making)
    ↓
Actuators/Actions
    |
    ↓
Environment (changed)
```

| Component | Simple Meaning | Real Example |
|---|---|---|
| **Agent** | The system doing the thinking | A self-driving car's AI |
| **Environment** | The world the agent operates in | The road, other cars, traffic lights |
| **Perception** | How the agent senses the environment | Cameras, LiDAR sensors |
| **State** | The current snapshot of the environment | Position of all nearby cars |
| **Action** | What the agent does | Turn left, accelerate, brake |
| **Goal** | What the agent is trying to achieve | Reach destination safely |
| **Utility** | A measure of how well the agent achieves its goal | Arrived safely in 15 minutes |

### Rational Agent

A **rational agent** selects the action that maximizes its expected utility — it makes the best decision given what it knows.

This is not the same as a perfect agent — it can only be rational with respect to the information it has.

---

## 3.6 What is Machine Learning?

### Definition

Machine Learning is a subset of AI where systems **learn patterns from data** to make predictions or decisions — without being explicitly programmed with rules.

### Simple Explanation

Instead of writing code that says *"if this, do that"*, you give the machine:
1. A large collection of examples (data)
2. The correct answers for those examples (labels)
3. An algorithm that finds the pattern connecting the examples to the answers

The machine figures out the rules by itself.

---

## 3.7 Traditional Programming vs Machine Learning

This comparison is one of the most important conceptual distinctions to understand.

### Traditional Programming

```
Input Data  +  Rules (written by humans)
                       ↓
                    Output
```

You explicitly tell the computer **exactly** what to do at every step.

**Example — Spam filter:**
```python
def is_spam(email):
    if "WIN PRIZE" in email:
        return True
    if "FREE MONEY" in email:
        return True
    if "CLICK HERE NOW" in email:
        return True
    return False
```

**Problem:** Spammers adapt. They start writing "W1N PR1ZE". You cannot write rules for every variation. The world is too complex.

---

### Machine Learning

```
Input Data  +  Output Examples (labeled data)
                       ↓
                   Algorithm
                       ↓
                 Learned Model
                       ↓
         Given new input → produces output
```

You give the machine examples and let it find the rules.

**Example — ML Spam filter:**
```
Training:
  Feed 100,000 emails + "spam" or "not spam" labels
  → Model learns what patterns correlate with spam

Inference:
  New email arrives → Model predicts "spam" or "not spam"
```

The model might learn:
- Certain words are spam indicators
- Certain sender domains are suspicious
- Certain times of day are more common for spam
- etc.

You never wrote any of these rules — the model discovered them.

---

### Side-by-Side Comparison

| Aspect | Traditional Programming | Machine Learning |
|---|---|---|
| Rules | Written by humans | Learned from data |
| Scalability | Hard to scale with complexity | Scales with more data |
| Adaptability | Needs manual updates | Can retrain on new data |
| Explainability | Clear, readable code | Often a "black box" |
| Best for | Simple, rule-based tasks | Complex, pattern-based tasks |
| Example | Tax calculation | Fraud detection |

---

## 3.8 Types of Machine Learning

```
Machine Learning
│
├── Supervised Learning       ← You provide input + correct output
│   ├── Classification        ← Output is a category
│   └── Regression            ← Output is a number
│
├── Unsupervised Learning     ← You provide input only, no labels
│   ├── Clustering            ← Find natural groups
│   ├── Dimensionality Reduction ← Compress data
│   └── Association           ← Find co-occurring patterns
│
├── Semi-supervised Learning  ← Small amount of labels + large unlabeled data
│
└── Reinforcement Learning    ← Agent learns by trial, error, and reward
```

---

## 3.9 Supervised Learning

### Definition

In supervised learning, you train a model on a dataset where **every input has a corresponding correct output label**.

The model learns the mapping: `Input → Output`.

"Supervised" because a teacher (the labels) guides the learning.

### Flow

```
Labeled Training Data
  (input, correct_output)
         ↓
    Training Algorithm
         ↓
      Learned Model
         ↓
 New Input → Prediction
```

---

### 3.9.1 Classification

**Goal:** Predict which **category** an input belongs to.

The output is a discrete class label.

**Examples:**
- Email → Spam or Not Spam
- Tumor → Malignant or Benign
- Image → Cat, Dog, or Bird
- Customer → Will churn or Will stay

**Binary Classification** → 2 classes (spam/not spam)

**Multi-class Classification** → 3+ classes (cat/dog/bird/fish)

**Multi-label Classification** → Multiple correct classes at once (a news article can be tagged as both "Sports" and "Business")

---

### 3.9.2 Regression

**Goal:** Predict a **continuous numerical value**.

The output is a number, not a category.

**Examples:**
- Predict house price based on size, location, age
- Predict tomorrow's temperature
- Predict a patient's blood pressure
- Predict how many units a product will sell

```
Key difference:
  Classification → "Is this email spam?" (Yes/No)
  Regression     → "How much will this house sell for?" ($425,000)
```

---

### Classification vs Regression — Summary

| Aspect | Classification | Regression |
|---|---|---|
| Output type | Category / Label | Continuous number |
| Example output | "Spam" / "Not Spam" | "$425,000" |
| Evaluation metric | Accuracy, F1, AUC | MAE, RMSE, R² |
| Algorithms | Logistic Regression, Decision Tree, SVM | Linear Regression, Ridge, Lasso |

---

## 3.10 Unsupervised Learning

### Definition

In unsupervised learning, you train a model on data that has **no labels**. The model must find structure and patterns in the data on its own.

"Unsupervised" because there is no teacher — no correct answers provided.

### Why is it needed?

Labeling data is expensive and time-consuming. A hospital might have millions of patient records but no one has manually labeled them as "high risk" or "low risk." Unsupervised learning can find natural groupings without labels.

---

### 3.10.1 Clustering

**Goal:** Group similar data points together.

The model finds groups (clusters) in the data — without being told what the groups should be.

```
Raw Data Points (unlabeled):
   × × ×      ○ ○      △ △ △
    × × ×    ○ ○ ○       △ △

After Clustering:
   [Cluster 1]  [Cluster 2]  [Cluster 3]
    × × × ×      ○ ○ ○        △ △ △ △
```

**Examples:**
- Customer segmentation (group customers by purchasing behavior)
- Document clustering (group news articles by topic)
- Anomaly detection (points that don't belong to any cluster = outliers)
- Gene expression analysis (group similar genes)

**Key Algorithm:** K-Means (covered in Part 5)

---

### 3.10.2 Dimensionality Reduction

**Goal:** Reduce the number of features in your data while preserving as much useful information as possible.

**Why:** High-dimensional data is hard to visualize, computationally expensive, and often contains redundant features.

```
100 features → 2 or 3 features
(while keeping the important structure)
```

**Examples:**
- Compress 1000-dimensional word embeddings to 2D for visualization
- Remove redundant features before training
- Speed up training by reducing input size

**Key Algorithm:** PCA — Principal Component Analysis (covered in Part 5)

---

### 3.10.3 Association Rule Learning

**Goal:** Find rules that describe relationships between variables in large datasets.

Classic example — Market Basket Analysis:
```
"Customers who buy bread and butter also tend to buy eggs"

{bread, butter} → {eggs}   (support = 35%, confidence = 70%)
```

Used in:
- Retail (product recommendations)
- Medical diagnosis (symptoms that co-occur)
- Web usage mining

---

## 3.11 Semi-Supervised Learning

### Definition

A hybrid approach — uses a **small amount of labeled data** combined with a **large amount of unlabeled data**.

### Why is it needed?

Labeling data is expensive. But collecting raw data is cheap (photos, web pages, text). Semi-supervised learning lets you get most of the benefit of supervised learning with a fraction of the labeling cost.

### How it works (conceptually)

```
Small Labeled Dataset
    +
Large Unlabeled Dataset
         ↓
  Model trains on labeled data
         ↓
  Uses structure of unlabeled data to improve
         ↓
  Better model than using labeled data alone
```

**Real-world example:** Google Photos can identify people in your photos with high accuracy after you label just a few. It uses those few labeled examples plus the structure across millions of unlabeled photos.

---

## 3.12 Reinforcement Learning

### Definition

Reinforcement Learning (RL) is a type of ML where an **agent** learns to take actions in an **environment** by receiving **rewards** or **penalties** for its actions — like training a dog with treats.

### Flow

```
Agent
  |
  | chooses an Action
  ↓
Environment
  |
  | returns new State + Reward
  ↓
Agent
  |
  | updates its policy based on reward
  ↓
Repeat
```

### Key Components

| Component | Simple Meaning | Example |
|---|---|---|
| **Agent** | The learner/decision-maker | Game-playing AI |
| **Environment** | The world the agent interacts with | The game itself |
| **State** | Current situation of the environment | Board position in chess |
| **Action** | What the agent does | Move a piece |
| **Reward** | Feedback from the environment | +1 for win, -1 for loss |
| **Policy** | The agent's strategy for choosing actions | "If state X, take action Y" |
| **Value** | Expected future reward from a state | "Being in this position is good" |

### Learning Process

```
Start in random state
      ↓
Take an action (explore or exploit)
      ↓
Receive reward (+ or -)
      ↓
Update: "that action in that state was good/bad"
      ↓
Repeat millions of times
      ↓
Agent learns optimal policy
```

### Exploration vs Exploitation Tradeoff

- **Exploration:** Try new actions to discover if they lead to better rewards
- **Exploitation:** Use the best-known action to maximize current reward

**Problem:** If you only exploit, you might miss a better strategy. If you only explore, you never cash in on what you've learned.

**Example:** A restaurant app recommendation system. It could always recommend the restaurant you rated highest (exploit), or occasionally recommend a new restaurant you've never tried (explore) — which might be even better.

### Real-World Applications of RL

- **Game playing:** AlphaGo (chess, Go), OpenAI Five (Dota 2)
- **Robotics:** Teaching robots to walk, grasp objects
- **RLHF:** Reinforcement Learning from Human Feedback — used to align ChatGPT and similar LLMs
- **Autonomous driving:** Learning driving policies
- **Trading:** Portfolio management strategies

---

## 3.13 The Complete ML Workflow

Every ML project — from a simple classifier to a complex production system — follows this general workflow. You must be able to describe each step in an interview.

```
1. Problem Definition
         ↓
2. Data Collection
         ↓
3. Data Cleaning
         ↓
4. Exploratory Data Analysis (EDA)
         ↓
5. Feature Engineering
         ↓
6. Train / Validation / Test Split
         ↓
7. Model Selection
         ↓
8. Model Training
         ↓
9. Model Evaluation
         ↓
10. Hyperparameter Tuning
         ↓
11. Final Evaluation on Test Set
         ↓
12. Deployment
         ↓
13. Monitoring
         ↓
14. Retrain (if needed)
```

---

### Step 1 — Problem Definition

Before writing any code, clarify:
- What are you predicting?
- Is it classification or regression?
- What does success look like? (metric)
- What data is available?
- What are the constraints? (latency, cost, interpretability)

**Interview question:** "How would you approach a new ML problem?"
**Answer:** Always start by understanding the problem deeply before touching any data.

---

### Step 2 — Data Collection

Gather data from:
- Internal databases
- Public datasets
- Web scraping
- APIs
- Manual labeling
- Sensors / logs

**Key concerns:**
- Is it representative of real-world data?
- Is the quantity sufficient?
- Is it properly labeled?

---

### Step 3 — Data Cleaning

Fix data quality issues:
- Handle missing values
- Remove duplicates
- Fix incorrect values
- Handle outliers
- Standardize formats (dates, units, spelling)

**Rule of thumb:** Data scientists spend ~80% of their time on data collection and cleaning.

---

### Step 4 — Exploratory Data Analysis (EDA)

Understand your data before modeling:
- What is the distribution of each feature?
- Are there correlations between features?
- Is the data imbalanced?
- Are there obvious patterns?

**Tools:** Pandas, Matplotlib, Seaborn

```python
import pandas as pd
import seaborn as sns

df = pd.read_csv("data.csv")
print(df.describe())               # statistical summary
print(df.isnull().sum())           # missing values
sns.heatmap(df.corr())             # correlation matrix
sns.histplot(df["age"])            # distribution of age
```

---

### Step 5 — Feature Engineering

Transform raw data into features that best represent the problem for the model:
- Create new features from existing ones
- Encode categorical variables
- Scale numerical features
- Handle date/time features
- Remove irrelevant features

**Example:**
```
Raw: "date_of_birth = 1990-05-15"
→ Engineered: "age = 34", "birth_month = 5", "is_weekend_born = False"
```

---

### Step 6 — Train / Validation / Test Split

```python
from sklearn.model_selection import train_test_split

# First split: separate test set
X_temp, X_test, y_temp, y_test = train_test_split(X, y, test_size=0.15, random_state=42)

# Second split: separate validation from training
X_train, X_val, y_train, y_val = train_test_split(X_temp, y_temp, test_size=0.18, random_state=42)

# Result: ~67% train, ~18% val, ~15% test
```

**Key point:** `random_state=42` makes the split reproducible. Set it for consistency.

---

### Step 7 — Model Selection

Choose the right algorithm based on:
- Type of problem (classification? regression? clustering?)
- Size of dataset (small → simpler models; large → more complex)
- Interpretability requirements (can it be a black box?)
- Speed requirements (training time, inference time)

---

### Step 8 — Model Training

Fit the model to the training data:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)          # training
predictions = model.predict(X_val)  # validation prediction
```

`fit()` → adjusts internal parameters to minimize error on training data.

---

### Step 9 — Model Evaluation

Measure performance on the **validation set**:
- Classification: Accuracy, Precision, Recall, F1, AUC
- Regression: MAE, RMSE, R²

**Important:** Do NOT touch the test set here.

---

### Step 10 — Hyperparameter Tuning

Improve performance by tuning model settings (hyperparameters) — the settings you choose before training, as opposed to parameters the model learns during training.

```
Parameters (learned during training):
  - Weights, biases in a neural network
  - Split thresholds in a Decision Tree

Hyperparameters (set before training):
  - Number of trees in Random Forest
  - Learning rate
  - Number of layers in neural network
  - Regularization strength
```

**Tuning methods:**
- **Grid Search** — try all combinations of a predefined set
- **Random Search** — randomly sample combinations
- **Bayesian Optimization** — intelligently choose next combination based on past results

---

### Step 11 — Final Evaluation on Test Set

Only now, after all tuning is complete, evaluate on the **test set**.

This gives an honest estimate of how the model will perform on new, unseen data.

If this is done before tuning, you risk overfitting to the test set.

---

### Step 12 — Deployment

Package the model and make it available for use:
- Wrap in a FastAPI service
- Containerize with Docker
- Deploy to cloud (AWS, GCP, Azure)

---

### Step 13 — Monitoring

Watch the model's performance over time:
- **Data drift:** Input data distribution changes
- **Concept drift:** The relationship between input and output changes
- **Model degradation:** Performance drops over time

---

### Step 14 — Retrain

When performance drops, retrain the model with fresh data.

---

## 3.14 Interview Questions — Part 3

---

**Q: What is the difference between AI, ML, and Deep Learning?**

A: AI is the broad field of making machines intelligent. ML is a subset of AI where systems learn patterns from data instead of following hand-coded rules. Deep Learning is a subset of ML that uses multi-layered neural networks, making it capable of learning complex patterns from raw data like images and text. All DL is ML, and all ML is AI — but not all AI is ML.

---

**Q: What is the difference between Supervised and Unsupervised Learning?**

A: In supervised learning, every training example has a label (correct answer) — the model learns to map inputs to outputs. In unsupervised learning, there are no labels — the model must find structure in the data on its own (e.g., groups or patterns). Supervised learning is used for prediction (spam detection, price prediction). Unsupervised learning is used for exploration (customer segmentation, anomaly detection).

---

**Q: What is the difference between Classification and Regression?**

A: Both are supervised learning tasks. Classification predicts a discrete category (spam / not spam, cat / dog / bird). Regression predicts a continuous number (house price, temperature, stock return). Use different evaluation metrics: F1/accuracy for classification, RMSE/R² for regression.

---

**Q: Why do we need a separate test set? Can't we just use the validation set?**

A: The validation set is used repeatedly during development — to tune hyperparameters and compare models. Every time you look at validation performance and make a decision, you are indirectly learning information from it. Over time, you can "overfit" to the validation set. The test set is used exactly once, at the very end, to get an unbiased estimate of real-world performance.

---

**Q: What is Reinforcement Learning and how is it different from Supervised Learning?**

A: In supervised learning, the model is given correct answers and learns to imitate them. In reinforcement learning, there are no correct answers — the agent learns by trial and error, receiving rewards or penalties for its actions. RL is used where the correct action cannot be specified in advance but can be evaluated after the fact (game playing, robotics, RLHF for LLMs).

---

**Q: What is the difference between a parameter and a hyperparameter?**

A: A parameter is learned by the model during training (e.g., weights in a neural network, split thresholds in a decision tree). A hyperparameter is set by the engineer before training (e.g., learning rate, number of trees, number of layers). You tune hyperparameters using the validation set; parameters are adjusted automatically by the training algorithm.

---

**Q: What is AGI?**

A: Artificial General Intelligence is a hypothetical AI system capable of performing any intellectual task that a human can, with the ability to generalize across different domains. Unlike today's Narrow AI (which excels at one specific task), AGI would reason, learn, and adapt across all types of problems. It does not exist yet.

---

**Q: What is an Intelligent Agent?**

A: An intelligent agent is any system that perceives its environment through sensors, makes decisions based on a goal, and takes actions through actuators. A rational agent selects the action expected to maximize its utility. This concept is foundational to understanding AI agents built with LLMs.

---

**Q: What happens if you evaluate your model on the training data?**

A: The model will appear to perform perfectly because it has already "seen" that data. This gives a completely misleading picture of real-world performance. A model that memorizes training data (overfitting) will score 100% on training data but fail on new data. This is why we always evaluate on held-out data.

---

**Q: What is concept drift and why does it matter?**

A: Concept drift is when the statistical relationship between features and labels changes over time in production. For example, a fraud detection model trained on 2022 data may perform poorly in 2024 because fraudsters have changed their behavior. This is why model monitoring and periodic retraining are essential in production ML systems.

---

**Q: Describe the complete ML workflow from raw data to deployment.**

A: 
1. Define the problem and metric for success
2. Collect and clean data
3. Explore data (EDA) to understand distributions and patterns
4. Engineer features
5. Split into train/validation/test sets
6. Select and train a model
7. Evaluate on validation set
8. Tune hyperparameters
9. Final evaluation on test set
10. Deploy model as API
11. Monitor for drift
12. Retrain as needed

---

## Summary — What You Learned in Part 3

```
ARTIFICIAL INTELLIGENCE
│
├── Types: Narrow AI (today) → AGI (future) → ASI (theoretical)
│
├── Approaches:
│   Rule-based → Search → Knowledge → ML → DL → GenAI
│
├── Intelligent Agents: perceive → decide → act
│
└── MACHINE LEARNING
    │
    ├── Traditional vs ML: rules vs data-driven
    │
    ├── Supervised Learning
    │   ├── Classification → predict category
    │   └── Regression → predict number
    │
    ├── Unsupervised Learning
    │   ├── Clustering → find groups
    │   ├── Dimensionality Reduction → compress
    │   └── Association → find co-patterns
    │
    ├── Semi-supervised → mix of labeled + unlabeled
    │
    ├── Reinforcement Learning → reward-based learning
    │
    └── ML Workflow (14 steps, end-to-end)
```

The conceptual clarity here is critical. Before jumping into any algorithm, you need to know **which type of problem** you're solving. Every algorithm decision in the next sections starts from this framework.

---

**Next:** PART 4 — Data Preprocessing & Exploratory Data Analysis

> Say **NEXT** to continue to Part 4.
