# Minimum Flips to Make a OR b Equal to c

## Problem Summary

We are given three integers:

```cpp id="m1r8v4"
a
b
c
```

We may flip bits in:

```text id="q7m2k5"
a or b
```

Goal:

Make:

(a\ |\ b)=c

using:

# minimum flips

---

# What Is A Flip?

Flip means:

```text id="x4m9r1"
0 → 1
OR
1 → 0
```

---

# MOST Important Insight

Bit operations work:

# independently at every bit position

So we solve:

```text id="t8m1k6"
one bit at a time
```

---

# OR Truth Table

Very important.

```text id="f5m7r2"
0 | 0 = 0
0 | 1 = 1
1 | 0 = 1
1 | 1 = 1
```

---

# Key Observation

OR becomes:

```text id="p2m8v4"
0
```

ONLY when:

```text id="v6m1r9"
both bits are 0
```

Otherwise OR becomes 1.

---

# Strategy

For every bit position:

* extract bit from `a`
* extract bit from `b`
* extract bit from `c`

Then determine:

```text id="m4r8k1"
how many flips needed
```

---

# Extracting Last Bit

Use:

```cpp id="q9m2v7"
a & 1
```

Similarly:

```cpp id="x3m7r5"
b & 1
c & 1
```

---

# Why `& 1` Works

It extracts:

# rightmost binary bit

---

# Example

```text id="f8m1k2"
13 = 1101
```

```text id="p5m9r1"
13 & 1 = 1
```

because last bit is 1.

---

# Another Example

```text id="t2m7v4"
12 = 1100
```

```text id="n8m4r6"
12 & 1 = 0
```

because last bit is 0.

---

# Moving To Next Bit

Use:

```cpp id="r4m1k8"
a >>= 1;
```

Same for:

```text id="q7m5v2"
b
c
```

---

# Why Right Shift?

Removes already processed last bit.

Equivalent to:

a>>1=a/2

for integers.

---

# MOST Important Cases

---

# Case 1 — `cBit = 0`

We need:

aBit\ |\ bBit=0

When does OR become 0?

ONLY when:

```text id="x1m8r4"
both bits are 0
```

---

# Subcase 1

```text id="m9r2k5"
aBit = 0
bBit = 0
```

Already correct.

Flips:

```text id="v5m7r1"
0
```

---

# Subcase 2

```text id="f2m8v6"
aBit = 1
bBit = 0
```

Need:

```text id="t4m1r9"
flip aBit
```

Flips:

```text id="q8m5r2"
1
```

---

# Subcase 3

```text id="x6m1v7"
aBit = 0
bBit = 1
```

Need one flip.

---

# Subcase 4

```text id="m3r8k1"
aBit = 1
bBit = 1
```

Need BOTH become 0.

Flips:

```text id="p7m2v4"
2
```

---

# Important Formula

When:

```text id="f4m9r2"
cBit == 0
```

flips needed:

flips=aBit+bBit

---

# Why?

Bits are only:

```text id="q1m7k8"
0 or 1
```

Each `1` bit must flip to 0.

So:

* `0 + 0 = 0`
* `1 + 0 = 1`
* `0 + 1 = 1`
* `1 + 1 = 2`

Exactly matches required flips.

---

# Case 2 — `cBit = 1`

We need:

aBit\ |\ bBit=1

OR becomes 1 if:

```text id="v8m4r1"
at least one bit is 1
```

---

# Subcase 1

```text id="n5m2v7"
aBit = 1
bBit = 0
```

Already correct.

---

# Subcase 2

```text id="x2m8r5"
aBit = 0
bBit = 1
```

Already correct.

---

# Subcase 3

```text id="m6r1k9"
aBit = 1
bBit = 1
```

Already correct.

---

# Subcase 4

```text id="t8m4v2"
aBit = 0
bBit = 0
```

Problem.

Need ONE flip.

---

# Condition

```cpp id="q3m7r1"
if(aBit == 0 && bBit == 0)
```

then:

```cpp id="v9m2k4"
flips++;
```

---

# Complete Algorithm

Process bits until all numbers become 0.

At every iteration:

1. Extract bits
2. Count flips
3. Right shift numbers

---

# Complete Optimal Solution

```cpp id="f1m8r6"
class Solution {
public:

    int minFlips(int a, int b, int c) {

        int flips = 0;

        while(a || b || c){

            int aBit = a & 1;
            int bBit = b & 1;
            int cBit = c & 1;

            // Need OR result = 0
            if(cBit == 0){

                flips += aBit + bBit;
            }

            // Need OR result = 1
            else{

                if(aBit == 0 && bBit == 0){

                    flips++;
                }
            }

            // Move to next bit
            a >>= 1;
            b >>= 1;
            c >>= 1;
        }

        return flips;
    }
};
```

---

# Complexity Analysis

## Time Complexity

At most 32 bits processed.

```text id="p4m7v2"
O(32)
```

which is effectively:

```text id="x7m1r5"
O(1)
```

---

# Space Complexity

Only variables used.

```text id="m2r9k8"
O(1)
```

---

# Important Pattern Learned

This is:

# Per-Bit Greedy Logic

Very important bit manipulation category.

---

# Similar Problems

* Single Number
* Number of 1 Bits
* Counting Bits
* Missing Number
* Reverse Bits

---

# Common Mistakes

## Mistake 1

Trying to solve using whole numbers instead of:

```text id="q5m8v1"
individual bits
```

---

## Mistake 2

Forgetting OR truth table.

Remember:

```text id="t1m7r4"
OR becomes 0 ONLY when both bits are 0
```

---

## Mistake 3

Not shifting numbers after processing bit.

Need:

```cpp id="f8m2k5"
a >>= 1;
b >>= 1;
c >>= 1;
```

---

# Recognition Pattern

Whenever problem involves:

* AND
* OR
* XOR
* flips
* binary conditions

think:

# solve independently per bit

---

# Important Mental Models

## Bit Independence

Each bit position works independently.

---

## Right Shift

```text id="v4m9r1"
>> 1
```

moves to next bit.

---

## Last Bit Extraction

```text id="n6m1v8"
& 1
```

extracts rightmost bit.

---

# Biggest Takeaway

Bit manipulation problems become much easier when you think:

```text id="x5m2r7"
one bit at a time
```

instead of entire numbers together.
