# 🧩 Max Number of K-Sum Pairs

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/max-number-of-k-sum-pairs/](https://leetcode.com/problems/max-number-of-k-sum-pairs/)

---

# 🧠 Problem Understanding

Given:

* Array `nums`
* Integer `k`

👉 Goal:

> Find maximum number of pairs such that:

```text
nums[i] + nums[j] = k
```

### ⚠️ Constraint:

* Each element can be used **only once**

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Recognize pattern

👉 We are:

* forming **pairs**
* under a **sum constraint**

---

## 🧩 Step 2: Optimize search

Brute force:

```text
Try all pairs → O(n²) ❌
```

---

## 🧩 Step 3: Key idea

👉 Sort the array

Why?

> Allows controlled movement using two pointers

---

## 🧩 Step 4: Two-pointer setup

```text
i → smallest element  
j → largest element
```

---

## 🧩 Step 5: Decision logic

Let:

```text
sum = nums[i] + nums[j]
```

| Condition  | Action              |
| ---------- | ------------------- |
| `sum == k` | `count++, i++, j--` |
| `sum < k`  | `i++`               |
| `sum > k`  | `j--`               |

---

## 🧠 Why it works

* Sorted array gives structure
* Small + large lets us adjust sum efficiently
* Each move eliminates invalid pairs

---

# ⚡ Approach 1: Sorting + Two Pointers

---

## 💻 Code

```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end());

        int i = 0, j = nums.size() - 1;
        int count = 0;

        while (i < j) {
            int sum = nums[i] + nums[j];

            if (sum == k) {
                count++;
                i++;
                j--;
            } 
            else if (sum < k) {
                i++;
            } 
            else {
                j--;
            }
        }

        return count;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n log n)** (sorting)
* Space: **O(1)**

---

# ⚡ Approach 2: HashMap (Alternative)

---

## 🧠 Idea

👉 For each number:

* Check if `(k - num)` exists
* Use frequency map

---

## 💻 Code

```cpp
class Solution {
public:
    int maxOperations(vector<int>& nums, int k) {
        unordered_map<int,int> mp;
        int count = 0;

        for (int num : nums) {
            if (mp[k - num] > 0) {
                count++;
                mp[k - num]--;
            } else {
                mp[num]++;
            }
        }

        return count;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# ❗ Common Mistakes

* ❌ Forgetting to move both pointers when `sum == k`
* ❌ Reusing elements
* ❌ Not sorting before two-pointer approach
* ❌ Infinite loop due to wrong pointer movement

---

# 🧠 Patterns Learned

* Two-pointer pairing
* Greedy elimination
* Complement matching

---

# 🏷 Tags

* Array
* Two Pointers
* HashMap

---

# 🔥 Mental Model (REVISION KEY)

> “Use smallest + largest to control the sum”

---

# ⚡ Two-Pointer Patterns (BIG PICTURE)

---

## 🔹 1. Scan Pattern (Subsequence)

```text
i and j move forward
Goal: match sequence
```

---

## 🔹 2. Rearrangement (Move Zeroes)

```text
i scans, k writes
Goal: reorder elements
```

---

## 🔹 3. Optimization (Container)

```text
i and j from ends
Goal: maximize/minimize value
```

---

## 🔹 4. Pairing (THIS PROBLEM)

```text
i and j from ends
Goal: find valid pairs
```

---

# 🧠 When to use which?

| Situation                 | Pattern       |
| ------------------------- | ------------- |
| match sequence            | scan          |
| rearrange array           | write pointer |
| maximize/minimize         | optimization  |
| find pairs with condition | pairing       |

---

# 🚀 Takeaway

* Sorting often unlocks two-pointer solutions
* Always ask:

  * Can I reduce search space?
  * Can I use order to my advantage?
