# Binary Search — Complete Mastery Notes

---

# 1. Introduction

Binary Search is one of the most fundamental algorithms in Computer Science.

It efficiently searches for an element in a sorted collection by repeatedly dividing the search space into half.

Instead of checking every element one by one:

```text
Linear Search:
1 3 5 7 9 11 13

Check:
1 → 3 → 5 → 7 → 9

O(n)
```

Binary Search does:

```text
1 3 5 7 9 11 13

Check middle:
7

9 > 7

Discard left half.

Check middle of remaining.

Done.

O(log n)
```

---

# 2. Core Idea

The algorithm works because the data is sorted.

Every comparison eliminates half of the remaining search space.

```text
N
N/2
N/4
N/8
...
1
```

Number of divisions:

```math
log₂(N)
```

Therefore:

```text
Time Complexity = O(log N)
```

---

# 3. Intuition

Imagine a dictionary.

To find:

```text
Algorithm
```

You do NOT start from page 1.

You open somewhere near the middle.

If your word is alphabetically larger:

```text
Discard left half.
```

Otherwise:

```text
Discard right half.
```

This is Binary Search.

---

# 4. Mathematical Analysis

Suppose:

```text
N = 1,000,000
```

Linear Search:

```text
Worst Case:
1,000,000 comparisons
```

Binary Search:

```text
log₂(1,000,000)

≈ 20
```

Only about:

```text
20 comparisons
```

Huge difference.

---

# 5. Preconditions

Binary Search requires:

## Condition 1

Sorted Data

Example:

```text
1 3 5 7 9 11
```

Works.

---

Example:

```text
7 1 9 3 11
```

Does NOT work.

---

## Condition 2

Monotonic Behavior

A function is monotonic if it only changes in one direction.

Example:

```text
False False False True True True
```

Boundary exists.

Binary Search can find it.

---

# 6. Standard Binary Search

Goal:

Find exact target.

Example:

```text
Array:

1 3 5 7 9 11 13

Target:

9
```

---

Step 1

```text
left = 0
right = 6

mid = 3

value = 7
```

---

Step 2

```text
9 > 7

Search right half

left = 4
```

---

Step 3

```text
mid = 5

value = 11
```

---

Step 4

```text
9 < 11

right = 4
```

---

Step 5

```text
mid = 4

value = 9

Found
```

---

# 7. Standard Implementation

```cpp
int binarySearch(vector<int>& arr, int target)
{
    int left = 0;
    int right = arr.size() - 1;

    while(left <= right)
    {
        int mid = left + (right - left) / 2;

        if(arr[mid] == target)
            return mid;

        if(arr[mid] < target)
            left = mid + 1;
        else
            right = mid - 1;
    }

    return -1;
}
```

---

# 8. Why Mid Calculation Matters

Wrong:

```cpp
mid = (left + right) / 2;
```

Problem:

```text
Integer Overflow
```

Suppose:

```cpp
left = 2,000,000,000
right = 2,100,000,000
```

Their sum exceeds integer range.

---

Correct:

```cpp
mid = left + (right - left) / 2;
```

Safe.

Always use this.

---

# 9. Time Complexity

Each iteration:

```text
Search Space / 2
```

Therefore:

```text
O(log N)
```

---

# 10. Space Complexity

Only variables:

```cpp
left
right
mid
```

Thus:

```text
O(1)
```

---

# 11. Lower Bound

One of the most important variations.

Definition:

Find first element:

```text
>= target
```

Example:

```text
Array:

1 3 5 5 5 7 9

Target:

5
```

Answer:

```text
Index 2
```

First occurrence.

---

Implementation:

```cpp
int lowerBound(vector<int>& arr, int target)
{
    int left = 0;
    int right = arr.size();

    while(left < right)
    {
        int mid = left + (right - left) / 2;

        if(arr[mid] < target)
            left = mid + 1;
        else
            right = mid;
    }

    return left;
}
```

---

# 12. Upper Bound

Definition:

Find first element:

```text
> target
```

Example:

```text
1 3 5 5 5 7 9
```

Target:

```text
5
```

Answer:

```text
Index = 5
```

Value:

```text
7
```

---

Implementation:

```cpp
int upperBound(vector<int>& arr, int target)
{
    int left = 0;
    int right = arr.size();

    while(left < right)
    {
        int mid = left + (right - left) / 2;

        if(arr[mid] <= target)
            left = mid + 1;
        else
            right = mid;
    }

    return left;
}
```

---

# 13. First Occurrence

Example:

```text
1 2 2 2 2 3 4
```

Target:

```text
2
```

Need:

```text
Index 1
```

Use:

```text
Lower Bound
```

---

# 14. Last Occurrence

Example:

```text
1 2 2 2 2 3 4
```

Need:

```text
Index 4
```

Formula:

```cpp
upperBound(arr,target)-1
```

---

# 15. Binary Search on Answer

Most important advanced application.

Many interview problems are secretly Binary Search.

---

Example:

Koko Eating Bananas

Need minimum speed.

Possible speeds:

```text
1
2
3
...
1000000000
```

Search space:

```text
Answers
```

NOT array.

---

Observation:

```text
Speed = 3 → impossible

Speed = 4 → impossible

Speed = 5 → possible

Speed = 6 → possible
```

Pattern:

```text
False False False True True True
```

Binary Search works.

---

# 16. Monotonic Functions

Binary Search requires:

```text
F F F F T T T T
```

or

```text
T T T T F F F F
```

Examples:

### Capacity

```text
Can X servers handle load?
```

### Shipping

```text
Can ship packages in D days?
```

### Scheduling

```text
Can complete tasks within K hours?
```

---

# 17. Common Binary Search Patterns

---

## Pattern 1

Exact Search

```text
Find target
```

---

## Pattern 2

First True

```text
F F F F T T T
```

---

## Pattern 3

Last True

```text
T T T T F F F
```

---

## Pattern 4

Minimum Valid Answer

```text
Minimum speed
Minimum cost
Minimum capacity
```

---

## Pattern 5

Maximum Valid Answer

```text
Maximum distance
Maximum profit
Maximum score
```

---

# 18. Real World Applications

---

## Databases

B-Trees use binary-search-like logic.

Used in:

```text
MySQL
PostgreSQL
MongoDB
```

---

## Search Engines

Used for:

```text
Index lookup
```

---

## Operating Systems

Used in:

```text
Memory allocation
Page management
```

---

## Networking

Used in:

```text
Routing tables
```

---

## Machine Learning

Used in:

```text
Hyperparameter optimization
```

---

# 19. Common Interview Questions

Easy:

```text
Binary Search
Search Insert Position
Guess Number
```

---

Medium:

```text
Search Rotated Array
Koko Eating Bananas
Find Peak Element
```

---

Hard:

```text
Split Array Largest Sum
Median of Two Sorted Arrays
```

---

# 20. Common Mistakes

---

Mistake 1

Using Binary Search on unsorted data.

Wrong.

---

Mistake 2

Infinite loops.

Wrong:

```cpp
left = mid;
```

Correct:

```cpp
left = mid + 1;
```

---

Mistake 3

Overflow.

Wrong:

```cpp
(left + right)/2
```

Correct:

```cpp
left + (right-left)/2
```

---

Mistake 4

Wrong search boundaries.

Always define:

```text
What does left mean?
What does right mean?
```

Before coding.

---

# 21. Master-Level Recognition

If the problem contains:

```text
Minimum possible
Maximum possible
Capacity
Speed
Threshold
Boundary
Sorted
First occurrence
Last occurrence
```

Immediately suspect:

```text
Binary Search
```

---

# 22. Competitive Programming Insight

Top coders rarely use Binary Search only on arrays.

They use:

```text
Binary Search on Answer
```

80% of advanced Binary Search problems are actually:

```text
Find smallest X satisfying condition.
```

---

# 23. Mastery Checklist

Can explain Binary Search intuition.

Can derive complexity.

Can code from memory.

Can code lower bound.

Can code upper bound.

Can identify monotonic functions.

Can solve Binary Search on Answer.

Can recognize interview patterns instantly.

---

# 24. Final Cheat Sheet

Exact Search

```cpp
while(left <= right)
```

---

Lower Bound

```cpp
first >= target
```

---

Upper Bound

```cpp
first > target
```

---

Complexity

```text
Time : O(log N)
Space: O(1)
```

---

Golden Rule

Don't think:

"Find element."

Think:

"Find boundary."
