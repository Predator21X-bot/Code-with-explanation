# 104. Maximum Depth of Binary Tree (Easy)

**Problem Link:** https://leetcode.com/problems/maximum-depth-of-binary-tree/

---

# Pattern Recognition

### Pattern
- Binary Tree
- Depth First Search (DFS)
- Recursion

### Why?

The maximum depth of a tree depends on the maximum depth of its left and right subtrees.

This is a recursive problem because every subtree is itself a binary tree, and we solve each subtree in exactly the same way.

---

# Problem Statement

Given the root of a binary tree, return its **maximum depth**.

**Maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

---

# Intuition

To know the depth of the current node, we need to know:

- The depth of its left subtree.
- The depth of its right subtree.

Once we know both depths, we simply take the larger one and add **1** for the current node.

```text
Depth(Current Node)
=
1 + max(Left Depth, Right Depth)
```

---

# Key Observation

Every node asks the same question:

> "What is the maximum depth of my left subtree?"

> "What is the maximum depth of my right subtree?"

Once both answers are known,

```cpp
Current Depth = 1 + max(leftDepth, rightDepth)
```

The recursion naturally computes these values from the bottom of the tree upward.

---

# Why Recursion?

Suppose we're standing at the root.

Can we immediately determine its depth?

No.

First, we must know:

- How deep is the left subtree?
- How deep is the right subtree?

Each subtree is again a binary tree.

So we recursively solve the same problem for both children.

---

# Base Case

If the node is `nullptr`, the tree has no nodes.

Therefore,

```cpp
if (!root)
    return 0;
```

An empty tree has depth **0**.

---

# Recursive Formula

Suppose

```cpp
left = maxDepth(root->left);
right = maxDepth(root->right);
```

Then

```cpp
return 1 + max(left, right);
```

The `+1` counts the current node.

---

# Dry Run

Input

```text
        3
       / \
      9   20
         /  \
        15   7
```

---

### Step 1

At node `15`

```text
Left Depth = 0
Right Depth = 0

Depth = 1
```

---

### Step 2

At node `7`

```text
Depth = 1
```

---

### Step 3

At node `20`

```text
Left Depth = 1
Right Depth = 1

Depth = 1 + max(1,1)
      = 2
```

---

### Step 4

At node `9`

```text
Depth = 1
```

---

### Step 5

At node `3`

```text
Left Depth = 1
Right Depth = 2

Depth = 1 + max(1,2)
      = 3
```

Answer

```text
3
```

---

# Visualizing the Return Values

```text
        3
       / \
      9   20
         /  \
        15   7
```

Leaf nodes return

```text
15 → 1

7 → 1

9 → 1
```

Then

```text
20

= 1 + max(1,1)

= 2
```

Finally

```text
3

= 1 + max(1,2)

= 3
```

Notice how the answers travel **from the leaves back to the root**.

---

# Algorithm

1. If the current node is `nullptr`, return `0`.
2. Recursively find the maximum depth of the left subtree.
3. Recursively find the maximum depth of the right subtree.
4. Return:

```cpp
1 + max(leftDepth, rightDepth)
```

---

# Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {

        if (!root)
            return 0;

        int leftDepth = maxDepth(root->left);

        int rightDepth = maxDepth(root->right);

        return 1 + max(leftDepth, rightDepth);
    }
};
```

---

# Why do we add `1`?

Suppose

```text
Left Depth = 2

Right Depth = 4
```

The current node itself must also be counted.

Therefore,

```text
Depth

= Current Node

+ Maximum Child Depth

= 1 + max(2,4)

= 5
```

Without adding `1`, the current node would never be counted.

---

# Complexity

### Time Complexity

```text
O(n)
```

Every node is visited exactly once.

---

### Space Complexity

```text
O(h)
```

where `h` is the height of the tree.

- Balanced Tree → **O(log n)**
- Skewed Tree → **O(n)**

The extra space comes from the recursion stack.

---

# Key Takeaways

- This is a classic **DFS + Recursion** problem.
- Every node computes its answer using the answers returned by its children.
- An empty tree has depth **0**.
- A leaf node has depth **1**.
- The recursive formula is:

```cpp
1 + max(leftDepth, rightDepth)
```

- The recursion works from the leaves upward until the root receives the final answer.

---

# Recursion Pattern Learned

This problem introduces an important recursion pattern where recursive calls **return information**.

```cpp
ReturnType dfs(TreeNode* root){

    if(!root)
        return baseValue;

    ReturnType left = dfs(root->left);

    ReturnType right = dfs(root->right);

    return combine(left, right);
}
```

For this problem,

```cpp
combine(left, right)

=

1 + max(left, right)
```

This same pattern is used in many binary tree problems.

---

# Similar Problems

- Balanced Binary Tree
- Diameter of Binary Tree
- Minimum Depth of Binary Tree
- Path Sum
- Binary Tree Tilt
- Longest Univalue Path
- Count Good Nodes in Binary Tree
