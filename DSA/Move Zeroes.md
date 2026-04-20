# 🧩 Move Zeroes

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/move-zeroes/](https://leetcode.com/problems/move-zeroes/)

---

# 🧠 Problem Understanding

Given an array:

* Move all `0`s to the end
* Maintain **relative order** of non-zero elements
* Do it **in-place**

---

## 🔍 Example

```text
Input:  [0,1,0,3,12]
Output: [1,3,12,0,0]
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Identify constraint

👉 “Maintain order”
→ Cannot arbitrarily swap with end ❌

---

## 🧩 Step 2: Key idea

> “Bring non-zero elements forward”

---

## 🧩 Step 3: Use two pointers

| Pointer | Role                            |
| ------- | ------------------------------- |
| `i`     | scans array                     |
| `k`     | position to place next non-zero |

---

# ⚡ Approach 1: Overwrite + Fill (your approach)

---

## 🧠 Algorithm

1. Traverse array
2. Copy non-zero elements to front
3. Fill remaining with zeros

---

## 💻 Code

```cpp id="overwrite"
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int n = nums.size();
        int k = 0;

        // move non-zero elements forward
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) {
                nums[k++] = nums[i];
            }
        }

        // fill remaining with zeros
        while (k < n) {
            nums[k++] = 0;
        }
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ⚡ Approach 2: Swap (Cleaner)

---

## 🧠 Idea

👉 Instead of overwriting:

> Swap current non-zero with position `k`

---

## 💻 Code

```cpp id="swap"
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int k = 0;

        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                swap(nums[i], nums[k]);
                k++;
            }
        }
    }
};
```

---

## 🧠 Why this is better

* Single loop
* No second pass
* Cleaner logic

---

# ❗ Common Mistakes

* ❌ Swapping with end (breaks order)
* ❌ Forgetting to fill zeros
* ❌ Infinite loop (`k` not incremented)
* ❌ Using extra array (not in-place)

---

# 🧠 Patterns Learned

* Two-pointer technique
* Stable partitioning
* In-place array manipulation

---

# 🏷 Tags

* Array
* Two Pointers

---

# 🔥 Mental Model (REVISION KEY)

> “Collect all non-zeros at front, push zeros to back”

---

# ⚡ Recognition Pattern

Use this when:

* Need to move elements based on condition
* Order must be preserved
* In-place modification required

---

# 🧠 Template (Reusable)

```text
k = 0
for i in range:
    if condition:
        swap/assign to k
        k++
```

---

# 🚀 Takeaway

* Think in terms of **positions, not values**
* Separate:

  * where to read (`i`)
  * where to write (`k`)
