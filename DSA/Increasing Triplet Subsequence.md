# 🧩 Increasing Triplet Subsequence

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/increasing-triplet-subsequence/](https://leetcode.com/problems/increasing-triplet-subsequence/)

---

## 🧠 Problem Understanding

Given an array `nums`, return `true` if there exists:

```text
i < j < k such that nums[i] < nums[j] < nums[k]
```

Otherwise return `false`.

---

## 🔍 Key Observations

* We need **3 increasing numbers in order**
* Not necessarily consecutive
* Order matters (indices increasing)

---

# 🐢 Approach 1: Brute Force

---

## 🧠 Idea

* Use 3 nested loops
* Check all triplets

---

## ⏱ Complexity

* Time: **O(n³)** ❌
* Space: **O(1)**

---

## ❌ Drawback

* Too slow for large inputs

---

# ⚡ Approach 2: Greedy (Optimal)

---

## 🧠 Core Idea

Maintain two variables:

```cpp
first  → smallest number so far  
second → smallest number greater than first
```

---

## 🧠 Logic

For each number `num`:

```cpp
if (num <= first)
    first = num;
else if (num <= second)
    second = num;
else
    return true;
```

---

## 🧠 Why it works

* `first` tracks smallest candidate
* `second` tracks next valid candidate
* If a number > `second` appears → triplet found

---

## 💻 Code

```cpp id="triplet-code"
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        int first = INT_MAX;
        int second = INT_MAX;

        for (int num : nums) {
            if (num <= first) {
                first = num;
            } else if (num <= second) {
                second = num;
            } else {
                return true;
            }
        }

        return false;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# 🧠 Dry Run

Input:

```text
[2, 1, 5, 0, 4, 6]
```

---

## Step-by-step

```text
first = 2
first = 1
second = 5
first = 0
second = 4
num = 6 → 6 > 4 → ✅ return true
```

---

# ❗ Common Mistakes

### ❌ Wrong initialization

```cpp
int first = nums[0];
int second = nums[1];
```

👉 Breaks logic if order is not sorted

---

### ✅ Correct

```cpp
int first = INT_MAX;
int second = INT_MAX;
```

---

### ❌ Using `<` instead of `<=`

👉 May fail for duplicates

---

### ❌ Thinking in terms of indices

👉 This is **value-based greedy**, not index tracking

---

# 🧠 Patterns Learned

* Greedy tracking
* Candidate optimization
* One-pass scanning

---

# 🏷 Tags

* Arrays
* Greedy
* Optimization

---

# 🧠 When to Use This

* When problem asks:

  * increasing subsequence
  * existence of pattern
* When you can track **partial answers**

---

# 🔥 Mental Model (Revision Key)

> “Track smallest → track next bigger → if bigger appears → done”

---

# ⚡ Key Insight

* You don’t need all triplets
* Just maintain **best candidates**

---

# 🚀 Takeaway

* Greedy reduces complexity drastically
* Always ask:

  > “What minimum info do I need to track?”
