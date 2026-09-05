# Dynamic Programming Patterns — LeetCode Code Sheet

Memoization-first solutions in a Striver/TUF-style approach. No separate plain-recursion or tabulation solutions unless a simpler bottom-up solution is more natural.

## V. Dynamic Programming (DP) Patterns

## Pattern 27: DP — 1D Array (Fibonacci Style)

### 70. Climbing Stairs

**LC 70 — Climbing Stairs**

```python
# Plain recursion solution
class RecursiveSolution:
    def climbStairs(self, n: int) -> int:
        dp = [-1] * (n + 1)

        def solve(i):
            if i <= 1:
                return 1

            dp[i] = solve(i - 1) + solve(i - 2)
            return dp[i]

        return solve(n)

# Memoized DP solution (LeetCode submission)
class Solution:
    def climbStairs(self, n: int) -> int:
        dp = [-1] * (n + 1)

        def solve(i):
            if i <= 1:
                return 1
            if dp[i] != -1:
                return dp[i]

            dp[i] = solve(i - 1) + solve(i - 2)
            return dp[i]

        return solve(n)
```
### 91. Decode Ways

**LC 91 — Decode Ways**

```python
# Plain recursion solution
class RecursiveSolution:
    def numDecodings(self, s: str) -> int:
        n = len(s)
        dp = [-1] * (n + 1)

        def solve(i):
            if i == n:
                return 1
            if s[i] == '0':
                return 0

            ans = solve(i + 1)

            if i + 1 < n and 10 <= int(s[i:i + 2]) <= 26:
                ans += solve(i + 2)

            dp[i] = ans
            return dp[i]

        return solve(0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def numDecodings(self, s: str) -> int:
        n = len(s)
        dp = [-1] * (n + 1)

        def solve(i):
            if i == n:
                return 1
            if s[i] == '0':
                return 0
            if dp[i] != -1:
                return dp[i]

            ans = solve(i + 1)

            if i + 1 < n and 10 <= int(s[i:i + 2]) <= 26:
                ans += solve(i + 2)

            dp[i] = ans
            return dp[i]

        return solve(0)
```
### 198. House Robber

**LC 198 — House Robber**

```python
# Plain recursion solution
class RecursiveSolution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [-1] * n

        def solve(i):
            if i >= n:
                return 0

            take = nums[i] + solve(i + 2)
            skip = solve(i + 1)

            dp[i] = max(take, skip)
            return dp[i]

        return solve(0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def rob(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [-1] * n

        def solve(i):
            if i >= n:
                return 0
            if dp[i] != -1:
                return dp[i]

            take = nums[i] + solve(i + 2)
            skip = solve(i + 1)

            dp[i] = max(take, skip)
            return dp[i]

        return solve(0)
```
### 213. House Robber II

**LC 213 — House Robber II**

```python
# Plain recursion solution
class RecursiveSolution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        def rob_line(arr):
            dp = [-1] * len(arr)

            def solve(i):
                if i >= len(arr):
                    return 0

                dp[i] = max(
                    arr[i] + solve(i + 2),
                    solve(i + 1)
                )
                return dp[i]

            return solve(0)

        return max(rob_line(nums[:-1]), rob_line(nums[1:]))

# Memoized DP solution (LeetCode submission)
class Solution:
    def rob(self, nums: List[int]) -> int:
        if len(nums) == 1:
            return nums[0]

        def rob_line(arr):
            dp = [-1] * len(arr)

            def solve(i):
                if i >= len(arr):
                    return 0
                if dp[i] != -1:
                    return dp[i]

                dp[i] = max(
                    arr[i] + solve(i + 2),
                    solve(i + 1)
                )
                return dp[i]

            return solve(0)

        return max(rob_line(nums[:-1]), rob_line(nums[1:]))
```
### 337. House Robber III

**LC 337 — House Robber III**

```python
# Plain recursion solution
class RecursiveSolution:
    def rob(self, root: Optional[TreeNode]) -> int:
        dp = {}

        def solve(node):
            if not node:
                return 0

            take = node.val

            if node.left:
                take += solve(node.left.left)
                take += solve(node.left.right)

            if node.right:
                take += solve(node.right.left)
                take += solve(node.right.right)

            skip = solve(node.left) + solve(node.right)

            dp[node] = max(take, skip)
            return dp[node]

        return solve(root)

# Memoized DP solution (LeetCode submission)
class Solution:
    def rob(self, root: Optional[TreeNode]) -> int:
        dp = {}

        def solve(node):
            if not node:
                return 0
            if node in dp:
                return dp[node]

            take = node.val

            if node.left:
                take += solve(node.left.left)
                take += solve(node.left.right)

            if node.right:
                take += solve(node.right.left)
                take += solve(node.right.right)

            skip = solve(node.left) + solve(node.right)

            dp[node] = max(take, skip)
            return dp[node]

        return solve(root)
```
### 509. Fibonacci Number

**LC 509 — Fibonacci Number**

```python
# Plain recursion solution
class RecursiveSolution:
    def fib(self, n: int) -> int:
        dp = [-1] * (n + 1)

        def solve(i):
            if i <= 1:
                return i

            dp[i] = solve(i - 1) + solve(i - 2)
            return dp[i]

        return solve(n)

# Memoized DP solution (LeetCode submission)
class Solution:
    def fib(self, n: int) -> int:
        dp = [-1] * (n + 1)

        def solve(i):
            if i <= 1:
                return i
            if dp[i] != -1:
                return dp[i]

            dp[i] = solve(i - 1) + solve(i - 2)
            return dp[i]

        return solve(n)
```
### 740. Delete and Earn

**LC 740 — Delete and Earn**

```python
# Plain recursion solution
class RecursiveSolution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        maximum = max(nums)
        points = [0] * (maximum + 1)

        for x in nums:
            points[x] += x

        dp = [-1] * (maximum + 1)

        def solve(i):
            if i <= 0:
                return 0

            dp[i] = max(
                points[i] + solve(i - 2),
                solve(i - 1)
            )
            return dp[i]

        return solve(maximum)

# Memoized DP solution (LeetCode submission)
class Solution:
    def deleteAndEarn(self, nums: List[int]) -> int:
        maximum = max(nums)
        points = [0] * (maximum + 1)

        for x in nums:
            points[x] += x

        dp = [-1] * (maximum + 1)

        def solve(i):
            if i <= 0:
                return 0
            if dp[i] != -1:
                return dp[i]

            dp[i] = max(
                points[i] + solve(i - 2),
                solve(i - 1)
            )
            return dp[i]

        return solve(maximum)
```
### 746. Min Cost Climbing Stairs

**LC 746 — Min Cost Climbing Stairs**

```python
# Plain recursion solution
class RecursiveSolution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [-1] * (n + 1)

        def solve(i):
            if i >= n:
                return 0

            dp[i] = cost[i] + min(solve(i + 1), solve(i + 2))
            return dp[i]

        return min(solve(0), solve(1))

# Memoized DP solution (LeetCode submission)
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        n = len(cost)
        dp = [-1] * (n + 1)

        def solve(i):
            if i >= n:
                return 0
            if dp[i] != -1:
                return dp[i]

            dp[i] = cost[i] + min(solve(i + 1), solve(i + 2))
            return dp[i]

        return min(solve(0), solve(1))
```
## Pattern 28: DP — 1D Array (Kadane's Algorithm for Max/Min Subarray)

### 53. Maximum Subarray

**LC 53 — Maximum Subarray**

```python
# Plain recursion solution
class RecursiveSolution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [-1] * len(nums)

        def solve(i):
            if i == 0:
                return nums[0]

            dp[i] = max(nums[i], nums[i] + solve(i - 1))
            return dp[i]

        return max(solve(i) for i in range(len(nums)))

# Memoized DP solution (LeetCode submission)
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        dp = [-1] * len(nums)

        def solve(i):
            if i == 0:
                return nums[0]
            if dp[i] != -1:
                return dp[i]

            dp[i] = max(nums[i], nums[i] + solve(i - 1))
            return dp[i]

        return max(solve(i) for i in range(len(nums)))
```
## Pattern 29: DP — 1D Array (Coin Change / Unbounded Knapsack Style)

### 322. Coin Change

**LC 322 — Coin Change**

```python
# Plain recursion solution
class RecursiveSolution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [-1] * (amount + 1)
        dp[0] = 0

        def solve(x):
            if x < 0:
                return float('inf')

            dp[x] = min(1 + solve(x - coin) for coin in coins)
            return dp[x]

        ans = solve(amount)
        return -1 if ans == float('inf') else ans

# Memoized DP solution (LeetCode submission)
class Solution:
    def coinChange(self, coins: List[int], amount: int) -> int:
        dp = [-1] * (amount + 1)
        dp[0] = 0

        def solve(x):
            if x < 0:
                return float('inf')
            if dp[x] != -1:
                return dp[x]

            dp[x] = min(1 + solve(x - coin) for coin in coins)
            return dp[x]

        ans = solve(amount)
        return -1 if ans == float('inf') else ans
```
### 377. Combination Sum IV

**LC 377 — Combination Sum IV**

```python
# Plain recursion solution
class RecursiveSolution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [-1] * (target + 1)
        dp[0] = 1

        def solve(x):

            dp[x] = sum(
                solve(x - num)
                for num in nums
                if num <= x
            )
            return dp[x]

        return solve(target)

# Memoized DP solution (LeetCode submission)
class Solution:
    def combinationSum4(self, nums: List[int], target: int) -> int:
        dp = [-1] * (target + 1)
        dp[0] = 1

        def solve(x):
            if dp[x] != -1:
                return dp[x]

            dp[x] = sum(
                solve(x - num)
                for num in nums
                if num <= x
            )
            return dp[x]

        return solve(target)
```
### 518. Coin Change II

**LC 518 — Coin Change II**

```python
# Plain recursion solution
class RecursiveSolution:
    def change(self, amount: int, coins: List[int]) -> int:
        n = len(coins)
        dp = [[-1] * (amount + 1) for _ in range(n)]

        def solve(i, total):
            if total == 0:
                return 1
            if i == n:
                return 0

            ans = solve(i + 1, total)

            if coins[i] <= total:
                ans += solve(i, total - coins[i])

            dp[i][total] = ans
            return ans

        return solve(0, amount)

# Memoized DP solution (LeetCode submission)
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        n = len(coins)
        dp = [[-1] * (amount + 1) for _ in range(n)]

        def solve(i, total):
            if total == 0:
                return 1
            if i == n:
                return 0
            if dp[i][total] != -1:
                return dp[i][total]

            ans = solve(i + 1, total)

            if coins[i] <= total:
                ans += solve(i, total - coins[i])

            dp[i][total] = ans
            return ans

        return solve(0, amount)
```
## Pattern 30: DP — 1D Array (0/1 Knapsack Subset Sum Style)

### 416. Partition Equal Subset Sum

**LC 416 — Partition Equal Subset Sum**

```python
# Plain recursion solution
class RecursiveSolution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)

        if total % 2:
            return False

        target = total // 2
        dp = {}

        def solve(i, target):
            if target == 0:
                return True
            if i == len(nums) or target < 0:
                return False

            dp[(i, target)] = (
                solve(i + 1, target - nums[i])
                or solve(i + 1, target)
            )

            return dp[(i, target)]

        return solve(0, target)

# Memoized DP solution (LeetCode submission)
class Solution:
    def canPartition(self, nums: List[int]) -> bool:
        total = sum(nums)

        if total % 2:
            return False

        target = total // 2
        dp = {}

        def solve(i, target):
            if target == 0:
                return True
            if i == len(nums) or target < 0:
                return False
            if (i, target) in dp:
                return dp[(i, target)]

            dp[(i, target)] = (
                solve(i + 1, target - nums[i])
                or solve(i + 1, target)
            )

            return dp[(i, target)]

        return solve(0, target)
```
### 494. Target Sum

**LC 494 — Target Sum**

```python
# Plain recursion solution
class RecursiveSolution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        dp = {}

        def solve(i, total):
            if i == len(nums):
                return 1 if total == target else 0


            dp[(i, total)] = (
                solve(i + 1, total + nums[i])
                + solve(i + 1, total - nums[i])
            )

            return dp[(i, total)]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def findTargetSumWays(self, nums: List[int], target: int) -> int:
        dp = {}

        def solve(i, total):
            if i == len(nums):
                return 1 if total == target else 0

            if (i, total) in dp:
                return dp[(i, total)]

            dp[(i, total)] = (
                solve(i + 1, total + nums[i])
                + solve(i + 1, total - nums[i])
            )

            return dp[(i, total)]

        return solve(0, 0)
```
## Pattern 31: DP — 1D Array (Word Break Style)

### 139. Word Break

**LC 139 — Word Break**

```python
# Plain recursion solution
class RecursiveSolution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        words = set(wordDict)
        dp = {}

        def solve(i):
            if i == len(s):
                return True

            for j in range(i + 1, len(s) + 1):
                if s[i:j] in words and solve(j):
                    dp[i] = True
                    return True

            dp[i] = False
            return False

        return solve(0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        words = set(wordDict)
        dp = {}

        def solve(i):
            if i == len(s):
                return True
            if i in dp:
                return dp[i]

            for j in range(i + 1, len(s) + 1):
                if s[i:j] in words and solve(j):
                    dp[i] = True
                    return True

            dp[i] = False
            return False

        return solve(0)
```
### 140. Word Break II

**LC 140 — Word Break II**

```python
# Plain recursion solution
class RecursiveSolution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        words = set(wordDict)
        dp = {}

        def solve(i):
            if i == len(s):
                return [""]


            ans = []

            for j in range(i + 1, len(s) + 1):
                word = s[i:j]

                if word in words:
                    for rest in solve(j):
                        ans.append(word if not rest else word + " " + rest)

            dp[i] = ans
            return ans

        return solve(0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> List[str]:
        words = set(wordDict)
        dp = {}

        def solve(i):
            if i == len(s):
                return [""]

            if i in dp:
                return dp[i]

            ans = []

            for j in range(i + 1, len(s) + 1):
                word = s[i:j]

                if word in words:
                    for rest in solve(j):
                        ans.append(word if not rest else word + " " + rest)

            dp[i] = ans
            return ans

        return solve(0)
```
## Pattern 32: DP — 2D Array (Longest Common Subsequence — LCS)

### 583. Delete Operation for Two Strings

**LC 583 — Delete Operation for Two Strings**

```python
# Plain recursion solution
class RecursiveSolution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        dp = [[-1] * (m + 1) for _ in range(n + 1)]

        def lcs(i, j):
            if i == n or j == m:
                return 0

            if word1[i] == word2[j]:
                dp[i][j] = 1 + lcs(i + 1, j + 1)
            else:
                dp[i][j] = max(lcs(i + 1, j), lcs(i, j + 1))

            return dp[i][j]

        return n + m - 2 * lcs(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        dp = [[-1] * (m + 1) for _ in range(n + 1)]

        def lcs(i, j):
            if i == n or j == m:
                return 0
            if dp[i][j] != -1:
                return dp[i][j]

            if word1[i] == word2[j]:
                dp[i][j] = 1 + lcs(i + 1, j + 1)
            else:
                dp[i][j] = max(lcs(i + 1, j), lcs(i, j + 1))

            return dp[i][j]

        return n + m - 2 * lcs(0, 0)
```
### 1143. Longest Common Subsequence

**LC 1143 — Longest Common Subsequence**

```python
# Plain recursion solution
class RecursiveSolution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n, m = len(text1), len(text2)
        dp = [[-1] * m for _ in range(n)]

        def solve(i, j):
            if i == n or j == m:
                return 0

            if text1[i] == text2[j]:
                dp[i][j] = 1 + solve(i + 1, j + 1)
            else:
                dp[i][j] = max(
                    solve(i + 1, j),
                    solve(i, j + 1)
                )

            return dp[i][j]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def longestCommonSubsequence(self, text1: str, text2: str) -> int:
        n, m = len(text1), len(text2)
        dp = [[-1] * m for _ in range(n)]

        def solve(i, j):
            if i == n or j == m:
                return 0
            if dp[i][j] != -1:
                return dp[i][j]

            if text1[i] == text2[j]:
                dp[i][j] = 1 + solve(i + 1, j + 1)
            else:
                dp[i][j] = max(
                    solve(i + 1, j),
                    solve(i, j + 1)
                )

            return dp[i][j]

        return solve(0, 0)
```
## Pattern 33: DP — 2D Array (Edit Distance / Levenshtein Distance)

### 72. Edit Distance

**LC 72 — Edit Distance**

```python
# Plain recursion solution
class RecursiveSolution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        dp = [[-1] * m for _ in range(n)]

        def solve(i, j):
            if i == n:
                return m - j
            if j == m:
                return n - i


            if word1[i] == word2[j]:
                dp[i][j] = solve(i + 1, j + 1)
            else:
                insert = solve(i, j + 1)
                delete = solve(i + 1, j)
                replace = solve(i + 1, j + 1)

                dp[i][j] = 1 + min(insert, delete, replace)

            return dp[i][j]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        n, m = len(word1), len(word2)
        dp = [[-1] * m for _ in range(n)]

        def solve(i, j):
            if i == n:
                return m - j
            if j == m:
                return n - i

            if dp[i][j] != -1:
                return dp[i][j]

            if word1[i] == word2[j]:
                dp[i][j] = solve(i + 1, j + 1)
            else:
                insert = solve(i, j + 1)
                delete = solve(i + 1, j)
                replace = solve(i + 1, j + 1)

                dp[i][j] = 1 + min(insert, delete, replace)

            return dp[i][j]

        return solve(0, 0)
```
## Pattern 34: DP — 2D Array (Unique Paths on Grid)

### 62. Unique Paths

**LC 62 — Unique Paths**

```python
# Plain recursion solution
class RecursiveSolution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r == m - 1 and c == n - 1:
                return 1
            if r >= m or c >= n:
                return 0

            dp[r][c] = solve(r + 1, c) + solve(r, c + 1)
            return dp[r][c]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r == m - 1 and c == n - 1:
                return 1
            if r >= m or c >= n:
                return 0
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = solve(r + 1, c) + solve(r, c + 1)
            return dp[r][c]

        return solve(0, 0)
```
### 63. Unique Paths II

**LC 63 — Unique Paths II**

```python
# Plain recursion solution
class RecursiveSolution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r >= m or c >= n or obstacleGrid[r][c] == 1:
                return 0
            if r == m - 1 and c == n - 1:
                return 1

            dp[r][c] = solve(r + 1, c) + solve(r, c + 1)
            return dp[r][c]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m, n = len(obstacleGrid), len(obstacleGrid[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r >= m or c >= n or obstacleGrid[r][c] == 1:
                return 0
            if r == m - 1 and c == n - 1:
                return 1
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = solve(r + 1, c) + solve(r, c + 1)
            return dp[r][c]

        return solve(0, 0)
```
### 64. Minimum Path Sum

**LC 64 — Minimum Path Sum**

```python
# Plain recursion solution
class RecursiveSolution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r == m - 1 and c == n - 1:
                return grid[r][c]
            if r >= m or c >= n:
                return float('inf')

            dp[r][c] = grid[r][c] + min(
                solve(r + 1, c),
                solve(r, c + 1)
            )

            return dp[r][c]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        m, n = len(grid), len(grid[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r == m - 1 and c == n - 1:
                return grid[r][c]
            if r >= m or c >= n:
                return float('inf')
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = grid[r][c] + min(
                solve(r + 1, c),
                solve(r, c + 1)
            )

            return dp[r][c]

        return solve(0, 0)
```
### 120. Triangle

**LC 120 — Triangle**

```python
# Plain recursion solution
class RecursiveSolution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        dp = {}

        def solve(r, c):
            if r == n - 1:
                return triangle[r][c]

            dp[(r, c)] = triangle[r][c] + min(
                solve(r + 1, c),
                solve(r + 1, c + 1)
            )

            return dp[(r, c)]

        return solve(0, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def minimumTotal(self, triangle: List[List[int]]) -> int:
        n = len(triangle)
        dp = {}

        def solve(r, c):
            if r == n - 1:
                return triangle[r][c]
            if (r, c) in dp:
                return dp[(r, c)]

            dp[(r, c)] = triangle[r][c] + min(
                solve(r + 1, c),
                solve(r + 1, c + 1)
            )

            return dp[(r, c)]

        return solve(0, 0)
```
### 221. Maximal Square

**LC 221 — Maximal Square**

```python
# Plain recursion solution
class RecursiveSolution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[-1] * n for _ in range(m)]
        best = 0

        def solve(r, c):
            nonlocal best

            if r >= m or c >= n or matrix[r][c] == '0':
                return 0

            dp[r][c] = 1 + min(
                solve(r + 1, c),
                solve(r, c + 1),
                solve(r + 1, c + 1)
            )

            best = max(best, dp[r][c])
            return dp[r][c]

        solve(0, 0)
        return best * best

# Memoized DP solution (LeetCode submission)
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[-1] * n for _ in range(m)]
        best = 0

        def solve(r, c):
            nonlocal best

            if r >= m or c >= n or matrix[r][c] == '0':
                return 0
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = 1 + min(
                solve(r + 1, c),
                solve(r, c + 1),
                solve(r + 1, c + 1)
            )

            best = max(best, dp[r][c])
            return dp[r][c]

        solve(0, 0)
        return best * best
```
### 931. Minimum Falling Path Sum

**LC 931 — Minimum Falling Path Sum**

```python
# Plain recursion solution
class RecursiveSolution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        dp = [[-1] * n for _ in range(n)]

        def solve(r, c):
            if c < 0 or c >= n:
                return float('inf')
            if r == n - 1:
                return matrix[r][c]

            dp[r][c] = matrix[r][c] + min(
                solve(r + 1, c - 1),
                solve(r + 1, c),
                solve(r + 1, c + 1)
            )

            return dp[r][c]

        return min(solve(0, c) for c in range(n))

# Memoized DP solution (LeetCode submission)
class Solution:
    def minFallingPathSum(self, matrix: List[List[int]]) -> int:
        n = len(matrix)
        dp = [[-1] * n for _ in range(n)]

        def solve(r, c):
            if c < 0 or c >= n:
                return float('inf')
            if r == n - 1:
                return matrix[r][c]
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = matrix[r][c] + min(
                solve(r + 1, c - 1),
                solve(r + 1, c),
                solve(r + 1, c + 1)
            )

            return dp[r][c]

        return min(solve(0, c) for c in range(n))
```
### 1277. Count Square Submatrices with All Ones

**LC 1277 — Count Square Submatrices with All Ones**

```python
# Plain recursion solution
class RecursiveSolution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r >= m or c >= n or matrix[r][c] == 0:
                return 0

            dp[r][c] = 1 + min(
                solve(r + 1, c),
                solve(r, c + 1),
                solve(r + 1, c + 1)
            )

            return dp[r][c]

        ans = 0

        for r in range(m):
            for c in range(n):
                ans += solve(r, c)

        return ans

# Memoized DP solution (LeetCode submission)
class Solution:
    def countSquares(self, matrix: List[List[int]]) -> int:
        m, n = len(matrix), len(matrix[0])
        dp = [[-1] * n for _ in range(m)]

        def solve(r, c):
            if r >= m or c >= n or matrix[r][c] == 0:
                return 0
            if dp[r][c] != -1:
                return dp[r][c]

            dp[r][c] = 1 + min(
                solve(r + 1, c),
                solve(r, c + 1),
                solve(r + 1, c + 1)
            )

            return dp[r][c]

        ans = 0

        for r in range(m):
            for c in range(n):
                ans += solve(r, c)

        return ans
```
## Pattern 35: DP — Interval DP

### 312. Burst Balloons

**LC 312 — Burst Balloons**

```python
# Plain recursion solution
class RecursiveSolution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        dp = {}

        def solve(l, r):
            if l > r:
                return 0

            ans = 0

            for k in range(l, r + 1):
                coins = (
                    nums[l - 1] * nums[k] * nums[r + 1]
                    + solve(l, k - 1)
                    + solve(k + 1, r)
                )
                ans = max(ans, coins)

            dp[(l, r)] = ans
            return ans

        return solve(1, n - 2)

# Memoized DP solution (LeetCode submission)
class Solution:
    def maxCoins(self, nums: List[int]) -> int:
        nums = [1] + nums + [1]
        n = len(nums)
        dp = {}

        def solve(l, r):
            if l > r:
                return 0
            if (l, r) in dp:
                return dp[(l, r)]

            ans = 0

            for k in range(l, r + 1):
                coins = (
                    nums[l - 1] * nums[k] * nums[r + 1]
                    + solve(l, k - 1)
                    + solve(k + 1, r)
                )
                ans = max(ans, coins)

            dp[(l, r)] = ans
            return ans

        return solve(1, n - 2)
```
### 546. Remove Boxes

**LC 546 — Remove Boxes**

```python
# Plain recursion solution
class RecursiveSolution:
    def removeBoxes(self, boxes: List[int]) -> int:

        def solve(l, r, k):
            if l > r:
                return 0

            while l < r and boxes[l] == boxes[l + 1]:
                l += 1
                k += 1

            ans = (k + 1) ** 2 + solve(l + 1, r, 0)

            for i in range(l + 1, r + 1):
                if boxes[i] == boxes[l]:
                    ans = max(
                        ans,
                        solve(l + 1, i - 1, 0)
                        + solve(i, r, k + 1)
                    )

            return ans

        return solve(0, len(boxes) - 1, 0)

# Memoized DP solution (LeetCode submission)
class Solution:
    def removeBoxes(self, boxes: List[int]) -> int:
        from functools import lru_cache

        @lru_cache(None)
        def solve(l, r, k):
            if l > r:
                return 0

            while l < r and boxes[l] == boxes[l + 1]:
                l += 1
                k += 1

            ans = (k + 1) ** 2 + solve(l + 1, r, 0)

            for i in range(l + 1, r + 1):
                if boxes[i] == boxes[l]:
                    ans = max(
                        ans,
                        solve(l + 1, i - 1, 0)
                        + solve(i, r, k + 1)
                    )

            return ans

        return solve(0, len(boxes) - 1, 0)
```
## Pattern 36: DP — Catalan Numbers

### 95. Unique Binary Search Trees II

**LC 95 — Unique Binary Search Trees II**

```python
# Plain recursion solution
class RecursiveSolution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:

        def solve(l, r):
            if l > r:
                return (None,)

            ans = []

            for root_val in range(l, r + 1):
                left_trees = solve(l, root_val - 1)
                right_trees = solve(root_val + 1, r)

                for left in left_trees:
                    for right in right_trees:
                        root = TreeNode(root_val)
                        root.left = left
                        root.right = right
                        ans.append(root)

            return tuple(ans)

        return list(solve(1, n))

# Memoized DP solution (LeetCode submission)
class Solution:
    def generateTrees(self, n: int) -> List[Optional[TreeNode]]:
        from functools import lru_cache

        @lru_cache(None)
        def solve(l, r):
            if l > r:
                return (None,)

            ans = []

            for root_val in range(l, r + 1):
                left_trees = solve(l, root_val - 1)
                right_trees = solve(root_val + 1, r)

                for left in left_trees:
                    for right in right_trees:
                        root = TreeNode(root_val)
                        root.left = left
                        root.right = right
                        ans.append(root)

            return tuple(ans)

        return list(solve(1, n))
```
### 96. Unique Binary Search Trees

**LC 96 — Unique Binary Search Trees**

```python
# Plain recursion solution
class RecursiveSolution:
    def numTrees(self, n: int) -> int:
        dp = [-1] * (n + 1)
        dp[0] = 1
        if n >= 1:
            dp[1] = 1

        def solve(nodes):

            ans = 0

            for root in range(nodes):
                ans += solve(root) * solve(nodes - 1 - root)

            dp[nodes] = ans
            return ans

        return solve(n)

# Memoized DP solution (LeetCode submission)
class Solution:
    def numTrees(self, n: int) -> int:
        dp = [-1] * (n + 1)
        dp[0] = 1
        if n >= 1:
            dp[1] = 1

        def solve(nodes):
            if dp[nodes] != -1:
                return dp[nodes]

            ans = 0

            for root in range(nodes):
                ans += solve(root) * solve(nodes - 1 - root)

            dp[nodes] = ans
            return ans

        return solve(n)
```
### 241. Different Ways to Add Parentheses

**LC 241 — Different Ways to Add Parentheses**

```python
# Plain recursion solution
class RecursiveSolution:
    def diffWaysToCompute(self, expression: str) -> List[int]:

        def solve(expr):
            ans = []

            for i, ch in enumerate(expr):
                if ch in "+-*":
                    left = solve(expr[:i])
                    right = solve(expr[i + 1:])

                    for a in left:
                        for b in right:
                            if ch == "+":
                                ans.append(a + b)
                            elif ch == "-":
                                ans.append(a - b)
                            else:
                                ans.append(a * b)

            if not ans:
                ans.append(int(expr))

            return tuple(ans)

        return list(solve(expression))

# Memoized DP solution (LeetCode submission)
class Solution:
    def diffWaysToCompute(self, expression: str) -> List[int]:
        from functools import lru_cache

        @lru_cache(None)
        def solve(expr):
            ans = []

            for i, ch in enumerate(expr):
                if ch in "+-*":
                    left = solve(expr[:i])
                    right = solve(expr[i + 1:])

                    for a in left:
                        for b in right:
                            if ch == "+":
                                ans.append(a + b)
                            elif ch == "-":
                                ans.append(a - b)
                            else:
                                ans.append(a * b)

            if not ans:
                ans.append(int(expr))

            return tuple(ans)

        return list(solve(expression))
```
## Pattern 37: DP — Longest Increasing Subsequence (LIS)

### 300. Longest Increasing Subsequence

**LC 300 — Longest Increasing Subsequence**

```python
# Plain recursion solution
class RecursiveSolution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [-1] * n

        def solve(i):

            best = 1

            for j in range(i + 1, n):
                if nums[j] > nums[i]:
                    best = max(best, 1 + solve(j))

            dp[i] = best
            return best

        return max(solve(i) for i in range(n))

# Memoized DP solution (LeetCode submission)
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        n = len(nums)
        dp = [-1] * n

        def solve(i):
            if dp[i] != -1:
                return dp[i]

            best = 1

            for j in range(i + 1, n):
                if nums[j] > nums[i]:
                    best = max(best, 1 + solve(j))

            dp[i] = best
            return best

        return max(solve(i) for i in range(n))
```
### 354. Russian Doll Envelopes

**LC 354 — Russian Doll Envelopes**

```python
# Plain recursion solution
class RecursiveSolution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        import bisect

        envelopes.sort(key=lambda x: (x[0], -x[1]))

        lis = []

        for _, h in envelopes:
            i = bisect.bisect_left(lis, h)

            if i == len(lis):
                lis.append(h)
            else:
                lis[i] = h

        return len(lis)

# Memoized DP solution (LeetCode submission)
class Solution:
    def maxEnvelopes(self, envelopes: List[List[int]]) -> int:
        import bisect

        envelopes.sort(key=lambda x: (x[0], -x[1]))

        lis = []

        for _, h in envelopes:
            i = bisect.bisect_left(lis, h)

            if i == len(lis):
                lis.append(h)
            else:
                lis[i] = h

        return len(lis)
```
### 1671. Minimum Number of Removals to Make Mountain Array

**LC 1671 — Minimum Number of Removals to Make Mountain Array**

```python
# Plain recursion solution
class RecursiveSolution:
    def minimumMountainRemovals(self, nums: List[int]) -> int:
        n = len(nums)

        left = [1] * n
        right = [1] * n

        for i in range(n):
            for j in range(i):
                if nums[j] < nums[i]:
                    left[i] = max(left[i], left[j] + 1)

        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if nums[j] < nums[i]:
                    right[i] = max(right[i], right[j] + 1)

        best = 0

        for i in range(1, n - 1):
            if left[i] > 1 and right[i] > 1:
                best = max(best, left[i] + right[i] - 1)

        return n - best

# Memoized DP solution (LeetCode submission)
class Solution:
    def minimumMountainRemovals(self, nums: List[int]) -> int:
        n = len(nums)

        left = [1] * n
        right = [1] * n

        for i in range(n):
            for j in range(i):
                if nums[j] < nums[i]:
                    left[i] = max(left[i], left[j] + 1)

        for i in range(n - 1, -1, -1):
            for j in range(i + 1, n):
                if nums[j] < nums[i]:
                    right[i] = max(right[i], right[j] + 1)

        best = 0

        for i in range(1, n - 1):
            if left[i] > 1 and right[i] > 1:
                best = max(best, left[i] + right[i] - 1)

        return n - best
```
### 2407. Longest Increasing Subsequence II

**LC 2407 — Longest Increasing Subsequence II**

```python
# Plain recursion solution
class RecursiveSolution:
    def lengthOfLIS(self, nums: List[int], k: int) -> int:
        size = max(nums) + 1
        tree = [0] * (4 * size)

        def query(node, l, r, ql, qr):
            if ql > r or qr < l:
                return 0

            if ql <= l and r <= qr:
                return tree[node]

            mid = (l + r) // 2

            return max(
                query(node * 2, l, mid, ql, qr),
                query(node * 2 + 1, mid + 1, r, ql, qr)
            )

        def update(node, l, r, pos, value):
            if l == r:
                tree[node] = max(tree[node], value)
                return

            mid = (l + r) // 2

            if pos <= mid:
                update(node * 2, l, mid, pos, value)
            else:
                update(node * 2 + 1, mid + 1, r, pos, value)

            tree[node] = max(tree[node * 2], tree[node * 2 + 1])

        ans = 0

        for x in nums:
            best = query(1, 1, size, max(1, x - k), x - 1) + 1
            update(1, 1, size, x, best)
            ans = max(ans, best)

        return ans

# Memoized DP solution (LeetCode submission)
class Solution:
    def lengthOfLIS(self, nums: List[int], k: int) -> int:
        size = max(nums) + 1
        tree = [0] * (4 * size)

        def query(node, l, r, ql, qr):
            if ql > r or qr < l:
                return 0

            if ql <= l and r <= qr:
                return tree[node]

            mid = (l + r) // 2

            return max(
                query(node * 2, l, mid, ql, qr),
                query(node * 2 + 1, mid + 1, r, ql, qr)
            )

        def update(node, l, r, pos, value):
            if l == r:
                tree[node] = max(tree[node], value)
                return

            mid = (l + r) // 2

            if pos <= mid:
                update(node * 2, l, mid, pos, value)
            else:
                update(node * 2 + 1, mid + 1, r, pos, value)

            tree[node] = max(tree[node * 2], tree[node * 2 + 1])

        ans = 0

        for x in nums:
            best = query(1, 1, size, max(1, x - k), x - 1) + 1
            update(1, 1, size, x, best)
            ans = max(ans, best)

        return ans
```
