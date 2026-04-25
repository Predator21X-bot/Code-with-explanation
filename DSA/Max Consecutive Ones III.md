# 🧩 Max Consecutive Ones III

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/max-consecutive-ones-iii/](https://leetcode.com/problems/max-consecutive-ones-iii/)

---

# 🧠 Problem Understanding

Given:

* Binary array `nums`
* Integer `k`

👉 You can flip at most `k` zeros → `1`

👉 Goal:

> Find **longest subarray of 1s**

---

## 🔍 Key Translation

```text
Flip ≤ k zeros → window can have at most k zeros
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Identify pattern

👉 Not fixed size
👉 Need longest valid subarray

---

## 🧠 Conclusion

> This is a **Variable Sliding Window problem**

---

## 🧩 Step 2: What to track?

```text
zeroCount = number of zeros in current window
```

---

## 🧩 Step 3: Valid condition

```text
zeroCount ≤ k  → valid window
zeroCount > k  → invalid window
```

---

# ⚡ Approach: Variable Sliding Window

---

## 🧠 Core idea

> Expand → if invalid → shrink → update answer

---

## 💻 Code

```cpp id="code"
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        int left = 0;
        int zeroCount = 0;
        int maxLen = 0;

        for (int right = 0; right < nums.size(); right++) {

            // expand
            if (nums[right] == 0) zeroCount++;

            // shrink until valid
            while (zeroCount > k) {
                if (nums[left] == 0) zeroCount--;
                left++;
            }

            // update answer
            maxLen = max(maxLen, right - left + 1);
        }

        return maxLen;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Using fixed window approach
* ❌ Not shrinking window properly
* ❌ Using `if` instead of `while` (less robust)
* ❌ Forgetting to update answer after shrink

---

# 🧠 Patterns Learned

* Variable sliding window
* Constraint-based window
* Dynamic resizing

---

# 🏷 Tags

* Array
* Sliding Window

---

# 🔥 Mental Model (REVISION KEY)

> “Grow window → fix when invalid → record best”

---

# ⚡ Sliding Window Comparison (VERY IMPORTANT)

---

## 🔹 Fixed Window

```text
Window size = k (constant)
```

### Examples:

* max sum
* max average
* max vowels

---

## 🔹 Variable Window (THIS)

```text
Window size changes dynamically
```

### Examples:

* longest substring
* max consecutive ones
* min subarray ≥ target

---

# 🧠 Generic Template (MUST REMEMBER)

```cpp id="template"
int left = 0;

for (int right = 0; right < n; right++) {

    // expand
    update state

    while (invalid condition) {
        // shrink
        fix state
        left++;
    }

    // valid window
    update answer
}
```

---

# 🧠 How to identify variable window

Look for:

* “longest / smallest subarray”
* “at most k” / “at least k”
* condition-based constraints

---

# 🚀 Takeaway

* Don’t fix window size — let it adapt
* Always maintain **valid window**
* Track only what matters (zeroCount here)
