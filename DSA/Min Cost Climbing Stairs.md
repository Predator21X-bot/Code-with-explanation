# Min Cost Climbing Stairs

## Problem Summary

We are given:

```cpp id="m1r8v4"
cost[i]
```

Meaning:

```text id="q7m2k5"
cost to step on stair i
```

---

# Movement Rules

From stair `i`, we can move:

```text id="x4m9r1"
1 step
OR
2 steps
```

---

# Goal

Reach:

# top of staircase

with:

```text id="t8m1k6"
minimum total cost
```

---

# Important Detail

We may start from:

```text id="f5m7r2"
step 0
OR
step 1
```

Very important.

---

# Example 1

## Input

```text id="p2m8v4"
cost = [10,15,20]
```

---

# Option 1

Start at step 0:

```text id="v6m1r9"
10 -> 20
```

Total:

```text id="m4r8k1"
30
```

---

# Option 2

Start at step 1:

```text id="q9m2v7"
15 -> top
```

Total:

```text id="x3m7r5"
15
```

Minimum answer:

```text id="f8m1k2"
15
```

---

# Key Observation

At every stair:

```text id="p5m9r1"
we want cheapest way to reach there
```

This is classic:

# Dynamic Programming

---

# Important DP Definition

Define:

```text id="t2m7v4"
dp[i]
```

as:

# minimum cost to stand on stair i

Very important definition.

---

# How Can We Reach Stair i?

Only two possibilities:

```text id="n8m4r6"
from i-1
from i-2
```

because movement allows:

```text id="r4m1k8"
1 or 2 jumps
```

---

# Recurrence Relation

To stand on stair `i`:

```cpp id="q7m5v2"
dp[i] =
cost[i]
+
min(dp[i-1], dp[i-2]);
```

---

# Why?

Suppose we want cheapest path to stair `i`.

We choose:

```text id="x1m8r4"
cheaper of previous two paths
```

then pay:

```text id="m9r2k5"
cost[i]
```

to step on current stair.

---

# Base Cases

Very important.

```cpp id="v5m7r1"
dp[0] = cost[0];
dp[1] = cost[1];
```

---

# Why?

Problem allows starting directly from:

```text id="f2m8v6"
0 or 1
```

So minimum cost to stand there is simply their own cost.

---

# Example

Suppose:

```text id="t4m1r9"
cost = [10,15,20]
```

Then:

```text id="q8m5r2"
dp[0] = 10
dp[1] = 15
```

---

# Compute dp[2]

Formula:

```cpp id="x6m1v7"
20 + min(10,15)
```

Result:

```text id="m3r8k1"
30
```

So:

```text id="p7m2v4"
dp[2] = 30
```

---

# VERY Important Final Insight

We do NOT need to stand exactly on last stair.

We need to reach:

# top

which is:

```text id="f4m9r2"
after last stair
```

---

# So Final Answer Is

```cpp id="q1m7k8"
min(dp[n-1], dp[n-2])
```

---

# Why?

Because top can be reached from:

```text id="v8m4r1"
last stair
OR
second last stair
```

Choose cheaper path.

---

# Visual Understanding

Suppose:

```text id="n5m2v7"
stairs = [0,1,2]
```

Top is position:

```text id="x2m8r5"
3
```

We can jump to top from:

```text id="m6r1k9"
1 or 2
```

So answer is:

```cpp id="t8m4v2"
min(dp[2], dp[1])
```

---

# Dry Run

## Input

```text id="q3m7r1"
cost = [10,15,20]
```

---

# Initial DP

```text id="v9m2k4"
dp[0] = 10
dp[1] = 15
```

---

# Compute dp[2]

```text id="f1m8r6"
20 + min(10,15)
= 20 + 10
= 30
```

Now:

```text id="p4m7v2"
dp = [10,15,30]
```

---

# Final Answer

```cpp id="x7m1r5"
min(30,15)
```

Result:

```text id="m2r9k8"
15
```

Correct.

---

# Complete DP Solution

```cpp id="q5m8v1"
class Solution {
public:

    int minCostClimbingStairs(vector<int>& cost) {

        int n = cost.size();

        vector<int> dp(n);

        // Base cases
        dp[0] = cost[0];
        dp[1] = cost[1];

        // Build DP
        for(int i = 2; i < n; i++){

            dp[i] =
            cost[i]
            +
            min(dp[i-1], dp[i-2]);
        }

        // Reach top
        return min(dp[n-1], dp[n-2]);
    }
};
```

---

# Complexity Analysis

## Time Complexity

Loop runs once through array.

```text id="t1m7r4"
O(n)
```

---

## Space Complexity

DP array stores `n` values.

```text id="f8m2k5"
O(n)
```

---

# Space Optimization

Do we really need entire DP array?

❌ No.

At every step we only need:

```text id="v4m9r1"
previous 2 values
```

So space can become:

```text id="n6m1v8"
O(1)
```

using variables.

---

# Optimized Idea

Store:

```text id="x5m2r7"
prev2 = dp[i-2]
prev1 = dp[i-1]
```

Then compute current.

---

# Optimized Code

```cpp id="k4m9v2"
class Solution {
public:

    int minCostClimbingStairs(vector<int>& cost) {

        int prev2 = cost[0];
        int prev1 = cost[1];

        for(int i = 2; i < cost.size(); i++){

            int current =
            cost[i]
            +
            min(prev1, prev2);

            prev2 = prev1;
            prev1 = current;
        }

        return min(prev1, prev2);
    }
};
```

---

# Why Optimization Works

Current state depends ONLY on:

```text id="w8m1r3"
previous two states
```

So full array unnecessary.

---

# Common Mistakes

## Mistake 1

Creating empty DP vector:

```cpp id="q6m2v7"
vector<int> dp;
```

Then accessing:

```cpp id="t8m1k5"
dp[0]
```

causes out-of-bounds error.

Correct:

```cpp id="f2m7r9"
vector<int> dp(n);
```

---

## Mistake 2

Returning:

```cpp id="m5r8k1"
dp[n]
```

Invalid index.

Last valid index:

```text id="x7m4v2"
n-1
```

---

## Mistake 3

Returning only:

```cpp id="q9m1r6"
dp[n-1]
```

Wrong because top may be reached cheaper from:

```text id="w4m8k2"
n-2
```

---

## Mistake 4

Misunderstanding DP definition.

Always define clearly:

```text id="p1m7r5"
What exactly does dp[i] represent?
```

Most DP mistakes happen here.

---

# Recognition Pattern

Whenever problem says:

* minimum cost
* minimum steps
* cheapest path

and current answer depends on smaller answers:

think:

# Dynamic Programming

---

# Important Mental Models

## DP

```text id="t6m2v8"
best current answer
depends on
best previous answers
```

---

## Recurrence

```text id="r8m5k4"
current state built from previous states
```

---

## Optimization

If only few previous states are needed:

```text id="x2m9r1"
replace DP array with variables
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="m1r8v6"
breaking problem into overlapping smaller subproblems
```

and building optimal answers gradually.
