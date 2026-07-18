# Number of Islands (LeetCode 200)

## Problem Statement

Given an `m x n` 2D binary grid where:

- `'1'` represents **land**
- `'0'` represents **water**

Return the **number of islands**.

An island is formed by connecting adjacent lands **horizontally or vertically** (not diagonally).

---

## Pattern Recognition

### Key Observation

The grid can be viewed as a **graph**.

- Every `'1'` is a node.
- Each node is connected to its **4 neighbors**:
  - Up
  - Down
  - Left
  - Right

We need to count the number of **connected components** in the graph.

### Pattern

```
Grid
   ↓
Graph
   ↓
Connected Components
   ↓
DFS / BFS
```

---

# Intuition

We traverse every cell in the grid.

Whenever we encounter an **unvisited land** (`'1'`):

- We've discovered a **new island**.
- Increment the island count.
- Perform a DFS to visit every connected land cell.
- Mark each visited land as `'0'` so it won't be counted again.

After DFS completes, the entire island has been explored.

Continue scanning the grid for the next unvisited island.

---

# Why mark visited cells as `'0'`?

Suppose the grid is:

```
1 1
1 1
```

Starting DFS from the top-left visits all four cells.

If we don't mark visited cells:

- We may revisit the same cells repeatedly.
- We may count the same island multiple times.

By changing

```cpp
grid[i][j] = '0';
```

we "sink" the island after visiting it.

---

# Dry Run

Input

```
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

Initially

```
count = 0
```

---

### Scan begins

First land found

```
(0,0)
```

Increment

```
count = 1
```

Run DFS.

DFS visits

```
(0,0)
(0,1)
(1,0)
(1,1)
```

Grid becomes

```
0 0 0 0 0
0 0 0 0 0
0 0 1 0 0
0 0 0 1 1
```

---

Continue scanning.

Next land

```
(2,2)
```

Increment

```
count = 2
```

DFS visits only that cell.

---

Continue scanning.

Next land

```
(3,3)
```

Increment

```
count = 3
```

DFS visits

```
(3,3)
(3,4)
```

Entire grid becomes water.

Answer

```
3
```

---

# Algorithm

## DFS

### Base Case

Stop if:

- Row is outside the grid.
- Column is outside the grid.
- Current cell is water (`'0'`).

### Recursive Step

1. Mark current land as visited.

```cpp
grid[i][j] = '0';
```

2. Visit all four neighbors.

```cpp
dfs(up)
dfs(down)
dfs(left)
dfs(right)
```

---

## Main Function

1. Traverse every cell.
2. If current cell is `'1'`:
   - Increment island count.
   - Run DFS.
3. Return island count.

---

# Code

```cpp
class Solution {
public:

    void dfs(vector<vector<char>>& grid, int i, int j) {

        int rows = grid.size();
        int cols = grid[0].size();

        if (i < 0 || i >= rows ||
            j < 0 || j >= cols ||
            grid[i][j] == '0') {
            return;
        }

        // Mark visited
        grid[i][j] = '0';

        // Explore all four directions
        dfs(grid, i - 1, j); // Up
        dfs(grid, i + 1, j); // Down
        dfs(grid, i, j - 1); // Left
        dfs(grid, i, j + 1); // Right
    }

    int numIslands(vector<vector<char>>& grid) {

        if (grid.empty())
            return 0;

        int m = grid.size();
        int n = grid[0].size();

        int count = 0;

        for (int i = 0; i < m; i++) {

            for (int j = 0; j < n; j++) {

                if (grid[i][j] == '1') {

                    count++;

                    dfs(grid, i, j);
                }
            }
        }

        return count;
    }
};
```

---

# Complexity Analysis

### Time Complexity

Each cell is visited **at most once**.

```
O(m × n)
```

---

### Space Complexity

Recursive DFS stack in the worst case:

```
O(m × n)
```

Worst case occurs when the entire grid is one large island.

If using iterative BFS instead of recursive DFS:

- Queue space is also `O(m × n)` in the worst case.

---

# Key Takeaways

- A 2D grid can be modeled as a **graph**.
- Every land cell (`'1'`) is a graph node.
- We need to count **connected components**.
- DFS/BFS explores an entire island from one starting cell.
- Increment the island count **only when a new unvisited `'1'` is found**, not inside DFS.
- Mark visited cells as `'0'` to avoid revisiting and recounting the same island.
- DFS follows the classic pattern:
  - Base case
  - Mark visited
  - Explore 4 directions
- Overall complexity is **O(m × n)** since each cell is processed only once.
