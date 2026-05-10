# 🌳 Tree Traversals — Complete Notes

---

# 🧠 What is Tree Traversal?

Traversal means:

```text id="tt1"
visiting every node of tree in some order
```

---

# 🌳 Two Main Categories

---

# 1️⃣ DFS (Depth First Search)

👉 Explore:

```text id="tt2"
deep into subtree first
```

Uses:

* recursion
* stack

---

# 🔥 DFS Traversal Types

---

# 🧩 Preorder Traversal

---

## Order

```text id="tt3"
Root → Left → Right
```

---

## Example

```text id="tt4"
        1
       / \
      2   3
```

Traversal:

```text id="tt5"
1 2 3
```

---

## Recursive Code

```cpp id="tt6"
void preorder(TreeNode* root) {

    if (!root) return;

    cout << root->val << " ";

    preorder(root->left);
    preorder(root->right);
}
```

---

# 🧠 Mental Model

> “Process current node BEFORE children”

---

# 🔥 Use Cases

* tree copying
* serialization
* expression trees

---

# 🧩 Inorder Traversal

---

## Order

```text id="tt7"
Left → Root → Right
```

---

## Example

```text id="tt8"
        1
       / \
      2   3
```

Traversal:

```text id="tt9"
2 1 3
```

---

## Recursive Code

```cpp id="tt10"
void inorder(TreeNode* root) {

    if (!root) return;

    inorder(root->left);

    cout << root->val << " ";

    inorder(root->right);
}
```

---

# 🧠 Mental Model

> “Process current node BETWEEN children”

---

# 🔥 Important Property

For BST:

```text id="tt11"
Inorder gives sorted order
```

---

# 🧩 Postorder Traversal

---

## Order

```text id="tt12"
Left → Right → Root
```

---

## Example

```text id="tt13"
        1
       / \
      2   3
```

Traversal:

```text id="tt14"
2 3 1
```

---

## Recursive Code

```cpp id="tt15"
void postorder(TreeNode* root) {

    if (!root) return;

    postorder(root->left);
    postorder(root->right);

    cout << root->val << " ";
}
```

---

# 🧠 Mental Model

> “Process current node AFTER children”

---

# 🔥 Use Cases

* deleting tree
* bottom-up calculations
* subtree aggregation

Examples:

* Diameter of Tree
* LCA
* Maximum Path Sum

---

# 🌳 2️⃣ BFS (Breadth First Search)

👉 Explore:

```text id="tt16"
level by level
```

Uses:

* queue

---

# 🧩 Level Order Traversal

---

## Order

```text id="tt17"
Level 1 → Level 2 → Level 3
```

---

## Example

```text id="tt18"
        1
       / \
      2   3
     / \
    4   5
```

Traversal:

```text id="tt19"
1 2 3 4 5
```

---

## BFS Code

```cpp id="tt20"
void levelOrder(TreeNode* root) {

    if (!root) return;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {

        TreeNode* node = q.front();
        q.pop();

        cout << node->val << " ";

        if (node->left)
            q.push(node->left);

        if (node->right)
            q.push(node->right);
    }
}
```

---

# 🧠 Mental Model

> “Visit nodes level-by-level”

---

# 🔥 BFS Use Cases

* shortest path
* level processing
* visibility problems

Examples:

* Right Side View
* Maximum Level Sum
* Zigzag Level Order

---

# ⚡ DFS vs BFS

| Feature        | DFS                | BFS         |
| -------------- | ------------------ | ----------- |
| Style          | deep first         | level first |
| Data Structure | recursion/stack    | queue       |
| Space          | O(h)               | O(width)    |
| Best For       | subtree/path logic | level logic |

---

# 🔍 Visual Difference

---

# DFS

```text id="tt21"
1 → 2 → 4
```

👉 go deep first

---

# BFS

```text id="tt22"
1
2 3
4 5
```

👉 process by levels

---

# 🧠 Recognition Patterns

---

# ✅ Use DFS when:

* subtree problems
* recursion
* path calculations
* bottom-up logic

Examples:

* Maximum Depth
* LCA
* Path Sum
* Diameter

---

# ✅ Use BFS when:

* levels matter
* shortest path
* nearest node
* visibility

Examples:

* Right Side View
* Level Order Traversal
* Maximum Level Sum

---

# ⚡ Time Complexity

All traversals visit every node once:

```text id="tt23"
O(n)
```

---

# ⚡ Space Complexity

---

## DFS

```text id="tt24"
O(h)
```

(height of tree)

---

## BFS

```text id="tt25"
O(width)
```

(maximum nodes in one level)

---

# ❗ Common Mistakes

* ❌ Mixing DFS with BFS
* ❌ Using queue for DFS
* ❌ Forgetting recursion base case
* ❌ Confusing preorder/inorder/postorder order

---

# 🧠 Memory Tricks

---

## Preorder

```text id="tt26"
Root first
```

---

## Inorder

```text id="tt27"
Root middle
```

---

## Postorder

```text id="tt28"
Root last
```

---

## BFS

```text id="tt29"
Level-wise
```

---

# 🚀 Traversal Summary Table

| Traversal   | Order           | Type |
| ----------- | --------------- | ---- |
| Preorder    | Root Left Right | DFS  |
| Inorder     | Left Root Right | DFS  |
| Postorder   | Left Right Root | DFS  |
| Level Order | Level-by-Level  | BFS  |

---

# 🚀 Advanced Insight

Most tree interview problems are based on:

```text id="tt30"
“How information flows through traversal”
```

---

# DFS

Information flows:

```text id="tt31"
deep recursively
```

---

# BFS

Information flows:

```text id="tt32"
level by level
```

---

# 🎯 Final Takeaway

* DFS = recursive subtree exploration
* BFS = queue-based level traversal
* Choosing correct traversal is half the solution

---

## ✅ Status

* DFS traversal types mastered
* BFS vs DFS distinction clear
* Strong tree foundation built

---

# 🎯 Next (Recommended)

We can go:

* Diameter of Binary Tree 🔥
* Binary Tree Maximum Path Sum 🔥
* Level Order Traversal variations

---
