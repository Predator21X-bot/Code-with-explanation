# 04. Recurrence Relation #2

# T(n) = T(n − 1) + n

*Abdul Bari Algorithms Playlist — Lecture 2.1.2*

---

# 🎯 Objective

Analyze the recursive function:

```cpp
void Test(int n)
{
    if(n > 0)
    {
        for(int i=0; i<n; i++)
        {
            printf("%d ", n);
        }

        Test(n-1);
    }
}
```

Determine:

* Recurrence Relation
* Time Complexity
* Growth Pattern

---

# Understanding the Function

Unlike the previous lecture:

```cpp
printf("%d", n);
```

which performed constant work,

here we have:

```cpp
for(int i=0; i<n; i++)
{
    printf("%d ", n);
}
```

The loop executes:

```text
n times
```

for every recursive call.

---

# Example Execution

Suppose:

```cpp
Test(4);
```

---

## First Call

```cpp
Test(4)
```

Loop executes:

```text
4 times
```

Output:

```text
4 4 4 4
```

Then:

```cpp
Test(3)
```

---

## Second Call

```cpp
Test(3)
```

Loop executes:

```text
3 times
```

Output:

```text
3 3 3
```

Then:

```cpp
Test(2)
```

---

## Third Call

```cpp
Test(2)
```

Output:

```text
2 2
```

Then:

```cpp
Test(1)
```

---

## Fourth Call

```cpp
Test(1)
```

Output:

```text
1
```

Then:

```cpp
Test(0)
```

Stops.

---

# Recursion Tree

```text
Test(4)
 |
 +-- Work = 4
 |
 +-- Test(3)
       |
       +-- Work = 3
       |
       +-- Test(2)
              |
              +-- Work = 2
              |
              +-- Test(1)
                     |
                     +-- Work = 1
                     |
                     +-- Test(0)
```

---

# Cost at Each Level

| Call    | Work Done |
| ------- | --------- |
| Test(4) | 4         |
| Test(3) | 3         |
| Test(2) | 2         |
| Test(1) | 1         |
| Test(0) | Constant  |

---

# Deriving the Recurrence

Let:

```text
T(n)
```

be the running time of:

```cpp
Test(n)
```

---

## Current Work

The loop runs:

```cpp
for(i=0;i<n;i++)
```

Cost:

```text
n
```

---

## Recursive Work

```cpp
Test(n-1)
```

Cost:

```text
T(n-1)
```

---

## Therefore

```text
T(n)=T(n-1)+n
```

This is the recurrence relation.

---

# Base Condition

When:

```cpp
n=0
```

Function immediately returns.

Thus:

```text
T(0)=1
```

(Constant)

---

# Recurrence Relation

```text
T(n)=T(n-1)+n
```

with

```text
T(0)=1
```

---

# Solving by Substitution

---

## Step 1

```text
T(n)
=
T(n-1)+n
```

---

## Step 2

Substitute:

```text
T(n-1)
=
T(n-2)+(n-1)
```

Therefore:

```text
T(n)
=
T(n-2)+(n-1)+n
```

---

## Step 3

Substitute again:

```text
T(n-2)
=
T(n-3)+(n-2)
```

Result:

```text
T(n)
=
T(n-3)
+(n-2)
+(n-1)
+n
```

---

## General Form

After k substitutions:

```text
T(n)
=
T(n-k)
+
(n-k+1)
+
(n-k+2)
+
...
+
n
```

---

# Find k

Expansion stops when:

```text
n-k=0
```

Thus:

```text
k=n
```

---

# Substituting

```text
T(n)
=
T(0)
+
1+2+3+...+n
```

---

# Use Summation Formula

We know:

```text
1+2+3+...+n
=
n(n+1)/2
```

Therefore:

```text
T(n)
=
1 + n(n+1)/2
```

---

# Simplify

```text
T(n)
=
1
+
(n²+n)/2
```

```text
T(n)
=
(n²/2)
+
(n/2)
+
1
```

---

# Dominant Term

Largest growing term:

```text
n²
```

Therefore:

```text
T(n)=Θ(n²)
```

and

```text
T(n)=O(n²)
```

---

# Mathematical Derivation

```text
T(n)
=
T(n-1)+n
```

```text
=
T(n-2)+(n-1)+n
```

```text
=
T(n-3)+(n-2)+(n-1)+n
```

```text
=
T(0)+1+2+...+n
```

```text
=
1+n(n+1)/2
```

```text
=
Θ(n²)
```

---

# Verification Example

Suppose:

```text
n=4
```

Then:

```text
T(4)
=
T(3)+4
```

```text
=
T(2)+3+4
```

```text
=
T(1)+2+3+4
```

```text
=
T(0)+1+2+3+4
```

```text
=
1+10
```

```text
=
11
```

Using formula:

```text
1 + n(n+1)/2
```

```text
=
1 + 4×5/2
```

```text
=
11
```

Correct.

---

# Complete C++ Program

```cpp
#include <iostream>
using namespace std;

void Test(int n)
{
    if(n > 0)
    {
        for(int i=0;i<n;i++)
        {
            cout << n << " ";
        }

        cout << endl;

        Test(n-1);
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
5 5 5 5 5
4 4 4 4
3 3 3
2 2
1
```

---

# Complexity Analysis

## Time Complexity

Recurrence:

```text
T(n)=T(n-1)+n
```

Solution:

```text
T(n)=Θ(n²)
```

---

## Space Complexity

Recursion depth:

```text
n
```

Therefore:

```text
Space = O(n)
```

---

# Comparison with Previous Lecture

| Recurrence    | Complexity |
| ------------- | ---------- |
| T(n)=T(n−1)+1 | O(n)       |
| T(n)=T(n−1)+n | O(n²)      |

---

# Important Observation

Whenever expansion produces:

```text
1+2+3+...+n
```

Immediately remember:

```text
n(n+1)/2
```

which grows as:

```text
Θ(n²)
```

---

# Interview Questions

### Q1. What recurrence is generated?

```text
T(n)=T(n−1)+n
```

---

### Q2. Why is the extra term n?

Because the loop executes:

```cpp
n times
```

for each recursive call.

---

### Q3. What is the solution?

```text
T(n)=1+n(n+1)/2
```

---

### Q4. Time Complexity?

```text
O(n²)
```

---

### Q5. Space Complexity?

```text
O(n)
```

---

# Quick Revision

```text
Function
   |
   +--> Loop n times
   |
   +--> Test(n-1)

Recurrence
   |
   +--> T(n)=T(n-1)+n

Expansion
   |
   +--> T(0)+1+2+...+n

Formula
   |
   +--> n(n+1)/2

Complexity
   |
   +--> O(n²)
```

---

# Key Takeaway

For recursive functions that:

```text
Perform n units of work
+
Call Test(n−1)
```

the recurrence becomes:

```text
T(n)=T(n−1)+n
```

which expands into:

```text
1+2+3+...+n
```

and results in:

```text
T(n)=Θ(n²)
```

This is the second fundamental recurrence relation and serves as the basis for analyzing many recursive algorithms involving loops inside recursion.
