# 98. Validate Binary Search Tree (Medium)

**Problem Link:** https://leetcode.com/problems/validate-binary-search-tree/

---

# Pattern Recognition

### Pattern

- Binary Search Tree (BST)
- DFS
- Recursion
- Range Validation

---

# Problem Statement

Given the root of a binary tree, determine whether it is a valid **Binary Search Tree (BST)**.

A valid BST satisfies:

- Every node in the left subtree is **strictly smaller** than the current node.
- Every node in the right subtree is **strictly greater** than the current node.
- Both left and right subtrees must themselves be valid BSTs.

---

# Intuition

Many beginners make this mistake:

They only compare a node with its **immediate parent**.

Example:

```text
        10
       /  \
      5    15
          /  \
         6    20
```

At node `15`

```text
6 < 15 ✔

20 > 15 ✔
```

Looks correct.

But the tree is **not** a BST because

```text
6 < 10
```

and `6` belongs to the **right subtree of 10**, where every node must be greater than `10`.

So checking only parent-child relationships is **not enough**.

---

# Key Observation

Every node must satisfy **all constraints from its ancestors**, not just its parent.

Instead of asking

> "Am I smaller than my parent?"

Ask

> "Am I inside my allowed range?"

---

# Range Validation

Each recursive call carries two values.

```text
Minimum Allowed Value

Maximum Allowed Value
```

Every node must satisfy

```text
Minimum < Node Value < Maximum
```

---

# How the Range Changes

Suppose

```text
        5
       / \
      2   8
```

Initially

```text
Node 5

Range

(-∞ , +∞)
```

---

## Left Child

```text
        5
       /
      2
```

Everything in the left subtree must be

```text
(-∞ , 5)
```

The maximum changes.

---

## Right Child

```text
        5
          \
           8
```

Everything in the right subtree must be

```text
(5 , +∞)
```

The minimum changes.

---

# Why Ancestors Matter

Example

```text
        5
       /
      2
       \
        4
```

At node `4`

Rules from parent:

```text
4 > 2 ✔
```

Rules from ancestor:

```text
4 < 5 ✔
```

So

```text
Allowed Range

(2 , 5)
```

---

Another example

```text
        10
       /
      5
       \
        12
```

At node `12`

Parent rule

```text
12 > 5 ✔
```

Ancestor rule

```text
12 < 10 ✖
```

The tree is invalid.

---

# Base Case

If the node is `nullptr`

```cpp
if(root == nullptr)
    return true;
```

An empty tree is always a valid BST.

---

# Checking the Current Node

Every node must satisfy

```text
Minimum < Node Value < Maximum
```

If not

```cpp
return false;
```

Condition

```cpp
if(root->val <= minValue || root->val >= maxValue)
    return false;
```

Notice the strict comparisons.

Duplicates are **not allowed**.

---

# Recursive Calls

For the left subtree

```text
Allowed Range

(minValue , root->val)
```

Recursive call

```cpp
isValid(root->left, minValue, root->val)
```

---

For the right subtree

```text
Allowed Range

(root->val , maxValue)
```

Recursive call

```cpp
isValid(root->right, root->val, maxValue)
```

---

# Dry Run

Input

```text
        5
       / \
      1   4
         / \
        3   6
```

---

Root

```text
Node = 5

Range

(-∞ , +∞)
```

Valid.

---

Go Right

```text
Node = 4

Range

(5 , +∞)
```

Check

```text
4 > 5 ✖
```

Fails immediately.

Return

```text
false
```

The tree is not a BST.

---

# Algorithm

1. Start with range `(-∞ , +∞)`.
2. If node is `nullptr`, return `true`.
3. Check if current value lies inside the allowed range.
4. Validate the left subtree using

```text
(minValue , root->val)
```

5. Validate the right subtree using

```text
(root->val , maxValue)
```

6. Return true only if both subtrees are valid.

---

# Code

```cpp
class Solution {
public:

    bool isValid(TreeNode* root, long minValue, long maxValue){

        if(root == nullptr)
            return true;

        if(root->val <= minValue || root->val >= maxValue)
            return false;

        return isValid(root->left, minValue, root->val) &&
               isValid(root->right, root->val, maxValue);
    }

    bool isValidBST(TreeNode* root) {

        return isValid(root, LONG_MIN, LONG_MAX);
    }
};
```

---

# Why use `long` instead of `int`?

Constraints

```text
-2^31 <= Node.val <= 2^31 - 1
```

Suppose the root value is

```text
INT_MIN
```

If we also initialize

```cpp
minValue = INT_MIN
```

then

```cpp
root->val <= minValue
```

becomes true immediately, incorrectly marking a valid tree as invalid.

Using

```cpp
LONG_MIN
LONG_MAX
```

creates a wider range that safely contains every possible `int` value.

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

- Checking only the parent is **not sufficient**.
- Every node must satisfy the constraints of **all its ancestors**.
- Pass a valid `(min, max)` range during recursion.
- Left subtree updates the **maximum**.
- Right subtree updates the **minimum**.
- Use **strict inequalities** because duplicates are not allowed.
- Use `LONG_MIN` and `LONG_MAX` to safely handle extreme integer values.

---

# Recursion Pattern Learned

This problem introduces **Range Validation**.

General template:

```cpp
bool dfs(TreeNode* root, long minValue, long maxValue){

    if(root == nullptr)
        return true;

    if(root->val <= minValue || root->val >= maxValue)
        return false;

    return dfs(root->left, minValue, root->val) &&
           dfs(root->right, root->val, maxValue);
}
```

This pattern is commonly used whenever recursive calls need to carry **constraints from ancestor nodes**.

---

# Similar Problems

- Lowest Common Ancestor of a BST
- Recover Binary Search Tree
- Kth Smallest Element in a BST
- Convert Sorted Array to BST
- Insert into a BST
- Delete Node in a BST
- Trim a Binary Search Tree
```
