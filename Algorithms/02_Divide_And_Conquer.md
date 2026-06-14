# 03. Divide and Conquer

*Abdul Bari - Algorithms Playlist*

---

# 🎯 Definition

**Divide and Conquer** is an algorithm design technique in which:

1. A problem is divided into smaller subproblems.
2. Each subproblem is solved independently.
3. Solutions of subproblems are combined to get the final solution.

---

## General Idea

```text
Problem(n)
     |
     v
 Divide
     |
     v
Subproblems
     |
     v
 Solve Recursively
     |
     v
 Combine
     |
     v
Final Solution
```

---

# Why Divide and Conquer?

Suppose we have a large problem.

Instead of solving it directly:

```text
Problem Size = n
```

We divide it into smaller pieces:

```text
n
|
+-- n/2
|
+-- n/2
```

Smaller problems are usually easier and faster to solve.

---

# Three Steps of Divide and Conquer

## 1. Divide

Break the problem into smaller parts.

Example:

```text
Array of Size 16

[................]
```

Divide:

```text
[........][........]
```

---

## 2. Conquer

Solve each part recursively.

```text
[....][....]
```

becomes

```text
[..][..][..][..]
```

Continue until the smallest possible problem is reached.

---

## 3. Combine

Combine the solutions obtained from subproblems.

```text
Solution(A)
      +
Solution(B)
      =
Final Answer
```

---

# Recursive Division

Suppose problem size is:

```text
n
```

After first division:

```text
n/2
n/2
```

After second division:

```text
n/4
n/4
n/4
n/4
```

After third division:

```text
n/8
n/8
n/8
n/8
n/8
n/8
n/8
n/8
```

Tree representation:

```text
                    n
                 /     \
              n/2      n/2
             /   \    /   \
          n/4  n/4 n/4  n/4
```

---

# Binary Search as Divide and Conquer

Abdul Bari uses Binary Search as the simplest example.

Given:

```text
10 20 30 40 50 60 70 80 90
```

Find:

```text
70
```

---

## Step 1

Take middle element.

```text
Middle = 50
```

Since:

```text
70 > 50
```

Discard left half.

---

## Step 2

Remaining:

```text
60 70 80 90
```

Again take middle.

---

## Step 3

Find element.

---

## Visualization

```text
10 20 30 40 50 60 70 80 90
             ^
            50

Discard Left Half

60 70 80 90
   ^
  70
```

---

# Binary Search Code (C++)

```cpp
int BinarySearch(int A[], int l, int h, int key)
{
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

# Recursive Binary Search

```cpp
int RBinarySearch(int A[], int l, int h, int key)
{
    if(l <= h)
    {
        int mid = (l + h) / 2;

        if(key == A[mid])
            return mid;

        else if(key < A[mid])
            return RBinarySearch(A,l,mid-1,key);

        else
            return RBinarySearch(A,mid+1,h,key);
    }

    return -1;
}
```

---

# Time Complexity of Binary Search

At every step:

```text
n
n/2
n/4
n/8
...
```

We continue until:

```text
1
```

Therefore:

```text
n / 2^k = 1
```

```text
2^k = n
```

```text
k = log₂(n)
```

Hence:

```text
Time Complexity = O(log n)
```

---

# General Recurrence Relation

A Divide and Conquer algorithm can often be represented as:

```text
T(n) = aT(n/b) + f(n)
```

Where:

* a = Number of subproblems
* b = Division factor
* f(n) = Extra work

---

## Binary Search Recurrence

Only one subproblem:

```text
T(n)=T(n/2)+1
```

Explanation:

```text
T(n/2) -> Recursive Call
+1     -> Comparison Work
```

---

# Important Observation

In Divide and Conquer:

```text
Subproblems are Independent
```

Example:

```text
Left Half
```

does not depend on

```text
Right Half
```

This property makes Divide & Conquer powerful.

---

# Advantages

## Faster Algorithms

Example:

| Method        | Complexity |
| ------------- | ---------- |
| Linear Search | O(n)       |
| Binary Search | O(log n)   |

---

## Natural Recursive Structure

Most Divide & Conquer algorithms can be written recursively.

---

## Parallel Processing

Independent subproblems can be solved simultaneously.

```text
CPU 1 -> Left Half

CPU 2 -> Right Half
```

---

# Applications Mentioned

Divide and Conquer is used in:

* Binary Search
* Merge Sort
* Quick Sort
* Strassen Matrix Multiplication
* Closest Pair Problem

These algorithms are discussed later in the playlist.

---

# Key Takeaways

✅ Divide Problem

✅ Solve Smaller Problems

✅ Combine Solutions

✅ Binary Search is a Divide & Conquer Algorithm

✅ Binary Search Complexity:

```text
O(log n)
```

✅ General Form:

```text
T(n)=aT(n/b)+f(n)
```

---

# Quick Revision

```text
Divide & Conquer
│
├── Divide
│
├── Conquer
│
├── Combine
│
├── Binary Search
│      └── O(log n)
│
└── Recurrence
       └── T(n)=aT(n/b)+f(n)
```
