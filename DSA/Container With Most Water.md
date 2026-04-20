# 🧩 Container With Most Water

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/container-with-most-water/](https://leetcode.com/problems/container-with-most-water/)

---

# 🧠 Problem Understanding

Given an array `height`:

* Each index represents a vertical line
* Choose two lines → form a container
* Find **maximum water area**

---

## 🔍 Formula

```text
Area = (j - i) × min(height[i], height[j])
       width       limiting height
```

---

## 🔍 Example

```text id="mj8hso"
height = [1,8,6,2,5,4,8,3,7]
Output = 49
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Brute force

* Try all pairs → **O(n²)** ❌

---

## 🧩 Step 2: Key observation

> Area depends on:

* width (`j - i`)
* smaller height

---

## 🧩 Step 3: Strategy

👉 Start with:

```text
i = 0, j = n-1 (maximum width)
```

---

## 🧩 Step 4: Greedy insight

> Move the pointer with **smaller height**

---

## 🧠 Why?

If:

```text id="7kr17q"
height[i] < height[j]
```

👉 Area limited by `height[i]`

👉 Moving `j`:

* width ↓
* height limit unchanged ❌

👉 Only option:

```text id="db4wbd"
move i → maybe find taller height
```

---

# ⚡ Approach: Two Pointers

---

## 💻 Code

```cpp id="code"
class Solution {
public:
    int maxArea(vector<int>& height) {
        int n = height.size();
        int i = 0, j = n - 1;
        int maxArea = 0;

        while (i < j) {
            int area = (j - i) * min(height[i], height[j]);
            maxArea = max(maxArea, area);

            if (height[i] < height[j]) {
                i++;
            } else {
                j--;
            }
        }

        return maxArea;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Moving both pointers
* ❌ Moving larger height pointer
* ❌ Using max instead of min in formula
* ❌ Not understanding why pointer moves

---

# 🧠 Patterns Learned

* Two-pointer optimization
* Greedy decision making
* Shrinking window

---

# 🏷 Tags

* Array
* Two Pointers
* Greedy

---

# 🔥 Mental Model (REVISION KEY)

> “Start widest → improve limiting factor”

---

# ⚡ Recognition Pattern

Use this when:

* Need to **maximize/minimize something between two indices**
* Both ends matter
* Brute force is O(n²)
* Decision depends on **min/max of two values**

---

# 🧠 Comparison with other two-pointer patterns

---

## 🔹 Type 1: Scan (Subsequence)

```text id="13ppg1"
i and j move independently
Goal: match pattern
```

---

## 🔹 Type 2: Rearrangement (Move Zeroes)

```text id="g4wb72"
i scans, k writes
Goal: reorder elements
```

---

## 🔹 Type 3: Optimization (THIS PROBLEM)

```text id="uhskka"
i and j start at ends
Goal: maximize/minimize value
```

---

# 🚀 Takeaway

* Always ask:

  * What is limiting factor?
  * Can I eliminate cases greedily?
