# 206. Reverse Linked List (Easy)

**Problem Link:** https://leetcode.com/problems/reverse-linked-list/

---

# Pattern Recognition

### Pattern
- **Linked List Pointer Manipulation**

### Why?

We need to reverse the direction of every pointer in the linked list.

Unlike arrays, we cannot access previous nodes directly, so we must carefully manipulate pointers while traversing the list.

---

# Intuition

Each node points to the next node.

Example:

```text
1 → 2 → 3 → 4 → NULL
```

After reversal:

```text
4 → 3 → 2 → 1 → NULL
```

The challenge is:

- If we immediately reverse the current pointer, we lose access to the remaining list.
- Therefore, we first save the next node before changing any links.

---

# Three Pointer Technique

We maintain three pointers:

```cpp
ListNode* prev = nullptr;
ListNode* curr = head;
ListNode* next = nullptr;
```

### Purpose

- **prev** → Previous node (already reversed part)
- **curr** → Current node being processed
- **next** → Saves the remaining list

---

# Algorithm

1. Initialize:
   - `prev = nullptr`
   - `curr = head`
2. Traverse the list until `curr == nullptr`.
3. Save the next node.
4. Reverse the current pointer.
5. Move `prev` forward.
6. Move `curr` forward.
7. Return `prev` (new head).

---

# Pointer Updates

For every node:

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

These four lines are repeated for every node.

---

# Dry Run

Input:

```text
1 → 2 → 3 → NULL
```

### Initial

```text
prev = NULL
curr = 1
next = NULL
```

---

### Iteration 1

Save next:

```text
next = 2
```

Reverse:

```text
1 → NULL
```

Move pointers:

```text
prev = 1
curr = 2
```

---

### Iteration 2

Save next:

```text
next = 3
```

Reverse:

```text
2 → 1 → NULL
```

Move pointers:

```text
prev = 2
curr = 3
```

---

### Iteration 3

Save next:

```text
next = NULL
```

Reverse:

```text
3 → 2 → 1 → NULL
```

Move pointers:

```text
prev = 3
curr = NULL
```

Loop ends.

Return:

```text
prev
```

Output:

```text
3 → 2 → 1 → NULL
```

---

# Why do we need the `next` pointer?

Suppose we directly do:

```cpp
curr->next = prev;
```

Before saving:

```text
1 → 2 → 3
```

After reversing:

```text
1 → NULL
```

We have now lost the only reference to node `2`.

So we first save:

```cpp
next = curr->next;
```

Only then do we reverse:

```cpp
curr->next = prev;
```

---

# Edge Cases

- Empty list (`head == nullptr`)
- Single node
- Two nodes
- Multiple nodes

---

# Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* prev = nullptr;
        ListNode* curr = head;
        ListNode* next = nullptr;

        while (curr != nullptr) {

            next = curr->next;

            curr->next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
};
```

---

# Complexity

- **Time:** O(n)
- **Space:** O(1)

---

# Key Takeaways

- Linked Lists are solved using **pointer manipulation**, not indexing.
- Always save the next node before reversing a pointer.
- Three pointers are sufficient:
  - `prev`
  - `curr`
  - `next`
- After the loop, `prev` becomes the new head of the reversed list.
- Each node is visited exactly once.

---

# Similar Problems

- Reverse Linked List II
- Reverse Nodes in k-Group
- Swap Nodes in Pairs
- Palindrome Linked List
- Reorder List
```
