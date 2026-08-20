# Two Pointers — Complete DSA Interview Guide

> **Goal:** After reading this, you should be able to look at any new problem and answer:
> *"Is this a Two Pointer problem? Which pattern? What do the pointers represent? Why can I safely move each pointer?"*

---

## Table of Contents

- [Pattern 1 — Converging Pointers](#pattern-1--converging-pointers)
- [Pattern 2 — Fast & Slow Pointers](#pattern-2--fast--slow-pointers)
- [Pattern 3 — Fixed Separation Pointers](#pattern-3--fixed-separation-pointers)
- [Pattern 4 — In-place Array Modification](#pattern-4--in-place-array-modification)
- [Pattern 5 — String Comparison with Backspaces](#pattern-5--string-comparison-with-backspaces)
- [Pattern 6 — Expanding From Center](#pattern-6--expanding-from-center)
- [Pattern 7 — String Reversal](#pattern-7--string-reversal)
- [Two Pointer Pattern Cheat Sheet](#two-pointer-pattern-cheat-sheet)
- [Most Important Problems to Master First](#most-important-problems-to-master-first)

---

# Pattern 1 — Converging Pointers

> Both pointers start at opposite ends of a sorted (or sortable) array and move toward each other. The key insight: at every step you can eliminate an entire half of the remaining search space.

---

## Problem 1 — Two Sum

**LeetCode #1 | Difficulty: Easy | Pattern: Converging (+ Hash Map variant)**

Given an array `nums` and a `target`, return the **indices** of the two numbers that add up to `target`. You may assume exactly one solution exists. You cannot use the same element twice.

---

### 2. How to Recognize This Pattern

**Clues:**
- Find a **pair** that satisfies a condition (sum = target)
- Return **indices** (original positions matter)
- Unsorted input

**Important distinction:**
- The classic unsorted Two Sum is best solved with a **Hash Map** (O(n) time, O(n) space).
- Two Pointers requires a **sorted array**. Sorting loses original indices unless you save them.
- In an interview: if the array is **unsorted** and you need **indices** → use Hash Map. If the array is **sorted** and you need **values** → use Two Pointers.

**Pointer direction:** Converging (toward each other)

---

### 3. Brute Force Approach

**Idea:** Check every pair.

**Algorithm:**
1. Use two nested loops, `i` from 0 to n-1, `j` from i+1 to n-1.
2. If `nums[i] + nums[j] == target`, return `[i, j]`.

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        n = len(nums)
        for i in range(n):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []
```

```
Time:  O(n²) — every pair is checked
Space: O(1)
```

**Why inefficient:** For n = 10,000, you check ~50 million pairs. We're ignoring all structure.

---

### 4. Better Approach — Hash Map (Optimal for unsorted input)

**Idea:** For each element, check if `target - nums[i]` has already been seen.

**Algorithm:**
1. Create an empty dictionary `seen = {}`.
2. For each index `i`, compute `complement = target - nums[i]`.
3. If `complement` is in `seen`, return `[seen[complement], i]`.
4. Otherwise, store `seen[nums[i]] = i`.

```python
class Solution:
    def twoSum(self, nums: list[int], target: int) -> list[int]:
        seen = {}  # value -> index
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
        return []
```

```
Time:  O(n) — single pass
Space: O(n) — hash map stores up to n elements
```

---

### 5. Optimal Two Pointer Approach (for sorted input / Two Sum II)

> **Note:** This approach is demonstrated fully in Problem 6 (Two Sum II). The Two Pointer approach for this problem requires sorting, which changes the indices. The Hash Map approach above is preferred for the standard unsorted Two Sum.

**If the array were sorted (or if we only needed values, not indices):**

#### Core Idea
Place one pointer at the leftmost (smallest) element and one at the rightmost (largest). If their sum is too small, we need a larger number → move left pointer right. If too large → move right pointer left.

#### Pointer Meaning
```
left  = index 0 (smallest element)
right = index n-1 (largest element)
```

#### Why Pointer Movement Works
- If `nums[left] + nums[right] < target`: We need a bigger sum. `nums[right]` is already the largest possible. So the only way to increase the sum is to increase the left element → move `left` right.
- If `nums[left] + nums[right] > target`: We need a smaller sum. `nums[left]` is already the smallest possible. The only way to decrease the sum is to decrease the right element → move `right` left.
- If equal: found the answer.

#### Algorithm
1. Sort the array (save original indices if needed).
2. Set `left = 0`, `right = n - 1`.
3. While `left < right`:
   - If sum < target → `left += 1`
   - If sum > target → `right -= 1`
   - If sum == target → return answer

#### Python Code (values only, sorted input assumed)
```python
def twoSumSorted(nums, target):
    left, right = 0, len(nums) - 1
    while left < right:
        s = nums[left] + nums[right]
        if s == target:
            return [left, right]
        elif s < target:
            left += 1
        else:
            right -= 1
    return []
```

#### Dry Run
```
nums = [2, 7, 11, 15], target = 9

left=0, right=3: 2+15=17 > 9 → right=2
left=0, right=2: 2+11=13 > 9 → right=1
left=0, right=1: 2+7=9 == 9 → return [0, 1]
```

```
Time:  O(n) after sorting | O(n log n) including sort
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- The problem asks for a **pair** that meets a condition (sum, product, etc.)
- If input is **unsorted + need indices** → Hash Map is cleaner
- If input is **sorted** → Two Pointers is the natural O(n) approach
- The key phrase is "find two numbers that add up to target"
- If there is no constraint on returning indices and the array can be sorted → Two Pointers

---

## Sum Problems — Progression Overview

Before diving into the next problems, here is how the sum problems build on each other:

```
1. Two Sum (unsorted) → Hash Map O(n)
         ↓
2. Two Sum II (sorted) → Two Pointers O(n)
         ↓
3. 3Sum = Sort + Fix one element + Two Pointers on the rest
         ↓
4. 3Sum Closest = Fix one + Two Pointers + track closest distance
         ↓
5. 3Sum Smaller = Fix one + Two Pointers + count valid pairs
         ↓
6. 4Sum = Sort + Fix two elements + Two Pointers on the rest
```

**Why sorting helps:**
- It creates a monotone structure. Moving left pointer increases the sum. Moving right pointer decreases the sum. This lets you eliminate half the search space at each step instead of checking every pair.

**How duplicates are handled:**
- After sorting, duplicates are adjacent. You skip them by checking `if nums[i] == nums[i-1]: continue` after processing each fixed element.

---

## Problem 2 — Container With Most Water

**LeetCode #11 | Difficulty: Medium | Pattern: Converging**

Given an array `height` of `n` integers where `height[i]` is the height of a vertical line at position `i`, find two lines that together with the x-axis form a container that holds the most water. Return the maximum area.

Area = `min(height[left], height[right]) * (right - left)`

---

### 2. How to Recognize This Pattern

**Clues:**
- Working with an array of heights/values
- Need to maximize area/volume formed by two elements
- Width between them matters (position difference counts)

**Why Two Pointers:**
- The container's width decreases as we move pointers inward. To compensate, we need greater height. We should always try to move the **shorter** side inward (keeping the taller side gives a chance for greater height).

**Pointer direction:** Converging

---

### 3. Brute Force Approach

**Idea:** Try every pair of lines.

```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        n = len(height)
        max_water = 0
        for i in range(n):
            for j in range(i + 1, n):
                water = min(height[i], height[j]) * (j - i)
                max_water = max(max_water, water)
        return max_water
```

```
Time:  O(n²)
Space: O(1)
```

**Why inefficient:** n = 10^5 → 5 billion operations.

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Start with the widest possible container (pointers at both ends). This gives maximum width. Then try to find a taller pair by moving the shorter line inward. The width decreases by 1 each step, so we need the height to increase to possibly improve area.

#### Pointer Meaning
```
left  = left boundary of the container
right = right boundary of the container
```

#### Why Pointer Movement Works
Suppose `height[left] < height[right]`.

The current area = `height[left] * (right - left)`.

If we move `right` inward (right -= 1):
- Width decreases by 1.
- The limiting factor is `height[left]` (the shorter side).
- Even if `height[right-1]` is taller, the area is still limited by `height[left]`.
- So moving `right` inward **cannot** improve the area while `height[left]` remains the bottleneck.
- Therefore we must move `left` inward to have any chance of finding a better area.

Similarly, if `height[right] < height[left]`, move `right` inward.

#### Algorithm
1. Set `left = 0`, `right = n - 1`, `max_water = 0`.
2. While `left < right`:
   - Compute `water = min(height[left], height[right]) * (right - left)`.
   - Update `max_water`.
   - If `height[left] <= height[right]` → `left += 1`
   - Else → `right -= 1`
3. Return `max_water`.

#### Python Code
```python
class Solution:
    def maxArea(self, height: list[int]) -> int:
        left, right = 0, len(height) - 1
        max_water = 0
        while left < right:
            water = min(height[left], height[right]) * (right - left)
            max_water = max(max_water, water)
            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1
        return max_water
```

#### Dry Run
```
height = [1, 8, 6, 2, 5, 4, 8, 3, 7]

left=0(h=1), right=8(h=7): water=1*8=8,  max=8,  h[l]<h[r] → left=1
left=1(h=8), right=8(h=7): water=7*7=49, max=49, h[l]>h[r] → right=7
left=1(h=8), right=7(h=3): water=3*6=18, max=49, h[l]>h[r] → right=6
left=1(h=8), right=6(h=8): water=8*5=40, max=49, h[l]<=h[r]→ left=2
left=2(h=6), right=6(h=8): water=6*4=24, max=49, h[l]<h[r] → left=3
...
Answer: 49
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Two boundaries define a container (area, volume, etc.)
- Width = distance between boundaries; height = minimum of the two sides
- You want to **maximize** an expression involving both pointers
- The "move the shorter side" greedy insight is key
- No sorting needed — position matters

---

## Problem 3 — 3Sum

**LeetCode #15 | Difficulty: Medium | Pattern: Converging**

Given an array `nums`, find all unique triplets `[a, b, c]` such that `a + b + c = 0`. The solution set must not contain duplicate triplets.

---

### 2. How to Recognize This Pattern

**Clues:**
- Find a **triplet** that sums to a target (here, 0)
- All unique combinations (no duplicates)
- Can we reduce to a known sub-problem?

**Why Two Pointers:**
After sorting, fixing one element (`nums[i]`) turns the problem into "Two Sum in the rest of the array = -nums[i]", which is solved optimally with Two Pointers.

**Pointer direction:** Converging (inner two pointers)

---

### 3. Brute Force Approach

**Idea:** Check every triplet.

```python
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        nums.sort()
        result = set()
        n = len(nums)
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    if nums[i] + nums[j] + nums[k] == 0:
                        result.add((nums[i], nums[j], nums[k]))
        return [list(t) for t in result]
```

```
Time:  O(n³)
Space: O(n) for result set
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Sort the array. Fix the first element at index `i`. Now find all pairs in `nums[i+1 ... n-1]` that sum to `-nums[i]`. Use Two Pointers for that sub-problem. Skip duplicates at every level.

#### Pointer Meaning
```
i     = fixed element (iterates from 0 to n-3)
left  = i + 1 (start of the remaining sub-array)
right = n - 1 (end of the array)
```

#### Why Pointer Movement Works
After fixing `nums[i]`, we need `nums[left] + nums[right] == -nums[i]`.
- If sum < target → `left += 1` (need larger left value)
- If sum > target → `right -= 1` (need smaller right value)
- If sum == target → record triplet, then skip duplicates on both sides

**Skipping duplicates:** After sorting, duplicates are adjacent. After recording a triplet, if `nums[left] == nums[left+1]`, advancing `left` by 1 gives the same pair → skip it. Same logic for `right`.

**Pruning:** If `nums[i] > 0`, since the array is sorted, all following elements are also > 0, so no three of them can sum to 0 → break early.

**Early skip:** If `nums[i] == nums[i-1]` (and i > 0), we'd find the same triplets → skip.

#### Algorithm
1. Sort `nums`.
2. For each `i` from 0 to n-3:
   - If `nums[i] > 0`: break (no valid triplet possible).
   - If `i > 0` and `nums[i] == nums[i-1]`: skip duplicate fixed element.
   - Set `left = i+1`, `right = n-1`.
   - While `left < right`:
     - Compute `s = nums[i] + nums[left] + nums[right]`.
     - If `s == 0`: add to result; skip duplicates; `left += 1`, `right -= 1`.
     - If `s < 0`: `left += 1`.
     - If `s > 0`: `right -= 1`.
3. Return result.

#### Python Code
```python
class Solution:
    def threeSum(self, nums: list[int]) -> list[list[int]]:
        nums.sort()
        result = []
        n = len(nums)

        for i in range(n - 2):
            # Pruning: all remaining elements are positive
            if nums[i] > 0:
                break
            # Skip duplicate fixed element
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, n - 1
            while left < right:
                s = nums[i] + nums[left] + nums[right]
                if s == 0:
                    result.append([nums[i], nums[left], nums[right]])
                    # Skip duplicates for left and right
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1
                    left += 1
                    right -= 1
                elif s < 0:
                    left += 1
                else:
                    right -= 1

        return result
```

#### Dry Run
```
nums = [-4, -1, -1, 0, 1, 2]  (after sort)

i=0, nums[i]=-4, left=1, right=5:
  -4 + (-1) + 2 = -3 < 0 → left=2
  -4 + (-1) + 2 = -3 < 0 → left=3
  -4 + 0 + 2 = -2 < 0 → left=4
  -4 + 1 + 2 = -1 < 0 → left=5 → left>=right → stop

i=1, nums[i]=-1, left=2, right=5:
  -1 + (-1) + 2 = 0 → add [-1,-1,2]
  skip dup: nums[2]=-1, nums[3]=0 → no dup on left
  skip dup: nums[5]=2, nums[4]=1 → no dup on right
  left=3, right=4:
  -1 + 0 + 1 = 0 → add [-1,0,1]
  left=4, right=3 → stop

i=2, nums[2]=-1 == nums[1]=-1 → skip

i=3, nums[i]=0, left=4, right=5:
  0 + 1 + 2 = 3 > 0 → right=4 → left>=right → stop

Result: [[-1,-1,2], [-1,0,1]]
```

```
Time:  O(n²) — outer loop O(n) × inner Two Pointers O(n)
       Sorting adds O(n log n) but O(n²) dominates
Space: O(1) extra (excluding output)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Find all **triplets** summing to a target
- Need unique combinations (no duplicates in output)
- "Reduce to Two Sum" insight after sorting
- Duplicate handling is the hardest part — always skip adjacent equal values
- Pruning (`nums[i] > 0 → break`) is expected in optimal solution

---

## Problem 4 — 3Sum Closest

**LeetCode #16 | Difficulty: Medium | Pattern: Converging**

Given `nums` and a `target`, find the sum of three integers that is closest to `target`. There is exactly one closest sum.

---

### 2. How to Recognize This Pattern

**Clues:**
- Triplet sum problem (extends 3Sum)
- Instead of finding exact match, minimize the difference
- "Closest" → track a running best

**Why Two Pointers:** Same as 3Sum — fix one element, use Two Pointers for the inner pair. Instead of checking equality, track the minimum absolute difference.

---

### 3. Brute Force Approach

```python
class Solution:
    def threeSumClosest(self, nums: list[int], target: int) -> int:
        n = len(nums)
        closest = float('inf')
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    s = nums[i] + nums[j] + nums[k]
                    if abs(s - target) < abs(closest - target):
                        closest = s
        return closest
```

```
Time:  O(n³)
Space: O(1)
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Sort, fix one element, use Two Pointers for the inner pair. At each step, update the closest sum if the current sum is nearer to `target`.

#### Pointer Meaning
```
i     = fixed element
left  = i + 1
right = n - 1
```

#### Why Pointer Movement Works
Same as 3Sum:
- Sum < target → we need a bigger sum → `left += 1`
- Sum > target → we need a smaller sum → `right -= 1`
- Sum == target → perfect match, return immediately

We never need to check the "other direction" for the fixed `i`, because moving the other pointer cannot give a closer result given the current fixed element and the monotone structure.

#### Algorithm
1. Sort `nums`.
2. Initialize `closest = nums[0] + nums[1] + nums[2]`.
3. For each `i` from 0 to n-3:
   - Skip duplicate `i` values (optional, doesn't affect correctness, only speed).
   - Set `left = i+1`, `right = n-1`.
   - While `left < right`:
     - Compute `s = nums[i] + nums[left] + nums[right]`.
     - If `abs(s - target) < abs(closest - target)` → update `closest = s`.
     - If `s == target` → return `s`.
     - If `s < target` → `left += 1`.
     - Else → `right -= 1`.
4. Return `closest`.

#### Python Code
```python
class Solution:
    def threeSumClosest(self, nums: list[int], target: int) -> int:
        nums.sort()
        n = len(nums)
        closest = nums[0] + nums[1] + nums[2]

        for i in range(n - 2):
            left, right = i + 1, n - 1
            while left < right:
                s = nums[i] + nums[left] + nums[right]
                if abs(s - target) < abs(closest - target):
                    closest = s
                if s == target:
                    return s
                elif s < target:
                    left += 1
                else:
                    right -= 1

        return closest
```

#### Dry Run
```
nums = [-1, 2, 1, -4], target = 1
After sort: [-4, -1, 1, 2]

i=0 (-4), left=1(-1), right=3(2):
  s = -4-1+2 = -3, |−3−1|=4 < |−2−1|=3? init closest=-2
  Actually: closest starts as -4-1+1=-4
  s=-3, |-3-1|=4 < |-4-1|=5 → closest=-3
  s<1 → left=2

left=2(1), right=3(2):
  s = -4+1+2 = -1, |-1-1|=2 < |-3-1|=4 → closest=-1
  s<1 → left=3 → stop

i=1 (-1), left=2(1), right=3(2):
  s = -1+1+2 = 2, |2-1|=1 < |-1-1|=2 → closest=2
  s>1 → right=2 → left>=right → stop

Answer: 2
```

```
Time:  O(n²)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Triplet problem with "closest" rather than exact match
- "Track running minimum distance" is the extra step compared to 3Sum
- Direct extension of 3Sum — if you know 3Sum, this is immediate
- Early return when exact target is found

---

## Problem 5 — 4Sum

**LeetCode #18 | Difficulty: Medium | Pattern: Converging**

Given `nums` and `target`, find all unique quadruplets `[a, b, c, d]` such that `a + b + c + d == target`.

---

### 2. How to Recognize This Pattern

**Clues:**
- Quadruplet sum → extends 3Sum by one more fixed element
- "All unique quadruplets" → duplicate handling needed at two levels
- Can reduce to "fix two elements + Two Sum on the rest"

---

### 3. Brute Force Approach

```python
class Solution:
    def fourSum(self, nums: list[int], target: int) -> list[list[int]]:
        nums.sort()
        result = set()
        n = len(nums)
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    for l in range(k + 1, n):
                        if nums[i] + nums[j] + nums[k] + nums[l] == target:
                            result.add((nums[i], nums[j], nums[k], nums[l]))
        return [list(t) for t in result]
```

```
Time:  O(n⁴)
Space: O(n)
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Fix two elements with two nested loops (`i` and `j`), then apply Two Pointers for the inner pair. Three levels of duplicate skipping.

**Integer overflow note (Python):** Python handles arbitrarily large integers natively — no overflow risk. In Java/C++, you'd cast to `long` before adding.

#### Pointer Meaning
```
i     = first fixed element
j     = second fixed element (i+1 to n-1)
left  = j + 1
right = n - 1
```

#### Why Pointer Movement Works
After fixing `nums[i]` and `nums[j]`, we need `nums[left] + nums[right] == target - nums[i] - nums[j]`. Same Two Pointer logic as Two Sum II applies.

#### Algorithm
1. Sort `nums`.
2. For `i` from 0 to n-4:
   - Skip duplicate `i`.
   - **Pruning:** If `nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target` → break (smallest sum too big).
   - **Pruning:** If `nums[i] + nums[n-3] + nums[n-2] + nums[n-1] < target` → continue (largest sum too small).
   - For `j` from `i+1` to n-3:
     - Skip duplicate `j`.
     - **Pruning:** Similar min/max checks.
     - Set `left = j+1`, `right = n-1`.
     - While `left < right`: apply Two Pointer logic.
3. Return result.

#### Python Code
```python
class Solution:
    def fourSum(self, nums: list[int], target: int) -> list[list[int]]:
        nums.sort()
        n = len(nums)
        result = []

        for i in range(n - 3):
            # Skip duplicate i
            if i > 0 and nums[i] == nums[i - 1]:
                continue
            # Pruning: smallest possible quad with this i is too large
            if nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target:
                break
            # Pruning: largest possible quad with this i is too small
            if nums[i] + nums[n-3] + nums[n-2] + nums[n-1] < target:
                continue

            for j in range(i + 1, n - 2):
                # Skip duplicate j
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue
                # Pruning
                if nums[i] + nums[j] + nums[j+1] + nums[j+2] > target:
                    break
                if nums[i] + nums[j] + nums[n-2] + nums[n-1] < target:
                    continue

                left, right = j + 1, n - 1
                while left < right:
                    s = nums[i] + nums[j] + nums[left] + nums[right]
                    if s == target:
                        result.append([nums[i], nums[j], nums[left], nums[right]])
                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1
                        left += 1
                        right -= 1
                    elif s < target:
                        left += 1
                    else:
                        right -= 1

        return result
```

#### Dry Run
```
nums = [1, 0, -1, 0, -2, 2], target = 0
After sort: [-2, -1, 0, 0, 1, 2]

i=0(-2), j=1(-1), left=2(0), right=5(2):
  -2-1+0+2=-1<0 → left=3
  -2-1+0+2=-1<0 → left=4
  -2-1+1+2=0 == 0 → add [-2,-1,1,2], left=5 → stop

i=0(-2), j=2(0), left=3(0), right=5(2):
  -2+0+0+2=0 → add [-2,0,0,2], skip dup left: nums[3]=0=nums[4]? no, left=4
  -2+0+1+2=1>0 → right=4 → left>=right → stop

... (continue)
Result: [[-2,-1,1,2], [-2,0,0,2], [-1,0,0,1]]
```

```
Time:  O(n³) — two nested loops × Two Pointers
Space: O(1) extra (excluding output)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Quadruplet sum → generalize from 3Sum (add one more fixed loop)
- kSum pattern: sort + (k-2) nested loops + Two Pointers at the innermost level
- Duplicate skipping at every loop level
- Pruning min/max bounds dramatically improves average case

---

## Problem 6 — Two Sum II - Input Array Is Sorted

**LeetCode #167 | Difficulty: Medium | Pattern: Converging**

Given a **1-indexed** sorted array `numbers`, find two numbers that add up to `target`. Return their 1-based indices. You may not use the same element twice. Exactly one solution exists.

---

### 2. How to Recognize This Pattern

**Clues:**
- Array is **sorted** — this is explicitly stated
- Find a pair summing to a target
- The sorted property makes Two Pointers the obvious optimal approach
- Must use O(1) extra space (the problem hints at this)

---

### 3. Brute Force Approach

```python
class Solution:
    def twoSum(self, numbers: list[int], target: int) -> list[int]:
        n = len(numbers)
        for i in range(n):
            for j in range(i + 1, n):
                if numbers[i] + numbers[j] == target:
                    return [i + 1, j + 1]
        return []
```

```
Time:  O(n²)
Space: O(1)
```

---

### 4. Better Approach — Binary Search

**Idea:** For each element at index `i`, binary search for `target - numbers[i]` in the remaining array.

```python
import bisect

class Solution:
    def twoSum(self, numbers: list[int], target: int) -> list[int]:
        n = len(numbers)
        for i in range(n):
            complement = target - numbers[i]
            j = bisect.bisect_left(numbers, complement, i + 1, n)
            if j < n and numbers[j] == complement:
                return [i + 1, j + 1]
        return []
```

```
Time:  O(n log n)
Space: O(1)
```

---

### 5. Optimal Two Pointer Approach

#### Core Idea
The array is sorted. Place one pointer at the start (smallest) and one at the end (largest). Their sum is either too small, too large, or exactly right. Converge based on comparison.

#### Pointer Meaning
```
left  = index 0 (smallest element)
right = index n-1 (largest element)
```

#### Why Pointer Movement Works
- `sum < target`: Need to increase the sum. `right` is already at the largest possible value. The only way to increase sum is to pick a larger `left` → `left += 1`.
- `sum > target`: Need to decrease the sum. `left` is already at the smallest possible value. The only way to decrease sum is to pick a smaller `right` → `right -= 1`.
- When we move `left`, we permanently discard `numbers[left]` as the left element of the answer. Is this safe? Yes, because we already know pairing `numbers[left]` with every value from `right` down to `left+1` gives a sum ≤ current sum < target. No valid pair exists with this `left`. Similarly for moving `right`.

#### Algorithm
1. Set `left = 0`, `right = n - 1`.
2. While `left < right`:
   - `s = numbers[left] + numbers[right]`
   - If `s == target` → return `[left+1, right+1]`
   - If `s < target` → `left += 1`
   - If `s > target` → `right -= 1`

#### Python Code
```python
class Solution:
    def twoSum(self, numbers: list[int], target: int) -> list[int]:
        left, right = 0, len(numbers) - 1
        while left < right:
            s = numbers[left] + numbers[right]
            if s == target:
                return [left + 1, right + 1]  # 1-indexed
            elif s < target:
                left += 1
            else:
                right -= 1
        return []
```

#### Dry Run
```
numbers = [2, 3, 4, 6, 9], target = 11

left=0(2), right=4(9): 2+9=11 → return [1, 5]
```

```
numbers = [1, 2, 4, 6, 8], target = 10

left=0(1), right=4(8): 1+8=9 < 10 → left=1
left=1(2), right=4(8): 2+8=10 → return [2, 5]
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Array is sorted + find pair = Two Pointers (almost always)
- O(1) space constraint rules out Hash Map
- This is the "canonical" Two Pointer problem — master this and all sum variants follow

---

## Problem 7 — Intersection of Two Arrays

**LeetCode #349 | Difficulty: Easy | Pattern: Converging**

Given two arrays `nums1` and `nums2`, return their intersection. Each element in the result must appear only once.

---

### 2. How to Recognize This Pattern

**Clues:**
- Comparing elements across two arrays
- Finding common elements (intersection)
- Sorted arrays allow simultaneous traversal

**Why Two Pointers:** Sort both arrays. Use two pointers, one per array. Advance the smaller one. When they match, record the element.

---

### 3. Brute Force Approach

```python
class Solution:
    def intersection(self, nums1: list[int], nums2: list[int]) -> list[int]:
        result = set()
        set1 = set(nums1)
        for num in nums2:
            if num in set1:
                result.add(num)
        return list(result)
```

```
Time:  O(n + m)
Space: O(n) for set
```

> **Note:** The set-based approach is actually optimal here too. The Two Pointer approach trades space for a slightly different characteristic — useful when arrays are already sorted or extra memory is not allowed.

---

### 4. Better Approach

**There is no meaningful better intermediate approach.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Sort both arrays. Use pointer `i` for `nums1` and `j` for `nums2`. Advance the pointer pointing to the smaller value. If they match, add to result (but only once).

#### Pointer Meaning
```
i = current index in nums1
j = current index in nums2
```

#### Why Pointer Movement Works
- If `nums1[i] < nums2[j]`: `nums1[i]` cannot match any element in `nums2` from `j` onward (since `nums2` is sorted and all following values ≥ `nums2[j]` > `nums1[i]`) → advance `i`.
- If `nums1[i] > nums2[j]`: Similarly, `nums2[j]` cannot match anything from `i` onward → advance `j`.
- If equal: record the value, advance both. Skip duplicates in both arrays.

#### Python Code
```python
class Solution:
    def intersection(self, nums1: list[int], nums2: list[int]) -> list[int]:
        nums1.sort()
        nums2.sort()
        i, j = 0, 0
        result = []

        while i < len(nums1) and j < len(nums2):
            if nums1[i] == nums2[j]:
                # Avoid duplicates in result
                if not result or result[-1] != nums1[i]:
                    result.append(nums1[i])
                i += 1
                j += 1
            elif nums1[i] < nums2[j]:
                i += 1
            else:
                j += 1

        return result
```

#### Dry Run
```
nums1 = [4, 9, 5], nums2 = [9, 4, 9, 8, 4]
After sort: nums1 = [4, 5, 9], nums2 = [4, 4, 8, 9, 9]

i=0(4), j=0(4): match → add 4, i=1, j=1
i=1(5), j=1(4): 5>4 → j=2
i=1(5), j=2(8): 5<8 → i=2
i=2(9), j=2(8): 9>8 → j=3
i=2(9), j=3(9): match → add 9, i=3, j=4 → stop

Result: [4, 9]
```

```
Time:  O(n log n + m log m) — dominated by sorting
Space: O(1) extra (excluding output)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Find common elements in two arrays
- If you can sort (or they're already sorted) → Two Pointers, O(1) space
- If you cannot sort → Hash Set, O(n) space
- "Result must not contain duplicates" → skip consecutive duplicates after match

---

## Problem 8 — Is Subsequence

**LeetCode #392 | Difficulty: Easy | Pattern: Converging (same direction)**

Given two strings `s` and `t`, return `True` if `s` is a subsequence of `t`. A subsequence means characters of `s` appear in `t` in the same order (not necessarily contiguous).

---

### 2. How to Recognize This Pattern

**Clues:**
- Comparing a short string against a longer one, in order
- "Can I match all characters of s in t, in order?"
- Both pointers move in the **same direction** (left to right), but at different rates

This is technically a "two-pointer on two strings moving in same direction" problem, sometimes called a read/write pointer pattern.

---

### 3. Brute Force Approach

**There is no brute force meaningfully different from optimal here.** A recursive approach is less efficient but conceptually the same.

---

### 4. Better Approach

**The greedy pointer approach is already optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Use one pointer `i` for `s` and one pointer `j` for `t`. Advance `j` through `t` always. When `s[i] == t[j]`, advance both (we matched a character of `s`). If `i` reaches the end of `s`, all characters matched → return True.

#### Pointer Meaning
```
i = current character in s we are trying to match
j = current position in t we are scanning
```

#### Why Pointer Movement Works
We always advance `j` because we're scanning `t` linearly. We advance `i` only when we find a match — greedy matching the earliest possible occurrence. This greedy choice is safe: matching `s[i]` at the earliest position leaves all remaining positions of `t` available for remaining characters of `s`. Skipping a match could only reduce options.

#### Algorithm
1. Set `i = 0`, `j = 0`.
2. While `i < len(s)` and `j < len(t)`:
   - If `s[i] == t[j]` → `i += 1`, `j += 1`
   - Else → `j += 1`
3. Return `i == len(s)`.

#### Python Code
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i, j = 0, 0
        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
            j += 1
        return i == len(s)
```

#### Dry Run
```
s = "ace", t = "abcde"

i=0(a), j=0(a): match → i=1, j=1
i=1(c), j=1(b): no match → j=2
i=1(c), j=2(c): match → i=2, j=3
i=2(e), j=3(d): no match → j=4
i=2(e), j=4(e): match → i=3, j=5 → i=len(s)=3 → True
```

```
Time:  O(n) where n = len(t)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Is string A a subsequence of string B?"
- One pointer scans the target (always advances), one pointer tracks progress through the source
- Greedy: match as early as possible
- No sorting needed

---

## Problem 9 — Boats to Save People

**LeetCode #881 | Difficulty: Medium | Pattern: Converging**

Each boat can carry at most 2 people, with a weight limit of `limit`. Given `people` weights, find the minimum number of boats needed.

---

### 2. How to Recognize This Pattern

**Clues:**
- Pair people optimally (one heavy + one light)
- Sorted array lets us pair heaviest with lightest
- Greedy: try to pair the heaviest person with the lightest

---

### 3. Brute Force Approach

**Idea:** Try all pairings (exponential). Not practical. Skip directly to greedy.

---

### 4. Better Approach

**There is no meaningful better approach; we move directly to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Sort the weights. Try to pair the heaviest person (`right`) with the lightest person (`left`). If they fit together, great — both board. If they don't fit, the heaviest person goes alone (since no one lighter can help — even the lightest person is too heavy to share).

#### Pointer Meaning
```
left  = lightest remaining person
right = heaviest remaining person
```

#### Why Pointer Movement Works
- If `people[left] + people[right] <= limit`: They can share a boat. `left` can no longer be paired with anyone heavier (we're trying `right` first). Move both pointers.
- If `people[left] + people[right] > limit`: `right` cannot share with anyone — even the lightest person (`left`) is too heavy. `right` must go alone. Move `right` left.

We never miss a better pairing: we always try the heaviest with the lightest. If they can share, great. If not, the heaviest must go solo (the only potentially helpful person was the lightest, and they can't fit).

#### Algorithm
1. Sort `people`.
2. Set `left = 0`, `right = n - 1`, `boats = 0`.
3. While `left <= right`:
   - If `people[left] + people[right] <= limit` → `left += 1` (both board together)
   - `right -= 1` (heaviest person always boards, alone or with lightest)
   - `boats += 1`
4. Return `boats`.

#### Python Code
```python
class Solution:
    def numRescueBoats(self, people: list[int], limit: int) -> int:
        people.sort()
        left, right = 0, len(people) - 1
        boats = 0

        while left <= right:
            if people[left] + people[right] <= limit:
                left += 1  # light person also boards
            right -= 1      # heavy person always boards
            boats += 1

        return boats
```

#### Dry Run
```
people = [3, 5, 3, 4], limit = 5
After sort: [3, 3, 4, 5]

left=0(3), right=3(5): 3+5=8>5 → right alone → right=2, boats=1
left=0(3), right=2(4): 3+4=7>5 → right alone → right=1, boats=2
left=0(3), right=1(3): 3+3=6>5 → right alone → right=0, boats=3
left=0(3), right=0(3): left==right (single person) → right alone → right=-1, boats=4
Answer: 4

people = [1, 2], limit = 3:
left=0(1), right=1(2): 1+2=3≤3 → left=1, right=0, boats=1 → stop
Answer: 1
```

```
Time:  O(n log n) for sorting
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Pair elements (subject to a sum constraint) to minimize total groups
- Greedy: heaviest with lightest
- Sorted + Two Pointers
- Whenever `right` cannot pair, it goes alone → `right -= 1` always happens

---

## Problem 10 — Squares of a Sorted Array

**LeetCode #977 | Difficulty: Easy | Pattern: Converging**

Given an integer array `nums` sorted in non-decreasing order, return an array of the squares of each number, also in non-decreasing order.

---

### 2. How to Recognize This Pattern

**Clues:**
- Array has negative numbers; squares of negatives are positive
- After squaring, the largest values can be at either end (most negative or most positive)
- Need sorted output → fill from the back

---

### 3. Brute Force Approach

```python
class Solution:
    def sortedSquares(self, nums: list[int]) -> list[int]:
        return sorted(x * x for x in nums)
```

```
Time:  O(n log n)
Space: O(n)
```

---

### 4. Better Approach

**There is no meaningful better intermediate approach.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
The largest square must come from either the leftmost element (most negative) or the rightmost element (most positive). Use two pointers converging from both ends, always placing the larger square at the back of the result array.

#### Pointer Meaning
```
left  = index 0 (most negative or smallest)
right = index n-1 (most positive or largest)
```

#### Why Pointer Movement Works
`nums` is sorted. The absolute values are largest at the two ends and smallest near zero. So the largest square is always `max(nums[left]^2, nums[right]^2)`. Place it at the current back of the result and advance the pointer that contributed it.

We fill the result array **from right to left** (largest square first).

#### Algorithm
1. Create result array of size `n`.
2. Set `left = 0`, `right = n-1`, `pos = n-1`.
3. While `left <= right`:
   - If `abs(nums[left]) >= abs(nums[right])`:
     - `result[pos] = nums[left] ** 2`
     - `left += 1`
   - Else:
     - `result[pos] = nums[right] ** 2`
     - `right -= 1`
   - `pos -= 1`
4. Return `result`.

#### Python Code
```python
class Solution:
    def sortedSquares(self, nums: list[int]) -> list[int]:
        n = len(nums)
        result = [0] * n
        left, right = 0, n - 1
        pos = n - 1

        while left <= right:
            if abs(nums[left]) >= abs(nums[right]):
                result[pos] = nums[left] ** 2
                left += 1
            else:
                result[pos] = nums[right] ** 2
                right -= 1
            pos -= 1

        return result
```

#### Dry Run
```
nums = [-4, -1, 0, 3, 10]

pos=4: |(-4)|=4 < |(10)|=10 → result[4]=100, right=3
pos=3: |(-4)|=4 > |(3)|=3   → result[3]=16,  left=1
pos=2: |(-1)|=1 < |(3)|=3   → result[2]=9,   right=2
pos=1: |(-1)|=1 > |(0)|=0   → result[1]=1,   left=2
pos=0: left==right=2, |(0)| → result[0]=0,   left=3 → stop

Result: [0, 1, 9, 16, 100] ✓
```

```
Time:  O(n) — single pass
Space: O(n) for output array
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Sorted array + negatives → squares are "V-shaped" (largest at both ends)
- Fill result from the back (two pointers converging, fill backward)
- This "fill from back" pattern is common when merging sorted data

---

## Problem 11 — 3Sum Smaller

**LeetCode #259 | Difficulty: Medium | Pattern: Converging**

Given `nums` and `target`, count the number of **triplets** `i < j < k` such that `nums[i] + nums[j] + nums[k] < target`.

---

### 2. How to Recognize This Pattern

**Clues:**
- Triplet count with a less-than condition (not equality)
- Sort + fix one element + Two Pointers on the rest
- When `sum < target`, multiple valid pairs exist at once — count them efficiently

---

### 3. Brute Force Approach

```python
class Solution:
    def threeSumSmaller(self, nums: list[int], target: int) -> int:
        n = len(nums)
        count = 0
        for i in range(n):
            for j in range(i + 1, n):
                for k in range(j + 1, n):
                    if nums[i] + nums[j] + nums[k] < target:
                        count += 1
        return count
```

```
Time:  O(n³)
Space: O(1)
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Sort the array. Fix element at index `i`. Use Two Pointers (`left = i+1`, `right = n-1`) for the inner pair.

**Key insight (different from 3Sum):** If `nums[i] + nums[left] + nums[right] < target`, then **all** pairs `(left, k)` where `left < k <= right` also satisfy the condition, because `nums[k] <= nums[right]` (sorted). So we can add `right - left` pairs at once.

#### Pointer Meaning
```
i     = fixed first element
left  = i + 1
right = n - 1
```

#### Why Pointer Movement Works
- If `sum < target`: All pairs `(left, left+1), (left, left+2), ..., (left, right)` are valid. Count = `right - left`. Then `right -= 1` to check if a smaller `right` still satisfies.
- If `sum >= target`: Need a smaller sum → `right -= 1`.

Wait — why not `left += 1`? Because if the sum is too large, making the left bigger makes it worse. We must reduce the right.

#### Algorithm
1. Sort `nums`.
2. `count = 0`.
3. For `i` from 0 to n-3:
   - Set `left = i+1`, `right = n-1`.
   - While `left < right`:
     - If `nums[i] + nums[left] + nums[right] < target`:
       - Add `right - left` to count.
       - `left += 1`
     - Else: `right -= 1`
4. Return `count`.

#### Python Code
```python
class Solution:
    def threeSumSmaller(self, nums: list[int], target: int) -> int:
        nums.sort()
        n = len(nums)
        count = 0

        for i in range(n - 2):
            left, right = i + 1, n - 1
            while left < right:
                s = nums[i] + nums[left] + nums[right]
                if s < target:
                    # All pairs (left, left+1), ..., (left, right) are valid
                    count += right - left
                    left += 1
                else:
                    right -= 1

        return count
```

#### Dry Run
```
nums = [-2, 0, 1, 3], target = 2
After sort: [-2, 0, 1, 3]

i=0(-2), left=1(0), right=3(3):
  -2+0+3=1 < 2 → count += 3-1=2, left=2
  -2+1+3=2 >= 2 → right=2
  left=right=2 → stop (count=2)

i=1(0), left=2(1), right=3(3):
  0+1+3=4 >= 2 → right=2
  left=right=2 → stop

Answer: 2
Valid triplets: (-2,0,1)=-1, (-2,0,3)=1 → both < 2 ✓
```

```
Time:  O(n²)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Count triplets with sum **less than** target (not equal)
- "Count at once" insight: when `sum < target`, all smaller right values also work → add `right - left`
- This is 3Sum with "counting valid pairs" instead of recording exact values

---

---

# Pattern 2 — Fast & Slow Pointers

> Two pointers move through a sequence at different speeds. The "slow" pointer moves one step at a time; the "fast" pointer moves two steps at a time (or some multiple). Used to detect cycles and find midpoints in linked lists, and to detect cycles in mathematical sequences.

---

## Problem 12 — Linked List Cycle

**LeetCode #141 | Difficulty: Easy | Pattern: Fast & Slow**

Given the head of a linked list, determine if it contains a cycle. A cycle means some node's `next` pointer points back to an earlier node.

---

### 2. How to Recognize This Pattern

**Clues:**
- Linked list problem
- "Does a cycle exist?" or "Will this sequence ever repeat?"
- Must detect infinite loop
- O(1) space required (otherwise use a visited set)

**Why Fast & Slow:** If there's a cycle, the fast pointer will eventually catch up to the slow pointer (like a runner lapping another on a circular track). If there's no cycle, the fast pointer will reach the end.

---

### 3. Brute Force Approach

**Idea:** Track all visited nodes in a set. If we visit a node we've seen before, there's a cycle.

```python
class Solution:
    def hasCycle(self, head) -> bool:
        visited = set()
        curr = head
        while curr:
            if curr in visited:
                return True
            visited.add(curr)
            curr = curr.next
        return False
```

```
Time:  O(n)
Space: O(n) — hash set of visited nodes
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Fast & Slow Pointer Approach

#### Core Idea
Use two pointers: `slow` moves one step at a time, `fast` moves two steps at a time. If there's a cycle, they will inevitably meet inside the cycle. If there's no cycle, `fast` will reach `None`.

#### Pointer Meaning
```
slow = moves 1 step per iteration (tortoise)
fast = moves 2 steps per iteration (hare)
```

#### Why Pointer Movement Works
**If no cycle:** `fast` will reach `None` in O(n/2) = O(n) steps.

**If there is a cycle:** Think of it this way — once both pointers are inside the cycle, `fast` gains 1 step on `slow` per iteration (fast moves 2, slow moves 1 → net gain = 1). If the cycle length is `L`, then after at most `L` iterations inside the cycle, `fast` catches `slow`. So they will always meet.

**Edge cases:**
- `head = None` → no cycle, return False
- Single node with `next = None` → no cycle
- `fast.next` must be checked before `fast.next.next` to avoid null pointer error

#### Algorithm
1. If `head` is None, return False.
2. Set `slow = head`, `fast = head`.
3. While `fast` and `fast.next` are not None:
   - `slow = slow.next`
   - `fast = fast.next.next`
   - If `slow == fast` → return True (cycle detected)
4. Return False (fast reached end, no cycle)

#### Python Code
```python
class Solution:
    def hasCycle(self, head) -> bool:
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

**Note on `while fast and fast.next`:**
- `fast` checks if `fast` itself is not None.
- `fast.next` checks if `fast.next` is not None (required before doing `fast.next.next`).
- Both must be checked to avoid `AttributeError`.

#### Dry Run
```
List: 3 → 2 → 0 → -4
                ↑_____|   (cycle: -4 points back to 2)

Step 1: slow=3, fast=3
Step 2: slow=2, fast=0
Step 3: slow=0, fast=2  (fast: 0→-4→2)
Step 4: slow=-4, fast=-4 (fast: 2→0→-4, slow: 0→-4)
slow == fast → True ✓
```

```
Time:  O(n) — at most n steps to detect cycle
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Linked list + "does a cycle exist?" → Fast & Slow immediately
- O(1) space requirement rules out visited set
- The "runner metaphor": on a circular track, the fast runner always laps the slow runner
- `while fast and fast.next` is the standard loop condition (memorize this)

---

## Problem 13 — Happy Number

**LeetCode #202 | Difficulty: Easy | Pattern: Fast & Slow**

A "happy number" is defined by: Start with any positive integer, replace the number by the sum of the squares of its digits, and repeat. If you eventually reach 1, it's happy. If it loops endlessly without reaching 1, it's not.

---

### 2. How to Recognize This Pattern

**Clues:**
- A process that either terminates or cycles
- "Will this sequence ever repeat?" → cycle detection
- The sequence of values is like a linked list — each value has exactly one "next" value

---

### 3. Brute Force Approach

**Idea:** Detect the cycle using a set.

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def sum_of_squares(num):
            total = 0
            while num:
                digit = num % 10
                total += digit * digit
                num //= 10
            return total

        seen = set()
        while n != 1:
            if n in seen:
                return False
            seen.add(n)
            n = sum_of_squares(n)
        return True
```

```
Time:  O(log n) per step (digit count), O(k log n) total where k = cycle length
Space: O(k) for the set
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Fast & Slow Pointer Approach

#### Core Idea
The sequence of values forms a "linked list" where each number's "next" is the sum of squares of its digits. If the sequence is happy, it reaches 1 (which maps to 1 forever). If not, it enters a cycle. Use Floyd's cycle detection.

#### Pointer Meaning
```
slow = moves one step (one sum-of-squares computation)
fast = moves two steps
```

#### Why Pointer Movement Works
Same as Linked List Cycle: if a cycle exists, fast catches slow. If the sequence reaches 1, both pointers will eventually be at 1 → `slow == fast == 1` → return True.

#### Python Code
```python
class Solution:
    def isHappy(self, n: int) -> bool:
        def get_next(num):
            total = 0
            while num:
                digit = num % 10
                total += digit * digit
                num //= 10
            return total

        slow = n
        fast = get_next(n)

        while fast != 1 and slow != fast:
            slow = get_next(slow)
            fast = get_next(get_next(fast))

        return fast == 1
```

**Python note:** `num % 10` gives the last digit. `num //= 10` removes the last digit (`//` is integer division in Python 3).

#### Dry Run
```
n = 19

get_next(19) = 1² + 9² = 1 + 81 = 82
get_next(82) = 8² + 2² = 64 + 4 = 68
get_next(68) = 6² + 8² = 36 + 64 = 100
get_next(100) = 1² + 0² + 0² = 1

slow: 19 → 82 → 68 → 100 → 1
fast: 82 → 100 → 1 → 1 → ...

When fast == 1 → return True ✓
```

```
Time:  O(log n) — number of digits determines computation per step
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Process that either terminates or repeats → cycle detection
- Each state has exactly one "next state" → implicit linked list
- Fast & Slow replaces a visited set, saving O(n) space
- Return `fast == 1` (not just checking if they meet — we need to check *where* they meet)

---

## Problem 14 — Find the Duplicate Number

**LeetCode #287 | Difficulty: Medium | Pattern: Fast & Slow**

Given an array `nums` of `n+1` integers where each integer is in `[1, n]`, find the duplicate number. There is exactly one repeated number but it may appear more than once. Do not modify the array. Use only O(1) extra space.

---

### 2. How to Recognize This Pattern

**Clues:**
- Array of length n+1, values in [1,n] → by pigeonhole, a duplicate exists
- "Do not modify the array, O(1) space" → rules out sort and hash map
- The array can be viewed as a linked list: `nums[i]` gives the "next" of node `i`
- A duplicate value means two nodes point to the same "next" → a cycle

---

### 3. Brute Force Approach

```python
class Solution:
    def findDuplicate(self, nums: list[int]) -> int:
        seen = set()
        for num in nums:
            if num in seen:
                return num
            seen.add(num)
        return -1
```

```
Time:  O(n)
Space: O(n)
```

---

### 4. Better Approach — Sort (but modifies input)

```python
class Solution:
    def findDuplicate(self, nums: list[int]) -> int:
        nums.sort()  # Modifies input — not allowed per constraints
        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return nums[i]
        return -1
```

```
Time:  O(n log n)
Space: O(1) — but modifies input
```

---

### 5. Optimal Fast & Slow Pointer Approach (Floyd's Cycle Detection)

#### Core Idea
Think of the array as a linked list: node `i` has `next = nums[i]`. Since `nums[i]` is in `[1,n]` and there are `n+1` elements, there must be a cycle (the duplicate is the entry point of the cycle).

Two phases:
1. **Phase 1:** Find the meeting point of slow and fast inside the cycle.
2. **Phase 2:** Find the entry point of the cycle (= duplicate number). Start one pointer at `nums[0]` and one at the meeting point, advance both one step at a time → they meet at the cycle entry.

#### Pointer Meaning
```
slow = follows nums[slow] (one step)
fast = follows nums[nums[fast]] (two steps)
```

#### Why Phase 2 Works (Mathematical insight)
Let:
- `F` = distance from start to cycle entry
- `C` = cycle length
- `h` = distance from cycle entry to meeting point

When they meet: slow has traveled `F + h`, fast has traveled `F + h + C` (one full extra cycle). Since fast moves twice as fast: `2(F + h) = F + h + C` → `F = C - h`.

This means: if one pointer starts from the head and another from the meeting point, both moving one step, they travel `F` steps and meet at the cycle entry.

#### Algorithm
1. `slow = nums[0]`, `fast = nums[0]`.
2. Phase 1: while True:
   - `slow = nums[slow]`
   - `fast = nums[nums[fast]]`
   - If `slow == fast` → break
3. Phase 2: `slow = nums[0]`. While `slow != fast`:
   - `slow = nums[slow]`
   - `fast = nums[fast]`
4. Return `slow`.

#### Python Code
```python
class Solution:
    def findDuplicate(self, nums: list[int]) -> int:
        # Phase 1: Find meeting point inside cycle
        slow = nums[0]
        fast = nums[0]
        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break

        # Phase 2: Find cycle entry = duplicate
        slow = nums[0]
        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]

        return slow
```

#### Dry Run
```
nums = [1, 3, 4, 2, 2]
Linked list: 0→1→3→2→4→2→4→... (cycle at node 2)

Phase 1:
slow: 1 → 3 → 2 → 4 → 2
fast: 3 → 2 → 2 (nums[nums[3]]=nums[2]=4, nums[nums[4]]=nums[2]=4, ...)

Let's trace carefully:
Start: slow=nums[0]=1, fast=nums[0]=1
Iter 1: slow=nums[1]=3, fast=nums[nums[1]]=nums[3]=2
Iter 2: slow=nums[3]=2, fast=nums[nums[2]]=nums[4]=2
slow==fast=2 → break. Meeting point = 2.

Phase 2:
slow=nums[0]=1, fast=2
Iter 1: slow=nums[1]=3, fast=nums[2]=4
Iter 2: slow=nums[3]=2, fast=nums[4]=2
slow==fast=2 → return 2 ✓
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Array of n+1 values in [1,n] + "find duplicate without modifying + O(1) space"
- Key realization: treat array as a linked list (index = node, value = next pointer)
- Duplicate = cycle entry point
- Floyd's algorithm in two phases: detect meeting point, then find cycle entry
- This is a clever application — explain the array-as-linked-list insight clearly

---

---

# Pattern 3 — Fixed Separation Pointers

> Two pointers maintain a fixed distance (gap) between them. Typically used to find the k-th node from the end, the middle node, or to delete specific nodes in a linked list.

---

## Problem 15 — Remove Nth Node From End of List

**LeetCode #19 | Difficulty: Medium | Pattern: Fixed Separation**

Given the head of a linked list, remove the `n`-th node from the end and return the head.

---

### 2. How to Recognize This Pattern

**Clues:**
- Linked list + "nth from end"
- Can't access nodes by index directly
- Two pointers with a fixed gap of `n` nodes allow you to "see" the nth-from-end node

---

### 3. Brute Force Approach

**Idea:** Two-pass. First pass: count total length. Second pass: navigate to `(length - n)`-th node.

```python
class Solution:
    def removeNthFromEnd(self, head, n: int):
        # Count length
        length = 0
        curr = head
        while curr:
            length += 1
            curr = curr.next

        # Create dummy node to handle edge cases (removing head)
        dummy = ListNode(0)
        dummy.next = head
        curr = dummy

        # Navigate to node before the target
        for _ in range(length - n):
            curr = curr.next

        curr.next = curr.next.next
        return dummy.next
```

```
Time:  O(n) — two passes
Space: O(1)
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Fixed Separation Approach (One Pass)

#### Core Idea
Use a dummy node before head. Set `fast` pointer `n+1` steps ahead of `slow`. Advance both until `fast` reaches `None`. At that point, `slow` is right before the node to delete.

#### Pointer Meaning
```
slow = will stop just before the node to be deleted
fast = always exactly n+1 ahead of slow
```

#### Why Pointer Movement Works
If `fast` is `n+1` ahead of `slow`, when `fast` reaches `None` (end), `slow` is at position `length - (n+1)` from the start (0-indexed). That means `slow.next` is the nth node from the end → delete it by `slow.next = slow.next.next`.

The dummy node handles the edge case of removing the head (the 1st node, which is the nth from end when n = length).

#### Algorithm
1. Create `dummy = ListNode(0)`, `dummy.next = head`.
2. Set `fast = dummy`, `slow = dummy`.
3. Advance `fast` by `n+1` steps.
4. While `fast` is not None: advance both `slow` and `fast` one step.
5. `slow.next = slow.next.next` (delete the target node).
6. Return `dummy.next`.

#### Python Code
```python
class Solution:
    def removeNthFromEnd(self, head, n: int):
        dummy = ListNode(0)
        dummy.next = head
        slow, fast = dummy, dummy

        # Move fast n+1 steps ahead
        for _ in range(n + 1):
            fast = fast.next

        # Move both until fast reaches None
        while fast:
            slow = slow.next
            fast = fast.next

        # Delete the nth node from end
        slow.next = slow.next.next
        return dummy.next
```

#### Dry Run
```
List: 1 → 2 → 3 → 4 → 5, n = 2
dummy → 1 → 2 → 3 → 4 → 5 → None

After moving fast 3 steps (n+1=3):
slow = dummy, fast = 3

Advance both:
slow=1, fast=4
slow=2, fast=5
slow=3, fast=None → stop

slow.next = 4 → delete 4 → slow.next = 5
List: 1 → 2 → 3 → 5 ✓ (removed 4, which is 2nd from end)
```

**Edge cases:**
- Remove head (n = length): `fast` will be None after n+1 steps, `slow` stays at dummy → `dummy.next = dummy.next.next` removes head correctly.

```
Time:  O(L) — single pass
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Nth from end" in a linked list → Fixed Separation
- Move one pointer n steps ahead, then advance both → gap is maintained
- Always use a dummy node to handle head deletion edge case
- The gap should be `n+1` (not `n`) if you want slow to stop *before* the target

---

## Problem 16 — Middle of the Linked List

**LeetCode #876 | Difficulty: Easy | Pattern: Fast & Slow**

Given the head of a singly linked list, return the middle node. If there are two middle nodes (even length), return the second middle.

---

### 2. How to Recognize This Pattern

**Clues:**
- Find middle of linked list without knowing the length
- Can't use indices
- Fast & Slow: when fast reaches end, slow is at middle

---

### 3. Brute Force Approach

```python
class Solution:
    def middleNode(self, head):
        nodes = []
        curr = head
        while curr:
            nodes.append(curr)
            curr = curr.next
        return nodes[len(nodes) // 2]
```

```
Time:  O(n)
Space: O(n)
```

---

### 4. Better Approach

**There is no meaningful better approach; we move directly from brute force to optimal.**

---

### 5. Optimal Fast & Slow Pointer Approach

#### Core Idea
`slow` moves 1 step, `fast` moves 2 steps. When `fast` reaches the end, `slow` is at the middle.

#### Pointer Meaning
```
slow = current candidate for middle
fast = pace-setter (moves twice as fast)
```

#### Why Pointer Movement Works
After k iterations: slow is at position k, fast is at position 2k. When `fast` reaches `None` (position n): `2k ≈ n` → `k ≈ n/2` → slow is at the middle.

For **even length** (n = 4): fast reaches position 4 (None) when k = 2 → slow is at index 2 (second middle node). This matches the problem requirement.

#### Algorithm
1. Set `slow = head`, `fast = head`.
2. While `fast` and `fast.next` are not None:
   - `slow = slow.next`
   - `fast = fast.next.next`
3. Return `slow`.

#### Python Code
```python
class Solution:
    def middleNode(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

#### Dry Run
```
Odd length: 1 → 2 → 3 → 4 → 5
slow: 1 → 2 → 3 (fast reaches 5, fast.next=None → stop)
Return node 3 ✓

Even length: 1 → 2 → 3 → 4
slow: 1 → 2 → 3 (fast reaches 4, fast.next=None → stop... wait)
Actually:
Iter 1: slow=2, fast=3
Iter 2: slow=3, fast=None (fast=fast.next.next = 4.next.next... 4.next=None) → stop
Return node 3 (second middle) ✓
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Find middle without knowing length" → Fast & Slow
- For first middle (even): use `while fast.next and fast.next.next`
- For second middle (even): use `while fast and fast.next` (current default)
- This is a building block for many other problems (merge sort on lists, etc.)

---

## Problem 17 — Delete the Middle Node of a Linked List

**LeetCode #2095 | Difficulty: Medium | Pattern: Fixed Separation / Fast & Slow**

Delete the **middle node** of a linked list and return the head. The middle is index `n // 2` (0-indexed).

---

### 2. How to Recognize This Pattern

**Clues:**
- Delete the middle → need to find the node *before* the middle
- Extends "Middle of Linked List": instead of stopping at middle, stop one node before

---

### 3. Brute Force Approach

```python
class Solution:
    def deleteMiddle(self, head):
        if not head or not head.next:
            return None

        nodes = []
        curr = head
        while curr:
            nodes.append(curr)
            curr = curr.next

        mid = len(nodes) // 2
        if mid == 0:
            return head.next

        nodes[mid - 1].next = nodes[mid].next
        return head
```

```
Time:  O(n)
Space: O(n)
```

---

### 5. Optimal Fast & Slow Approach (Modified)

#### Core Idea
Use a `prev` pointer tracking the node before `slow`. When `fast` reaches the end, `slow` is at the middle and `prev` is right before it — delete `slow`.

#### Python Code
```python
class Solution:
    def deleteMiddle(self, head):
        if not head or not head.next:
            return None

        slow, fast = head, head
        prev = None

        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next

        # slow is at the middle; prev is before it
        prev.next = slow.next
        return head
```

#### Dry Run
```
List: 1 → 2 → 3 → 4 → 5
mid index = 5//2 = 2 → delete node with value 3

Iter 1: prev=1, slow=2, fast=3
Iter 2: prev=2, slow=3, fast=5 (fast.next=None → stop)
prev.next = slow.next = 4
List: 1 → 2 → 4 → 5 ✓
```

**Edge cases:**
- Single node: return `None` (no middle to delete, or the only node is the middle)
- Two nodes: middle is index 1 → delete second node

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Delete middle node" = find node *before* middle → use `prev` pointer
- Directly extends #876; just track `prev = slow` each iteration
- Handle single-node edge case first (return None)

---

---

# Pattern 4 — In-place Array Modification

> One pointer (`read`) scans the array, another pointer (`write`) tracks where to place the next valid element. Elements are overwritten in-place. The write pointer only advances when a valid element is placed.

---

## Problem 18 — Remove Duplicates from Sorted Array

**LeetCode #26 | Difficulty: Easy | Pattern: In-place Modification**

Given a sorted array `nums`, remove duplicates **in-place** such that each unique element appears once. Return the count of unique elements. The first `k` elements of `nums` should contain the unique values.

---

### 2. How to Recognize This Pattern

**Clues:**
- Sorted array (duplicates are adjacent)
- "In-place" modification
- Return count + modify first k elements
- Read pointer scans, write pointer places valid values

---

### 3. Brute Force Approach

**Idea:** Use extra space to store unique elements.

```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        unique = list(dict.fromkeys(nums))  # preserves order, removes dups
        for i in range(len(unique)):
            nums[i] = unique[i]
        return len(unique)
```

```
Time:  O(n)
Space: O(n)
```

---

### 5. Optimal Two Pointer Approach

#### Core Idea
`write` starts at 1 (the second position). `read` scans from 1 to end. If `nums[read] != nums[read-1]` (or != `nums[write-1]`), it's a new unique element → copy to `nums[write]` and advance `write`.

#### Pointer Meaning
```
write = next position to place a unique element
read  = current element being examined
```

#### Why Pointer Movement Works
The array is sorted → duplicates are consecutive. `write` only advances when we place a new unique element. `read` always advances. After processing, `nums[0...write-1]` contains all unique elements in order.

#### Python Code
```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        write = 1
        for read in range(1, len(nums)):
            if nums[read] != nums[read - 1]:
                nums[write] = nums[read]
                write += 1
        return write
```

**Python note:** `for read in range(1, len(nums))` iterates `read` from 1 to n-1 inclusive.

#### Dry Run
```
nums = [1, 1, 2, 2, 3]

read=1: nums[1]=1 == nums[0]=1 → skip
read=2: nums[2]=2 != nums[1]=1 → nums[1]=2, write=2
read=3: nums[3]=2 == nums[2]=2 → skip
read=4: nums[4]=3 != nums[3]=2 → nums[2]=3, write=3

nums = [1, 2, 3, _, _], return 3
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Sorted array + remove duplicates in-place → read/write two pointers
- `write` = "next valid slot"; only advances on valid placement
- Compare `nums[read]` with `nums[write-1]` (the last placed element)

---

## Problem 19 — Remove Element

**LeetCode #27 | Difficulty: Easy | Pattern: In-place Modification**

Given an array `nums` and a value `val`, remove all occurrences of `val` in-place. Return the count of remaining elements.

---

### 5. Optimal Two Pointer Approach

#### Core Idea
`write` starts at 0. Scan `read` from 0 to end. If `nums[read] != val` → copy to `nums[write]`, advance `write`.

#### Python Code
```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        write = 0
        for read in range(len(nums)):
            if nums[read] != val:
                nums[write] = nums[read]
                write += 1
        return write
```

#### Dry Run
```
nums = [3, 2, 2, 3], val = 3

read=0: nums[0]=3 == val → skip
read=1: nums[1]=2 != val → nums[0]=2, write=1
read=2: nums[2]=2 != val → nums[1]=2, write=2
read=3: nums[3]=3 == val → skip

nums = [2, 2, _, _], return 2
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Remove specific values in-place (array not necessarily sorted)
- `read` scans all, `write` only advances on "keep" elements
- Simpler than #26 — no comparison with previous element needed

---

## Problem 20 — Sort Colors

**LeetCode #75 | Difficulty: Medium | Pattern: In-place Modification (Dutch National Flag)**

Given an array `nums` with values 0, 1, 2 (representing red, white, blue), sort them in-place so that 0s come first, then 1s, then 2s.

---

### 2. How to Recognize This Pattern

**Clues:**
- Three distinct values to sort
- In-place, O(n), one-pass preferred
- "Dutch National Flag" — classic three-way partition

---

### 3. Brute Force Approach

```python
class Solution:
    def sortColors(self, nums: list[int]) -> None:
        nums.sort()
```

```
Time:  O(n log n)
Space: O(1)
```

Or count occurrences and fill:
```python
class Solution:
    def sortColors(self, nums: list[int]) -> None:
        count = [0, 0, 0]
        for num in nums:
            count[num] += 1
        i = 0
        for color in range(3):
            for _ in range(count[color]):
                nums[i] = color
                i += 1
```

```
Time:  O(n) — two passes
Space: O(1)
```

---

### 5. Optimal Three-Pointer Approach (One Pass)

#### Core Idea
**Dutch National Flag Algorithm.** Three pointers:
- `low`: everything to the left of `low` is 0
- `mid`: the current element being examined
- `high`: everything to the right of `high` is 2

When `mid` passes `high`, we're done.

#### Pointer Meaning
```
low  = next position for 0
mid  = current element
high = next position for 2 (from the right)
```

#### Why Pointer Movement Works
- `nums[mid] == 0`: Swap with `nums[low]`. Both `low` and `mid` advance (the swapped-in element is known to be 1, from a previous step or it's the start).
- `nums[mid] == 1`: It's in the right place → just advance `mid`.
- `nums[mid] == 2`: Swap with `nums[high]`. Decrease `high`. Do **not** advance `mid` — the swapped-in element is unknown and must be re-examined.

#### Algorithm
1. `low = 0`, `mid = 0`, `high = n - 1`.
2. While `mid <= high`:
   - If `nums[mid] == 0`: swap `nums[mid]` and `nums[low]`; `low += 1`; `mid += 1`.
   - If `nums[mid] == 1`: `mid += 1`.
   - If `nums[mid] == 2`: swap `nums[mid]` and `nums[high]`; `high -= 1`.
3. Done.

#### Python Code
```python
class Solution:
    def sortColors(self, nums: list[int]) -> None:
        low, mid, high = 0, 0, len(nums) - 1
        while mid <= high:
            if nums[mid] == 0:
                nums[low], nums[mid] = nums[mid], nums[low]
                low += 1
                mid += 1
            elif nums[mid] == 1:
                mid += 1
            else:  # nums[mid] == 2
                nums[mid], nums[high] = nums[high], nums[mid]
                high -= 1
```

**Python note:** `a, b = b, a` swaps two variables in one line without a temp variable.

#### Dry Run
```
nums = [2, 0, 2, 1, 1, 0]
low=0, mid=0, high=5

nums[0]=2: swap(0,5) → [0,0,2,1,1,2], high=4
nums[0]=0: swap(0,0) → [0,0,2,1,1,2], low=1, mid=1
nums[1]=0: swap(1,1) → [0,0,2,1,1,2], low=2, mid=2
nums[2]=2: swap(2,4) → [0,0,1,1,2,2], high=3
nums[2]=1: mid=3
nums[3]=1: mid=4 > high=3 → stop

Result: [0, 0, 1, 1, 2, 2] ✓
```

```
Time:  O(n) — single pass
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Sort array of 3 distinct values in-place" → Dutch National Flag
- Three partitions → three pointers (`low`, `mid`, `high`)
- Don't advance `mid` when swapping with `high` — the new element is unknown
- This pattern generalizes to "partition around two pivots"

---

## Problem 21 — Remove Duplicates from Sorted Array II

**LeetCode #80 | Difficulty: Medium | Pattern: In-place Modification**

Like #26, but each element may appear **at most twice** in the result.

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Allow each element to appear at most twice. Compare `nums[read]` with `nums[write-2]` (two positions back in the write side). If different, it's safe to include.

#### Why Pointer Movement Works
If `nums[read] != nums[write-2]`, then writing `nums[read]` won't create a third consecutive duplicate (since the last two written were different from `nums[read]` at minimum).

If `nums[read] == nums[write-2]`, including it would make three consecutive duplicates → skip.

#### Python Code
```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        write = 0
        for read in range(len(nums)):
            # Allow the element if write < 2 (first two always included)
            # or if it's different from the element two positions back
            if write < 2 or nums[read] != nums[write - 2]:
                nums[write] = nums[read]
                write += 1
        return write
```

**Generalization:** For "at most k occurrences", replace `2` with `k`:
```python
if write < k or nums[read] != nums[write - k]:
```

#### Dry Run
```
nums = [1, 1, 1, 2, 2, 3]

read=0: write<2 → nums[0]=1, write=1
read=1: write<2 → nums[1]=1, write=2
read=2: nums[2]=1 == nums[write-2]=nums[0]=1 → skip
read=3: nums[3]=2 != nums[0]=1 → nums[2]=2, write=3
read=4: nums[4]=2 != nums[1]=1 → nums[3]=2, write=4
read=5: nums[5]=3 != nums[2]=2 → nums[4]=3, write=5

nums = [1,1,2,2,3,_], return 5 ✓
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Allow at most k duplicates in-place" → compare with `nums[write - k]`
- Elegant generalization of #26
- `write < k` condition handles the initial fill phase

---

## Problem 22 — Move Zeroes

**LeetCode #283 | Difficulty: Easy | Pattern: In-place Modification**

Move all `0`s to the end of the array while maintaining the relative order of non-zero elements. Do it in-place.

---

### 5. Optimal Two Pointer Approach

#### Core Idea
`write` tracks the next position for a non-zero element. Scan `read` through the array. When `nums[read] != 0`, copy it to `nums[write]`. After the loop, fill remaining positions with `0`.

#### Python Code
```python
class Solution:
    def moveZeroes(self, nums: list[int]) -> None:
        write = 0
        for read in range(len(nums)):
            if nums[read] != 0:
                nums[write] = nums[read]
                write += 1
        # Fill remaining positions with zeros
        while write < len(nums):
            nums[write] = 0
            write += 1
```

**Alternative (swap-based, slightly more writes but avoids the fill loop):**
```python
class Solution:
    def moveZeroes(self, nums: list[int]) -> None:
        write = 0
        for read in range(len(nums)):
            if nums[read] != 0:
                nums[write], nums[read] = nums[read], nums[write]
                write += 1
```

#### Dry Run
```
nums = [0, 1, 0, 3, 12]

read=0: 0 → skip
read=1: 1 → nums[0]=1, write=1
read=2: 0 → skip
read=3: 3 → nums[1]=3, write=2
read=4: 12 → nums[2]=12, write=3

Fill: nums[3]=0, nums[4]=0
Result: [1, 3, 12, 0, 0] ✓
```

```
Time:  O(n)
Space: O(1)
```

---

## Problem 23 — String Compression

**LeetCode #443 | Difficulty: Medium | Pattern: In-place Modification**

Compress an array of characters in-place using the rule: consecutive repeated characters are replaced by the character followed by its count (only if count > 1). Return the new length.

---

### 2. How to Recognize This Pattern

**Clues:**
- In-place modification of a character array
- Processing runs of consecutive characters
- Read pointer scans, write pointer builds the output

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Use `read` to count the length of each group of consecutive characters. Use `write` to write the compressed result in-place.

#### Python Code
```python
class Solution:
    def compress(self, chars: list[str]) -> int:
        write = 0
        read = 0
        n = len(chars)

        while read < n:
            char = chars[read]
            count = 0

            # Count consecutive same characters
            while read < n and chars[read] == char:
                read += 1
                count += 1

            # Write the character
            chars[write] = char
            write += 1

            # Write the count (only if > 1)
            if count > 1:
                for digit in str(count):  # handles multi-digit counts
                    chars[write] = digit
                    write += 1

        return write
```

**Python note:** `str(count)` converts integer to string. `for digit in str(count)` iterates character by character (e.g., `str(12)` gives `'1'`, `'2'`).

#### Dry Run
```
chars = ['a','a','b','b','c','c','c']

read=0: char='a', count: read goes 0→1→2, count=2
  write 'a' → chars[0]='a', write=1
  count>1: write '2' → chars[1]='2', write=2

read=2: char='b', count: read goes 2→3→4, count=2
  write 'b' → chars[2]='b', write=3
  write '2' → chars[3]='2', write=4

read=4: char='c', count: read goes 4→5→6→7, count=3
  write 'c' → chars[4]='c', write=5
  write '3' → chars[5]='3', write=6

Result: ['a','2','b','2','c','3'], return 6 ✓
```

```
Time:  O(n)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Compress in-place" → read scans entire array, write builds compressed result
- Inner while loop counts run length
- Multi-digit counts require writing each digit separately

---

## Problem 24 — Sort Array By Parity

**LeetCode #905 | Difficulty: Easy | Pattern: In-place Modification**

Given `nums`, move all even integers to the beginning and odd integers to the end. Any valid answer is accepted.

---

### 5. Optimal Two Pointer Approach

**Approach 1: Read/Write**
```python
class Solution:
    def sortArrayByParity(self, nums: list[int]) -> list[int]:
        write = 0
        for read in range(len(nums)):
            if nums[read] % 2 == 0:
                nums[write], nums[read] = nums[read], nums[write]
                write += 1
        return nums
```

**Approach 2: Converging (swap from both ends)**
```python
class Solution:
    def sortArrayByParity(self, nums: list[int]) -> list[int]:
        left, right = 0, len(nums) - 1
        while left < right:
            while left < right and nums[left] % 2 == 0:
                left += 1
            while left < right and nums[right] % 2 == 1:
                right -= 1
            if left < right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right -= 1
        return nums
```

```
Time:  O(n)
Space: O(1)
```

---

## Problem 25 — Move Pieces to Obtain a String

**LeetCode #2337 | Difficulty: Medium | Pattern: In-place Modification / Two-string comparison**

Given two strings `start` and `target` (same length), both containing 'L', 'R', '_':
- 'L' can move left into a '_' (but not past another piece)
- 'R' can move right into a '_'
- Return True if `start` can be transformed into `target`.

---

### 2. How to Recognize This Pattern

**Clues:**
- Two strings, check if one transforms into another
- Pieces move in restricted directions
- Use two pointers (one per string), skip blanks, validate position constraints

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Both strings have the same L/R pieces in the same order (ignoring blanks). If the order differs → False. Then check positional constraints:
- 'L' in target must be at a position ≤ its position in start (L can only move left)
- 'R' in target must be at a position ≥ its position in start (R can only move right)

#### Python Code
```python
class Solution:
    def canChange(self, start: str, target: str) -> bool:
        n = len(start)
        i, j = 0, 0  # i for start, j for target

        while i <= n and j <= n:
            # Skip blanks in both strings
            while i < n and start[i] == '_':
                i += 1
            while j < n and target[j] == '_':
                j += 1

            # Both exhausted simultaneously → valid
            if i == n and j == n:
                return True
            # One exhausted but not the other → invalid
            if i == n or j == n:
                return False

            # Characters must match (same piece)
            if start[i] != target[j]:
                return False

            # Check movement direction constraint
            if start[i] == 'L' and i < j:
                return False  # L cannot move right
            if start[i] == 'R' and i > j:
                return False  # R cannot move left

            i += 1
            j += 1

        return True
```

#### Dry Run
```
start = "_L__R__R_", target = "L______RR"
n = 9

Pieces in start (non-blank): L(1), R(4), R(7)
Pieces in target (non-blank): L(0), R(7), R(8)

i=1(L), j=0(L): same char, L: i=1 >= j=0 → OK. i=2, j=1
i=4(R), j=7(R): same char, R: i=4 <= j=7 → OK. i=5, j=8
i=7(R), j=8(R): same char, R: i=7 <= j=8 → OK. i=8, j=9
Both at end → True ✓
```

```
Time:  O(n)
Space: O(1)
```

---

## Problem 26 — Separate Black and White Balls

**LeetCode #2938 | Difficulty: Medium | Pattern: In-place Modification**

Given a binary string `s` (0 = white, 1 = black), count the minimum number of swaps to group all blacks (1s) to the right. Adjacent swaps only.

---

### 5. Optimal Two Pointer / Greedy Approach

#### Core Idea
Count how many 0s appear after the current 1. Each 1 needs to "bubble past" all the 0s to its right to reach the end. The total number of such swaps is the answer.

**Alternative two-pointer view:** Use `right` pointer that tracks where the next 1 should be placed (from the right). Count how far each 1 is from its target position.

```python
class Solution:
    def minimumSwaps(self, s: str) -> int:
        swaps = 0
        black_count = 0  # number of 1s seen so far

        for ch in s:
            if ch == '1':
                black_count += 1
            else:
                # This 0 needs to pass 'black_count' ones to move left
                swaps += black_count

        return swaps
```

**Alternative (converging pointers):**
```python
class Solution:
    def minimumSwaps(self, s: str) -> int:
        swaps = 0
        right = len(s) - 1
        for i in range(len(s) - 1, -1, -1):
            if s[i] == '1':
                swaps += right - i
                right -= 1
        return swaps
```

```
Time:  O(n)
Space: O(1)
```

---

---

# Pattern 5 — String Comparison with Backspaces

---

## Problem 27 — Backspace String Compare

**LeetCode #844 | Difficulty: Easy | Pattern: String Comparison with Backspaces**

Given two strings `s` and `t` where `#` means backspace, return True if they are equal after applying backspaces.

---

### 2. How to Recognize This Pattern

**Clues:**
- Process strings with "delete" characters
- Compare resulting strings without extra O(n) space
- Scan from **right to left** — backspaces affect characters to the left

---

### 3. Brute Force Approach

**Idea:** Simulate the typing process using a stack.

```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        def process(string):
            stack = []
            for ch in string:
                if ch != '#':
                    stack.append(ch)
                elif stack:
                    stack.pop()
            return stack

        return process(s) == process(t)
```

```
Time:  O(n + m)
Space: O(n + m)
```

---

### 5. Optimal Two Pointer Approach (O(1) space)

#### Core Idea
Scan both strings from right to left. Skip characters that are "deleted" by `#`. Compare the actual characters one by one.

#### Pointer Meaning
```
i = current index in s (scanning right to left)
j = current index in t (scanning right to left)
```

#### Why Pointer Movement Works
Scanning right to left, a `#` tells us to skip the next non-`#` character to the left. We track how many characters to skip (`skip_s` and `skip_t`). When we find a real character with `skip = 0`, it's the next character to compare.

#### Python Code
```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        i, j = len(s) - 1, len(t) - 1
        skip_s, skip_t = 0, 0

        while i >= 0 or j >= 0:
            # Find next valid char in s
            while i >= 0:
                if s[i] == '#':
                    skip_s += 1
                    i -= 1
                elif skip_s > 0:
                    skip_s -= 1
                    i -= 1
                else:
                    break

            # Find next valid char in t
            while j >= 0:
                if t[j] == '#':
                    skip_t += 1
                    j -= 1
                elif skip_t > 0:
                    skip_t -= 1
                    j -= 1
                else:
                    break

            # Compare
            if i >= 0 and j >= 0:
                if s[i] != t[j]:
                    return False
            elif i >= 0 or j >= 0:
                # One string has a char left, the other doesn't
                return False

            i -= 1
            j -= 1

        return True
```

#### Dry Run
```
s = "ab#c", t = "ad#c"

i=3(c), j=3(c): both valid. s[3]='c' == t[3]='c' ✓. i=2, j=2
i=2(#): skip_s=1, i=1
i=1(b): skip_s>0 → skip, skip_s=0, i=0
i=0(a): valid, break.
j=2(#): skip_t=1, j=1
j=1(d): skip_t>0 → skip, skip_t=0, j=0
j=0(a): valid, break.
Compare: s[0]='a' == t[0]='a' ✓. i=-1, j=-1

Both exhausted → True ✓
```

```
Time:  O(n + m)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- `#` as backspace in strings → scan right to left
- Track `skip` count to handle consecutive backspaces
- Two pointers on two different strings advancing independently

---

---

# Pattern 6 — Expanding From Center

> For palindrome problems, expand outward from a center. Each palindrome has a center (single character for odd length, between two characters for even length). Try all possible centers.

---

## Problem 28 — Longest Palindromic Substring

**LeetCode #5 | Difficulty: Medium | Pattern: Expanding From Center**

Given string `s`, return the longest palindromic substring.

---

### 2. How to Recognize This Pattern

**Clues:**
- Palindrome problem on a string
- "Longest" → need to try all possibilities
- Expanding from center is more intuitive than DP for this pattern

---

### 3. Brute Force Approach

**Idea:** Check every substring.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def is_palindrome(sub):
            return sub == sub[::-1]

        n = len(s)
        best = ""
        for i in range(n):
            for j in range(i, n):
                sub = s[i:j+1]
                if is_palindrome(sub) and len(sub) > len(best):
                    best = sub
        return best
```

```
Time:  O(n³) — O(n²) substrings × O(n) palindrome check
Space: O(1)
```

---

### 4. Better Approach — Dynamic Programming

**Idea:** `dp[i][j]` = True if `s[i..j]` is a palindrome.

```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        n = len(s)
        dp = [[False] * n for _ in range(n)]
        start, max_len = 0, 1

        for i in range(n):
            dp[i][i] = True

        for length in range(2, n + 1):
            for i in range(n - length + 1):
                j = i + length - 1
                if s[i] == s[j]:
                    if length == 2 or dp[i+1][j-1]:
                        dp[i][j] = True
                        if length > max_len:
                            start = i
                            max_len = length

        return s[start:start + max_len]
```

```
Time:  O(n²)
Space: O(n²)
```

---

### 5. Optimal Expand From Center Approach

#### Core Idea
For each possible center, expand outward as long as characters match. There are `2n - 1` centers: `n` single characters (odd length) + `n-1` gaps between characters (even length).

#### Pointer Meaning
```
left  = left boundary of the current expansion
right = right boundary of the current expansion
```

#### Why Pointer Movement Works
A palindrome is symmetric. If `s[left] == s[right]`, the substring `s[left..right]` is a palindrome (given `s[left+1..right-1]` is). We expand outward: move `left -= 1` and `right += 1`. We stop when `s[left] != s[right]` or we reach the boundaries.

We try both odd centers (`left = right = i`) and even centers (`left = i, right = i+1`).

#### Python Code
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        def expand(left, right):
            # Expand while characters match and within bounds
            while left >= 0 and right < len(s) and s[left] == s[right]:
                left -= 1
                right += 1
            # After the loop, s[left+1..right-1] is the palindrome
            return s[left + 1:right]

        result = ""
        for i in range(len(s)):
            odd = expand(i, i)       # Odd-length palindrome
            even = expand(i, i + 1)  # Even-length palindrome
            if len(odd) > len(result):
                result = odd
            if len(even) > len(result):
                result = even

        return result
```

**Python note:** `s[left + 1:right]` — after the while loop exits, `left` and `right` are one step outside the valid palindrome. So the actual palindrome is `s[left+1 : right]`.

#### Dry Run
```
s = "babad"

i=0 (center='b'): odd expand(0,0): 'b'→ can't expand (index -1) → 'b'
                   even expand(0,1): s[0]='b',s[1]='a' → no match → ''

i=1 (center='a'): odd expand(1,1): expand: l=0,r=2 s[0]='b',s[2]='b' → expand
                   l=-1,r=3 → stop → s[0:3]='bab'
                   even expand(1,2): s[1]='a',s[2]='b' → no match → ''

i=2 (center='b'): odd expand(2,2): expand: l=1,r=3 s[1]='a',s[3]='a' → expand
                   l=0,r=4 s[0]='b',s[4]='d' → no match → s[1:4]='aba'
                   even expand(2,3): s[2]='b',s[3]='a' → no match → ''

i=3: 'aba' or 'a'
i=4: 'd'

Best: 'bab' (length 3) or 'aba' (length 3) — return 'bab' (first found)
```

```
Time:  O(n²) — n centers × O(n) expansion
Space: O(1) extra (excluding output)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Longest palindromic substring" → Expand from center
- `2n-1` centers: n odd centers + (n-1) even centers
- Expand function returns the palindrome (remember the off-by-one: `s[left+1:right]`)
- Cleaner and more space-efficient than DP

---

## Problem 29 — Palindromic Substrings

**LeetCode #647 | Difficulty: Medium | Pattern: Expanding From Center**

Count the total number of palindromic substrings in `s`.

---

### 5. Optimal Expand From Center Approach

**Same pattern as #5**, but count instead of tracking the longest.

```python
class Solution:
    def countSubstrings(self, s: str) -> int:
        count = 0

        def expand(left, right):
            nonlocal count
            while left >= 0 and right < len(s) and s[left] == s[right]:
                count += 1
                left -= 1
                right += 1

        for i in range(len(s)):
            expand(i, i)      # Odd-length
            expand(i, i + 1)  # Even-length

        return count
```

**Python note:** `nonlocal count` allows the nested function to modify the `count` variable from the enclosing scope.

#### Dry Run
```
s = "aaa"

i=0: odd(0,0): [a] count=1; expand: [aaa] count=2
      even(0,1): [aa] count=3; expand: out of bounds → stop

i=1: odd(1,1): [a] count=4; (already counted [aaa])
      even(1,2): [aa] count=5

i=2: odd(2,2): [a] count=6

Answer: 6 ✓ (a,a,a,aa,aa,aaa)
```

```
Time:  O(n²)
Space: O(1)
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- Count palindromic substrings → expand from center, increment count at each valid expansion
- Both odd and even centers must be tried
- Each successful expansion adds exactly 1 to the count

---

---

# Pattern 7 — String Reversal

> Use two pointers starting at both ends of the string and moving toward each other, swapping characters. Also used to reverse specific portions while preserving structure.

---

## Problem 30 — Reverse Words in a String

**LeetCode #151 | Difficulty: Medium | Pattern: String Reversal**

Given a string `s`, reverse the order of the words. Remove leading/trailing spaces and reduce multiple spaces between words to a single space.

---

### 2. How to Recognize This Pattern

**Clues:**
- "Reverse the words" (not the characters of the whole string)
- Need to handle extra spaces
- Classic two-step approach: split on spaces + reverse the list

---

### 5. Optimal Approach

#### Pythonic approach (split + reverse):
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        words = s.split()    # splits on any whitespace, removes extra spaces
        words.reverse()      # reverses the list in-place
        return ' '.join(words)
```

**Python notes:**
- `s.split()` without arguments splits on any whitespace and removes empty strings from leading/trailing/multiple spaces.
- `' '.join(words)` joins words with a single space.

#### Manual Two Pointer approach (interview-preferred for O(1) space):
```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # Step 1: Trim and split
        words = s.split()
        # Step 2: Reverse using two pointers
        left, right = 0, len(words) - 1
        while left < right:
            words[left], words[right] = words[right], words[left]
            left += 1
            right -= 1
        return ' '.join(words)
```

#### Dry Run
```
s = "  hello world  "
words = ['hello', 'world']
After reverse: ['world', 'hello']
Result: "world hello" ✓
```

```
Time:  O(n)
Space: O(n) for word list
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Reverse word order" → split + reverse the list
- Multiple spaces are handled by `s.split()` automatically
- If asked for O(1) extra space (in a character array context): reverse whole string, then reverse each word individually

---

## Problem 31 — Reverse String

**LeetCode #344 | Difficulty: Easy | Pattern: String Reversal**

Reverse an array of characters in-place.

---

### 5. Optimal Two Pointer Approach

```python
class Solution:
    def reverseString(self, s: list[str]) -> None:
        left, right = 0, len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1
```

#### Dry Run
```
s = ['h','e','l','l','o']

left=0,right=4: swap → ['o','e','l','l','h']
left=1,right=3: swap → ['o','l','l','e','h']
left=2,right=2: stop

Result: ['o','l','l','e','h'] ✓
```

```
Time:  O(n)
Space: O(1)
```

---

## Problem 32 — Reverse Vowels of a String

**LeetCode #345 | Difficulty: Easy | Pattern: String Reversal (selective)**

Reverse only the vowels in a string.

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Two pointers from both ends. Skip non-vowels. When both pointers are at vowels, swap them.

```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        vowels = set('aeiouAEIOU')
        s = list(s)  # strings are immutable in Python; convert to list
        left, right = 0, len(s) - 1

        while left < right:
            while left < right and s[left] not in vowels:
                left += 1
            while left < right and s[right] not in vowels:
                right -= 1
            if left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1

        return ''.join(s)
```

**Python note:** Strings in Python are immutable. We convert to a list to allow in-place modification, then rejoin with `''.join(s)`.

#### Dry Run
```
s = "hello"
s = ['h','e','l','l','o']

left=0(h): not vowel → left=1
left=1(e): vowel → stop
right=4(o): vowel → stop
Swap: ['h','o','l','l','e'], left=2, right=3

left=2(l): not vowel → left=3
right=3(l): not vowel → right=2
left>=right → stop

Result: "holle" ✓
```

```
Time:  O(n)
Space: O(n) for list conversion
```

---

## Problem 33 — Reverse String II

**LeetCode #541 | Difficulty: Easy | Pattern: String Reversal**

Given string `s` and integer `k`, reverse the first `k` characters for every `2k` characters. If fewer than `k` characters remain, reverse them all. If between `k` and `2k` remain, reverse only the first `k`.

---

### 5. Optimal Two Pointer Approach

#### Core Idea
Iterate in steps of `2k`. For each chunk, reverse the first `k` characters (or fewer if at end).

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        s = list(s)
        n = len(s)

        for start in range(0, n, 2 * k):
            left = start
            right = min(start + k - 1, n - 1)  # don't go past end
            while left < right:
                s[left], s[right] = s[right], s[left]
                left += 1
                right -= 1

        return ''.join(s)
```

**Python note:** `range(0, n, 2*k)` generates 0, 2k, 4k, ... — the `step` parameter of `range`.

#### Dry Run
```
s = "abcdefg", k = 2
Chunks of 2k=4: [0,4), [4,8)

Chunk starting at 0: reverse s[0..1] → ['b','a','c','d','e','f','g']
Chunk starting at 4: reverse s[4..4] (min(4+2-1,6)=min(5,6)=5)
  → reverse s[4..5] → [...,'f','e','g']

Result: "bacdfeg" ✓
```

```
Time:  O(n)
Space: O(n) for list conversion
```

---

### 6. Interview Recognition Trick

**How I should recognize this in an interview:**
- "Reverse specific segments" → iterate in steps of `2k`, reverse first `k` of each chunk
- `min(start + k - 1, n - 1)` handles the edge case where fewer than k characters remain

---

---

# Two Pointer Pattern Cheat Sheet

## Pattern 1: Converging

```
left  = 0
right = n - 1

Use when:
- Array is sorted (or can be sorted)
- Finding pairs/triplets satisfying a condition
- Maximizing/minimizing something involving two endpoints

Move left when:
- sum < target (need larger value)
- left side is the bottleneck (container problem)

Move right when:
- sum > target (need smaller value)
- right side is the bottleneck

Both move when:
- Found the answer (triplets: skip duplicates, then both move)

Typical complexity: O(n) after sorting O(n log n)

Example problems:
Two Sum II, 3Sum, Container With Most Water, Boats to Save People
```

---

## Pattern 2: Fast & Slow

```
slow = head (or start of sequence)
fast = head (or start of sequence)

Use when:
- Detecting cycles in linked lists or sequences
- Finding middle of linked list
- Any "will this sequence repeat?" question

slow moves: 1 step per iteration
fast moves: 2 steps per iteration

Stop when:
- slow == fast (cycle detected / middle found)
- fast == None or fast.next == None (no cycle, end reached)

Typical complexity: O(n), O(1) space

Example problems:
Linked List Cycle, Happy Number, Find Duplicate, Middle of Linked List
```

---

## Pattern 3: Fixed Separation

```
slow = start
fast = start + k (k positions ahead)

Use when:
- Need kth element from end of linked list
- Need to delete specific node relative to end

Move: both advance simultaneously, maintaining fixed gap

When fast reaches None:
- slow is exactly k positions before the end

Typical complexity: O(n), O(1) space

Example problems:
Remove Nth Node From End, Middle of Linked List
```

---

## Pattern 4: In-place Array Modification

```
write = 0 (or 1)
read  = 0 (or 1)

Use when:
- Modify array in-place, return new length
- Remove elements, move elements, compress array
- Array may or may not be sorted

read: always advances
write: only advances when a valid element is placed

Typical complexity: O(n), O(1) space

Example problems:
Remove Duplicates, Remove Element, Move Zeroes, Sort Colors, String Compression
```

---

## Pattern 5: String Comparison with Backspaces

```
i = len(s) - 1  (right to left)
j = len(t) - 1  (right to left)

Use when:
- Strings contain "delete" characters
- Need to compare effective content
- O(1) space required

skip_s, skip_t track pending deletions
Advance left, decrement skip when character should be deleted

Typical complexity: O(n + m), O(1) space

Example problems:
Backspace String Compare
```

---

## Pattern 6: Expanding From Center

```
left  = center
right = center (odd) or center+1 (even)

Use when:
- Palindrome problems
- Need to find/count symmetric substrings
- Try all 2n-1 centers

Expand: left -= 1, right += 1 while s[left] == s[right]
Stop: when out of bounds or mismatch

Typical complexity: O(n²), O(1) space

Example problems:
Longest Palindromic Substring, Palindromic Substrings
```

---

## Pattern 7: String Reversal

```
left  = 0
right = n - 1 (or end of segment)

Use when:
- Reverse a string or portion of it
- Reverse word order
- Selectively reverse specific characters

Swap s[left] and s[right], move both inward

Typical complexity: O(n), O(1) space (O(n) if converting immutable string to list)

Example problems:
Reverse String, Reverse Vowels, Reverse Words, Reverse String II
```

---

---

# Most Important Problems to Master First

## Tier 1 — Must Know

| Problem | Why It Matters |
|---|---|
| **167. Two Sum II** | The canonical Two Pointer problem. All converging patterns derive from this. |
| **15. 3Sum** | Most common medium-level interview problem. Tests sorting, Two Pointers, duplicate handling all at once. |
| **11. Container With Most Water** | Tests greedy intuition: move the shorter side. Pure converging pointer logic. |
| **141. Linked List Cycle** | Fast & Slow in its simplest form. Foundational for all linked list problems. |
| **876. Middle of Linked List** | Building block for many other linked list problems. |
| **26. Remove Duplicates from Sorted Array** | Foundational read/write pointer pattern. Appears in many variations. |
| **75. Sort Colors** | Dutch National Flag — appears very frequently. Tests three-pointer partitioning. |
| **5. Longest Palindromic Substring** | Expand from center — appears in many palindrome variants. |

**Why these are Tier 1:** Each one introduces a core pattern that is immediately applicable to 3–5 other problems. If you can solve these from scratch, the rest follow naturally.

---

## Tier 2 — Important

| Problem | Why It Matters |
|---|---|
| **16. 3Sum Closest** | Direct extension of 3Sum with "track closest" logic. |
| **18. 4Sum** | Generalizes 3Sum. Tests whether you can extend patterns. |
| **19. Remove Nth Node From End** | Classic dummy node + fixed separation. Edge cases make it tricky. |
| **283. Move Zeroes** | Very common easy problem. Tests read/write pattern. |
| **80. Remove Duplicates II** | Elegant generalization of #26. Shows pattern extensibility. |
| **287. Find the Duplicate Number** | Floyd's cycle detection on arrays — conceptually deep. |
| **977. Squares of a Sorted Array** | Fill-from-back pattern. Common in interviews. |
| **344. Reverse String** | Simplest reversal; every interview template starts here. |
| **647. Palindromic Substrings** | Count variant of #5; tests expand-from-center with counting. |
| **844. Backspace String Compare** | Tricky right-to-left scanning with skip counters. |

---

## Tier 3 — Advanced / Variations

| Problem | Why It Matters |
|---|---|
| **259. 3Sum Smaller** | Count triplets, not list them. "Count at once" insight is non-obvious. |
| **202. Happy Number** | Cycle detection on implicit sequences, not linked lists. |
| **2095. Delete Middle Node** | Variation of #876 with deletion. |
| **392. Is Subsequence** | Same-direction two pointers on two strings. |
| **349. Intersection of Two Arrays** | Two-pointer merge on sorted arrays. |
| **443. String Compression** | Multi-digit count handling makes it tricky. |
| **881. Boats to Save People** | Greedy + converging. The "heaviest always boards" insight. |
| **151. Reverse Words** | String manipulation + split awareness. |
| **345. Reverse Vowels** | Selective reversal with inner while loops. |
| **541. Reverse String II** | Segment reversal with boundary handling. |
| **905. Sort Array By Parity** | Simple partition; good warm-up problem. |
| **2337. Move Pieces** | Two-string comparison with movement constraints. |
| **2938. Separate Black/White Balls** | Bubble-sort counting insight. |

---

## Final Summary — The Core Intuition

```
Ask yourself these 4 questions for any new problem:

1. Is it a Two Pointer problem?
   → Do I need to find a pair/triplet satisfying a condition?
   → Do I need to modify an array in-place?
   → Do I need to detect a cycle or find a midpoint?
   → Do I need to expand around a center?

2. Which pattern is it?
   → Sorted array + pair/triplet  → Converging
   → Linked list cycle/middle     → Fast & Slow
   → Nth from end                 → Fixed Separation
   → Remove/move elements         → In-place (read/write)
   → Backspace in strings         → Right-to-left scan
   → Palindrome problems          → Expand from center
   → Reverse string/words         → Converging swap

3. What do the pointers represent?
   → Write out: "left = ..." and "right = ..." before coding

4. Why can I safely move each pointer?
   → Moving left ELIMINATES all right partners for old left
   → Moving right ELIMINATES all left partners for old right
   → This is only safe because of the SORTED (monotone) structure
   → Always justify: "moving this pointer cannot miss the answer because..."
```

---

*End of Two Pointers — Complete DSA Interview Guide*
