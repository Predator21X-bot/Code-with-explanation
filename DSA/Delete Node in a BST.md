# 🧩 Delete Node in a BST

### 🔗 Problem Link

https://leetcode.com/problems/delete-node-in-a-bst/

---

# 🧠 Problem Understanding

Given:

* Binary Search Tree (BST)
* key value

👉 Goal:

```text id="del1"
delete node having key
```

while still maintaining:

```text id="del2"
valid BST structure
```

---

# 🌳 BST Property

For every node:

---

## Left subtree

```text id="del3"
all values smaller than root
```

---

## Right subtree

```text id="del4"
all values greater than root
```

---

# 🧠 Why deletion is tricky?

Because after deleting:

```text id="del5"
BST ordering must still remain correct
```

---

# 🔍 Example

```text id="del6"
        5
       / \
      3   6
     / \    \
    2   4    7
```

Delete:

```text id="del7"
3
```

We cannot simply remove it because:

```text id="del8"
children would disconnect
```

---

# 🔥 Three Deletion Cases (VERY IMPORTANT)

---

# 🧩 Case 1 — Leaf Node

Node has:

```text id="del9"
no children
```

Example:

```text id="del10"
delete 2
```

---

## Action

Simply remove node.

```cpp id="del11"
return NULL;
```

---

# 🔍 Visual

Before:

```text id="del12"
    3
   /
  2
```

After:

```text id="del13"
    3
```

---

# 🧩 Case 2 — One Child

Node has:

```text id="del14"
only one child
```

Example:

```text id="del15"
    6
     \
      7
```

Delete:

```text id="del16"
6
```

---

## Action

Child replaces deleted node.

```cpp id="del17"
return root->right;
```

OR

```cpp id="del18"
return root->left;
```

---

# 🔍 Visual

Before:

```text id="del19"
parent
  |
  6
   \
    7
```

After:

```text id="del20"
parent
  |
  7
```

---

# 🧩 Case 3 — Two Children (HARDEST)

Node has:

```text id="del21"
left and right child
```

Example:

```text id="del22"
        5
       / \
      3   6
     / \
    2   4
```

Delete:

```text id="del23"
3
```

---

# 🧠 Core Idea

Replace node with:

```text id="del24"
next greater value
```

called:

```text id="del25"
inorder successor
```

---

# 🧠 What is inorder successor?

```text id="del26"
smallest node in RIGHT subtree
```

---

# 🔍 Why does this work?

Successor is:

* greater than all left subtree nodes ✅
* smaller than remaining right subtree nodes ✅

👉 BST remains valid

---

# 🧩 Step 1 — Find successor

Right subtree:

```text id="del27"
4
```

Successor:

```text id="del28"
4
```

---

# 🧠 How to find smallest node?

Keep moving LEFT:

```cpp id="del29"
TreeNode* curr = root->right;

while (curr->left) {
    curr = curr->left;
}
```

---

# 🔥 Important BST Insight

In BST:

```text id="del30"
smallest value = leftmost node
```

---

# 🧩 Step 2 — Replace value

```cpp id="del31"
root->val = curr->val;
```

---

# 🧩 Step 3 — Delete duplicate successor node

VERY IMPORTANT 🔥

```cpp id="del32"
root->right =
    deleteNode(root->right, curr->val);
```

---

# ❗ Why delete again?

Because:

```text id="del33"
we copied successor value
```

but original successor node still exists.

Without deleting:

```text id="del34"
duplicate node remains
```

---

# ⚡ Complete Recursive Strategy

---

# 🧩 Step 1 — Search for node

Exactly like BST search.

---

## Go LEFT

```cpp id="del35"
if (key < root->val)
```

```cpp id="del36"
root->left =
    deleteNode(root->left, key);
```

---

## Go RIGHT

```cpp id="del37"
if (key > root->val)
```

```cpp id="del38"
root->right =
    deleteNode(root->right, key);
```

---

# 🧩 Step 2 — Node found

```text id="del39"
root->val == key
```

Now handle:

* leaf
* one child
* two children

---

# 💻 Full Code

```cpp id="del40"
class Solution {
public:

    TreeNode* deleteNode(TreeNode* root, int key) {

        if (!root)
            return NULL;

        // search left
        if (key < root->val) {

            root->left =
                deleteNode(root->left, key);
        }

        // search right
        else if (key > root->val) {

            root->right =
                deleteNode(root->right, key);
        }

        // node found
        else {

            // no left child
            if (!root->left)
                return root->right;

            // no right child
            if (!root->right)
                return root->left;

            // two children
            TreeNode* curr = root->right;

            while (curr->left) {
                curr = curr->left;
            }

            // replace value
            root->val = curr->val;

            // delete successor
            root->right =
                deleteNode(root->right, curr->val);
        }

        return root;
    }
};
```

---

# 🔍 Step-by-Step Dry Run

Tree:

```text id="del41"
        5
       / \
      3   6
     / \    \
    2   4    7
```

Delete:

```text id="del42"
3
```

---

# 🧩 Node found

Node:

```text id="del43"
3
```

Has:

```text id="del44"
two children
```

---

# 🧩 Find successor

Right subtree:

```text id="del45"
4
```

Successor:

```text id="del46"
4
```

---

# 🧩 Replace value

```text id="del47"
3 → 4
```

Tree becomes:

```text id="del48"
        5
       / \
      4   6
     /      \
    2        7
```

---

# 🧩 Delete original successor

Recursive deletion removes old `4`.

---

# ✅ Final BST

```text id="del49"
        5
       / \
      4   6
     /      \
    2        7
```

---

# 🧠 Mental Model (REVISION KEY)

> “Search → replace → reconnect”

---

# ⚡ Key BST Insight

Deletion is really:

```text id="del50"
rebuilding subtree correctly
```

after removing node.

---

# ⚡ Why recursion works well?

Because recursive calls naturally:

```text id="del51"
return updated subtree roots
```

---

# ⚡ Why return child in one-child case?

Because parent automatically reconnects:

```cpp id="del52"
root->left = ...
```

or

```cpp id="del53"
root->right = ...
```

---

# ⏱ Complexity

---

## Balanced BST

Time:

```text id="del54"
O(log n)
```

---

## Worst case (skewed tree)

Time:

```text id="del55"
O(n)
```

---

# ⚡ Space Complexity

Recursive stack:

---

## Balanced BST

```text id="del56"
O(log n)
```

---

## Worst case

```text id="del57"
O(n)
```

---

# ❗ Common Mistakes

* ❌ Forgetting to delete successor again
* ❌ Not handling one-child cases
* ❌ Searching both subtrees unnecessarily
* ❌ Breaking BST ordering

---

# 🧠 Patterns Learned

* BST restructuring
* Recursive subtree rebuilding
* Inorder successor logic

---

# 🏷 Tags

* Tree
* BST
* DFS
* Recursion

---

# 🔥 Recognition Pattern

Use inorder successor when:

```text id="del58"
deleting BST node with two children
```
