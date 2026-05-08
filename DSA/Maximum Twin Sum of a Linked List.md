# 🧩 Maximum Twin Sum of a Linked List

### 🔗 Problem Link

https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/

---

# 🧠 Problem Understanding

Given:

* Even-length linked list

👉 Twin nodes are:

```text
first ↔ last
second ↔ second-last
third ↔ third-last
```

---

## 🔍 Example

```text
1 → 2 → 3 → 4
```

Twin pairs:

```text
1 ↔ 4
2 ↔ 3
```

Twin sums:

```text
1 + 4 = 5
2 + 3 = 5
```

👉 Answer:

```text
5
```

---

# ❗ Important Constraint

```text
Linked list length is ALWAYS even
```

👉 Every node gets exactly one twin

---

# 🧠 Core Challenge

In singly linked list:

```text
Cannot move backward ❌
```

👉 Hard to access:

* last node
* second-last node
* etc.

---

# 🔥 Key Insight

👉 Reverse second half of list

This converts:

```text
front ↔ back
```

into:

```text
front → front
```

👉 Now twin nodes align naturally

---

# ⚡ Full Strategy

---

## Step 1: Find middle

Use fast & slow pointers

```cpp
ListNode* slow = head;
ListNode* fast = head;

while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
}
```

---

## 🔍 Example

```text
1 → 2 → 3 → 4
```

After loop:

```text
slow = 3
```

👉 second half starts at `3`

---

# ⚡ Step 2: Reverse second half

---

## 🧠 Why reverse?

Without reverse:

```text
1 ↔ 4
2 ↔ 3
```

requires backward traversal ❌

After reverse:

```text
1 → 2
4 → 3
```

👉 easy forward comparison ✅

---

## 💻 Reverse code

```cpp
ListNode* prev = NULL;
ListNode* curr = slow;

while (curr) {
    ListNode* next = curr->next;

    curr->next = prev;

    prev = curr;
    curr = next;
}
```

---

## 🔍 Result

Original:

```text
1 → 2 → 3 → 4
```

After reversing second half:

```text
1 → 2 → 4 → 3
```

👉 `prev = 4`

---

# ⚡ Step 3: Compare twin sums

---

## Two pointers

```cpp
ListNode* first = head;
ListNode* second = prev;
```

---

## Compare

```cpp
int maxSum = 0;

while (second) {
    maxSum = max(maxSum, first->val + second->val);

    first = first->next;
    second = second->next;
}
```

---

# 💻 Full Code

```cpp
class Solution {
public:
    int pairSum(ListNode* head) {

        // Step 1: Find middle
        ListNode* slow = head;
        ListNode* fast = head;

        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse second half
        ListNode* prev = NULL;
        ListNode* curr = slow;

        while (curr) {
            ListNode* next = curr->next;

            curr->next = prev;

            prev = curr;
            curr = next;
        }

        // Step 3: Compare twin sums
        ListNode* first = head;
        ListNode* second = prev;

        int maxSum = 0;

        while (second) {
            maxSum = max(maxSum, first->val + second->val);

            first = first->next;
            second = second->next;
        }

        return maxSum;
    }
};
```

---

# 🔍 Full Walkthrough

```text
Input:
1 → 2 → 3 → 4
```

---

## Find middle

```text
slow = 3
```

---

## Reverse second half

```text
4 → 3
```

---

## Compare

```text
1 + 4 = 5
2 + 3 = 5
```

👉 Maximum:

```text
5
```

---

# 🧠 Mental Model (REVISION KEY)

> “Reverse second half so twin nodes line up”

---

# ⚡ Why this is efficient

Without reverse:

```text
Need repeated traversal from front ❌
O(n²)
```

With reverse:

```text
Single pass comparison ✅
O(n)
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Forgetting list length is even
* ❌ Incorrect reverse logic
* ❌ Returning wrong pointer after reverse
* ❌ Losing list during reversal
* ❌ Not understanding why reverse is needed

---

# 🧠 Patterns Learned

* Fast & Slow Pointer
* Reverse Linked List
* Two-pointer comparison

---

# 🏷 Tags

* Linked List
* Two Pointers

---

# 🔥 Recognition Pattern

Use this approach when:

* Need front ↔ back comparison
* Cannot traverse backward
* Need O(1) space solution
