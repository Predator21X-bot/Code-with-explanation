# 🧩 Nearest Exit from Entrance in Maze

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/)

---

# 🧠 Problem Understanding

You are given:

* A grid `maze`
* `'.'` → empty cell
* `'+'` → wall
* An `entrance`

👉 Goal:

> Find the **minimum steps to reach nearest exit**

---

## 🧠 What is an exit?

👉 A cell is an exit if:

* It is on **boundary**
* It is **NOT the entrance**

---

# 🔍 Key Observations

* Grid = graph
* Each cell = node
* Movement = edges (up/down/left/right)
* All edges weight = 1

👉 So:

> **Shortest path in unweighted graph → BFS**

---

# ⚡ Approach: BFS

---

## 🧠 Why BFS?

> BFS guarantees **shortest path** in unweighted graphs

---

## 🧠 Data structure

```cpp id="queue-type"
queue<pair<pair<int,int>, int>> q;
```

👉 Stores:

```text id="df1i3l"
((row, col), steps)
```

---

## 🧠 Directions

```cpp id="dirs"
vector<pair<int,int>> dirs = {
    {0,1}, {1,0}, {-1,0}, {0,-1}
};
```

---

# 🧠 Algorithm

---

## 🔹 Step 1: Initialize

```cpp id="init"
q.push({{entrance[0], entrance[1]}, 0});
maze[entrance[0]][entrance[1]] = '+';
```

---

## 🔹 Step 2: BFS Loop

```cpp id="loop"
while (!q.empty()) {
    auto current = q.front();
    q.pop();
}
```

---

## 🔹 Step 3: Explore neighbors

For each direction:

```cpp id="neighbors"
nr = r + dr
nc = c + dc
```

---

## 🔹 Step 4: Validity check

```cpp id="valid"
nr >= 0 && nc >= 0 &&
nr < rows && nc < cols &&
maze[nr][nc] == '.'
```

---

## 🔹 Step 5: Exit check

```cpp id="exit-check"
nr == 0 || nc == 0 ||
nr == rows-1 || nc == cols-1
```

👉 If true:

```cpp id="return"
return steps + 1;
```

---

## 🔹 Step 6: Continue BFS

```cpp id="continue"
maze[nr][nc] = '+';
q.push({{nr, nc}, steps + 1});
```

---

## 🔹 Step 7: No exit found

```cpp id="fail"
return -1;
```

---

# 💻 Final Code

```cpp id="final-code"
class Solution {
public:
    int nearestExit(vector<vector<char>>& maze, vector<int>& entrance) {
        int rows = maze.size();
        int cols = maze[0].size();

        queue<pair<pair<int,int>, int>> q;
        q.push({{entrance[0], entrance[1]}, 0});
        maze[entrance[0]][entrance[1]] = '+';

        vector<pair<int,int>> dirs = {
            {0,1}, {1,0}, {-1,0}, {0,-1}
        };

        while (!q.empty()) {
            auto current = q.front();
            q.pop();

            int r = current.first.first;
            int c = current.first.second;
            int steps = current.second;

            for (auto &d : dirs) {
                int nr = r + d.first;
                int nc = c + d.second;

                if (nr >= 0 && nc >= 0 &&
                    nr < rows && nc < cols &&
                    maze[nr][nc] == '.') {

                    if (nr == 0 || nc == 0 ||
                        nr == rows-1 || nc == cols-1) {
                        return steps + 1;
                    }

                    maze[nr][nc] = '+';
                    q.push({{nr, nc}, steps + 1});
                }
            }
        }

        return -1;
    }
};
```

---

# ⏱ Complexity

* Time: **O(rows × cols)**
* Space: **O(rows × cols)**

---

# ❗ Common Mistakes

* ❌ Using DFS (won’t guarantee shortest path)
* ❌ Not marking visited
* ❌ Checking wrong cell (`r,c` instead of `nr,nc`)
* ❌ Forgetting entrance is NOT exit
* ❌ Boundary condition mistakes

---

# 🧠 Patterns Learned

* BFS on grid
* Shortest path in unweighted graph
* Multi-direction traversal

---

# 🏷 Tags

* Graph
* BFS
* Grid

---

# 🔥 Mental Model (VERY IMPORTANT)

> “From each cell → try all 4 directions → stop at first valid exit”

---

# ⚡ BFS Template (REUSABLE)

```text id="template"
1. Push start
2. Mark visited
3. While queue:
   - pop
   - explore neighbors
   - check validity
   - check goal
   - mark visited
   - push
```

---

# 🧠 BFS vs DFS (REVISION GOLD)

| Problem                    | Use  |
| -------------------------- | ---- |
| shortest path (unweighted) | BFS  |
| explore all paths          | DFS  |
| path existence             | both |
| minimum steps              | BFS  |

---

# 🚀 Takeaway

* Always identify:

  * graph type
  * weight type
  * required output
