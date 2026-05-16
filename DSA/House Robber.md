# House Robber

## Problem Summary

We are given:

```cpp id="m1r8v4"
nums[i]
```

Meaning:

```text id="q7m2k5"
money inside house i
```

---

# Rule

We CANNOT rob:

```text id="x4m9r1"
two adjacent houses
```

because security alarms trigger.

---

# Goal

Find:

# maximum money

that can be robbed.

---

# Example 1

## Input

```text id="t8m1k6"
[1,2,3,1]
```

---

# Option 1

Rob:

```text id="f5m7r2"
1 + 3
```

Total:

```text id="p2m8v4"
4
```

---

# Option 2

Rob:

```text id="v6m1r9"
2 + 1
```

Total:

```text id="m4r8k1"
3
```

Best answer:

```text id="q9m2v7"
4
```

---

# Key Observation

At every house we have ONLY TWO choices:

# Choice 1 — Rob Current House

If we rob current house:

```text id="x3m7r5"
cannot rob previous house
```

So total becomes:

```text id="f8m1k2"
nums[i] + dp[i-2]
```

---

# Choice 2 — Skip Current House

Then best money remains:

```text id="p5m9r1"
dp[i-1]
```

---

# Recurrence Relation

So:

```cpp id="t2m7v4"
dp[i] =
max(
    dp[i-1],
    nums[i] + dp[i-2]
);
```

---

# MOST Important DP Definition

Define:

```text id="n8m4r6"
dp[i]
```

as:

# maximum money we can rob up to house i

This definition is EVERYTHING.

---

# Why Recurrence Works

At house `i`:

## Option 1 — Skip House

Best answer remains:

```text id="r4m1k8"
dp[i-1]
```

---

## Option 2 — Rob House

If we rob current house:

```text id="q7m5v2"
cannot rob previous house
```

So:

```text id="x1m8r4"
nums[i] + dp[i-2]
```

---

Choose best option:

```cpp id="m9r2k5"
max(...)
```

---

# Example

## Input

```text id="v5m7r1"
[2,7,9,3,1]
```

---

# Base Cases

## House 0

Only one option:

```text id="f2m8v6"
rob house 0
```

So:

```cpp id="t4m1r9"
dp[0] = nums[0];
```

---

## House 1

Choose better of:

```text id="q8m5r2"
house 0
or
house 1
```

So:

```cpp id="x6m1v7"
dp[1] = max(nums[0], nums[1]);
```

---

# Dry Run

## Initial

```text id="m3r8k1"
nums = [2,7,9,3,1]
```

---

# dp[0]

```text id="p7m2v4"
2
```

---

# dp[1]

```text id="f4m9r2"
max(2,7) = 7
```

---

# Compute dp[2]

## Skip current house

```text id="q1m7k8"
dp[1] = 7
```

---

## Rob current house

```text id="v8m4r1"
9 + dp[0]
```

```text id="n5m2v7"
9 + 2 = 11
```

Best:

```text id="x2m8r5"
11
```

So:

```text id="m6r1k9"
dp[2] = 11
```

---

# Compute dp[3]

## Skip

```text id="t8m4v2"
11
```

---

## Rob

```text id="q3m7r1"
3 + 7 = 10
```

Best:

```text id="v9m2k4"
11
```

So:

```text id="f1m8r6"
dp[3] = 11
```

---

# Compute dp[4]

## Skip

```text id="p4m7v2"
11
```

---

## Rob

```text id="x7m1r5"
1 + 11 = 12
```

Best:

```text id="m2r9k8"
12
```

Final answer:

```text id="q5m8v1"
12
```

---

# Important Final Insight

Unlike stair problems:

```text id="t1m7r4"
there is NO extra position after last house
```

So final answer is simply:

```cpp id="f8m2k5"
dp[n-1]
```

because:

```text id="v4m9r1"
dp[n-1]
```

already means:

# best answer up to last house

---

# Complete DP Solution

```cpp id="n6m1v8"
class Solution {
public:

    int rob(vector<int>& nums) {

        int n = nums.size();

        // Edge case
        if(n == 1)
            return nums[0];

        vector<int> dp(n);

        // Base cases
        dp[0] = nums[0];

        dp[1] = max(nums[0], nums[1]);

        // Build DP
        for(int i = 2; i < n; i++){

            dp[i] =
            max(
                dp[i-1],
                nums[i] + dp[i-2]
            );
        }

        return dp[n-1];
    }
};
```

---

# Complexity Analysis

## Time Complexity

Single loop through array.

```text id="x5m2r7"
O(n)
```

---

## Space Complexity

DP array stores `n` values.

```text id="k4m9v2"
O(n)
```

---

# Space Optimization

Do we really need full DP array?

❌ No.

We only need:

```text id="w8m1r3"
previous 2 states
```

So optimize to:

```text id="q6m2v7"
O(1)
```

space.

---

# Optimized Variables

Store:

```text id="t8m1k5"
prev2 = dp[i-2]
prev1 = dp[i-1]
```

---

# Optimized Code

```cpp id="f2m7r9"
class Solution {
public:

    int rob(vector<int>& nums) {

        int n = nums.size();

        if(n == 1)
            return nums[0];

        int prev2 = nums[0];

        int prev1 =
        max(nums[0], nums[1]);

        for(int i = 2; i < n; i++){

            int current =
            max(
                prev1,
                nums[i] + prev2
            );

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

---

# Why Optimization Works

Current state depends ONLY on:

```text id="m5r8k1"
previous 2 states
```

So full DP array unnecessary.

---

# Important Pattern Learned

This problem teaches:

# Take or Skip DP

At every position:

```text id="x7m4v2"
take current
OR
skip current
```

Very important interview pattern.

---

# Similar Problems

* House Robber II
* Delete and Earn
* Maximum Sum Non Adjacent
* Paint House
* Partition Array

---

# Common Mistakes

## Mistake 1

Returning:

```cpp id="q9m1r6"
max(dp[n-1], dp[n-2])
```

Incorrect.

Final answer is already:

```cpp id="w4m8k2"
dp[n-1]
```

---

## Mistake 2

Forgetting edge case:

```text id="p1m7r5"
n == 1
```

Then:

```cpp id="t6m2v8"
dp[1]
```

becomes invalid.

---

## Mistake 3

Wrong DP definition.

Always define clearly:

```text id="r8m5k4"
What exactly does dp[i] represent?
```

Most DP errors come from unclear state meaning.

---

# Recognition Pattern

Whenever problem says:

* cannot take adjacent elements
* choose or skip
* maximize non-adjacent sum

think:

# Dynamic Programming

---

# Important Mental Models

## DP

```text id="x2m9r1"
best current answer
depends on
best smaller answers
```

---

## Take/Skip Pattern

```text id="m1r8v6"
take current + safe previous
OR
skip current
```

---

## State Transition

```text id="q6m2v7"
current state built from previous optimal states
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="t8m1k5"
making optimal decisions using already solved smaller subproblems
```

instead of recomputing everything repeatedly.
