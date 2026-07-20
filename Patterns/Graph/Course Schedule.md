# 📘 LeetCode 207: Course Schedule

## 🔗 Problem

You are given `numCourses` and a list of prerequisite pairs.

Each prerequisite `[a, b]` means:

> To take course `a`, you must first complete course `b`.

Determine whether it is possible to finish all courses.

---

# 🧠 Pattern Recognition

```
Courses
    ↓
Directed Graph
    ↓
Need to determine if every course can be completed
    ↓
Equivalent to checking:
"Does the graph contain a cycle?"
```

### Key Insight

A cycle means:

```
A requires B
B requires C
C requires A
```

No course in the cycle can ever be completed.

So the problem becomes:

> **Detect a cycle in a Directed Graph.**

---

# 🏗 Graph Representation

Given:

```cpp
prerequisites = {
    {1,0},
    {2,0},
    {3,1}
};
```

Remember:

```
[a, b]
```

means

```
b → a
```

Therefore,

```
0 → 1
0 → 2
1 → 3
```

Adjacency List:

```cpp
graph[0] = {1,2};
graph[1] = {3};
graph[2] = {};
graph[3] = {};
```

Construction:

```cpp
for (const auto& pre : prerequisites)
{
    graph[pre[1]].push_back(pre[0]);
}
```

---

# 🚫 Why a Single Visited Array Doesn't Work

Consider:

```
    0
   / \
  1   2
   \ /
    3
```

Node `3` is reached twice.

If we only use:

```cpp
visited[3] = true;
```

The second visit would incorrectly appear to be a cycle.

It is **not** a cycle.

---

# ✅ Three-State DFS

Instead of one boolean array, use three states.

```
0 → Unvisited
1 → Visiting
2 → Visited
```

Meaning:

### 0 — Unvisited

```
Never explored.
```

---

### 1 — Visiting

```
Currently inside recursion stack.
```

Example:

```
0
↓
1
↓
2
```

Current recursion stack:

```
0
1
2
```

If `2` reaches `1` again:

```
0
↓
1
↓
2
↑
└──────
```

Node `1` is still **Visiting**.

Cycle detected.

---

### 2 — Visited

Means:

```
Finished exploring all descendants.
```

No cycle exists from this node.

Future DFS calls can safely return immediately.

---

# 🚀 DFS Algorithm

### Step 1

Already Visiting?

```cpp
if(state[course] == 1)
    return false;
```

A back edge is found.

Cycle exists.

---

### Step 2

Already Visited?

```cpp
if(state[course] == 2)
    return true;
```

No need to explore again.

---

### Step 3

Mark as Visiting

```cpp
state[course] = 1;
```

---

### Step 4

Explore every neighbor

```cpp
for(int neighbor : graph[course])
{
    if(!dfs(neighbor))
        return false;
}
```

If any neighbor detects a cycle,

propagate `false` upward.

---

### Step 5

Finished exploring

```cpp
state[course] = 2;
```

---

### Step 6

No cycle

```cpp
return true;
```

---

# 🌍 Why Start DFS From Every Course?

Consider:

```
0 → 1

2 → 3
↑   ↓
└───┘
```

If we only call:

```cpp
dfs(0);
```

We never visit

```
2
3
```

and miss the cycle.

Instead:

```cpp
for(int course = 0; course < numCourses; course++)
{
    if(!dfs(course))
        return false;
}
```

This ensures every connected component is explored.

---

# 💻 Final Code

```cpp
class Solution {
private:
    vector<vector<int>> graph;
    vector<int> state;   // 0 = Unvisited, 1 = Visiting, 2 = Visited

    bool dfs(int course) {
        if (state[course] == 1)
            return false;

        if (state[course] == 2)
            return true;

        state[course] = 1;

        for (int neighbor : graph[course]) {
            if (!dfs(neighbor))
                return false;
        }

        state[course] = 2;
        return true;
    }

public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        graph.resize(numCourses);
        state.assign(numCourses, 0);

        for (const auto& pre : prerequisites) {
            graph[pre[1]].push_back(pre[0]);
        }

        for (int course = 0; course < numCourses; course++) {
            if (!dfs(course))
                return false;
        }

        return true;
    }
};
```

---

# ⏱ Complexity Analysis

### Time Complexity

Building graph:

```
O(E)
```

DFS:

```
O(V + E)
```

Overall:

```
O(V + E)
```

Where:

* `V = numCourses`
* `E = prerequisites.size()`

---

### Space Complexity

Adjacency List:

```
O(V + E)
```

State Array:

```
O(V)
```

Recursion Stack (worst case):

```
O(V)
```

Overall:

```
O(V + E)
```

---

# 💡 C++ Notes Learned

## Why `const auto&`?

Given:

```cpp
vector<vector<int>> prerequisites;
```

Each element is a `vector<int>`.

### ❌ Using `auto`

```cpp
for (auto pre : prerequisites)
```

Copies every `vector<int>`.

---

### ✅ Using `auto&`

```cpp
for (auto& pre : prerequisites)
```

No copy.

`pre` refers directly to the original vector.

---

### ✅ Best Practice

```cpp
for (const auto& pre : prerequisites)
```

* No copying
* Cannot accidentally modify the original vector
* More efficient for large objects

Equivalent to:

```cpp
for (const vector<int>& pre : prerequisites)
```

---

# 🔑 Key Takeaways

* Convert prerequisites into a **directed graph**.
* `[a, b]` means `b → a`.
* A cycle means it's impossible to finish all courses.
* A boolean `visited` array is **not enough** for directed graphs.
* Use a **3-state DFS**:

  * `0` → Unvisited
  * `1` → Visiting (currently in recursion stack)
  * `2` → Visited (fully processed)
* Encountering a **Visiting** node again means a cycle exists.
* Mark a node as **Visited** only after exploring all its neighbors.
* Start DFS from **every course** to handle disconnected components.
* Prefer `const auto&` in range-based loops when reading large objects to avoid unnecessary copies while preventing accidental modification.
