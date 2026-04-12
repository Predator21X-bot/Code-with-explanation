# 📘 Number of Provinces (DFS + Union-Find)
---

# 🧩 Number of Provinces

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/number-of-provinces/](https://leetcode.com/problems/number-of-provinces/)

---

## 🧠 Problem Understanding

We are given:

* `n x n` matrix `isConnected`
* `isConnected[i][j] = 1` → city `i` is connected to city `j`

👉 Goal:

* Find number of **provinces**

  * A province = group of directly or indirectly connected cities

---

## 🔍 Key Observations

* Cities = nodes
* Connections = edges
* This is a **graph problem**
* Matrix = **adjacency matrix**

---

## 🧠 Adjacency Matrix Intuition

```cpp
isConnected[i][j] == 1
```

👉 Means:

> City `i` is connected to city `j`

---

### 💡 Mental Model

* Row `i` → neighbors of city `i`
* Scan entire row to find connections

---

# ⚡ Approach 1: DFS (Recommended)

---

## 🧠 Core Idea

1. Maintain `visited[]`
2. Loop through all cities:

   * If not visited:

     * Run DFS
     * Increment province count

---

## 💻 Code (DFS)

```cpp
class Solution {
public:
    void dfs(int current, vector<vector<int>>& isConnected, vector<bool>& visited) {
        visited[current] = true;

        int n = isConnected.size();

        for (int j = 0; j < n; j++) {
            if (isConnected[current][j] == 1 && !visited[j]) {
                dfs(j, isConnected, visited);
            }
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<bool> visited(n, false);

        int count = 0;

        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                dfs(i, isConnected, visited);
                count++;
            }
        }

        return count;
    }
};
```

---

## ⏱ Complexity (DFS)

* Time: **O(n²)**
* Space: **O(n)**

---

## 🧠 Why it works

* Each DFS call explores **one full component**
* Every unvisited node = new province

---

# ⚡ Approach 2: Union-Find (Disjoint Set)

---

## 🧠 Core Idea

* Each city starts as its own parent
* Merge cities if connected
* Count remaining groups

---

## 💻 Code (Union-Find)

```cpp
class Solution {
public:
    int find(int x, vector<int>& parent) {
        if (parent[x] != x)
            parent[x] = find(parent[x], parent); // path compression
        return parent[x];
    }

    void unite(int a, int b, vector<int>& parent) {
        int pa = find(a, parent);
        int pb = find(b, parent);

        if (pa != pb) {
            parent[pa] = pb;
        }
    }

    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        vector<int> parent(n);

        for (int i = 0; i < n; i++)
            parent[i] = i;

        int count = n;

        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (isConnected[i][j] == 1) {
                    int pi = find(i, parent);
                    int pj = find(j, parent);

                    if (pi != pj) {
                        unite(i, j, parent);
                        count--;
                    }
                }
            }
        }

        return count;
    }
};
```

---

## ⏱ Complexity (Union-Find)

* Time: **O(n² · α(n))** ≈ O(n²)
* Space: **O(n)**

---

## 🧠 Why it works

* Initially: each node = separate group
* Each union merges groups
* `count--` tracks number of components

---

# 🔥 Key Insight (VERY IMPORTANT)

```cpp
if (pi != pj)
```

👉 Means:

> Cities belong to different provinces → merge them

---

# ⚖️ DFS vs Union-Find

| Feature   | DFS            | Union-Find          |
| --------- | -------------- | ------------------- |
| Idea      | Traverse graph | Merge sets          |
| Intuition | Easier         | Harder initially    |
| Code      | Simple         | Slightly complex    |
| Use case  | Static graph   | Dynamic connections |

---

# ❗ Common Mistakes

### DFS:

* Calling `dfs(0)` instead of `dfs(i)`
* Not using visited
* Confusing matrix indices

### Union-Find:

* Forgetting `find()` before union
* Not checking `pi != pj`
* Missing `count--`

---

# 🧠 Patterns Learned

* Connected components
* Graph traversal
* Disjoint Set (Union-Find)

---

# 🏷 Tags

* Graph
* DFS
* Union-Find
* Matrix

---

# 🧠 When to Use

👉 Use this pattern when:

* “Number of groups”
* “Connected components”
* “Clusters in graph”

---

# 🔥 Mental Model (Revision Key)

### DFS:

> “Unvisited node → explore fully → count++”

### Union-Find:

> “Everyone separate → merge connected → count groups”

---

# 🚀 Takeaway

* DFS = go-to solution
* Union-Find = advanced + powerful
* Both solve same problem differently
