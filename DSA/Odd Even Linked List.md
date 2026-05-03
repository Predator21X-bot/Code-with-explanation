# 🧩 Odd Even Linked List

### 🔗 Problem Link

https://leetcode.com/problems/odd-even-linked-list/

---

# 🧠 Problem Understanding

Given:

* Head of a singly linked list

👉 Goal:

```text
Group all odd-index nodes first, followed by even-index nodes
```

---

## ❗ Important Clarification

👉 “Odd / Even” refers to:

```text
Position (1-based index) ✅
NOT node values ❌
```

---

## 🔍 Example

```text
Input:  1 → 2 → 3 → 4 → 5
Index:  1   2   3   4   5
```

```text
Output: 1 → 3 → 5 → 2 → 4
```

---

# 🧠 Key Idea

👉 Maintain **two chains inside same list**:

```text
odd  → 1 → 3 → 5 → ...
even → 2 → 4 → 6 → ...
```

👉 Then connect:

```text
odd → even
```

---

# ⚡ Approach: In-place Pointer Manipulation

---

## 🧩 Step 1: Initialize pointers

```cpp
ListNode* odd = head;
ListNode* even = head->next;
ListNode* evenHead = even;
```

---

## 🧩 Step 2: Traverse and rearrange

```cpp
while (even && even->next) {
    odd->next = even->next;
    odd = odd->next;

    even->next = odd->next;
    even = even->next;
}
```

---

## 🧩 Step 3: Connect lists

```cpp
odd->next = evenHead;
```

---

# 💻 Full Code

```cpp
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* odd = head;
        ListNode* even = head->next;
        ListNode* evenHead = even;

        while (even && even->next) {
            odd->next = even->next;
            odd = odd->next;

            even->next = odd->next;
            even = even->next;
        }

        odd->next = evenHead;

        return head;
    }
};
```

---

# 🔍 Step-by-Step Example

```text
Input: 1 → 2 → 3 → 4 → 5
```

---

## Initial State

```text
odd = 1  
even = 2  
evenHead = 2
```

---

## Iteration 1

```text
odd->next = 3  
odd = 3  

even->next = 4  
even = 4
```

---

## Iteration 2

```text
odd->next = 5  
odd = 5  

even->next = NULL  
even = NULL
```

---

## Final Connection

```text
odd → 1 → 3 → 5  
even → 2 → 4  

Result: 1 → 3 → 5 → 2 → 4
```

---

# 🧠 Mental Model (REVISION KEY)

> “Build odd chain and even chain separately, then join them”

---

# ⚡ Key Insights

---

## 🔹 Why `evenHead`?

```text
We need to reconnect even list at the end
```

---

## 🔹 Why pointer jumping works?

```text
odd jumps over even nodes  
even jumps over odd nodes
```

👉 Automatically separates lists

---

## 🔹 In-place modification

```text
No extra space needed
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Confusing value vs index
* ❌ Forgetting `evenHead`
* ❌ Breaking links incorrectly
* ❌ Not checking `even && even->next`

---

# 🧠 Patterns Learned

* Linked list reordering
* Pointer manipulation
* Two-chain technique

---

# 🏷 Tags

* Linked List
* Two Pointers

---

# 🔥 Recognition Pattern

Use this when:

* Reordering list by position
* Partitioning list into groups
* Maintaining relative order
