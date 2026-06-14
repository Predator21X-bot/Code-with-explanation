# 11. Binary Search - Iterative Method

*Abdul Bari Algorithms Playlist — Lecture 2.6.1*

---

# 🎯 Objective

Learn how Binary Search works using the iterative approach and understand:

* Prerequisites
* Search Procedure
* Algorithm
* C++ Implementation
* Time Complexity Analysis

---

# What is Binary Search?

Binary Search is a searching technique that repeatedly divides the search space into two halves.

Instead of checking every element one by one:

```text
Linear Search
```

Binary Search eliminates half of the remaining elements after every comparison.

---

# Prerequisite

⚠️ Binary Search works only on:

```text
Sorted Arrays
```

Example:

```text
3 6 8 12 14 17 25 29 31 36 42 47 53 55 62
```

Index:

```text
1  2  3  4  5  6  7  8  9  10 11 12 13 14 15
```

---

# Example Problem

Find:

```text
Key = 47
```

Array:

```text
3 6 8 12 14 17 25 29 31 36 42 47 53 55 62
```

---

# Step 1

Initialize:

```cpp
l = 0
h = n-1
```

For our example:

```text
l = 0
h = 14
```

---

# Step 2

Find Middle Element

Formula:

```cpp
mid = (l + h) / 2;
```

Calculation:

```text
mid = (0 + 14) / 2
     = 7
```

Element:

```text
A[7] = 29
```

---

# Comparison

```text
47 > 29
```

Therefore:

```text
Discard Left Half
```

New Search Space:

```text
31 36 42 47 53 55 62
```

---

# Step 3

Update:

```cpp
l = mid + 1
```

Now:

```text
l = 8
h = 14
```

---

# Step 4

Compute New Mid

```text
mid = (8+14)/2
     = 11
```

Element:

```text
A[11] = 47
```

Found!

---

# Visualization

Initial Array

```text
3 6 8 12 14 17 25 29 31 36 42 47 53 55 62
              ↑
             29
```

Discard Left Side

```text
31 36 42 47 53 55 62
         ↑
        47
```

Found.

---

# Binary Search Algorithm

```text
Repeat while l <= h

    mid = (l+h)/2

    if key == A[mid]
        return mid

    else if key < A[mid]
        h = mid - 1

    else
        l = mid + 1
```

---

# Iterative C++ Implementation

```cpp
int BinarySearch(int A[], int n, int key)
{
    int l = 0;
    int h = n - 1;

    while(l <= h)
    {
        int mid = (l + h) / 2;

        if(key == A[mid])
            return mid;

        else if(key < A[mid])
            h = mid - 1;

        else
            l = mid + 1;
    }

    return -1;
}
```

---

# Complete Program

```cpp
#include <iostream>
using namespace std;

int BinarySearch(int A[], int n, int key)
{
    int l = 0;
    int h = n - 1;

    while(l <= h)
    {
        int mid = (l + h) / 2;

        if(key == A[mid])
            return mid;

        else if(key < A[mid])
            h = mid - 1;

        else
            l = mid + 1;
    }

    return -1;
}

int main()
{
    int A[] = {3,6,8,12,14,17,25,29,31,36,42,47,53,55,62};

    int n = sizeof(A)/sizeof(A[0]);

    int key = 47;

    int index = BinarySearch(A,n,key);

    if(index != -1)
        cout << "Element Found at Index: " << index;
    else
        cout << "Element Not Found";

    return 0;
}
```

---

# Trace Table

Searching:

```text
47
```

| l | h  | mid | A[mid] |
| - | -- | --- | ------ |
| 0 | 14 | 7   | 29     |
| 8 | 14 | 11  | 47     |

Element found.

---

# Why is Binary Search Fast?

Linear Search:

```text
Check Every Element
```

Worst Case:

```text
n comparisons
```

Complexity:

```text
O(n)
```

---

Binary Search:

```text
Remove Half
```

every iteration.

```text
n
n/2
n/4
n/8
...
1
```

---

# Complexity Analysis

Let:

```text
n / 2ᵏ = 1
```

Therefore:

```text
2ᵏ = n
```

Taking log:

```text
k = log₂(n)
```

Hence:

```text
Time Complexity = O(log n)
```

---

# Best Case

Element found immediately.

```text
O(1)
```

Example:

```text
Middle Element = Key
```

---

# Worst Case

Element found at last step.

or

```text
Element Not Present
```

Complexity:

```text
O(log n)
```

---

# Space Complexity

Iterative version uses:

```text
l
h
mid
```

Only a few variables.

Therefore:

```text
O(1)
```

---

# Advantages

✅ Very Fast

✅ Logarithmic Complexity

✅ Efficient for Large Data

✅ Simple Implementation

---

# Limitations

❌ Array Must Be Sorted

❌ Insertion/Deletion in sorted arrays can be costly

---

# Comparison

| Technique     | Complexity |
| ------------- | ---------- |
| Linear Search | O(n)       |
| Binary Search | O(log n)   |

---

# Interview Questions

### Q1. What is the prerequisite for Binary Search?

```text
Array must be sorted.
```

---

### Q2. Why is Binary Search O(log n)?

Because every step removes half of the search space.

---

### Q3. Best Case Complexity?

```text
O(1)
```

---

### Q4. Worst Case Complexity?

```text
O(log n)
```

---

### Q5. Space Complexity of Iterative Binary Search?

```text
O(1)
```

---

# Quick Revision

```text
Binary Search
│
├── Sorted Array Required
│
├── Find Middle
│
├── Compare Key
│
├── Eliminate Half
│
├── Repeat
│
└── Complexity
      ├── Best : O(1)
      ├── Worst: O(log n)
      └── Space: O(1)
```

---

# Key Takeaway

Binary Search is one of the most important searching algorithms.

Core idea:

```text
Compare with Middle
       ↓
Discard Half
       ↓
Repeat
```

This reduction by half at every step gives Binary Search its powerful:

```text
O(log n)
```

time complexity, making it significantly faster than Linear Search for large sorted datasets.
