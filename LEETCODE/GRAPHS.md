# Graph Traversal Patterns — LeetCode Code Sheet

## Pattern 18: Tree — Serialization and Deserialization

### 297. Serialize and Deserialize Binary Tree

**LC 297 — Serialize and Deserialize Binary Tree**

```python
class Codec:

    def serialize(self, root):
        ans = []

        def dfs(node):
            if not node:
                ans.append("#")
                return

            ans.append(str(node.val))
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        return ",".join(ans)

    def deserialize(self, data):
        vals = iter(data.split(","))

        def dfs():
            val = next(vals)

            if val == "#":
                return None

            node = TreeNode(int(val))
            node.left = dfs()
            node.right = dfs()
            return node

        return dfs()

```
### 572. Subtree of Another Tree

**LC 572 — Subtree of Another Tree**

```python
class Solution:
def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
def same(a, b):
if not a and not b:
return True
if not a or not b or a.val != b.val:
return False
return same(a.left, b.left) and same(a.right, b.right)

        if not subRoot:
            return True
        if not root:
            return False

        return (
            same(root, subRoot)
            or self.isSubtree(root.left, subRoot)
            or self.isSubtree(root.right, subRoot)
        )

```
### 652. Find Duplicate Subtrees

**LC 652 — Find Duplicate Subtrees**

```python
class Solution:
def findDuplicateSubtrees(self, root: Optional[TreeNode]) -> List[Optional[TreeNode]]:
from collections import defaultdict

        ids = {}
        count = defaultdict(int)
        ans = []
        next_id = 1

        def dfs(node):
            nonlocal next_id

            if not node:
                return 0

            key = (node.val, dfs(node.left), dfs(node.right))

            if key not in ids:
                ids[key] = next_id
                next_id += 1

            uid = ids[key]
            count[uid] += 1

            if count[uid] == 2:
                ans.append(node)

            return uid

        dfs(root)
        return ans

```
## IV. Graph Traversal Patterns (DFS & BFS)

## Pattern 19: Graph DFS — Connected Components / Island Counting

### 130. Surrounded Regions

**LC 130 — Surrounded Regions**

```python
class Solution:
def solve(self, board: List[List[str]]) -> None:
if not board:
return

        n, m = len(board), len(board[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or board[r][c] != "O":
                return

            board[r][c] = "#"

            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(n):
            dfs(r, 0)
            dfs(r, m - 1)

        for c in range(m):
            dfs(0, c)
            dfs(n - 1, c)

        for r in range(n):
            for c in range(m):
                if board[r][c] == "O":
                    board[r][c] = "X"
                elif board[r][c] == "#":
                    board[r][c] = "O"

```
### 200. Number of Islands

**LC 200 — Number of Islands**

```python
class Solution:
def numIslands(self, grid: List[List[str]]) -> int:
n, m = len(grid), len(grid[0])
ans = 0

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] != "1":
                return

            grid[r][c] = "0"

            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(n):
            for c in range(m):
                if grid[r][c] == "1":
                    ans += 1
                    dfs(r, c)

        return ans

```
### 417. Pacific Atlantic Water Flow

**LC 417 — Pacific Atlantic Water Flow**

```python
class Solution:
def pacificAtlantic(self, heights: List[List[int]]) -> List[List[int]]:
n, m = len(heights), len(heights[0])

        def flow(starts):
            seen = set()

            def dfs(r, c):
                if (r, c) in seen:
                    return

                seen.add((r, c))

                for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                    nr, nc = r + dr, c + dc

                    if (
                        0 <= nr < n and
                        0 <= nc < m and
                        (nr, nc) not in seen and
                        heights[nr][nc] >= heights[r][c]
                    ):
                        dfs(nr, nc)

            for r, c in starts:
                dfs(r, c)

            return seen

        pacific = [(r, 0) for r in range(n)] + [(0, c) for c in range(m)]
        atlantic = [(r, m - 1) for r in range(n)] + [(n - 1, c) for c in range(m)]

        p = flow(pacific)
        a = flow(atlantic)

        return [[r, c] for r, c in p & a]

```
### 547. Number of Provinces

**LC 547 — Number of Provinces**

```python
class Solution:
def findCircleNum(self, isConnected: List[List[int]]) -> int:
n = len(isConnected)
seen = set()
ans = 0

        def dfs(node):
            seen.add(node)

            for nei in range(n):
                if isConnected[node][nei] == 1 and nei not in seen:
                    dfs(nei)

        for i in range(n):
            if i not in seen:
                ans += 1
                dfs(i)

        return ans

```
### 695. Max Area of Island

**LC 695 — Max Area of Island**

```python
class Solution:
def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
n, m = len(grid), len(grid[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] == 0:
                return 0

            grid[r][c] = 0

            return (
                1
                + dfs(r + 1, c)
                + dfs(r - 1, c)
                + dfs(r, c + 1)
                + dfs(r, c - 1)
            )

        ans = 0

        for r in range(n):
            for c in range(m):
                if grid[r][c] == 1:
                    ans = max(ans, dfs(r, c))

        return ans

```
### 733. Flood Fill

**LC 733 — Flood Fill**

```python
class Solution:
def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
old = image[sr][sc]

        if old == color:
            return image

        n, m = len(image), len(image[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or image[r][c] != old:
                return

            image[r][c] = color

            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        dfs(sr, sc)
        return image

```
### 841. Keys and Rooms

**LC 841 — Keys and Rooms**

```python
class Solution:
def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
seen = set()

        def dfs(room):
            if room in seen:
                return

            seen.add(room)

            for key in rooms[room]:
                dfs(key)

        dfs(0)
        return len(seen) == len(rooms)

```
### 1020. Number of Enclaves

**LC 1020 — Number of Enclaves**

```python
class Solution:
def numEnclaves(self, grid: List[List[int]]) -> int:
n, m = len(grid), len(grid[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or grid[r][c] == 0:
                return

            grid[r][c] = 0

            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(n):
            dfs(r, 0)
            dfs(r, m - 1)

        for c in range(m):
            dfs(0, c)
            dfs(n - 1, c)

        return sum(row.count(1) for row in grid)

```
### 1254. Number of Closed Islands

**LC 1254 — Number of Closed Islands**

```python
class Solution:
def closedIsland(self, grid: List[List[int]]) -> int:
n, m = len(grid), len(grid[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m:
                return False

            if grid[r][c] == 1:
                return True

            grid[r][c] = 1

            return (
                dfs(r + 1, c)
                and dfs(r - 1, c)
                and dfs(r, c + 1)
                and dfs(r, c - 1)
            )

        ans = 0

        for r in range(n):
            for c in range(m):
                if grid[r][c] == 0 and dfs(r, c):
                    ans += 1

        return ans

```
### 1905. Count Sub Islands

**LC 1905 — Count Sub Islands**

```python
class Solution:
def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
n, m = len(grid2), len(grid2[0])

        def dfs(r, c):
            if r < 0 or r >= n or c < 0 or c >= m or grid2[r][c] == 0:
                return True

            grid2[r][c] = 0

            valid = grid1[r][c] == 1

            valid = dfs(r + 1, c) and valid
            valid = dfs(r - 1, c) and valid
            valid = dfs(r, c + 1) and valid
            valid = dfs(r, c - 1) and valid

            return valid

        ans = 0

        for r in range(n):
            for c in range(m):
                if grid2[r][c] == 1 and dfs(r, c):
                    ans += 1

        return ans

```
### 2101. Detonate the Maximum Bombs

**LC 2101 — Detonate the Maximum Bombs**

```python
class Solution:
def maximumDetonation(self, bombs: List[List[int]]) -> int:
n = len(bombs)
graph = [[] for \_ in range(n)]

        for i in range(n):
            x1, y1, r = bombs[i]

            for j in range(n):
                if i == j:
                    continue

                x2, y2, _ = bombs[j]

                if (x1 - x2) ** 2 + (y1 - y2) ** 2 <= r ** 2:
                    graph[i].append(j)

        def dfs(start):
            seen = {start}
            stack = [start]

            while stack:
                node = stack.pop()

                for nei in graph[node]:
                    if nei not in seen:
                        seen.add(nei)
                        stack.append(nei)

            return len(seen)

        return max(dfs(i) for i in range(n))

```
## Pattern 20: Graph BFS — Connected Components / Island Counting

### 127. Word Ladder

**LC 127 — Word Ladder**

```python
class Solution:
def ladderLength(self, beginWord: str, endWord: str, wordList: List[str]) -> int:
words = set(wordList)

        if endWord not in words:
            return 0

        from collections import deque

        q = deque([(beginWord, 1)])
        seen = {beginWord}

        while q:
            word, dist = q.popleft()

            if word == endWord:
                return dist

            for i in range(len(word)):
                for c in "abcdefghijklmnopqrstuvwxyz":
                    nxt = word[:i] + c + word[i + 1:]

                    if nxt in words and nxt not in seen:
                        seen.add(nxt)
                        q.append((nxt, dist + 1))

        return 0

```
### 542. 01 Matrix

**LC 542 — 01 Matrix**

```python
class Solution:
def updateMatrix(self, mat: List[List[int]]) -> List[List[int]]:
from collections import deque

        n, m = len(mat), len(mat[0])
        q = deque()

        for r in range(n):
            for c in range(m):
                if mat[r][c] == 0:
                    q.append((r, c))
                else:
                    mat[r][c] = -1

        while q:
            r, c = q.popleft()

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < m and mat[nr][nc] == -1:
                    mat[nr][nc] = mat[r][c] + 1
                    q.append((nr, nc))

        return mat

```
### 994. Rotting Oranges

**LC 994 — Rotting Oranges**

```python
class Solution:
def orangesRotting(self, grid: List[List[int]]) -> int:
from collections import deque

        n, m = len(grid), len(grid[0])
        q = deque()
        fresh = 0

        for r in range(n):
            for c in range(m):
                if grid[r][c] == 2:
                    q.append((r, c))
                elif grid[r][c] == 1:
                    fresh += 1

        minutes = 0

        while q and fresh:
            for _ in range(len(q)):
                r, c = q.popleft()

                for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                    nr, nc = r + dr, c + dc

                    if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] == 1:
                        grid[nr][nc] = 2
                        fresh -= 1
                        q.append((nr, nc))

            minutes += 1

        return minutes if fresh == 0 else -1

```
### 1091. Shortest Path in Binary Matrix

**LC 1091 — Shortest Path in Binary Matrix**

```python
class Solution:
def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
from collections import deque

        n = len(grid)

        if grid[0][0] != 0 or grid[n - 1][n - 1] != 0:
            return -1

        q = deque([(0, 0, 1)])
        grid[0][0] = 1

        while q:
            r, c, dist = q.popleft()

            if r == n - 1 and c == n - 1:
                return dist

            for dr in (-1, 0, 1):
                for dc in (-1, 0, 1):
                    if dr == 0 and dc == 0:
                        continue

                    nr, nc = r + dr, c + dc

                    if 0 <= nr < n and 0 <= nc < n and grid[nr][nc] == 0:
                        grid[nr][nc] = 1
                        q.append((nr, nc, dist + 1))

        return -1

```
## Pattern 21: Graph DFS — Cycle Detection (Directed Graph)

### 207. Course Schedule

**LC 207 — Course Schedule**

```python
class Solution:
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
graph = [[] for \_ in range(numCourses)]

        for course, pre in prerequisites:
            graph[pre].append(course)

        state = [0] * numCourses

        def dfs(node):
            if state[node] == 1:
                return False
            if state[node] == 2:
                return True

            state[node] = 1

            for nei in graph[node]:
                if not dfs(nei):
                    return False

            state[node] = 2
            return True

        return all(dfs(i) for i in range(numCourses))

```
### 210. Course Schedule II

**LC 210 — Course Schedule II**

```python
class Solution:
def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
graph = [[] for \_ in range(numCourses)]

        for course, pre in prerequisites:
            graph[pre].append(course)

        state = [0] * numCourses
        order = []

        def dfs(node):
            if state[node] == 1:
                return False
            if state[node] == 2:
                return True

            state[node] = 1

            for nei in graph[node]:
                if not dfs(nei):
                    return False

            state[node] = 2
            order.append(node)
            return True

        for i in range(numCourses):
            if not dfs(i):
                return []

        return order[::-1]

```
### 802. Find Eventual Safe States

**LC 802 — Find Eventual Safe States**

```python
class Solution:
def eventualSafeNodes(self, graph: List[List[int]]) -> List[int]:
n = len(graph)
state = [0] \* n

        def dfs(node):
            if state[node] == 1:
                return False
            if state[node] == 2:
                return True

            state[node] = 1

            for nei in graph[node]:
                if not dfs(nei):
                    return False

            state[node] = 2
            return True

        return [i for i in range(n) if dfs(i)]

```
### 1059. All Paths from Source Lead to Destination

**LC 1059 — All Paths from Source Lead to Destination**

```python
class Solution:
def leadsToDestination(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
graph = [[] for \_ in range(n)]

        for u, v in edges:
            graph[u].append(v)

        state = [0] * n

        def dfs(node):
            if state[node] == 1:
                return False
            if state[node] == 2:
                return True

            if not graph[node]:
                return node == destination

            state[node] = 1

            for nei in graph[node]:
                if not dfs(nei):
                    return False

            state[node] = 2
            return True

        return dfs(source)

```
## Pattern 22: Graph BFS — Topological Sort (Kahn's Algorithm)

### 207. Course Schedule

**LC 207 — Course Schedule**

```python
class Solution:
def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
from collections import deque

        graph = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses

        for course, pre in prerequisites:
            graph[pre].append(course)
            indegree[course] += 1

        q = deque(i for i in range(numCourses) if indegree[i] == 0)
        count = 0

        while q:
            node = q.popleft()
            count += 1

            for nei in graph[node]:
                indegree[nei] -= 1

                if indegree[nei] == 0:
                    q.append(nei)

        return count == numCourses

```
### 210. Course Schedule II

**LC 210 — Course Schedule II**

```python
class Solution:
def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
from collections import deque

        graph = [[] for _ in range(numCourses)]
        indegree = [0] * numCourses

        for course, pre in prerequisites:
            graph[pre].append(course)
            indegree[course] += 1

        q = deque(i for i in range(numCourses) if indegree[i] == 0)
        ans = []

        while q:
            node = q.popleft()
            ans.append(node)

            for nei in graph[node]:
                indegree[nei] -= 1

                if indegree[nei] == 0:
                    q.append(nei)

        return ans if len(ans) == numCourses else []

```
### 269. Alien Dictionary

**LC 269 — Alien Dictionary**

```python
class Solution:
def alienOrder(self, words: List[str]) -> str:
from collections import deque

        graph = {c: set() for word in words for c in word}
        indegree = {c: 0 for c in graph}

        for a, b in zip(words, words[1:]):
            if len(a) > len(b) and a.startswith(b):
                return ""

            for x, y in zip(a, b):
                if x != y:
                    if y not in graph[x]:
                        graph[x].add(y)
                        indegree[y] += 1
                    break

        q = deque(c for c in indegree if indegree[c] == 0)
        ans = []

        while q:
            c = q.popleft()
            ans.append(c)

            for nei in graph[c]:
                indegree[nei] -= 1
                if indegree[nei] == 0:
                    q.append(nei)

        return "".join(ans) if len(ans) == len(graph) else ""

```
### 310. Minimum Height Trees

**LC 310 — Minimum Height Trees**

```python
class Solution:
def findMinHeightTrees(self, n: int, edges: List[List[int]]) -> List[int]:
from collections import deque

        if n == 1:
            return [0]

        graph = [[] for _ in range(n)]
        degree = [0] * n

        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
            degree[u] += 1
            degree[v] += 1

        q = deque(i for i in range(n) if degree[i] == 1)
        remaining = n

        while remaining > 2:
            size = len(q)
            remaining -= size

            for _ in range(size):
                node = q.popleft()

                for nei in graph[node]:
                    degree[nei] -= 1
                    if degree[nei] == 1:
                        q.append(nei)

        return list(q)

```
### 444. Sequence Reconstruction

**LC 444 — Sequence Reconstruction**

```python
class Solution:
def sequenceReconstruction(self, nums: List[int], sequences: List[List[int]]) -> bool:
from collections import defaultdict, deque

        graph = defaultdict(set)
        indegree = {x: 0 for x in nums}

        for seq in sequences:
            for x in seq:
                if x not in indegree:
                    return False

            for a, b in zip(seq, seq[1:]):
                if b not in graph[a]:
                    graph[a].add(b)
                    indegree[b] += 1

        q = deque(x for x in nums if indegree[x] == 0)
        ans = []

        while q:
            if len(q) > 1:
                return False

            node = q.popleft()
            ans.append(node)

            for nei in graph[node]:
                indegree[nei] -= 1
                if indegree[nei] == 0:
                    q.append(nei)

        return ans == nums

```
### 1136. Parallel Courses

**LC 1136 — Parallel Courses**

```python
class Solution:
def minimumSemesters(self, n: int, relations: List[List[int]]) -> int:
from collections import deque

        graph = [[] for _ in range(n)]
        indegree = [0] * n

        for u, v in relations:
            u -= 1
            v -= 1
            graph[u].append(v)
            indegree[v] += 1

        q = deque(i for i in range(n) if indegree[i] == 0)
        semesters = 0
        count = 0

        while q:
            semesters += 1

            for _ in range(len(q)):
                node = q.popleft()
                count += 1

                for nei in graph[node]:
                    indegree[nei] -= 1
                    if indegree[nei] == 0:
                        q.append(nei)

        return semesters if count == n else -1

```
### 1857. Largest Color Value in a Directed Graph

**LC 1857 — Largest Color Value in a Directed Graph**

```python
class Solution:
def largestPathValue(self, colors: str, edges: List[List[int]]) -> int:
from collections import deque

        n = len(colors)
        graph = [[] for _ in range(n)]
        indegree = [0] * n

        for u, v in edges:
            graph[u].append(v)
            indegree[v] += 1

        dp = [[0] * 26 for _ in range(n)]
        q = deque(i for i in range(n) if indegree[i] == 0)

        count = 0
        ans = 0

        while q:
            node = q.popleft()
            count += 1

            c = ord(colors[node]) - ord('a')
            dp[node][c] += 1
            ans = max(ans, dp[node][c])

            for nei in graph[node]:
                for j in range(26):
                    dp[nei][j] = max(dp[nei][j], dp[node][j])

                indegree[nei] -= 1
                if indegree[nei] == 0:
                    q.append(nei)

        return ans if count == n else -1

```
### 2050. Parallel Courses III

**LC 2050 — Parallel Courses III**

```python
class Solution:
def minimumTime(self, n: int, relations: List[List[int]], time: List[int]) -> int:
from collections import deque

        graph = [[] for _ in range(n)]
        indegree = [0] * n

        for u, v in relations:
            u -= 1
            v -= 1
            graph[u].append(v)
            indegree[v] += 1

        finish = time[:]
        q = deque(i for i in range(n) if indegree[i] == 0)

        while q:
            node = q.popleft()

            for nei in graph[node]:
                finish[nei] = max(finish[nei], finish[node] + time[nei])
                indegree[nei] -= 1

                if indegree[nei] == 0:
                    q.append(nei)

        return max(finish)

```
### 2115. Find All Possible Recipes from Given Supplies

**LC 2115 — Find All Possible Recipes from Given Supplies**

```python
class Solution:
def findAllRecipes(self, recipes: List[str], ingredients: List[List[str]], supplies: List[str]) -> List[str]:
from collections import defaultdict, deque

        graph = defaultdict(list)
        indegree = {recipe: 0 for recipe in recipes}

        for recipe, items in zip(recipes, ingredients):
            for item in items:
                graph[item].append(recipe)
                indegree[recipe] += 1

        q = deque(supplies)
        ans = []

        while q:
            item = q.popleft()

            for recipe in graph[item]:
                indegree[recipe] -= 1

                if indegree[recipe] == 0:
                    ans.append(recipe)
                    q.append(recipe)

        return ans

```
### 2392. Build a Matrix With Conditions

**LC 2392 — Build a Matrix With Conditions**

```python
class Solution:
def buildMatrix(self, k: int, rowConditions: List[List[int]], colConditions: List[List[int]]) -> List[List[int]]:
from collections import deque

        def topo(edges):
            graph = [[] for _ in range(k + 1)]
            indegree = [0] * (k + 1)

            for a, b in edges:
                graph[a].append(b)
                indegree[b] += 1

            q = deque(i for i in range(1, k + 1) if indegree[i] == 0)
            order = []

            while q:
                x = q.popleft()
                order.append(x)

                for nei in graph[x]:
                    indegree[nei] -= 1
                    if indegree[nei] == 0:
                        q.append(nei)

            return order if len(order) == k else []

        rows = topo(rowConditions)
        cols = topo(colConditions)

        if not rows or not cols:
            return []

        row_pos = {x: i for i, x in enumerate(rows)}
        col_pos = {x: i for i, x in enumerate(cols)}

        matrix = [[0] * k for _ in range(k)]

        for x in range(1, k + 1):
            matrix[row_pos[x]][col_pos[x]] = x

        return matrix

```
## Pattern 23: Graph — Deep Copy / Cloning

### 133. Clone Graph

**LC 133 — Clone Graph**

```python
class Solution:
def cloneGraph(self, node: Optional['Node']) -> Optional['Node']:
if not node:
return None

        clones = {}

        def dfs(cur):
            if cur in clones:
                return clones[cur]

            copy = Node(cur.val)
            clones[cur] = copy

            for nei in cur.neighbors:
                copy.neighbors.append(dfs(nei))

            return copy

        return dfs(node)

```
## Pattern 24: Graph — Shortest Path (Dijkstra's Algorithm)

### 743. Network Delay Time

**LC 743 — Network Delay Time**

```python
class Solution:
def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
import heapq

        graph = [[] for _ in range(n + 1)]

        for u, v, w in times:
            graph[u].append((v, w))

        dist = [float('inf')] * (n + 1)
        dist[k] = 0
        heap = [(0, k)]

        while heap:
            d, node = heapq.heappop(heap)

            if d != dist[node]:
                continue

            for nei, w in graph[node]:
                nd = d + w

                if nd < dist[nei]:
                    dist[nei] = nd
                    heapq.heappush(heap, (nd, nei))

        ans = max(dist[1:])
        return -1 if ans == float('inf') else ans

```
### 778. Swim in Rising Water

**LC 778 — Swim in Rising Water**

```python
class Solution:
def swimInWater(self, grid: List[List[int]]) -> int:
import heapq

        n = len(grid)
        heap = [(grid[0][0], 0, 0)]
        seen = {(0, 0)}

        while heap:
            time, r, c = heapq.heappop(heap)

            if r == n - 1 and c == n - 1:
                return time

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < n and (nr, nc) not in seen:
                    seen.add((nr, nc))
                    heapq.heappush(heap, (max(time, grid[nr][nc]), nr, nc))

```
### 1514. Path with Maximum Probability

**LC 1514 — Path with Maximum Probability**

```python
class Solution:
def maxProbability(self, n: int, edges: List[List[int]], succProb: List[float], start_node: int, end_node: int) -> float:
import heapq

        graph = [[] for _ in range(n)]

        for (u, v), p in zip(edges, succProb):
            graph[u].append((v, p))
            graph[v].append((u, p))

        best = [0.0] * n
        best[start_node] = 1.0

        heap = [(-1.0, start_node)]

        while heap:
            prob, node = heapq.heappop(heap)
            prob = -prob

            if node == end_node:
                return prob

            if prob < best[node]:
                continue

            for nei, p in graph[node]:
                new_prob = prob * p

                if new_prob > best[nei]:
                    best[nei] = new_prob
                    heapq.heappush(heap, (-new_prob, nei))

        return 0.0

```
### 1631. Path With Minimum Effort

**LC 1631 — Path With Minimum Effort**

```python
class Solution:
def minimumEffortPath(self, heights: List[List[int]]) -> int:
import heapq

        n, m = len(heights), len(heights[0])
        dist = [[float('inf')] * m for _ in range(n)]
        dist[0][0] = 0

        heap = [(0, 0, 0)]

        while heap:
            effort, r, c = heapq.heappop(heap)

            if (r, c) == (n - 1, m - 1):
                return effort

            if effort > dist[r][c]:
                continue

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < m:
                    new_effort = max(
                        effort,
                        abs(heights[r][c] - heights[nr][nc])
                    )

                    if new_effort < dist[nr][nc]:
                        dist[nr][nc] = new_effort
                        heapq.heappush(heap, (new_effort, nr, nc))

```
### 1976. Number of Ways to Arrive at Destination

**LC 1976 — Number of Ways to Arrive at Destination**

```python
class Solution:
def countPaths(self, n: int, roads: List[List[int]]) -> int:
import heapq

        MOD = 10**9 + 7
        graph = [[] for _ in range(n)]

        for u, v, w in roads:
            graph[u].append((v, w))
            graph[v].append((u, w))

        dist = [float('inf')] * n
        ways = [0] * n

        dist[0] = 0
        ways[0] = 1

        heap = [(0, 0)]

        while heap:
            d, node = heapq.heappop(heap)

            if d > dist[node]:
                continue

            for nei, w in graph[node]:
                nd = d + w

                if nd < dist[nei]:
                    dist[nei] = nd
                    ways[nei] = ways[node]
                    heapq.heappush(heap, (nd, nei))

                elif nd == dist[nei]:
                    ways[nei] = (ways[nei] + ways[node]) % MOD

        return ways[n - 1]

```
### 2045. Second Minimum Time to Reach Destination

**LC 2045 — Second Minimum Time to Reach Destination**

```python
class Solution:
def secondMinimum(self, n: int, edges: List[List[int]], time: int, change: int) -> int:
import heapq

        graph = [[] for _ in range(n + 1)]

        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)

        best = [[] for _ in range(n + 1)]
        heap = [(0, 1)]

        while heap:
            t, node = heapq.heappop(heap)

            if best[node] and best[node][-1] == t:
                continue

            if len(best[node]) == 2:
                continue

            best[node].append(t)

            if node == n and len(best[node]) == 2:
                return t

            for nei in graph[node]:
                nt = t

                if (nt // change) % 2 == 1:
                    nt += change - (nt % change)

                nt += time

                if len(best[nei]) < 2:
                    heapq.heappush(heap, (nt, nei))

```
### 2203. Minimum Weighted Subgraph With the Required Paths

**LC 2203 — Minimum Weighted Subgraph With the Required Paths**

```python
class Solution:
def minimumWeight(self, n: int, edges: List[List[int]], src1: int, src2: int, dest: int) -> int:
import heapq

        graph = [[] for _ in range(n)]
        reverse = [[] for _ in range(n)]

        for u, v, w in edges:
            graph[u].append((v, w))
            reverse[v].append((u, w))

        def dijkstra(start, g):
            dist = [float('inf')] * n
            dist[start] = 0
            heap = [(0, start)]

            while heap:
                d, node = heapq.heappop(heap)

                if d != dist[node]:
                    continue

                for nei, w in g[node]:
                    nd = d + w

                    if nd < dist[nei]:
                        dist[nei] = nd
                        heapq.heappush(heap, (nd, nei))

            return dist

        d1 = dijkstra(src1, graph)
        d2 = dijkstra(src2, graph)
        dd = dijkstra(dest, reverse)

        ans = float('inf')

        for i in range(n):
            if d1[i] != float('inf') and d2[i] != float('inf') and dd[i] != float('inf'):
                ans = min(ans, d1[i] + d2[i] + dd[i])

        return -1 if ans == float('inf') else ans

```
### 2290. Minimum Obstacle Removal to Reach Corner

**LC 2290 — Minimum Obstacle Removal to Reach Corner**

```python
class Solution:
def minimumObstacles(self, grid: List[List[int]]) -> int:
import heapq

        n, m = len(grid), len(grid[0])
        dist = [[float('inf')] * m for _ in range(n)]
        dist[0][0] = 0

        heap = [(0, 0, 0)]

        while heap:
            cost, r, c = heapq.heappop(heap)

            if (r, c) == (n - 1, m - 1):
                return cost

            if cost > dist[r][c]:
                continue

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < m:
                    new_cost = cost + grid[nr][nc]

                    if new_cost < dist[nr][nc]:
                        dist[nr][nc] = new_cost
                        heapq.heappush(heap, (new_cost, nr, nc))

```
### 2577. Minimum Time to Visit a Cell In a Grid

**LC 2577 — Minimum Time to Visit a Cell In a Grid**

```python
class Solution:
def minimumTime(self, grid: List[List[int]]) -> int:
import heapq

        n, m = len(grid), len(grid[0])

        if n > 1 and m > 1:
            if grid[0][1] > 1 and grid[1][0] > 1:
                return -1

        dist = [[float('inf')] * m for _ in range(n)]
        dist[0][0] = 0

        heap = [(0, 0, 0)]

        while heap:
            t, r, c = heapq.heappop(heap)

            if (r, c) == (n - 1, m - 1):
                return t

            if t != dist[r][c]:
                continue

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < m:
                    nt = t + 1

                    if nt < grid[nr][nc]:
                        wait = grid[nr][nc] - nt

                        if wait % 2 == 0:
                            nt = grid[nr][nc]
                        else:
                            nt = grid[nr][nc] + 1

                    if nt < dist[nr][nc]:
                        dist[nr][nc] = nt
                        heapq.heappush(heap, (nt, nr, nc))

        return -1

```
### 2812. Find the Safest Path in a Grid

**LC 2812 — Find the Safest Path in a Grid**

```python
class Solution:
def maximumSafenessFactor(self, grid: List[List[int]]) -> int:
from collections import deque
import heapq

        n = len(grid)

        dist = [[-1] * n for _ in range(n)]
        q = deque()

        for r in range(n):
            for c in range(n):
                if grid[r][c] == 1:
                    dist[r][c] = 0
                    q.append((r, c))

        while q:
            r, c = q.popleft()

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < n and dist[nr][nc] == -1:
                    dist[nr][nc] = dist[r][c] + 1
                    q.append((nr, nc))

        heap = [(-dist[0][0], 0, 0)]
        seen = {(0, 0)}

        while heap:
            safety, r, c = heapq.heappop(heap)
            safety = -safety

            if r == n - 1 and c == n - 1:
                return safety

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < n and 0 <= nc < n and (nr, nc) not in seen:
                    seen.add((nr, nc))
                    ns = min(safety, dist[nr][nc])
                    heapq.heappush(heap, (-ns, nr, nc))

```
## Pattern 25: Graph — Shortest Path (Bellman-Ford / BFS + K)

### 787. Cheapest Flights Within K Stops

**LC 787 — Cheapest Flights Within K Stops**

```python
class Solution:
def findCheapestPrice(self, n: int, flights: List[List[int]], src: int, dst: int, k: int) -> int:
dist = [float('inf')] \* n
dist[src] = 0

        for _ in range(k + 1):
            new_dist = dist[:]

            for u, v, price in flights:
                if dist[u] != float('inf'):
                    new_dist[v] = min(new_dist[v], dist[u] + price)

            dist = new_dist

        return -1 if dist[dst] == float('inf') else dist[dst]

```
## Pattern 26: Graph — Union-Find (Disjoint Set Union — DSU)

### 200. Number of Islands

**LC 200 — Number of Islands**

```python
class Solution:
def numIslands(self, grid: List[List[str]]) -> int:
n, m = len(grid), len(grid[0])
parent = list(range(n _ m))
rank = [0] _ (n \* m)
count = 0

        def find(x):
            while x != parent[x]:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra == rb:
                return False

            if rank[ra] < rank[rb]:
                ra, rb = rb, ra

            parent[rb] = ra

            if rank[ra] == rank[rb]:
                rank[ra] += 1

            return True

        for r in range(n):
            for c in range(m):
                if grid[r][c] == "1":
                    count += 1

                    if r > 0 and grid[r - 1][c] == "1" and union(r * m + c, (r - 1) * m + c):
                        count -= 1

                    if c > 0 and grid[r][c - 1] == "1" and union(r * m + c, r * m + c - 1):
                        count -= 1

        return count

```
### 261. Graph Valid Tree

**LC 261 — Graph Valid Tree**

```python
class Solution:
def validTree(self, n: int, edges: List[List[int]]) -> bool:
if len(edges) != n - 1:
return False

        parent = list(range(n))

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra == rb:
                return False

            parent[rb] = ra
            return True

        for u, v in edges:
            if not union(u, v):
                return False

        return True

```
### 305. Number of Islands II

**LC 305 — Number of Islands II**

```python
class Solution:
def numIslands2(self, m: int, n: int, positions: List[List[int]]) -> List[int]:
parent = list(range(m _ n))
rank = [0] _ (m _ n)
land = [False] _ (m \* n)
count = 0
ans = []

        def find(x):
            while x != parent[x]:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra == rb:
                return False

            if rank[ra] < rank[rb]:
                ra, rb = rb, ra

            parent[rb] = ra

            if rank[ra] == rank[rb]:
                rank[ra] += 1

            return True

        for r, c in positions:
            idx = r * n + c

            if land[idx]:
                ans.append(count)
                continue

            land[idx] = True
            count += 1

            for dr, dc in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                nr, nc = r + dr, c + dc

                if 0 <= nr < m and 0 <= nc < n:
                    nei = nr * n + nc

                    if land[nei] and union(idx, nei):
                        count -= 1

            ans.append(count)

        return ans

```
### 323. Number of Connected Components in an Undirected Graph

**LC 323 — Number of Connected Components in an Undirected Graph**

```python
class Solution:
def countComponents(self, n: int, edges: List[List[int]]) -> int:
parent = list(range(n))
count = n

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        for u, v in edges:
            ru, rv = find(u), find(v)

            if ru != rv:
                parent[rv] = ru
                count -= 1

        return count

```
### 547. Number of Provinces

**LC 547 — Number of Provinces**

```python
class Solution:
def findCircleNum(self, isConnected: List[List[int]]) -> int:
n = len(isConnected)
parent = list(range(n))
count = n

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j] == 1:
                    ri, rj = find(i), find(j)

                    if ri != rj:
                        parent[rj] = ri
                        count -= 1

        return count

```
### 684. Redundant Connection

**LC 684 — Redundant Connection**

```python
class Solution:
def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
parent = list(range(len(edges) + 1))

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        for u, v in edges:
            ru, rv = find(u), find(v)

            if ru == rv:
                return [u, v]

            parent[rv] = ru

```
### 721. Accounts Merge

**LC 721 — Accounts Merge**

```python
class Solution:
def accountsMerge(self, accounts: List[List[str]]) -> List[List[str]]:
parent = list(range(len(accounts)))

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        email_owner = {}

        for i, account in enumerate(accounts):
            for email in account[1:]:
                if email in email_owner:
                    a, b = find(i), find(email_owner[email])
                    parent[b] = a
                else:
                    email_owner[email] = i

        groups = {}

        for email, owner in email_owner.items():
            root = find(owner)
            groups.setdefault(root, []).append(email)

        return [
            [accounts[root][0]] + sorted(emails)
            for root, emails in groups.items()
        ]

```
### 737. Sentence Similarity II

**LC 737 — Sentence Similarity II**

```python
class Solution:
def areSentencesSimilarTwo(self, sentence1: List[str], sentence2: List[str], similarPairs: List[List[str]]) -> bool:
if len(sentence1) != len(sentence2):
return False

        parent = {}

        def find(x):
            if x not in parent:
                parent[x] = x

            if parent[x] != x:
                parent[x] = find(parent[x])

            return parent[x]

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra != rb:
                parent[rb] = ra

        for a, b in similarPairs:
            union(a, b)

        for a, b in zip(sentence1, sentence2):
            if a == b:
                continue

            if find(a) != find(b):
                return False

        return True

```
### 947. Most Stones Removed with Same Row or Column

**LC 947 — Most Stones Removed with Same Row or Column**

```python
class Solution:
def removeStones(self, stones: List[List[int]]) -> int:
parent = {}

        def find(x):
            if parent.setdefault(x, x) != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra != rb:
                parent[rb] = ra
                return True

            return False

        count = 0

        for r, c in stones:
            if union(("r", r), ("c", c)):
                count += 1

        return count

```
### 952. Largest Component Size by Common Factor

**LC 952 — Largest Component Size by Common Factor**

```python
class Solution:
def largestComponentSize(self, nums: List[int]) -> int:
from collections import defaultdict

        n = len(nums)
        parent = list(range(n))
        size = [1] * n
        factor_owner = {}

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(a, b):
            ra, rb = find(a), find(b)

            if ra == rb:
                return

            if size[ra] < size[rb]:
                ra, rb = rb, ra

            parent[rb] = ra
            size[ra] += size[rb]

        for i, x in enumerate(nums):
            d = 2

            while d * d <= x:
                if x % d == 0:
                    if d in factor_owner:
                        union(i, factor_owner[d])
                    else:
                        factor_owner[d] = i

                    while x % d == 0:
                        x //= d

                d += 1

            if x > 1:
                if x in factor_owner:
                    union(i, factor_owner[x])
                else:
                    factor_owner[x] = i

        return max(size[find(i)] for i in range(n))

```
### 959. Regions Cut By Slashes

**LC 959 — Regions Cut By Slashes**

```python
class Solution:
def regionsBySlashes(self, grid: List[str]) -> int:
n = len(grid)
parent = list(range(4 _ n _ n))

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        def union(a, b):
            a, b = find(a), find(b)
            if a != b:
                parent[b] = a

        for r in range(n):
            for c in range(n):
                base = 4 * (r * n + c)
                ch = grid[r][c]

                if ch != "/":
                    union(base, base + 1)
                    union(base + 2, base + 3)

                if ch != "\\":
                    union(base, base + 3)
                    union(base + 1, base + 2)

                if r > 0:
                    union(base, base - 4 * n + 2)

                if r < n - 1:
                    union(base + 2, base + 4 * n)

                if c > 0:
                    union(base + 3, base - 4 + 1)

                if c < n - 1:
                    union(base + 1, base + 4 + 3)

        return sum(find(i) == i for i in range(4 * n * n))

```
### 1101. The Earliest Moment When Everyone Become Friends

**LC 1101 — The Earliest Moment When Everyone Become Friends**

```python
class Solution:
def earliestAcq(self, logs: List[List[int]], n: int) -> int:
logs.sort()
parent = list(range(n))
components = n

        def find(x):
            if parent[x] != x:
                parent[x] = find(parent[x])
            return parent[x]

        for time, a, b in logs:
            ra, rb = find(a), find(b)

            if ra != rb:
                parent[rb] = ra
                components -= 1

                if components == 1:
                    return time

        return -1

```