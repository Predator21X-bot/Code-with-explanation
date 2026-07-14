# 21. Merge Two Sorted Lists (Easy)

**Problem Link:** https://leetcode.com/problems/merge-two-sorted-lists/

---

# Pattern Recognition

### Pattern
- **Linked List**
- **Two Pointers**
- **Dummy Node**

### Why?

Both linked lists are already sorted.

We repeatedly compare the current nodes of both lists and attach the smaller one to the answer.

---

# Problem Statement

Given two sorted linked lists,

Merge them into **one sorted linked list**.

The merged list should consist of the original nodes (do not create new nodes except the dummy node).

---

# Intuition

Suppose

```text
List1

1 → 2 → 4

List2

1 → 3 → 4
```

We compare the current nodes.

Whichever node has the smaller value is attached to the merged list.

Move the pointer of the list from which the node was chosen.

Repeat until one list finishes.

Finally attach the remaining list.

---

# Why use a Dummy Node?

Instead of handling the first node separately,

we create a placeholder node.

```text
dummy

↓

0 → NULL
```

`tail` always points to the last node of the merged list.

At the end,

```cpp
return dummy->next;
```

because the dummy node is not part of the answer.

---

# Two Pointer Technique

Maintain three pointers.

```cpp
ListNode* dummy = new ListNode(0);

ListNode* tail = dummy;

ListNode* list1;

ListNode* list2;
```

### Purpose

- `list1` → Traverses first list.
- `list2` → Traverses second list.
- `tail` → Builds the merged list.

---

# Algorithm

1. Create a dummy node.
2. Let `tail` point to the dummy.
3. Compare `list1->val` and `list2->val`.
4. Attach the smaller node.
5. Move the corresponding list pointer.
6. Move `tail`.
7. Repeat until one list becomes `nullptr`.
8. Attach the remaining list.
9. Return `dummy->next`.

---

# Dry Run

Input

```text
List1

1 → 2 → 4

List2

1 → 3 → 4
```

---

### Initial

```text
dummy

↓

0

tail
```

---

### Compare

```text
1 vs 1
```

Pick either.

Suppose we pick List1.

```text
0 → 1
     ↑
    tail

List1

2 → 4

List2

1 → 3 → 4
```

---

### Compare

```text
2 vs 1
```

Pick List2.

```text
0 → 1 → 1
         ↑
        tail
```

---

### Compare

```text
2 vs 3
```

Pick List1.

```text
0 → 1 → 1 → 2
             ↑
            tail
```

Continue until one list ends.

---

### Remaining List

Suppose

```text
List1 = NULL

List2

4 → 5
```

Simply attach

```cpp
tail->next = list2;
```

No more comparisons are needed because the remaining list is already sorted.

---

# Why attach the remaining list directly?

Suppose

```text
Merged

1 → 1 → 2 → 3

List2

4 → 5 → 6
```

The remaining nodes are already sorted.

Instead of comparing each node,

simply connect

```cpp
tail->next = list2;
```

This is an **O(1)** operation.

---

# Why return `dummy->next`?

The dummy node is only a placeholder.

Final list

```text
dummy

↓

0 → 1 → 1 → 2 → 3 → 4
```

The actual merged list starts after the dummy.

Hence,

```cpp
return dummy->next;
```

---

# Code

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {

        ListNode* dummy = new ListNode(0);
        ListNode* tail = dummy;

        while (list1 != nullptr && list2 != nullptr) {

            if (list1->val < list2->val) {

                tail->next = list1;
                list1 = list1->next;

            } else {

                tail->next = list2;
                list2 = list2->next;
            }

            tail = tail->next;
        }

        if (list1)
            tail->next = list1;
        else
            tail->next = list2;

        return dummy->next;
    }
};
```

---

# Complexity

### Time Complexity

```text
O(n + m)
```

where

- `n` = length of List1
- `m` = length of List2

Every node is visited exactly once.

---

### Space Complexity

```text
O(1)
```

Only a few pointers are used.

---

# Key Takeaways

- Since both lists are sorted, always attach the smaller current node.
- Move only the pointer of the list whose node was selected.
- `tail` always points to the last node of the merged list.
- A **dummy node** eliminates special handling for the first insertion.
- Once one list ends, directly attach the remaining list.
- Return `dummy->next`, not `dummy`.

---

# Similar Problems

- Merge K Sorted Lists
- Sort List
- Intersection of Two Linked Lists
- Partition List
- Add Two Numbers
```
