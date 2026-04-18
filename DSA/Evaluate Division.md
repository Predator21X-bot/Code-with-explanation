# 📘 Evaluate Division (Graph + DFS)

(File: `Graphs/evaluate-division.md`)

---

# 🧩 Evaluate Division

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/evaluate-division/](https://leetcode.com/problems/evaluate-division/)

---

# 🧠 Problem Understanding

We are given:

* Equations like:

```text
a / b = 2.0
b / c = 3.0
```

👉 Queries:

```text
a / c = ?
c / a = ?
```

---

## 🎯 Goal

Compute the result for each query or return `-1.0` if not possible.

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Model the problem

Ask:

> Can this be represented as a graph?

👉 YES

* Variables → nodes
* Equations → edges
* Values → weights

---

## 🔗 Graph Representation

For:

```text
a / b = 2.0
```

We store:

```text
a → b (2.0)
b → a (1/2.0)
```

👉 Why reverse edge?
Because:

```text
b / a = 1 / (a / b)
```

---

## 🧠 Step 2: What is the query asking?

```text
a / c = ?
```

👉 Means:

> Find a path from `a → c`

---

## 🧠 Step 3: What operation along path?

👉 Multiply weights:

```text
a → b → c
2 × 3 = 6
```

---

## 🧠 Step 4: Choose traversal

* Need any path → DFS / BFS
* No shortest constraint → DFS works

---

## 🧠 Step 5: What does DFS carry?

👉 We carry:

```text
current product
```

---

## 🧠 Step 6: Base case

```cpp
if (current == target)
    return product;
```

---

## 🧠 Step 7: Handle cycles

👉 Use `visited`

---

# ⚡ Approach

1. Build graph
2. For each query:

   * Run DFS
   * Multiply weights along path

---

# 💻 Code

```cpp
class Solution {
public:
    double dfs(string current, string target,
               unordered_map<string, vector<pair<string, double>>>& graph,
               unordered_set<string>& visited,
               double product) {

        if (current == target)
            return product;

        visited.insert(current);

        for (auto& neighbor : graph[current]) {
            string next = neighbor.first;
            double weight = neighbor.second;

            if (!visited.count(next)) {
                double result =
                    dfs(next, target, graph, visited, product * weight);

                if (result != -1.0)
                    return result;
            }
        }
        return -1.0;
    }

    vector<double> calcEquation(vector<vector<string>>& equations,
                                vector<double>& values,
                                vector<vector<string>>& queries) {

        unordered_map<string, vector<pair<string, double>>> graph;

        // Build graph
        for (int i = 0; i < equations.size(); i++) {
            string a = equations[i][0];
            string b = equations[i][1];
            double value = values[i];

            graph[a].push_back({b, value});
            graph[b].push_back({a, 1.0 / value});
        }

        vector<double> result;

        // Process queries
        for (auto& q : queries) {
            string src = q[0];
            string dest = q[1];

            if (!graph.count(src) || !graph.count(dest)) {
                result.push_back(-1.0);
                continue;
            }

            unordered_set<string> visited;
            result.push_back(dfs(src, dest, graph, visited, 1.0));
        }

        return result;
    }
};
```

---

# ⏱ Complexity

* Time: **O(N + Q * (V + E))**
* Space: **O(V + E)**

---

# ❗ Common Mistakes

* Forgetting reverse edge ❌
* Not using visited → infinite loop ❌
* Returning wrong type (bool instead of double) ❌
* Not checking if variable exists ❌

---

# 🧠 Patterns Learned

* Graph modeling
* Weighted DFS
* Path-based computation

---

# 🏷 Tags

* Graph
* DFS
* Weighted Graph

---

# 🧠 When to Use This

* When:

  * relationships can be chained
  * values propagate along path
  * queries ask indirect relations

---

# 🔥 Mental Model (Revision Key)

> “Convert to graph → follow path → multiply weights”

---

# ⚡ Universal Graph Thinking Template (VERY IMPORTANT)

Before coding, ask:

1. What are nodes?
2. What are edges?
3. Is graph weighted?
4. What do I compute along path?
5. Which traversal?
6. What is base case?

---

# 🚀 Takeaway

* Don’t jump to code
* First build **model + logic**
* Then implement
