# 🟦 LeetCode 198 - House Robber

**Difficulty:** Medium
**Pattern:** Dynamic Programming (1D DP)
**Time Complexity:** `O(n)`
**Space Complexity:** `O(n)` → Optimizable to `O(1)`

---

# 📌 Problem

A robber plans to rob houses along a street.

The only constraint is:

> **Two adjacent houses cannot be robbed.**

Return the **maximum amount of money** that can be robbed.

---

# Examples

### Example 1

```cpp
Input:
nums = [1,2,3,1]

Output:
4

Explanation:

Rob house 0 and house 2

1 + 3 = 4
```

---

### Example 2

```cpp
Input:
nums = [2,7,9,3,1]

Output:
12

Explanation:

Rob house 0, 2 and 4

2 + 9 + 1 = 12
```

---

# 💡 Key Observation

At every house, we always have **two choices**.

## Choice 1

Rob the current house.

If we rob house `i`, we **cannot** rob house `i-1`.

Money becomes

```cpp
dp[i-2] + nums[i]
```

---

## Choice 2

Skip the current house.

Then whatever maximum money we had till the previous house remains.

```cpp
dp[i-1]
```

---

Take the better choice.

---

# DP State

Define

```cpp
dp[i]
```

as

> **Maximum amount of money that can be robbed from houses `0` to `i` (inclusive).**

---

# Base Cases

Only one house

```cpp
dp[0] = nums[0];
```

---

Two houses

```cpp
dp[1] = max(nums[0], nums[1]);
```

because both cannot be robbed together.

---

# DP Transition

For every house

Two choices

### Skip current

```cpp
dp[i-1]
```

---

### Rob current

```cpp
dp[i-2] + nums[i]
```

Hence

```cpp
dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
```

---

# Dry Run

```cpp
nums = [2,7,9,3,1]
```

---

### Initialize

```cpp
dp[0] = 2

dp[1] = 7
```

Current DP

```text
[2,7]
```

---

### i = 2

House value

```text
9
```

Option 1

```cpp
dp[1] = 7
```

Option 2

```cpp
dp[0] + 9

2 + 9 = 11
```

Take maximum

```cpp
dp[2] = 11
```

DP

```text
[2,7,11]
```

---

### i = 3

House value

```text
3
```

Option 1

```cpp
dp[2] = 11
```

Option 2

```cpp
dp[1] + 3

7 + 3 = 10
```

Take maximum

```cpp
dp[3] = 11
```

DP

```text
[2,7,11,11]
```

---

### i = 4

House value

```text
1
```

Option 1

```cpp
dp[3] = 11
```

Option 2

```cpp
dp[2] + 1

11 + 1 = 12
```

Take maximum

```cpp
dp[4] = 12
```

Final DP

```text
[2,7,11,11,12]
```

Answer

```cpp
12
```

---

# Why is DP size `n` and not `n+1`?

This is one of the most common DP questions.

It depends on **what `dp[i]` represents**.

Here,

```cpp
dp[i]
```

means

> Maximum money till **house index `i`**.

If there are `n` houses,

indices are

```text
0 1 2 ... n-1
```

The last state is

```cpp
dp[n-1]
```

Hence we only need

```cpp
vector<int> dp(n);
```

---

### Compare with Word Break

There,

```cpp
dp[i]
```

means

> Can the **first `i` characters** be segmented?

States are

```text
0 1 2 ... n
```

because `dp[n]` represents the entire string.

Hence

```cpp
vector<bool> dp(n+1);
```

---

### Rule

If `dp[i]` represents

* **Array index** → use `n`
* **Steps / Prefix / Amount / Abstract State** → use `n+1`

Examples

| Problem         | DP Size    |
| --------------- | ---------- |
| House Robber    | `n`        |
| LIS             | `n`        |
| Climbing Stairs | `n+1`      |
| Coin Change     | `amount+1` |
| Word Break      | `n+1`      |

---

# Algorithm

1. Handle edge cases.
2. Initialize

```cpp
dp[0] = nums[0];

dp[1] = max(nums[0], nums[1]);
```

3. For every remaining house

```cpp
dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
```

4. Return

```cpp
dp[n-1];
```

---

# C++ Solution (DP)

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {

        int n = nums.size();

        if (n == 0)
            return 0;

        if (n == 1)
            return nums[0];

        vector<int> dp(n);

        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }

        return dp[n - 1];
    }
};
```

---

# Space Optimization

Observe the recurrence

```cpp
dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
```

We only need

* `dp[i-1]`
* `dp[i-2]`

So instead of an array,

store only two variables.

---

## Initialization

```cpp
prev2 = nums[0];

prev1 = max(nums[0], nums[1]);
```

where

```text
prev2 = dp[i-2]

prev1 = dp[i-1]
```

---

For every house

```cpp
current = max(prev1, prev2 + nums[i]);

prev2 = prev1;

prev1 = current;
```

Finally

```cpp
return prev1;
```

---

# Optimized C++ Solution

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {

        int n = nums.size();

        if (n == 0)
            return 0;

        if (n == 1)
            return nums[0];

        int prev2 = nums[0];
        int prev1 = max(nums[0], nums[1]);

        for (int i = 2; i < n; i++) {

            int current = max(prev1, prev2 + nums[i]);

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

---

# Complexity Analysis

### DP Solution

**Time Complexity**

```text
O(n)
```

**Space Complexity**

```text
O(n)
```

---

### Optimized Solution

**Time Complexity**

```text
O(n)
```

**Space Complexity**

```text
O(1)
```

---

# Common Mistakes

### ❌ Using `vector<int> dp(n+1)`

Wrong.

`dp[i]` represents **house index `i`**, so only `0...n-1` are needed.

Use

```cpp
vector<int> dp(n);
```

---

### ❌ Returning `dp[n]`

Wrong.

The last valid state is

```cpp
dp[n-1];
```

---

### ❌ Forgetting edge cases

Always handle

```cpp
n == 0
```

and

```cpp
n == 1
```

Otherwise,

```cpp
dp[1]
```

may access an invalid index.

---

### ❌ Robbing adjacent houses

Incorrect thinking

```text
2 + 7 + 9
```

is illegal because

```text
7 and 9
```

are adjacent.

---

# Pattern Recognition

This problem belongs to the

> **Take / Skip DP Pattern**

At every position,

you decide:

* **Take** current element
* **Skip** current element

General recurrence

```cpp
dp[i] = max(skip, take);
```

where

```cpp
skip = dp[i-1];

take = dp[i-2] + currentValue;
```

---

# Similar Problems

* LeetCode 213 – House Robber II
* LeetCode 337 – House Robber III
* LeetCode 740 – Delete and Earn
* LeetCode 198 – House Robber (Space Optimized)
* Maximum Sum of Non-Adjacent Elements

---

# Interview Takeaways

* Define the DP state clearly:

  > `dp[i]` = Maximum money till house `i`.
* At every house, make a **Take vs Skip** decision.
* The recurrence is:

```cpp
dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
```

* Since the recurrence depends only on the previous two states, optimize the space from **O(n)** to **O(1)** using `prev1` and `prev2`.
* Remember the DP sizing rule:

  * **Array-index DP** (`House Robber`, `LIS`) → `vector<T> dp(n)`
  * **State-based DP** (`Climbing Stairs`, `Coin Change`, `Word Break`) → `vector<T> dp(n+1)`
