# AI / ML / DL / GenAI — Complete Interview Study Notes
# PART 1 — Foundations & Prerequisites

---

> **How to use these notes:**
> Read from top to bottom. Every concept builds on the previous one.
> Do not skip sections — the flow is intentional.
> At the end of each part, review the Interview Questions before moving on.

---

## Table of Contents — PART 1

- [1.1 Why Foundations Matter](#11-why-foundations-matter)
- [1.2 Programming Foundations](#12-programming-foundations)
- [1.3 Computer Science Foundations](#13-computer-science-foundations)
- [1.4 Software Engineering Foundations](#14-software-engineering-foundations)
- [1.5 Interview Questions — Part 1](#15-interview-questions--part-1)

---

## 1.1 Why Foundations Matter

Before you write a single line of ML code, you need to understand what you are working with.

AI/ML is not magic. It is:
- **Mathematics** applied to data
- **Algorithms** running on computers
- **Software engineering** to make it production-ready

If you are already a software engineer who knows Python, data structures, and REST APIs — you are closer to AI/ML than you think. The goal of this section is to make sure your foundation is solid before you go deeper.

---

## 1.2 Programming Foundations

### Python

Python is the primary language for AI/ML. Almost every ML framework (TensorFlow, PyTorch, scikit-learn, Hugging Face) has a Python API.

**Why Python?**
- Simple, readable syntax
- Massive library ecosystem for data and ML
- Fast prototyping
- Huge community

---

### 1.2.1 Python Basics You Must Know

#### Functions

```python
def add(a, b):
    return a + b

result = add(3, 4)  # result = 7
```

A function takes input, does something, and returns output. In ML, almost every operation (forward pass, loss calculation, gradient update) is a function.

#### Classes and OOP

```python
class Dog:
    def __init__(self, name):
        self.name = name        # attribute

    def bark(self):             # method
        return f"{self.name} says Woof!"

d = Dog("Rex")
print(d.bark())                 # Rex says Woof!
```

**OOP Concepts you need:**

| Concept | Simple Meaning |
|---|---|
| Class | Blueprint for an object |
| Object | An instance of a class |
| Attribute | Data stored inside an object |
| Method | Function defined inside a class |
| Inheritance | One class inherits from another |
| Encapsulation | Hiding internal details |
| Polymorphism | Different objects respond to the same method differently |

In ML: every model (`LinearRegression`, `RandomForest`, `BERT`) is a class. You create objects from them and call methods like `.fit()` and `.predict()`.

#### Data Structures

```python
# List — ordered, mutable
scores = [85, 92, 78, 95]

# Tuple — ordered, immutable
point = (10, 20)

# Dictionary — key-value pairs
student = {"name": "Alice", "grade": "A"}

# Set — unique, unordered
unique_words = {"the", "cat", "sat"}
```

**Why they matter in ML:**
- Lists/arrays → store features, labels, predictions
- Dictionaries → configuration, hyperparameters
- Sets → vocabulary in NLP

---

### 1.2.2 NumPy

NumPy (Numerical Python) is the foundation of all numerical computation in Python.

**The core idea:** Instead of using slow Python loops, NumPy lets you operate on entire arrays at once (vectorized operations).

```python
import numpy as np

a = np.array([1, 2, 3, 4])
b = np.array([10, 20, 30, 40])

print(a + b)        # [11, 22, 33, 44] — element-wise
print(a * 2)        # [ 2,  4,  6,  8]
print(np.mean(a))   # 2.5
print(np.dot(a, b)) # 1*10 + 2*20 + 3*30 + 4*40 = 300
```

**Key NumPy concepts:**

| Concept | Meaning |
|---|---|
| `ndarray` | N-dimensional array (the core data structure) |
| `shape` | Dimensions of the array, e.g., `(100, 5)` = 100 rows, 5 columns |
| `dtype` | Data type of elements (float32, int64, etc.) |
| `reshape` | Change the shape without changing data |
| `broadcasting` | Operations between arrays of different shapes |
| `dot product` | Fundamental matrix operation used in every neural network |

```python
# Shape example
X = np.zeros((100, 5))  # 100 samples, 5 features
print(X.shape)           # (100, 5)
```

**Why NumPy matters:** Every ML operation — weight matrices, feature arrays, gradient calculations — is a NumPy array (or its equivalent in PyTorch/TensorFlow).

---

### 1.2.3 Pandas

Pandas is for working with **tabular data** — tables with rows and columns, like a spreadsheet or SQL table.

```python
import pandas as pd

# Create a DataFrame (a table)
df = pd.DataFrame({
    "age": [25, 30, 35],
    "salary": [50000, 70000, 90000],
    "city": ["Delhi", "Mumbai", "Bangalore"]
})

print(df.head())       # first 5 rows
print(df.shape)        # (3, 3) — 3 rows, 3 columns
print(df.describe())   # statistical summary
print(df["age"].mean()) # 30.0
```

**Key Pandas operations you need:**

| Operation | Code | Purpose |
|---|---|---|
| Load CSV | `pd.read_csv("data.csv")` | Load a dataset |
| Check shape | `df.shape` | How many rows and columns |
| Check missing | `df.isnull().sum()` | Count missing values per column |
| Select column | `df["column"]` | Get one column |
| Filter rows | `df[df["age"] > 30]` | Filter by condition |
| Group by | `df.groupby("city").mean()` | Aggregate by group |
| Drop column | `df.drop("col", axis=1)` | Remove a column |

**Why Pandas matters:** Before training any ML model, you load your data with Pandas, inspect it, clean it, and prepare it.

---

### 1.2.4 Exception Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
finally:
    print("This always runs")
```

In ML pipelines, errors can happen when loading data, calling APIs, or running inference. Proper error handling keeps systems robust.

---

### 1.2.5 File Handling and JSON

ML systems constantly read/write files — datasets, model configs, results.

```python
# Reading a file
with open("data.txt", "r") as f:
    content = f.read()

# JSON — the universal format for APIs and configs
import json

config = {"model": "gpt-4", "temperature": 0.7, "max_tokens": 512}

# Write to file
with open("config.json", "w") as f:
    json.dump(config, f)

# Read from file
with open("config.json", "r") as f:
    loaded = json.load(f)
```

**Why JSON matters:** Every LLM API (OpenAI, Anthropic, Google) sends and receives data as JSON.

---

### 1.2.6 Virtual Environments

A virtual environment is an isolated Python environment where you install only the packages your project needs.

```bash
# Create a virtual environment
python -m venv myenv

# Activate it (Windows)
myenv\Scripts\activate

# Install packages
pip install numpy pandas scikit-learn

# Save dependencies
pip freeze > requirements.txt
```

**Why?** Different projects often need different versions of libraries. Virtual environments prevent conflicts.

---

### 1.2.7 APIs and HTTP

An **API** (Application Programming Interface) is a way for programs to talk to each other.

In AI/ML:
- You call an OpenAI API to get a response from GPT-4
- You expose your model as an API so other services can use it

```python
import requests

response = requests.post(
    "https://api.openai.com/v1/chat/completions",
    headers={"Authorization": "Bearer YOUR_API_KEY"},
    json={
        "model": "gpt-4",
        "messages": [{"role": "user", "content": "Hello!"}]
    }
)

data = response.json()
print(data["choices"][0]["message"]["content"])
```

**Key HTTP concepts:**

| Concept | Meaning |
|---|---|
| GET | Retrieve data |
| POST | Send data / create resource |
| PUT | Update a resource |
| DELETE | Remove a resource |
| Status 200 | OK |
| Status 400 | Bad Request |
| Status 401 | Unauthorized |
| Status 500 | Server Error |

---

## 1.3 Computer Science Foundations

### 1.3.1 Data Structures

You don't need to be a DSA expert for ML interviews, but you need to understand these:

| Data Structure | Simple Meaning | When Used in ML |
|---|---|---|
| Array / List | Ordered collection of elements | Feature vectors, training data |
| Dictionary / Hash Map | Key-value storage | Vocabulary mapping in NLP |
| Stack | Last-in, first-out | Backpropagation (call stack) |
| Queue | First-in, first-out | Batch processing |
| Tree | Hierarchical structure | Decision trees, search |
| Graph | Nodes connected by edges | Neural network graphs, knowledge graphs |
| Heap | Fast min/max retrieval | Priority queuing |

---

### 1.3.2 Algorithms and Complexity

**Time Complexity** tells you how the runtime of an algorithm grows with input size.
**Space Complexity** tells you how much memory it uses.

Common notations:

| Notation | Meaning | Example |
|---|---|---|
| O(1) | Constant time | Array index lookup |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Scanning all items once |
| O(n²) | Quadratic | Nested loops over n items |
| O(2ⁿ) | Exponential | Brute-force search |

**Why this matters in ML:**
- Training on a million samples in O(n²) is too slow
- KNN (K-Nearest Neighbors) is O(n) per query — this is a real problem at scale
- Sorting 1 billion embeddings for search — you need efficient indexing

---

### 1.3.3 OOP in ML Context

Every ML model follows OOP principles:

```python
from sklearn.linear_model import LinearRegression

# Creating an object (model instance)
model = LinearRegression()

# Calling a method (training)
model.fit(X_train, y_train)

# Calling a method (prediction)
predictions = model.predict(X_test)

# Accessing attributes (learned parameters)
print(model.coef_)    # learned weights
print(model.intercept_)  # learned bias
```

Understanding OOP helps you:
- Read ML library source code
- Write custom model classes
- Understand PyTorch's `nn.Module` pattern

---

### 1.3.4 DBMS and SQL

Many ML pipelines involve databases. You need basic SQL.

```sql
-- Get average salary by department
SELECT department, AVG(salary)
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000
ORDER BY AVG(salary) DESC;
```

**Key SQL concepts for ML:**

| Concept | Purpose |
|---|---|
| SELECT | Retrieve data |
| WHERE | Filter rows |
| JOIN | Combine tables |
| GROUP BY | Aggregate by category |
| ORDER BY | Sort results |
| INDEX | Speed up queries |

**Why?** Training data often lives in SQL databases. Feature engineering sometimes happens in SQL before data reaches the model.

---

### 1.3.5 Operating Systems Basics

| Concept | Why It Matters for AI |
|---|---|
| Process vs Thread | Model serving uses multiprocessing/threading |
| Memory management | GPU VRAM is limited — you need to manage it |
| File system | Reading large datasets efficiently |
| Environment variables | API keys, config settings |
| Signals | Graceful shutdown of training jobs |

---

### 1.3.6 Computer Networks Basics

| Concept | Why It Matters |
|---|---|
| IP/DNS | Connecting to API servers |
| HTTP/HTTPS | Calling LLM APIs |
| TCP vs UDP | Streaming responses use SSE/WebSocket |
| Latency | Critical for real-time inference |
| Bandwidth | Large model downloads, large dataset transfers |

---

### 1.3.7 Git and GitHub

Version control is essential.

```bash
git init                    # start a repo
git add .                   # stage changes
git commit -m "Add model"  # commit
git push origin main        # push to GitHub
git pull                    # get latest changes
git checkout -b feature     # create a branch
git merge feature           # merge a branch
```

**Why Git matters in ML:**
- Track model experiments
- Collaborate on ML pipelines
- CI/CD for model deployment triggers on Git push

---

### 1.3.8 Linux Basics

Most ML servers run Linux. You need basic shell commands.

```bash
ls -la              # list files
pwd                 # current directory
cd /path/to/dir     # change directory
mkdir mydir         # make directory
cp file1 file2      # copy file
mv file1 file2      # move/rename file
rm file             # delete file
cat file.txt        # print file content
grep "word" file    # search in file
top / htop          # monitor CPU/memory
nvidia-smi          # monitor GPU (crucial for ML)
ps aux              # list running processes
kill PID            # stop a process
chmod +x script.sh  # make script executable
```

The `nvidia-smi` command is especially important — it shows you GPU memory usage during training.

---

## 1.4 Software Engineering Foundations

### 1.4.1 REST APIs

**REST** (Representational State Transfer) is the standard way to build web APIs.

```
Client                Server
  |                     |
  |--- GET /users ----→ |
  |                     | (look up users in DB)
  |←-- 200 + JSON ----- |
```

**Key principles:**
- Each URL is a resource (e.g., `/users`, `/predictions`)
- Use HTTP methods (GET, POST, PUT, DELETE)
- Stateless — each request is independent
- Returns JSON

**In ML context:** Your trained model becomes a REST API that other services call.

---

### 1.4.2 FastAPI (Important for ML Deployment)

FastAPI is the most popular Python framework for exposing ML models as APIs.

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

class PredictionRequest(BaseModel):
    text: str

@app.post("/predict")
def predict(request: PredictionRequest):
    # call your model here
    result = model.predict(request.text)
    return {"prediction": result}
```

Run with:
```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

**Why FastAPI?**
- Fast (built on Starlette + Pydantic)
- Auto-generates API documentation at `/docs`
- Type-safe inputs with Pydantic
- Async support for handling concurrent requests

---

### 1.4.3 Docker

Docker lets you package an application and all its dependencies into a single portable unit called a **container**.

**The problem Docker solves:**
> "It works on my machine but not on the server."

```dockerfile
# Dockerfile for an ML API
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
docker build -t my-ml-api .        # build image
docker run -p 8000:8000 my-ml-api  # run container
```

**Flow:**

```
Your Code + Model + Dependencies
            ↓
       Dockerfile
            ↓
      Docker Image
            ↓
   Docker Container (runs anywhere)
```

**Why Docker matters in ML:**
- Consistent environment between development and production
- Easy to deploy to cloud platforms
- Kubernetes uses Docker containers to scale services

---

### 1.4.4 Cloud Basics

The three major cloud providers for AI/ML:

| Provider | Key ML Services |
|---|---|
| AWS | SageMaker, Bedrock, EC2 GPU instances |
| Google Cloud | Vertex AI, TPUs, BigQuery |
| Azure | Azure ML, Azure OpenAI Service |

**Key cloud concepts:**

| Concept | Simple Meaning |
|---|---|
| VM / Instance | A virtual computer you rent in the cloud |
| GPU Instance | A computer with GPU for training/inference |
| Object Storage | Store large files (e.g., S3, GCS) |
| Serverless | Run code without managing servers |
| Auto-scaling | Automatically add/remove servers based on load |

---

### 1.4.5 Authentication and Security Basics

When building AI applications:

| Concept | Purpose |
|---|---|
| API Keys | Secret token to authenticate API calls |
| Environment Variables | Store secrets (never hardcode keys in code) |
| HTTPS | Encrypt data in transit |
| Rate Limiting | Prevent abuse of your API |
| Input Validation | Reject malicious input |

```python
# Never do this:
api_key = "sk-abc123..."  # hardcoded key — DANGEROUS

# Always do this:
import os
api_key = os.getenv("OPENAI_API_KEY")
```

---

### 1.4.6 Caching

Caching stores the result of an expensive operation so you don't have to repeat it.

```
First Request:
User asks question → LLM generates answer (expensive, slow) → Cache the result

Second Request (same question):
User asks question → Return cached answer (fast, free)
```

Tools:
- **Redis** — in-memory cache, extremely fast
- **Memcached** — similar to Redis

**Why caching matters in AI:**
- LLM API calls cost money
- Caching repeated queries saves cost and reduces latency
- Embedding computations are expensive — cache them

---

## 1.5 Interview Questions — Part 1

---

**Q: What is the difference between a list and a tuple in Python?**

A: A list is mutable (can be changed after creation). A tuple is immutable (cannot be changed). Lists use `[]`, tuples use `()`. In ML, model parameters that shouldn't change during inference are often stored as tuples.

---

**Q: What is a virtual environment and why should you use one?**

A: A virtual environment is an isolated Python installation for a specific project. It prevents library version conflicts between projects. For example, one project might need NumPy 1.21 and another might need NumPy 1.26 — virtual environments keep them separate.

---

**Q: What is the difference between a process and a thread?**

A: A process is an independent running program with its own memory. A thread is a unit of execution within a process, sharing memory with other threads. In ML, PyTorch DataLoader uses multiple processes for fast data loading.

---

**Q: Why is Python slow but still used for ML?**

A: Python itself is slow because it is interpreted. However, ML libraries like NumPy, PyTorch, and TensorFlow are written in C/C++/CUDA underneath. Python is just the interface. The actual heavy computation happens in highly optimized native code.

---

**Q: What is Docker and why is it important for ML deployment?**

A: Docker packages code, dependencies, and configuration into a portable container. It solves the "works on my machine" problem by ensuring the same environment runs everywhere. In ML, you package your model, FastAPI server, and dependencies into a Docker container and deploy it to any cloud server.

---

**Q: What is an API and what does REST mean?**

A: An API is a way for programs to communicate. REST (Representational State Transfer) is an architectural style for web APIs that uses standard HTTP methods (GET, POST, PUT, DELETE) and returns data as JSON. LLM services like OpenAI use REST APIs.

---

**Q: What is time complexity and why does it matter in ML?**

A: Time complexity describes how an algorithm's runtime grows with input size. It matters in ML because you often work with millions of data points. An O(n²) algorithm on 1 million samples would perform 1 trillion operations — completely infeasible. Choosing efficient algorithms and data structures is critical.

---

**Q: What is the purpose of `model.fit(X_train, y_train)` in scikit-learn?**

A: `fit()` trains the model. It takes:
- `X_train` → input features (what the model learns from)
- `y_train` → expected outputs (labels)
The model adjusts its internal parameters to map inputs to outputs.

---

**Q: How do you keep an API key secure in a Python application?**

A: Store it as an environment variable and read it with `os.getenv("KEY_NAME")`. Never hardcode secrets in source code because they can be exposed if the code is committed to a public repository.

---

**Q: What is caching and when would you use it in an AI application?**

A: Caching stores results of expensive operations. In AI, you'd cache LLM responses to identical queries (saves cost), cache computed embeddings (saves time), or cache database query results (reduces load). Redis is the most common tool for this.

---

## Summary — What You Learned in Part 1

```
Software Engineering Foundation
           |
    ┌──────┼──────────┐
    |      |          |
 Python  CS Basics   SE Skills
    |      |          |
 NumPy   Data      REST APIs
 Pandas  Structures  FastAPI
 OOP     SQL        Docker
 JSON    Git        Cloud
 APIs    Linux      Security
    |
    └──→  Ready to understand Data and Mathematics
```

The concepts in this part are not "nice to have" — they are **used every single day** in ML engineering. A model without good software engineering around it never reaches production.

---

**Next:** PART 2 — Data Fundamentals & Mathematics for ML

> Say **NEXT** to continue to Part 2.
