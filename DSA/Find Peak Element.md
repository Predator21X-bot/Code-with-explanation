# Find Peak Element

## Problem Summary

We are given:

```cpp id="m1p8r4"
nums[]
```

We must return the index of ANY:

# Peak Element

A peak element means:

```text id="q7m2v5"
nums[i] > nums[i-1]
AND
nums[i] > nums[i+1]
```

The element is greater than both neighbors.

---

# Important Detail

Problem guarantees:

```text id="x4m9r1"
nums[i] != nums[i+1]
```

So equal neighboring values do not exist.

---

# Another Important Detail

We only need:

```text id="t8m1k6"
ANY peak
```

NOT necessarily the maximum element.

---

# Example 1

## Input

```text id="f5m7r2"
[1,2,3,1]
```

Peak:

```text id="p2m8v4"
3
```

because:

```text id="v6m1r9"
3 > 2
3 > 1
```

Return index:

```text id="m4r8k1"
2
```

---

# Example 2

## Input

```text id="q9m2v7"
[1,2,1,3,5,6,4]
```

Possible peaks:

```text id="x3m7r5"
2
or
6
```

Both are valid answers.

---

# Brute Force Approach

Check every element individually.

If:

```text id="f8m1k2"
nums[i] > neighbors
```

return index.

---

# Brute Force Complexity

## Time

```text id="p5m9r1"
O(n)
```

---

# But Problem Requires

```text id="t2m7v4"
O(log n)
```

So Binary Search is needed.

---

# BIG QUESTION

How can Binary Search work on an UNSORTED array?

This is the key insight.

---

# Important Observation

We do NOT use sorted order.

We use:

# Slope / Gradient

relationship between neighboring elements.

---

# Increasing Slope Case

Suppose:

```text id="n8m4r6"
nums[mid] < nums[mid+1]
```

Example:

```text id="r4m1k8"
1 2 3 4 5
        ^
       mid
```

This means:

```text id="q7m5v2"
we are moving upward
```

So where must a peak exist?

👉 On the RIGHT side.

---

# Why?

Because increasing cannot continue forever.

Eventually one of two things happens:

1. array ends
2. slope becomes decreasing

Both situations create a peak.

---

# Visual Intuition

```text id="x1m8r4"
1 2 3 4 5 2
```

Peak clearly exists on right side.

---

# Decreasing Slope Case

Suppose:

```text id="m9r2k5"
nums[mid] > nums[mid+1]
```

Example:

```text id="v5m7r1"
5 4 3 2
^
mid
```

This means:

```text id="f2m8v6"
we are on decreasing slope
```

Peak exists on LEFT side.

Possibly:

```text id="t4m1r9"
mid itself
```

---

# MOST IMPORTANT Insight

We are NOT asking:

```text id="q8m5r2"
Is mid the answer?
```

Instead we ask:

```text id="x6m1v7"
Which side definitely contains a peak?
```

This is advanced Binary Search thinking.

---

# Core Binary Search Logic

## Case 1 — Increasing Slope

```cpp id="m3r8k1"
nums[mid] < nums[mid + 1]
```

Peak exists on RIGHT.

Move:

```cpp id="p7m2v4"
left = mid + 1;
```

---

## Case 2 — Decreasing Slope

```cpp id="f4m9r2"
nums[mid] > nums[mid + 1]
```

Peak exists on LEFT.

Move:

```cpp id="q1m7k8"
right = mid;
```

---

# VERY Important Detail

We use:

```cpp id="v8m4r1"
right = mid
```

NOT:

```cpp id="n5m2v7"
right = mid - 1
```

---

# Why?

Because:

```text id="x2m8r5"
mid itself may already be peak
```

Example:

```text id="m6r1k9"
[1,2,5,3]
      ^
     mid
```

If we remove `mid`, we may discard correct answer.

---

# Loop Condition

Use:

```cpp id="t8m4v2"
while(left < right)
```

NOT:

```cpp id="q3m7r1"
<=
```

because search space shrinks toward ONE final position.

---

# Final State

Eventually:

```text id="v9m2k4"
left == right
```

That index is guaranteed peak.

Return:

```cpp id="f1m8r6"
left
```

---

# Dry Run

## Input

```text id="p4m7v2"
[1,2,3,1]
```

---

# Iteration 1

```text id="x7m1r5"
left = 0
right = 3
mid = 1
```

Check:

```text id="m2r9k8"
nums[1] = 2
nums[2] = 3
```

Increasing slope.

Move:

```text id="q5m8v1"
left = 2
```

---

# Iteration 2

```text id="t1m7r4"
left = 2
right = 3
mid = 2
```

Check:

```text id="f8m2k5"
3 > 1
```

Decreasing slope.

Move:

```text id="v4m9r1"
right = 2
```

Now:

```text id="n6m1v8"
left == right == 2
```

Return:

```text id="x5m2r7"
2
```

Correct peak index.

---

# Final Code

```cpp id="k4m9v2"
class Solution {
public:
    int findPeakElement(vector<int>& nums) {

        int left = 0;
        int right = nums.size() - 1;

        while(left < right){

            int mid = left + (right - left) / 2;

            if(nums[mid] < nums[mid + 1]){
                left = mid + 1;
            }
            else{
                right = mid;
            }
        }

        return left;
    }
};
```

---

# Complexity Analysis

Each iteration removes half search space.

---

# Time Complexity

```text id="w8m1r3"
O(log n)
```

---

# Space Complexity

Only variables used.

```text id="k7m2r4"
O(1)
```

---

# Common Mistakes

## Mistake 1

Using:

```cpp id="q4m8k1"
right = mid - 1
```

Wrong because:

```text id="x1m7r5"
mid itself may be peak
```

Correct:

```cpp id="t8m2v6"
right = mid
```

---

## Mistake 2

Using:

```cpp id="f5m9r2"
while(left <= right)
```

Can complicate logic.

Cleaner approach:

```cpp id="p2m8k4"
while(left < right)
```

---

## Mistake 3

Trying to directly verify peak at every step.

Not necessary.

We only need to determine:

```text id="v6m1r9"
which side definitely contains peak
```

---

## Mistake 4

Thinking Binary Search only works on sorted arrays.

This problem teaches:

```text id="m4r7k2"
Binary Search can work on patterns/slopes too
```

---

# Recognition Pattern

This is classic:

# Binary Search on Gradient / Slope

Instead of searching sorted values.

---

# Important Mental Models

## Increasing Slope

```text id="q9m2v5"
Peak exists on right side
```

---

## Decreasing Slope

```text id="x3m8r1"
Peak exists on left side
```

---

# Biggest Takeaway

Binary Search is fundamentally about:

```text id="f7m4k8"
eliminating impossible regions
```

not necessarily searching sorted arrays.

This problem is one of the best examples of that idea.
