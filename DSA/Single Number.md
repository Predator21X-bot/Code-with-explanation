# Single Number

## Problem Summary

We are given an array:

```cpp id="m1r8v4"
nums
```

Every number appears:

```text id="q7m2k5"
twice
```

EXCEPT one number which appears:

```text id="x4m9r1"
once
```

We must find that single number.

---

# Example

```text id="t8m1k6"
nums = [2,2,1]
```

Answer:

```text id="f5m7r2"
1
```

because:

* 2 appears twice
* 1 appears once

---

# Brute Force Ideas

Possible approaches:

* hashmap frequency count
* sorting

But problem expects:

```text id="p2m8v4"
O(1) extra space
```

---

# MOST Important Insight

Use:

# XOR operator

```text id="v6m1r9"
^
```

---

# XOR Works On Binary

XOR compares bits one-by-one.

---

# XOR Truth Table

```text id="m4r8k1"
0 ^ 0 = 0
1 ^ 1 = 0
0 ^ 1 = 1
1 ^ 0 = 1
```

---

# MOST Important XOR Rule

```text id="q9m2v7"
same bits → 0
different bits → 1
```

---

# Important XOR Properties

---

# Property 1

a\oplus a=0

Same numbers cancel out.

---

# Property 2

a\oplus0=a

XOR with zero changes nothing.

---

# Property 3 — Commutative

a\oplus b=b\oplus a

Order does not matter.

---

# Property 4 — Associative

(a\oplus b)\oplus c=a\oplus(b\oplus c)

Grouping does not matter.

---

# Understanding XOR Step-by-Step

---

# Example 1 — `4 ^ 1`

---

# Convert To Binary

```text id="x3m7r5"
4 = 100
1 = 001
```

---

# Apply XOR Bit-by-Bit

```text id="f8m1k2"
100
001
---
101
```

---

# Why?

## First Bit

```text id="p5m9r1"
1 ^ 0 = 1
```

Different bits.

---

# Second Bit

```text id="t2m7v4"
0 ^ 0 = 0
```

Same bits.

---

# Third Bit

```text id="n8m4r6"
0 ^ 1 = 1
```

Different bits.

---

# Final Result

```text id="r4m1k8"
101
```

Binary `101` equals:

```text id="q7m5v2"
5
```

So:

```text id="x1m8r4"
4 ^ 1 = 5
```

---

# Example 2 — `5 ^ 2`

---

# Convert To Binary

```text id="m9r2k5"
5 = 101
2 = 010
```

---

# XOR

```text id="v5m7r1"
101
010
---
111
```

Binary `111` equals:

```text id="f2m8v6"
7
```

So:

```text id="t4m1r9"
5 ^ 2 = 7
```

---

# MOST Important Property — Duplicate Cancellation

Example:

```text id="q8m5r2"
2 ^ 2
```

Binary:

```text id="x6m1v7"
10
10
--
00
```

which equals:

```text id="m3r8k1"
0
```

Therefore:

a\oplus a=0

always.

---

# Entire Problem Logic

Suppose array:

```text id="p7m2v4"
[4,1,2,1,2]
```

XOR all numbers:

```text id="f4m9r2"
4 ^ 1 ^ 2 ^ 1 ^ 2
```

Rearrange using commutative property:

```text id="q1m7k8"
4 ^ (1 ^ 1) ^ (2 ^ 2)
```

Duplicates cancel:

```text id="v8m4r1"
4 ^ 0 ^ 0
```

Final result:

```text id="n5m2v7"
4
```

---

# Algorithm

Initialize:

```cpp id="x2m8r5"
ans = 0
```

Then XOR every number.

---

# Why Initialize With 0?

Because:

a\oplus0=a

---

# Dry Run

Suppose:

```text id="m6r1k9"
nums = [4,1,2,1,2]
```

---

# Start

```text id="t8m4v2"
ans = 0
```

---

# XOR 4

```text id="q3m7r1"
0 ^ 4 = 4
```

---

# XOR 1

```text id="v9m2k4"
4 ^ 1 = 5
```

---

# XOR 2

```text id="f1m8r6"
5 ^ 2 = 7
```

---

# XOR 1

```text id="p4m7v2"
7 ^ 1 = 6
```

---

# XOR 2

```text id="x7m1r5"
6 ^ 2 = 4
```

Final answer:

```text id="m2r9k8"
4
```

---

# Complete Optimal Solution

```cpp id="q5m8v1"
class Solution {
public:

    int singleNumber(vector<int>& nums) {

        int ans = 0;

        for(int num : nums){

            ans ^= num;
        }

        return ans;
    }
};
```

---

# Complexity Analysis

## Time Complexity

Single traversal:

```text id="t1m7r4"
O(n)
```

---

## Space Complexity

Only one variable:

```text id="f8m2k5"
O(1)
```

---

# Important Pattern Learned

This is classic:

# XOR Cancellation Pattern

Very important interview trick.

---

# Similar Problems

* Missing Number
* Single Number II
* Two Single Numbers
* XOR Queries
* Find Missing Value

---

# Recognition Pattern

Whenever problem says:

* every number appears twice
* except one

think:

# XOR immediately

---

# Important Mental Models

## Same Bits Cancel

```text id="v4m9r1"
1 ^ 1 = 0
0 ^ 0 = 0
```

---

# Different Bits Survive

```text id="n6m1v8"
1 ^ 0 = 1
0 ^ 1 = 1
```

---

# XOR Is Bitwise

Always think in:

```text id="x5m2r7"
binary representation
```

NOT decimal directly.

---

# Duplicate Cancellation

Repeated XOR naturally removes duplicate numbers.

---

# Biggest Takeaway

The entire problem works because:

```text id="k4m9v2"
duplicate numbers cancel each other under XOR
```

leaving only the unique number behind.
