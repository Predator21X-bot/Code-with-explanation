# Max Consecutive Ones III — Complete Notes

## Problem Link

LeetCode: Max Consecutive Ones III

---

# Problem Summary

Given:

```cpp
vector<int> nums;
int k;
```

where:

```text
nums contains only 0 and 1
```

You may flip:

```text
at most k zeros → 1
```

Goal:

# Find the length of the longest contiguous subarray that can become all 1s.

---

# Example

```cpp
nums = [1,1,1,0,0,0,1,1,1,1,0]
k = 2
```

Output:

```cpp
6
```

---

# Why?

Choose window:

```text
1 1 0 0 1 1
```

Contains:

```text
2 zeros
```

Flip both:

```text
1 1 1 1 1 1
```

Length:

```text
6
```

---

# MOST Important Observation

The problem says:

```text
flip at most k zeros
```

Let's rewrite it.

Instead of thinking:

```text
which zeros should I flip?
```

Think:

# How many zeros exist inside my current window?

---

# Key Translation

```text
Window contains ≤ k zeros
```

means:

```text
I can flip all of them
```

Therefore:

# valid window

---

# Example

Window:

```text
1 1 0 1 0 1
```

Zeros:

```text
2
```

If:

```text
k = 2
```

Then:

```text
flip both zeros
```

Window becomes:

```text
1 1 1 1 1 1
```

Length:

```text
6
```

Valid candidate.

---

# REAL Goal

We are NOT searching for:

```text
longest sequence of 1s
```

We are searching for:

# longest subarray containing at most k zeros

This is the entire problem.

---

# Step 1 — Identify Pattern

Question asks:

```text
longest subarray
```

and

```text
at most k
```

These are classic signs of:

# Variable Sliding Window

---

# Why Not Fixed Window?

Fixed window problems say:

```text
window size = k
```

Example:

* Maximum sum of size k
* Maximum average of size k

Here:

```text
window size is not given
```

We need the longest valid window.

Therefore:

# Variable Sliding Window

---

# Core Idea

Maintain a window:

```text
left -------- right
```

that always satisfies:

```text
zeroCount <= k
```

---

# What Should We Track?

Only one thing:

```cpp
int zeroCount;
```

---

# Why?

Because validity depends only on:

```text
number of zeros
```

inside current window.

---

# Window Rules

---

# Valid Window

```text
zeroCount <= k
```

Means:

```text
all zeros can be flipped
```

---

# Invalid Window

```text
zeroCount > k
```

Means:

```text
too many zeros
```

Cannot flip them all.

---

# Strategy

## Expand Window

Move:

```cpp
right++
```

Add new element.

---

## If Window Becomes Invalid

Shrink from left.

Move:

```cpp
left++
```

until:

```text
zeroCount <= k
```

again.

---

# Detailed Dry Run

Input:

```cpp
nums = [1,1,0,1,0,1,0]
k = 2
```

---

# Start

```text
L
R

1
```

Window:

```text
[1]
```

Zeros:

```text
0
```

Valid.

Length:

```text
1
```

---

# Expand

```text
L
  R

1 1
```

Zeros:

```text
0
```

Valid.

Length:

```text
2
```

---

# Expand

```text
L
    R

1 1 0
```

Zeros:

```text
1
```

Valid.

Length:

```text
3
```

---

# Expand

```text
L
      R

1 1 0 1
```

Zeros:

```text
1
```

Valid.

Length:

```text
4
```

---

# Expand

```text
L
        R

1 1 0 1 0
```

Zeros:

```text
2
```

Valid.

Length:

```text
5
```

---

# Expand

```text
L
          R

1 1 0 1 0 1
```

Zeros:

```text
2
```

Valid.

Length:

```text
6
```

---

# Expand Again

```text
L
            R

1 1 0 1 0 1 0
```

Zeros:

```text
3
```

Now:

```text
3 > 2
```

Invalid.

---

# What Do We Do?

Shrink.

---

Move Left:

```text
1 1 0 1 0 1 0
  ↑
```

Removed:

```text
1
```

Zeros unchanged.

Still invalid.

---

Move Again:

```text
1 1 0 1 0 1 0
    ↑
```

Removed:

```text
1
```

Still invalid.

---

Move Again:

```text
1 1 0 1 0 1 0
      ↑
```

Removed:

```text
0
```

Now:

```text
zeroCount--
```

Becomes:

```text
2
```

Valid again.

---

# Why Use While?

Suppose:

```text
zeroCount = 5
k = 2
```

One shrink won't fix it.

Need multiple shrinks.

Therefore:

```cpp
while(zeroCount > k)
```

NOT:

```cpp
if(zeroCount > k)
```

---

# Important Mental Model

Think of window as:

```text
rubber band
```

Expand:

```text
right++
```

Too many zeros?

Shrink:

```text
left++
```

until valid again.

---

# Code Walkthrough

## Initialize

```cpp
int left = 0;
int zeroCount = 0;
int maxLen = 0;
```

---

## Expand Window

```cpp
for(int right = 0; right < nums.size(); right++)
```

---

## Count New Zero

```cpp
if(nums[right] == 0)
    zeroCount++;
```

---

## Fix Invalid Window

```cpp
while(zeroCount > k)
```

---

## Remove Left Element

If left element is zero:

```cpp
if(nums[left] == 0)
    zeroCount--;
```

---

## Move Left

```cpp
left++;
```

---

## Update Answer

Current valid window length:

```cpp
right - left + 1
```

Update:

```cpp
maxLen =
max(maxLen,
    right-left+1);
```

---

# Complete Code

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {

        int left = 0;
        int zeroCount = 0;
        int maxLen = 0;

        for(int right = 0;right < nums.size();right++) {

            if(nums[right] == 0)
                zeroCount++;

            while(zeroCount > k) {

                if(nums[left] == 0)
                    zeroCount--;

                left++;
            }

            maxLen =
                max(maxLen,right-left+1);
        }

        return maxLen;
    }
};
```

---

# Complexity Analysis

## Time

Each pointer moves at most:

```text
n times
```

Therefore:

```text
O(n)
```

---

# Space

Only variables used.

```text
O(1)
```

---

# Common Mistakes

## Mistake 1

Trying to actually flip zeros.

Not needed.

Just count zeros.

---

## Mistake 2

Using fixed window.

This is variable window.

---

## Mistake 3

Using:

```cpp
if(zeroCount > k)
```

instead of:

```cpp
while(zeroCount > k)
```

Window may require multiple shrinks.

---

## Mistake 4

Updating answer before fixing invalid window.

Always:

```text
fix first
then update answer
```

---

# Recognition Pattern

Whenever you see:

```text
Longest ...
Smallest ...
At most k ...
At least k ...
Condition on subarray ...
```

Think:

# Variable Sliding Window

---

# Generic Variable Sliding Window Template

```cpp
int left = 0;

for(int right = 0; right < n; right++) {

    // expand
    update state

    while(invalid condition) {

        // shrink
        fix state

        left++;
    }

    // valid window
    update answer
}
```

---

# Similar Problems

* Longest Repeating Character Replacement
* Fruit Into Baskets
* Longest Substring Without Repeating Characters
* Binary Subarrays With Sum
* Minimum Size Subarray Sum

---

# Biggest Takeaway

The key observation is:

# "At most k flips"

becomes

# "Window can contain at most k zeros"

Once you see that translation, the entire problem becomes a standard variable sliding window problem.
