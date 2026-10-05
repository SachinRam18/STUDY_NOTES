## 1. What are Indexes in DBMS? What problem do they solve?

An index is a data structure created on one or more columns of a database table to speed up data retrieval.
Without an index, the database may need to scan the entire table to find matching records.
Example:

```sql
SELECT * FROM Users WHERE email = 'abc@gmail.com';
```

If email has an index, the database can find the record much faster.

Problem with indexes
The main disadvantage is extra storage and slower write operations.
Whenever we INSERT, UPDATE, or DELETE data, the database may also need to update the index.

Interview answer:
"Indexes improve read performance by allowing the database to find records faster, but they consume additional storage and can slow down insert, update, and delete operations."

## 2. What is Thrashing in OS?

Thrashing occurs when the system spends more time swapping pages between RAM and disk than executing processes.
It usually happens when there is insufficient physical memory and processes require more pages than can fit in RAM.

Example

```text
RAM is full
↓
Process needs another page
↓
Page must be loaded from disk
↓
Another page is removed
↓
Process needs the removed page again
↓
Constant page swapping
↓
CPU spends more time handling pages
```

Why is it a problem?
Thrashing causes:

- Very high page faults
- Heavy disk I/O
- Low CPU utilization
- Severe performance degradation

Interview answer:
"Thrashing is a condition where excessive page faults cause the OS to spend most of its time swapping pages between memory and disk instead of executing processes."

## 3. Prime Number — Three Approaches

A prime number is a number greater than 1 that has exactly two factors: 1 and itself.
For example:
7 → 1, 7 → Prime
8 → 1, 2, 4, 8 → Not Prime

### Approach 1 — Check all numbers

Check divisibility from 2 to n-1.

```python
def is_prime(n):
    if n <= 1:
        return False
    for i in range(2, n):
        if n % i == 0:
            return False
    return True
```

Time: O(n)

### Approach 2 — Check up to √n

If n has a factor greater than √n, it must also have a corresponding factor smaller than √n.
So we only check:

```text
i * i <= n
```

```python
def is_prime(n):
    if n <= 1:
        return False
    i = 2
    while i * i <= n:
        if n % i == 0:
            return False
        i += 1
    return True
```

Time: O(√n)
This is the standard approach for checking one number.

### Approach 3 — Sieve of Eratosthenes

If we need to find all prime numbers up to N, use the Sieve of Eratosthenes.
Instead of checking every number individually, we repeatedly mark the multiples of each prime as non-prime.
Time: O(N log log N)
Space: O(N)

Interview shortcut:

- One number → √n approach
- Many primes up to N → Sieve of Eratosthenes

## 4. Linked List Cycle Detection

The standard approach is Floyd's Cycle Detection Algorithm, also called the Tortoise and Hare algorithm.
We use two pointers:
slow → moves 1 step
fast → moves 2 steps

If there is a cycle, eventually slow and fast will meet.

Example

```text
1 → 2 → 3 → 4
↑ ↓
← ← ←
```

slow and fast will eventually point to the same node.

Code

```python
def hasCycle(head):
    slow = head
    fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

Why does this work?
If there is no cycle, fast will eventually reach None.
If there is a cycle, both pointers enter the cycle. Since fast moves faster than slow, it will eventually catch up with slow.
Time: O(n)
Space: O(1)

Interview answer:
"I would use Floyd's cycle detection algorithm with slow and fast pointers. Slow moves one step and fast moves two steps. If they meet, a cycle exists; if fast reaches null, there is no cycle."
