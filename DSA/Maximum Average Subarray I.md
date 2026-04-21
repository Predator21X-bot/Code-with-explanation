# 🧩 Maximum Average Subarray I

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/maximum-average-subarray-i/](https://leetcode.com/problems/maximum-average-subarray-i/)

---

# 🧠 Problem Understanding

Given:

* Array `nums`
* Integer `k`

👉 Goal:

> Find **maximum average** of any subarray of size `k`

---

## 🔍 Example

```text id="ex1"
nums = [1,12,-5,-6,50,3], k = 4  
Output = 12.75
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Brute force

👉 For each subarray of size `k`:

* compute sum
* track max

---

## ⏱ Complexity

```text id="bf"
O(n * k) ❌
```

---

## 🧩 Step 2: Key observation

> Adjacent subarrays overlap

---

## 🧠 Insight

```text id="overlap"
[1,2,3] → sum = 6  
[2,3,4] → sum = 6 - 1 + 4
```

👉 Reuse previous sum instead of recomputing

---

# ⚡ Approach: Fixed Sliding Window

---

## 🧠 Core idea

> “Remove left, add right”

---

## 🧠 Window movement

```text id="window"
old window: [i ... i+k-1]  
new window: [i+1 ... i+k]
```

---

## 🧠 Formula

```cpp id="formula"
sum = sum - nums[i - k] + nums[i];
```

---

# 💻 Code

```cpp id="code"
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int sum = 0;

        // first window
        for (int i = 0; i < k; i++) {
            sum += nums[i];
        }

        int maxSum = sum;

        // sliding window
        for (int i = k; i < nums.size(); i++) {
            sum = sum - nums[i - k] + nums[i];
            maxSum = max(maxSum, sum);
        }

        return (double)maxSum / k;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Recomputing sum every time
* ❌ Wrong index (`i-k`)
* ❌ Forgetting first window initialization
* ❌ Integer division bug

  ```cpp
  double(maxSum / k) ❌
  ```

---

# 🧠 Patterns Learned

* Sliding window (fixed size)
* Optimization using overlap
* Running sum technique

---

# 🏷 Tags

* Array
* Sliding Window

---

# 🔥 Mental Model (REVISION KEY)

> “Reuse previous window → remove left, add right”

---

# ⚡ Recognition Pattern

Use this when:

* Subarray of **fixed size k**
* Need sum / average / max / min
* Continuous segment required

---

# 🧠 Template (Reusable)

```text id="template"
1. Build first window
2. Slide window:
   remove left
   add right
3. update answer
```

---

# 🚀 Fixed vs Variable Sliding Window

---

## 🔹 Fixed Window (THIS problem)

```text id="fixed"
size = k (constant)
```

👉 Example:

* max sum of size k
* max average

---

## 🔹 Variable Window (NEXT LEVEL 🔥)

```text id="variable"
window size changes dynamically
```

👉 Example:

* longest substring without repeating
* minimum subarray sum

---

# 🚀 Takeaway

* Don’t recompute → reuse
* Sliding window = optimized brute force
* Always identify:

  * fixed size vs variable size
