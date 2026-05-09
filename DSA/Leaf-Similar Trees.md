# 🧩 Leaf-Similar Trees

### 🔗 Problem Link

https://leetcode.com/problems/leaf-similar-trees/

---

# 🧠 Problem Understanding

Two binary trees are called:

```text id="rjg3s0"
leaf-similar
```

if their leaf nodes appear in the:

```text id="x3sjp9"
same order
```

---

# 🧠 What is a leaf node?

A node with:

```text id="ux6zt7"
no left child  
no right child
```

---

## 🔍 Example

```text id="t7mttf"
        3
       / \
      5   1
     / \
    6   2
```

Leaf nodes:

```text id="7j0x01"
6, 2, 1
```

---

# 🎯 Goal

👉 Collect leaf sequences from both trees

Then compare:

```text id="zopzxf"
sequence1 == sequence2
```

---

# ⚡ Approach: DFS Traversal

---

# 🧠 Why DFS?

DFS naturally explores:

```text id="kv9yfc"
left subtree → right subtree
```

👉 This preserves leaf order automatically

---

# 🧩 Step 1: DFS helper function

```cpp id="bxyw5r"
void dfs(TreeNode* root, vector<int>& leaves)
```

👉 Purpose:

```text id="c3z64i"
store leaf sequence
```

---

# 🧩 Step 2: Base case

```cpp id="h5m9g5"
if (!root) return;
```

👉 Empty node → stop recursion

---

# 🧩 Step 3: Leaf condition

```cpp id="itvuxj"
if (!root->left && !root->right)
```

👉 Means:

```text id="dgt4tp"
node has no children
```

---

## Store leaf

```cpp id="vc7wgi"
leaves.push_back(root->val);
```

---

# 🧩 Step 4: Recursive traversal

```cpp id="4tb2tm"
dfs(root->left, leaves);
dfs(root->right, leaves);
```

---

# 💻 Full Code

```cpp id="44rxkk"
class Solution {
public:

    void dfs(TreeNode* root, vector<int>& leaves) {

        if (!root) return;

        // leaf node
        if (!root->left && !root->right) {
            leaves.push_back(root->val);
        }

        dfs(root->left, leaves);
        dfs(root->right, leaves);
    }

    bool leafSimilar(TreeNode* root1, TreeNode* root2) {

        vector<int> leaves1;
        vector<int> leaves2;

        dfs(root1, leaves1);
        dfs(root2, leaves2);

        return leaves1 == leaves2;
    }
};
```

---

# 🔍 Step-by-Step Example

Tree 1:

```text id="rjlwm9"
        3
       / \
      5   1
     / \
    6   2
```

Leaf sequence:

```text id="7rjlwm"
6, 2, 1
```

---

Tree 2:

```text id="bjlwm3"
        7
       / \
      6   2
           \
            1
```

Leaf sequence:

```text id="rjlwm2"
6, 2, 1
```

---

## Compare

```cpp id="o8vs7e"
[6,2,1] == [6,2,1]
```

👉 Answer:

```text id="efv8zn"
true
```

---

# 🧠 Recursion Flow

```text id="h6jlwm"
dfs(root)
    → dfs(left)
    → dfs(right)
```

Whenever leaf found:

```text id="83e90e"
store value
```

---

# 🧠 Mental Model (REVISION KEY)

> “DFS collects leaf sequence in left-to-right order”

---

# ⚡ Key Insight

```text id="mkjlwm"
Problem cares about leaf order, NOT tree structure
```

---

# ⏱ Complexity

* Time: **O(n + m)**
* Space:

  * recursion stack + vectors

---

# ❗ Common Mistakes

* ❌ Confusing leaf nodes with all children
* ❌ Forgetting base case
* ❌ Using incorrect leaf condition
* ❌ Comparing structure instead of sequences

---

# 🧠 Patterns Learned

* DFS recursion
* Tree traversal
* Sequence comparison

---

# 🏷 Tags

* Tree
* DFS
* Recursion

---

# 🔥 Recognition Pattern

Use this approach when:

* Need specific nodes from traversal
* Compare traversal sequences
* Tree filtering problems
