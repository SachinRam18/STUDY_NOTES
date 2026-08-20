# Linked List — Complete Interview Preparation Guide (Java)

> **Goal:** After studying this guide, you should be able to see any new Linked List problem and immediately identify which pointer technique and algorithm to use.

---

## Table of Contents

- [Part 1 — Fundamentals](#part-1--linked-list-fundamentals)
- [Part 2 — Types of Linked Lists](#part-2--types-of-linked-lists)
- [Part 3 — Important Pointer Techniques](#part-3--important-linked-list-pointer-techniques)
- [Part 4 — Linked List Patterns](#part-4--linked-list-patterns)
- [Part 5 — Algorithm Toolkit](#part-5--linked-list-algorithm-toolkit)
- [Part 6 — LeetCode-style Questions](#part-6--important-leetcode-style-questions)
- [Part 7 — Pattern Recognition Cheat Sheet](#part-7--pattern-recognition-cheat-sheet)
- [Part 8 — Edge Cases](#part-8--edge-cases)
- [Part 9 — Common Mistakes](#part-9--common-linked-list-mistakes)
- [Part 10 — Interview Theory Questions](#part-10--interview-theory-questions)
- [Part 11 — Java Template](#part-11--java-linked-list-template)
- [Final 1-Page Revision](#final-revision--linked-list-1-page-revision)

---

# Part 1 — Linked List Fundamentals

---

## 1. What Is a Linked List?

A **linked list** is a linear data structure where each element (called a **node**) stores:
1. The **data** (the actual value)
2. A **pointer** (called `next`) pointing to the next node in the sequence

Unlike an array, the nodes are **not stored in contiguous memory**. Each node can be anywhere in memory, and they are connected only through these `next` pointers.

```text
head
 ↓
[10 | •] → [20 | •] → [30 | null]
```

The last node's `next` is `null`, meaning the list has ended.

---

## 2. Why Linked Lists When Arrays Exist?

Arrays have a fundamental limitation: **fixed size and expensive insertion/deletion**.

| Operation | Array | Linked List |
|---|---|---|
| Insert at beginning | O(n) — shift all elements | O(1) — update head |
| Insert at end | O(1) amortized | O(n) or O(1) with tail pointer |
| Delete at beginning | O(n) — shift all elements | O(1) |
| Delete from middle | O(n) | O(n) to find + O(1) to delete |
| Access by index | O(1) | O(n) |
| Memory | Fixed block | Flexible, dynamic |

**Use a Linked List when:**
- You insert/delete frequently (especially at the head)
- You don't know the size in advance
- You don't need random (index-based) access

---

## 3. Array vs Linked List

```text
Array:
[10][20][30][40]   ← stored in one contiguous block
  ↑               index 0, 1, 2, 3 are next to each other in memory

Linked List:
[10|•]  ....  [20|•]  ....  [30|null]
  ↑                             ↑
 addr 100                    addr 500   ← can be anywhere in memory
```

**Key differences:**
- **Arrays:** Random access (jump to index directly). Expensive insert/delete.
- **Linked Lists:** Sequential access only (must traverse). Cheap insert/delete once you're at the position.

---

## 4. Node Structure

In Java, a node is represented as a class:

```java
class ListNode {
    int val;        // the data stored in this node
    ListNode next;  // pointer to the next node (null if last)

    ListNode(int val) {
        this.val = val;
        this.next = null;
    }
}
```

Each `ListNode` object lives in heap memory. The `next` field holds the **memory address** (reference) of the next node.

---

## 5. Head and Tail

- **Head:** The first node of the list. If you lose the reference to head, you lose the entire list.
- **Tail:** The last node. Its `next` is `null`.

```text
head                    tail
 ↓                       ↓
10 → 20 → 30 → 40 → 50 → null
```

**Rule:** Always keep track of `head`. It is your entry point to the list.

---

## 6. How Nodes Are Stored in Memory

```text
Memory addresses (conceptual):

Address 100: [val=10, next=→500]
Address 500: [val=20, next=→300]
Address 300: [val=30, next=null]

Linked List: 10 → 20 → 30 → null
```

The nodes are scattered in memory. The `next` pointer "links" them together. This is why it is called a **linked** list.

---

## 7. How `next` Works

`next` is simply a reference (pointer) to the next `ListNode` object.

```java
ListNode a = new ListNode(10);
ListNode b = new ListNode(20);
ListNode c = new ListNode(30);

a.next = b;   // 10 → 20
b.next = c;   // 20 → 30
c.next = null; // 30 → null

// head is a
```

Changing `next` rewires the connection between nodes — this is the core of all linked list manipulation.

---

## 8. How Traversal Works

**Idea:** Start at `head`. Move to `curr.next` repeatedly until `curr` is `null`.

```java
ListNode curr = head;
while (curr != null) {
    System.out.print(curr.val + " ");
    curr = curr.next;   // move to the next node
}
```

**Diagram:**
```text
Step 1: curr = head = 10 → print 10
Step 2: curr = 10.next = 20 → print 20
Step 3: curr = 20.next = 30 → print 30
Step 4: curr = 30.next = null → stop
```

**Time: O(n) | Space: O(1)**

---

## 9. Insertion

### A. Insert at Beginning

**Idea:** Create a new node and make it point to the current head. The new node becomes the new head.

```text
Before: head → 10 → 20 → 30
Insert 5 at beginning
After:  head → 5 → 10 → 20 → 30
```

**Algorithm:**
1. Create `newNode` with value `val`.
2. Set `newNode.next = head`.
3. Update `head = newNode`.

```java
public ListNode insertAtBeginning(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    newNode.next = head;  // Step 1: new node points to old head
    return newNode;        // Step 2: new node IS the new head
}
```

**Time: O(1) | Space: O(1)**

---

### B. Insert at End

**Idea:** Traverse to the last node. Make its `next` point to the new node.

```text
Before: head → 10 → 20 → 30 → null
Insert 40 at end
After:  head → 10 → 20 → 30 → 40 → null
```

**Algorithm:**
1. Create `newNode`.
2. If `head == null`, return `newNode` (empty list case).
3. Traverse until `curr.next == null`.
4. Set `curr.next = newNode`.

```java
public ListNode insertAtEnd(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    if (head == null) return newNode;

    ListNode curr = head;
    while (curr.next != null) {  // go to last node
        curr = curr.next;
    }
    curr.next = newNode;  // attach new node
    return head;
}
```

**Time: O(n) | Space: O(1)**

---

### C. Insert at a Specific Position

**Idea:** Traverse to position `pos - 1`, then insert.

```text
Before: head → 10 → 20 → 30 → null
Insert 15 at position 2 (1-indexed)
After:  head → 10 → 15 → 20 → 30 → null
```

**Algorithm:**
1. If `pos == 1`, insert at beginning.
2. Traverse to node at position `pos - 1`.
3. Save `next` of that node.
4. Connect: `curr.next = newNode`, `newNode.next = savedNext`.

```java
public ListNode insertAtPosition(ListNode head, int val, int pos) {
    ListNode newNode = new ListNode(val);
    if (pos == 1) {
        newNode.next = head;
        return newNode;
    }
    ListNode curr = head;
    for (int i = 1; i < pos - 1 && curr != null; i++) {
        curr = curr.next;
    }
    if (curr == null) return head; // position out of range
    newNode.next = curr.next;
    curr.next = newNode;
    return head;
}
```

**Time: O(n) | Space: O(1)**

---

### D. Insert After a Given Node

**Idea:** Directly use the node reference — no traversal needed.

```java
public void insertAfterNode(ListNode node, int val) {
    if (node == null) return;
    ListNode newNode = new ListNode(val);
    newNode.next = node.next;  // new node points to what node was pointing to
    node.next = newNode;        // node now points to new node
}
```

**Diagram:**
```text
Before: ... → node → X → ...
After:  ... → node → newNode → X → ...
```

**Time: O(1) | Space: O(1)**

---

## 10. Deletion

### A. Delete from Beginning

**Idea:** Move head to the next node.

```text
Before: head → 10 → 20 → 30 → null
After:  head → 20 → 30 → null
```

```java
public ListNode deleteFromBeginning(ListNode head) {
    if (head == null) return null;
    return head.next;  // new head is the second node
}
```

**Time: O(1) | Space: O(1)**

---

### B. Delete from End

**Idea:** Traverse to the second-last node. Set its `next` to `null`.

```text
Before: head → 10 → 20 → 30 → null
After:  head → 10 → 20 → null
```

```java
public ListNode deleteFromEnd(ListNode head) {
    if (head == null || head.next == null) return null;

    ListNode curr = head;
    while (curr.next.next != null) {  // stop at second-last node
        curr = curr.next;
    }
    curr.next = null;  // disconnect last node
    return head;
}
```

**Why `curr.next.next != null`?** We need to stop at the *second-last* node so we can disconnect the last node by setting `curr.next = null`.

**Time: O(n) | Space: O(1)**

---

### C. Delete from Specific Position

```java
public ListNode deleteAtPosition(ListNode head, int pos) {
    if (head == null) return null;
    if (pos == 1) return head.next;  // delete head

    ListNode curr = head;
    for (int i = 1; i < pos - 1 && curr.next != null; i++) {
        curr = curr.next;
    }
    if (curr.next != null) {
        curr.next = curr.next.next;  // skip the target node
    }
    return head;
}
```

**Diagram:**
```text
Before: ... → prev → target → next → ...
After:  ... → prev → next → ...
(target is skipped and disconnected)
```

**Time: O(n) | Space: O(1)**

---

### D. Delete by Value

**Idea:** Find the node *before* the node with the target value. Skip the target.

```java
public ListNode deleteByValue(ListNode head, int val) {
    if (head == null) return null;
    if (head.val == val) return head.next;

    ListNode curr = head;
    while (curr.next != null) {
        if (curr.next.val == val) {
            curr.next = curr.next.next;  // skip the matching node
            return head;
        }
        curr = curr.next;
    }
    return head;  // value not found
}
```

**Time: O(n) | Space: O(1)**

---

## 11. Searching

**Idea:** Traverse and compare each node's value.

```java
public boolean search(ListNode head, int target) {
    ListNode curr = head;
    while (curr != null) {
        if (curr.val == target) return true;
        curr = curr.next;
    }
    return false;
}
```

**Time: O(n) | Space: O(1)**

---

## 12. Updating a Node

**Idea:** Find the node by value or position, then update.

```java
public void updateByPosition(ListNode head, int pos, int newVal) {
    ListNode curr = head;
    for (int i = 1; i < pos && curr != null; i++) {
        curr = curr.next;
    }
    if (curr != null) curr.val = newVal;
}
```

**Time: O(n) | Space: O(1)**

---

## 13. Length of a Linked List

```java
public int length(ListNode head) {
    int count = 0;
    ListNode curr = head;
    while (curr != null) {
        count++;
        curr = curr.next;
    }
    return count;
}
```

**Time: O(n) | Space: O(1)**

---

## 14. Reversing a Linked List

**Idea:** Use three pointers: `prev`, `curr`, `next`. Reverse the `next` pointer of each node one by one.

```text
Before: null  ←   10 → 20 → 30 → null
After:  null ← 10 ← 20 ← 30   (head now points to 30)
```

**Algorithm:**
1. `prev = null`, `curr = head`.
2. While `curr != null`:
   - Save `next = curr.next`.
   - Reverse the link: `curr.next = prev`.
   - Move forward: `prev = curr`, `curr = next`.
3. Return `prev` (new head).

```java
public ListNode reverse(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next;  // save next before breaking link
        curr.next = prev;           // reverse the link
        prev = curr;                // advance prev
        curr = next;                // advance curr
    }
    return prev;  // prev is the new head
}
```

**Dry Run:**
```text
Initial: prev=null, curr=10→20→30

Iter 1: next=20, 10.next=null, prev=10, curr=20
Iter 2: next=30, 20.next=10,   prev=20, curr=30
Iter 3: next=null, 30.next=20, prev=30, curr=null → stop

Return prev = 30  →  List: 30 → 20 → 10 → null ✓
```

**Time: O(n) | Space: O(1)**

---

# Part 2 — Types of Linked Lists

---

## A. Singly Linked List

Each node has exactly one pointer: `next`.

```text
Node structure:  [data | next]

Example:
head
 ↓
10 → 20 → 30 → null
```

**Advantages:**
- Simple to implement
- Less memory per node (only one pointer)

**Disadvantages:**
- Can only traverse forward
- To reach the previous node, you must restart from head (O(n))
- Deletion requires the previous node's reference

**Common use cases:** Stacks, simple queues, adjacency lists in graphs.

---

## B. Doubly Linked List

Each node has two pointers: `prev` and `next`.

```text
Node structure:  [prev | data | next]

Example:
null ← [10] ⇄ [20] ⇄ [30] → null
```

```java
class DoublyNode {
    int data;
    DoublyNode prev;
    DoublyNode next;

    DoublyNode(int data) {
        this.data = data;
        this.prev = null;
        this.next = null;
    }
}
```

### Forward Traversal

```java
DoublyNode curr = head;
while (curr != null) {
    System.out.print(curr.data + " ");
    curr = curr.next;
}
```

### Backward Traversal

```java
// First reach the tail
DoublyNode tail = head;
while (tail.next != null) tail = tail.next;

// Now traverse backward
DoublyNode curr = tail;
while (curr != null) {
    System.out.print(curr.data + " ");
    curr = curr.prev;
}
```

### Insertion in Doubly Linked List

```java
public void insertAfter(DoublyNode node, int val) {
    DoublyNode newNode = new DoublyNode(val);
    newNode.next = node.next;
    newNode.prev = node;
    if (node.next != null) {
        node.next.prev = newNode;  // update the next node's prev pointer
    }
    node.next = newNode;
}
```

### Deletion in Doubly Linked List

**Why deletion is easier when you have the node itself:**

In a singly linked list, to delete node X, you need the node *before* X (to update `prev.next = X.next`). But you can't go backward → you must traverse from head.

In a doubly linked list, node X knows its own `prev`. So:

```java
public void deleteNode(DoublyNode node) {
    if (node.prev != null) node.prev.next = node.next;
    if (node.next != null) node.next.prev = node.prev;
    // node is now disconnected — O(1) deletion!
}
```

This is a true **O(1) deletion** (given the node reference). No traversal needed.

### Reversal

```java
public DoublyNode reverse(DoublyNode head) {
    DoublyNode curr = head;
    DoublyNode temp = null;
    while (curr != null) {
        temp = curr.prev;
        curr.prev = curr.next;  // swap prev and next
        curr.next = temp;
        curr = curr.prev;       // move to what was "next"
    }
    return (temp != null) ? temp.prev : head;
}
```

| Operation | Singly | Doubly |
|---|---|---|
| Forward traversal | O(n) | O(n) |
| Backward traversal | O(n) restart | O(n) via prev |
| Insert after node | O(1) | O(1) |
| Delete given node | O(n) | O(1) |
| Memory per node | Less | More (extra `prev`) |

**Disadvantages:** More memory per node, more pointers to maintain (bugs if prev not updated).

**Use cases:** Browsers (back/forward), LRU Cache, text editors (undo/redo), deques.

---

## C. Circular Linked List

The last node's `next` points back to the head (instead of `null`).

```text
Circular Singly:
head
 ↓
10 → 20 → 30
↑          ↓
└──────────┘

Circular Doubly:
null ← [10] ⇄ [20] ⇄ [30] (30.next → 10, 10.prev → 30)
```

### Why `curr != null` CANNOT be the stopping condition

In a regular list, you stop when `curr == null` because the last node's `next` is `null`. But in a circular list, **no node has `null` as its next**. The last node points back to the head. So `curr` will never become `null` — your loop will run forever!

**Correct stopping condition:**

```java
// Option 1: Stop when we return to head
ListNode curr = head;
do {
    System.out.print(curr.val + " ");
    curr = curr.next;
} while (curr != head);  // stop when we've gone full circle

// Option 2: Count nodes
```

### Traversal

```java
public void traverse(ListNode head) {
    if (head == null) return;
    ListNode curr = head;
    do {
        System.out.print(curr.val + " ");
        curr = curr.next;
    } while (curr != head);
}
```

### Insertion in Circular List

**Insert at beginning:**
```java
public ListNode insertAtBeginning(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    if (head == null) {
        newNode.next = newNode;  // points to itself
        return newNode;
    }
    // Find tail
    ListNode tail = head;
    while (tail.next != head) tail = tail.next;

    newNode.next = head;
    tail.next = newNode;  // tail must now point to new head
    return newNode;
}
```

### Deletion in Circular List

```java
public ListNode delete(ListNode head, int val) {
    if (head == null) return null;

    // If head is the only node
    if (head.next == head && head.val == val) return null;

    ListNode curr = head;
    ListNode prev = null;

    // Find the node to delete
    do {
        if (curr.val == val) {
            if (prev != null) prev.next = curr.next;
            else {
                // Deleting head: find tail and update it
                ListNode tail = head;
                while (tail.next != head) tail = tail.next;
                head = head.next;
                tail.next = head;
            }
            return head;
        }
        prev = curr;
        curr = curr.next;
    } while (curr != head);

    return head;  // value not found
}
```

### Advantages of Circular List
- Can start traversal from any node
- Useful for round-robin scheduling (OS, multiplayer games)
- Efficient queue implementation (no need for a separate tail pointer)

### Disadvantages
- Complex implementation
- Easy to create infinite loops
- Need careful condition to stop traversal

**Real-world applications:** CPU scheduling (round-robin), circular buffers, multiplayer game turns.

---

# Part 3 — Important Linked List Pointer Techniques

---

## 1. Previous / Current / Next (The Reversal Trio)

This is the fundamental technique for **in-place reversal**. You need three pointers because when you reverse `curr.next`, you lose access to the rest of the list.

```text
Step-by-step reversal of 10 → 20 → 30:

Start:
prev=null   curr=10   next=?

Step 1: Save next = curr.next = 20
Step 2: Reverse: curr.next = prev (10.next = null)
Step 3: Move prev forward: prev = curr (prev=10)
Step 4: Move curr forward: curr = next (curr=20)

Now:
prev=10   curr=20   next=?
```

**Why order matters:**

```java
ListNode next = curr.next;  // 1. Save FIRST — if you do curr.next = prev first, you lose the rest of the list
curr.next = prev;            // 2. Reverse the link
prev = curr;                 // 3. Advance prev
curr = next;                 // 4. Advance curr using the saved next
```

If you do step 2 before step 1, `curr.next` is overwritten and you can never reach the rest of the list.

---

## 2. Fast and Slow Pointers (Floyd's Algorithm)

**Concept:** Two pointers start at head. Slow moves 1 step per iteration, fast moves 2 steps per iteration.

```text
List: 1 → 2 → 3 → 4 → 5

After 1 iteration: slow=2, fast=3
After 2 iterations: slow=3, fast=5
After 3 iterations: slow=4, fast=null → stop
→ slow is at position 3 (middle of 5 nodes)
```

### Why does fast move 2 instead of 3 or 4?

**For finding the middle:**

If fast moves at speed 2 and slow at speed 1, when fast covers n nodes, slow has covered n/2 nodes → slow is at the middle. Simple ratio: 2:1.

If fast moved at speed 3, when it covers n nodes, slow is at n/3 — that's one-third, not half.

**For cycle detection:**

The key is **relative speed**. Fast gains **1 node per iteration** over slow (2 - 1 = 1). This means:
- If a cycle exists of length L, fast will lap slow in exactly L iterations inside the cycle.
- They will always meet.

If fast moved at speed 4, it gains 3 per iteration. But this can cause fast to *skip over* slow if the cycle length L doesn't divide evenly with the step size — in some configurations, fast and slow never meet.

**Example showing speed-4 can fail:**

Cycle: → A → B → A (length 2)
- Slow at A, fast at A. After 1 iter: slow=B, fast=B. They meet. (OK here)
- But with some cycle lengths and start positions, fast can perpetually skip slow.

**Speed 2 is the minimum speed that guarantees meeting**, which is why it's the standard.

**Relative speed = fast - slow = 2 - 1 = 1**

Since fast gains exactly 1 position per iteration, after at most L iterations inside any cycle of length L, they meet. No skipping possible.

### Use Cases

**Find Middle:**
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow is at the middle
```

**Detect Cycle:**
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) return true; // cycle exists
}
return false;
```

**Find Cycle Start:** (Detailed in Part 5)

**Check Palindrome:** Find middle → reverse second half → compare.

---

## 3. Two Pointer Gap Technique

**Concept:** Keep two pointers with a constant gap of `n` nodes between them. When the faster pointer (ahead by n nodes) reaches the end, the slower pointer is at the target position.

```text
Find 2nd node from end in: 1 → 2 → 3 → 4 → 5

Move fast 2 steps ahead:
fast=3, slow=1   (gap = 2)

Advance both until fast.next == null:
Iter 1: fast=4, slow=2
Iter 2: fast=5, slow=3

fast.next == null → slow is at 3rd node from end? No...

Wait — for nth from end, move fast n+1 steps ahead:
Move fast 3 steps (n+1 = 3) ahead:
fast=4, slow=1

Advance both:
Iter 1: fast=5, slow=2
fast.next == null → slow = 2, which is 2 steps before the end → slow.next is the 2nd from end!
```

**Why the gap remains constant:** Both pointers advance at the same speed (1 step each iteration). So the distance between them never changes after the initial setup.

**For nth from end:** Move fast `n` steps ahead first. Then advance both. When fast reaches the last node, slow is at the (n+1)th-from-end node — i.e., the node *before* the nth-from-end node.

```java
// Remove nth node from end
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode slow = dummy, fast = dummy;

// Move fast n+1 steps ahead (we use dummy so slow stops before the target)
for (int i = 0; i <= n; i++) {
    fast = fast.next;
}

while (fast != null) {
    slow = slow.next;
    fast = fast.next;
}

slow.next = slow.next.next; // delete the nth node from end
return dummy.next;
```

---

## 4. Dummy Node Technique

**Concept:** Create a fake node (`dummy`) before the head. Return `dummy.next` at the end.

```text
dummy → head → ...
```

**Why it helps:**

When you delete or modify the head node, you normally need special handling (the head pointer itself must be updated). With a dummy node:
- `dummy.next` is always the real head
- You treat the head exactly like any other node
- `slow.next = slow.next.next` works even for deleting the head (because `dummy` plays the role of "node before head")
- At the end: `return dummy.next`

**When to use a dummy node:**
- Removing the nth node from end (to handle deleting head)
- Removing duplicates
- Merging two sorted lists (avoids special case for first node)
- Partitioning a list
- Reordering a list

```java
// Template with dummy node
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode curr = dummy; // start from dummy, not head

// ... manipulate the list ...

return dummy.next; // return the real head
```

---

## 5. Multiple Pointer Technique — Decision Guide

```text
Problem Type                   Pointers to Use
─────────────────────────────────────────────────────
Reverse a list                 prev + curr + next
Reverse a sublist              prev + curr + next + (sublist head/tail tracking)
Detect cycle                   slow + fast
Find middle                    slow + fast
Find cycle start               slow + fast (Phase 1) + reset slow to head (Phase 2)
Nth node from end              fast (ahead by n) + slow
Remove nth from end            dummy + fast (ahead by n+1) + slow
Merge two sorted lists         p1 + p2 + dummy
Intersection of two lists      pA + pB
Palindrome check               slow + fast → find middle, then prev+curr+next to reverse
Reorder list                   slow + fast (middle) + prev+curr+next (reverse) + p1+p2 (merge)
Swap pairs                     dummy + prev + first + second
Partition list                 lessHead/lessTail + greaterHead/greaterTail
Odd-even grouping              odd + even + evenHead
Rotate list                    length traverse + tail + new head
```

---

# Part 4 — Linked List Patterns

---

# Pattern 71 — In-Place Reversal

> **When to recognize:** The problem asks you to reverse a list, a sub-range of a list, or process nodes in reverse order — without using extra memory.

---

## LC 206 — Reverse Linked List

**LeetCode #206 | Easy**

### 1. Problem Understanding

Reverse the entire linked list in-place and return the new head.

```text
Input:  1 → 2 → 3 → 4 → 5 → null
Output: 5 → 4 → 3 → 2 → 1 → null
```

### 2. Pattern Recognition

> You need to change the direction of every `next` pointer. This is an in-place pointer manipulation problem. Use `prev + curr + next`.

### 3. Brute Force

Store all values in an array, then overwrite nodes in reverse order.

```java
public ListNode reverseList(ListNode head) {
    List<Integer> vals = new ArrayList<>();
    ListNode curr = head;
    while (curr != null) { vals.add(curr.val); curr = curr.next; }
    curr = head;
    for (int i = vals.size() - 1; i >= 0; i--) { curr.val = vals.get(i); curr = curr.next; }
    return head;
}
```

```
Time: O(n) | Space: O(n)
```

This modifies values, not structure. Not ideal for interviews.

### 4. Optimal Approach

Reverse the `next` pointers themselves. Three pointers: `prev`, `curr`, `next`.

### 5. Pointer Diagram

```text
Initial:
prev=null   curr=1→2→3→4→5

After Iter 1:
prev=1      curr=2→3→4→5
1 → null

After Iter 2:
prev=2      curr=3→4→5
2 → 1 → null

... after all iterations:
prev=5      curr=null
5 → 4 → 3 → 2 → 1 → null
```

### 6. Algorithm

1. Initialize `prev = null`, `curr = head`.
2. While `curr != null`:
   - Save `next = curr.next`
   - Set `curr.next = prev`
   - Set `prev = curr`
   - Set `curr = next`
3. Return `prev` (new head).

### 7. Java Code

```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    while (curr != null) {
        ListNode next = curr.next; // save next
        curr.next = prev;          // reverse the pointer
        prev = curr;               // move prev forward
        curr = next;               // move curr forward
    }
    return prev; // prev is the new head
}
```

### 8. Dry Run

```text
List: 1 → 2 → 3 → null

Start: prev=null, curr=1

Iter 1: next=2, 1.next=null, prev=1, curr=2
        [null ← 1]   [2 → 3 → null]

Iter 2: next=3, 2.next=1, prev=2, curr=3
        [null ← 1 ← 2]   [3 → null]

Iter 3: next=null, 3.next=2, prev=3, curr=null
        [null ← 1 ← 2 ← 3]
        curr == null → stop

Return prev = 3  →  3 → 2 → 1 → null ✓
```

### 9. Edge Cases

- `head == null` → return `null` ✓ (while loop doesn't execute, prev remains null)
- Single node → return that node ✓

### 10. Complexity

```
Time: O(n) — traverse once
Space: O(1) — three pointers only
```

**Interview Answer:** "I use three pointers: prev, curr, and next. For each node, I save its next before breaking the link. Then I redirect curr.next to prev, and advance both pointers. At the end, prev is the new head. This is O(n) time and O(1) space."

---

## LC 92 — Reverse Linked List II

**LeetCode #92 | Medium**

### 1. Problem Understanding

Reverse only the nodes from position `left` to position `right` (1-indexed). Do it in one pass.

```text
Input:  1 → 2 → 3 → 4 → 5, left=2, right=4
Output: 1 → 4 → 3 → 2 → 5
```

### 2. Pattern Recognition

> Only reverse a sublist. Need to track: the node just before the sublist, the head of the sublist (which becomes the tail after reversal), and reconnect both ends.

### 3. Brute Force

Traverse, store positions, extract sublist values, reverse them, rewrite.

```
Time: O(n) | Space: O(n)
```

### 4. Optimal Approach

One-pass. Navigate to position `left-1`. Reverse the sublist `[left..right]`. Reconnect.

### 5. Pointer Diagram

```text
Initial (left=2, right=4):
dummy → 1 → 2 → 3 → 4 → 5

Step 1: Navigate to node before left:
prevLeft → 1 (position left-1 = 1)
currLeft → 2 (this will become the tail of the reversed sublist)

Step 2: Reverse 4 nodes starting from currLeft:
         sublistTail = 2 (it moves to the end of reversed sublist)
         After reversing: 4 → 3 → 2

Step 3: Reconnect:
         prevLeft.next = sublistHead (4)
         currLeft.next = nodeAfterRight (5)

Result: dummy → 1 → 4 → 3 → 2 → 5
```

### 6. Algorithm

1. Create `dummy` node before head. Set `prev = dummy`.
2. Move `prev` to position `left - 1`.
3. `currLeft = prev.next` (this is the start of the sublist — it will become the tail).
4. Reverse `right - left` times using prev/curr/next, starting from `currLeft`.
5. Reconnect: `prev.next = reversed_head`, `currLeft.next = node_after_right`.

### 7. Java Code

```java
public ListNode reverseBetween(ListNode head, int left, int right) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prevLeft = dummy;

    // Step 1: Move prevLeft to node just before 'left'
    for (int i = 1; i < left; i++) {
        prevLeft = prevLeft.next;
    }

    // currLeft will become the tail of the reversed sublist
    ListNode currLeft = prevLeft.next;
    ListNode prev = null;
    ListNode curr = currLeft;

    // Step 2: Reverse (right - left + 1) nodes
    for (int i = 0; i < right - left + 1; i++) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    // After loop: prev = new head of reversed sublist, curr = node after 'right'

    // Step 3: Reconnect
    prevLeft.next = prev;       // connect node before sublist to new sublist head
    currLeft.next = curr;       // connect old sublist head (now tail) to rest of list

    return dummy.next;
}
```

### 8. Dry Run

```text
List: 1 → 2 → 3 → 4 → 5, left=2, right=4
dummy → 1 → 2 → 3 → 4 → 5

Move prevLeft to position 1: prevLeft = node(1)
currLeft = node(2)

Reverse 3 nodes (right-left+1=3):
  Iter 1: next=3, 2.next=null, prev=2, curr=3
  Iter 2: next=4, 3.next=2,   prev=3, curr=4
  Iter 3: next=5, 4.next=3,   prev=4, curr=5
  Loop ends: prev=4, curr=5

Reconnect:
  node(1).next = prev = node(4)      → 1 → 4 → 3 → 2
  currLeft (node(2)).next = curr = node(5)  → 2 → 5

Result: dummy → 1 → 4 → 3 → 2 → 5 ✓
```

### 9. Edge Cases

- `left == right` → no reversal needed (still works, 1-iteration reverse = no change)
- `left == 1` → dummy handles reconnection of the new head

### 10. Complexity

```
Time: O(n)
Space: O(1)
```

---

## LC 25 — Reverse Nodes in k-Group

**LeetCode #25 | Hard**

### 1. Problem Understanding

Reverse every k consecutive nodes. If fewer than k nodes remain at the end, leave them as-is.

```text
Input:  1 → 2 → 3 → 4 → 5, k=2
Output: 2 → 1 → 4 → 3 → 5

Input:  1 → 2 → 3 → 4 → 5, k=3
Output: 3 → 2 → 1 → 4 → 5
```

### 2. Pattern Recognition

> Repeated in-place reversal in chunks of k. Need to: check if k nodes exist, reverse them, reconnect, repeat.

### 3. Brute Force

Collect all values, reverse in groups, rewrite. `O(n)` space.

### 4. Optimal Approach

Iteratively reverse k nodes at a time. Track the tail of the previous group for reconnection.

### 5. Pointer Diagram

```text
dummy → 1 → 2 → 3 → 4 → 5, k=2

GroupTail (prev group tail) = dummy

Group 1: nodes 1, 2
  kTail = node(2) (end of this group)
  Reverse 1→2 to get 2→1
  Reconnect: dummy.next = 2, node(1).next = node(3)
  GroupTail = node(1) (which is now the tail of group 1)

Group 2: nodes 3, 4
  Reverse 3→4 to get 4→3
  Reconnect: node(1).next = 4, node(3).next = node(5)
  GroupTail = node(3)

Group 3: only node(5), fewer than k=2, leave as-is.

Result: dummy → 2 → 1 → 4 → 3 → 5 ✓
```

### 6. Algorithm

1. Create `dummy`. Set `groupPrev = dummy`.
2. Loop:
   - Check if k nodes remain starting from `groupPrev.next`. If not, stop.
   - Save `kTail` = k-th node from `groupPrev`.
   - Save `nextGroupStart = kTail.next`.
   - Reverse the k nodes between `groupPrev` and `kTail`.
   - Reconnect: `groupPrev.next = kTail (new group head)`, `old_group_head.next = nextGroupStart`.
   - Advance `groupPrev` to `old_group_head` (now the group tail).

### 7. Java Code

```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode groupPrev = dummy;

    while (true) {
        // Check if k nodes remain
        ListNode kTail = getKth(groupPrev, k);
        if (kTail == null) break;

        ListNode groupStart = groupPrev.next;
        ListNode nextGroupStart = kTail.next;

        // Reverse k nodes
        ListNode prev = nextGroupStart; // reversed group will connect to next group
        ListNode curr = groupStart;
        while (curr != nextGroupStart) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        // After reversal: prev = kTail (new head of group), groupStart = tail of group

        // Reconnect
        groupPrev.next = kTail;    // kTail is the new head of this group
        groupPrev = groupStart;    // groupStart is now the tail of this group

        // Move to next group
    }
    return dummy.next;
}

// Helper: get the k-th node from 'curr'
private ListNode getKth(ListNode curr, int k) {
    while (curr != null && k > 0) {
        curr = curr.next;
        k--;
    }
    return curr;
}
```

### 8. Dry Run

```text
List: 1→2→3→4→5, k=2

dummy → 1 → 2 → 3 → 4 → 5
groupPrev = dummy

Iter 1:
  kTail = getKth(dummy, 2) = node(2)
  groupStart = 1, nextGroupStart = 3
  Reverse with prev=3: 2.next=3(already), then...
    curr=1: next=2, 1.next=2(prev=2... wait, prev=nextGroupStart=3)
    Actually: prev=3, curr=1
    Iter: next=2, 1.next=3, prev=1, curr=2
    Iter: next=3, 2.next=1, prev=2, curr=3 (==nextGroupStart, stop)
  After: 2→1→3→4→5
  groupPrev.next = kTail = 2 → dummy→2→1→3→4→5
  groupPrev = groupStart = node(1)

Iter 2:
  kTail = getKth(node(1), 2) = node(4)
  groupStart = 3, nextGroupStart = 5
  Reverse: 4→3→5
  groupPrev.next = 4 → 1→4→3→5
  groupPrev = node(3)

Iter 3:
  kTail = getKth(node(3), 2) = null (only node(5) remains) → break

Result: 2→1→4→3→5 ✓
```

### 9. Edge Cases

- `k == 1` → no change needed (still correct, reversal of 1 node = same)
- Remaining nodes < k → left as-is ✓

### 10. Complexity

```
Time: O(n) — each node is visited twice (once to find kTail, once to reverse)
Space: O(1)
```

---

## LC 83 — Remove Duplicates from Sorted List

**LeetCode #83 | Easy**

### 1. Problem Understanding

Remove all duplicate values from a sorted linked list so each value appears only once.

```text
Input:  1 → 1 → 2 → 3 → 3 → null
Output: 1 → 2 → 3 → null
```

### 2. Pattern Recognition

> Sorted list means duplicates are adjacent. Scan and skip consecutive duplicates using one pointer.

### 7. Java Code

```java
public ListNode deleteDuplicates(ListNode head) {
    ListNode curr = head;
    while (curr != null && curr.next != null) {
        if (curr.val == curr.next.val) {
            curr.next = curr.next.next; // skip the duplicate
        } else {
            curr = curr.next; // only advance if no duplicate
        }
    }
    return head;
}
```

**Important:** Only advance `curr` when there's no duplicate. If there is, keep `curr` fixed and keep removing the next node (handles 3+ duplicates).

### Dry Run

```text
1 → 1 → 1 → 2 → 3 → 3

curr=1: 1==1 → skip: 1→1→2→3→3
curr=1: 1==1 → skip: 1→2→3→3
curr=1: 1!=2 → advance: curr=2
curr=2: 2!=3 → advance: curr=3
curr=3: 3==3 → skip: 3→null
curr=3: curr.next==null → stop

Result: 1→2→3 ✓
```

```
Time: O(n) | Space: O(1)
```

---

## LC 82 — Remove Duplicates from Sorted List II

**LeetCode #82 | Medium**

### 1. Problem Understanding

Remove ALL nodes that have duplicate values. Only keep nodes with unique values.

```text
Input:  1 → 2 → 3 → 3 → 4 → 4 → 5
Output: 1 → 2 → 5

Input:  1 → 1 → 1 → 2 → 3
Output: 2 → 3
```

### 2. Pattern Recognition

> Use dummy node + `prev` pointer. When a duplicate value is found, skip all nodes with that value by advancing past them.

### 7. Java Code

```java
public ListNode deleteDuplicates(ListNode head) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prev = dummy;

    ListNode curr = head;
    while (curr != null) {
        // Check if curr starts a group of duplicates
        if (curr.next != null && curr.val == curr.next.val) {
            int dupVal = curr.val;
            // Skip all nodes with this duplicate value
            while (curr != null && curr.val == dupVal) {
                curr = curr.next;
            }
            prev.next = curr; // connect prev to the node after all duplicates
        } else {
            prev = curr;       // curr is unique, move prev forward
            curr = curr.next;
        }
    }
    return dummy.next;
}
```

### Dry Run

```text
dummy → 1 → 2 → 3 → 3 → 4 → 4 → 5
prev = dummy, curr = 1

curr=1: 1!=2 → unique, prev=1, curr=2
curr=2: 2!=3 → unique, prev=2, curr=3
curr=3: 3==3 → duplicate! dupVal=3
  Skip: curr=3→3→4, stop when curr.val!=3 → curr=4
  prev(2).next = 4  →  dummy→1→2→4→4→5
curr=4: 4==4 → duplicate! dupVal=4
  Skip: curr=4→4→5, stop → curr=5
  prev(2).next = 5  →  dummy→1→2→5
curr=5: 5==null → unique, prev=5, curr=null → stop

Result: 1→2→5 ✓
```

### Edge Cases

- All nodes are duplicates → return `null` (dummy.next will be null) ✓
- Head is a duplicate → dummy node handles this ✓

```
Time: O(n) | Space: O(1)
```

---

## LC 234 — Palindrome Linked List

**LeetCode #234 | Easy**

### 1. Problem Understanding

Return `true` if the linked list is a palindrome (reads the same forward and backward).

```text
Input:  1 → 2 → 2 → 1  → true
Input:  1 → 2 → 3       → false
```

### 2. Pattern Recognition

> Three-step combined pattern: Find Middle (Fast+Slow) → Reverse second half (prev+curr+next) → Compare both halves.

### 3. Brute Force

Copy all values to an array, use two-pointer comparison.

```
Time: O(n) | Space: O(n)
```

### 4. Optimal Approach

In-place with O(1) space:

### 6. Algorithm

1. Find middle using fast+slow pointers.
2. Reverse the second half of the list.
3. Compare first half and reversed second half node by node.
4. (Optional: restore the list.)

### 7. Java Code

```java
public boolean isPalindrome(ListNode head) {
    if (head == null || head.next == null) return true;

    // Step 1: Find middle
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    // slow is now at the middle (second half starts at slow for odd, slow for even)

    // Step 2: Reverse second half
    ListNode secondHalf = reverse(slow);

    // Step 3: Compare
    ListNode p1 = head, p2 = secondHalf;
    boolean result = true;
    while (p2 != null) {
        if (p1.val != p2.val) {
            result = false;
            break;
        }
        p1 = p1.next;
        p2 = p2.next;
    }

    // Step 4: Restore (good practice)
    reverse(secondHalf);

    return result;
}

private ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### 8. Dry Run

```text
List: 1 → 2 → 2 → 1

Find middle:
slow: 1→2, fast: 1→2→1 → fast.next=null → stop
slow = node(2) [second 2]

Reverse second half from node(2):
2 → 1 → null  reversed to  1 → 2 → null
secondHalf = node(1)

Compare:
p1=1, p2=1 → match
p1=2, p2=2 → match
p2=null → stop
Result: true ✓
```

### 9. Edge Cases

- `head == null` or single node → `true`
- Even length list: middle is exactly at the start of the second half

```
Time: O(n) | Space: O(1)
```

**Interview Answer:** "I split the problem into three steps: find the middle using fast and slow pointers, reverse the second half in-place, then compare the first and second halves. This runs in O(n) time and O(1) space."

---

# Pattern 72 — Merging Two Sorted Lists

---

## LC 21 — Merge Two Sorted Lists

**LeetCode #21 | Easy**

### 1. Problem Understanding

Merge two sorted linked lists into one sorted linked list. Do not create new nodes — reuse existing nodes.

```text
List1: 1 → 3 → 5 → null
List2: 2 → 4 → 6 → null

Result: 1 → 2 → 3 → 4 → 5 → 6 → null
```

### 2. Pattern Recognition

> Two pointers, one on each list. Pick the smaller head and attach it. Advance that pointer. Use a dummy node to simplify the first attachment.

### 3. Brute Force

Collect all values from both lists, sort, create a new list.

```
Time: O((n+m) log(n+m)) | Space: O(n+m)
```

### 4. Optimal Approach

Two pointers. Compare heads of each list. Attach the smaller one to the merged list. Advance only the pointer that was attached.

**Why we don't need to create new nodes:** We're just changing the `next` pointers of the existing nodes. The data values remain unchanged.

### 5. Pointer Diagram

```text
dummy → ?
p1 → [1] → [3] → [5] → null
p2 → [2] → [4] → [6] → null

Step 1: p1.val(1) < p2.val(2) → attach p1
dummy → 1, p1 → 3

Step 2: p2.val(2) < p1.val(3) → attach p2
dummy → 1 → 2, p2 → 4

Step 3: p1.val(3) < p2.val(4) → attach p1
dummy → 1 → 2 → 3, p1 → 5

... continue ...
```

### 6. Algorithm

1. Create `dummy = new ListNode(0)`. Set `curr = dummy`.
2. While both `p1` and `p2` are not null:
   - If `p1.val <= p2.val`: `curr.next = p1`, advance `p1`.
   - Else: `curr.next = p2`, advance `p2`.
   - Advance `curr`.
3. Attach remaining nodes: `curr.next = (p1 != null) ? p1 : p2`.
4. Return `dummy.next`.

### 7. Java Code

```java
public ListNode mergeTwoLists(ListNode list1, ListNode list2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (list1 != null && list2 != null) {
        if (list1.val <= list2.val) {
            curr.next = list1;
            list1 = list1.next;
        } else {
            curr.next = list2;
            list2 = list2.next;
        }
        curr = curr.next;
    }

    // Attach the remaining portion (no need to loop — it's already linked)
    curr.next = (list1 != null) ? list1 : list2;

    return dummy.next;
}
```

### 8. Dry Run

```text
list1: 1 → 3 → 5 | list2: 2 → 4

dummy → curr

1 < 2: curr→1, list1=3
2 < 3: curr→2, list2=4
3 < 4: curr→3, list1=5
4 < 5: curr→4, list2=null → stop while loop

list2==null, so curr.next = list1 = 5
Result: dummy → 1 → 2 → 3 → 4 → 5 ✓
```

### 9. Edge Cases

- One or both lists empty → `curr.next = remaining` handles this
- Duplicate values → `<=` ensures stability

### 10. Complexity

```
Time:  O(n + m) — each node is visited once
Space: O(1) — no new nodes created
```

**Interview Answer:** "I use a dummy node as the merged list's head. Two pointers traverse each list. I always attach the smaller head to the merged list and advance that pointer. When one list is exhausted, I attach the remaining portion directly (it's already sorted). O(n+m) time, O(1) space."

---

# Pattern 73 — Addition of Numbers

---

## LC 2 — Add Two Numbers

**LeetCode #2 | Medium**

### 1. Problem Understanding

Two non-empty linked lists represent two non-negative integers. Digits are stored in **reverse order** (least significant digit first). Return the sum as a linked list in the same format.

```text
List1: 2 → 4 → 3   represents 342
List2: 5 → 6 → 4   represents 465

342 + 465 = 807

Result: 7 → 0 → 8   represents 807
```

### 2. Pattern Recognition

> Digit-by-digit addition from least significant digit (head) to most significant. Use a carry variable. Use dummy node for result.

### 3. Why lists are stored in reverse

Storing digits in reverse (least significant first) means the **head of each list is aligned** — they both start at the units place. This makes addition straightforward: add corresponding nodes from head to tail, handling carry.

If stored in forward order, the lengths might differ and alignment would be complex.

### 4. Algorithm

1. Initialize `carry = 0`, create `dummy` node.
2. While `l1 != null` OR `l2 != null` OR `carry != 0`:
   - `val1 = (l1 != null) ? l1.val : 0`
   - `val2 = (l2 != null) ? l2.val : 0`
   - `sum = val1 + val2 + carry`
   - `carry = sum / 10` (integer division)
   - `digit = sum % 10`
   - Append `new ListNode(digit)` to result.
   - Advance `l1`, `l2` if not null.
3. Return `dummy.next`.

### 5. Java Code

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    int carry = 0;

    while (l1 != null || l2 != null || carry != 0) {
        int val1 = (l1 != null) ? l1.val : 0;
        int val2 = (l2 != null) ? l2.val : 0;

        int sum = val1 + val2 + carry;
        carry = sum / 10;      // integer division: 10/10=1, 9/10=0
        int digit = sum % 10;  // remainder: 10%10=0, 9%10=9

        curr.next = new ListNode(digit);
        curr = curr.next;

        if (l1 != null) l1 = l1.next;
        if (l2 != null) l2 = l2.next;
    }

    return dummy.next;
}
```

### 6. Dry Run

```text
l1: 2→4→3  (342)
l2: 5→6→4  (465)

Iter 1: val1=2, val2=5, sum=7, carry=0, digit=7 → result: 7
Iter 2: val1=4, val2=6, sum=10, carry=1, digit=0 → result: 7→0
Iter 3: val1=3, val2=4, sum=7+1=8, carry=0, digit=8 → result: 7→0→8
l1,l2 both null, carry=0 → stop

Result: 7→0→8 (807) ✓
```

### 7. Edge Cases

- Different lengths (e.g., [9,9] + [1]) → `(l1 != null) ? l1.val : 0` handles shorter list
- Final carry (e.g., 99+1=100) → `carry != 0` in while condition handles final carry node

```
Time: O(max(n, m)) | Space: O(max(n, m)) for result
```

---

## LC 369 — Plus One Linked List

**LeetCode #369 | Medium**

### 1. Problem Understanding

A non-negative integer is represented as a linked list in **forward order** (most significant digit first). Add 1 to it and return the result.

```text
Input: 1 → 2 → 3  (123)
Output: 1 → 2 → 4  (124)

Input: 9 → 9  (99)
Output: 1 → 0 → 0  (100)
```

### 2. Why It's Different from LC 2

Here digits are in **forward order**, so carry propagates **rightward to leftward** (from the least significant to most significant — which is the tail to head direction). We must work backward.

### 3. Approach 1 — Reverse the List

1. Reverse the list.
2. Add 1 using the same carry technique as LC 2 (but only one list).
3. Reverse back.

```java
public ListNode plusOne(ListNode head) {
    // Step 1: Reverse
    ListNode reversed = reverse(head);

    // Step 2: Add 1
    ListNode curr = reversed;
    int carry = 1;
    while (curr != null && carry > 0) {
        int sum = curr.val + carry;
        curr.val = sum % 10;
        carry = sum / 10;
        if (curr.next == null && carry > 0) {
            curr.next = new ListNode(carry);
            carry = 0;
        }
        curr = curr.next;
    }

    // Step 3: Reverse back
    return reverse(reversed);
}
```

### 4. Approach 2 — Find Rightmost Non-9 Digit (More Elegant)

**Insight:** Adding 1 only changes the trailing 9s. All trailing 9s become 0, and the digit just before them increments by 1.

```text
1 → 2 → 9 → 9  +1 = 1 → 3 → 0 → 0
Find rightmost non-9: node(2)
Increment it by 1: node(3)
Set all nodes after it to 0
```

If ALL digits are 9:
```text
9 → 9 → 9  +1 = 1 → 0 → 0 → 0
Create a new head node(1), set all existing to 0
```

```java
public ListNode plusOne(ListNode head) {
    // Step 1: Add dummy node to handle all-9s case
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode notNine = dummy; // last node that is not 9

    // Step 2: Find the rightmost non-9 node
    ListNode curr = head;
    while (curr != null) {
        if (curr.val != 9) notNine = curr;
        curr = curr.next;
    }

    // Step 3: Increment that node
    notNine.val += 1;

    // Step 4: Set all subsequent nodes to 0
    curr = notNine.next;
    while (curr != null) {
        curr.val = 0;
        curr = curr.next;
    }

    // If dummy's value was changed (from 0 to 1), include it
    return dummy.val == 1 ? dummy : dummy.next;
}
```

**Comparison:**

| Approach | Time | Space | Notes |
|---|---|---|---|
| Reverse + Add | O(n) | O(1) | Two traversals + 2 reverses |
| Rightmost Non-9 | O(n) | O(1) | Cleaner, one traversal, more intuitive |

**Interview preferred:** Rightmost non-9 approach is more clever and shows deeper understanding.

---

# Pattern 74 — Intersection Detection

---

## LC 160 — Intersection of Two Linked Lists

**LeetCode #160 | Easy**

### 1. Problem Understanding

Find the node at which two singly linked lists intersect. **Intersection means the same node object** (same memory address), not just same value.

```text
A: 1 → 2 ─────────┐
                   ↓
                   8 → 9 → null
                   ↑
B:     5 → 6 → 7 ─┘

Intersection node: 8
```

Once two lists share a node, they share all subsequent nodes (they merge into one path).

### 2. Pattern Recognition

> Two lists of possibly different lengths. Need to find common node. Use two pointers with head switching to equalize path lengths.

### 3. Brute Force

Use a hash set: put all nodes of list A in the set, then traverse list B and check if any node is in the set.

```
Time: O(n + m) | Space: O(n) — for the hash set
```

### 4. Optimal Two Pointer (No Extra Space)

**Key Insight:** If pointer `pA` travels path A then path B, and pointer `pB` travels path B then path A, they both travel a total of `len(A) + len(B)` steps. At the intersection, they will be at the same position at the same step.

```text
pA: A1 → A2 → C1 → C2 → B1 → B2 → B3 → C1 → ...
pB: B1 → B2 → B3 → C1 → C2 → A1 → A2 → C1 → ...
                             ↑
                         they meet here (intersection node C1)
```

**Why switching heads makes them meet:**

Let:
- `a` = number of unique nodes in A before intersection
- `b` = number of unique nodes in B before intersection
- `c` = number of common nodes after intersection

`pA` travels: `a + c + b` nodes before meeting pB
`pB` travels: `b + c + a` nodes before meeting pA

Since `a + c + b == b + c + a`, they arrive at the intersection simultaneously.

**If no intersection:** Both travel `a + c + b + c` nodes, both reach `null` simultaneously → return `null`.

### 5. Algorithm

1. Set `pA = headA`, `pB = headB`.
2. While `pA != pB`:
   - If `pA == null`, redirect `pA = headB`, else `pA = pA.next`.
   - If `pB == null`, redirect `pB = headA`, else `pB = pB.next`.
3. Return `pA` (either the intersection node or `null`).

### 6. Java Code

```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    ListNode pA = headA;
    ListNode pB = headB;

    while (pA != pB) {
        // When reaching null, redirect to the other list's head
        pA = (pA == null) ? headB : pA.next;
        pB = (pB == null) ? headA : pB.next;
    }

    return pA; // null if no intersection, otherwise the intersection node
}
```

### 7. Dry Run

```text
A: 1 → 2 → 8 → 9 (len=4)
B: 5 → 6 → 7 → 8 → 9 (len=5, 8 is the intersection)

pA path: 1,2,8,9, null→headB: 5,6,7,8 ← meets pB here
pB path: 5,6,7,8,9, null→headA: 1,2,8 ← meets pA here

Steps: pA does 4+3=7 steps to reach intersection
       pB does 5+2=7 steps to reach intersection ✓
```

### 8. Edge Cases

- No intersection: both reach `null` at the same time → `pA == pB == null` → return `null`
- Intersection at head: both are redirected to the same head immediately

```
Time: O(n + m) | Space: O(1)
```

**Interview Answer:** "Each pointer travels the combined length of both lists. When pA exhausts list A, it switches to headB. When pB exhausts list B, it switches to headA. Both then travel the same total distance and meet at the intersection. If there's no intersection, they both reach null simultaneously."

---

# Pattern 75 — Reordering / Partitioning

---

## LC 24 — Swap Nodes in Pairs

**LeetCode #24 | Medium**

### 1. Problem Understanding

Swap every two adjacent nodes. Do not modify node values.

```text
Input:  1 → 2 → 3 → 4
Output: 2 → 1 → 4 → 3
```

### 2. Pattern Recognition

> Swap pairs using pointer manipulation. Use dummy node to handle swapping the first pair.

### 5. Pointer Diagram

```text
dummy → 1 → 2 → 3 → 4

prev = dummy, first = 1, second = 2

Step 1: prev.next = second (dummy→2)
Step 2: first.next = second.next (1→3)
Step 3: second.next = first (2→1)
Step 4: prev = first (prev = 1)

Next iteration: first = 3, second = 4
... (same swap)

Result: dummy → 2 → 1 → 4 → 3
```

### 7. Java Code

```java
public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode prev = dummy;

    while (prev.next != null && prev.next.next != null) {
        ListNode first = prev.next;
        ListNode second = prev.next.next;

        // Perform the swap
        prev.next = second;         // step 1: prev points to second
        first.next = second.next;   // step 2: first points to what was after second
        second.next = first;        // step 3: second points back to first

        // Move prev to the end of the swapped pair (which is 'first' now)
        prev = first;
    }

    return dummy.next;
}
```

### 8. Dry Run

```text
dummy→1→2→3→4, prev=dummy

Iter 1: first=1, second=2
  dummy→2, 1→3, 2→1
  prev=1
  State: dummy→2→1→3→4

Iter 2: first=3, second=4
  1→4, 3→null, 4→3
  prev=3
  State: dummy→2→1→4→3→null

prev.next = null → stop
Result: 2→1→4→3 ✓
```

### 9. Edge Cases

- Odd number of nodes: last single node stays as-is ✓ (`prev.next.next == null`)
- 0 or 1 node: while condition false, return original

```
Time: O(n) | Space: O(1)
```

---

## LC 61 — Rotate List

**LeetCode #61 | Medium**

### 1. Problem Understanding

Rotate the list to the right by `k` places.

```text
Input:  1 → 2 → 3 → 4 → 5, k = 2
Output: 4 → 5 → 1 → 2 → 3
```

### 2. Pattern Recognition

> Find where to "cut" the list and reconnect the tail to the head. Convert to circular temporarily.

### 3. Optimal Approach — Circular Connection

**Key Insight:** Rotating right by `k` means the last `k` nodes become the new head. Find the new tail (at position `length - k` from start), cut there, and the old tail connects to the old head.

**Why `k = k % length`:** If `k == length`, the list rotates back to itself. If `k == 2 * length`, same thing. So we only need `k % length` actual rotations.

### 6. Algorithm

1. Find the length and tail of the list.
2. `k = k % length`. If `k == 0`, return `head` (no rotation).
3. Connect `tail.next = head` (make circular).
4. New tail is at position `length - k` from start. Move there.
5. New head = `newTail.next`. Set `newTail.next = null`.

### 7. Java Code

```java
public ListNode rotateRight(ListNode head, int k) {
    if (head == null || head.next == null || k == 0) return head;

    // Step 1: Find length and tail
    int length = 1;
    ListNode tail = head;
    while (tail.next != null) {
        tail = tail.next;
        length++;
    }

    // Step 2: Effective rotations
    k = k % length;
    if (k == 0) return head;

    // Step 3: Make circular
    tail.next = head;

    // Step 4: Find new tail (at position length - k from start)
    ListNode newTail = head;
    for (int i = 0; i < length - k - 1; i++) {
        newTail = newTail.next;
    }

    // Step 5: Cut
    ListNode newHead = newTail.next;
    newTail.next = null;

    return newHead;
}
```

### 8. Dry Run

```text
List: 1→2→3→4→5, k=2, length=5

k = 2 % 5 = 2 (effective)

Make circular: 5→1 (tail connects to head)
1→2→3→4→5→1→2→...

New tail at position 5-2-1 = 2 (0-indexed):
Move 2 steps from head: 1→2→3 → newTail = node(3)

newHead = 3.next = 4
3.next = null

Result: 4→5→1→2→3 ✓
```

### 9. Edge Cases

- `k >= length` → `k % length` handles it
- Single node → return head immediately
- `k == 0` after modulo → return head

```
Time: O(n) | Space: O(1)
```

---

## LC 86 — Partition List

**LeetCode #86 | Medium**

### 1. Problem Understanding

Partition a list around value `x`. All nodes with values less than `x` come before nodes with values ≥ `x`. The original relative order within each group must be preserved.

```text
Input:  1 → 4 → 3 → 2 → 5 → 2, x = 3
Output: 1 → 2 → 2 → 4 → 3 → 5
```

### 2. Pattern Recognition

> Two separate chains: `less` (values < x) and `greater` (values ≥ x). Traverse and append each node to the appropriate chain. Combine.

### 5. Pointer Diagram

```text
Input: 1 → 4 → 3 → 2 → 5 → 2, x=3

less:    lessHead(dummy) → 1 → 2 → 2
greater: greaterHead(dummy) → 4 → 3 → 5

After combining: lessHead → 1 → 2 → 2 → greaterHead → 4 → 3 → 5
(lessHead and greaterHead are dummies, so actual: 1→2→2→4→3→5)
```

### 7. Java Code

```java
public ListNode partition(ListNode head, int x) {
    ListNode lessHead = new ListNode(0);    // dummy for less chain
    ListNode greaterHead = new ListNode(0); // dummy for greater/equal chain
    ListNode less = lessHead;
    ListNode greater = greaterHead;

    ListNode curr = head;
    while (curr != null) {
        if (curr.val < x) {
            less.next = curr;
            less = less.next;
        } else {
            greater.next = curr;
            greater = greater.next;
        }
        curr = curr.next;
    }

    // IMPORTANT: disconnect the greater chain's tail to avoid cycles
    greater.next = null;

    // Connect the two chains
    less.next = greaterHead.next;

    return lessHead.next;
}
```

**Why `greater.next = null`?**

The last node in the greater chain still has its original `next` pointer pointing to some other node in the list. Without setting it to `null`, you'd create a cycle or include incorrect nodes.

### 8. Dry Run

```text
List: 1→4→3→2→5→2, x=3

curr=1: 1<3 → less: d→1, less=1
curr=4: 4≥3 → greater: d→4, greater=4
curr=3: 3≥3 → greater: d→4→3, greater=3
curr=2: 2<3 → less: d→1→2, less=2
curr=5: 5≥3 → greater: d→4→3→5, greater=5
curr=2: 2<3 → less: d→1→2→2, less=2(second)
curr=null → stop

greater.next = null → 5.next = null (safe)
less.next = greaterHead.next = 4
Result: 1→2→2→4→3→5 ✓
```

### 9. Edge Cases

- All nodes less than x → greater chain is empty, `less.next = null` ✓
- All nodes ≥ x → less chain is empty, returns greaterHead.next ✓

```
Time: O(n) | Space: O(1) — no new nodes, only reconnecting
```

---

## LC 143 — Reorder List

**LeetCode #143 | Medium**

### 1. Problem Understanding

Given list `L0 → L1 → L2 → ... → Ln`, reorder it to `L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → ...`. Do it in-place.

```text
Input:  1 → 2 → 3 → 4 → 5
Output: 1 → 5 → 2 → 4 → 3
```

### 2. Pattern Recognition

> Three-step combined pattern: Find Middle + Reverse second half + Merge alternately.

### 4. The Three-Step Approach

```text
Step 1: Find middle
1 → 2 → 3 → 4 → 5
          ↑
         mid = 3

Step 2: Reverse second half (from mid)
Second half: 3 → 4 → 5
Reversed:    5 → 4 → 3

Step 3: Merge alternately
First:   1 → 2 → 3
Second:  5 → 4 → 3

Merge: 1 → 5 → 2 → 4 → 3
```

### 5. Pointer Diagram (Merging Step)

```text
p1 = 1 → 2 → 3
p2 = 5 → 4 → 3

Iter 1:
  p1Next = 2
  p2Next = 4
  1.next = 5   (attach p2 after p1)
  5.next = 2   (attach p1Next after p2)
  p1 = 2, p2 = 4

Iter 2:
  p1Next = 3
  p2Next = 3
  2.next = 4
  4.next = 3
  p1 = 3, p2 = 3

p2 == null or p1 == p2 → stop
```

### 7. Java Code

```java
public void reorderList(ListNode head) {
    if (head == null || head.next == null) return;

    // Step 1: Find middle (slow ends at first middle for even length)
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    // Step 2: Reverse second half
    ListNode secondHalf = reverse(slow.next);
    slow.next = null; // cut the first half

    // Step 3: Merge alternately
    ListNode p1 = head, p2 = secondHalf;
    while (p2 != null) {
        ListNode p1Next = p1.next;
        ListNode p2Next = p2.next;
        p1.next = p2;
        p2.next = p1Next;
        p1 = p1Next;
        p2 = p2Next;
    }
}

private ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### 8. Dry Run

```text
1→2→3→4→5

Find mid: slow=3 (fast stops when fast.next=5 and fast.next.next=null... wait:
  fast: 1→3→5, fast.next=null at 5 → stop when fast.next.next==null → stop when fast=3
  slow: 1→2→3
  slow=3, cut: first half 1→2→3, second half starting at 4

Reverse 4→5: 5→4
secondHalf = 5

Merge:
p1=1, p2=5:
  p1Next=2, p2Next=4
  1.next=5, 5.next=2
  p1=2, p2=4

p1=2, p2=4:
  p1Next=3, p2Next=null
  2.next=4, 4.next=3
  p1=3, p2=null → stop

Result: 1→5→2→4→3 ✓
```

### 9. Edge Cases

- Even length (4 nodes): middle is second node of first half, second half has 2 nodes
- Single or two nodes: return immediately

```
Time: O(n) — three linear passes
Space: O(1)
```

**Interview Answer:** "I split the problem into three steps: find the middle using fast-slow pointers, reverse the second half in-place, then merge the two halves alternately. This combines three classic linked list techniques and runs in O(n) time and O(1) space."

---

## LC 328 — Odd Even Linked List

**LeetCode #328 | Medium**

### 1. Problem Understanding

Group all nodes at **odd positions** (1st, 3rd, 5th...) together, then all nodes at **even positions** (2nd, 4th, 6th...). Position means node index (1-based), NOT node value.

```text
Input:  1 → 2 → 3 → 4 → 5
Output: 1 → 3 → 5 → 2 → 4
```

### 2. Pattern Recognition

> Two chains: odd-positioned nodes and even-positioned nodes. Link alternately by advancing two pointers simultaneously.

### 5. Pointer Diagram

```text
Initial:
odd  → 1 → 3 → 5
even → 2 → 4
evenHead = 2 (saved so we can connect at the end)

odd.next = even.next (3)
even.next = odd.next (4)
...

At the end: odd.next = evenHead
```

### 7. Java Code

```java
public ListNode oddEvenList(ListNode head) {
    if (head == null || head.next == null) return head;

    ListNode odd = head;
    ListNode even = head.next;
    ListNode evenHead = even; // save even head for final connection

    while (even != null && even.next != null) {
        odd.next = even.next;   // odd skips over even to next odd
        odd = odd.next;

        even.next = odd.next;   // even skips over (new) odd to next even
        even = even.next;
    }

    odd.next = evenHead; // connect odd chain to even chain

    return head;
}
```

### 8. Dry Run

```text
1→2→3→4→5
odd=1, even=2, evenHead=2

Iter 1:
  odd.next = even.next = 3   → 1→3→4→5
  odd = 3
  even.next = odd.next = 4   → 2→4→5
  even = 4

Iter 2:
  odd.next = even.next = 5   → 3→5
  odd = 5
  even.next = odd.next = null → 4→null
  even = null → stop while

odd.next = evenHead = 2
Result: 1→3→5→2→4 ✓
```

### 9. Edge Cases

- Even number of nodes: last even node gets `null` from the `odd.next` at the end ✓
- 1 or 2 nodes: return immediately ✓

```
Time: O(n) | Space: O(1)
```

**Interview Answer:** "I maintain two pointers, odd and even, starting at the first and second nodes. In each iteration, odd skips over even to the next odd-positioned node, and even skips over odd to the next even-positioned node. I save the even head at the start and connect the end of the odd chain to it."

---

# Part 5 — Linked List Algorithm Toolkit

---

| # | Algorithm | Purpose | Core Idea | Time | Space |
|---|---|---|---|---|---|
| 1 | Traverse | Visit all nodes | `while curr != null: curr = curr.next` | O(n) | O(1) |
| 2 | Find Length | Count nodes | Traverse + count | O(n) | O(1) |
| 3 | Search | Find a value | Traverse + compare | O(n) | O(1) |
| 4 | Insert at head | Add to front | `new.next = head; head = new` | O(1) | O(1) |
| 5 | Insert at tail | Add to end | Traverse to last, `last.next = new` | O(n) | O(1) |
| 6 | Insert at position | Add at index | Traverse to pos-1, insert | O(n) | O(1) |
| 7 | Delete head | Remove first | `head = head.next` | O(1) | O(1) |
| 8 | Delete tail | Remove last | Traverse to second-last | O(n) | O(1) |
| 9 | Delete by value | Remove first match | `prev.next = curr.next` | O(n) | O(1) |
| 10 | Reverse entire | Flip all pointers | prev+curr+next | O(n) | O(1) |
| 11 | Reverse sublist | Flip [left,right] | Navigate + prev+curr+next + reconnect | O(n) | O(1) |
| 12 | Reverse in k-groups | Flip in chunks | Repeated sublist reversal | O(n) | O(1) |
| 13 | Find middle | Get center node | Fast+slow pointers | O(n) | O(1) |
| 14 | Detect cycle | Has cycle? | Fast+slow; if they meet → cycle | O(n) | O(1) |
| 15 | Find cycle start | Cycle entry point | Floyd Phase 1+2 | O(n) | O(1) |
| 16 | Nth from end | Get kth last node | Gap of n between two pointers | O(n) | O(1) |
| 17 | Remove nth from end | Delete kth last | Dummy + gap of n+1 | O(n) | O(1) |
| 18 | Merge two sorted | Combine sorted lists | Two pointers + dummy | O(n+m) | O(1) |
| 19 | Merge k sorted | Combine k sorted lists | Min-heap or divide & conquer | O(N log k) | O(k) |
| 20 | Find intersection | Shared node | Two pointers + head switch | O(n+m) | O(1) |
| 21 | Check palindrome | Is symmetric? | Middle + reverse + compare | O(n) | O(1) |
| 22 | Remove duplicates | Keep uniques | One pointer; skip adjacent dups | O(n) | O(1) |
| 23 | Partition list | Group by value | Two chains (less + greater) | O(n) | O(1) |
| 24 | Rotate list | Shift right by k | Find length + circular + cut | O(n) | O(1) |
| 25 | Reorder list | L0,Ln,L1,Ln-1... | Middle + Reverse + Merge | O(n) | O(1) |
| 26 | Split linked list | Divide at middle | Fast+slow, cut at mid | O(n) | O(1) |
| 27 | Clone with random | Deep copy | Hash map: old→new node | O(n) | O(n) |
| 28 | Add two numbers | List addition | Digit-by-digit with carry | O(n+m) | O(n+m) |
| 29 | Sort linked list | Sort in-place | Merge sort (find mid + merge) | O(n log n) | O(log n) |
| 30 | Reverse doubly | Flip double links | Swap prev/next for every node | O(n) | O(1) |
| 31 | Circular insert/delete | Circular operations | Always update tail.next to head | O(n) | O(1) |

---

## Key Algorithms Explained

### 15. Find Cycle Start (Floyd's Phase 2)

```java
// Phase 1: Detect meeting point
ListNode slow = head, fast = head;
while (true) {
    slow = slow.next;
    fast = fast.next.next;
    if (slow == fast) break;
}

// Phase 2: Find cycle entry
slow = head;
while (slow != fast) {
    slow = slow.next;
    fast = fast.next;
}
return slow; // cycle start
```

**Why Phase 2 works:**
Let `F` = steps from head to cycle start, `h` = steps from cycle start to meeting point, `C` = cycle length.

When they meet: `F + h + C = 2(F + h)` → `C - h = F`.

So if slow restarts from head (traveling F steps) and fast stays at meeting point (traveling C - h = F steps), they meet at the cycle start.

---

### 19. Merge K Sorted Lists

```java
import java.util.PriorityQueue;

public ListNode mergeKLists(ListNode[] lists) {
    PriorityQueue<ListNode> pq = new PriorityQueue<>((a, b) -> a.val - b.val);

    // Add all heads to the min-heap
    for (ListNode head : lists) {
        if (head != null) pq.offer(head);
    }

    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;

    while (!pq.isEmpty()) {
        ListNode node = pq.poll(); // get smallest
        curr.next = node;
        curr = curr.next;
        if (node.next != null) pq.offer(node.next); // add next from same list
    }

    return dummy.next;
}
```

```
Time: O(N log k) where N = total nodes, k = number of lists
Space: O(k) for the heap
```

---

### 27. Clone with Random Pointer (LC 138)

```java
import java.util.HashMap;

public Node copyRandomList(Node head) {
    if (head == null) return null;
    HashMap<Node, Node> map = new HashMap<>();

    // First pass: create all new nodes
    Node curr = head;
    while (curr != null) {
        map.put(curr, new Node(curr.val));
        curr = curr.next;
    }

    // Second pass: assign next and random pointers
    curr = head;
    while (curr != null) {
        map.get(curr).next = map.get(curr.next);
        map.get(curr).random = map.get(curr.random);
        curr = curr.next;
    }

    return map.get(head);
}
```

```
Time: O(n) | Space: O(n)
```

---

### 29. Sort Linked List (Merge Sort)

**Why Merge Sort for Linked Lists?**
- Arrays: QuickSort is preferred (O(1) space, cache-friendly random access).
- Linked Lists: Merge Sort is preferred because:
  - Finding the middle is O(n) (fast+slow), splitting is easy.
  - Merging two sorted lists is O(n) and requires no extra space.
  - QuickSort needs random access for partitioning, which is O(n) per step in a linked list.

```java
public ListNode sortList(ListNode head) {
    if (head == null || head.next == null) return head;

    // Find middle and split
    ListNode mid = findMid(head);
    ListNode rightHalf = mid.next;
    mid.next = null;

    // Recursively sort both halves
    ListNode left = sortList(head);
    ListNode right = sortList(rightHalf);

    // Merge
    return mergeTwoLists(left, right);
}

private ListNode findMid(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast.next != null && fast.next.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

```
Time: O(n log n) | Space: O(log n) for recursion stack
```

---

# Part 6 — Important LeetCode-Style Questions

---

## Easy Problems

### 206. Reverse Linked List
**Pattern clue:** "Reverse a list" → `prev + curr + next`. Fully covered in Pattern 71.

### 21. Merge Two Sorted Lists
**Pattern clue:** "Merge two sorted" → `dummy + two-pointer comparison`. Covered in Pattern 72.

### 876. Middle of the Linked List
**Pattern clue:** "Find middle" → `fast + slow`.
```java
ListNode slow = head, fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
return slow;
```

### 141. Linked List Cycle
**Pattern clue:** "Detect cycle" → `fast + slow`; if they meet, cycle exists.

### 83. Remove Duplicates from Sorted List
**Pattern clue:** "Remove duplicates from sorted" → single pointer; skip adjacent duplicates. Covered in Pattern 71.

### 234. Palindrome Linked List
**Pattern clue:** "Check palindrome" → `Middle + Reverse + Compare`. Covered in Pattern 71.

---

## Medium Problems

### 19. Remove Nth Node From End of List
**Pattern clue:** "Nth from end" → `dummy + gap technique`.
```java
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode fast = dummy, slow = dummy;
for (int i = 0; i <= n; i++) fast = fast.next;
while (fast != null) { slow = slow.next; fast = fast.next; }
slow.next = slow.next.next;
return dummy.next;
```

### 2. Add Two Numbers
**Pattern clue:** "Add digit-by-digit" → `carry + dummy`. Covered in Pattern 73.

### 160. Intersection of Two Linked Lists
**Pattern clue:** "Find shared node" → `two pointers + head switch`. Covered in Pattern 74.

### 24. Swap Nodes in Pairs
**Pattern clue:** "Swap adjacent pairs" → `dummy + prev/first/second`. Covered in Pattern 75.

### 61. Rotate List
**Pattern clue:** "Rotate by k" → `length + circular + cut`. Covered in Pattern 75.

### 86. Partition List
**Pattern clue:** "Group by comparison" → `two chains + combine`. Covered in Pattern 75.

### 143. Reorder List
**Pattern clue:** "Reorder as L0,Ln,L1,Ln-1..." → `Middle + Reverse + Merge`. Covered in Pattern 75.

### 328. Odd Even Linked List
**Pattern clue:** "Group by position parity" → `two chains (odd/even positions)`. Covered in Pattern 75.

### 92. Reverse Linked List II
**Pattern clue:** "Reverse sublist [left,right]" → navigate + reverse + reconnect. Covered in Pattern 71.

### 25. Reverse Nodes in k-Group
**Pattern clue:** "Reverse every k nodes" → repeated sublist reversal. Covered in Pattern 71.

### 142. Linked List Cycle II
**Pattern clue:** "Find where cycle starts" → Floyd's Phase 1 + Phase 2.

```java
public ListNode detectCycle(ListNode head) {
    ListNode slow = head, fast = head;
    // Phase 1: Detect meeting point
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            // Phase 2: Find cycle start
            slow = head;
            while (slow != fast) {
                slow = slow.next;
                fast = fast.next;
            }
            return slow;
        }
    }
    return null; // no cycle
}
```

### 148. Sort List
**Pattern clue:** "Sort linked list efficiently" → Merge Sort on linked list.

### 138. Copy List with Random Pointer
**Pattern clue:** "Deep copy with random pointers" → HashMap old→new.

### 369. Plus One Linked List
**Pattern clue:** "Add 1, forward order" → find rightmost non-9. Covered in Pattern 73.

---

# Part 7 — Pattern Recognition Cheat Sheet

| Problem Clue | Technique | Why |
|---|---|---|
| Find middle | Fast + Slow | fast at 2x speed, slow ends at n/2 |
| Detect cycle | Fast + Slow | if cycle, they inevitably meet |
| Find cycle start | Fast + Slow (Phase 2) | mathematical: F = C - h |
| Nth from end | Two pointers with gap n | gap stays constant as both advance |
| Remove nth from end | Dummy + gap n+1 | slow stops *before* the target |
| Reverse entire list | prev + curr + next | redirect each `next` pointer backward |
| Reverse a range | Navigate + prev+curr+next + reconnect | reverse sublist, then stitch |
| Reverse in k-groups | getKth helper + repeated reversal | check k nodes exist, then reverse chunk |
| Merge sorted lists | Two pointers + Dummy | always pick the smaller head |
| Remove head safely | Dummy node | dummy.next = head; return dummy.next |
| Palindrome | Middle + Reverse + Compare | check second half reversed = first half |
| Reorder list | Middle + Reverse + Merge | L0→Ln→L1→Ln-1 pattern |
| Rotate list | Length + Circular connection | tail.next = head, find new tail |
| Partition | Two separate chains | less + greater, then combine |
| Intersection | Two pointers + head switch | both travel a+b+c distance |
| Swap pairs | Dummy + prev/first/second | careful 3-step pointer update |
| Odd/Even positions | Two-chain pointers | odd chain + even chain, connect at end |
| Remove duplicates | Single pointer; skip dups | sorted → dups adjacent |
| Remove ALL duplicates | Dummy + prev + skip group | skip all nodes with a duplicate value |
| Add two numbers | Dummy + carry | digit-by-digit addition from head |
| Sort list | Merge Sort | find mid + sort halves + merge |
| Clone with random | HashMap | map old node → new node |

---

**Explanation of Key Rows:**

- **Find cycle start:** The math shows: distance(head → cycle entry) = distance(meeting point → cycle entry going forward). So reset slow to head and both advance 1 step until they meet at the entry.

- **Intersection head switch:** pA travels `lenA + lenB` total, pB travels `lenB + lenA` total. They arrive at the intersection simultaneously because the total distance is the same.

- **Remove nth from end:** Moving fast by `n+1` (not n) so that when fast hits `null`, slow is the node just *before* the nth-from-end — this allows `slow.next = slow.next.next`.

- **Two separate chains (partition):** Instead of moving nodes around (which loses references), create two independent chains and link them. Set the tail of the greater chain to `null` explicitly.

---

# Part 8 — Edge Cases

For every linked list problem, check these cases:

| Edge Case | How It Affects | What to Do |
|---|---|---|
| `head == null` | Empty list | Return `null` or 0 immediately |
| `head.next == null` | Single node | Most operations return `head` or trivially true/false |
| Two nodes | Minimum non-trivial case | Test reversal, palindrome, cycle detection |
| `k == 0` | No operation | Return `head` directly |
| `k > length` | Over-rotation | `k = k % length` |
| `k == length` | Full rotation = no change | `k % length = 0`, return `head` |
| Duplicate values | Incorrect equality comparisons | Make sure you compare node references for intersection/cycle |
| All same values | Multiple duplicates in a row | Keep `curr` fixed while removing duplicates, don't advance prematurely |
| Cycle exists | Infinite traversal | Always check for null BEFORE advancing fast pointer |
| Cycle doesn't exist | fast eventually reaches null | `while (fast != null && fast.next != null)` |
| Intersection doesn't exist | Both reach null | Two-pointer returns null ✓ |
| Intersection at head | First node is shared | Still detected correctly; head switch still works |
| Intersection at tail | Only last node shared | Still detected ✓ |
| Delete head node | Head pointer must update | Use dummy node: `dummy.next = head` |
| `left == right` in reverse range | No reversal needed | Single iteration of the loop does nothing harmful |
| Even-length list for palindrome | Two midpoints | Use `fast.next != null && fast.next.next != null` to get first middle |

---

# Part 9 — Common Linked List Mistakes

---

### Mistake 1 — Losing `next` Before Changing a Link

```java
// WRONG: next is lost forever
curr.next = prev;
curr = curr.next; // this is now prev, not the original next!

// CORRECT: Save next first
ListNode next = curr.next;
curr.next = prev;
curr = next;
```

---

### Mistake 2 — Forgetting to Update `head`

```java
// WRONG: head is not returned/updated
public void reverseList(ListNode head) {
    // ... reversal code ...
    // head still points to original head (now the tail)
}

// CORRECT: Return the new head
public ListNode reverseList(ListNode head) {
    // ... reversal code ...
    return prev; // the new head
}
```

---

### Mistake 3 — Forgetting to Update `tail`

When inserting at the end, if you track a `tail` pointer, update it:
```java
tail.next = newNode;
tail = newNode; // ← must update tail!
```

---

### Mistake 4 — Creating Accidental Cycles

```java
// Partition problem — WRONG: forgetting greater.next = null
// If the last node of the greater chain still points somewhere inside the list,
// you get a cycle. Always:
greater.next = null; // explicitly terminate the chain
```

---

### Mistake 5 — Not Handling `null` Before Accessing `.next`

```java
// WRONG: fast.next.next crashes if fast.next is null
while (fast.next.next != null) { ... }

// CORRECT: check fast.next first
while (fast != null && fast.next != null) { ... }
```

---

### Mistake 6 — Using Value Equality Instead of Reference Equality

```java
// Intersection problem — WRONG
if (pA.val == pB.val) return pA; // wrong! same value ≠ same node

// CORRECT
if (pA == pB) return pA; // reference equality (same object in memory)
```

---

### Mistake 7 — Forgetting `k % length`

```java
// WRONG: k=10, length=3 → you rotate 10 times unnecessarily
for (int i = 0; i < k; i++) { ... }

// CORRECT
k = k % length;
if (k == 0) return head;
```

---

### Mistake 8 — Not Using Dummy Node When Needed

```java
// Removing head without dummy — awkward special case
if (curr.val == val) return head.next;
// ... then separate logic for rest

// With dummy — uniform handling
ListNode dummy = new ListNode(0);
dummy.next = head;
ListNode curr = dummy;
// ... same logic handles head and non-head cases
return dummy.next;
```

---

### Mistake 9 — Moving Fast Pointer Incorrectly

```java
// For nth from end: fast must move n+1 times (not n times)
// so that slow stops BEFORE the node to delete

// WRONG: fast moves n times → slow stops AT the target, can't delete
for (int i = 0; i < n; i++) fast = fast.next;

// CORRECT: fast moves n+1 times → slow stops BEFORE the target
for (int i = 0; i <= n; i++) fast = fast.next;
```

---

### Mistake 10 — Forgetting to Disconnect the Old Link

When reversing a sublist and reconnecting:

```java
// The original sublist head (now tail) still points to the old next
// You must explicitly set it to the node after the sublist
currLeft.next = nextGroupStart; // ← this line is often forgotten
```

---

### Mistake 11 — Infinite Loop in Circular Lists

```java
// WRONG: will never stop
ListNode curr = head;
while (curr != null) { curr = curr.next; } // null never reached

// CORRECT: stop when we return to head
ListNode curr = head;
do {
    // process
    curr = curr.next;
} while (curr != head);
```

---

### Mistake 12 — Returning `head` Instead of `dummy.next`

```java
// WRONG: head might have been deleted or changed
return head;

// CORRECT: dummy.next always points to the real (possibly new) head
return dummy.next;
```

---

# Part 10 — Interview Theory Questions

---

**Q1. What is a linked list?**
A linked list is a linear data structure where nodes are connected via `next` pointers. Each node stores data and a reference to the next node. Unlike arrays, nodes are not contiguous in memory.

**Q2. Array vs Linked List?**
Arrays offer O(1) random access but O(n) insertion/deletion. Linked lists offer O(1) insertion/deletion at known positions but O(n) access. Arrays are cache-friendly; linked lists are not.

**Q3. Why is random access O(n)?**
There are no indices. You must start at the head and traverse node by node until you reach the desired position. You cannot jump directly.

**Q4. Why is insertion at head O(1)?**
You only update two pointers: the new node's `next` points to the old head, and `head` becomes the new node. No shifting required.

**Q5. Singly vs Doubly Linked List?**
Singly has only `next` (one direction). Doubly has both `prev` and `next` (bidirectional). Doubly uses more memory but allows O(1) deletion given a node reference and bidirectional traversal.

**Q6. What is a circular linked list?**
A linked list where the last node's `next` points back to the head instead of `null`. Used for round-robin scheduling and circular buffers.

**Q7. Why use a dummy node?**
A dummy node before the head provides a "node before the head" so that head manipulation (insert/delete at head) uses the same code as any other position. At the end, return `dummy.next`.

**Q8. Why do we use fast and slow pointers?**
To find midpoints and detect cycles in O(n) time and O(1) space, without needing to know the list length upfront.

**Q9. Why does Floyd's cycle detection work?**
If a cycle exists, fast gains 1 node per iteration over slow (moves 2 vs 1). Within at most L iterations inside a cycle of length L, fast laps slow and they meet. If no cycle, fast reaches null.

**Q10. Why does fast move twice as fast?**
Speed 2 gives a relative speed of exactly 1 (2 - 1 = 1). This guarantees fast will catch slow in exactly L iterations inside any cycle of length L — no skipping possible. Higher speeds can cause fast to skip over slow in some cycle lengths.

**Q11. Why not use fast = 4 × slow?**
Relative speed = 3. For a cycle of length 3, fast might perpetually skip slow (e.g., if the meeting point never aligns). Speed 2 is the minimum that guarantees meeting in all cases.

**Q12. How do you find the middle?**
Fast and slow start at head. Slow moves 1 step, fast moves 2. When fast reaches null (or fast.next is null), slow is at the middle.

**Q13. How do you find the nth node from the end?**
Move a `fast` pointer n steps ahead of `slow`. Then advance both until fast reaches the last node. Slow is now at the nth node from the end.

**Q14. How do you reverse a linked list?**
Use three pointers: `prev = null`, `curr = head`. In each iteration: save `next = curr.next`, set `curr.next = prev`, advance `prev = curr`, advance `curr = next`. Return `prev`.

**Q15. How do you reverse without extra space?**
The iterative approach with three pointers uses O(1) space. The three pointers are `prev`, `curr`, and `next` — all local variables, not arrays.

**Q16. How do you detect a cycle?**
Fast and slow pointers. If they ever point to the same node, a cycle exists.

**Q17. How do you find the cycle starting point?**
After Phase 1 (detection meeting point), reset slow to head. Advance both slow and fast one step at a time. They meet at the cycle entry.

**Q18. How do you find intersection?**
Two pointers pA and pB. When pA exhausts listA, redirect to headB. When pB exhausts listB, redirect to headA. They meet at the intersection node (or both reach null if no intersection).

**Q19. How do you check palindrome?**
Find middle → reverse second half → compare first half with reversed second half → O(n) time, O(1) space.

**Q20. How do you merge sorted lists?**
Use a dummy node. Two pointers compare heads. Attach the smaller head to the merged list and advance that pointer. When one list is exhausted, attach the remainder.

**Q21. How do you rotate a linked list?**
Find length and tail. Compute `k = k % length`. Connect tail to head (circular). Find the new tail at position `length - k`. The new head is `newTail.next`. Set `newTail.next = null`.

**Q22. How do you remove duplicates?**
Sorted: single pointer; if `curr.val == curr.next.val`, skip `curr.next`.  
Unsorted: hash set of seen values.

**Q23. How do you reorder a list?**
Three steps: find middle (fast+slow), reverse second half (prev+curr+next), merge both halves alternately (p1, p2 pointers).

**Q24. How do you clone a list with random pointers?**
Two-pass with a HashMap: first create all new nodes (map old → new), then assign `next` and `random` using the map.

**Q25. Why is Merge Sort preferred for linked lists?**
Merge Sort's splitting (finding middle) is O(n) with fast+slow pointers, and merging two sorted lists is O(n) with O(1) extra space. There's no random access penalty. QuickSort needs random access for efficient partitioning, which is O(n) per step in linked lists — making it inefficient.

**Q26. Why is Quick Sort less convenient for linked lists?**
QuickSort relies on random access to partition around a pivot efficiently. In a linked list, accessing an arbitrary node is O(n), making the partitioning step slow. Additionally, swapping elements requires careful pointer manipulation.

**Q27. Can binary search be applied to a linked list?**
Technically yes (find middle, compare, recurse on left or right half), but each binary search step requires O(n) to find the middle, making the total O(n log n) — worse than a simple O(n) linear scan. Not practical.

**Q28. Why is doubly linked list deletion easier?**
Given a node reference, you can access `node.prev` directly to update `node.prev.next = node.next` and `node.next.prev = node.prev`. This is true O(1) deletion. In a singly linked list, you need to traverse from head to find the previous node — O(n).

**Q29. What causes an infinite loop in a circular linked list?**
Using `while (curr != null)` as the loop condition. In a circular list, no node's `next` is null. The loop never terminates. Use `do { ... } while (curr != head)` instead.

**Q30. Difference between node value equality and node reference equality?**
Value equality: two nodes have the same `val`. Reference equality: two variables point to the exact same node object in memory. Intersection detection requires reference equality (`pA == pB`), not value equality (`pA.val == pB.val`), because two different nodes could coincidentally hold the same value.

---

# Part 11 — Java Linked List Template

---

## Standard Node Structure

```java
class ListNode {
    int val;
    ListNode next;

    ListNode() {}                          // empty constructor

    ListNode(int val) {                    // constructor with value
        this.val = val;
    }

    ListNode(int val, ListNode next) {     // constructor with value and next
        this.val = val;
        this.next = next;
    }
}
```

**Which constructor does LeetCode use?** All three are defined. Most problems use `ListNode(int val)`.

---

## Create a List

```java
// Build: 1 → 2 → 3 → 4 → 5
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
head.next.next.next = new ListNode(4);
head.next.next.next.next = new ListNode(5);

// OR using helper method:
public static ListNode buildList(int[] arr) {
    if (arr.length == 0) return null;
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    for (int val : arr) {
        curr.next = new ListNode(val);
        curr = curr.next;
    }
    return dummy.next;
}
```

---

## Print a List

```java
public static void printList(ListNode head) {
    ListNode curr = head;
    StringBuilder sb = new StringBuilder();
    while (curr != null) {
        sb.append(curr.val);
        if (curr.next != null) sb.append(" → ");
        curr = curr.next;
    }
    sb.append(" → null");
    System.out.println(sb.toString());
}
```

---

## Insert Nodes

```java
// At head
public static ListNode insertAtHead(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    newNode.next = head;
    return newNode;
}

// At tail
public static ListNode insertAtTail(ListNode head, int val) {
    ListNode newNode = new ListNode(val);
    if (head == null) return newNode;
    ListNode curr = head;
    while (curr.next != null) curr = curr.next;
    curr.next = newNode;
    return head;
}
```

---

## Delete Nodes

```java
// Delete head
public static ListNode deleteHead(ListNode head) {
    return (head == null) ? null : head.next;
}

// Delete tail
public static ListNode deleteTail(ListNode head) {
    if (head == null || head.next == null) return null;
    ListNode curr = head;
    while (curr.next.next != null) curr = curr.next;
    curr.next = null;
    return head;
}
```

---

## Find Length

```java
public static int length(ListNode head) {
    int count = 0;
    while (head != null) { count++; head = head.next; }
    return count;
}
```

---

## Reverse

```java
public static ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

---

## Complete Practice Class

```java
public class LinkedListPractice {
    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    // --- Core Operations ---
    static ListNode buildList(int[] arr) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        for (int v : arr) { curr.next = new ListNode(v); curr = curr.next; }
        return dummy.next;
    }

    static void print(ListNode head) {
        while (head != null) {
            System.out.print(head.val + (head.next != null ? "→" : "→null"));
            head = head.next;
        }
        System.out.println();
    }

    static ListNode reverse(ListNode head) {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }

    static ListNode findMid(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next; fast = fast.next.next;
        }
        return slow;
    }

    static boolean hasCycle(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next; fast = fast.next.next;
            if (slow == fast) return true;
        }
        return false;
    }

    static ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0), curr = dummy;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) { curr.next = l1; l1 = l1.next; }
            else { curr.next = l2; l2 = l2.next; }
            curr = curr.next;
        }
        curr.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }

    public static void main(String[] args) {
        ListNode list = buildList(new int[]{1, 2, 3, 4, 5});
        print(list);                        // 1→2→3→4→5→null
        print(reverse(list));               // 5→4→3→2→1→null

        ListNode l1 = buildList(new int[]{1, 3, 5});
        ListNode l2 = buildList(new int[]{2, 4, 6});
        print(mergeTwoLists(l1, l2));       // 1→2→3→4→5→6→null
    }
}
```

---

# Final Revision — Linked List 1-Page Revision

---

## Core Techniques

```text
1. prev + curr + next     → Reversal (in-place pointer redirection)
2. fast + slow            → Middle, Cycle Detection, Cycle Start
3. Two-pointer gap        → Nth from end (fast ahead by n)
4. Dummy node             → Simplifies head operations, removal, merging
5. Two-list merge         → Merge sorted lists, partition
6. Reverse + Merge        → Palindrome check, Reorder List
7. Circular connection    → Rotate list (make circular then cut)
```

---

## Most Important Algorithms

| Algorithm | Steps |
|---|---|
| Reverse | prev=null; save next; curr.next=prev; advance both |
| Middle | fast+slow; stop when fast.next==null or fast.next.next==null |
| Cycle detect | fast+slow; if slow==fast → cycle |
| Cycle start | Phase1: detect; Phase2: slow=head, both 1 step until meet |
| Nth from end | move fast n steps; advance both; slow is target |
| Remove nth | dummy + fast moves n+1; slow stops before target |
| Merge sorted | dummy; compare p1,p2; attach smaller; advance |
| Intersection | pA, pB; redirect to other head at null; meet = intersection |
| Palindrome | findMid → reverse 2nd half → compare halves |
| Reorder | findMid → reverse 2nd half → merge alternately |
| Rotate | findLen+tail → tail.next=head → find newTail → cut |
| Partition | lessHead + greaterHead; assign; greater.next=null; combine |
| Odd-even | odd+even+evenHead; skip over each other; odd.next=evenHead |
| K-group reverse | getKth; if null stop; reverse chunk; reconnect; advance |

---

## Pattern Recognition — Quick Decision

```text
Question Clue                  → Pattern → Algorithm
────────────────────────────────────────────────────────
"Find middle"                  → Fast+Slow
"Detect cycle"                 → Fast+Slow (compare pointers)
"Find cycle start"             → Fast+Slow Phase 1+2
"kth from end"                 → Gap technique (fast ahead k)
"Remove kth from end"          → Dummy + Gap (ahead k+1)
"Reverse list"                 → prev+curr+next
"Reverse [left, right]"        → Navigate + reverse + reconnect
"Reverse every k nodes"        → getKth + repeated reversal
"Merge two sorted"             → Dummy + two pointers
"Delete head safely"           → Dummy node
"Palindrome"                   → Mid + Reverse + Compare
"Reorder L0,Ln,L1..."          → Mid + Reverse + Merge
"Rotate by k"                  → Length + Circular + Cut (k%len)
"Partition by value x"         → Two chains (less, greater)
"Find intersection"            → Two pointers + head switch
"Swap adjacent pairs"          → Dummy + prev/first/second
"Group odd/even positions"     → Two chains (odd, even) + connect
"Clone with random pointer"    → HashMap (old→new)
"Sort linked list"             → Merge Sort (Mid+Sort+Merge)
"Add numbers (reverse order)"  → Carry + dummy
"Add 1 (forward order)"        → Rightmost non-9 trick
```

---

## Key Invariants to Remember

```text
• After fast+slow, slow is at the MIDDLE (or second middle for even length)
• Gap technique: gap = n between fast and slow stays CONSTANT
  → Dummy + gap n+1 → slow stops BEFORE the nth-from-end node
• Floyd Phase 2: reset slow to head, both advance 1 step → meet at cycle entry
• Intersection: pA travels len(A)+len(B), pB travels len(B)+len(A) → equal!
• Dummy.next is ALWAYS the true head, even after head modifications
• greater.next = null to PREVENT accidental cycles in partition
• k = k % length before rotation (full rotations cancel out)
```

---

*End of Linked List — Complete Interview Preparation Guide*
