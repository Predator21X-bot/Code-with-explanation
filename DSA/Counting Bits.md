# Counting Bits

## Problem Summary

For every number:

```text id="m1r8v4"
0 → n
```

we must calculate:

# number of set bits

in its binary representation.

---

# What Is A Set Bit?

Set bit means:

```text id="q7m2k5"
bit = 1
```

---

# Example

```text id="x4m9r1"
5
```

Binary:

```text id="t8m1k6"
101
```

Contains:

```text id="f5m7r2"
2 set bits
```

---

# Another Example

```text id="p2m8v4"
7
```

Binary:

```text id="v6m1r9"
111
```

Contains:

```text id="m4r8k1"
3 set bits
```

---

# Goal

Return array:

```text id="q9m2v7"
ans[i]
```

where:

```text id="x3m7r5"
ans[i] = number of set bits in i
```

---

# Brute Force Idea

For every number:

* convert to binary
* count ones

But repeated work occurs.

---

# MOST Important Observation

Suppose:

```text id="f8m1k2"
i = 13
```

Binary:

```text id="p5m9r1"
1101
```

If we remove the last bit:

```text id="t2m7v4"
110
```

which is:

```text id="n8m4r6"
6
```

Notice:

```text id="r4m1k8"
13 depends on smaller number 6
```

This is the key DP insight.

---

# Right Shift Operator

```text id="q7m5v2"
i >> 1
```

means:

# right shift by 1

Equivalent to:

i>>1=i/2

for integers.

---

# Example

```text id="x1m8r4"
13 = 1101
```

Right shift:

```text id="m9r2k5"
110
```

which equals:

```text id="v5m7r1"
6
```

---

# Checking Last Bit

Expression:

```text id="f2m8v6"
(i & 1)
```

checks:

# whether last bit is 1

---

# If Last Bit Is 0

Example:

```text id="t4m1r9"
10 = 1010
```

```text id="q8m5r2"
10 & 1 = 0
```

because last bit is 0.

---

# If Last Bit Is 1

Example:

```text id="x6m1v7"
11 = 1011
```

```text id="m3r8k1"
11 & 1 = 1
```

because last bit is 1.

---

# MOST Important Formula

bits[i]=bits[i>>1]+(i&1)

---

# Why Formula Works

Suppose:

```text id="p7m2v4"
13 = 1101
```

Set bits in:

```text id="f4m9r2"
1101
```

=

* set bits in `110`

-

* last bit `1`

So:

```text id="q1m7k8"
bits[13]=bits[6]+1
```

---

# Another Example

```text id="v8m4r1"
12 = 1100
```

Right shift:

```text id="n5m2v7"
110
```

Last bit:

```text id="x2m8r5"
0
```

So:

```text id="m6r1k9"
bits[12]=bits[6]+0
```

---

# Important DP Definition

Define:

```text id="t8m4v2"
dp[i]
```

as:

# number of set bits in i

---

# Base Case

```text id="q3m7r1"
dp[0] = 0
```

because:

```text id="v9m2k4"
0 has no set bits
```

---

# Build Remaining Answers

For:

```text id="f1m8r6"
1 → n
```

use recurrence:

dp[i]=dp[i>>1]+(i&1)

---

# Step-by-Step Dry Run

Suppose:

```text id="p4m7v2"
n = 5
```

---

# Initialize

```text id="x7m1r5"
dp[0] = 0
```

Array:

```text id="m2r9k8"
[0, _, _, _, _, _]
```

---

# i = 1

Binary:

```text id="q5m8v1"
1
```

Formula:

```text id="t1m7r4"
dp[1] = dp[0] + 1
```

```text id="f8m2k5"
= 1
```

Array:

```text id="v4m9r1"
[0,1,_,_,_,_]
```

---

# i = 2

Binary:

```text id="n6m1v8"
10
```

Formula:

```text id="x5m2r7"
dp[2] = dp[1] + 0
```

```text id="k4m9v2"
= 1
```

Array:

```text id="w8m1r3"
[0,1,1,_,_,_]
```

---

# i = 3

Binary:

```text id="q6m2v7"
11
```

Formula:

```text id="t8m1k5"
dp[3] = dp[1] + 1
```

```text id="f2m7r9"
= 2
```

Array:

```text id="m5r8k1"
[0,1,1,2,_,_]
```

---

# i = 4

Binary:

```text id="x7m4v2"
100
```

Formula:

```text id="q9m1r6"
dp[4] = dp[2] + 0
```

```text id="w4m8k2"
= 1
```

---

# Final Array

```text id="p1m7r5"
[0,1,1,2,1,2]
```

---

# Complete DP Solution

```cpp id="t6m2v8"
class Solution {
public:

    vector<int> countBits(int n) {

        vector<int> bits(n + 1, 0);

        for(int i = 1; i <= n; i++){

            bits[i] =
            bits[i >> 1]
            +
            (i & 1);
        }

        return bits;
    }
};
```

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text id="r8m5k4"
O(n)
```

---

## Space Complexity

DP array size:

```text id="x2m9r1"
O(n)
```

---

# Important Pattern Learned

This is:

# Bit Manipulation + Dynamic Programming

Very famous category.

---

# Similar Problems

* Number of 1 Bits
* Single Number
* Power of Two
* Reverse Bits
* Missing Number

---

# Common Mistakes

## Mistake 1

Confusing:

```text id="m7r1k5"
>>
```

with division.

Remember:

```text id="q2m8v4"
right shift removes last binary bit
```

---

## Mistake 2

Not understanding:

```text id="v7m4r1"
(i & 1)
```

It checks whether number is:

* odd → 1
* even → 0

---

## Mistake 3

Trying brute force binary conversion.

DP reuse is the key optimization.

---

# Recognition Pattern

Whenever problem involves:

* binary representation
* set bits
* smaller derived numbers

think:

# Bit DP

---

# Important Mental Models

## Remove Last Bit

```text id="f6m2k9"
i >> 1
```

removes final binary digit.

---

## Last Bit Contribution

```text id="t9m5r2"
(i & 1)
```

adds:

* 1 if odd
* 0 if even

---

## DP Reuse

Current answer built from:

```text id="x8m1v4"
already solved smaller number
```

---

# Biggest Takeaway

The entire problem comes from one insight:

```text id="m1v7k3"
binary number
=
smaller binary prefix
+
last bit
```

and DP efficiently reuses the smaller prefix answers.
