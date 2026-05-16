# N-th Tribonacci Number

## Problem Summary

The Tribonacci sequence is defined as:

```text id="m1r8v4"
T0 = 0
T1 = 1
T2 = 1
```

For:

```text id="q7m2k5"
n >= 3
```

the recurrence relation is:

```text id="x4m9r1"
Tn = T(n-1) + T(n-2) + T(n-3)
```

We must return:

```text id="t8m1k6"
Tn
```

(the nth Tribonacci number).

---

# Example

## Find T3

Using formula:

```text id="f5m7r2"
T3 = T2 + T1 + T0
```

Substitute values:

```text id="p2m8v4"
1 + 1 + 0 = 2
```

So:

```text id="v6m1r9"
T3 = 2
```

---

# Find T4

```text id="m4r8k1"
T4 = T3 + T2 + T1
```

Substitute:

```text id="q9m2v7"
2 + 1 + 1 = 4
```

---

# Sequence Looks Like

```text id="x3m7r5"
0 1 1 2 4 7 13 ...
```

---

# Key Observation

Every number depends on:

```text id="f8m1k2"
previous 3 numbers
```

This is a classic:

# Recurrence Relation

---

# First Thought — Recursion

We could write:

```cpp id="p5m9r1"
tribonacci(n-1)
+
tribonacci(n-2)
+
tribonacci(n-3)
```

But recursion becomes VERY slow.

---

# Why Recursive Solution Is Slow

Because same values get recomputed repeatedly.

Example:

```text id="t2m7v4"
T5 needs T4
T4 needs T3
T5 also directly needs T3
```

So:

```text id="n8m4r6"
T3 gets recalculated many times
```

Huge repetition.

---

# This Is Where DP Comes In

Dynamic Programming means:

```text id="r4m1k8"
store already solved subproblems
```

so we never recompute them.

---

# Core DP Idea

Build solution:

```text id="q7m5v2"
from small values → larger values
```

---

# Base Cases

Very important.

```cpp id="x1m8r4"
if(n == 0)
    return 0;

if(n == 1)
    return 1;

if(n == 2)
    return 1;
```

These represent:

```text id="m9r2k5"
T0, T1, T2
```

---

# DP Array Approach (Basic DP)

We could create:

```cpp id="v5m7r1"
vector<int> dp(n + 1);
```

where:

```text id="f2m8v6"
dp[i] = ith tribonacci number
```

---

# Initialize Base Cases

```cpp id="t4m1r9"
dp[0] = 0;
dp[1] = 1;
dp[2] = 1;
```

---

# Transition Formula

For:

```text id="q8m5r2"
i = 3 → n
```

compute:

```cpp id="x6m1v7"
dp[i] =
dp[i-1]
+
dp[i-2]
+
dp[i-3];
```

---

# Example

Suppose:

```text id="m3r8k1"
n = 5
```

Initial:

```text id="p7m2v4"
dp = [0,1,1,_,_,_]
```

---

# i = 3

```text id="f4m9r2"
1 + 1 + 0 = 2
```

```text id="q1m7k8"
dp = [0,1,1,2,_,_]
```

---

# i = 4

```text id="v8m4r1"
2 + 1 + 1 = 4
```

```text id="n5m2v7"
dp = [0,1,1,2,4,_]
```

---

# i = 5

```text id="x2m8r5"
4 + 2 + 1 = 7
```

Final:

```text id="m6r1k9"
dp = [0,1,1,2,4,7]
```

Answer:

```text id="t8m4v2"
7
```

---

# Optimization Insight

Do we really need entire array?

❌ No.

At any point we only need:

```text id="q3m7r1"
last 3 values
```

So we can optimize space.

---

# Optimized Variables

Store:

```text id="v9m2k4"
a = T0
b = T1
c = T2
```

Meaning:

```cpp id="f1m8r6"
int a = 0;
int b = 1;
int c = 1;
```

---

# Transition

Next tribonacci number:

```cpp id="p4m7v2"
int next = a + b + c;
```

---

# Shift Window Forward

```cpp id="x7m1r5"
a = b;
b = c;
c = next;
```

---

# Why Shifting Works

Before shift:

```text id="m2r9k8"
a = T0
b = T1
c = T2
```

After shift:

```text id="q5m8v1"
a = T1
b = T2
c = T3
```

Window moves forward correctly.

---

# Loop

We must compute:

```text id="t1m7r4"
T3 → Tn
```

So:

```cpp id="f8m2k5"
for(int i = 3; i <= n; i++)
```

---

# Final Answer

At end:

```text id="v4m9r1"
c
```

contains latest tribonacci number.

So:

```cpp id="n6m1v8"
return c;
```

---

# Final Optimized Code

```cpp id="x5m2r7"
class Solution {
public:

    int tribonacci(int n) {

        // Base cases
        if(n == 0)
            return 0;

        if(n == 1)
            return 1;

        if(n == 2)
            return 1;

        // Initial tribonacci values
        int a = 0;
        int b = 1;
        int c = 1;

        // Build sequence iteratively
        for(int i = 3; i <= n; i++){

            int next = a + b + c;

            a = b;
            b = c;
            c = next;
        }

        return c;
    }
};
```

---

# Complexity Analysis

## Time Complexity

Loop runs:

```text id="k4m9v2"
n times
```

So:

```text id="w8m1r3"
O(n)
```

---

## Space Complexity

Only variables used.

```text id="q6m2v7"
O(1)
```

Optimal solution.

---

# Common Mistakes

## Mistake 1

Forgetting base cases.

Without them:

```text id="t8m1k5"
loop initialization breaks
```

---

## Mistake 2

Starting loop from wrong index.

Need:

```cpp id="f2m7r9"
i = 3
```

because:

```text id="m5r8k1"
T0, T1, T2 already known
```

---

## Mistake 3

Returning wrong variable.

Final answer is stored in:

```text id="x7m4v2"
c
```

---

## Mistake 4

Using recursion without memoization.

Causes huge repeated calculations.

---

# Recognition Pattern

Whenever:

```text id="q9m1r6"
current state depends on few previous states
```

think:

# Dynamic Programming

---

# Important Mental Models

## DP

```text id="w4m8k2"
solve smaller problems first
use them to build bigger answers
```

---

## Rolling Window DP

Only previous fixed states needed.

So variables can replace DP array.

---

## State Transition

```text id="p1m7r5"
next = previous states combination
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="t6m2v8"
reusing already computed results
```

instead of recomputing recursively again and again.
