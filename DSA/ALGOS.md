1. Standard BFS — Grid / Matrix

from collections import deque

class Solution:
def solve(self, grid):

        m, n = len(grid), len(grid[0])

        # ===== CHANGE: INITIALIZATION =====
        q = deque()
        visited = [[False] * n for _ in range(m)]

        # Add starting cells
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:       # <-- CHANGE CONDITION
                    q.append((i, j))
                    visited[i][j] = True

        # ===== PRESERVED =====
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        while q:
            r, c = q.popleft()

            # ===== CHANGE: PROCESS CURRENT CELL =====
            # do something with (r, c)

            # ===== PRESERVED =====
            for dr, dc in directions:
                nr = r + dr
                nc = c + dc

                if 0 <= nr < m and 0 <= nc < n:

                    # ===== CHANGE: VISIT CONDITION =====
                    if not visited[nr][nc]:

                        # ===== CHANGE: UPDATE =====
                        visited[nr][nc] = True

                        q.append((nr, nc))

        # ===== CHANGE: RETURN =====

🔥 Parts you normally change

1. Starting cells
2. Process current cell
3. Visit condition
4. Update/result
5. Return

Everything else can usually remain the same.

2. BFS with Distance

For problems like 01 Matrix, shortest path, rotten oranges, etc.:

from collections import deque

class Solution:
def solve(self, grid):

        m, n = len(grid), len(grid[0])

        q = deque()
        dist = [[-1] * n for _ in range(m)]

        # ===== CHANGE: STARTING CONDITION =====
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 0:
                    q.append((i, j))
                    dist[i][j] = 0

        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        while q:
            r, c = q.popleft()

            for dr, dc in directions:
                nr = r + dr
                nc = c + dc

                if 0 <= nr < m and 0 <= nc < n:

                    # ===== CHANGE: VISIT CONDITION =====
                    if dist[nr][nc] == -1:

                        # ===== CHANGE: DISTANCE UPDATE =====
                        dist[nr][nc] = dist[r][c] + 1

                        q.append((nr, nc))

        return dist

For 542. 01 Matrix, this is basically your reusable template.

3. Standard DFS — Grid / Matrix

For connected components, islands, flood fill, etc.:

class Solution:
def solve(self, grid):

        m, n = len(grid), len(grid[0])

        visited = [[False] * n for _ in range(m)]

        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

        def dfs(r, c):

            # ===== CHANGE: BASE CONDITION =====
            if not (0 <= r < m and 0 <= c < n):
                return

            # ===== CHANGE: VISIT CONDITION =====
            if visited[r][c]:
                return

            # ===== CHANGE: CELL CONDITION =====
            if grid[r][c] != 1:
                return

            # ===== PRESERVED =====
            visited[r][c] = True

            # ===== CHANGE: PROCESS =====
            # do something with (r, c)

            # ===== PRESERVED =====
            for dr, dc in directions:
                dfs(r + dr, c + dc)

        # ===== CHANGE: STARTING CELLS =====
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    dfs(i, j)

        # ===== CHANGE: RETURN =====

4. Even More Reusable DFS

You can make the DFS skeleton cleaner:

def dfs(r, c):

    if r < 0 or r >= m or c < 0 or c >= n:
        return

    if visited[r][c]:
        return

    if grid[r][c] != TARGET:
        return

    visited[r][c] = True

    # ===== QUESTION-SPECIFIC WORK =====


    for dr, dc in directions:
        dfs(r + dr, c + dc)

Then only change:

TARGET = 1

and the work inside DFS.

5. BFS vs DFS Skeleton to Memorize

BFS

q = deque()

q.append(start)
visited[start] = True

while q:
r, c = q.popleft()

    # PROCESS

    for dr, dc in directions:
        nr, nc = r + dr, c + dc

        if valid and not visited:
            visited[nr][nc] = True
            q.append((nr, nc))

DFS

def dfs(r, c):

    if invalid or visited:
        return

    visited[r][c] = True

    # PROCESS

    for dr, dc in directions:
        dfs(r + dr, c + dc)

🧠 What to identify in every Grid question

When you see a matrix problem, immediately ask:

1. What cells are my STARTING points?
   ↓
2. What cells can I VISIT?
   ↓
3. What are my DIRECTIONS?
   ↓
4. What do I do when I VISIT a cell?
   ↓
5. Do I need BFS or DFS?
   ↓
6. Do I need distance / count / component size / modification?

Quick pattern recognition

Question asksUsually

Explore all connected cells

DFS / BFS

Number of islands

DFS / BFS

Flood fill

DFS / BFS

Shortest distance

BFS

Nearest 0 / nearest source

Multi-source BFS

Rotting oranges / spreading

Multi-source BFS

Count connected components

DFS / BFS

Capture surrounded regions

DFS / BFS

Cycle detection in grid

DFS / BFS

Traverse level by level

BFS

Best strategy for LeetCode: memorize the BFS skeleton + DFS skeleton, not individual solutions. Then each problem mainly becomes identifying the starting condition, visit condition, and what to update.

for trees iff?

Yes — for Trees, you should also keep a fixed skeleton and change only the question-specific part.

🌳 Tree BFS — Standard Skeleton

from collections import deque

class Solution:
def solve(self, root):

        if not root:
            return

        q = deque([root])

        while q:
            node = q.popleft()

            # ===== CHANGE: PROCESS NODE =====
            # do something with node.val

            # ===== PRESERVED =====
            if node.left:
                q.append(node.left)

            if node.right:
                q.append(node.right)

        # ===== CHANGE: RETURN =====

If the question needs level-by-level processing

This is extremely common:

from collections import deque

class Solution:
def solve(self, root):

        if not root:
            return []

        q = deque([root])
        result = []

        while q:

            # ===== PRESERVED =====
            size = len(q)
            level = []

            for _ in range(size):

                node = q.popleft()

                # ===== CHANGE: PROCESS =====
                level.append(node.val)

                # ===== PRESERVED =====
                if node.left:
                    q.append(node.left)

                if node.right:
                    q.append(node.right)

            # ===== CHANGE: STORE RESULT =====
            result.append(level)

        return result

This is your Level Order BFS skeleton.

🌳 Tree DFS — Standard Skeleton

For most tree problems, recursive DFS is the most useful template.

class Solution:
def solve(self, root):

        # ===== BASE CASE =====
        if not root:
            return

        # ===== CHANGE: PROCESS =====
        # do something with root.val

        # ===== PRESERVED =====
        self.solve(root.left)
        self.solve(root.right)

        # ===== CHANGE: RETURN / COMBINE =====

But for interview problems, this version is even more important:

class Solution:
def solve(self, root):

        def dfs(node):

            # ===== BASE CASE =====
            if not node:
                return

            # ===== CHANGE: PROCESS BEFORE =====

            dfs(node.left)

            # ===== CHANGE: PROCESS BETWEEN =====

            dfs(node.right)

            # ===== CHANGE: PROCESS AFTER =====

        dfs(root)

        # ===== RETURN =====

This gives you the three traversals:

        1
       / \
      2   3

Preorder

Root → Left → Right
1 → 2 → 3

Process before recursive calls.

Inorder

Left → Root → Right
2 → 1 → 3

Process between calls.

Postorder

Left → Right → Root
2 → 3 → 1

Process after calls.

🔥 Most Important Tree DFS Pattern

A lot of LeetCode tree questions are actually:

Get something from left subtree + get something from right subtree + combine them at current node.

Use:

class Solution:
def solve(self, root):

        def dfs(node):

            if not node:
                return 0

            left = dfs(node.left)
            right = dfs(node.right)

            # ===== CHANGE THIS =====
            return 1 + max(left, right)

        return dfs(root)

For example:

Maximum Depth

left = dfs(node.left)
right = dfs(node.right)

return 1 + max(left, right)

Tree Diameter

left = dfs(node.left)
right = dfs(node.right)

answer = max(answer, left + right)

return 1 + max(left, right)

Balanced Binary Tree

left = dfs(node.left)
right = dfs(node.right)

if abs(left - right) > 1: # not balanced

return 1 + max(left, right)

So the DFS skeleton stays almost identical.

