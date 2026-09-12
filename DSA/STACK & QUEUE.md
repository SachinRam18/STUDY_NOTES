# Stack & Queue — Complete DSA & Interview Master Notes

---

## 1. Core Concepts & Visual Mechanics

### Stack (LIFO — Last In, First Out)
A linear data structure where elements are inserted and removed from the **same end** called the **TOP**.

```text
Visual Stack State:
PUSH 10          PUSH 20          PUSH 30          POP () -> 30      PEEK () -> 20
 TOP              TOP              TOP              TOP               TOP
  ↓                ↓                ↓                ↓                 ↓
[ 10 ]           [ 20 ]           [ 30 ]           [ 20 ]            [ 20 ]
                 [ 10 ]           [ 20 ]           [ 10 ]            [ 10 ]
                                  [ 10 ]
```

* **Core Terms**: 
  * `Push(x)`: Adds item `x` to top. 
  * `Pop()`: Removes and returns top item.
  * `Peek()` / `Top()`: Returns top item without removing it.
  * `isEmpty()`: Checks if stack has 0 elements.
  * `Overflow`: Pushing to a full stack (fixed array).
  * `Underflow`: Popping from an empty stack.

---

### Queue (FIFO — First In, First Out)
A linear data structure where elements enter at the **REAR** (Tail) and leave at the **FRONT** (Head).

```text
Visual Queue State:
ENQUEUE(10,20,30)       ENQUEUE(40)               DEQUEUE() -> 10           PEEK() -> 20
 FRONT         REAR      FRONT         REAR        FRONT         REAR       FRONT         REAR
   ↓             ↓         ↓             ↓           ↓             ↓          ↓             ↓
 [ 10 ][ 20 ][ 30 ]     [ 10 ][ 20 ][ 30 ][ 40 ]   [ 20 ][ 30 ][ 40 ]     [ 20 ][ 30 ][ 40 ]
```

* **Core Terms**: `Enqueue(x)` (Insert rear), `Dequeue()` (Delete front), `Peek()` (View front), `isEmpty()`, `isFull()`, `Overflow`, `Underflow`.

---

### Deque (Double-Ended Queue)
Data structure where insertion and deletion are allowed from **BOTH** Front and Rear in $O(1)$ time.

```text
   Front Push/Pop ← [ 10 ][ 20 ][ 30 ][ 40 ] → Rear Push/Pop
```
* **Variants**: 
  * **Input-Restricted**: Insertion only at Rear; deletion from both ends.
  * **Output-Restricted**: Deletion only at Front; insertion at both ends.

---

## 2. Implementations & Internal Mechanics

### Stack Implementation
1. **Array**: Fixed size (or dynamic array with resizing). Uses single integer index `top = -1`.
2. **Linked List**: Dynamic size. Top pointer points to `head`. `Push` = Insert at Head; `Pop` = Delete from Head.

| Feature | Array-Based Stack | Linked List-Based Stack |
| :--- | :--- | :--- |
| **Push / Pop / Peek** | $O(1)$ worst / amortized | $O(1)$ deterministic |
| **Memory Allocation** | Contiguous (Cache-friendly) | Non-contiguous (Node overhead pointers) |
| **Overflow Condition** | When `top == CAPACITY - 1` | Only on System Out-Of-Memory |

---

### Queue Implementation & The Circular Queue
1. **Naive Array Queue**: `front` and `rear` pointers move right. When elements are dequeued, space at index `0...front-1` is lost forever.
   * *Problem*: `[ _ ][ _ ][ 30 ][ 40 ][ 50 ]` (Cannot enqueue 60 even though 2 empty slots exist!).
2. **Circular Queue**: Connects array end back to front using Modulo arithmetic (`% N`).
   * `Enqueue`: `rear = (rear + 1) % capacity`
   * `Dequeue`: `front = (front + 1) % capacity`
   * `isEmpty`: `front == -1` or `size == 0`
   * `isFull`: `(rear + 1) % capacity == front` or `size == capacity`
3. **Linked List Queue**: `front` points to head, `rear` points to tail. `Enqueue` = append at tail ($O(1)$), `Dequeue` = remove head ($O(1)$).

---

## 3. Core Comparisons & Memory Models

### Stack vs Queue

| Property | Stack | Queue |
| :--- | :--- | :--- |
| **Principle** | LIFO (Last In, First Out) | FIFO (First In, First Out) |
| **Access Points** | Single end (TOP) | Two ends (FRONT for delete, REAR for insert) |
| **Primary Use Cases** | Backtracking, Evaluation, Recursion | Order processing, BFS, Buffering |
| **Pointers** | `top` | `front`, `rear` |

---

### Stack Data Structure vs Stack Memory

> 💡 **Interview Focus**: Interviewers love testing if candidates confuse Stack Memory with the Stack Data Structure!

* **Stack Data Structure**: An abstract data format adhering to LIFO rules.
* **Stack Memory**: A contiguous CPU RAM region managed automatically by OS/CPU for function execution call frames, storing local variables, function arguments, and return addresses.
* **Heap Memory**: Dynamically allocated RAM (e.g., `new Object()`) managed by garbage collector/programmer, non-contiguous, surviving beyond function calls.

---

### Queue vs Priority Queue

* **Normal Queue**: FIFO ordering ($O(1)$ push/pop).
* **Priority Queue**: Elements popped based on **highest priority** value, regardless of insertion order. Generally implemented using a **Binary Heap** ($O(\log N)$ push/pop).

---

## 4. Master Patterns & Algorithmic Blueprints

### Pattern 1: Matching / Balanced Symbols
* **Problem Type**: Valid Parentheses `()[]{}`, HTML tag matching, syntax parsing.
* **Why Stack**: Matching items must be resolved in reverse order of opening (innermost pair resolved first).
* **Algorithm**:
  1. Traverse character by character.
  2. If **Opening** bracket `( { [` $\rightarrow$ `push(char)`.
  3. If **Closing** bracket `) } ]` $\rightarrow$ Check if stack is empty (Invalid!). Pop top and check matching pair.
  4. At end: Valid if `stack.isEmpty()`.

---

### Pattern 2: Reverse / Undo / Recursion Simulation
* **Problem Type**: Undo/Redo history, Browser Back/Forward, Flatten Nested Iterator, Call Stack replacement.
* **Why Stack**: The most recent action performed is always the first action undone.

---

### Pattern 3: Monotonic Stack (Crucial Interview Topic!)
A stack whose elements are strictly kept in **Sorted Order** (Monotonically Increasing or Decreasing).

* **Monotonic Increasing**: Stack top is always the *largest* element `[1, 3, 5, 8]`.
* **Monotonic Decreasing**: Stack top is always the *smallest* element `[8, 5, 3, 1]`.

#### Recognition Trigger:
* *"Find Next Greater Element (NGE)"*
* *"Find Next Smaller Element (NSE)"*
* *"Find Previous Greater / Smaller Element"*
* *"Find subarray spans / largest rectangular area bound by lower heights"*

#### The Monotonic Stack Invariant Loop Template:
```text
For each element X in array:
    While stack is NOT empty AND stack.top() violates monotonic rule with X:
        Popped_Element = stack.pop()
        --> Process/Resolve Popped_Element (X is its Next Greater/Smaller!)
    stack.push(X or X's index)
```

> ⚠️ **Amortized Complexity Proof**: Even with a `while` loop nested in a `for` loop, **every element is pushed onto the stack exactly ONCE and popped at most ONCE**. Total operations $= 2N \rightarrow O(N)$ time complexity!

#### Key Problems & Monotonic Rules:
* **Next Greater Element**: Decreasing stack (pop smaller elements when bigger element arrives).
* **Daily Temperatures**: Monotonic decreasing stack of *indices*.
* **Stock Span**: Monotonic decreasing stack of `(price, span)` or indices.
* **Largest Rectangle in Histogram**: Monotonic increasing stack of height indices. When smaller height arrives, compute area for popped bar as height $\times$ width.

---

### Pattern 4: Expression Evaluation (Infix, Postfix, Prefix)
* **Infix**: `A + B` (Human readable, requires operator precedence & parentheses).
* **Postfix (RPN)**: `A B +` (No parentheses needed, processed Left-to-Right via Stack).
* **Postfix Evaluation Algorithm**:
  * Traverse tokens: If operand $\rightarrow$ push. If operator $\rightarrow$ pop two operands ($b = \text{pop}()$, $a = \text{pop}()$), compute $a \text{ op } b$, push result.

---

### Pattern 5: Breadth-First Search (BFS) & Level-Order Traversal
* **Why Queue**: BFS explores nodes level by level (distance $K$ before distance $K+1$). FIFO guarantees shallower nodes are processed before deeper nodes.
* **Algorithm**:
  ```text
  Queue.enqueue(root)
  While Queue is NOT empty:
      level_size = Queue.size()
      For i in 0...level_size:
          curr = Queue.dequeue()
          Process curr
          Enqueue unvisited children of curr
  ```

---

### Pattern 6: Sliding Window Maximum / Minimum (Deque Pattern)
* **Problem**: Find max in every sliding window of size $k$ in $O(N)$ time.
* **Why Deque**: Maintains indices of potential max elements in **Monotonically Decreasing** order.
* **Invariant**: Front of Deque always holds the index of the MAXIMUM element for current window.
* **Steps for each element `i`**:
  1. Remove indices out of window bound from **FRONT**: `deque.front() <= i - k`.
  2. Remove indices from **BACK** whose values are $\le arr[i]$ (they can never be max!).
  3. Push current index `i` to **BACK**.
  4. If `i >= k - 1`, `arr[deque.front()]` is current window max.

---

### Pattern 7: Multi-Source BFS & Topological Sort (Kahn's Algorithm)
* **Multi-Source BFS**: Enqueue ALL starting sources initially (e.g., Rotting Oranges, Distance Map). Process level-by-level simultaneously.
* **Topological Sort (Kahn's)**:
  1. Compute **In-degree** (number of incoming edges) for all nodes.
  2. Enqueue all nodes with `indegree == 0`.
  3. Dequeue node, add to ordering, decrement indegree of neighbors. Enqueue neighbor if indegree becomes 0.

---

## 5. System Design & Custom Data Structure Problems

### 1. Min Stack ($O(1)$ getMin)
* **Goal**: `push`, `pop`, `top`, and `getMin` all in $O(1)$ time.
* **Approach**: Use an Auxiliary Stack storing current minimum alongside main stack, OR store single stack of pairs `(val, current_min)`.

### 2. Queue using Stacks
* **Idea**: Use 2 stacks (`in_stack`, `out_stack`).
* **Push**: `in_stack.push(x)`.
* **Pop/Peek**: If `out_stack` empty, pop ALL items from `in_stack` and push into `out_stack` (reverses order to FIFO!). Pop from `out_stack`. Amortized $O(1)$.

### 3. Stack using Queues
* **Idea**: Single queue with rotation, OR 2 queues. When pushing `x`, enqueue `x`, then dequeue and re-enqueue previous $N-1$ elements so `x` moves to the front. $O(N)$ push, $O(1)$ pop.

---

## 6. Real Computer Science & System Applications

```text
STACK APPLICATIONS                         QUEUE APPLICATIONS
├── Call Stack / Function Frames           ├── CPU / Process Scheduling (Round Robin)
├── Undo/Redo Buffer (Text Editors)        ├── Network Packet Buffers (Routers)
├── Syntax Parsing & Compilers             ├── Async Task Queue (Celery, RabbitMQ)
├── Expression Evaluation                  ├── Disk I/O Request Scheduling
└── Depth-First Search (DFS)               └── Breadth-First Search (BFS) / Web Crawlers
```

---

## 7. Problem Recognition Decision Matrix

| Problem Indicator / Description | Recommended Data Structure / Pattern |
| :--- | :--- |
| **"Matching pairs, nested structures, undo, syntax balance"** | **Stack** |
| **"Find next/previous greater/smaller element in array"** | **Monotonic Stack** |
| **"Process elements strictly in arrival order / First Come First Served"** | **Queue** |
| **"Level by level processing, shortest path in unweighted graph"** | **Queue (BFS)** |
| **"Sliding window minimum or maximum over continuous subarray"** | **Deque** |
| **"Dependency ordering, prerequisite resolution"** | **Queue + Indegree (Kahn's Algo)** |
| **"Multiple propagation origins expanding simultaneously"** | **Multi-Source BFS Queue** |

---

## 8. Complexity Cheat Sheet

| Operation / Algorithm | Time Complexity | Space Complexity | Notes |
| :--- | :---: | :---: | :--- |
| **Stack Push / Pop / Peek** | $O(1)$ | $O(N)$ total | Constant time per operation |
| **Queue Enqueue / Dequeue** | $O(1)$ | $O(N)$ total | Array circular or linked list |
| **Deque All 4 End Operations** | $O(1)$ | $O(N)$ total | Double linked list / ring buffer |
| **Balanced Parentheses** | $O(N)$ | $O(N)$ | Linear traversal & stack space |
| **Monotonic Stack (NGE / Histogram)** | $O(N)$ | $O(N)$ | Amortized $O(1)$ per push/pop |
| **BFS Traversal** | $O(V + E)$ | $O(V)$ | Graph vertices $V$ & edges $E$ |
| **Sliding Window Max (Deque)** | $O(N)$ | $O(K)$ | Window size $K$ |
| **Kahn's Topological Sort** | $O(V + E)$ | $O(V)$ | Queue stores zero-indegree nodes |

---

## 9. Conceptual "WHY" Questions (Interview Deep Dives)

### Q1: Why does BFS guarantee the shortest path in unweighted graphs, but DFS does not?
* **Answer**: BFS expands radially, exploring all nodes at distance $d$ before distance $d+1$. The first time a target node is dequeued, it was reached via the minimum possible edges. DFS plunges deep down a single path, which might reach the target through an arbitrarily long circuitous path.

### Q2: Why is a Monotonic Stack $O(N)$ even with a nested `while` loop?
* **Answer**: The inner `while` loop pops elements. An element can only be popped if it was previously pushed. Since each of the $N$ elements is pushed exactly once, the inner loop body can execute at most $N$ times across the entire algorithm run. Aggregate time $= O(N + N) = O(N)$.

### Q3: Why do we remove elements from the BACK of the Deque in Sliding Window Maximum?
* **Answer**: If current element $arr[i]$ is greater than $arr[\text{back}]$, the smaller element at the back can **never** be the maximum of current or any future window (it is smaller AND older!). Removing it maintains monotonic order.

### Q4: What causes StackOverflowError in recursion?
* **Answer**: Every recursive function call pushes a stack frame (parameters, local variables, return address) onto the OS Call Stack Memory. Unbounded recursion fills the allocated stack frame space, exceeding memory limits and triggering StackOverflow.

---

## 10. Interview Practice Questions (By Level)

### Beginner
1. What is the fundamental difference between Stack (LIFO) and Queue (FIFO)?
2. How do `Push` and `Pop` work on an array stack vs linked list stack?
3. What are Stack Overflow and Underflow conditions?
4. Why does a naive array queue waste memory space?
5. How does a Circular Queue solve space wasted by a simple array queue?
6. What is a Deque, and how does it differ from a standard Queue?
7. What are the time complexities of standard Stack and Queue operations?

### Intermediate
8. How does function call execution rely on Stack Memory?
9. How to design a `MinStack` that retrieves minimum element in $O(1)$ time?
10. How do you implement a Queue using two Stacks? What is the amortized complexity?
11. How does Next Greater Element work using a Monotonic Stack?
12. Why is Queue preferred over Stack for Level-Order Tree Traversal?
13. How does Infix to Postfix conversion use Stack operator precedence?
14. Explain Kahn's Topological Sort algorithm using Queue and Indegrees.

### Advanced / Tricky
15. Prove why Monotonic Stack operations take $O(N)$ amortized time.
16. How does Sliding Window Maximum run in $O(N)$ using Deque instead of $O(N \log K)$ Heap?
17. Explain the invariant maintained by the stack in Largest Rectangle in Histogram.
18. Contrast Stack Memory vs Heap Memory in terms of allocation, speed, and lifetime.
19. How does Multi-source BFS compute shortest distance maps concurrently?

---

## 11. Pattern-Based Practice Problem List

| Category | Problem Name | Pattern | Difficulty | Core Insight |
| :--- | :--- | :--- | :---: | :--- |
| **Stack** | Valid Parentheses | Symbol Matching | Easy | Push open bracket, pop matching close |
| **Stack** | Min Stack | Design / Aux Stack | Medium | Maintain stack of values + current mins |
| **Stack** | Daily Temperatures | Monotonic Stack | Medium | Monotonic decreasing stack of indices |
| **Stack** | Next Greater Element I & II | Monotonic Stack | Medium | Pop smaller elements when greater arrives |
| **Stack** | Stock Span Problem | Monotonic Stack | Medium | Monotonic decreasing stack of prices |
| **Stack** | Largest Rectangle in Histogram | Monotonic Stack | Hard | Height bounded by next and previous smaller |
| **Queue** | Implement Queue using Stacks | Custom Design | Easy | Transfer `in_stack` to `out_stack` on demand |
| **Queue** | Design Circular Queue | Ring Buffer | Medium | Modulo arithmetic `(ptr + 1) % size` |
| **Queue** | Binary Tree Level Order | BFS Queue | Medium | Queue size snapshot per level iteration |
| **Queue** | Rotting Oranges | Multi-Source BFS | Medium | Initial push of all rotten oranges, level order |
| **Queue** | Course Schedule I & II | Kahn's Topo Sort | Medium | Indegree array + zero-indegree queue |
| **Deque** | Sliding Window Maximum | Monotonic Deque | Hard | Monotonic decreasing deque of indices |

---

## 12. Last-Minute 10-Minute Revision Cheat Sheet

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                STACK & QUEUE REVISION SHEET                            │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ • STACK  : LIFO | Operations: push/pop/peek O(1) | Pointers: top                       │
│ • QUEUE  : FIFO | Operations: enqueue/dequeue O(1) | Pointers: front, rear             │
│ • CIRCULAR QUEUE: (rear + 1) % capacity = next slot. Prevents space leakage.           │
│ • DEQUE  : Double Ended Queue | Push & Pop at BOTH Front and Rear in O(1).               │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ KEY PATTERNS & IDENTIFIERS                                                             │
│ 1. Matching / Reverse / Undo        ──> Stack (e.g. Valid Parentheses)                 │
│ 2. Next/Prev Greater/Smaller        ──> Monotonic Stack O(N)                           │
│ 3. Shortest Path / Level Order      ──> Queue (BFS)                                    │
│ 4. Sliding Window Min/Max           ──> Monotonic Deque O(N)                           │
│ 5. Prerequisites / Dependencies     ──> Queue + Indegree (Kahn's Topo Sort)             │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ COMMON PITFALLS                                                                        │
│ ✘ Forgetting to check isEmpty() before peek/pop (Underflow error).                     │
│ ✘ Confusing Stack Memory (OS call stack frames) with Stack Data Structure.             │
│ ✘ Using simple Queue for Sliding Window Max instead of Deque (causes O(N*K) time).     │
│ ✘ Inner loop in Monotonic Stack isn't O(N^2) because each element is popped AT MOST once! │
└────────────────────────────────────────────────────────────────────────────────────────┘
```
