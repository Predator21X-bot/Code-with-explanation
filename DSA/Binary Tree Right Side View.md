# 🧩 Binary Tree Right Side View

### 🔗 Problem Link

https://leetcode.com/problems/binary-tree-right-side-view/

---

# 🧠 Problem Understanding

Given:

* Binary tree

👉 Imagine standing on:

```text id="rv101"
RIGHT side of tree
```

Return:

```text id="rv102"
nodes visible from right side
```

---

# 🔍 Example

```text id="rv103"
        1
       / \
      2   3
       \    \
        5    4
```

---

# 👀 Visible nodes

---

## Level 1

```text id="rv104"
1
```

Rightmost:

```text id="rv105"
1
```

---

## Level 2

```text id="rv106"
2, 3
```

Rightmost:

```text id="rv107"
3
```

---

## Level 3

```text id="rv108"
5, 4
```

Rightmost:

```text id="rv109"
4
```

---

# ✅ Answer

```text id="rv110"
[1,3,4]
```

---

# 🧠 Key Observation

At every level:

```text id="rv111"
we only need LAST node
```

Because:

```text id="rv112"
last node = rightmost visible node
```

---

# ⚡ Best Approach: BFS (Level Order Traversal)

---

# 🧠 Why BFS?

BFS naturally processes:

```text id="rv113"
tree level by level
```

Perfect for:

* left/right view
* level sums
* zigzag levels
* shortest path

---

# ⚡ Data Structure

Use queue:

```cpp id="rv114"
queue<TreeNode*> q;
```

---

# 🧩 Step 1: Push root

```cpp id="rv115"
q.push(root);
```

---

# 🧩 Step 2: Process levels

At each level:

```cpp id="rv116"
int size = q.size();
```

👉 `size` tells:

```text id="rv117"
number of nodes in current level
```

---

# 🔍 Example

Queue:

```text id="rv118"
2, 3
```

Then:

```text id="rv119"
size = 2
```

---

# 🧩 Step 3: Traverse level

```cpp id="rv120"
for (int i = 0; i < size; i++)
```

---

# 🧠 Important Trick

When:

```cpp id="rv121"
i == size - 1
```

👉 current node is:

```text id="rv122"
rightmost node of level
```

Store it.

---

# 🧩 Step 4: Push children

```cpp id="rv123"
if (node->left)
    q.push(node->left);

if (node->right)
    q.push(node->right);
```

---

# 💻 Full Code

```cpp id="rv124"
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {

        vector<int> result;

        if (!root) return result;

        queue<TreeNode*> q;
        q.push(root);

        while (!q.empty()) {

            int size = q.size();

            for (int i = 0; i < size; i++) {

                TreeNode* node = q.front();
                q.pop();

                // rightmost node
                if (i == size - 1) {
                    result.push_back(node->val);
                }

                // push children
                if (node->left)
                    q.push(node->left);

                if (node->right)
                    q.push(node->right);
            }
        }

        return result;
    }
};
```

---

# 🔍 Step-by-Step Dry Run

Tree:

```text id="rv125"
        1
       / \
      2   3
       \    \
        5    4
```

---

# 🧩 Level 1

Queue:

```text id="rv126"
1
```

Size:

```text id="rv127"
1
```

Last node:

```text id="rv128"
1
```

Push:

```text id="rv129"
2,3
```

---

# 🧩 Level 2

Queue:

```text id="rv130"
2,3
```

Size:

```text id="rv131"
2
```

---

## i = 0

Node:

```text id="rv132"
2
```

Push:

```text id="rv133"
5
```

---

## i = 1

Node:

```text id="rv134"
3
```

👉 rightmost node

Store:

```text id="rv135"
3
```

Push:

```text id="rv136"
4
```

---

# 🧩 Level 3

Queue:

```text id="rv137"
5,4
```

Last node:

```text id="rv138"
4
```

---

# ✅ Final Answer

```text id="rv139"
[1,3,4]
```

---

# 🧠 Mental Model (REVISION KEY)

> “Take the last node from every BFS level”

---

# ⚡ Key BFS Insight

Queue always stores:

```text id="rv140"
next level nodes
```

---

# ⚡ Why `size` is important

Without `size`:

```text id="rv141"
cannot separate levels
```

---

# ⚡ Why `q.front()` not `top()`?

Queue operations:

```cpp id="rv142"
q.front()
q.pop()
```

---

# ❌ `top()` is for:

```text id="rv143"
stack / priority_queue
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * queue → **O(width of tree)**

---

# ❗ Common Mistakes

* ❌ Using `top()` instead of `front()`
* ❌ Forgetting to push children
* ❌ `size` outside while loop
* ❌ Forgetting empty-tree case

---

# 🧠 Patterns Learned

* BFS traversal
* Level-order processing
* Queue-based tree traversal

---

# 🏷 Tags

* Tree
* BFS
* Queue
* Level Order Traversal

---

# 🔥 Recognition Pattern

Use this approach when:

* Need level-wise processing
* Left/right view problems
* BFS visibility problems
