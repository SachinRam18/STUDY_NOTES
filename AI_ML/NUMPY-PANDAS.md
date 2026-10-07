# NUMPY & PANDAS INTERVIEW NOTES

## NUMPY

### NumPy Basics

**What is it?**

A library for fast multi-dimensional arrays.

**Why?**

Faster and uses less memory than Python lists.

```python
import numpy as np

arr = np.array([1, 2, 3])  # Creates a 1D array
```

**Explanation:**

Converts list to a NumPy array.

**Interview Point:**

Written in C; supports vectorization; elements must be the same data type.

### Array Attributes (`shape`, `size`, `ndim`, `dtype`)

**What is it?**

Properties of an array.

**Why?**

To check data structure before feeding to models.

```python
arr = np.zeros((2, 3))
print(arr.shape, arr.size, arr.ndim, arr.dtype)
```

**Explanation:**

`(2, 3)` creates 2 rows, 3 cols. Prints `(2, 3) 6 2 float64`.

**Interview Point:**

`shape` is essential to debug matrix multiplication errors.

### Array Creation (`zeros`, `ones`, `arange`, `linspace`)

**What is it?**

Built-in functions to generate arrays.

**Why?**

Quickly initialize data.

```python
a = np.zeros((2,2))
b = np.arange(0, 10, 2)
c = np.linspace(0, 1, 5)
```

**Explanation:**

`zeros` makes a matrix of 0s; `arange` steps by 2 `[0,2,4,6,8]`; `linspace` gives 5 evenly spaced numbers.

**Interview Point:**

`arange` excludes the endpoint, `linspace` includes it.

### Indexing, Slicing & 2D Indexing

**What is it?**

Extracting parts of an array.

**Why?**

To view or modify specific data.

```python
arr = np.array([[1, 2], [3, 4]])
val = arr[0, 1]
sub = arr[:, 0]
```

**Explanation:**

`arr[0, 1]` gets row 0 col 1 (`2`). `arr[:, 0]` gets all rows, 1st col.

**Interview Point:**

Slicing creates a **view**, not a copy. Modifying it changes the original.

### Array Manipulation (`reshape`, `flatten`, `T`)

**What is it?**

Changing array dimensions.

**Why?**

ML models require specific input shapes.

```python
flat = arr.flatten()
reshaped = flat.reshape(2, 2)
trans = reshaped.T
```

**Explanation:**

`flatten` makes it 1D. `reshape` makes it 2D again. `T` swaps rows/cols.

**Interview Point:**

`reshape` returns a view; `flatten` returns a copy.

### Element-wise Math & Aggregation (`sum`, `mean`, `max`)

**What is it?**

Math over arrays.

**Why?**

Fast statistical calculations.

```python
res = arr * 10
total = np.sum(arr, axis=0)
```

**Explanation:**

`arr * 10` instantly multiplies all elements. `axis=0` sums columns downwards.

**Interview Point:**

`axis=0` acts vertically (across rows), `axis=1` acts horizontally.

### ⭐ Vectorization & Broadcasting

**What is it?**

NumPy's ability to do math without loops.

**Why?**

Massive speedups.

```python
res = np.array([[1, 2], [3, 4]]) + np.array([10, 20])
```

**Explanation:**

The 1D array stretches (broadcasts) to add to each row of the 2D array.

**Interview Point:**

Never use `for` loops with NumPy if a vectorized operation exists.

### ⭐ Boolean Indexing (Filtering)

**What is it?**

Filtering arrays using conditions.

**Why?**

To extract data meeting criteria.

```python
arr = np.array([1, -2, 3])
pos = arr[arr > 0]
```

**Explanation:**

`arr > 0` creates a True/False mask. `arr[]` applies it, returning `[1, 3]`.

**Interview Point:**

Standard, fastest way to filter data in NumPy/Pandas.

---

## PANDAS

### Pandas Basics (Series & DataFrame)

**What is it?**

Data manipulation library. Series = 1D column, DataFrame = 2D table.

**Why?**

To handle real-world tabular data.

```python
import pandas as pd

df = pd.DataFrame({"Age": [25, 30], "City": ["NY", "LA"]})
```

**Explanation:**

Creates a table from a dictionary.

**Interview Point:**

DataFrames automatically align data by index and handle missing values (`NaN`).

### Inspecting Data (`read_csv`, `head`, `info`, `describe`)

**What is it?**

Loading and viewing data.

**Why?**

To understand dataset structure.

```python
df = pd.read_csv("data.csv")
df.info()
print(df.describe())
```

**Explanation:**

Loads CSV, shows column types/missing values (`info`), and shows math stats (`describe`).

**Interview Point:**

`info()` is best for finding missing values; `describe()` ignores strings.

### ⭐ Selecting Data (`loc` vs `iloc`)

**What is it?**

Extracting rows/cols.

**Why?**

To isolate features.

```python
by_name = df.loc[0, "Age"]
by_pos = df.iloc[0, 0]
```

**Explanation:**

`loc` selects by label ("Age"). `iloc` selects by integer index (0).

**Interview Point:**

If index is `[10, 20]`, `loc[10]` gets row 10. `iloc[0]` gets the first row.

### Filtering (`&`, `|`, `~`, `isin`)

**What is it?**

Subset rows based on logic.

**Why?**

SQL `WHERE` equivalent.

```python
subset = df[(df["Age"] > 18) & (df["City"].isin(["NY", "LA"]))]
```

**Explanation:**

Filters for Age > 18 AND City is NY or LA.

**Interview Point:**

You MUST use `&`, `|`, `~` and wrap conditions in parentheses `()`.

### Handling Missing Data (`isnull`, `dropna`, `fillna`)

**What is it?**

Dealing with `NaN` values.

**Why?**

ML models crash on missing data.

```python
df["Age"] = df["Age"].fillna(df["Age"].mean())
df = df.dropna()
```

**Explanation:**

Fills empty ages with average age, then drops any remaining rows with missing data.

**Interview Point:**

Use `fillna` (imputation) to preserve rows instead of blindly dropping them.

### ⭐ GroupBy

**What is it?**

Splits data, applies a function, combines results.

**Why?**

Equivalent to SQL `GROUP BY`.

```python
avg_salary = df.groupby("City")["Salary"].mean()
```

**Explanation:**

Groups by City, calculates average Salary per city.

**Interview Point:**

`groupby()` must be followed by an aggregate function (`mean()`, `sum()`, `agg()`).

### ⭐ Combining Data (`merge` vs `concat`)

**What is it?**

Joining DataFrames.

**Why?**

Consolidating multiple data sources.

```python
joined = pd.merge(df1, df2, on="ID", how="left")
stacked = pd.concat([df1, df2])
```

**Explanation:**

`merge` joins horizontally on a key (SQL Left Join). `concat` stacks vertically.

**Interview Point:**

`merge` is relational joining; `concat` is brute-force stacking.

### Transformation (`apply` vs `map`)

**What is it?**

Modifying data values.

**Why?**

For custom logic standard functions can't do.

```python
df["Is_Adult"] = df["Age"].apply(lambda x: x > 18)
df["Gen"] = df["G"].map({"M":1, "F":0})
```

**Explanation:**

`apply` runs a function on every row. `map` uses a dictionary to replace values.

**Interview Point:**

`apply` is essentially a slow `for` loop; avoid it if vectorized math works.

### Value Counts & Duplicates

**What is it?**

Finding unique frequencies.

**Why?**

Crucial for Exploratory Data Analysis.

```python
counts = df["City"].value_counts()
df = df.drop_duplicates()
```

**Explanation:**

`value_counts` counts occurrences of each city. `drop_duplicates` removes duplicate rows.

**Interview Point:**

`value_counts` automatically sorts by highest frequency.

---

## SQL → PANDAS CONNECTION
| SQL | Pandas |
|---|---|
| SELECT col | `df["col"]` |
| WHERE | `df[df["col"] == val]` |
| GROUP BY | `groupby()` |
| ORDER BY | `sort_values()` |
| JOIN | `merge()` |
| COUNT / DISTINCT | `value_counts()` / `nunique()` |

---

## INTERVIEW CHEAT SHEET

### Top 10 Must-Know Questions
1. **NumPy vs Lists:** NumPy is faster, C-based, uses less memory, and supports vectorization.
2. **Vectorization:** Doing math on entire arrays instantly without slow Python loops.
3. **Broadcasting:** NumPy implicitly stretches differently shaped arrays to do math.
4. **`loc` vs `iloc`:** `loc` selects by name/label. `iloc` selects by integer position.
5. **Handling Missing Data:** Find with `isnull()`, drop with `dropna()`, or impute with `fillna()`.
6. **`merge` vs `concat`:** `merge` joins on keys (SQL JOIN). `concat` stacks horizontally/vertically.
7. **`groupby`:** Groups data by category and applies aggregation (like mean or sum).
8. **`apply` vs `map`:** `map` uses a dictionary to swap values. `apply` runs a custom function.
9. **`view` vs `copy`:** View shares memory with original; copy duplicates the data safely.
10. **`axis=0` vs `axis=1`:** `axis=0` operates downward across rows; `axis=1` operates across cols.
