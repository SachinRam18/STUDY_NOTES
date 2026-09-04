# Tree Traversal Patterns — LeetCode Code Sheet

## Pattern 12: Tree BFS — Level Order Traversal

### 102. Binary Tree Level Order Traversal

**LC 102 — Binary Tree Level Order Traversal**

```python
class Solution:
def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
from collections import deque

        if not root:
            return []

        q = deque([root])
        ans = []

        while q:
            level = []

            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)

                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

            ans.append(level)

        return ans

```
### 103. Binary Tree Zigzag Level Order Traversal

**LC 103 — Binary Tree Zigzag Level Order Traversal**

```python
class Solution:
def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
from collections import deque

        if not root:
            return []

        q = deque([root])
        ans = []
        left_to_right = True

        while q:
            level = []

            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)

                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

            if not left_to_right:
                level.reverse()

            ans.append(level)
            left_to_right = not left_to_right

        return ans

```
### 199. Binary Tree Right Side View

**LC 199 — Binary Tree Right Side View**

```python
class Solution:
def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
from collections import deque

        if not root:
            return []

        q = deque([root])
        ans = []

        while q:
            for i in range(len(q)):
                node = q.popleft()

                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

                if i == 0:
                    last = node

            ans.append(last.val)

        return ans

```
### 515. Find Largest Value in Each Tree Row

**LC 515 — Find Largest Value in Each Tree Row**

```python
class Solution:
def largestValues(self, root: Optional[TreeNode]) -> List[int]:
from collections import deque

        if not root:
            return []

        q = deque([root])
        ans = []

        while q:
            maximum = float('-inf')

            for _ in range(len(q)):
                node = q.popleft()
                maximum = max(maximum, node.val)

                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

            ans.append(maximum)

        return ans

```
### 1161. Maximum Level Sum of a Binary Tree

**LC 1161 — Maximum Level Sum of a Binary Tree**

```python
class Solution:
def maxLevelSum(self, root: Optional[TreeNode]) -> int:
from collections import deque

        q = deque([root])
        level = 1
        best_level = 1
        best_sum = float('-inf')

        while q:
            total = 0

            for _ in range(len(q)):
                node = q.popleft()
                total += node.val

                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)

            if total > best_sum:
                best_sum = total
                best_level = level

            level += 1

        return best_level

```
## Pattern 13: Tree DFS — Recursive Preorder Traversal

### 100. Same Tree

**LC 100 — Same Tree**

```python
class Solution:
def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
if not p and not q:
return True

        if not p or not q or p.val != q.val:
            return False

        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)

```
### 101. Symmetric Tree

**LC 101 — Symmetric Tree**

```python
class Solution:
def isSymmetric(self, root: Optional[TreeNode]) -> bool:
def check(a, b):
if not a and not b:
return True

            if not a or not b or a.val != b.val:
                return False

            return check(a.left, b.right) and check(a.right, b.left)

        return check(root.left, root.right)

```
### 105. Construct Binary Tree from Preorder and Inorder Traversal

**LC 105 — Construct Binary Tree from Preorder and Inorder Traversal**

```python
class Solution:
def buildTree(self, preorder: List[int], inorder: List[int]) -> Optional[TreeNode]:
if not preorder:
return None

        pos = {v: i for i, v in enumerate(inorder)}

        def build(pl, pr, il, ir):
            if pl > pr:
                return None

            root_val = preorder[pl]
            root = TreeNode(root_val)
            mid = pos[root_val]

            left_size = mid - il

            root.left = build(pl + 1, pl + left_size, il, mid - 1)
            root.right = build(pl + left_size + 1, pr, mid + 1, ir)

            return root

        return build(0, len(preorder) - 1, 0, len(inorder) - 1)

```
### 114. Flatten Binary Tree to Linked List

**LC 114 — Flatten Binary Tree to Linked List**

```python
class Solution:
def flatten(self, root: Optional[TreeNode]) -> None:
if not root:
return

        self.flatten(root.left)
        self.flatten(root.right)

        right = root.right
        root.right = root.left
        root.left = None

        cur = root
        while cur.right:
            cur = cur.right

        cur.right = right

```
### 226. Invert Binary Tree

**LC 226 — Invert Binary Tree**

```python
class Solution:
def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
if not root:
return None

        root.left, root.right = root.right, root.left

        self.invertTree(root.left)
        self.invertTree(root.right)

        return root

```
### 257. Binary Tree Paths

**LC 257 — Binary Tree Paths**

```python
class Solution:
def binaryTreePaths(self, root: Optional[TreeNode]) -> List[str]:
ans = []

        def dfs(node, path):
            if not node:
                return

            path += str(node.val)

            if not node.left and not node.right:
                ans.append(path)
                return

            path += "->"
            dfs(node.left, path)
            dfs(node.right, path)

        dfs(root, "")
        return ans

```
### 988. Smallest String Starting From Leaf

**LC 988 — Smallest String Starting From Leaf**

```python
class Solution:
def smallestFromLeaf(self, root: Optional[TreeNode]) -> str:
ans = None

        def dfs(node, path):
            nonlocal ans

            if not node:
                return

            path.append(chr(node.val + ord('a')))

            if not node.left and not node.right:
                cur = ''.join(reversed(path))
                if ans is None or cur < ans:
                    ans = cur

            dfs(node.left, path)
            dfs(node.right, path)

            path.pop()

        dfs(root, [])
        return ans

```
## Pattern 14: Tree DFS — Recursive Inorder Traversal

### 94. Binary Tree Inorder Traversal

**LC 94 — Binary Tree Inorder Traversal**

```python
class Solution:
def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
ans = []

        def dfs(node):
            if not node:
                return

            dfs(node.left)
            ans.append(node.val)
            dfs(node.right)

        dfs(root)
        return ans

```
### 98. Validate Binary Search Tree

**LC 98 — Validate Binary Search Tree**

```python
class Solution:
def isValidBST(self, root: Optional[TreeNode]) -> bool:
prev = None

        def dfs(node):
            nonlocal prev

            if not node:
                return True

            if not dfs(node.left):
                return False

            if prev is not None and node.val <= prev:
                return False

            prev = node.val
            return dfs(node.right)

        return dfs(root)

```
### 173. Binary Search Tree Iterator

**LC 173 — Binary Search Tree Iterator**

```python
class BSTIterator:

    def __init__(self, root: Optional[TreeNode]):
        self.stack = []
        self.pushLeft(root)

    def pushLeft(self, node):
        while node:
            self.stack.append(node)
            node = node.left

    def next(self) -> int:
        node = self.stack.pop()
        self.pushLeft(node.right)
        return node.val

    def hasNext(self) -> bool:
        return len(self.stack) > 0

```
### 230. Kth Smallest Element in a BST

**LC 230 — Kth Smallest Element in a BST**

```python
class Solution:
def kthSmallest(self, root: Optional[TreeNode], k: int) -> int:
stack = []
node = root

        while True:
            while node:
                stack.append(node)
                node = node.left

            node = stack.pop()
            k -= 1

            if k == 0:
                return node.val

            node = node.right

```
### 501. Find Mode in Binary Search Tree

**LC 501 — Find Mode in Binary Search Tree**

```python
class Solution:
def findMode(self, root: Optional[TreeNode]) -> List[int]:
ans = []
prev = None
count = 0
max_count = 0

        def dfs(node):
            nonlocal prev, count, max_count

            if not node:
                return

            dfs(node.left)

            if node.val == prev:
                count += 1
            else:
                count = 1

            if count > max_count:
                max_count = count
                ans.clear()
                ans.append(node.val)
            elif count == max_count:
                ans.append(node.val)

            prev = node.val

            dfs(node.right)

        dfs(root)
        return ans

```
### 530. Minimum Absolute Difference in BST

**LC 530 — Minimum Absolute Difference in BST**

```python
class Solution:
def getMinimumDifference(self, root: Optional[TreeNode]) -> int:
prev = None
ans = float('inf')

        def dfs(node):
            nonlocal prev, ans

            if not node:
                return

            dfs(node.left)

            if prev is not None:
                ans = min(ans, node.val - prev)

            prev = node.val

            dfs(node.right)

        dfs(root)
        return ans

```
## Pattern 15: Tree DFS — Recursive Postorder Traversal

### 104. Maximum Depth of Binary Tree

**LC 104 — Maximum Depth of Binary Tree**

```python
class Solution:
def maxDepth(self, root: Optional[TreeNode]) -> int:
if not root:
return 0

        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))

```
### 110. Balanced Binary Tree

**LC 110 — Balanced Binary Tree**

```python
class Solution:
def isBalanced(self, root: Optional[TreeNode]) -> bool:
def height(node):
if not node:
return 0

            left = height(node.left)
            if left == -1:
                return -1

            right = height(node.right)
            if right == -1:
                return -1

            if abs(left - right) > 1:
                return -1

            return 1 + max(left, right)

        return height(root) != -1

```
### 124. Binary Tree Maximum Path Sum

**LC 124 — Binary Tree Maximum Path Sum**

```python
class Solution:
def maxPathSum(self, root: Optional[TreeNode]) -> int:
ans = float('-inf')

        def dfs(node):
            nonlocal ans

            if not node:
                return 0

            left = max(0, dfs(node.left))
            right = max(0, dfs(node.right))

            ans = max(ans, node.val + left + right)

            return node.val + max(left, right)

        dfs(root)
        return ans

```
### 145. Binary Tree Postorder Traversal

**LC 145 — Binary Tree Postorder Traversal**

```python
class Solution:
def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
ans = []

        def dfs(node):
            if not node:
                return

            dfs(node.left)
            dfs(node.right)
            ans.append(node.val)

        dfs(root)
        return ans

```
### 337. House Robber III

**LC 337 — House Robber III**

```python
class Solution:
def rob(self, root: Optional[TreeNode]) -> int:
def dfs(node):
if not node:
return (0, 0)

            left = dfs(node.left)
            right = dfs(node.right)

            rob = node.val + left[1] + right[1]
            skip = max(left) + max(right)

            return (rob, skip)

        return max(dfs(root))

```
### 366. Find Leaves of Binary Tree

**LC 366 — Find Leaves of Binary Tree**

```python
class Solution:
def findLeaves(self, root: Optional[TreeNode]) -> List[List[int]]:
ans = []

        def dfs(node):
            if not node:
                return -1

            level = 1 + max(dfs(node.left), dfs(node.right))

            if level == len(ans):
                ans.append([])

            ans[level].append(node.val)
            return level

        dfs(root)
        return ans

```
### 543. Diameter of Binary Tree

**LC 543 — Diameter of Binary Tree**

```python
class Solution:
def diameterOfBinaryTree(self, root: Optional[TreeNode]) -> int:
ans = 0

        def dfs(node):
            nonlocal ans

            if not node:
                return 0

            left = dfs(node.left)
            right = dfs(node.right)

            ans = max(ans, left + right)

            return 1 + max(left, right)

        dfs(root)
        return ans

```
### 863. All Nodes Distance K in Binary Tree

**LC 863 — All Nodes Distance K in Binary Tree**

```python
class Solution:
def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
from collections import defaultdict, deque

        graph = defaultdict(list)

        def build(node, parent=None):
            if not node:
                return

            if parent:
                graph[node].append(parent)
                graph[parent].append(node)

            build(node.left, node)
            build(node.right, node)

        build(root)

        q = deque([(target, 0)])
        seen = {target}

        while q:
            node, dist = q.popleft()

            if dist == k:
                return [node.val for node, _ in q] + [node.val]

            for nei in graph[node]:
                if nei not in seen:
                    seen.add(nei)
                    q.append((nei, dist + 1))

        return []

```
### 1110. Delete Nodes And Return Forest

**LC 1110 — Delete Nodes And Return Forest**

```python
class Solution:
def delNodes(self, root: Optional[TreeNode], to_delete: List[int]) -> List[TreeNode]:
delete = set(to_delete)
ans = []

        def dfs(node, is_root):
            if not node:
                return None

            deleted = node.val in delete

            if is_root and not deleted:
                ans.append(node)

            node.left = dfs(node.left, deleted)
            node.right = dfs(node.right, deleted)

            return None if deleted else node

        dfs(root, True)
        return ans

```
### 2458. Height of Binary Tree After Subtree Removal Queries

**LC 2458 — Height of Binary Tree After Subtree Removal Queries**

```python
class Solution:
def treeQueries(self, root: Optional[TreeNode], queries: List[int]) -> List[int]:
height = {}
ans = {}

        def dfs(node):
            if not node:
                return -1

            height[node] = 1 + max(dfs(node.left), dfs(node.right))
            return height[node]

        dfs(root)

        def dfs2(node, depth, best):
            if not node:
                return

            ans[node.val] = best

            left_height = height.get(node.left, -1)
            right_height = height.get(node.right, -1)

            dfs2(node.left, depth + 1, max(best, depth + 1 + right_height))
            dfs2(node.right, depth + 1, max(best, depth + 1 + left_height))

        dfs2(root, 0, 0)

        return [ans[x] for x in queries]

```
## Pattern 17: Tree — Lowest Common Ancestor (LCA) Finding

### 235. Lowest Common Ancestor of a Binary Search Tree

**LC 235 — Lowest Common Ancestor of a Binary Search Tree**

```python
class Solution:
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
while root:
if p.val < root.val and q.val < root.val:
root = root.left
elif p.val > root.val and q.val > root.val:
root = root.right
else:
return root

```
### 236. Lowest Common Ancestor of a Binary Tree

**LC 236 — Lowest Common Ancestor of a Binary Tree**

```python
class Solution:
def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
if not root or root == p or root == q:
return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root

        return left if left else right

```
## Pattern 18: Tree — Serialization and Deserialization

### 297. Serialize and Deserialize Binary Tree

**LC 297 — Serialize and Deserialize Binary Tree**

```python
class Codec:

    def serialize(self, root):
        vals = []

        def dfs(node):
            if not node:
                vals.append("#")
                return

            vals.append(str(node.val))
            dfs(node.left)
            dfs(node.right)

        dfs(root)
        return ",".join(vals)

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

            key = (
                node.val,
                dfs(node.left),
                dfs(node.right)
            )

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