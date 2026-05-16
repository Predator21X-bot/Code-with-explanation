# Domino and Tromino Tiling

## Problem Summary

We are given a:

```text id="m1r8v4"
2 x n board
```

We must count:

# number of ways

to completely tile the board using:

* Dominoes
* Trominoes

---

# Allowed Tiles

## 1. Domino

Can be placed:

### Vertically

```text id="q7m2k5"
⬜
⬜
```

---

### Horizontally

```text id="x4m9r1"
⬜⬜
```

---

# 2. Tromino (L Shape)

Example shape:

```text id="t8m1k6"
⬜⬜
⬜
```

Rotations are also allowed.

---

# Goal

Completely fill:

```text id="f5m7r2"
2 x n board
```

without:

* overlaps
* empty cells

---

# Example 1

## n = 1

Board:

```text id="p2m8v4"
⬜
⬜
```

Only one way:

```text id="v6m1r9"
one vertical domino
```

Answer:

```text id="m4r8k1"
1
```

---

# Example 2

## n = 2

Board:

```text id="q9m2v7"
⬜⬜
⬜⬜
```

Ways:

## Way 1

Two vertical dominoes.

---

## Way 2

Two horizontal dominoes.

Answer:

```text id="x3m7r5"
2
```

---

# Example 3

## n = 3

More arrangements become possible because of:

```text id="f8m1k2"
L trominoes
```

Answer:

```text id="p5m9r1"
5
```

---

# Key Observation

This is a:

# Counting DP Problem

We are counting:

```text id="t2m7v4"
number of valid arrangements
```

NOT:

* minimum
* maximum
* shortest path

---

# Core DP Thinking

Suppose we already know:

```text id="n8m4r6"
ways to tile smaller boards
```

Can we build bigger board from them?

YES.

That is Dynamic Programming.

---

# Important DP Definition

Define:

```text id="r4m1k8"
dp[i]
```

as:

# number of ways to tile a 2 x i board

Very important definition.

---

# Famous Recurrence

The recurrence for this problem is:

f(n)=2f(n-1)+f(n-3)

---

# Understanding The Recurrence

We build a:

```text id="q7m5v2"
2 x n board
```

from smaller boards.

---

# Case 1 — Add Vertical Domino

Last column filled using:

```text id="x1m8r4"
one vertical domino
```

Then remaining board becomes:

```text id="m9r2k5"
2 x (n-1)
```

Ways contributed:

```text id="v5m7r1"
f(n-1)
```

---

# Case 2 — Add Horizontal Dominoes

Last two columns filled using:

```text id="f2m8v6"
two horizontal dominoes
```

Then remaining board becomes:

```text id="t4m1r9"
2 x (n-2)
```

---

# Case 3 — Tromino Arrangements

L trominoes create:

```text id="q8m5r2"
special incomplete shapes
```

which eventually simplify mathematically into:

f(n)=2f(n-1)+f(n-3)

after recurrence derivation.

---

# Important Interview Insight

Usually interviewers care more about:

* recognizing recurrence
* implementing DP correctly

than deriving recurrence mathematically from scratch.

---

# Base Cases

Very important.

## n = 1

```cpp id="x6m1v7"
dp[1] = 1;
```

---

## n = 2

```cpp id="m3r8k1"
dp[2] = 2;
```

---

## n = 3

```cpp id="p7m2v4"
dp[3] = 5;
```

---

# Why Base Cases Matter

All future DP values depend on smaller states.

Incorrect base cases break entire recurrence.

---

# Transition Formula

For:

```text id="f4m9r2"
i >= 4
```

use:

dp[i]=(2dp[i-1]+dp[i-3])\bmod(10^9+7)

---

# Why Modulo?

Answers grow extremely large.

Problem requires:

```text id="q1m7k8"
mod 1e9+7
```

to avoid overflow.

---

# Important Modulo Rule

Correct usage:

```cpp id="v8m4r1"
value % MOD
```

NOT:

```text id="n5m2v7"
value * MOD
```

---

# Dry Run

Suppose:

```text id="x2m8r5"
n = 5
```

---

# Initial

```text id="m6r1k9"
dp[1] = 1
dp[2] = 2
dp[3] = 5
```

---

# Compute dp[4]

dp[4]=2\cdot5+1=11

---

# Compute dp[5]

dp[5]=2\cdot11+2=24

Answer:

```text id="t8m4v2"
24
```

---

# Complete DP Solution

```cpp id="q3m7r1"
class Solution {
public:

    int numTilings(int n) {

        // Base cases
        if(n == 1)
            return 1;

        if(n == 2)
            return 2;

        if(n == 3)
            return 5;

        const int MOD = 1e9 + 7;

        vector<long long> dp(n + 1);

        dp[1] = 1;
        dp[2] = 2;
        dp[3] = 5;

        // Build DP
        for(int i = 4; i <= n; i++){

            dp[i] =
            (2 * dp[i - 1] + dp[i - 3]) % MOD;
        }

        return dp[n];
    }
};
```

---

# Complexity Analysis

## Time Complexity

Single loop from:

```text id="v9m2k4"
4 → n
```

So:

```text id="f1m8r6"
O(n)
```

---

## Space Complexity

DP array stores:

```text id="p4m7v2"
n values
```

So:

```text id="x7m1r5"
O(n)
```

---

# Space Optimization

Observe recurrence:

dp[i]\text{ depends only on }dp[i-1]\text{ and }dp[i-3]

So full array unnecessary.

Can optimize to:

```text id="m2r9k8"
O(1)
```

using rolling variables.

---

# Important Pattern Learned

This is:

# Counting Arrangements DP

Different from:

* minimum cost DP
* maximum profit DP

Here we COUNT possibilities.

---

# Common Mistakes

## Mistake 1

Using:

```cpp id="q5m8v1"
* MOD
```

instead of:

```cpp id="t1m7r4"
% MOD
```

Modulo reduces numbers, not multiplies them.

---

## Mistake 2

Forgetting base cases.

Recurrence depends on them.

---

## Mistake 3

Using wrong recurrence.

The correct relation is:

f(n)=2f(n-1)+f(n-3)

---

## Mistake 4

Using `int` DP array.

Intermediate values may overflow.

Safer to use:

```cpp id="f8m2k5"
long long
```

---

# Recognition Pattern

Whenever problem asks:

* number of ways
* count arrangements
* count valid structures

think:

# Counting DP

---

# Important Mental Models

## DP

```text id="v4m9r1"
build larger solutions
using smaller solved states
```

---

## Recurrence

```text id="n6m1v8"
current state depends on previous states
```

---

## Counting DP

```text id="x5m2r7"
total ways =
sum/combinations of previous ways
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="k4m9v2"
recognizing repeated substructure
and building solutions incrementally
```

using recurrence relations.
