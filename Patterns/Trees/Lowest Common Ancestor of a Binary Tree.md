# 236. Lowest Common Ancestor of a Binary Tree (Medium)

**Problem Link:** https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

---

# Pattern Recognition

### Pattern

- Binary Tree
- DFS
- Recursion
- Bottom-Up Recursion
- Return Information from Subtrees

---

# Problem Statement

Given the root of a binary tree and two nodes `p` and `q`, return their **Lowest Common Ancestor (LCA)**.

The Lowest Common Ancestor is the **lowest node in the tree that has both `p` and `q` as descendants** (a node can be a descendant of itself).

---

# Intuition

Instead of searching from the root downward, think from the bottom upward.

Every subtree answers one simple question:

> "Did I find `p` or `q`?"

Each recursive call returns information to its parent.

---

# Key Observation

Every recursive call can return only one of four things:

### Case 1

```cpp
nullptr
```

Neither `p` nor `q` exists in this subtree.

---

### Case 2

```cpp
p
```

This subtree found node `p`.

---

### Case 3

```cpp
q
```

This subtree found node `q`.

---

### Case 4

```cpp
LCA Node
```

This subtree has already found the Lowest Common Ancestor.

---

# Why Recursion?

Suppose we're standing at node `3`.

```text
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4
```

We don't immediately know whether `3` is the LCA.

Instead, we ask:

- Does my left subtree contain `p` or `q`?
- Does my right subtree contain `p` or `q`?

The recursive calls answer these questions.

---

# Base Cases

## 1. Empty Tree

```cpp
if (root == nullptr)
    return nullptr;
```

Nothing exists here.

---

## 2. Found p

```cpp
if (root == p)
    return p;
```

Return `p` immediately.

---

## 3. Found q

```cpp
if (root == q)
    return q;
```

Return `q` immediately.

---

# Recursive Calls

Search both subtrees.

```cpp
TreeNode* left = lowestCommonAncestor(root->left, p, q);

TreeNode* right = lowestCommonAncestor(root->right, p, q);
```

Now combine the answers.

---

# Three Recursive Cases

## Case 1

Both sides found something.

```text
Left  -> p

Right -> q
```

```text
        Root
       /    \
      ✓      ✓
```

Current node is the first place where both paths meet.

Return

```cpp
return root;
```

---

## Case 2

Only left subtree returned a node.

```text
Left -> p

Right -> nullptr
```

Both nodes must be inside the left subtree.

The left subtree has already found the answer.

Return

```cpp
return left;
```

---

## Case 3

Only right subtree returned a node.

```text
Left -> nullptr

Right -> q
```

Return

```cpp
return right;
```

---

# Dry Run

Input

```text
         3
       /   \
      5     1
     / \   / \
    6   2 0   8
       / \
      7   4

p = 5
q = 1
```

---

### At Node 5

Returns

```text
5
```

---

### At Node 1

Returns

```text
1
```

---

### At Node 3

Receives

```text
left = 5

right = 1
```

Since both sides returned valid nodes,

```cpp
return root;
```

Answer

```text
3
```

---

# Another Example

```text
         3
       /   \
      5     1
     / \
    6   2
       / \
      7   4

p = 5
q = 4
```

---

### At Node 4

Returns

```text
4
```

---

### At Node 2

Receives

```text
left = nullptr

right = 4
```

Returns

```text
4
```

---

### At Node 5

Current node itself is `p`.

Base case triggers immediately.

Returns

```text
5
```

---

### At Node 3

Receives

```text
left = 5

right = nullptr
```

Only one side contains the answer.

Returns

```text
5
```

Correct LCA

```text
5
```

---

# Algorithm

1. If current node is `nullptr`, return `nullptr`.
2. If current node equals `p`, return `p`.
3. If current node equals `q`, return `q`.
4. Recursively search left subtree.
5. Recursively search right subtree.
6. If both sides return non-null, current node is LCA.
7. Otherwise return whichever side is non-null.

---

# Code

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root,
                                   TreeNode* p,
                                   TreeNode* q) {

        if (root == nullptr)
            return nullptr;

        if (root == p)
            return p;

        if (root == q)
            return q;

        TreeNode* left = lowestCommonAncestor(root->left, p, q);

        TreeNode* right = lowestCommonAncestor(root->right, p, q);

        if (left && right)
            return root;

        if (left)
            return left;

        return right;
    }
};
```

---

# Why Does This Work?

Each subtree returns one of four possibilities:

```text
nullptr

p

q

LCA
```

The parent combines these answers.

If both children return valid nodes, the current node is the first common meeting point.

Otherwise, the parent simply forwards the valid answer upward.

The answer eventually propagates back to the original root.

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

Balanced Tree

```text
O(log n)
```

Worst Case (Skewed Tree)

```text
O(n)
```

The extra space comes from the recursion stack.

---

# Key Takeaways

- This is a Bottom-Up DFS problem.
- Every recursive call returns information to its parent.
- The recursive function returns a `TreeNode*`, not an integer.
- The answer is found when one target is discovered in the left subtree and the other in the right subtree.
- If only one subtree contains the answer, simply return that answer upward.
- The current node itself can be the LCA if it matches `p` or `q`.

---

# Recursion Pattern Learned

Unlike previous tree problems, recursion here returns a **node** instead of a number.

General pattern:

```cpp
ReturnType dfs(TreeNode* root) {

    if (base_case)
        return base_answer;

    ReturnType left = dfs(root->left);

    ReturnType right = dfs(root->right);

    return combine(left, right);
}
```

For this problem:

```cpp
combine(left, right)

if (left && right)
    return root;

if (left)
    return left;

return right;
```

This "return information from children and combine it" pattern is one of the most important recursion techniques for binary trees.

---

# Similar Problems

- Lowest Common Ancestor of a BST
- Smallest Subtree with All the Deepest Nodes
- Binary Tree Cameras
- Distance Between Two Nodes in a Binary Tree
- Step-by-Step Directions From a Binary Tree Node to Another
```
