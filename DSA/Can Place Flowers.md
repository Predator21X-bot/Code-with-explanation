# 🧩 Can Place Flowers

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/can-place-flowers/](https://leetcode.com/problems/can-place-flowers/)

---

## 🧠 Problem Understanding

We are given:

* A binary array `flowerbed[]`

  * `0` → empty plot
  * `1` → already planted
* An integer `n`

👉 Goal:

* Determine if we can plant `n` flowers such that **no two flowers are adjacent**

---

## 🔍 Key Observations

* To plant at index `i`, we must check:

  * Current is empty
  * Left neighbor is empty (or doesn’t exist)
  * Right neighbor is empty (or doesn’t exist)

---

## 🧠 Core Condition

```cpp
flowerbed[i] == 0 &&
(i == 0 || flowerbed[i - 1] == 0) &&
(i == size - 1 || flowerbed[i + 1] == 0)
```

---

## 🧪 Example

```cpp id="ex1"
Input:
flowerbed = [1,0,0,0,1], n = 1

Output:
true
```

---

## 🐢 Brute Force Idea

* Try placing flower at each position
* After placing, check entire array for adjacency violation

### ⏱ Complexity

* Time: O(n²)
* Space: O(1)

---

# ⚡ Optimized Approach 1 (Recommended — Greedy + Marking)

## 💡 Idea

* Traverse array once
* When valid:

  * Place flower (`flowerbed[i] = 1`)
  * Decrement `n`
* Stop early if `n == 0`

---

## 💻 Code

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        if (n == 0) return true;

        int size = flowerbed.size();

        for (int i = 0; i < size; i++) {
            if (flowerbed[i] == 0 &&
                (i == 0 || flowerbed[i - 1] == 0) &&
                (i == size - 1 || flowerbed[i + 1] == 0)) {

                flowerbed[i] = 1;
                n--;

                if (n == 0) return true;
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

# ⚡ Optimized Approach 2 (Skip Method — Advanced)

## 💡 Idea

* Instead of modifying array:

  * After planting → skip next index (`i += 2`)

---

## ⚠️ Caution

* Easy to make mistakes with boundaries
* Requires careful condition handling

---

## 💻 Code (Reference)

```cpp
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int size = flowerbed.size();

        for (int i = 0; i < size; ) {
            if (flowerbed[i] == 0 &&
                (i == 0 || flowerbed[i - 1] == 0) &&
                (i == size - 1 || flowerbed[i + 1] == 0)) {

                n--;
                if (n == 0) return true;

                i += 2; // skip next
            } else {
                i++;
            }
        }

        return false;
    }
};
```

---

## ⚠️ Edge Cases

* `n == 0` → always true
* Single element array
* All zeros
* All ones
* Alternating patterns

---

## ❗ Common Mistakes (VERY IMPORTANT)

* Mixing up:

  * `i-1` (left) and `i+1` (right)
* Accessing out-of-bounds indices
* Using wrong variable (`n-1` instead of `size-1`)
* Double increment (`i += 2` + `i++`)
* Forgetting base case:

  ```cpp
  if (n == 0) return true;
  ```
* Mixing two approaches:

  * skip logic + marking ❌

---

## 🧠 Patterns Learned

* Greedy placement
* Boundary handling using OR
* In-place simulation
* Early exit optimization

---

## 🏷 Tags

* Arrays
* Greedy
* Simulation

---

## 🧠 When to Use This

* When placing items with **adjacency constraints**
* When local decisions guarantee global optimum
* When skipping or marking simplifies logic

---

## 🔥 Mental Model (Revision Key)

> “Check 3 → plant → decrement → continue”

---

## 🚀 Takeaway

* Treat missing neighbors as **safe (0)**
* Prefer **marking approach** for clarity
* Avoid mixing strategies
