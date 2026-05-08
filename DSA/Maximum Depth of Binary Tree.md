# 🧩 Maximum Depth of Binary Tree

### 🔗 Problem Link

https://leetcode.com/problems/maximum-depth-of-binary-tree/

---

# 🧠 Problem Understanding

Given:

* Root of a binary tree

👉 Goal:

```text
Return the maximum depth of the tree
```

---

# 🧠 What is depth?

👉 Depth = number of levels from root to deepest leaf

---

## 🔍 Example

```text
        1
       / \
      2   3
     /
    4
```

---

## Levels

```text
Level 1 → 1
Level 2 → 2,3
Level 3 → 4
```

👉 Maximum depth:

```text
3
```

---

# 🧠 Core Idea

At every node:

```text
depth = 1 + max(leftDepth, rightDepth)
```

---

# 🔥 Why?

👉 Current node contributes:

```text
1
```

👉 Then choose deeper subtree:

```text
max(left, right)
```

---

# ⚡ Recursive DFS Approach

---

## 🧩 Base case

```cpp
if (root == NULL) return 0;
```

👉 Empty tree has depth:

```text
0
```

---

## 🧩 Recursive calls

```cpp
int leftDepth = maxDepth(root->left);
int rightDepth = maxDepth(root->right);
```

---

## 🧩 Return answer

```cpp
return 1 + max(leftDepth, rightDepth);
```

---

# 💻 Full Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {

        // base case
        if (root == NULL) return 0;

        int leftDepth = maxDepth(root->left);
        int rightDepth = maxDepth(root->right);

        return 1 + max(leftDepth, rightDepth);
    }
};
```

---

# 🔍 Step-by-Step Walkthrough

Tree:

```text
        1
       / \
      2   3
     /
    4
```

---

## Node 4

```text
left = 0
right = 0

depth = 1
```

---

## Node 2

```text
left = 1
right = 0

depth = 2
```

---

## Node 3

```text
left = 0
right = 0

depth = 1
```

---

## Node 1

```text
left = 2
right = 1

depth = 3
```

---

# 🧠 Recursion Flow

```text
maxDepth(1)
    → maxDepth(2)
        → maxDepth(4)
            → return 1
        → return 2
    → maxDepth(3)
        → return 1
    → return 3
```

---

# 🧠 Mental Model (REVISION KEY)

> “Each node asks children for their depth”

---

# ⚡ Why recursion works naturally in trees

Because:

```text
Every subtree is itself a tree
```

👉 Solve smaller trees recursively

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * Average: **O(h)** recursion stack
  * Worst case: **O(n)**

---

# ❗ Common Mistakes

* ❌ Forgetting base case
* ❌ Using `min()` instead of `max()`
* ❌ Returning wrong value
* ❌ Confusing height vs depth terminology

---

# 🧠 Patterns Learned

* DFS recursion
* Divide & conquer on trees
* Bottom-up recursion

---

# 🏷 Tags

* Tree
* DFS
* Recursion

---

# 🔥 Recognition Pattern

Use this approach when:

* Need information from subtrees
* Tree height/depth problems
* Recursive tree traversal

---

# ⚡ Iterative BFS Approach (Alternative)

---

## 🧠 Idea

Use level-order traversal:

* each level increases depth by 1

---

## 💻 Code

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {

        if (!root) return 0;

        queue<TreeNode*> q;
        q.push(root);

        int depth = 0;

        while (!q.empty()) {

            int size = q.size();

            while (size--) {
                TreeNode* node = q.front();
                q.pop();

                if (node->left) q.push(node->left);
                if (node->right) q.push(node->right);
            }

            depth++;
        }

        return depth;
    }
};
```

---

# 🧠 BFS Mental Model

> “Process tree level by level”

---

# ⚡ DFS vs BFS

| DFS             | BFS                  |
| --------------- | -------------------- |
| recursive       | iterative            |
| subtree-focused | level-focused        |
| elegant         | intuitive for levels |
