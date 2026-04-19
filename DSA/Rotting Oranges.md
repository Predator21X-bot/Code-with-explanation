# 🧩 Rotting Oranges

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/rotting-oranges/](https://leetcode.com/problems/rotting-oranges/)

---

# 🧠 Problem Understanding

Grid contains:

* `0` → empty
* `1` → fresh orange
* `2` → rotten orange

👉 Every minute:

* Rotten oranges infect adjacent fresh ones (4 directions)

---

## 🎯 Goal

Return:

* Minimum time to rot all oranges
* `-1` if impossible

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Model the problem

👉 Grid = Graph

* Cell = node
* Adjacent cells = edges

---

## 🧩 Step 2: Identify pattern

Ask:

> Is this shortest path or spread problem?

👉 This is:

> **Spread over time**

---

## 🧩 Step 3: Key observation

> All rotten oranges spread **simultaneously**

👉 So:

> **Multiple starting points → Multi-source BFS**

---

## 🧩 Step 4: What does BFS level mean?

👉 Each level = **1 minute**

---

## 🧩 Step 5: What do we track?

* Queue → rotten oranges
* Count → fresh oranges

---

# ⚡ Approach: Multi-source BFS

---

## 🧠 Step 1: Initialize

```cpp id="init"
queue<pair<int,int>> q;
int count = 0;

for each cell:
    if 2 → push into queue
    if 1 → count++
```

---

## 🧠 Step 2: Directions

```cpp id="dirs"
vector<pair<int,int>> dirs = {
    {0,1}, {1,0}, {-1,0}, {0,-1}
};
```

---

## 🧠 Step 3: BFS traversal

```cpp id="bfs"
while (!q.empty()) {
    int size = q.size();

    for each element in current level:
        explore 4 directions
        if fresh → rot it
```

---

## 🧠 Step 4: Rotting logic

```cpp id="rot"
if valid cell && grid[nr][nc] == 1:
    grid[nr][nc] = 2
    q.push({nr, nc})
    count--
```

---

## 🧠 Step 5: Time tracking

```cpp id="time"
if (!q.empty()) minutes++;
```

👉 Only increment if next level exists

---

## 🧠 Step 6: Final check

```cpp id="result"
return count == 0 ? minutes : -1;
```

---

# 💻 Code

```cpp id="code"
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        queue<pair<int,int>> q;
        int count = 0;
        int minutes = 0;

        int rows = grid.size();
        int cols = grid[0].size();

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == 2)
                    q.push({i, j});
                else if (grid[i][j] == 1)
                    count++;
            }
        }

        vector<pair<int,int>> dirs = {
            {0,1}, {1,0}, {-1,0}, {0,-1}
        };

        while (!q.empty()) {
            int size = q.size();

            for (int i = 0; i < size; i++) {
                auto current = q.front();
                q.pop();

                int r = current.first;
                int c = current.second;

                for (auto dir : dirs) {
                    int nr = r + dir.first;
                    int nc = c + dir.second;

                    if (nr >= 0 && nc >= 0 &&
                        nr < rows && nc < cols &&
                        grid[nr][nc] == 1) {

                        grid[nr][nc] = 2;
                        q.push({nr, nc});
                        count--;
                    }
                }
            }

            if (!q.empty()) minutes++;
        }

        return count == 0 ? minutes : -1;
    }
};
```

---

# ⏱ Complexity

* Time: **O(rows × cols)**
* Space: **O(rows × cols)**

---

# ❗ Common Mistakes

* ❌ Using DFS (wrong for shortest time)
* ❌ Incrementing minutes per node instead of per level
* ❌ Not decreasing fresh count
* ❌ Not pushing newly rotten nodes
* ❌ Boundary mistakes

---

# 🧠 Patterns Learned

* Multi-source BFS
* Level-based time tracking
* Grid traversal

---

# 🏷 Tags

* Graph
* BFS
* Grid

---

# 🔥 Mental Model (REVISION KEY)

> “All rotten spread together → each BFS layer = 1 minute”

---

# ⚡ Recognition Pattern

Use **Multi-source BFS** when you see:

* infection
* spreading
* burning
* minimum time
* multiple starting points

---

# 🧠 BFS vs Multi-source BFS

| Type             | Start      | Use                  |
| ---------------- | ---------- | -------------------- |
| BFS              | one node   | shortest path        |
| Multi-source BFS | many nodes | spread/time problems |

---

# 🚀 Takeaway

* Think in terms of **waves/levels**
* Don’t simulate step-by-step — use BFS
