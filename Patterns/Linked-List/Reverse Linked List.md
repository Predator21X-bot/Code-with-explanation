# 206. Reverse Linked List (Easy)

**Problem Link:** https://leetcode.com/problems/reverse-linked-list/

---

# Pattern Recognition

### Pattern
- **Linked List Pointer Manipulation**

### Why?

The problem requires reversing the direction of every pointer in a singly linked list.

Unlike arrays, linked lists cannot be traversed backwards, so we manipulate the `next` pointers while traversing.

---

# Intuition

Original List:

```text
1 → 2 → 3 → 4 → NULL
```

Desired Output:

```text
4 → 3 → 2 → 1 → NULL
```

The challenge is:

If we reverse a pointer immediately, we lose access to the remaining list.

Therefore, we first save the next node before changing any links.

---

# Three Pointer Technique

We maintain three pointers.

```cpp
ListNode* prev = nullptr;
ListNode* curr = head;
ListNode* next = nullptr;
```

### Purpose

- **prev** → Previous node (already reversed part)
- **curr** → Current node being processed
- **next** → Stores the remaining list before reversing

---

# Iterative Algorithm

1. Initialize:
   - `prev = nullptr`
   - `curr = head`
2. While `curr != nullptr`
   - Save next node.
   - Reverse current pointer.
   - Move `prev`.
   - Move `curr`.
3. Return `prev`.

---

# Pointer Updates

For every node:

```cpp
next = curr->next;

curr->next = prev;

prev = curr;

curr = next;
```

These four statements are repeated until the list ends.

---

# Dry Run

Input

```text
1 → 2 → 3 → NULL
```

---

### Initial

```text
prev = NULL

curr = 1

next = NULL
```

---

### Iteration 1

Save next

```text
next = 2
```

Reverse

```text
1 → NULL
```

Move pointers

```text
prev = 1

curr = 2
```

---

### Iteration 2

Save next

```text
next = 3
```

Reverse

```text
2 → 1 → NULL
```

Move pointers

```text
prev = 2

curr = 3
```

---

### Iteration 3

Save next

```text
next = NULL
```

Reverse

```text
3 → 2 → 1 → NULL
```

Move pointers

```text
prev = 3

curr = NULL
```

Loop ends.

Return

```text
prev
```

Output

```text
3 → 2 → 1 → NULL
```

---

# Why do we need the `next` pointer?

Suppose we directly reverse:

```cpp
curr->next = prev;
```

Original

```text
1 → 2 → 3
```

After reversal

```text
1 → NULL
```

The reference to node `2` is permanently lost.

Therefore we first save:

```cpp
next = curr->next;
```

Only then reverse:

```cpp
curr->next = prev;
```

---

# Recursive Approach (Follow-up)

### Intuition

Instead of reversing the list ourselves, let recursion reverse everything **after the current node**.

Example:

```text
1 → 2 → 3 → 4
```

Node `1` asks node `2` to reverse the rest.

Node `2` asks node `3`.

Node `3` asks node `4`.

Node `4` reaches the base case and becomes the new head.

Now while recursion returns, every node simply reverses one pointer.

---

# Base Case

If there is only one node (or the list is empty), it is already reversed.

```cpp
if(head == nullptr || head->next == nullptr)
    return head;
```

---

# Recursive Steps

Reverse the remaining list.

```cpp
ListNode* newHead = reverseList(head->next);
```

Reverse the current pointer.

```cpp
head->next->next = head;
```

Break the old forward link.

```cpp
head->next = nullptr;
```

Return the new head.

```cpp
return newHead;
```

---

# Why `head->next = nullptr`?

Suppose we don't do this.

After executing

```cpp
head->next->next = head;
```

We get

```text
1 ↔ 2
```

A cycle is formed.

Setting

```cpp
head->next = nullptr;
```

breaks the old connection.

---

# Recursive Dry Run

Input

```text
1 → 2 → 3 → NULL
```

Calls

```text
reverse(1)

    reverse(2)

        reverse(3)
```

Node `3` is returned.

Now recursion unwinds.

Node `2`

```text
3 → 2
```

Node `1`

```text
3 → 2 → 1
```

Return

```text
3
```

---

# Iterative Code

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

# Recursive Code

```cpp
class Solution {
public:

    ListNode* reverseList(ListNode* head) {

        if (head == nullptr || head->next == nullptr)
            return head;

        ListNode* newHead = reverseList(head->next);

        head->next->next = head;

        head->next = nullptr;

        return newHead;
    }
};
```

---

# Complexity

## Iterative

- **Time:** O(n)
- **Space:** O(1)

---

## Recursive

- **Time:** O(n)
- **Space:** O(n) (Recursive Call Stack)

---

# Comparison

| Iterative | Recursive |
|-----------|-----------|
| O(n) Time | O(n) Time |
| O(1) Space | O(n) Space |
| Uses three pointers | Uses recursion |
| Preferred in interviews | Common follow-up question |

---

# Key Takeaways

- Linked Lists are solved using **pointer manipulation**.
- Always save the next node before reversing a pointer.
- Three pointers are sufficient:
  - `prev`
  - `curr`
  - `next`
- After traversal, `prev` becomes the new head.
- Recursive solution first reverses the remaining list, then fixes the current node.
- Always set `head->next = nullptr` in recursion to avoid cycles.

---

# Similar Problems

- Reverse Linked List II
- Reverse Nodes in k-Group
- Swap Nodes in Pairs
- Palindrome Linked List
- Reorder List
- Remove Nth Node From End of List
