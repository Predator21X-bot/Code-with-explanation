# 🧩 Reverse Linked List

### 🔗 Problem Link

https://leetcode.com/problems/reverse-linked-list/

---

# 🧠 Problem Understanding

Given:

* Head of a singly linked list

👉 Goal:

```text
Reverse the linked list and return new head
```

---

## 🔍 Example

```text
Input:  1 → 2 → 3 → 4 → 5 → NULL
Output: 5 → 4 → 3 → 2 → 1 → NULL
```

---

# 🧠 Core Idea

👉 Linked list is controlled by:

```text
next pointers
```

👉 So reversing means:

```text
reverse direction of each pointer
```

---

# ⚡ Approach 1: Iterative (3-Pointer Technique)

---

## 🧠 Idea

Use 3 pointers:

```text
prev → previous node  
curr → current node  
next → next node (to avoid losing list)
```

---

## 🧩 Algorithm

```text
1. Initialize:
   prev = NULL
   curr = head

2. Loop until curr == NULL:
   - store next node
   - reverse pointer
   - move prev and curr forward

3. Return prev
```

---

## 💻 Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {

        ListNode* prev = NULL;
        ListNode* curr = head;

        while (curr != NULL) {

            ListNode* next = curr->next;  // store next

            curr->next = prev;           // reverse link

            prev = curr;                 // move prev
            curr = next;                 // move curr
        }

        return prev;
    }
};
```

---

# 🔍 Step-by-Step Example

```text
1 → 2 → 3 → 4 → 5
```

---

## Iteration 1

```text
prev = NULL  
curr = 1  
next = 2  

1 → NULL
```

---

## Iteration 2

```text
prev = 1  
curr = 2  

2 → 1 → NULL
```

---

## Iteration 3+

```text
3 → 2 → 1 → NULL  
4 → 3 → 2 → 1 → NULL  
5 → 4 → 3 → 2 → 1 → NULL
```

---

## Final

```text
prev = 5 → 4 → 3 → 2 → 1
```

---

# 🧠 Mental Model (REVISION KEY)

> “Save next → reverse → move forward”

---

# ⚡ Key Insight

```text
Reversing = pointer redirection, NOT node swapping
```

---

# ❗ Important Order

```cpp
next = curr->next;
curr->next = prev;
prev = curr;
curr = next;
```

👉 Order matters ⚠️

---

# ⚡ Approach 2: Recursive

---

## 🧠 Idea

👉 Reverse smaller list first, then fix current node

---

## 💻 Code

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* newHead = reverseList(head->next);

        head->next->next = head;
        head->next = NULL;

        return newHead;
    }
};
```

---

## 🧠 How it works

```text
1 → 2 → 3 → 4
```

* reverse(2 → 3 → 4)
* then fix:
  2 → 1
  3 → 2
  4 → 3

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * Iterative: **O(1)**
  * Recursive: **O(n)** (call stack)

---

# ❗ Common Mistakes

* ❌ Forgetting `next` pointer
* ❌ Returning `head` instead of `prev`
* ❌ Breaking pointer order
* ❌ Missing base case in recursion

---

# 🧠 Patterns Learned

* Pointer manipulation
* In-place reversal
* Recursion + backtracking

---

# 🏷 Tags

* Linked List
* Two Pointers
* Recursion

---

# 🔥 Recognition Pattern

Use this when:

* Reverse entire list
* Reverse part of list
* Reverse in groups
