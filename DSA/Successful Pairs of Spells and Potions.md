# Successful Pairs of Spells and Potions

## Problem Summary

We are given:

```cpp id="m7a2q9"
spells[]
potions[]
success
```

A pair is considered successful if:

```text id="q2v8n4"
spell * potion >= success
```

For EACH spell:

```text id="r5x1m8"
count how many potions create successful pairs
```

Return answer array where:

```text id="f8k3w2"
answer[i]
```

represents successful potion count for:

```text id="p4n7r1"
spells[i]
```

---

# Example

## Input

```text id="v1m8k4"
spells  = [4,5]
potions = [1,2,3,4]
success = 16
```

---

# For spell = 4

Condition:

```text id="m9r2k5"
4 * potion >= 16
```

Meaning:

```text id="q7m1v8"
potion >= 4
```

Only:

```text id="x5m4r2"
4
```

works.

Count:

```text id="f2m8k9"
1
```

---

# For spell = 5

Condition:

```text id="r6m1v4"
5 * potion >= 16
```

Check potions:

```text id="t3m8r7"
5*1 = 5
5*2 = 10
5*3 = 15
5*4 = 20
```

Only:

```text id="p9m2k6"
4
```

works.

Count:

```text id="w4m7r1"
1
```

---

# Brute Force Approach

For every spell:

check every potion.

Complexity:

```text id="k2m8v5"
O(n * m)
```

Too slow for large constraints.

---

# Key Observation

Suppose potions are sorted:

```text id="n7m4r2"
[1,2,3,4,5,6]
```

For some spell:

successful pattern becomes:

```text id="v8m1k7"
❌ ❌ ❌ ✅ ✅ ✅
```

VERY important.

---

# Why?

Because once condition becomes true:

```text id="x3m9r4"
all larger potions also work
```

This creates:

# Monotonic Behavior

which strongly suggests:

# Binary Search

---

# Core Insight

We are NOT searching for:

```text id="p6m2k8"
exact value
```

We are searching for:

# First Valid Potion

Meaning first index where:

```text id="t5m8r1"
spell * potion >= success
```

---

# Why First Valid Position?

Because then:

```text id="f4m7v2"
everything to the right also works
```

So answer becomes:

```cpp id="m1r8k5"
total_potions - first_valid_index
```

Elegant trick.

---

# Sorting Step

We sort only:

```cpp id="v7m2r4"
potions
```

NOT spells.

Why?

Because output must match:

```text id="q8m5k1"
original spell order
```

---

# Important Binary Search Pattern

This is classic:

```text id="r2m9v6"
Find First True
```

pattern.

---

# Visual Representation

Suppose:

```text id="w1m7k3"
❌ ❌ ❌ ✅ ✅ ✅
```

Binary search finds boundary between:

```text id="x4m8r2"
invalid | valid
```

The first `✅` is our answer.

---

# Complete Algorithm

## Step 1

Sort potions:

```cpp id="n6m2v8"
sort(potions.begin(), potions.end());
```

---

## Step 2

For every spell:

run binary search on potions.

---

## Step 3

Find first potion satisfying:

```cpp id="m9r4k7"
1LL * spell * potions[mid] >= success
```

---

## Step 4

If condition is TRUE:

```text id="p7m1v5"
mid might be answer
BUT earlier valid may exist
```

So move LEFT:

```cpp id="t2m8r4"
right = mid - 1;
```

---

## Step 5

If condition is FALSE:

```text id="f5m7k2"
need larger potion
```

Move RIGHT:

```cpp id="q1m9v8"
left = mid + 1;
```

---

## Step 6

After binary search:

```text id="r4m2k7"
left = first valid index
```

Successful count:

```cpp id="x8m5r1"
potions.size() - left
```

---

# Why `left` Works

At end of boundary search:

```text id="v3m7k4"
left always lands on first valid position
```

because binary search eliminates all invalid positions.

---

# Important Overflow Detail

Use:

```cpp id="m1r8v6"
1LL * spell * potion
```

Why?

Because:

```text id="k9m4r2"
100000 * 100000
```

can overflow normal int.

Using:

```text id="q6m2v7"
long long
```

prevents overflow.

---

# Dry Run

## Input

```text id="t8m1k5"
spells  = [5]
potions = [1,2,3,4,5]
success = 16
```

---

# Sorted Potions

```text id="f2m7r9"
[1,2,3,4,5]
```

---

# Binary Search

## Iteration 1

```text id="m5r8k1"
left = 0
right = 4
mid = 2
```

Check:

```text id="x7m4v2"
5 * 3 = 15
```

Too small.

Move:

```text id="q9m1r6"
left = 3
```

---

## Iteration 2

```text id="w4m8k2"
left = 3
right = 4
mid = 3
```

Check:

```text id="p1m7r5"
5 * 4 = 20
```

Valid.

Move:

```text id="t6m2v8"
right = 2
```

Loop ends.

---

# Final Position

```text id="r8m5k4"
left = 3
```

Count:

```text id="x2m9r1"
5 - 3 = 2
```

Valid potions:

```text id="m7r4v5"
4,5
```

Correct.

---

# Final Code

```cpp id="v9m2k7"
class Solution {
public:
    vector<int> successfulPairs(
        vector<int>& spells,
        vector<int>& potions,
        long long success
    ) {

        vector<int> ans;

        sort(potions.begin(), potions.end());

        for(int spell : spells){

            int left = 0;
            int right = potions.size() - 1;

            while(left <= right){

                int mid = left + (right - left) / 2;

                if(1LL * spell * potions[mid] >= success){
                    right = mid - 1;
                }
                else{
                    left = mid + 1;
                }
            }

            ans.push_back(potions.size() - left);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

## Sorting

```text id="k4m1r8"
O(m log m)
```

where:

```text id="f7m2v5"
m = potions.size()
```

---

## Binary Search For Each Spell

Each search:

```text id="q5m8r1"
O(log m)
```

For all spells:

```text id="t9m4k2"
O(n log m)
```

---

# Final Complexity

## Time

```text id="x6m1r7"
O(m log m + n log m)
```

---

## Space

Ignoring answer vector:

```text id="m3r8v4"
O(1)
```

---

# Common Mistakes

## Mistake 1

Sorting spells.

Wrong because:

```text id="r7m2k5"
answer order must match original spell order
```

---

## Mistake 2

Using:

```cpp id="p4m9r1"
==
```

instead of:

```cpp id="w1m7v8"
>=
```

Problem requires successful pairs, not exact equality.

---

## Mistake 3

Calculating `mid` outside loop.

Wrong because:

```text id="f8m5k2"
left/right change every iteration
```

---

## Mistake 4

Using:

```cpp id="n6m2r9"
left = mid - 1
```

when condition fails.

Wrong direction.

Need larger potion:

```cpp id="q2m8v4"
left = mid + 1
```

---

## Mistake 5

Pushing answer inside binary search loop.

Boundary is only finalized AFTER loop ends.

---

# Recognition Pattern

This is a classic:

# Binary Search Boundary Problem

Pattern:

```text id="v4m7k1"
Find first valid position
```

---

# Important Mental Models

## Binary Search

```text id="m1r9v6"
Find boundary between invalid and valid
```

---

## Monotonicity

```text id="x7m4k2"
Once valid, always valid to the right
```

This is WHY binary search works.

---

# Biggest Takeaway

Whenever you see:

```text id="p5m8r1"
❌ ❌ ❌ ✅ ✅ ✅
```

immediately think:

# Binary Search Boundary Search
