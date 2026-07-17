# Binary Tree Level Order Traversal (LeetCode 102)

## Problem Statement

Given the `root` of a binary tree, return the **level order traversal** of its nodes' values.

Return the values level by level from **left to right**.

Example:

```text
Input:

        3
       / \
      9   20
         /  \
        15   7

Output:

[
  [3],
  [9,20],
  [15,7]
]
```

---

# Intuition

The question asks us to visit nodes **level by level**.

Depth First Search (DFS) goes deep into one branch first, so it is **not the ideal choice**.

Instead, we need **Breadth First Search (BFS)**, which naturally visits nodes one level at a time.

BFS is implemented using a **Queue (FIFO)**.

---

# Key Observation

At every moment, the queue contains **all nodes of the current level**.

Before processing a level, store

```cpp
int levelSize = q.size();
```

This tells us exactly how many nodes belong to the current level.

After processing these `levelSize` nodes, any newly added children automatically become the next level.

---

# Algorithm

1. If the tree is empty, return an empty vector.
2. Push the root into a queue.
3. While the queue is not empty:

   * Store the current queue size (`levelSize`).
   * Create a vector for the current level.
   * Repeat `levelSize` times:

     * Pop the front node.
     * Store its value.
     * Push its left child if it exists.
     * Push its right child if it exists.
   * Add the current level vector to the answer.
4. Return the answer.

---

# Dry Run

Tree:

```text
        3
       / \
      9   20
         /  \
        15   7
```

### Initial Queue

```text
Queue = [3]
```

---

### Level 1

```text
levelSize = 1

Pop 3

Level = [3]

Push 9
Push 20

Queue = [9,20]
```

Answer:

```text
[[3]]
```

---

### Level 2

```text
levelSize = 2

Pop 9
Level = [9]

Pop 20
Level = [9,20]

Push 15
Push 7

Queue = [15,7]
```

Answer

```text
[[3],
 [9,20]]
```

---

### Level 3

```text
levelSize = 2

Pop 15

Pop 7

Queue becomes empty
```

Final Answer

```text
[
 [3],
 [9,20],
 [15,7]
]
```

---

# Code

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {

        if (!root)
            return {};

        queue<TreeNode*> q;
        vector<vector<int>> ans;

        q.push(root);

        while (!q.empty()) {

            int levelSize = q.size();
            vector<int> level;

            for (int i = 0; i < levelSize; i++) {

                TreeNode* node = q.front();
                q.pop();

                level.push_back(node->val);

                if (node->left)
                    q.push(node->left);

                if (node->right)
                    q.push(node->right);
            }

            ans.push_back(level);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

**Time Complexity:** `O(N)`

* Every node is visited exactly once.

**Space Complexity:** `O(N)`

* The queue may contain all nodes of the widest level in the tree.

---

# Pattern Recognition

Whenever you see phrases like:

* Level by level traversal
* Print each level
* Average of each level
* Right side view
* Left side view
* Zigzag traversal
* Minimum distance in an unweighted graph
* Shortest path (unweighted)

Think:

```text
Queue
        ↓
Breadth First Search (BFS)
        ↓
Process one level at a time
```

---

# Key Takeaways

* Use **BFS** whenever traversal is required **level by level**.
* A **Queue (FIFO)** is the core data structure for BFS.
* `levelSize = q.size()` tells us how many nodes belong to the current level.
* Children added during processing are automatically handled in the **next iteration**, representing the next level.
* This is the standard BFS template and can be reused for many binary tree and graph problems.
