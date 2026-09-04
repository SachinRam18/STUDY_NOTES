# Two Pointer Patterns — LeetCode Code Sheet

## Pattern 1: Two Pointers — Converging

### 1. Two Sum

**LC 1 — Two Sum**

```python
class Solution:
def twoSum(self, nums: List[int], target: int) -> List[int]:
seen = {}
for i, x in enumerate(nums):
if target - x in seen:
return [seen[target - x], i]
seen[x] = i

```
### 11. Container With Most Water

**LC 11 — Container With Most Water**

```python
class Solution:
def maxArea(self, height: List[int]) -> int:
l, r = 0, len(height) - 1
ans = 0
while l < r:
ans = max(ans, min(height[l], height[r]) \* (r - l))
if height[l] < height[r]:
l += 1
else:
r -= 1
return ans

```
### 15. 3Sum

**LC 15 — 3Sum**

```python
class Solution:
def threeSum(self, nums: List[int]) -> List[List[int]]:
nums.sort()
ans = []

        for i in range(len(nums) - 2):
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            l, r = i + 1, len(nums) - 1
            while l < r:
                s = nums[i] + nums[l] + nums[r]

                if s == 0:
                    ans.append([nums[i], nums[l], nums[r]])
                    l += 1
                    r -= 1
                    while l < r and nums[l] == nums[l - 1]:
                        l += 1
                    while l < r and nums[r] == nums[r + 1]:
                        r -= 1
                elif s < 0:
                    l += 1
                else:
                    r -= 1

        return ans

```
### 16. 3Sum Closest

**LC 16 — 3Sum Closest**

```python
class Solution:
def threeSumClosest(self, nums: List[int], target: int) -> int:
nums.sort()
ans = nums[0] + nums[1] + nums[2]

        for i in range(len(nums) - 2):
            l, r = i + 1, len(nums) - 1

            while l < r:
                s = nums[i] + nums[l] + nums[r]

                if abs(s - target) < abs(ans - target):
                    ans = s

                if s < target:
                    l += 1
                elif s > target:
                    r -= 1
                else:
                    return s

        return ans

```
### 18. 4Sum

**LC 18 — 4Sum**

```python
class Solution:
def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
nums.sort()
ans = []
n = len(nums)

        for i in range(n - 3):
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            for j in range(i + 1, n - 2):
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue

                l, r = j + 1, n - 1

                while l < r:
                    s = nums[i] + nums[j] + nums[l] + nums[r]

                    if s == target:
                        ans.append([nums[i], nums[j], nums[l], nums[r]])
                        l += 1
                        r -= 1
                        while l < r and nums[l] == nums[l - 1]:
                            l += 1
                        while l < r and nums[r] == nums[r + 1]:
                            r -= 1
                    elif s < target:
                        l += 1
                    else:
                        r -= 1

        return ans

```
### 167. Two Sum II — Input Array Is Sorted

**LC 167 — Two Sum II - Input Array Is Sorted**

```python
class Solution:
def twoSum(self, numbers: List[int], target: int) -> List[int]:
l, r = 0, len(numbers) - 1

        while l < r:
            s = numbers[l] + numbers[r]
            if s == target:
                return [l + 1, r + 1]
            elif s < target:
                l += 1
            else:
                r -= 1

```
### 349. Intersection of Two Arrays

**LC 349 — Intersection of Two Arrays**

```python
class Solution:
def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
a = sorted(set(nums1))
b = sorted(set(nums2))
l = r = 0
ans = []

        while l < len(a) and r < len(b):
            if a[l] == b[r]:
                ans.append(a[l])
                l += 1
                r += 1
            elif a[l] < b[r]:
                l += 1
            else:
                r += 1

        return ans

```
### 392. Is Subsequence

**LC 392 — Is Subsequence**

```python
class Solution:
def isSubsequence(self, s: str, t: str) -> bool:
i = j = 0

        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
            j += 1

        return i == len(s)

```
### 881. Boats to Save People

**LC 881 — Boats to Save People**

```python
class Solution:
def numRescueBoats(self, people: List[int], limit: int) -> int:
people.sort()
l, r = 0, len(people) - 1
boats = 0

        while l <= r:
            if people[l] + people[r] <= limit:
                l += 1
            r -= 1
            boats += 1

        return boats

```
### 977. Squares of a Sorted Array

**LC 977 — Squares of a Sorted Array**

```python
class Solution:
def sortedSquares(self, nums: List[int]) -> List[int]:
l, r = 0, len(nums) - 1
ans = [0] \* len(nums)

        for i in range(len(nums) - 1, -1, -1):
            if abs(nums[l]) > abs(nums[r]):
                ans[i] = nums[l] * nums[l]
                l += 1
            else:
                ans[i] = nums[r] * nums[r]
                r -= 1

        return ans

```
### 259. 3Sum Smaller

**LC 259 — 3Sum Smaller**

```python
class Solution:
def threeSumSmaller(self, nums: List[int], target: int) -> int:
nums.sort()
ans = 0

        for i in range(len(nums) - 2):
            l, r = i + 1, len(nums) - 1

            while l < r:
                if nums[i] + nums[l] + nums[r] < target:
                    ans += r - l
                    l += 1
                else:
                    r -= 1

        return ans

```
## Pattern 2: Two Pointers — Fast & Slow

### 141. Linked List Cycle

**LC 141 — Linked List Cycle**

```python
class Solution:
def hasCycle(self, head: Optional[ListNode]) -> bool:
slow = fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                return True

        return False

```
### 202. Happy Number

**LC 202 — Happy Number**

```python
class Solution:
def isHappy(self, n: int) -> bool:
def digit_sum(x):
s = 0
while x:
s += (x % 10) \*\* 2
x //= 10
return s

        slow = fast = n

        while True:
            slow = digit_sum(slow)
            fast = digit_sum(digit_sum(fast))

            if fast == 1:
                return True
            if slow == fast:
                return False

```
### 287. Find the Duplicate Number

**LC 287 — Find the Duplicate Number**

```python
class Solution:
def findDuplicate(self, nums: List[int]) -> int:
slow = fast = nums[0]

        while True:
            slow = nums[slow]
            fast = nums[nums[fast]]
            if slow == fast:
                break

        slow = nums[0]

        while slow != fast:
            slow = nums[slow]
            fast = nums[fast]

        return slow

```
## Pattern 3: Two Pointers — Fixed Separation

### 19. Remove Nth Node From End of List

**LC 19 — Remove Nth Node From End of List**

```python
class Solution:
def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
dummy = ListNode(0, head)
slow = fast = dummy

        for _ in range(n):
            fast = fast.next

        while fast.next:
            slow = slow.next
            fast = fast.next

        slow.next = slow.next.next
        return dummy.next

```
### 876. Middle of the Linked List

**LC 876 — Middle of the Linked List**

```python
class Solution:
def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
slow = fast = head

        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        return slow

```
### 2095. Delete the Middle Node of a Linked List

**LC 2095 — Delete the Middle Node of a Linked List**

```python
class Solution:
def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
if not head or not head.next:
return None

        slow = fast = head
        prev = None

        while fast and fast.next:
            prev = slow
            slow = slow.next
            fast = fast.next.next

        prev.next = slow.next
        return head

```
## Pattern 4: Two Pointers — In-place Array Modification

### 26. Remove Duplicates from Sorted Array

**LC 26 — Remove Duplicates from Sorted Array**

```python
class Solution:
def removeDuplicates(self, nums: List[int]) -> int:
k = 0

        for x in nums:
            if k == 0 or x != nums[k - 1]:
                nums[k] = x
                k += 1

        return k

```
### 27. Remove Element

**LC 27 — Remove Element**

```python
class Solution:
def removeElement(self, nums: List[int], val: int) -> int:
k = 0

        for x in nums:
            if x != val:
                nums[k] = x
                k += 1

        return k

```
### 75. Sort Colors

**LC 75 — Sort Colors**

```python
class Solution:
def sortColors(self, nums: List[int]) -> None:
l, i, r = 0, 0, len(nums) - 1

        while i <= r:
            if nums[i] == 0:
                nums[l], nums[i] = nums[i], nums[l]
                l += 1
                i += 1
            elif nums[i] == 2:
                nums[i], nums[r] = nums[r], nums[i]
                r -= 1
            else:
                i += 1

```
### 80. Remove Duplicates from Sorted Array II

**LC 80 — Remove Duplicates from Sorted Array II**

```python
class Solution:
def removeDuplicates(self, nums: List[int]) -> int:
k = 0

        for x in nums:
            if k < 2 or x != nums[k - 2]:
                nums[k] = x
                k += 1

        return k

```
### 283. Move Zeroes

**LC 283 — Move Zeroes**

```python
class Solution:
def moveZeroes(self, nums: List[int]) -> None:
k = 0

        for i in range(len(nums)):
            if nums[i] != 0:
                nums[k], nums[i] = nums[i], nums[k]
                k += 1

```
### 443. String Compression

**LC 443 — String Compression**

```python
class Solution:
def compress(self, chars: List[str]) -> int:
write = 0
read = 0

        while read < len(chars):
            ch = chars[read]
            start = read

            while read < len(chars) and chars[read] == ch:
                read += 1

            chars[write] = ch
            write += 1

            count = read - start
            if count > 1:
                for digit in str(count):
                    chars[write] = digit
                    write += 1

        return write

```
### 905. Sort Array By Parity

**LC 905 — Sort Array By Parity**

```python
class Solution:
def sortArrayByParity(self, nums: List[int]) -> List[int]:
l, r = 0, len(nums) - 1

        while l < r:
            if nums[l] % 2 == 0:
                l += 1
            elif nums[r] % 2 == 1:
                r -= 1
            else:
                nums[l], nums[r] = nums[r], nums[l]

        return nums

```
### 2337. Move Pieces to Obtain a String

**LC 2337 — Move Pieces to Obtain a String**

```python
class Solution:
def canChange(self, start: str, target: str) -> bool:
s = [(c, i) for i, c in enumerate(start) if c != '_']
t = [(c, i) for i, c in enumerate(target) if c != '_']

        if [c for c, _ in s] != [c for c, _ in t]:
            return False

        for (c, i), (_, j) in zip(s, t):
            if c == 'L' and i < j:
                return False
            if c == 'R' and i > j:
                return False

        return True

```
### 2938. Separate Black and White Balls

**LC 2938 — Separate Black and White Balls**

```python
class Solution:
def minimumSteps(self, s: str) -> int:
white = 0
ans = 0

        for c in s:
            if c == '0':
                ans += white
                white += 1

        return ans

```
## Pattern 5: Two Pointers — String Comparison with Backspaces

### 844. Backspace String Compare

**LC 844 — Backspace String Compare**

```python
class Solution:
def backspaceCompare(self, s: str, t: str) -> bool:
def next_char(x, i):
skip = 0

            while i >= 0:
                if x[i] == '#':
                    skip += 1
                elif skip:
                    skip -= 1
                else:
                    return i
                i -= 1

            return -1

        i, j = len(s) - 1, len(t) - 1

        while i >= 0 or j >= 0:
            i = next_char(s, i)
            j = next_char(t, j)

            if i == -1 or j == -1:
                return i == j

            if s[i] != t[j]:
                return False

            i -= 1
            j -= 1

        return True

```
## Pattern 6: Two Pointers — Expanding From Center

### 5. Longest Palindromic Substring

**LC 5 — Longest Palindromic Substring**

```python
class Solution:
def longestPalindrome(self, s: str) -> str:
ans = ""

        def expand(l, r):
            while l >= 0 and r < len(s) and s[l] == s[r]:
                l -= 1
                r += 1
            return s[l + 1:r]

        for i in range(len(s)):
            p1 = expand(i, i)
            p2 = expand(i, i + 1)

            if len(p1) > len(ans):
                ans = p1
            if len(p2) > len(ans):
                ans = p2

        return ans

```
### 647. Palindromic Substrings

**LC 647 — Palindromic Substrings**

```python
class Solution:
def countSubstrings(self, s: str) -> int:
ans = 0

        def expand(l, r):
            nonlocal ans

            while l >= 0 and r < len(s) and s[l] == s[r]:
                ans += 1
                l -= 1
                r += 1

        for i in range(len(s)):
            expand(i, i)
            expand(i, i + 1)

        return ans

```
## Pattern 7: Two Pointers — String Reversal

### 151. Reverse Words in a String

**LC 151 — Reverse Words in a String**

```python
class Solution:
def reverseWords(self, s: str) -> str:
return " ".join(s.split()[::-1])

```
### 344. Reverse String

**LC 344 — Reverse String**

```python
class Solution:
def reverseString(self, s: List[str]) -> None:
l, r = 0, len(s) - 1

        while l < r:
            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1

```
### 345. Reverse Vowels of a String

**LC 345 — Reverse Vowels of a String**

```python
class Solution:
def reverseVowels(self, s: str) -> str:
s = list(s)
vowels = set("aeiouAEIOU")
l, r = 0, len(s) - 1

        while l < r:
            while l < r and s[l] not in vowels:
                l += 1
            while l < r and s[r] not in vowels:
                r -= 1

            s[l], s[r] = s[r], s[l]
            l += 1
            r -= 1

        return "".join(s)

```
### 541. Reverse String II

**LC 541 — Reverse String II**

```python
class Solution:
def reverseStr(self, s: str, k: int) -> str:
s = list(s)

        for start in range(0, len(s), 2 * k):
            l = start
            r = min(start + k - 1, len(s) - 1)

            while l < r:
                s[l], s[r] = s[r], s[l]
                l += 1
                r -= 1

        return "".join(s)

```