# Sliding Window Patterns — LeetCode Code Sheet

## Pattern 8: Sliding Window — Fixed Size

### 346. Moving Average from Data Stream

**LC 346 — Moving Average from Data Stream**

```python
class MovingAverage:

    def __init__(self, size: int):
        self.size = size
        self.queue = []
        self.total = 0

    def next(self, val: int) -> float:
        self.queue.append(val)
        self.total += val

        if len(self.queue) > self.size:
            self.total -= self.queue.pop(0)

        return self.total / len(self.queue)

```
### 643. Maximum Average Subarray I

**LC 643 — Maximum Average Subarray I**

```python
class Solution:
def findMaxAverage(self, nums: List[int], k: int) -> float:
window = sum(nums[:k])
ans = window

        for i in range(k, len(nums)):
            window += nums[i] - nums[i - k]
            ans = max(ans, window)

        return ans / k

```
### 2985. Calculate Compressed Mean

**LC 2985 — Calculate Compressed Mean**

```python
class Solution:
def compressedMean(self, nums: List[int]) -> float:
nums.sort()

        n = len(nums)
        total = sum(nums)

        if n == 0:
            return 0.0

        return total / n

```
### 3254. Find the Power of K-Size Subarrays I

**LC 3254 — Find the Power of K-Size Subarrays I**

```python
class Solution:
def resultsArray(self, nums: List[int], k: int) -> List[int]:
ans = []

        for i in range(len(nums) - k + 1):
            ok = True

            for j in range(i + 1, i + k):
                if nums[j] != nums[j - 1] + 1:
                    ok = False
                    break

            ans.append(nums[i + k - 1] if ok else -1)

        return ans

```
### 3318. Find X-Sum of All K-Long Subarrays I

**LC 3318 — Find X-Sum of All K-Long Subarrays I**

```python
class Solution:
def findXSum(self, nums: List[int], k: int, x: int) -> List[int]:
ans = []

        for i in range(len(nums) - k + 1):
            freq = {}

            for j in range(i, i + k):
                freq[nums[j]] = freq.get(nums[j], 0) + 1

            top = sorted(freq.items(), key=lambda p: (p[1], p[0]), reverse=True)[:x]
            ans.append(sum(num * count for num, count in top))

        return ans

```
## Pattern 9: Sliding Window — Variable Size

### 3. Longest Substring Without Repeating Characters

**LC 3 — Longest Substring Without Repeating Characters**

```python
class Solution:
def lengthOfLongestSubstring(self, s: str) -> int:
seen = {}
l = 0
ans = 0

        for r, ch in enumerate(s):
            if ch in seen and seen[ch] >= l:
                l = seen[ch] + 1

            seen[ch] = r
            ans = max(ans, r - l + 1)

        return ans

```
### 76. Minimum Window Substring

**LC 76 — Minimum Window Substring**

```python
class Solution:
def minWindow(self, s: str, t: str) -> str:
need = {}
for ch in t:
need[ch] = need.get(ch, 0) + 1

        have = {}
        formed = 0
        required = len(need)

        l = 0
        best = ""

        for r, ch in enumerate(s):
            have[ch] = have.get(ch, 0) + 1

            if ch in need and have[ch] == need[ch]:
                formed += 1

            while formed == required:
                window = s[l:r + 1]

                if not best or len(window) < len(best):
                    best = window

                left = s[l]
                have[left] -= 1

                if left in need and have[left] < need[left]:
                    formed -= 1

                l += 1

        return best

```
### 209. Minimum Size Subarray Sum

**LC 209 — Minimum Size Subarray Sum**

```python
class Solution:
def minSubArrayLen(self, target: int, nums: List[int]) -> int:
l = 0
total = 0
ans = float('inf')

        for r, x in enumerate(nums):
            total += x

            while total >= target:
                ans = min(ans, r - l + 1)
                total -= nums[l]
                l += 1

        return 0 if ans == float('inf') else ans

```
### 219. Contains Duplicate II

**LC 219 — Contains Duplicate II**

```python
class Solution:
def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
seen = {}

        for i, x in enumerate(nums):
            if x in seen and i - seen[x] <= k:
                return True
            seen[x] = i

        return False

```
### 424. Longest Repeating Character Replacement

**LC 424 — Longest Repeating Character Replacement**

```python
class Solution:
def characterReplacement(self, s: str, k: int) -> int:
freq = {}
l = 0
max_freq = 0
ans = 0

        for r, ch in enumerate(s):
            freq[ch] = freq.get(ch, 0) + 1
            max_freq = max(max_freq, freq[ch])

            while (r - l + 1) - max_freq > k:
                freq[s[l]] -= 1
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 713. Subarray Product Less Than K

**LC 713 — Subarray Product Less Than K**

```python
class Solution:
def numSubarrayProductLessThanK(self, nums: List[int], k: int) -> int:
if k <= 1:
return 0

        l = 0
        product = 1
        ans = 0

        for r, x in enumerate(nums):
            product *= x

            while product >= k:
                product //= nums[l]
                l += 1

            ans += r - l + 1

        return ans

```
### 904. Fruit Into Baskets

**LC 904 — Fruit Into Baskets**

```python
class Solution:
def totalFruit(self, fruits: List[int]) -> int:
freq = {}
l = 0
ans = 0

        for r, fruit in enumerate(fruits):
            freq[fruit] = freq.get(fruit, 0) + 1

            while len(freq) > 2:
                freq[fruits[l]] -= 1
                if freq[fruits[l]] == 0:
                    del freq[fruits[l]]
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 1004. Max Consecutive Ones III

**LC 1004 — Max Consecutive Ones III**

```python
class Solution:
def longestOnes(self, nums: List[int], k: int) -> int:
l = 0
zeros = 0
ans = 0

        for r, x in enumerate(nums):
            if x == 0:
                zeros += 1

            while zeros > k:
                if nums[l] == 0:
                    zeros -= 1
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit

**LC 1438 — Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit**

```python
class Solution:
def longestSubarray(self, nums: List[int], limit: int) -> int:
from collections import deque

        min_q = deque()
        max_q = deque()
        l = 0
        ans = 0

        for r, x in enumerate(nums):
            while min_q and nums[min_q[-1]] > x:
                min_q.pop()
            while max_q and nums[max_q[-1]] < x:
                max_q.pop()

            min_q.append(r)
            max_q.append(r)

            while nums[max_q[0]] - nums[min_q[0]] > limit:
                if min_q[0] == l:
                    min_q.popleft()
                if max_q[0] == l:
                    max_q.popleft()
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 1493. Longest Subarray of 1's After Deleting One Element

**LC 1493 — Longest Subarray of 1's After Deleting One Element**

```python
class Solution:
def longestSubarray(self, nums: List[int]) -> int:
l = 0
zeros = 0
ans = 0

        for r, x in enumerate(nums):
            if x == 0:
                zeros += 1

            while zeros > 1:
                if nums[l] == 0:
                    zeros -= 1
                l += 1

            ans = max(ans, r - l)

        return ans

```
### 1658. Minimum Operations to Reduce X to Zero

**LC 1658 — Minimum Operations to Reduce X to Zero**

```python
class Solution:
def minOperations(self, nums: List[int], x: int) -> int:
target = sum(nums) - x
if target < 0:
return -1
if target == 0:
return len(nums)

        l = 0
        total = 0
        longest = -1

        for r, num in enumerate(nums):
            total += num

            while total > target and l <= r:
                total -= nums[l]
                l += 1

            if total == target:
                longest = max(longest, r - l + 1)

        return -1 if longest == -1 else len(nums) - longest

```
### 1838. Frequency of the Most Frequent Element

**LC 1838 — Frequency of the Most Frequent Element**

```python
class Solution:
def maxFrequency(self, nums: List[int], k: int) -> int:
nums.sort()
l = 0
total = 0
ans = 1

        for r, x in enumerate(nums):
            total += x

            while x * (r - l + 1) - total > k:
                total -= nums[l]
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 2461. Maximum Sum of Distinct Subarrays With Length K

**LC 2461 — Maximum Sum of Distinct Subarrays With Length K**

```python
class Solution:
def maximumSubarraySum(self, nums: List[int], k: int) -> int:
freq = {}
total = 0
ans = 0
l = 0

        for r, x in enumerate(nums):
            total += x
            freq[x] = freq.get(x, 0) + 1

            if r - l + 1 > k:
                freq[nums[l]] -= 1
                total -= nums[l]
                if freq[nums[l]] == 0:
                    del freq[nums[l]]
                l += 1

            if r - l + 1 == k and len(freq) == k:
                ans = max(ans, total)

        return ans

```
### 2516. Take K of Each Character From Left and Right

**LC 2516 — Take K of Each Character From Left and Right**

```python
class Solution:
def takeCharacters(self, s: str, k: int) -> int:
total = {c: s.count(c) for c in "abc"}

        if any(total[c] < k for c in "abc"):
            return -1

        limit = {c: total[c] - k for c in "abc"}
        freq = {c: 0 for c in "abc"}

        l = 0
        longest = 0

        for r, ch in enumerate(s):
            freq[ch] += 1

            while any(freq[c] > limit[c] for c in "abc"):
                freq[s[l]] -= 1
                l += 1

            longest = max(longest, r - l + 1)

        return len(s) - longest

```
### 2762. Continuous Subarrays

**LC 2762 — Continuous Subarrays**

```python
class Solution:
def continuousSubarrays(self, nums: List[int]) -> int:
from collections import deque

        min_q = deque()
        max_q = deque()
        l = 0
        ans = 0

        for r, x in enumerate(nums):
            while min_q and nums[min_q[-1]] > x:
                min_q.pop()
            while max_q and nums[max_q[-1]] < x:
                max_q.pop()

            min_q.append(r)
            max_q.append(r)

            while nums[max_q[0]] - nums[min_q[0]] > 2:
                if min_q[0] == l:
                    min_q.popleft()
                if max_q[0] == l:
                    max_q.popleft()
                l += 1

            ans += r - l + 1

        return ans

```
### 2779. Maximum Beauty of an Array After Applying Operation

**LC 2779 — Maximum Beauty of an Array After Applying Operation**

```python
class Solution:
def maximumBeauty(self, nums: List[int], k: int) -> int:
nums.sort()
l = 0
ans = 0

        for r in range(len(nums)):
            while nums[r] - nums[l] > 2 * k:
                l += 1

            ans = max(ans, r - l + 1)

        return ans

```
### 2981. Find Longest Special Substring That Occurs Thrice I

**LC 2981 — Find Longest Special Substring That Occurs Thrice I**

```python
class Solution:
def maximumLength(self, s: str) -> int:
runs = []

        i = 0
        while i < len(s):
            j = i
            while j < len(s) and s[j] == s[i]:
                j += 1
            runs.append((s[i], j - i))
            i = j

        ans = 0

        for length in range(1, len(s) + 1):
            count = 0
            for _, run in runs:
                if run >= length:
                    count += run - length + 1

            if count >= 3:
                ans = length

        return ans if ans else -1

```
### 3026. Maximum Good Subarray Sum

**LC 3026 — Maximum Good Subarray Sum**

```python
class Solution:
def maximumSubarraySum(self, nums: List[int], k: int) -> int:
prefix = 0
first = {}
ans = float('-inf')

        for x in nums:
            if x - k in first:
                ans = max(ans, prefix + x - first[x - k])

            if x + k in first:
                ans = max(ans, prefix + x - first[x + k])

            if x not in first:
                first[x] = prefix

            prefix += x

        return ans if ans != float('-inf') else 0

```
### 3346. Maximum Frequency of an Element After Performing Operations I

**LC 3346 — Maximum Frequency of an Element After Performing Operations I**

```python
class Solution:
def maxFrequency(self, nums: List[int], k: int, numOperations: int) -> int:
nums.sort()
ans = 1
l = 0

        for r in range(len(nums)):
            while nums[r] - nums[l] > 2 * k:
                l += 1

            window = r - l + 1
            ans = max(ans, min(window, (r - l + 1) if True else 0))

        from collections import Counter
        freq = Counter(nums)

        for x in freq:
            l = 0
            while l < len(nums) and nums[l] < x - k:
                l += 1

            r = l
            while r < len(nums) and nums[r] <= x + k:
                r += 1

            ans = max(ans, min(r - l, freq[x] + numOperations))

        return ans

```
### 3347. Maximum Frequency of an Element After Performing Operations II

**LC 3347 — Maximum Frequency of an Element After Performing Operations II**

```python
class Solution:
def maxFrequency(self, nums: List[int], k: int, numOperations: int) -> int:
nums.sort()
from collections import Counter

        freq = Counter(nums)
        candidates = set(nums)

        for x in nums:
            candidates.add(x - k)
            candidates.add(x + k)

        ans = 0
        l = 0
        r = 0

        for x in sorted(candidates):
            while l < len(nums) and nums[l] < x - k:
                l += 1
            while r < len(nums) and nums[r] <= x + k:
                r += 1

            total = r - l
            same = freq.get(x, 0)

            ans = max(ans, same + min(numOperations, total - same))

        return ans

```
## Pattern 10: Sliding Window — Monotonic Queue for Max/Min

### 239. Sliding Window Maximum

**LC 239 — Sliding Window Maximum**

```python
class Solution:
def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
from collections import deque

        q = deque()
        ans = []

        for r, x in enumerate(nums):
            while q and nums[q[-1]] <= x:
                q.pop()

            q.append(r)

            if q[0] <= r - k:
                q.popleft()

            if r >= k - 1:
                ans.append(nums[q[0]])

        return ans

```
### 862. Shortest Subarray with Sum at Least K

**LC 862 — Shortest Subarray with Sum at Least K**

```python
class Solution:
def shortestSubarray(self, nums: List[int], k: int) -> int:
from collections import deque

        prefix = [0]
        for x in nums:
            prefix.append(prefix[-1] + x)

        q = deque()
        ans = len(nums) + 1

        for i, p in enumerate(prefix):
            while q and p - prefix[q[0]] >= k:
                ans = min(ans, i - q.popleft())

            while q and prefix[q[-1]] >= p:
                q.pop()

            q.append(i)

        return -1 if ans == len(nums) + 1 else ans

```
### 1696. Jump Game VI

**LC 1696 — Jump Game VI**

```python
class Solution:
def maxResult(self, nums: List[int], k: int) -> int:
from collections import deque

        dp = [0] * len(nums)
        dp[0] = nums[0]

        q = deque([0])

        for i in range(1, len(nums)):
            while q and q[0] < i - k:
                q.popleft()

            dp[i] = nums[i] + dp[q[0]]

            while q and dp[q[-1]] <= dp[i]:
                q.pop()

            q.append(i)

        return dp[-1]

```
## Pattern 11: Sliding Window — Character Frequency Matching

### 438. Find All Anagrams in a String

**LC 438 — Find All Anagrams in a String**

```python
class Solution:
def findAnagrams(self, s: str, p: str) -> List[int]:
if len(p) > len(s):
return []

        need = {}
        for ch in p:
            need[ch] = need.get(ch, 0) + 1

        window = {}
        ans = []
        l = 0

        for r, ch in enumerate(s):
            window[ch] = window.get(ch, 0) + 1

            if r - l + 1 > len(p):
                left = s[l]
                window[left] -= 1
                if window[left] == 0:
                    del window[left]
                l += 1

            if window == need:
                ans.append(l)

        return ans

```
### 567. Permutation in String

**LC 567 — Permutation in String**

```python
class Solution:
def checkInclusion(self, s1: str, s2: str) -> bool:
if len(s1) > len(s2):
return False

        need = {}
        for ch in s1:
            need[ch] = need.get(ch, 0) + 1

        window = {}
        l = 0

        for r, ch in enumerate(s2):
            window[ch] = window.get(ch, 0) + 1

            if r - l + 1 > len(s1):
                left = s2[l]
                window[left] -= 1
                if window[left] == 0:
                    del window[left]
                l += 1

            if window == need:
                return True

        return False

```