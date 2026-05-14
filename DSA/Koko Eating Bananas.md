# Koko Eating Bananas

## Problem Summary

We are given:

```cpp id="m1v8r4"
piles[]
```

Each value represents:

```text id="q7m2k5"
number of bananas in a pile
```

Koko eats bananas at speed:

```text id="x4m9r1"
k bananas/hour
```

Meaning:

* every hour she chooses ONE pile
* eats at most `k` bananas from that pile

If pile contains fewer than `k` bananas:

```text id="t8m1k6"
she eats entire pile
```

---

# Goal

Find:

# minimum eating speed `k`

such that Koko finishes ALL bananas within:

```text id="f5m7r2"
h hours
```

---

# Example

## Input

```text id="p2m8v4"
piles = [3,6,7,11]
h = 8
```

---

# Suppose Speed = 4

Hours needed:

| Pile | Hours |
| ---- | ----- |
| 3    | 1     |
| 6    | 2     |
| 7    | 2     |
| 11   | 3     |

Total:

```text id="v6m1r9"
1 + 2 + 2 + 3 = 8
```

Works.

---

# Suppose Speed = 3

| Pile | Hours |
| ---- | ----- |
| 3    | 1     |
| 6    | 2     |
| 7    | 3     |
| 11   | 4     |

Total:

```text id="m4r8k1"
10
```

Too slow.

---

# Key Observation

As eating speed increases:

```text id="q9m2v7"
required hours decrease
```

Very important monotonic behavior.

---

# Visual Pattern

Possible speeds look like:

```text id="x3m7r5"
speed too slow ❌
speed too slow ❌
speed works ✅
speed works ✅
```

Pattern becomes:

```text id="f8m1k2"
❌ ❌ ❌ ✅ ✅ ✅
```

This is EXACTLY:

# Binary Search Boundary Pattern

---

# Big Insight

We are NOT binary searching array indices.

We are searching:

# Answer Space

Possible eating speeds.

---

# Search Space

## Minimum Speed

```text id="p5m9r1"
1
```

Koko must eat at least 1 banana/hour.

---

## Maximum Speed

```text id="t2m7v4"
max(piles)
```

Why?

If Koko eats at speed equal to largest pile:

```text id="n8m4r6"
every pile finishes within 1 hour
```

No need for larger speed.

---

# Binary Search Range

```cpp id="r4m1k8"
left = 1
right = maxPile
```

---

# Core Binary Search Question

For some speed:

```text id="q7m5v2"
Can Koko finish within h hours?
```

This is called:

# Feasibility Check

Very important interview pattern.

---

# Calculating Hours

Suppose:

```text id="x1m8r4"
pile = 7
speed = 3
```

Hours needed:

```text id="m9r2k5"
ceil(7 / 3) = 3
```

because:

* hour 1 → eat 3
* hour 2 → eat 3
* hour 3 → eat remaining 1

---

# Ceiling Division Trick

Instead of floating point:

```cpp id="v5m7r1"
(pile + speed - 1) / speed
```

This computes:

# ceiling division

efficiently using integers.

---

# Example

```text id="f2m8v6"
(7 + 3 - 1) / 3
= 9 / 3
= 3
```

Correct.

---

# Complete Algorithm

## Step 1

Set binary search range:

```cpp id="t4m1r9"
left = 1
right = maxPile
```

---

## Step 2

Binary search on eating speed.

---

## Step 3

For each candidate speed:

calculate total hours required.

---

## Step 4

If total hours <= h:

```text id="q8m5r2"
current speed works
```

Try smaller speed.

Move:

```cpp id="x6m1v7"
right = mid - 1
```

---

## Step 5

If total hours > h:

```text id="m3r8k1"
current speed too slow
```

Need larger speed.

Move:

```cpp id="p7m2v4"
left = mid + 1
```

---

# Why `left` Is Final Answer

At end of binary search:

```text id="f4m9r2"
left lands on first valid speed
```

because search pattern is:

```text id="q1m7k8"
❌ ❌ ❌ ✅ ✅ ✅
```

We want:

```text id="v8m4r1"
minimum valid speed
```

---

# Dry Run

## Input

```text id="n5m2v7"
piles = [3,6,7,11]
h = 8
```

---

# Initial Search Space

```text id="x2m8r5"
1 → 11
```

---

# Iteration 1

```text id="m6r1k9"
mid = 6
```

Hours:

```text id="t8m4v2"
1 + 1 + 2 + 2 = 6
```

Works.

Try smaller speed.

Move:

```text id="q3m7r1"
right = 5
```

---

# Iteration 2

```text id="v9m2k4"
mid = 3
```

Hours:

```text id="f1m8r6"
1 + 2 + 3 + 4 = 10
```

Too slow.

Move:

```text id="p4m7v2"
left = 4
```

---

# Iteration 3

```text id="x7m1r5"
mid = 4
```

Hours:

```text id="m2r9k8"
1 + 2 + 2 + 3 = 8
```

Works.

Try smaller.

Eventually:

```text id="q5m8v1"
left = 4
```

Final answer:

```text id="t1m7r4"
4
```

---

# Final Code

```cpp id="f8m2k5"
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {

        int left = 1;
        int right = *max_element(piles.begin(), piles.end());

        while(left <= right){

            int mid = left + (right - left) / 2;

            long long hours = 0;

            for(int pile : piles){
                hours += (pile + mid - 1) / mid;
            }

            if(hours <= h){
                right = mid - 1;
            }
            else{
                left = mid + 1;
            }
        }

        return left;
    }
};
```

---

# Complexity Analysis

## Binary Search Iterations

Search range:

```text id="v4m9r1"
1 → maxPile
```

Iterations:

```text id="n6m1v8"
O(log maxPile)
```

---

## Feasibility Check

Each check traverses all piles:

```text id="x5m2r7"
O(n)
```

---

# Final Complexity

## Time

```text id="k4m9v2"
O(n log maxPile)
```

---

## Space

Only variables used.

```text id="w8m1r3"
O(1)
```

---

# Common Mistakes

## Mistake 1

Using:

```cpp id="q6m2v7"
right = piles.size() - 1
```

Wrong because:

```text id="t8m1k5"
we are searching speed
NOT indices
```

---

## Mistake 2

Using:

```cpp id="f2m7r9"
left = 0
```

Can cause:

```text id="m5r8k1"
division by zero
```

Minimum valid speed is:

```text id="x7m4v2"
1
```

---

## Mistake 3

Placing binary search decision inside pile loop.

Wrong because:

```text id="q9m1r6"
must first calculate TOTAL hours
```

before deciding.

---

## Mistake 4

Using floating point `ceil`.

Integer formula is cleaner:

```cpp id="w4m8k2"
(pile + speed - 1) / speed
```

---

# Recognition Pattern

This is classic:

# Binary Search on Answer

Pattern:

```text id="p1m7r5"
minimum valid value
```

---

# Important Mental Models

## Binary Search

```text id="t6m2v8"
Search feasible vs infeasible
```

---

## Monotonicity

```text id="r8m5k4"
speed increases → required hours decrease
```

---

## Pattern Shape

```text id="x2m9r1"
❌ ❌ ❌ ✅ ✅ ✅
```

First `✅` is answer.

---

# Biggest Takeaway

Binary Search is fundamentally about:

```text id="m7r4v5"
monotonic YES/NO decisions
```

not arrays specifically.

This problem is one of the most important examples of that concept.
