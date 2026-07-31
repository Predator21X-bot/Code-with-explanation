# 🟦 LeetCode 300 - Longest Increasing Subsequence (LIS)

**Difficulty:** Medium
**Pattern:** Dynamic Programming (1D DP)
**Time Complexity:** `O(n²)`
**Space Complexity:** `O(n)`

---

# 📌 Problem

Given an integer array `nums`, return the length of the **Longest Increasing Subsequence**.

A subsequence is obtained by deleting some (or no) elements without changing the order of the remaining elements.

> **Increasing** means every next element must be **strictly greater** than the previous one.

---

# Examples

### Example 1

```cpp
Input:
nums = [10,9,2,5,3,7,101,18]

Output:
4

Explanation:
Longest Increasing Subsequence = [2,3,7,101]
```

---

### Example 2

```cpp
Input:
nums = [0,1,0,3,2,3]

Output:
4
```

---

### Example 3

```cpp
Input:
nums = [7,7,7,7,7]

Output:
1
```

---

# 💡 Key Observation

The subsequence:

* **does not have to be contiguous**
* **can end at any index**

Therefore, while solving, we compute the best subsequence ending at every index.

---

# DP State

Define

```cpp
dp[i]
```

as

> **Length of the Longest Increasing Subsequence ending exactly at index `i`.**

Notice the words **ending at i**.

---

# Base Case

Every element alone is an increasing subsequence.

So,

```cpp
dp[i] = 1
```

for every index.

Hence,

```cpp
vector<int> dp(n,1);
```

---

# Transition

For every previous element,

```cpp
nums[j]
```

where

```cpp
j < i
```

If

```cpp
nums[j] < nums[i]
```

then we can extend that subsequence.

Candidate length becomes

```cpp
dp[j] + 1
```

Therefore,

```cpp
dp[i] = max(dp[i], dp[j] + 1);
```

---

# Dry Run

```
nums = [10,9,2,5,3,7]
```

Initially

```
dp = [1,1,1,1,1,1]
```

---

## i = 3 (value = 5)

Possible previous smaller element

```
2
```

```
dp[3] = dp[2] + 1
      = 2
```

DP becomes

```
[1,1,1,2,1,1]
```

---

## i = 4 (value = 3)

Only

```
2 < 3
```

```
dp[4] = 2
```

DP

```
[1,1,1,2,2,1]
```

---

## i = 5 (value = 7)

Possible predecessors

```
2
5
3
```

Candidates

```
dp[2]+1 = 2

dp[3]+1 = 3

dp[4]+1 = 3
```

Take maximum

```
dp[5] = 3
```

Final DP

```
[1,1,1,2,2,3]
```

---

# Why don't we return `dp[n-1]`?

Because

```
dp[i]
```

means

> LIS **ending at index i**

The longest subsequence may end anywhere.

Example

```
nums = [1,2,3,0]
```

DP becomes

```
[1,2,3,1]
```

Here,

```
dp[3] = 1
```

But answer is

```
3
```

Hence,

```cpp
return *max_element(dp.begin(), dp.end());
```

---

# Algorithm

1. Create DP array initialized with `1`.
2. For every index `i`
3. Check every previous index `j`
4. If `nums[j] < nums[i]`

   * Update

```cpp
dp[i] = max(dp[i], dp[j] + 1);
```

5. Return maximum value in DP array.

---

# C++ Solution

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {

        int n = nums.size();

        vector<int> dp(n, 1);

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < i; j++) {

                if (nums[j] < nums[i]) {

                    dp[i] = max(dp[i], dp[j] + 1);

                }

            }

        }

        return *max_element(dp.begin(), dp.end());
    }
};
```

---

# Complexity Analysis

### Time Complexity

Outer loop

```
O(n)
```

Inner loop

```
O(n)
```

Overall

```
O(n²)
```

---

### Space Complexity

DP array

```
O(n)
```

---

# Common Mistakes

### ❌ Returning `dp[n-1]`

Wrong because LIS may end before the last index.

Always return

```cpp
*max_element(dp.begin(), dp.end())
```

---

### ❌ Initializing DP with 0

Wrong.

Every element itself forms a subsequence.

Initialize with

```cpp
vector<int> dp(n,1);
```

---

### ❌ Using `<=`

Wrong.

Problem asks for **strictly increasing**.

Correct

```cpp
nums[j] < nums[i]
```

---

### ❌ Forgetting to check all previous indices

Don't compare only with

```
i-1
```

The best predecessor can be **any** previous index.

---

# Pattern Recognition

Look for this DP pattern when:

* Longest Increasing Subsequence
* Longest chain ending at current index
* Extend previous valid states
* "Ending at index i"
* Compare current element with previous elements

Typical recurrence

```cpp
for (int i = 0; i < n; i++) {

    for (int j = 0; j < i; j++) {

        if (condition) {

            dp[i] = max(dp[i], dp[j] + 1);

        }

    }

}
```

---

# Similar Problems

* LeetCode 673 – Number of Longest Increasing Subsequence
* LeetCode 646 – Maximum Length of Pair Chain
* LeetCode 354 – Russian Doll Envelopes
* LeetCode 368 – Largest Divisible Subset
* Longest Common Subsequence (2D DP)
* Edit Distance
* Distinct Subsequences

---

# Interview Takeaways

* Define the DP state **precisely**.
* `dp[i]` represents the **best answer ending at index `i`**.
* Initialize every state to its minimum valid answer (`1`).
* Check all valid previous states.
* Extend only when the condition (`nums[j] < nums[i]`) is satisfied.
* Since the optimal subsequence can end anywhere, return the maximum value in the DP array instead of `dp[n-1]`.
