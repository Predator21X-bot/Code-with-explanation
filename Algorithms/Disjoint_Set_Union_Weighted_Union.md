# 🌳 Disjoint Set (Union-Find) Data Structure

> Efficiently manage **disjoint groups**, perform **fast unions**, and detect **cycles in graphs**.

---

## 📚 Table of Contents

* [What is a Disjoint Set?](#-what-is-a-disjoint-set)
* [Operations](#-core-operations)
* [Tree Representation](#-tree-representation)
* [Find Operation](#-find-operation)
* [Union Operation](#-union-operation)
* [Weighted Union (Union by Size)](#-weighted-union-union-by-size)
* [Cycle Detection](#-cycle-detection-using-disjoint-set)
* [Complexity Analysis](#-complexity-analysis)
* [Applications](#-applications)
* [Interview Notes](#-interview-notes)
* [Quick Revision](#-quick-revision-sheet)

---

# 🎯 What is a Disjoint Set?

A **Disjoint Set** (also called **Union-Find**) is a data structure that maintains a collection of **non-overlapping sets**.

It supports two powerful operations:

| Operation    | Purpose                               |
| ------------ | ------------------------------------- |
| `Find(x)`    | Finds which set an element belongs to |
| `Union(x,y)` | Merges two sets                       |

---

## Example

Initially:

```text
{1} {2} {3} {4} {5} {6} {7} {8}
```

After:

```text
Union(1,2)
Union(3,4)
Union(5,6)
Union(7,8)
```

Result:

```text
{1,2} {3,4} {5,6} {7,8}
```

---

# ⚡ Core Operations

## 1️⃣ Find(x)

Returns the **representative/root** of the set containing `x`.

### Example

```text
    1
    |
    2
```

```cpp
Find(2) = 1
```

Because `1` is the root.

---

## 2️⃣ Union(x,y)

Combines two different sets into one.

### Example

Before:

```text
Set A          Set B

  1              3
  |              |
  2              4
```

After:

```text
      1
     / \
    2   3
         \
          4
```

Now all elements belong to the same set.

---

# 🌲 Tree Representation

Every set is represented using a tree.

Example:

```text
    1
    |
    2
```

Root:

```text
1
```

Representative of the set:

```cpp
Find(1) = 1
Find(2) = 1
```

---

# 📦 Parent Array Representation

Instead of storing actual trees, we use an array.

### Example

```text
    1
    |
    2
```

Parent Array:

| Index  | 1  | 2 |
| ------ | -- | - |
| Parent | -2 | 1 |

---

### Interpretation

```text
-2 → Root node
2  → Size of set

1  → Parent of node 2
```

Negative values indicate roots.

---

# 🔍 Find Operation

The goal is to climb parent pointers until a root is reached.

### Example

```text
      1
      |
      2
      |
      5
```

Find(5):

```text
5 → 2 → 1
```

Result:

```cpp
Find(5) = 1
```

---

## Recursive Implementation

```cpp
int Find(int x)
{
    if(parent[x] < 0)
        return x;

    return Find(parent[x]);
}
```

---

## Complexity

```text
O(height of tree)
```

Therefore, shorter trees = faster finds.

---

# 🤝 Union Operation

Suppose:

```text
    1          3
    |          |
    2          4
```

Perform:

```cpp
Union(1,3)
```

Result:

```text
       1
      / \
     2   3
          \
           4
```

---

# 🚨 Problem with Simple Union

Consider repeatedly attaching larger trees under smaller trees.

```text
1
|
2
|
3
|
4
|
5
|
6
```

This creates a tall chain.

### Height

```text
6
```

### Find Complexity

```text
O(n)
```

❌ Inefficient

---

# ⚖️ Weighted Union (Union by Size)

## Idea

Always attach:

```text
Smaller Tree
      ↓
Larger Tree
```

This keeps height small.

---

## Example

Tree A

```text
      1
     / \
    2   3
```

Size = 3

Tree B

```text
    4
    |
    5
```

Size = 2

---

After Union:

```text
         1
       / | \
      2  3  4
             |
             5
```

Height barely increases.

✅ Faster Find operations

---

# 📏 How Size is Stored

Root nodes store:

```text
Negative Size
```

Example:

```text
Parent[1] = -3
```

Meaning:

```text
Root = 1
Size = 3
```

---

Another set:

```text
Parent[4] = -2
```

Meaning:

```text
Root = 4
Size = 2
```

---

After Union:

```text
Parent[1] = -5
Parent[4] = 1
```

Meaning:

```text
New Set Size = 5
```

---

# 💻 Weighted Union Algorithm

```cpp
void Union(int u,int v)
{
    if(parent[u] < parent[v])
    {
        parent[u] += parent[v];
        parent[v] = u;
    }
    else
    {
        parent[v] += parent[u];
        parent[u] = v;
    }
}
```

---

## Why does this work?

Remember:

```text
-5 < -2
```

More negative means:

```text
Larger Set
```

Therefore:

```text
Attach smaller tree
under larger tree
```

---

# 🧪 Complete Example

Initially:

```text
1 2 3 4 5 6 7 8

-1 -1 -1 -1 -1 -1 -1 -1
```

---

### Union(1,2)

```text
-2 1 -1 -1 -1 -1 -1 -1
```

Tree:

```text
1
|
2
```

---

### Union(3,4)

```text
-2 1 -2 3 -1 -1 -1 -1
```

Tree:

```text
3
|
4
```

---

### Union(5,6)

```text
-2 1 -2 3 -2 5 -1 -1
```

---

### Union(7,8)

```text
-2 1 -2 3 -2 5 -2 7
```

---

### Union(2,4)

Find roots:

```text
Find(2) = 1
Find(4) = 3
```

Merge:

```text
-4 1 1 3 -2 5 -2 7
```

Tree:

```text
       1
      / \
     2   3
          \
           4
```

---

### Union(2,5)

Roots:

```text
1 and 5
```

Merge:

```text
-6 1 1 3 1 5 -2 7
```

Result:

```text
          1
       / / \ \
      2 3  5
        |   |
        4   6
```

---

# 🔄 Cycle Detection Using Disjoint Set

One of the most important applications.

---

## Graph

```text
1 ----- 2
|       |
|       |
4 ----- 3
```

Edges:

```text
(1,2)
(2,3)
(3,4)
(4,1)
```

---

## Algorithm

For every edge:

### Step 1

```cpp
Find(u)
Find(v)
```

---

### Step 2

If roots differ:

```cpp
Union(u,v)
```

---

### Step 3

If roots are same:

```text
Cycle Found 🚨
```

Because both vertices already belong to the same connected component.

---

## Example

### Edge (1,2)

Different sets

```text
Union
```

---

### Edge (2,3)

Different sets

```text
Union
```

---

### Edge (3,4)

Different sets

```text
Union
```

---

### Edge (4,1)

```text
Find(4) = 1
Find(1) = 1
```

Same root.

```text
Cycle Exists ✅
```

Visualization:

```text
1 ----- 2
|       |
|       |
4 ----- 3
```

The last edge closes the loop.

---

# ⏱ Complexity Analysis

| Operation | Simple Union | Weighted Union |
| --------- | ------------ | -------------- |
| Find      | O(n)         | O(log n)       |
| Union     | O(n)         | O(log n)       |

---

## Why Weighted Union Helps

Instead of:

```text
1
|
2
|
3
|
4
|
5
```

We get:

```text
        1
      / | \
     2  3  4
        |
        5
```

Much smaller height.

---

# 🚀 Applications

### Graph Algorithms

* Cycle Detection
* Connected Components
* Dynamic Connectivity

### Minimum Spanning Tree

Used extensively in:

### Kruskal's Algorithm

```text
Sort Edges
    ↓
Check Cycle using DSU
    ↓
Build MST
```

---

# 🎤 Interview Notes

### Frequently Asked Questions

### Q1. Why use negative values at roots?

To store:

```text
Set Size
```

without needing another array.

---

### Q2. Why weighted union?

To reduce tree height.

Smaller height:

```text
Faster Find
```

---

### Q3. Main applications?

* Kruskal's MST
* Cycle Detection
* Network Connectivity
* Social Grouping Problems

---

### Q4. What is the representative element?

The root node of the tree.

---

# 📝 Quick Revision Sheet

```text
Disjoint Set (Union-Find)
│
├── Find(x)
│      └── Finds root
│
├── Union(x,y)
│      └── Merges sets
│
├── Weighted Union
│      └── Smaller → Larger
│
├── Root Stores
│      └── Negative Size
│
└── Applications
       ├── Cycle Detection
       ├── Connected Components
       └── Kruskal MST
```

---

# 🧠 Memory Trick

```text
Find  → "Which family do I belong to?"

Union → "Let's create a bigger family."

Weighted Union
      → Keep the family tree short.

Same Root + New Edge
      → Cycle Detected 🚨
```

---

# 🎯 Final Takeaway

Disjoint Set Union (DSU) is one of the most powerful data structures for graph problems.

Remember:

✅ Find → locate representative

✅ Union → merge groups

✅ Weighted Union → keep trees shallow

✅ Same root while adding an edge → cycle exists

✅ Core component of Kruskal's MST

Master DSU once, and many graph problems become surprisingly easy.
