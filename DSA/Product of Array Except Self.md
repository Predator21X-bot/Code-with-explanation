# 🧩 Product of Array Except Self

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/product-of-array-except-self/](https://leetcode.com/problems/product-of-array-except-self/)

---

## 🧠 Problem Understanding

Given an array `nums`, return an array `answer` such that:

```text
answer[i] = product of all elements except nums[i]
```

### ❗ Constraints:

* Do NOT use division
* Solve in **O(n)** time

---

## 🔍 Key Observations

* For each index `i`, we need:

```text
(product of elements before i) × (product of elements after i)
```

---

# 🐢 Approach 1: Brute Force

---

## 🧠 Idea

For each index:

* Multiply all other elements

---

## ⏱ Complexity

* Time: **O(n²)**
* Space: **O(1)**

---

## ❌ Drawback

* Too slow for large inputs

---

# ⚠️ Approach 2: Division (Not Allowed)

---

## 🧠 Idea

* Compute total product
* Divide by `nums[i]`

---

## ❌ Issues

* Not allowed by problem
* Breaks when array contains `0`

---

# ⚡ Approach 3: Prefix + Suffix Arrays

---

## 🧠 Idea

* Compute:

  * `prefix[i]` → product before `i`
  * `suffix[i]` → product after `i`

---

## 💻 Code

```cpp
prefix[i] = prefix[i-1] * nums[i-1]
suffix[i] = suffix[i+1] * nums[i+1]

answer[i] = prefix[i] * suffix[i]
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# 🚀 Approach 4: Optimized (Best)

---

## 🧠 Core Idea

* Store prefix directly in result array
* Use a variable for suffix

---

## 💻 Code

```cpp id="opt-code"
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> result(n);

        result[0] = 1;

        // Prefix pass
        for (int i = 1; i < n; i++) {
            result[i] = result[i - 1] * nums[i - 1];
        }

        int suffix = 1;

        // Suffix pass
        for (int i = n - 1; i >= 0; i--) {
            result[i] *= suffix;
            suffix *= nums[i];
        }

        return result;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)** (excluding output)

---

# 🧠 Dry Run

Input:

```text
nums = [1,2,3,4]
```

---

## Prefix pass

```text
result = [1, 1, 2, 6]
```

---

## Suffix pass

```text
final = [24, 12, 8, 6]
```

---

# ❗ Common Mistakes

* Starting prefix loop from `i = 0` ❌
* Using `nums[i]` instead of `nums[i-1]` ❌
* Forgetting suffix multiplication ❌
* Using division ❌

---

# 🧠 Patterns Learned

* Prefix/Suffix pattern
* Space optimization
* Two-pass traversal

---

# 🏷 Tags

* Arrays
* Prefix/Suffix
* Optimization

---

# 🧠 When to Use This

* When problem involves:

  * “product except self”
  * cumulative operations
  * left + right contributions

---

# 🔥 Mental Model (Revision Key)

> “Store prefix → multiply suffix on the fly”

---

# ⚡ Key Insight

* You don’t need full prefix & suffix arrays
* You can reuse result array smartly

---

# 🚀 Takeaway

* Always think:

  * Can I reuse computation?
  * Can I reduce space?
