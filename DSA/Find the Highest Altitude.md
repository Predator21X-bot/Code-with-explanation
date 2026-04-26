# 🧩 Find the Highest Altitude

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/find-the-highest-altitude/](https://leetcode.com/problems/find-the-highest-altitude/)

---

# 🧠 Problem Understanding

Given:

* Array `gain[]`
* Starting altitude = **0**

👉 Each step:

```text id="p4w1sm"
altitude += gain[i]
```

👉 Goal:

> Find the **maximum altitude reached**

---

## 🔍 Example

```text id="ex1"
gain = [-5,1,5,0,-7]
altitudes = [0, -5, -4, 1, 1, -6]
Output = 1
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: What are we computing?

👉 Not max subarray
👉 Not sliding window

👉 This is:

```text id="l6m7px"
running / cumulative sum
```

---

## 🧩 Step 2: Prefix Sum idea

```text id="2l9u9x"
curr += gain[i]
```

👉 Each step builds on previous

---

## 🧩 Step 3: Track maximum

```cpp id="max"
maxAlt = max(maxAlt, curr);
```

---

# ⚡ Approach: Prefix Sum

---

## 💻 Code

```cpp id="code"
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int curr = 0;
        int maxAlt = 0;

        for (int i = 0; i < gain.size(); i++) {
            curr += gain[i];
            maxAlt = max(maxAlt, curr);
        }

        return maxAlt;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Not initializing `maxAlt = 0`
* ❌ Thinking it’s max subarray (Kadane)
* ❌ Overcomplicating with arrays

---

# 🧠 Patterns Learned

* Prefix sum (cumulative sum)
* Running total tracking
* Simple greedy tracking

---

# 🏷 Tags

* Array
* Prefix Sum

---

# 🔥 Mental Model (REVISION KEY)

> “Keep adding → track the highest point reached”

---

# ⚡ Prefix Sum Template

```text id="template"
curr = 0
for each element:
    curr += value
    update answer
```

---

# 🧠 When to use Prefix Sum

Look for:

* cumulative effect
* running total
* range sum queries
* altitude / balance / score tracking

---

# 🔥 Prefix Sum vs Other Patterns

| Pattern        | Use                    |
| -------------- | ---------------------- |
| Sliding Window | fixed/dynamic subarray |
| Prefix Sum     | cumulative values      |
| Kadane         | max subarray sum       |

---

# 🚀 Connection to Advanced Problems

This leads to:

---

## 🔹 Kadane’s Algorithm

```text id="kadane"
max subarray sum
```

---

## 🔹 Subarray Sum Equals K

```text id="subk"
prefix sum + hashmap
```

---

## 🔹 Range Sum Queries

```text id="range"
precompute prefix array
```

---

# 🚀 Takeaway

* Always ask:

  * Is this cumulative?
* If yes → prefix sum
