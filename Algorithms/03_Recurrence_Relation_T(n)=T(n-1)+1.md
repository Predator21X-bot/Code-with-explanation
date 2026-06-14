# 03. Recurrence Relation #1

# T(n) = T(n − 1) + 1

*Abdul Bari Algorithms Playlist — Lecture 2.1.1*

---

# 🎯 Objective

Given a recursive function:

```cpp
void Test(int n)
{
    if(n > 0)
    {
        printf("%d ", n);
        Test(n - 1);
    }
}
```

Determine its:

* Recurrence Relation
* Time Complexity

---

# Understanding the Function

Suppose:

```cpp
Test(3);
```

Execution:

```text
Test(3)
 |
 +-- print(3)
 |
 +-- Test(2)
        |
        +-- print(2)
        |
        +-- Test(1)
               |
               +-- print(1)
               |
               +-- Test(0)
```

Output:

```text
3 2 1
```

---

# Recursion Tree

```text
Test(4)
   |
   +---- Test(3)
              |
              +---- Test(2)
                         |
                         +---- Test(1)
                                    |
                                    +---- Test(0)
```

Each recursive call creates only:

```text
ONE Recursive Call
```

---

# Work Done in Each Call

Inside every call:

```cpp
printf("%d ", n);
```

This takes:

```text
O(1)
```

(Constant Time)

---

# Deriving the Recurrence Relation

Let:

```text
T(n)
```

represent time taken by:

```cpp
Test(n)
```

---

## What happens inside Test(n)?

### Current Work

```cpp
printf("%d ", n);
```

Cost:

```text
1
```

---

### Recursive Work

```cpp
Test(n-1)
```

Cost:

```text
T(n-1)
```

---

Therefore:

```text
T(n) = T(n-1) + 1
```

✅ This is the recurrence relation.

---

# Base Condition

When:

```cpp
n = 0
```

Function returns immediately.

```cpp
if(n > 0)
```

becomes false.

Therefore:

```text
T(0) = 1
```

(or constant)

---

# Recurrence Relation

```text
T(n) = T(n-1) + 1
```

with

```text
T(0) = 1
```

---

# Solving the Recurrence

We use the:

## Repeated Substitution Method

---

## Step 1

```text
T(n)
=
T(n-1)+1
```

---

## Step 2

Substitute:

```text
T(n-1)
=
T(n-2)+1
```

Therefore:

```text
T(n)
=
T(n-2)+1+1
```

---

## Step 3

Substitute again:

```text
T(n-2)
=
T(n-3)+1
```

Result:

```text
T(n)
=
T(n-3)+1+1+1
```

---

## Continue Expanding

```text
T(n)
=
T(n-k)+k
```

---

# Find k

Expansion stops when:

```text
n-k = 0
```

Therefore:

```text
k=n
```

Substituting:

```text
T(n)
=
T(0)+n
```

---

# Apply Base Condition

```text
T(0)=1
```

Hence:

```text
T(n)
=
1+n
```

---

# Final Result

```text
T(n)=n+1
```

Ignoring constants:

```text
T(n)=O(n)
```

---

# Verification Example

Suppose:

```text
n=4
```

Recurrence:

```text
T(4)
=
T(3)+1
```

```text
=
T(2)+2
```

```text
=
T(1)+3
```

```text
=
T(0)+4
```

```text
=
1+4
```

```text
=
5
```

Matches:

```text
n+1
```

---

# Mathematical Proof

From:

```text
T(n)=T(n-1)+1
```

After repeated substitution:

```text
T(n)=T(n-k)+k
```

Set:

```text
n-k=0
```

```text
k=n
```

Thus:

```text
T(n)=T(0)+n
```

```text
T(n)=1+n
```

Therefore:

```text
T(n)=Θ(n)
```

---

# Complete C++ Program

```cpp
#include <iostream>
using namespace std;

void Test(int n)
{
    if(n > 0)
    {
        cout << n << " ";
        Test(n - 1);
    }
}

int main()
{
    Test(5);

    return 0;
}
```

Output:

```text
5 4 3 2 1
```

---

# Complexity Analysis

## Time Complexity

Recurrence:

```text
T(n)=T(n-1)+1
```

Solution:

```text
T(n)=O(n)
```

---

## Space Complexity

Recursive calls stored on stack:

```text
Test(5)
Test(4)
Test(3)
Test(2)
Test(1)
```

Maximum depth:

```text
n
```

Therefore:

```text
Space Complexity = O(n)
```

---

# Important Observation

For recurrence:

```text
T(n)=T(n-1)+c
```

where c is constant:

Solution always becomes:

```text
O(n)
```

Examples:

```text
T(n)=T(n-1)+5
```

```text
T(n)=T(n-1)+10
```

```text
T(n)=T(n-1)+100
```

All are:

```text
O(n)
```

because constants are ignored in asymptotic analysis.

---

# Interview Questions

### Q1. What recurrence relation is generated?

```text
T(n)=T(n-1)+1
```

---

### Q2. Why +1?

Because:

```cpp
printf(...)
```

takes constant time.

---

### Q3. What is the time complexity?

```text
O(n)
```

---

### Q4. What is the recursion depth?

```text
n
```

---

### Q5. What is the space complexity?

```text
O(n)
```

---

# Quick Revision

```text
Function
   |
   +--> Test(n-1)

Recurrence
   |
   +--> T(n)=T(n-1)+1

Expansion
   |
   +--> T(n)=T(n-k)+k

Stopping Condition
   |
   +--> k=n

Solution
   |
   +--> T(n)=n+1

Complexity
   |
   +--> O(n)
```

---

# Key Takeaway

For a recursive function that makes:

```text
One Recursive Call
```

and performs:

```text
Constant Work
```

the recurrence becomes:

```text
T(n)=T(n-1)+1
```

and its solution is:

```text
T(n)=O(n)
```

This is the first and most fundamental recurrence relation in algorithm analysis.
