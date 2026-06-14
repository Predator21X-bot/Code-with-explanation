# 12. Binary Search - Recursive Method

*Abdul Bari Algorithms Playlist — Lecture 2.6.2*

---

# 🎯 Objective

Implement Binary Search using recursion.

In this lecture we will:

* Convert Binary Search into a recursive algorithm
* Understand recursive calls
* Trace the execution
* Implement Recursive Binary Search in C++
* Analyze complexity

---

# Prerequisite

Binary Search works only on:

```text
Sorted Array
```

Example:

```text
3 6 8 12 14 17 25 29 31 36 42 47 53 55 62
```

Index:

```text
0 1 2 3 4 5 6 7 8 9 10 11 12 13 14
```

---

# Review: Iterative Binary Search

In the iterative method:

```cpp
while(l <= h)
{
    mid = (l+h)/2;

    if(key == A[mid])
        return mid;

    else if(key < A[mid])
        h = mid-1;

    else
        l = mid+1;
}
```

We repeatedly shrink:

```text
Search Space
```

until the element is found.

---

# Recursive Idea

Instead of using:

```cpp
while(l <= h)
```

we let the function call itself.

At every step:

```text
Search Left Half
```

or

```text
Search Right Half
```

using recursion.

---

# Algorithm

Let:

```cpp
RBinSearch(l,h,key)
```

represent the recursive search.

---

## Step 1

Find middle element.

```cpp
mid = (l+h)/2;
```

---

## Step 2

If key is found:

```cpp
return mid;
```

---

## Step 3

If key is smaller:

```cpp
Search Left Half
```

```cpp
RBinSearch(l, mid-1, key);
```

---

## Step 4

If key is larger:

```cpp
Search Right Half
```

```cpp
RBinSearch(mid+1, h, key);
```

---

# Recursive Algorithm

```cpp
int RBinSearch(int A[], int l, int h, int key)
{
    if(l <= h)
    {
        int mid = (l + h) / 2;

        if(key == A[mid])
            return mid;

        else if(key < A[mid])
            return RBinSearch(A, l, mid - 1, key);

        else
            return RBinSearch(A, mid + 1, h, key);
    }

    return -1;
}
```

---

# Example

Array:

```text
3 6 8 12 14 17 25 29 31 36 42 47 53 55 62
```

Find:

```text
47
```

---

# First Call

```cpp
RBinSearch(A,0,14,47)
```

Middle:

```text
mid = 7
```

Element:

```text
29
```

Comparison:

```text
47 > 29
```

Search Right Half.

---

# Second Call

```cpp
RBinSearch(A,8,14,47)
```

Middle:

```text
mid = 11
```

Element:

```text
47
```

Found.

Return:

```text
11
```

---

# Recursion Tree

```text
RBinSearch(0,14)

        |
        |
        v

RBinSearch(8,14)

        |
        |
        v

Found
```

Only one recursive call is generated at each level.

---

# Another Example

Search:

```text
14
```

---

## Call 1

```text
mid = 7
A[mid] = 29
```

```text
14 < 29
```

Go Left.

---

## Call 2

```text
Range = 0 to 6
```

Middle:

```text
mid = 3
```

Element:

```text
12
```

```text
14 > 12
```

Go Right.

---

## Call 3

```text
Range = 4 to 6
```

Middle:

```text
mid = 5
```

Element:

```text
17
```

```text
14 < 17
```

Go Left.

---

## Call 4

```text
Range = 4 to 4
```

Middle:

```text
mid = 4
```

Element:

```text
14
```

Found.

---

# Complete C++ Program

```cpp
#include <iostream>
using namespace std;

int RBinSearch(int A[], int l, int h, int key)
{
    if(l <= h)
    {
        int mid = (l + h) / 2;

        if(key == A[mid])
            return mid;

        else if(key < A[mid])
            return RBinSearch(A, l, mid - 1, key);

        else
            return RBinSearch(A, mid + 1, h, key);
    }

    return -1;
}

int main()
{
    int A[] = {3,6,8,12,14,17,25,29,31,36,42,47,53,55,62};

    int n = sizeof(A)/sizeof(A[0]);

    int key = 47;

    int index = RBinSearch(A,0,n-1,key);

    if(index != -1)
        cout << "Element Found at Index " << index;
    else
        cout << "Element Not Found";

    return 0;
}
```

---

# Base Condition

The recursion stops when:

```cpp
l > h
```

Meaning:

```text
Search Space Empty
```

Element does not exist.

Return:

```cpp
-1
```

---

# How Much Does Search Space Shrink?

Every recursive call reduces:

```text
n
```

to

```text
n/2
```

Then:

```text
n/4
```

Then:

```text
n/8
```

and so on.

---

# Recurrence Relation

Recursive Binary Search follows:

```text
T(n)=T(n/2)+1
```

Explanation:

```text
T(n/2)
```

↓

Recursive call on half array

```text
+1
```

↓

Comparison work

---

# Solving

```text
T(n)=T(n/2)+1
```

Solution:

```text
T(n)=O(log n)
```

---

# Complexity Analysis

## Best Case

Element is middle element.

```text
O(1)
```

---

## Worst Case

Element found in last recursive call.

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

Unlike iterative version:

```text
Recursive Calls
```

occupy stack memory.

Maximum depth:

```text
log n
```

Therefore:

```text
Space Complexity = O(log n)
```

---

# Iterative vs Recursive

| Feature        | Iterative | Recursive |
| -------------- | --------- | --------- |
| Time           | O(log n)  | O(log n)  |
| Space          | O(1)      | O(log n)  |
| Implementation | Loop      | Recursion |
| Stack Usage    | No        | Yes       |

---

# Important Observation

At every level:

```text
Only One Recursive Call
```

is generated.

This is why:

```text
T(n)=T(n/2)+1
```

and not:

```text
T(n)=2T(n/2)+1
```

---

# Interview Questions

### Q1. What is the recurrence relation?

```text
T(n)=T(n/2)+1
```

---

### Q2. Why does Binary Search require sorting?

Because we eliminate half the search space based on comparisons.

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

### Q5. Space Complexity of Recursive Binary Search?

```text
O(log n)
```

---

# Quick Revision

```text
Binary Search
│
├── Sorted Array
│
├── Find Mid
│
├── Compare Key
│
├── Left Half
│
├── Right Half
│
├── Recurrence
│     └── T(n)=T(n/2)+1
│
└── Complexity
      ├── Best : O(1)
      ├── Worst: O(log n)
      └── Space: O(log n)
```

---

# Key Takeaway

Recursive Binary Search applies the Divide and Conquer strategy.

```text
Array
   ↓
Half
   ↓
Half
   ↓
Half
   ↓
Found / Not Found
```

Since the search space is reduced by half at every recursive call, the algorithm achieves:

```text
Time Complexity = O(log n)
```

making it one of the most efficient searching algorithms for sorted data.
