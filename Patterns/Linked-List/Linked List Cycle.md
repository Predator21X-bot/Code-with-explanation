# 141. Linked List Cycle (Easy)

**Problem Link:** https://leetcode.com/problems/linked-list-cycle/

---

# Pattern Recognition

### Pattern
- **Fast & Slow Pointers (Floyd's Cycle Detection Algorithm)**

### Why?

We need to determine whether a linked list contains a cycle while using **O(1)** extra space.

Using a HashSet would require **O(n)** space, which does not satisfy the follow-up.

---

# Intuition

Imagine two runners on a circular track.

- Slow runner moves **1 step**
- Fast runner moves **2 steps**

Two possibilities:

### No Cycle

The fast pointer reaches the end of the list (`nullptr`).

```text
1 → 2 → 3 → 4 → NULL
```

Answer:

```text
false
```

---

### Cycle Exists

The fast pointer eventually catches the slow pointer.

```text
1 → 2 → 3 → 4
    ↑       ↓
    └───────┘
```

Once both pointers are inside the cycle, the faster pointer must eventually lap the slower pointer.

If they meet, a cycle exists.

---

# Algorithm

1. Initialize two pointers:
   - `slow = head`
   - `fast = head`
2. Traverse while:
   - `fast != nullptr`
   - `fast->next != nullptr`
3. Move:
   - `slow` by one node.
   - `fast` by two nodes.
4. If both pointers become equal:
   - Return `true`.
5. If the loop ends:
   - Return `false`.

---

# Dry Run

Input

```text
3 → 2 → 0 → -4
    ↑         ↓
    └─────────┘
```

### Initial

```text
Slow = 3

Fast = 3
```

---

### Iteration 1

```text
Slow = 2

Fast = 0
```

---

### Iteration 2

```text
Slow = 0

Fast = 2
```

---

### Iteration 3

```text
Slow = -4

Fast = -4
```

Pointers meet.

Return

```text
true
```

---

# Why do we check

```cpp
while(fast && fast->next)
```

?

The fast pointer moves two steps:

```cpp
fast = fast->next->next;
```

Before accessing `fast->next->next`, we must ensure:

- `fast` exists.
- `fast->next` exists.

Otherwise, dereferencing a null pointer would cause a runtime error.

---

# Why do the pointers always meet?

Think of two runners on a circular track.

- Slow moves 1 step.
- Fast moves 2 steps.

Once both are inside the cycle:

- Fast gains **one extra step** on slow every iteration.
- Eventually, the distance between them becomes zero.
- They meet.

If there is no cycle, the fast pointer reaches `nullptr`.

---

# Code

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {

        ListNode* slow = head;
        ListNode* fast = head;

        while (fast && fast->next) {

            slow = slow->next;

            fast = fast->next->next;

            if (slow == fast)
                return true;
        }

        return false;
    }
};
```

---

# Complexity

**Time Complexity**

```text
O(n)
```

Each pointer traverses the list at most once.

---

**Space Complexity**

```text
O(1)
```

Only two pointers are used.

---

# Why not use a HashSet?

Another approach:

- Store every visited node.
- If a node is visited again, a cycle exists.

Complexities:

- Time: **O(n)**
- Space: **O(n)**

The follow-up asks for **O(1)** extra space, making Floyd's Algorithm the optimal solution.

---

# Key Takeaways

- This is the classic **Fast & Slow Pointer** problem.
- Always initialize both pointers at the head.
- Slow moves one step.
- Fast moves two steps.
- If they meet, a cycle exists.
- If `fast` reaches `nullptr`, no cycle exists.
- Compare **node addresses**, not node values.

---

# Similar Problems

- Linked List Cycle II
- Find the Duplicate Number
- Happy Number
- Middle of the Linked List
- Palindrome Linked List
```
