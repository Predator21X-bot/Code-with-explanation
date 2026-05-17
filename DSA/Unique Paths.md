# Unique Paths

## Problem Summary

We are given a:

```text id="m1r8v4"
m x n grid
```

A robot starts at:

```text id="q7m2k5"
top-left corner
```

and wants to reach:

```text id="x4m9r1"
bottom-right corner
```

---

# Allowed Moves

Robot can ONLY move:

## Right

```text id="t8m1k6"
→
```

OR

## Down

```text id="f5m7r2"
↓
```

---

# Goal

Find:

# total unique paths

from start to destination.

---

# Example 1

## Input

```text id="p2m8v4"
m = 3
n = 2
```

Grid:

```text id="v6m1r9"
⬜ ⬜
⬜ ⬜
⬜ ⬜
```

---

# Possible Paths

## Path 1

```text id="m4r8k1"
↓ ↓ →
```

---

## Path 2

```text id="q9m2v7"
↓ → ↓
```

---

## Path 3

```text id="x3m7r5"
→ ↓ ↓
```

Answer:

```text id="f8m1k2"
3
```

---

# MOST Important Observation

To reach any cell:

```text id="p5m9r1"
robot must come from:
top
OR
left
```

because allowed moves are:

```text id="t2m7v4"
down or right
```

This observation is EVERYTHING.

---

# Important DP Definition

Define:

```text id="n8m4r6"
dp[i][j]
```

as:

# number of unique ways to reach cell (i,j)

Very important definition.

---

# Key Recurrence

To reach current cell `(i,j)`:

* come from top
  OR
* come from left

So recurrence becomes:

dp[i][j]=dp[i-1][j]+dp[i][j-1]

---

# Why Addition?

Because:

```text id="r4m1k8"
all paths from top
+
all paths from left
```

gives total paths to current cell.

---

# Base Cases

## First Row

Robot can ONLY move:

```text id="q7m5v2"
right
```

So every cell in first row has exactly:

```text id="x1m8r4"
1 path
```

---

# First Column

Robot can ONLY move:

```text id="m9r2k5"
down
```

So every cell in first column also has exactly:

```text id="v5m7r1"
1 path
```

---

# Initialize Base Cases

```cpp id="f2m8v6"
dp[i][0] = 1;
dp[0][j] = 1;
```

---

# Visual Understanding

Suppose:

```text id="t4m1r9"
m = 3
n = 3
```

Initial DP grid:

```text id="q8m5r2"
1 1 1
1 _ _
1 _ _
```

---

# Compute Cell (1,1)

From:

* top = 1
* left = 1

So:

```text id="x6m1v7"
2
```

Grid becomes:

```text id="m3r8k1"
1 1 1
1 2 _
1 _ _
```

---

# Compute Cell (1,2)

From:

* top = 1
* left = 2

So:

```text id="p7m2v4"
3
```

Grid becomes:

```text id="f4m9r2"
1 1 1
1 2 3
1 _ _
```

---

# Continue Filling

Final grid:

```text id="q1m7k8"
1 1 1
1 2 3
1 3 6
```

Final answer:

```text id="v8m4r1"
6
```

---

# Dry Run

## Input

```text id="n5m2v7"
m = 3
n = 3
```

---

# Step 1 — Create DP Matrix

```cpp id="x2m8r5"
vector<vector<int>> dp(
    m,
    vector<int>(n,0)
);
```

Initial:

```text id="m6r1k9"
0 0 0
0 0 0
0 0 0
```

---

# Step 2 — Initialize First Column

```cpp id="t8m4v2"
for(int i = 0; i < m; i++){
    dp[i][0] = 1;
}
```

Result:

```text id="q3m7r1"
1 0 0
1 0 0
1 0 0
```

---

# Step 3 — Initialize First Row

```cpp id="v9m2k4"
for(int j = 0; j < n; j++){
    dp[0][j] = 1;
}
```

Result:

```text id="f1m8r6"
1 1 1
1 0 0
1 0 0
```

---

# Step 4 — Fill Remaining Cells

Use recurrence:

dp[i][j]=dp[i-1][j]+dp[i][j-1]

---

# Compute dp[1][1]

```text id="p4m7v2"
1 + 1 = 2
```

---

# Compute dp[1][2]

```text id="x7m1r5"
1 + 2 = 3
```

---

# Compute dp[2][1]

```text id="m2r9k8"
2 + 1 = 3
```

---

# Compute dp[2][2]

```text id="q5m8v1"
3 + 3 = 6
```

Final grid:

```text id="t1m7r4"
1 1 1
1 2 3
1 3 6
```

Answer:

```text id="f8m2k5"
6
```

---

# Complete DP Solution

```cpp id="v4m9r1"
class Solution {
public:

    int uniquePaths(int m, int n) {

        vector<vector<int>> dp(
            m,
            vector<int>(n,0)
        );

        // First column
        for(int i = 0; i < m; i++){
            dp[i][0] = 1;
        }

        // First row
        for(int j = 0; j < n; j++){
            dp[0][j] = 1;
        }

        // Fill remaining cells
        for(int i = 1; i < m; i++){

            for(int j = 1; j < n; j++){

                dp[i][j] =
                dp[i - 1][j]
                +
                dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }
};
```

---

# Complexity Analysis

## Time Complexity

We visit every cell once.

```text id="n6m1v8"
O(m × n)
```

---

## Space Complexity

DP table stores:

```text id="x5m2r7"
m × n
```

values.

So:

```text id="k4m9v2"
O(m × n)
```

---

# Space Optimization

Observe recurrence:

dp[i][j]\text{ depends only on top and left cells}

So we can optimize space using:

# 1D DP

---

# Optimized 1D DP Idea

Instead of full matrix:

store only current row.

---

# Optimized Code

```cpp id="w8m1r3"
class Solution {
public:

    int uniquePaths(int m, int n) {

        vector<int> dp(n,1);

        for(int i = 1; i < m; i++){

            for(int j = 1; j < n; j++){

                dp[j] =
                dp[j]
                +
                dp[j - 1];
            }
        }

        return dp[n - 1];
    }
};
```

---

# Why Optimization Works

At cell `(i,j)`:

```text id="q6m2v7"
dp[j]
```

stores:

```text id="t8m1k5"
top value
```

and:

```text id="f2m7r9"
dp[j-1]
```

stores:

```text id="m5r8k1"
left value
```

So full matrix unnecessary.

---

# Important DP Pattern Learned

This problem teaches:

# Grid Path Counting DP

Very important interview category.

---

# Similar Problems

* Unique Paths II
* Minimum Path Sum
* Dungeon Game
* Cherry Pickup
* Grid Obstacle Problems

---

# Common Mistakes

## Mistake 1

Using wrong DP definition.

Always clearly define:

```text id="x7m4v2"
What exactly does dp[i][j] represent?
```

---

## Mistake 2

Forgetting first row/column initialization.

Without them recurrence breaks.

---

## Mistake 3

Using wrong recurrence.

Correct recurrence:

dp[i][j]=dp[i-1][j]+dp[i][j-1]

---

## Mistake 4

Confusing rows and columns.

Remember:

```text id="q9m1r6"
dp[row][column]
```

---

# Recognition Pattern

Whenever problem says:

* count paths
* move in grid
* right/down movement
* count ways to reach destination

think:

# Grid Dynamic Programming

---

# Important Mental Models

## Grid DP

```text id="w4m8k2"
current cell answer
depends on neighboring cells
```

---

## Counting DP

```text id="p1m7r5"
total ways
=
sum of ways from valid previous states
```

---

## State Transition

```text id="t6m2v8"
build larger answers
from smaller solved cells
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="r8m5k4"
reusing previously solved subproblems
to efficiently build optimal/counting solutions
```

instead of recomputing repeatedly.
