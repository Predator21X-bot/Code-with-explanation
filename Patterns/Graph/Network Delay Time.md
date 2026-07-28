# LeetCode 743 – Network Delay Time

**Difficulty:** Medium
**Pattern:** Graphs, Shortest Path, Dijkstra's Algorithm, Min Heap (Priority Queue)

---

# Problem Statement

You are given a directed, weighted graph where:

* `times[i] = [u, v, w]`
* A signal starts from node `k`.
* The signal takes `w` units of time to travel from `u` to `v`.

Return the **minimum time** required for **all nodes** to receive the signal.

If any node cannot receive the signal, return **-1**.

---

# Key Observations

* Graph is **Directed**.
* Edges have **weights**.
* Need the **shortest distance** from one source (`k`) to every node.
* Edge weights are **positive**.

This is a classic **Single Source Shortest Path** problem.

---

# Pattern Recognition

| Question            | Answer                   |
| ------------------- | ------------------------ |
| Graph?              | ✅ Yes                    |
| Directed?           | ✅ Yes                    |
| Weighted?           | ✅ Yes                    |
| Positive Weights?   | ✅ Yes                    |
| Need shortest path? | ✅ Yes                    |
| Algorithm           | **Dijkstra's Algorithm** |

---

# Why Not BFS?

BFS works only when every edge has the same cost.

Example:

```
A ----5----> B

A --1--> C --1--> B
```

BFS chooses the path with fewer edges.

Actual shortest path is determined by **minimum total weight**, not minimum edges.

Hence BFS fails.

---

# Why Dijkstra?

Dijkstra always expands the node having the **smallest current distance**.

Since all weights are positive, once the smallest distance is chosen, it is guaranteed to be optimal.

---

# Data Structures Used

## 1. Adjacency List

```cpp
vector<vector<pair<int,int>>> adj(n + 1);
```

Each pair stores

```cpp
{neighbor, weight}
```

Example

```
2 --> 1 (weight 3)
2 --> 4 (weight 5)
```

Stored as

```cpp
adj[2] = {
    {1,3},
    {4,5}
};
```

---

## 2. Distance Array

```cpp
vector<int> distance(n + 1, INT_MAX);
```

Meaning

```
distance[i]
=
Shortest known distance from source to node i
```

Initially

```
INF INF INF INF
```

Source node

```cpp
distance[k] = 0;
```

---

## 3. Min Heap

```cpp
priority_queue<
pair<int,int>,
vector<pair<int,int>>,
greater<pair<int,int>>
> pq;
```

Stores

```cpp
{distance, node}
```

Example

```
(2,4)
(5,1)
(8,3)
```

Top element

```
(2,4)
```

because it has the smallest distance.

---

# Algorithm

### Step 1

Build adjacency list.

```
times

↓

Adjacency List
```

---

### Step 2

Initialize

```
distance[source] = 0
```

Remaining nodes

```
INT_MAX
```

---

### Step 3

Insert source into min heap.

```
pq.push({0,k});
```

---

### Step 4

While heap is not empty

```
Take node having minimum distance

↓

Relax all neighbours

↓

Update shorter paths

↓

Push updated distance
```

---

### Step 5

After Dijkstra

If any node has

```
INT_MAX
```

return

```
-1
```

Otherwise

Return

```
Maximum(distance[])
```

because the signal reaches every node only when the **last node** receives it.

---

# Edge Relaxation

Suppose

```
Current node = 2

Current shortest distance = 5

Edge

2 ----3----> 4
```

Current distance to node 4

```
10
```

Possible new path

```
5 + 3 = 8
```

Since

```
8 < 10
```

Update

```cpp
distance[4] = 8;
```

and push

```cpp
pq.push({8,4});
```

This step is called **Edge Relaxation**.

---

# Stale Heap Entries

Sometimes heap contains

```
(6,4)

(8,4)
```

Suppose

```
distance[4] = 6
```

When

```
(8,4)
```

is popped,

it is already outdated.

Ignore it.

```cpp
if(dist > distance[node])
    continue;
```

This optimization removes the need for a **visited array**.

---

# Dry Run

Input

```
times =

2 -> 1 (1)

2 -> 3 (1)

3 -> 4 (1)

k = 2
```

Initial

```
distance

∞ 0 ∞ ∞
```

Heap

```
(0,2)
```

Process node 2

```
distance[1] = 1

distance[3] = 1
```

Heap

```
(1,1)

(1,3)
```

Process node 3

```
distance[4] = 2
```

Final distances

```
1

0

1

2
```

Maximum

```
2
```

Answer

```
2
```

---

# Complexity Analysis

### Time Complexity

Building graph

```
O(E)
```

Dijkstra

```
O((V + E) log V)
```

Overall

```
O((V + E) log V)
```

---

### Space Complexity

Adjacency List

```
O(E)
```

Distance Array

```
O(V)
```

Priority Queue

```
O(V)
```

Overall

```
O(V + E)
```

---

# Complete C++ Solution

```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {

        // Build adjacency list
        vector<vector<pair<int,int>>> adj(n + 1);

        for (auto &edge : times) {
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];

            adj[u].push_back({v, w});
        }

        // Distance array
        vector<int> distance(n + 1, INT_MAX);
        distance[k] = 0;

        // Min Heap -> {distance, node}
        priority_queue<
            pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>
        > pq;

        pq.push({0, k});

        while (!pq.empty()) {

            auto [dist, node] = pq.top();
            pq.pop();

            // Ignore stale entries
            if (dist > distance[node])
                continue;

            for (auto [neighbor, weight] : adj[node]) {

                int newDistance = dist + weight;

                if (newDistance < distance[neighbor]) {
                    distance[neighbor] = newDistance;
                    pq.push({newDistance, neighbor});
                }
            }
        }

        int answer = 0;

        for (int i = 1; i <= n; i++) {

            if (distance[i] == INT_MAX)
                return -1;

            answer = max(answer, distance[i]);
        }

        return answer;
    }
};
```

---

# Interview Takeaways

* Recognize **weighted + positive edges + shortest path** → **Dijkstra**.
* Store graph as an **adjacency list** (`{neighbor, weight}`).
* Use a **min heap** storing `{distance, node}`.
* **Relax** every outgoing edge.
* Skip **stale heap entries** using:

  ```cpp
  if (dist > distance[node]) continue;
  ```
* If any node remains unreachable (`INT_MAX`), return `-1`.
* Otherwise, return the **maximum shortest distance**, since the answer is the time taken for the **last node** to receive the signal.

---

## Similar Problems

* **LeetCode 743** – Network Delay Time ⭐
* **LeetCode 1631** – Path With Minimum Effort
* **LeetCode 1514** – Path with Maximum Probability
* **LeetCode 1976** – Number of Ways to Arrive at Destination
* **LeetCode 778** – Swim in Rising Water
* **LeetCode 787** – Cheapest Flights Within K Stops (Modified Dijkstra/BFS)
* **LeetCode 505** – The Maze II

These problems build directly on the Dijkstra concepts you learned here.
