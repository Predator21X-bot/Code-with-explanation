# 226. Invert Binary Tree (Easy)

**Problem Link:** https://leetcode.com/problems/invert-binary-tree/

---

# Pattern Recognition

### Pattern
- **Binary Tree**
- **Depth First Search (DFS)**
- **Recursion**

### Why?

Every node in the tree requires the **same operation**:

- Swap its left and right children.
- Perform the same operation on its left subtree.
- Perform the same operation on its right subtree.

Since the problem repeats itself on smaller subtrees, recursion is the natural choice.

---

# Problem Statement

Given the root of a binary tree,

Invert the tree.

Inverting means:

- Every node swaps its left and right child.

---

# Intuition

Original Tree

```text
        4
      /   \
     2     7
    / \   / \
   1   3 6   9
```

After inversion

```text
        4
      /   \
     7     2
    / \   / \
   9   6 3   1
```

Notice:

- The root stays the same.
- Only the **left and right child pointers** are swapped.

We are **not swapping node values**.

---

# Key Observation

Every node performs exactly the same task.

For any node:

```text
Swap Left & Right Child

↓

Invert Left Subtree

↓

Invert Right Subtree
```

The same logic applies recursively to every subtree.

---

# Why Recursion?

Suppose we successfully invert

```text
        4
      /   \
     2     7
```

Now consider node `2`.

What should we do?

Exactly the same operation.

Likewise for node `7`.

Since the solution to a subtree is identical to the solution of the whole tree, recursion fits perfectly.

---

# Base Case

When do we stop?

If the node is `nullptr`.

```cpp
if (!root)
    return nullptr;
```

This happens after reaching beyond a leaf node.

---

# Algorithm

For every node:

1. If node is `nullptr`, return.
2. Swap left and right child.
3. Recursively invert left subtree.
4. Recursively invert right subtree.
5. Return the root.

---

# Dry Run

Input

```text
        4
      /   \
     2     7
```

---

### Step 1

Swap children of 4

```text
        4
      /   \
     7     2
```

---

### Step 2

Invert subtree rooted at 7

Before

```text
     7
    / \
   6   9
```

After

```text
     7
    / \
   9   6
```

---

### Step 3

Invert subtree rooted at 2

Before

```text
     2
    / \
   1   3
```

After

```text
     2
    / \
   3   1
```

---

### Final Tree

```text
        4
      /   \
     7     2
    / \   / \
   9   6 3   1
```

---

# Why don't we check

```cpp
if(root->left && root->right)
```

before swapping?

Because swapping works even if one child is `nullptr`.

Example

Before

```text
    1
   /
  2
```

After swap

```text
    1
     \
      2
```

Therefore simply write

```cpp
swap(root->left, root->right);
```

No extra condition is needed.

---

# Why does recursion work?

After swapping the children,

the left subtree and right subtree are still binary trees.

Each subtree needs to be inverted in exactly the same way.

Therefore we recursively solve:

```cpp
invertTree(root->left);

invertTree(root->right);
```

---

# Recursive Flow

Suppose

```text
        4
      /   \
     2     7
```

Function calls

```text
invert(4)

↓

swap(2,7)

↓

invert(7)

↓

invert(9)

↓

invert(6)

↓

return

↓

invert(2)

↓

invert(3)

↓

invert(1)

↓

return
```

Each recursive call solves one subtree independently.

---

# Code

```cpp
class Solution {
public:

    TreeNode* invertTree(TreeNode* root) {

        if (!root)
            return nullptr;

        swap(root->left, root->right);

        invertTree(root->left);

        invertTree(root->right);

        return root;
    }
};
```

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

This is due to the recursion stack.

---

# Key Takeaways

- This is a classic **DFS + Recursion** problem.
- Every node performs the same operation:
  - Swap left and right children.
- The recursion stops when the node is `nullptr`.
- No need to check whether both children exist before swapping.
- We swap **child pointers**, not node values.
- The root remains the same; only the tree structure changes.

---

# Recursion Template Learned

This problem introduces the standard DFS recursion template for trees.

```cpp
TreeNode* dfs(TreeNode* root){

    if(!root)
        return nullptr;

    // Process current node

    dfs(root->left);

    dfs(root->right);

    return root;
}
```

Many tree problems follow this exact structure.

---

# Similar Problems

- Maximum Depth of Binary Tree
- Same Tree
- Balanced Binary Tree
- Diameter of Binary Tree
- Subtree of Another Tree
- Symmetric Tree
- Binary Tree Paths
- Binary Tree Pruning
