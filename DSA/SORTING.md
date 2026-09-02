# Sorting Algorithms — Complete Notes

> **For:** College Students | Placement Preparation | Interview Revision  
> **Goal:** Understand every sorting algorithm clearly, quickly, and confidently.

---

# Table of Contents

| # | Algorithm | Category |
|---|-----------|----------|
| 1 | [Selection Sort](#1-selection-sort) | Basic |
| 2 | [Bubble Sort](#2-bubble-sort) | Basic |
| 3 | [Insertion Sort](#3-insertion-sort) | Basic |
| 4 | [Merge Sort](#4-merge-sort) | Efficient |
| 5 | [Quick Sort](#5-quick-sort) | Efficient |
| 6 | [Heap Sort](#6-heap-sort) | Efficient |
| 7 | [Counting Sort](#7-counting-sort) | Non-Comparison |
| 8 | [Radix Sort](#8-radix-sort) | Non-Comparison |
| 9 | [Bucket Sort](#9-bucket-sort) | Non-Comparison |

---

---

# 1. Selection Sort

### Definition

Selection Sort repeatedly finds the **smallest element** from the unsorted portion of the array and swaps it into its correct position at the beginning of the unsorted portion.

It works by dividing the array into two parts: a **sorted left part** (grows one element at a time) and an **unsorted right part** (shrinks one element at a time).

---

### Main Idea

> **Find minimum → Swap with current position → Move boundary right**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

**Pass 1:** Find minimum in the entire array.

```text
[64, 25, 12, 22, 11]
                  ↑
              min = 11  (index 4)

Swap arr[0] and arr[4]:

[11, 25, 12, 22, 64]
 ↑
sorted
```

**Pass 2:** Find minimum in `[25, 12, 22, 64]`.

```text
[11 | 25, 12, 22, 64]
         ↑
     min = 12  (index 2)

Swap arr[1] and arr[2]:

[11, 12 | 25, 22, 64]
```

**Pass 3:** Find minimum in `[25, 22, 64]`.

```text
[11, 12 | 25, 22, 64]
              ↑
          min = 22  (index 3)

Swap arr[2] and arr[3]:

[11, 12, 22 | 25, 64]
```

**Pass 4:** Find minimum in `[25, 64]`.

```text
[11, 12, 22 | 25, 64]
              ↑
          min = 25  (index 3, already in position)

No swap needed.

[11, 12, 22, 25 | 64]
```

**Done!** Only one element remains → already sorted.

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
for i = 0 to n-2:
    minIndex = i

    for j = i+1 to n-1:
        if arr[j] < arr[minIndex]:
            minIndex = j

    swap(arr[i], arr[minIndex])
```

---

### Python

```python
def selection_sort(arr):
    n = len(arr)

    for i in range(n - 1):
        min_index = i

        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j

        arr[i], arr[min_index] = arr[min_index], arr[i]

    return arr
```

---

### Complexity

* **Best:** `O(n²)`
* **Average:** `O(n²)`
* **Worst:** `O(n²)`
* **Space:** `O(1)`
* **Stable:** ❌
* **In-place:** ✅

> Selection Sort always makes the same number of comparisons regardless of input — it's never faster than O(n²).

---

---

# 2. Bubble Sort

### Definition

Bubble Sort repeatedly compares **adjacent elements** and swaps them if they are in the wrong order. After each full pass, the **largest unsorted element "bubbles up"** to its correct position at the end.

It continues making passes until no more swaps are needed.

---

### Main Idea

> **Compare adjacent elements → Swap if out of order → Largest bubbles to the right**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

**Pass 1:** Compare and swap adjacent pairs. The largest element moves to the end.

```text
[64, 25, 12, 22, 11]

Step: Compare 64 and 25 → 64 > 25 → Swap
[25, 64, 12, 22, 11]

Step: Compare 64 and 12 → 64 > 12 → Swap
[25, 12, 64, 22, 11]

Step: Compare 64 and 22 → 64 > 22 → Swap
[25, 12, 22, 64, 11]

Step: Compare 64 and 11 → 64 > 11 → Swap
[25, 12, 22, 11, 64]
                  ↑
              64 is sorted
```

**Pass 2:** Work on `[25, 12, 22, 11]`. Second-largest bubbles to position.

```text
[25, 12, 22, 11 | 64]

Compare 25, 12 → Swap → [12, 25, 22, 11 | 64]
Compare 25, 22 → Swap → [12, 22, 25, 11 | 64]
Compare 25, 11 → Swap → [12, 22, 11, 25 | 64]
                              ↑
                          25 is sorted
```

**Pass 3:** Work on `[12, 22, 11]`.

```text
[12, 22, 11 | 25, 64]

Compare 12, 22 → No swap → [12, 22, 11 | 25, 64]
Compare 22, 11 → Swap    → [12, 11, 22 | 25, 64]
                                    ↑
                                22 is sorted
```

**Pass 4:** Work on `[12, 11]`.

```text
[12, 11 | 22, 25, 64]

Compare 12, 11 → Swap → [11, 12 | 22, 25, 64]
```

**Done!** All elements are sorted.

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
for i = 0 to n-2:
    swapped = false

    for j = 0 to n-2-i:
        if arr[j] > arr[j+1]:
            swap(arr[j], arr[j+1])
            swapped = true

    if swapped == false:
        break        ← already sorted, stop early
```

---

### Python

```python
def bubble_sort(arr):
    n = len(arr)

    for i in range(n - 1):
        swapped = False

        for j in range(n - 1 - i):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True

        if not swapped:
            break   # Array is already sorted

    return arr
```

---

### Complexity

* **Best:** `O(n)` ← when array is already sorted (with `swapped` optimization)
* **Average:** `O(n²)`
* **Worst:** `O(n²)`
* **Space:** `O(1)`
* **Stable:** ✅
* **In-place:** ✅

> The `swapped` flag is a key optimization — without it, best case is also O(n²).

---

---

# 3. Insertion Sort

### Definition

Insertion Sort builds the sorted array one element at a time. It **picks each element** from the unsorted portion and **inserts it into its correct position** in the already-sorted portion by shifting larger elements to the right.

It works similarly to how you sort a hand of playing cards.

---

### Main Idea

> **Pick element → Shift larger elements right → Insert at correct position**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

Start: First element `[64]` is trivially sorted.

**Pass 1:** Pick `25` (key). Compare with sorted portion `[64]`.

```text
[64 | 25, 12, 22, 11]
      ↑
    key = 25

25 < 64 → shift 64 right → insert 25

[25, 64 | 12, 22, 11]
```

**Pass 2:** Pick `12` (key). Compare with sorted portion `[25, 64]`.

```text
[25, 64 | 12, 22, 11]
          ↑
        key = 12

12 < 64 → shift 64 right
12 < 25 → shift 25 right
insert 12 at beginning

[12, 25, 64 | 22, 11]
```

**Pass 3:** Pick `22` (key). Compare with sorted portion `[12, 25, 64]`.

```text
[12, 25, 64 | 22, 11]
             ↑
           key = 22

22 < 64 → shift 64 right
22 < 25 → shift 25 right
22 > 12 → stop, insert 22

[12, 22, 25, 64 | 11]
```

**Pass 4:** Pick `11` (key). Compare with sorted portion `[12, 22, 25, 64]`.

```text
[12, 22, 25, 64 | 11]
                 ↑
               key = 11

11 < 64 → shift right
11 < 25 → shift right
11 < 22 → shift right
11 < 12 → shift right
insert 11 at beginning

[11, 12, 22, 25, 64]
```

**Done!**

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
for i = 1 to n-1:
    key = arr[i]
    j = i - 1

    while j >= 0 and arr[j] > key:
        arr[j+1] = arr[j]    ← shift element right
        j = j - 1

    arr[j+1] = key           ← insert key at correct position
```

---

### Python

```python
def insertion_sort(arr):
    n = len(arr)

    for i in range(1, n):
        key = arr[i]
        j = i - 1

        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]   # shift element right
            j -= 1

        arr[j + 1] = key          # insert key at correct position

    return arr
```

---

### Complexity

* **Best:** `O(n)` ← when array is already sorted
* **Average:** `O(n²)`
* **Worst:** `O(n²)`
* **Space:** `O(1)`
* **Stable:** ✅
* **In-place:** ✅

> Insertion Sort is efficient for **small arrays** and **nearly sorted arrays**. Used internally by Python's `timsort`.

---

---

# 4. Merge Sort

### Definition

Merge Sort is a **divide and conquer** algorithm. It divides the array into two halves, **recursively sorts each half**, and then **merges** the two sorted halves back into one sorted array.

It keeps dividing until each sub-array has only one element (a single element is always sorted), then merges them back up.

---

### Main Idea

> **Divide array in half → Recursively sort both halves → Merge sorted halves**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

**Phase 1 — Divide:**

```text
[64, 25, 12, 22, 11]
           ↓
    [64, 25, 12]        [22, 11]
           ↓                 ↓
   [64, 25]  [12]       [22]  [11]
       ↓
   [64]  [25]
```

**Phase 2 — Merge (bottom up):**

```text
[64] + [25]  →  merge  →  [25, 64]

[25, 64] + [12]  →  merge  →  [12, 25, 64]

[22] + [11]  →  merge  →  [11, 22]

[12, 25, 64] + [11, 22]  →  merge  →  [11, 12, 22, 25, 64]
```

**How merging works (example: merging `[25, 64]` and `[12]`):**

```text
Left:  [25, 64]
Right: [12]

Compare 25 vs 12 → 12 is smaller → pick 12
Result so far: [12]

Right is empty → take rest of left
Result: [12, 25, 64]
```

**Final merge (merging `[12, 25, 64]` and `[11, 22]`):**

```text
Left:  [12, 25, 64]
Right: [11, 22]

Compare 12 vs 11 → pick 11  →  [11]
Compare 12 vs 22 → pick 12  →  [11, 12]
Compare 25 vs 22 → pick 22  →  [11, 12, 22]
Right is empty → take 25, 64
Result: [11, 12, 22, 25, 64]
```

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
function mergeSort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) / 2
    left = mergeSort(arr[0..mid-1])
    right = mergeSort(arr[mid..n-1])

    return merge(left, right)

function merge(left, right):
    result = []
    i = 0, j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i++
        else:
            result.append(right[j])
            j++

    append remaining elements of left and right to result
    return result
```

---

### Python

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])

    return merge(left, right)

def merge(left, right):
    result = []
    i = 0
    j = 0

    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1

    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

---

### Complexity

* **Best:** `O(n log n)`
* **Average:** `O(n log n)`
* **Worst:** `O(n log n)`
* **Space:** `O(n)` ← extra space for temporary arrays
* **Stable:** ✅
* **In-place:** ❌

> Merge Sort is the best choice when **stability is required** or when sorting **linked lists**.

---

---

# 5. Quick Sort

### Definition

Quick Sort is a **divide and conquer** algorithm. It selects a **pivot** element and **partitions** the array so that all elements smaller than the pivot go to the left, and all elements larger go to the right. It then recursively sorts the left and right parts.

---

### Main Idea

> **Choose pivot → Partition (smaller left, larger right) → Recursively sort both sides**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

**Key concepts before we start:**
- **Pivot:** The element chosen to partition the array. Here we pick the **last element** as the pivot.
- **Partition:** Rearrange array so elements < pivot are on the left, elements > pivot on the right. Pivot ends up in its final correct position.

---

**Step 1:** Full array. Pivot = `11` (last element).

```text
[64, 25, 12, 22, 11]
                  ↑
              pivot = 11

Partition: move elements < 11 to left of pivot.

No elements are less than 11 → pivot 11 moves to index 0.

[11 | 64, 25, 12, 22]
 ↑
 11 is in its correct position
```

**Step 2:** Recursively sort right part `[64, 25, 12, 22]`. Pivot = `22`.

```text
[64, 25, 12, 22]
             ↑
         pivot = 22

Partition:
  12 < 22 → goes left
  64 > 22 → goes right
  25 > 22 → goes right

[12, 22, 64, 25]
      ↑
  22 is in correct position
```

**Step 3:** Recursively sort `[12]` → already sorted. Sort `[64, 25]`. Pivot = `25`.

```text
[64, 25]
      ↑
  pivot = 25

Partition:
  64 > 25 → goes right

[25, 64]
  ↑
 25 is in correct position
```

**Combine everything:**

```text
[11, 12, 22, 25, 64]
```

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
function quickSort(arr, low, high):
    if low < high:
        pivotIndex = partition(arr, low, high)
        quickSort(arr, low, pivotIndex - 1)    ← sort left
        quickSort(arr, pivotIndex + 1, high)   ← sort right

function partition(arr, low, high):
    pivot = arr[high]
    i = low - 1    ← boundary of smaller elements

    for j = low to high - 1:
        if arr[j] <= pivot:
            i++
            swap(arr[i], arr[j])

    swap(arr[i+1], arr[high])   ← place pivot in correct position
    return i + 1
```

---

### Python

```python
def quick_sort(arr, low, high):
    if low < high:
        pivot_index = partition(arr, low, high)
        quick_sort(arr, low, pivot_index - 1)   # sort left
        quick_sort(arr, pivot_index + 1, high)  # sort right

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1  # boundary of smaller elements

    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]  # place pivot
    return i + 1

# Usage:
arr = [64, 25, 12, 22, 11]
quick_sort(arr, 0, len(arr) - 1)
```

---

### Complexity

* **Best:** `O(n log n)` ← pivot divides array evenly
* **Average:** `O(n log n)`
* **Worst:** `O(n²)` ← pivot is always the smallest or largest (e.g., sorted array)
* **Space:** `O(log n)` ← recursion call stack
* **Stable:** ❌
* **In-place:** ✅

> Worst case is avoided in practice by using **random pivot selection** or the **median-of-three** strategy.

---

---

# 6. Heap Sort

### Definition

Heap Sort uses a special tree-based data structure called a **Heap** to sort elements. It first builds a **Max Heap** (where the largest element is at the top), then repeatedly moves the maximum element to the end of the array and fixes the remaining heap.

---

### Main Idea

> **Build Max Heap → Move maximum to end → Heapify remaining → Repeat**

---

### Example

```text
[64, 25, 12, 22, 11]
```

---

### Step-by-step

**Key concepts:**
- **Heap:** A complete binary tree where each parent is greater than or equal to its children (Max Heap).
- **Array as tree:** For index `i`, left child = `2i+1`, right child = `2i+2`, parent = `(i-1)/2`.
- **Heapify:** Fix the heap property when the root is removed.

---

**Phase 1 — Build Max Heap from `[64, 25, 12, 22, 11]`:**

```text
Array:  [64, 25, 12, 22, 11]

As tree:
         64
        /   \
      25     12
     /  \
   22    11

Start heapify from last non-leaf (index 1):
- Node 25 has children 22 and 11. 25 > 22 > 11. No swap needed.

Heapify index 0 (root = 64):
- 64 > 25 and 64 > 12. No swap needed.

Max Heap: [64, 25, 12, 22, 11]
```

**Phase 2 — Extract max and heapify:**

**Iteration 1:** Swap root (64) with last element (11).

```text
Swap arr[0] and arr[4]:
[11, 25, 12, 22 | 64]
                   ↑
               64 sorted

Heapify [11, 25, 12, 22]:
- 11 vs children 25 and 12 → 25 is largest → swap 11 and 25
[25, 11, 12, 22 | 64]
- 11 vs children 22 → 22 > 11 → swap 11 and 22
[25, 22, 12, 11 | 64]

Max Heap restored.
```

**Iteration 2:** Swap root (25) with last unsorted (11).

```text
Swap arr[0] and arr[3]:
[11, 22, 12 | 25, 64]

Heapify [11, 22, 12]:
- 11 vs 22 and 12 → 22 is largest → swap 11 and 22
[22, 11, 12 | 25, 64]
```

**Iteration 3:** Swap root (22) with last unsorted (12).

```text
Swap arr[0] and arr[2]:
[12, 11 | 22, 25, 64]

Heapify [12, 11]:
- 12 > 11. No swap needed.
```

**Iteration 4:** Swap root (12) with last unsorted (11).

```text
Swap arr[0] and arr[1]:
[11 | 12, 22, 25, 64]

Single element left → done.
```

---

### Final

```text
[11, 12, 22, 25, 64]
```

---

### Algorithm

```text
function heapSort(arr):
    n = len(arr)

    // Step 1: Build Max Heap
    for i = n/2 - 1 down to 0:
        heapify(arr, n, i)

    // Step 2: Extract elements one by one
    for i = n-1 down to 1:
        swap(arr[0], arr[i])       ← move max to end
        heapify(arr, i, 0)         ← fix heap for remaining elements

function heapify(arr, n, i):
    largest = i
    left = 2*i + 1
    right = 2*i + 2

    if left < n and arr[left] > arr[largest]:
        largest = left
    if right < n and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        swap(arr[i], arr[largest])
        heapify(arr, n, largest)    ← fix the affected subtree
```

---

### Python

```python
def heap_sort(arr):
    n = len(arr)

    # Step 1: Build Max Heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Step 2: Extract elements from heap one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]  # move max to end
        heapify(arr, i, 0)               # fix remaining heap

    return arr

def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] > arr[largest]:
        largest = left
    if right < n and arr[right] > arr[largest]:
        largest = right

    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)  # fix the affected subtree
```

---

### Complexity

* **Best:** `O(n log n)`
* **Average:** `O(n log n)`
* **Worst:** `O(n log n)`
* **Space:** `O(1)` ← sorts in-place (O(log n) for recursion stack)
* **Stable:** ❌
* **In-place:** ✅

> Heap Sort guarantees O(n log n) in ALL cases — no worst case like Quick Sort.

---

---

# 7. Counting Sort

### Definition

Counting Sort is a **non-comparison** sorting algorithm. Instead of comparing elements, it **counts how many times each value appears** and uses those counts to place elements directly into the correct positions in the output array.

It works best when elements are **integers within a known, limited range**.

---

### Main Idea

> **Count occurrences of each value → Use counts to place elements in correct positions**

---

### Example

```text
[4, 2, 2, 8, 3, 3, 1]
```

*(Using this smaller example because Counting Sort works best with limited integer ranges.)*

---

### Step-by-step

**Step 1:** Find the maximum value to know the count array size.

```text
arr = [4, 2, 2, 8, 3, 3, 1]
max value = 8
→ Create count array of size 9 (indices 0 to 8)
```

**Step 2:** Count occurrences of each element.

```text
arr = [4, 2, 2, 8, 3, 3, 1]

index:  0  1  2  3  4  5  6  7  8
count: [0, 1, 2, 2, 1, 0, 0, 0, 1]

Explanation:
  1 appears 1 time  → count[1] = 1
  2 appears 2 times → count[2] = 2
  3 appears 2 times → count[3] = 2
  4 appears 1 time  → count[4] = 1
  8 appears 1 time  → count[8] = 1
```

**Step 3:** Convert count array to cumulative (prefix sum). This tells us the final position of each element.

```text
index:  0  1  2  3  4  5  6  7  8
count: [0, 1, 3, 5, 6, 6, 6, 6, 7]

(Each index now holds: count[i] = count[i] + count[i-1])
count[i] = number of elements ≤ i
```

**Step 4:** Build output array. Traverse original array from right to left (for stability).

```text
Process arr right to left: [4, 2, 2, 8, 3, 3, 1]

  1 → count[1] = 1 → place at index 0 → count[1] becomes 0
  3 → count[3] = 5 → place at index 4 → count[3] becomes 4
  3 → count[3] = 4 → place at index 3 → count[3] becomes 3
  8 → count[8] = 7 → place at index 6 → count[8] becomes 6
  2 → count[2] = 3 → place at index 2 → count[2] becomes 2
  2 → count[2] = 2 → place at index 1 → count[2] becomes 1
  4 → count[4] = 6 → place at index 5 → count[4] becomes 5

output = [1, 2, 2, 3, 3, 4, 8]
```

---

### Final

```text
[1, 2, 2, 3, 3, 4, 8]
```

---

### Algorithm

```text
function countingSort(arr):
    max_val = max(arr)
    count = array of zeros, size = max_val + 1

    // Step 1: Count occurrences
    for each x in arr:
        count[x]++

    // Step 2: Cumulative sum
    for i = 1 to max_val:
        count[i] += count[i-1]

    // Step 3: Build output (traverse right to left for stability)
    output = array of size n
    for i = n-1 down to 0:
        output[count[arr[i]] - 1] = arr[i]
        count[arr[i]]--

    return output
```

---

### Python

```python
def counting_sort(arr):
    if not arr:
        return arr

    max_val = max(arr)
    count = [0] * (max_val + 1)

    # Step 1: Count occurrences
    for x in arr:
        count[x] += 1

    # Step 2: Cumulative sum
    for i in range(1, max_val + 1):
        count[i] += count[i - 1]

    # Step 3: Build output (right to left for stability)
    output = [0] * len(arr)
    for i in range(len(arr) - 1, -1, -1):
        output[count[arr[i]] - 1] = arr[i]
        count[arr[i]] -= 1

    return output
```

---

### Complexity

* **Best:** `O(n + k)` ← where `k` is the range of values
* **Average:** `O(n + k)`
* **Worst:** `O(n + k)`
* **Space:** `O(k)` ← count array of size k
* **Stable:** ✅
* **In-place:** ❌

> Counting Sort is extremely fast when `k` (value range) is small. It becomes inefficient when `k` is very large (e.g., sorting values from 0 to 1,000,000).

---

---

# 8. Radix Sort

### Definition

Radix Sort is a **non-comparison** sorting algorithm that sorts numbers **digit by digit**, starting from the **least significant digit (ones place)** to the **most significant digit (highest place)**. It uses a stable sort (typically Counting Sort) to sort by each digit.

---

### Main Idea

> **Sort by ones digit → Sort by tens digit → Sort by hundreds digit → ... → Array is sorted**

---

### Example

```text
[170, 45, 75, 90, 802, 24, 2, 66]
```

*(Using numbers with varying digits to best demonstrate Radix Sort.)*

---

### Step-by-step

**Overview:** Process digits from right (ones) to left (hundreds).

---

**Pass 1 — Sort by Ones digit:**

```text
Numbers:  [170, 45, 75, 90, 802, 24, 2, 66]
Ones:        0   5   5   0    2   4  2   6

Bucket 0: [170, 90]
Bucket 2: [802, 2]
Bucket 4: [24]
Bucket 5: [45, 75]
Bucket 6: [66]

Result after Pass 1:
[170, 90, 802, 2, 24, 45, 75, 66]
```

**Pass 2 — Sort by Tens digit:**

```text
Numbers:  [170, 90, 802, 2, 24, 45, 75, 66]
Tens:        7   9    0  0   2   4   7   6

Bucket 0: [802, 2]
Bucket 2: [24]
Bucket 4: [45]
Bucket 6: [66]
Bucket 7: [170, 75]
Bucket 9: [90]

Result after Pass 2:
[802, 2, 24, 45, 66, 170, 75, 90]
```

**Pass 3 — Sort by Hundreds digit:**

```text
Numbers:  [802, 2, 24, 45, 66, 170, 75, 90]
Hundreds:    8  0   0   0   0    1   0   0

Bucket 0: [2, 24, 45, 66, 75, 90]
Bucket 1: [170]
Bucket 8: [802]

Result after Pass 3:
[2, 24, 45, 66, 75, 90, 170, 802]
```

**Done!** ✅

---

### Final

```text
[2, 24, 45, 66, 75, 90, 170, 802]
```

---

### Algorithm

```text
function radixSort(arr):
    max_val = max(arr)
    place = 1    ← start with ones digit

    while max_val / place >= 1:
        countingSortByDigit(arr, place)
        place = place * 10    ← move to next digit

function countingSortByDigit(arr, place):
    // Sort arr based on digit at 'place' position
    // Use stable Counting Sort on (arr[i] / place) % 10
    count = array of zeros, size = 10

    for each x in arr:
        digit = (x / place) % 10
        count[digit]++

    cumulative sum of count

    build output using count (right to left for stability)
    copy output back to arr
```

---

### Python

```python
def radix_sort(arr):
    max_val = max(arr)
    place = 1  # start with ones digit

    while max_val // place >= 1:
        counting_sort_by_digit(arr, place)
        place *= 10  # move to next digit

    return arr

def counting_sort_by_digit(arr, place):
    n = len(arr)
    output = [0] * n
    count = [0] * 10  # digits 0-9

    # Count occurrences of each digit
    for x in arr:
        digit = (x // place) % 10
        count[digit] += 1

    # Cumulative sum
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Build output (right to left for stability)
    for i in range(n - 1, -1, -1):
        digit = (arr[i] // place) % 10
        output[count[digit] - 1] = arr[i]
        count[digit] -= 1

    # Copy back
    for i in range(n):
        arr[i] = output[i]
```

---

### Complexity

* **Best:** `O(nk)` ← where `k` is the number of digits
* **Average:** `O(nk)`
* **Worst:** `O(nk)`
* **Space:** `O(n + b)` ← `b` is the base (10 for decimal)
* **Stable:** ✅ ← because it uses stable Counting Sort internally
* **In-place:** ❌

> Radix Sort is very efficient for sorting **large numbers of integers** with a bounded number of digits. It outperforms comparison sorts when `k` is small relative to `n`.

---

---

# 9. Bucket Sort

### Definition

Bucket Sort distributes elements into several **buckets** (ranges), sorts each bucket individually (using any sorting algorithm, usually Insertion Sort), and then **combines all buckets** to get the sorted array.

It works best when input is **uniformly distributed** over a range (e.g., floating-point numbers between 0 and 1).

---

### Main Idea

> **Create buckets → Distribute elements into buckets → Sort each bucket → Combine**

---

### Example

```text
[0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]
```

*(Using floating-point values in [0, 1) to best demonstrate Bucket Sort.)*

---

### Step-by-step

**Step 1:** Create `n = 10` buckets for range [0, 1). Each bucket covers range of 0.1.

```text
Bucket 0: [0.0, 0.1)
Bucket 1: [0.1, 0.2)
Bucket 2: [0.2, 0.3)
Bucket 3: [0.3, 0.4)
...
Bucket 9: [0.9, 1.0)
```

**Step 2:** Distribute elements into their buckets.

```text
arr = [0.78, 0.17, 0.39, 0.26, 0.72, 0.94, 0.21, 0.12, 0.23, 0.68]

  0.78 → int(0.78 * 10) = 7 → Bucket 7
  0.17 → int(0.17 * 10) = 1 → Bucket 1
  0.39 → int(0.39 * 10) = 3 → Bucket 3
  0.26 → int(0.26 * 10) = 2 → Bucket 2
  0.72 → int(0.72 * 10) = 7 → Bucket 7
  0.94 → int(0.94 * 10) = 9 → Bucket 9
  0.21 → int(0.21 * 10) = 2 → Bucket 2
  0.12 → int(0.12 * 10) = 1 → Bucket 1
  0.23 → int(0.23 * 10) = 2 → Bucket 2
  0.68 → int(0.68 * 10) = 6 → Bucket 6

Buckets:
  Bucket 1: [0.17, 0.12]
  Bucket 2: [0.26, 0.21, 0.23]
  Bucket 3: [0.39]
  Bucket 6: [0.68]
  Bucket 7: [0.78, 0.72]
  Bucket 9: [0.94]
```

**Step 3:** Sort each bucket individually.

```text
  Bucket 1: [0.12, 0.17]    ← sorted
  Bucket 2: [0.21, 0.23, 0.26]  ← sorted
  Bucket 3: [0.39]
  Bucket 6: [0.68]
  Bucket 7: [0.72, 0.78]    ← sorted
  Bucket 9: [0.94]
```

**Step 4:** Combine all buckets in order.

```text
Bucket 0: (empty)
Bucket 1: [0.12, 0.17]
Bucket 2: [0.21, 0.23, 0.26]
Bucket 3: [0.39]
...
Bucket 6: [0.68]
Bucket 7: [0.72, 0.78]
...
Bucket 9: [0.94]

Combined: [0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]
```

---

### Final

```text
[0.12, 0.17, 0.21, 0.23, 0.26, 0.39, 0.68, 0.72, 0.78, 0.94]
```

---

### Algorithm

```text
function bucketSort(arr):
    n = len(arr)
    buckets = n empty lists

    // Step 1: Distribute into buckets
    for each x in arr:
        index = floor(x * n)
        buckets[index].append(x)

    // Step 2: Sort each bucket
    for each bucket in buckets:
        sort(bucket)    ← typically Insertion Sort

    // Step 3: Combine
    result = []
    for each bucket in buckets:
        result.extend(bucket)

    return result
```

---

### Python

```python
def bucket_sort(arr):
    n = len(arr)
    buckets = [[] for _ in range(n)]

    # Step 1: Distribute into buckets
    for x in arr:
        index = int(x * n)
        if index == n:
            index = n - 1  # handle edge case for x = 1.0
        buckets[index].append(x)

    # Step 2: Sort each bucket using insertion sort
    for bucket in buckets:
        bucket.sort()  # Python's built-in sort (or use insertion_sort)

    # Step 3: Combine all buckets
    result = []
    for bucket in buckets:
        result.extend(bucket)

    return result
```

---

### Complexity

* **Best:** `O(n + k)` ← when elements are uniformly distributed
* **Average:** `O(n + k)`
* **Worst:** `O(n²)` ← when all elements fall into one bucket
* **Space:** `O(n + k)` ← extra space for buckets
* **Stable:** ✅ ← if the bucket's internal sort is stable
* **In-place:** ❌

> Bucket Sort achieves near-linear time when data is **uniformly distributed**. Poor pivot/bucket selection can degrade performance significantly.

---

---

# Quick Comparison

| Algorithm | Best | Average | Worst | Space | Stable | In-place |
|-----------|------|---------|-------|-------|--------|----------|
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ | ✅ |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | ✅ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ | ✅ |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ | ❌ |
| Radix Sort | O(nk) | O(nk) | O(nk) | O(n+b) | ✅ | ❌ |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n+k) | ✅ | ❌ |

> `k` = range of values, `b` = base (10 for decimal)

---

# Easy Memory Tricks

* **Selection:** Find minimum → Swap with current position
* **Bubble:** Compare neighbors → Largest bubbles right → Repeat
* **Insertion:** Pick element → Shift larger ones right → Insert
* **Merge:** Divide in half → Sort both halves → Merge sorted halves
* **Quick:** Choose pivot → Partition smaller/larger → Recurse
* **Heap:** Build max heap → Move max to end → Heapify → Repeat
* **Counting:** Count each value → Cumulative sum → Place in output
* **Radix:** Sort digit by digit → Ones → Tens → Hundreds
* **Bucket:** Put in buckets → Sort buckets → Combine

---

# When to Use Which Sort?

| Situation | Best Choice |
|-----------|------------|
| Small array (n < 20) | Insertion Sort |
| Almost sorted array | Insertion Sort / Bubble Sort |
| General purpose, no stability needed | Quick Sort |
| Need guaranteed O(n log n) | Merge Sort or Heap Sort |
| Need stable sort | Merge Sort |
| Sorting integers in a small range | Counting Sort |
| Sorting large integers digit by digit | Radix Sort |
| Uniformly distributed floating-point data | Bucket Sort |
| Memory is very limited | Heap Sort |

---

*End of Sorting Algorithms Notes*
