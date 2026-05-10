# 🧩 Search in a Binary Search Tree

### 🔗 Problem Link

https://leetcode.com/problems/search-in-a-binary-search-tree/

---

# 🧠 Problem Understanding

Given:

* Binary Search Tree (BST)
* integer `val`

👉 Goal:

```text id="sbst1"
find node having value = val
```

Return:

* subtree rooted at that node
  OR
* `NULL` if value not found

---

# 🌳 BST Property (MOST IMPORTANT)

For every node:

---

## Left subtree

```text id="sbst2"
all values smaller than root
```

---

## Right subtree

```text id="sbst3"
all values greater than root
```

---

# 🔍 Example

```text id="sbst4"
        4
       / \
      2   7
     / \
    1   3
```

---

# 🎯 Search

```text id="sbst5"
val = 1
```

---

# 🧩 Step-by-step

---

## At node 4

```text id="sbst6"
1 < 4
```

👉 Go LEFT

Discard:

```text id="sbst7"
entire right subtree
```

---

## At node 2

```text id="sbst8"
1 < 2
```

👉 Go LEFT again

---

## At node 1

```cpp id="sbst9"
root->val == val
```

✅ Found

Return node.

---

# 🧠 Key Observation

This works exactly like:

```text id="sbst10"
Binary Search on sorted array
```

Because BST is:

```text id="sbst11"
ordered
```

---

# ⚡ Core Logic

At every node:

---

## If value found

```cpp id="sbst12"
root->val == val
```

✅ Return node

---

## If target smaller

```cpp id="sbst13"
val < root->val
```

👉 Search LEFT subtree only

---

## If target greater

```cpp id="sbst14"
val > root->val
```

👉 Search RIGHT subtree only

---

# 🔥 Huge Optimization

Unlike normal binary tree:

```text id="sbst15"
we NEVER search both sides
```

We discard:

```text id="sbst16"
half the tree every step
```

---

# ⚡ Recursive Approach

---

# 🧩 Base Case 1

```cpp id="sbst17"
if (root == NULL)
    return NULL;
```

👉 Value not found

---

# 🧩 Base Case 2

```cpp id="sbst18"
if (root->val == val)
    return root;
```

👉 Value found

---

# 🧩 Search LEFT

```cpp id="sbst19"
if (val < root->val)
    return searchBST(root->left, val);
```

---

# 🧩 Search RIGHT

```cpp id="sbst20"
else
    return searchBST(root->right, val);
```

---

# 💻 Full Code

```cpp id="sbst21"
class Solution {
public:

    TreeNode* searchBST(TreeNode* root, int val) {

        // value not found
        if (root == NULL)
            return NULL;

        // value found
        if (root->val == val)
            return root;

        // search left subtree
        else if (val < root->val)
            return searchBST(root->left, val);

        // search right subtree
        else
            return searchBST(root->right, val);
    }
};
```

---

# 🔍 Dry Run

Tree:

```text id="sbst22"
        4
       / \
      2   7
     / \
    1   3
```

Search:

```text id="sbst23"
3
```

---

# 🧩 Node 4

```text id="sbst24"
3 < 4
```

Go LEFT

---

# 🧩 Node 2

```text id="sbst25"
3 > 2
```

Go RIGHT

---

# 🧩 Node 3

```cpp id="sbst26"
root->val == val
```

✅ Found

Return node.

---

# 🧠 Mental Model (REVISION KEY)

> “Binary Search on a Tree”

---

# ⚡ Key BST Insight

At every step:

```text id="sbst27"
discard half the tree
```

---

# ⚡ Why recursion works well?

Because BST naturally breaks into:

```text id="sbst28"
smaller subproblems
```

(left subtree / right subtree)

---

# ⏱ Complexity

---

## Balanced BST

Height:

```text id="sbst29"
log n
```

Time:

```text id="sbst30"
O(log n)
```

---

## Worst case (skewed tree)

Tree becomes:

```text id="sbst31"
linked list
```

Time:

```text id="sbst32"
O(n)
```

---

# ⚡ Space Complexity

Recursive stack:

---

## Balanced BST

```text id="sbst33"
O(log n)
```

---

## Worst case

```text id="sbst34"
O(n)
```

---

# ❗ Common Mistakes

* ❌ Forgetting `return` before recursive calls
* ❌ Searching both subtrees unnecessarily
* ❌ Typo in recursive function name
* ❌ Forgetting base case

---

# 🧠 Patterns Learned

* BST traversal
* Binary-search-style recursion
* Search-space reduction

---

# 🏷 Tags

* Tree
* BST
* DFS
* Recursion

---

# 🔥 Recognition Pattern

Use BST property when:

```text id="sbst35"
left < root < right
```

to:

* discard half search space
* optimize traversal
