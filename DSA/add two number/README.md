# 2. Add Two Numbers

## Problem

You are given two **non-empty linked lists** representing two non-negative integers. The digits are stored in **reverse order**, and each node contains a single digit.

Add the two numbers and return the sum as a linked list.

### Example

```text
l1 = 2 → 4 → 3
l2 = 5 → 6 → 4

342 + 465 = 807

Output:
7 → 0 → 8
```

---

# Intuition

We add the numbers exactly as we do by hand:

* Add the current digits from both linked lists.
* Include any carry from the previous addition.
* Store the current digit (`sum % 10`).
* Carry the remaining value (`sum // 10`) to the next iteration.

Since the digits are stored in **reverse order**, we can process both linked lists from head to tail without reversing them.

---

# Approach

1. Create a **dummy node** to simplify building the answer list.
2. Maintain a pointer (`node`) to the last node of the result list.
3. Initialize `extra` (carry) to `0`.
4. Continue while:

   * `l1` still has nodes, **or**
   * `l2` still has nodes, **or**
   * there is a remaining carry.
5. For every iteration:

   * Read the current value from each list (or `0` if that list has ended).
   * Compute the total:

     ```python
     total = x + y + carry
     ```
   * Update the carry:

     ```python
     carry = total // 10
     ```
   * Create a node containing:

     ```python
     total % 10
     ```
   * Append it to the result list.
   * Move the pointers of `l1`, `l2`, and the result list forward.
6. Return `dummy.next` since the dummy node itself is only a placeholder.

---

# Dry Run

### Input

```text
l1 = 2 → 4 → 3
l2 = 5 → 6 → 4
```

| Iteration | l1 | l2 | Carry In | Sum | Digit | Carry Out | Result    |
| --------- | -: | -: | -------: | --: | ----: | --------: | --------- |
| 1         |  2 |  5 |        0 |   7 |     7 |         0 | 7         |
| 2         |  4 |  6 |        0 |  10 |     0 |         1 | 7 → 0     |
| 3         |  3 |  4 |        1 |   8 |     8 |         0 | 7 → 0 → 8 |

Final Answer:

```text
7 → 0 → 8
```

---

# Code

```python
class Solution:
    def addTwoNumbers(self, l1, l2):
        dummy = ListNode()
        extra = 0
        node = dummy

        while l1 or l2 or extra:
            val = (l1.val if l1 else 0) + \
                  (l2.val if l2 else 0) + extra

            extra = val // 10

            node.next = ListNode(val % 10)
            node = node.next

            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

        return dummy.next
```

---

# Complexity Analysis

### Time Complexity

* Each node from both linked lists is visited exactly once.

**Time Complexity:** **O(max(m, n))**

where:

* `m` = length of `l1`
* `n` = length of `l2`

---

### Space Complexity

Apart from the output linked list, only a few pointers and variables are used.

**Auxiliary Space:** **O(1)**

> If the output linked list is counted, the total space required is **O(max(m, n))**.

---

# Key Takeaways

* Use a **dummy node** to simplify linked list construction.
* Continue until **both lists are exhausted and no carry remains**.
* Treat missing nodes as **0** once one linked list ends.
* Compute:

  * **Digit** = `sum % 10`
  * **Carry** = `sum // 10`
* Always append new nodes to the **tail** of the result list to preserve the correct order.
