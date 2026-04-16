# 🧩 Reorder Routes to Make All Paths Lead to City Zero

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)

---

## 🧠 Problem Understanding

We are given:

* `n` cities (0 to n-1)
* `n - 1` directed edges (roads)

👉 Goal:

* Reorder minimum number of edges so that **every city can reach city 0**

---

## 🔍 Key Observations

* `n` nodes and `n-1` edges → **tree**
* Tree is:

  * Connected ✅
  * No cycles ❌

👉 So:

> There is exactly one path between any two nodes

---

## 🧠 Core Idea

We need to:

> Count how many edges are pointing in the **wrong direction**

---

## 🔥 Genius Trick (IMPORTANT)

👉 Convert direction into **cost**

For each edge `a → b`:

```cpp id="adj-build"
adj[a].push_back({b, 1}); // wrong direction
adj[b].push_back({a, 0}); // correct direction
```

---

### 💡 Meaning

| Edge  | Cost | Meaning         |
| ----- | ---- | --------------- |
| a → b | 1    | needs reversal  |
| b → a | 0    | already correct |

---

## 🧠 Why this works

👉 Start DFS from node `0`

* If we follow edge with cost `1` → wrong → count++
* If cost `0` → correct → ignore

---

## 💻 Code

```cpp id="main-code"
class Solution {
public:
    void dfs(int node, vector<vector<pair<int, int>>>& adj,
             vector<bool>& visited, int& count) {
        for (auto& neighbor : adj[node]) {
            int next = neighbor.first;
            int cost = neighbor.second;

            if (!visited[next]) {
                count += cost;
                visited[next] = true;
                dfs(next, adj, visited, count);
            }
        }
    }

    int minReorder(int n, vector<vector<int>>& connections) {
        vector<vector<pair<int, int>>> adj(n);

        for (auto& edge : connections) {
            int a = edge[0];
            int b = edge[1];

            adj[a].push_back({b, 1});
            adj[b].push_back({a, 0});
        }

        vector<bool> visited(n, false);
        int count = 0;

        visited[0] = true;
        dfs(0, adj, visited, count);

        return count;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

## 🧠 Dry Run Example

Input:

```cpp id="dry1"
n = 3
connections = [[0,1],[1,2]]
```

Graph:

```text id="dry2"
0 → 1 → 2
```

---

## DFS from 0

* 0 → 1 → cost = 1 → reverse needed
* 1 → 2 → cost = 1 → reverse needed

👉 Answer = **2**

---

## ❗ Common Mistakes

* Not converting to bidirectional graph ❌
* Forgetting to track visited ❌
* Not understanding cost encoding ❌
* Trying to reverse edges manually ❌

---

## 🧠 Patterns Learned

* Tree traversal
* DFS with state
* Encoding direction into weight
* Counting during traversal

---

## 🏷 Tags

* Graph
* DFS
* Tree
* Greedy Insight

---

## 🧠 When to Use This

* When graph is:

  * Tree structure
  * Directed but needs reorientation
* When problem asks:

  * “minimum changes”
  * “fix directions”

---

## 🔥 Mental Model (Revision Key)

> “Start from 0 → every wrong arrow → +1”

---

## ⚡ Key Insight

* Don’t fix edges
* Just **count wrong ones**

---

## 🚀 Takeaway

* Transform problem → simplify logic
* Use DFS to accumulate results
* Encode direction as cost
