# 🧩 Delete the Middle Node of a Linked List

### 🔗 Problem Link

https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/

---

# 🧠 Problem Understanding

Given:

* Head of a singly linked list

👉 Goal:

```text id="goal"
Delete the middle node and return updated list
```

---

## 🔍 Example

```text id="ex1"
Input:  1 → 2 → 3 → 4 → 5
Output: 1 → 2 → 4 → 5
```

---

## ❗ Important

👉 Middle node:

```text id="middef"
floor(n / 2) index (0-based)
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Key challenge

👉 We need:

```text id="challenge"
middle node AND node before it
```

---

## 🧠 Why?

👉 To delete node:

```text id="delete"
prev → slow → next  
becomes  
prev → next
```

---

## 🧩 Step 2: Use Two Pointers

---

### 🧠 Idea

```text id="idea"
slow → moves 1 step  
fast → moves 2 steps
```

---

## 🔥 Why it works

```text id="why"
When fast reaches end → slow is at middle
```

---

# ⚡ Approach: Fast & Slow Pointer

---

## 🧩 Initialization

```cpp id="init"
ListNode* slow = head;
ListNode* fast = head;
ListNode* prev = NULL;
```

---

## 🧩 Traversal

```cpp id="loop"
while (fast && fast->next) {
    prev = slow;
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## 🧩 After loop

```text id="after"
slow → middle  
prev → node before middle
```

---

## 🧩 Delete middle

```cpp id="del"
prev->next = slow->next;
```

---

## ❗ Edge case

```text id="edge"
If only one node → return NULL
```

---

# 💻 Full Code

```cpp id="code"
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {
        if (!head || !head->next) return NULL;

        ListNode* slow = head;
        ListNode* fast = head;
        ListNode* prev = NULL;

        while (fast && fast->next) {
            prev = slow;
            slow = slow->next;
            fast = fast->next->next;
        }

        prev->next = slow->next;

        return head;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# 🔥 Step-by-Step Example

```text id="example"
1 → 2 → 3 → 4 → 5
```

---

## Iteration 1

```text id="it1"
prev = 1  
slow = 2  
fast = 3
```

---

## Iteration 2

```text id="it2"
prev = 2  
slow = 3  
fast = 5
```

---

## Stop

```text id="stop"
fast reaches end
```

---

## Final

```text id="final"
prev = 2  
slow = 3 (middle)
```

---

## Delete

```text id="delete2"
1 → 2 → 4 → 5
```

---

# 🧠 Mental Model (REVISION KEY)

> “Fast reaches end → slow reaches middle → prev deletes it”

---

# ⚡ Key Insights

---

## 🔹 Why prev is needed

```text id="prev"
No backward pointer in singly linked list
```

---

## 🔹 Deletion logic

```text id="logic"
Deletion = pointer update (not actual removal)
```

---

## 🔹 Why fast moves 2 steps

```text id="reason"
To reach end faster → identify middle
```

---

# ❗ Common Mistakes

* ❌ Forgetting edge case (single node)
* ❌ Not tracking `prev`
* ❌ Trying to delete `prev` instead of `slow`
* ❌ Incorrect loop condition

---

# 🧠 Patterns Learned

* Fast & Slow Pointer (Tortoise-Hare)
* Linked list traversal
* Pointer manipulation

---

# 🏷 Tags

* Linked List
* Two Pointers

---

# 🔥 Recognition Pattern

Use this when:

* Find middle
* Detect cycle
* Split list
* Find kth from end
