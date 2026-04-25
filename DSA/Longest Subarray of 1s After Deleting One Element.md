# 🧩 Longest Subarray of 1s After Deleting One Element

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)

---

# 🧠 Problem Understanding

Given:

* Binary array `nums`

👉 You must:

* **delete exactly one element**

👉 Goal:

> Find longest subarray of only `1`s

---

## 🔍 Example

```text id="ex1"
nums = [1,1,0,1]
Output = 3
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Reframe the problem

Instead of:

```text id="s7l4zq"
delete one element
```

👉 Think:

```text id="k93slb"
window can contain at most 1 zero
```

---

## 🧩 Step 2: Why?

* That one zero = the element we delete
* So we allow it inside window

---

## 🧩 Step 3: Track

```text id="h6s1xf"
zeroCount = number of zeros in window
```

---

## 🧩 Step 4: Valid condition

```text id="e4u6fm"
zeroCount ≤ 1 → valid  
zeroCount > 1 → shrink window
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
    int longestSubarray(vector<int>& nums) {
        int left = 0;
        int zeroCount = 0;
        int maxLen = 0;

        for (int right = 0; right < nums.size(); right++) {

            if (nums[right] == 0) zeroCount++;

            while (zeroCount > 1) {
                if (nums[left] == 0) zeroCount--;
                left++;
            }

            maxLen = max(maxLen, right - left); // delete one element
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

* ❌ Using `right - left + 1`
* ❌ Wrong condition (`zeroCount < 1`)
* ❌ Shrinking incorrectly
* ❌ Not handling edge case (all 1s)

---

# 🧠 Key Insight (VERY IMPORTANT 🔥)

---

## 🔹 Previous problem

### Max Consecutive Ones III

```text id="fjbkzr"
Allow ≤ k zeros
Answer = window size
→ right - left + 1
```

---

## 🔹 This problem

```text id="m3xf0r"
Allow ≤ 1 zero
BUT must delete one element
Answer = window size - 1
→ right - left
```

---

# ⚡ Mental Model

> “Keep one zero → delete it logically”

---

# 🧠 Pattern Comparison (GOLD 🔥)

| Problem                  | Condition | Answer         |
| ------------------------ | --------- | -------------- |
| Max Consecutive Ones III | ≤ k zeros | `right-left+1` |
| This problem             | ≤ 1 zero  | `right-left`   |

---

# 🏷 Tags

* Array
* Sliding Window

---

# 🔥 Variable Sliding Window Template

```cpp id="template"
int left = 0;

for (int right = 0; right < n; right++) {

    // expand
    update state

    while (invalid condition) {
        // shrink
        fix state
        left++
    }

    // update answer
}
```

---

# 🧠 Recognition Pattern

Use this when:

* longest / shortest subarray
* condition-based
* dynamic window size

---

# 🚀 Takeaway

* Always reinterpret constraints
* Small wording change → big logic change
* Sliding window = flexible tool
