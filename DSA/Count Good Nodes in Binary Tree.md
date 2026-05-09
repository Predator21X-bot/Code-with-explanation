# 🧩 Count Good Nodes in Binary Tree

### 🔗 Problem Link

https://leetcode.com/problems/count-good-nodes-in-binary-tree/

---

# 🧠 Problem Understanding

A node is called:

```text id="p4jlwm"
good
```

if:

```text id="l6jlwm"
no node on the path from root to it
has a value greater than it
```

---

# 🔍 Example

```text id="8jlwm7"
        3
       / \
      1   4
         / \
        1   5
```

---

## Good nodes

### Node 3

Path:

```text id="0jlwm3"
3
```

No greater value ✅

---

### Node 1 (left)

Path:

```text id="yjlwm1"
3 → 1
```

3 > 1 ❌

---

### Node 4

Path:

```text id="njlwm2"
3 → 4
```

No greater value ✅

---

### Node 5

Path:

```text id="qjlwm5"
3 → 4 → 5
```

No greater value ✅

---

## Answer

```text id="djlwm4"
3 good nodes
```

---

# 🧠 Key Observation

At every node, we only need:

```text id="5jlwm8"
maximum value seen so far on current path
```

---

# ⚡ Core Idea

If:

```text id="jjlwm6"
current node >= maxSoFar
```

👉 Node is good ✅

---

# ⚡ Approach: DFS + Path Maximum

---

# 🧩 Recursive function

```cpp id="6jlwm9"
int dfs(TreeNode* root, int maxSoFar)
```

---

## Why pass `maxSoFar`?

👉 Tracks:

```text id="rjlwm0"
largest value from root → current node
```

---

# 🧩 Base case

```cpp id="gjlwm7"
if (!root) return 0;
```

---

# 🧩 Check current node

```cpp id="1jlwm5"
int count = 0;

if (root->val >= maxSoFar) {
    count = 1;
}
```

---

# 🧩 Update maximum

```cpp id="sjlwm4"
maxSoFar = max(maxSoFar, root->val);
```

---

# 🧩 Recurse on children

```cpp id="3jlwm2"
count += dfs(root->left, maxSoFar);
count += dfs(root->right, maxSoFar);
```

---

# 💻 Full Code

```cpp id="7jlwm8"
class Solution {
public:

    int dfs(TreeNode* root, int maxSoFar) {

        if (!root) return 0;

        int count = 0;

        // check if node is good
        if (root->val >= maxSoFar) {
            count = 1;
        }

        // update path maximum
        maxSoFar = max(maxSoFar, root->val);

        // recurse
        count += dfs(root->left, maxSoFar);
        count += dfs(root->right, maxSoFar);

        return count;
    }

    int goodNodes(TreeNode* root) {
        return dfs(root, root->val);
    }
};
```

---

# 🔍 Step-by-Step Walkthrough

Tree:

```text id="kjlwm9"
        3
       / \
      1   4
         / \
        1   5
```

---

## Node 3

```text id="bjlwm1"
maxSoFar = 3

3 >= 3 ✅
count = 1
```

---

## Left node 1

```text id="cjlwm2"
maxSoFar = 3

1 < 3 ❌
```

---

## Right node 4

```text id="hjlwm3"
4 >= 3 ✅

update maxSoFar = 4
```

---

## Node 5

```text id="mjlwm6"
5 >= 4 ✅
```

---

## Total

```text id="zjlwm7"
3 good nodes
```

---

# 🧠 Recursion Flow

```text id="tjlwm5"
dfs(node, maxSoFar)
```

At each step:

```text id="fjlwm8"
1. check node
2. update max
3. recurse
```

---

# 🧠 Mental Model (REVISION KEY)

> “Carry maximum value along root-to-node path”

---

# ⚡ Key Insight

```text id="vjlwm4"
DFS can carry information from parent → child
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * recursion stack → **O(h)**

---

# ❗ Common Mistakes

* ❌ Using vector unnecessarily
* ❌ Forgetting to update `maxSoFar`
* ❌ Wrong comparison logic
* ❌ Missing base case

---

# 🧠 Patterns Learned

* DFS recursion
* Path-based traversal
* State propagation in recursion

---

# 🏷 Tags

* Tree
* DFS
* Recursion

---

# 🔥 Recognition Pattern

Use this approach when:

* Need information along root-to-node path
* Parent affects child logic
* Tree traversal with constraints
