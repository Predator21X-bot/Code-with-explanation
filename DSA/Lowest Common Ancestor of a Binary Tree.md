# 🧩 Lowest Common Ancestor of a Binary Tree

### 🔗 Problem Link

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

---

# 🧠 Problem Understanding

Given:

* Binary tree
* Two nodes `p` and `q`

👉 Goal:

```text id="lca101"
Find the lowest node whose subtree contains BOTH p and q
```

---

# 🔍 Example

```text id="lca102"
        3
       / \
      5   1
     / \
    6   2
```

Suppose:

```text id="lca103"
p = 6
q = 2
```

---

# ❓ Which node contains both?

---

## Node 6

Contains:

```text id="lca104"
only 6
```

❌

---

## Node 2

Contains:

```text id="lca105"
only 2
```

❌

---

## Node 5

Subtree contains:

```text id="lca106"
6 and 2
```

✅ BOTH found

---

# ✅ Answer

```text id="lca107"
5
```

---

# 🧠 Core Recursive Idea

At every node, ask:

```text id="lca108"
Does left subtree contain p/q?
Does right subtree contain p/q?
```

---

# ⚡ Key Observation

If:

```text id="lca109"
left subtree finds one node
right subtree finds other node
```

👉 Current node becomes:

```text id="lca110"
Lowest Common Ancestor
```

---

# 🧠 Bottom-Up Thinking

Information flows:

```text id="lca111"
children → parent
```

---

# ⚡ Recursive Return Meaning

Each recursive call returns:

```text id="lca112"
- p
- q
- LCA
- or NULL
```

---

# 🧩 Base Case 1

```cpp id="lca113"
if (!root) return NULL;
```

👉 Means:

```text id="lca114"
nothing found
```

---

# 🧩 Base Case 2

```cpp id="lca115"
if (root == p || root == q)
    return root;
```

👉 Means:

```text id="lca116"
found one target node
```

---

# ⚡ Recursive Search

```cpp id="lca117"
TreeNode* left =
    lowestCommonAncestor(root->left, p, q);

TreeNode* right =
    lowestCommonAncestor(root->right, p, q);
```

---

# ⚡ Important Cases

---

# 🧩 Case 1

```text id="lca118"
left != NULL
right != NULL
```

👉 Both subtrees found something

✅ Current node becomes LCA

```cpp id="lca119"
return root;
```

---

# 🧩 Case 2

Only left subtree found something:

```cpp id="lca120"
return left;
```

---

# 🧩 Case 3

Only right subtree found something:

```cpp id="lca121"
return right;
```

---

# 💻 Full Code

```cpp id="lca122"
class Solution {
public:

    TreeNode* lowestCommonAncestor(
        TreeNode* root,
        TreeNode* p,
        TreeNode* q
    ) {

        // base cases
        if (!root || root == p || root == q)
            return root;

        // search subtrees
        TreeNode* left =
            lowestCommonAncestor(root->left, p, q);

        TreeNode* right =
            lowestCommonAncestor(root->right, p, q);

        // both sides found
        if (left && right)
            return root;

        // one side found
        if (left)
            return left;

        return right;
    }
};
```

---

# 🔍 Step-by-Step Dry Run

Tree:

```text id="lca123"
        3
       / \
      5   1
     / \
    6   2
```

---

## Find LCA of 6 and 2

---

# 🧩 At node 6

```text id="lca124"
root == p
```

Return:

```text id="lca125"
6
```

---

# 🧩 At node 2

```text id="lca126"
root == q
```

Return:

```text id="lca127"
2
```

---

# 🧩 At node 5

```text id="lca128"
left = 6
right = 2
```

👉 Both found

Return:

```text id="lca129"
5
```

---

# 🧠 Mental Model (REVISION KEY)

> “Subtrees report findings upward”

---

# ⚡ Why NO separate DFS helper function was needed?

This is VERY important 🔥

---

# 🧠 In many tree problems

We create separate DFS because:

```text id="lca130"
main function cannot directly solve problem recursively
```

Example:

* Path Sum III
* Count Good Nodes
* Leaf Similar Trees

They needed:

* extra parameters
* extra traversal logic
* helper states

---

# ⚡ But in LCA

The recursive function itself already naturally represents:

```text id="lca131"
“search subtree and report result upward”
```

---

# 🧠 The main function IS the DFS

```cpp id="lca132"
lowestCommonAncestor(...)
```

already:

* traverses tree
* explores subtrees
* combines answers

👉 So extra helper function unnecessary

---

# 🔥 Key Insight

If recursive function naturally:

```text id="lca133"
returns exactly what problem asks
```

👉 Separate DFS often not needed

---

# ⚡ Compare with Path Sum III

There:

```text id="lca134"
one DFS chooses start node
another explores paths
```

👉 Multiple responsibilities

So helper required

---

# ⚡ Compare with LCA

One recursion already handles:

```text id="lca135"
search + combine results
```

👉 Single responsibility

No helper needed

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * recursion stack → **O(h)**

---

# ❗ Common Mistakes

* ❌ Checking only immediate children
* ❌ Forgetting subtree recursion
* ❌ Not understanding upward propagation
* ❌ Returning wrong node

---

# 🧠 Patterns Learned

* Bottom-up DFS
* Recursive subtree reporting
* Information bubbling upward

---

# 🏷 Tags

* Tree
* DFS
* Recursion
* Binary Tree

---

# 🔥 Recognition Pattern

Use this approach when:

* Subtrees return partial answers
* Parent combines child results
* Need lowest/first meeting point
