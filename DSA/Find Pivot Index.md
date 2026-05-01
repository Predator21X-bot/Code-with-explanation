# 🧩 Find Pivot Index

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/find-pivot-index/](https://leetcode.com/problems/find-pivot-index/)

---

# 🧠 Problem Understanding

Given:

* Array `nums`

👉 Find index `i` such that:

```text
sum of elements before i == sum of elements after i
```

---

## 🔍 Example

```text
nums = [1,7,3,6,5,6]
Output = 3
```

👉 At index `3`:

```text
Left = 1+7+3 = 11  
Right = 5+6 = 11
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Brute force

For each index:

* compute left sum → O(n)
* compute right sum → O(n)

---

## ⏱ Complexity

```text
O(n²) ❌
```

---

## 🧩 Step 2: Optimization idea

> Don’t recompute sums again and again

---

## 🧠 Key trick

👉 Let:

```text
totalSum = sum of entire array
```

👉 While iterating:

```text
leftSum = sum before index i
rightSum = totalSum - leftSum - nums[i]
```

---

## 🧩 Step 3: Pivot condition

```cpp
leftSum == totalSum - leftSum - nums[i]
```

---

# ⚡ Approach: Prefix Sum (on-the-fly)

---

## 💻 Code

```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int totalSum = 0;

        for (int num : nums) {
            totalSum += num;
        }

        int leftSum = 0;

        for (int i = 0; i < nums.size(); i++) {
            if (leftSum == totalSum - leftSum - nums[i]) {
                return i;
            }
            leftSum += nums[i];
        }

        return -1;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Returning `nums[i]` instead of `i`
* ❌ Recomputing sums → O(n²)
* ❌ Wrong formula for right sum
* ❌ Updating `leftSum` before checking condition

---

# 🧠 Patterns Learned

* Prefix sum (running sum)
* Eliminating redundant computation
* Left vs right partition logic

---

# 🏷 Tags

* Array
* Prefix Sum

---

# 🔥 Mental Model (REVISION KEY)

> “Left sum equals everything except current and left”

---

# ⚡ Prefix Sum Template

```text
totalSum = sum of array
leftSum = 0

for each index i:
    if leftSum == totalSum - leftSum - nums[i]:
        return i
    leftSum += nums[i]
```

---

# 🧠 When to use this pattern

Look for:

* left vs right comparison
* partition of array
* balance / equilibrium index

---

# 🔥 Related Problems (VERY IMPORTANT)

---

## 🔹 Equilibrium Index (same concept)

---

## 🔹 Subarray Sum Equals K

👉 prefix sum + hashmap

---

## 🔹 Range Sum Query

👉 precomputed prefix array

---

# 🚀 Takeaway

* Always avoid recomputing sums
* Convert problems into:

  > “total - known = unknown”
