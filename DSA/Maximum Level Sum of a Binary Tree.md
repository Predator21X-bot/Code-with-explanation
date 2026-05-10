# 🧩 Maximum Level Sum of a Binary Tree

### 🔗 Problem Link

https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/

---

# 🧠 Problem Understanding

Given:

* Binary tree

👉 Goal:

```text id="ml101"
Find the level having maximum sum
```

Return:

```text id="ml102"
level number
```

NOT the sum itself.

---

# 🔍 Example

```text id="ml103"
        1
       / \
      7   0
     / \
    7  -8
```

---

# 🧩 Level sums

---

## Level 1

```text id="ml104"
1
```

Sum:

```text id="ml105"
1
```

---

## Level 2

```text id="ml106"
7 + 0
```

Sum:

```text id="ml107"
7
```

---

## Level 3

```text id="ml108"
7 + (-8)
```

Sum:

```text id="ml109"
-1
```

---

# ✅ Maximum level sum

```text id="ml110"
7
```

at:

```text id="ml111"
Level 2
```

---

# 🧠 Key Observation

We must process:

```text id="ml112"
one complete level at a time
```

👉 Perfect use case for:

```text id="ml113"
BFS / Level Order Traversal
```

---

# ⚡ Why BFS?

BFS naturally processes:

```text id="ml114"
tree level-by-level
```

Which makes:

* level sums
* averages
* right/left view
  easy.

---

# ⚡ Data Structure

Use queue:

```cpp id="ml115"
queue<TreeNode*> q;
```

---

# 🧩 Step 1: Push root

```cpp id="ml116"
q.push(root);
```

---

# 🧩 Step 2: Process levels

At every level:

```cpp id="ml117"
int size = q.size();
```

👉 `size` tells:

```text id="ml118"
number of nodes in current level
```

---

# 🔍 Example

Queue:

```text id="ml119"
7, 0
```

Then:

```text id="ml120"
size = 2
```

---

# 🧩 Step 3: Calculate level sum

```cpp id="ml121"
int sum = 0;
```

Inside loop:

```cpp id="ml122"
sum += curr->val;
```

---

# 🧩 Step 4: Push children

```cpp id="ml123"
if (curr->left)
    q.push(curr->left);

if (curr->right)
    q.push(curr->right);
```

---

# 🧠 Important Variables

---

## `maxSum`

Stores:

```text id="ml124"
largest level sum seen so far
```

---

## `answer`

Stores:

```text id="ml125"
which level has maximum sum
```

---

## `level`

Tracks:

```text id="ml126"
current level number
```

---

# ⚡ Why use `INT_MIN`?

VERY IMPORTANT 🔥

---

# ❌ Wrong

```cpp id="ml127"
int maxSum = 0;
```

Suppose ALL sums are negative:

```text id="ml128"
Level sums:
-1
-5
-2
```

Then:

```text id="ml129"
0 would incorrectly remain maximum
```

Even though:

```text id="ml130"
0 never existed
```

---

# ✅ Correct

```cpp id="ml131"
int maxSum = INT_MIN;
```

👉 ensures:

```text id="ml132"
negative sums also handled correctly
```

---

# 💻 Full Code

```cpp id="ml133"
class Solution {
public:
    int maxLevelSum(TreeNode* root) {

        queue<TreeNode*> q;
        q.push(root);

        int maxSum = INT_MIN;
        int level = 1;
        int answer = 1;

        while (!q.empty()) {

            int size = q.size();
            int sum = 0;

            for (int i = 0; i < size; i++) {

                TreeNode* curr = q.front();
                q.pop();

                sum += curr->val;

                if (curr->left)
                    q.push(curr->left);

                if (curr->right)
                    q.push(curr->right);
            }

            // update answer
            if (sum > maxSum) {
                maxSum = sum;
                answer = level;
            }

            level++;
        }

        return answer;
    }
};
```

---

# 🔍 Step-by-Step Dry Run

Tree:

```text id="ml134"
        1
       / \
      7   0
     / \
    7  -8
```

---

# 🧩 Level 1

Queue:

```text id="ml135"
1
```

Sum:

```text id="ml136"
1
```

Update:

```text id="ml137"
maxSum = 1
answer = 1
```

Push:

```text id="ml138"
7, 0
```

---

# 🧩 Level 2

Queue:

```text id="ml139"
7, 0
```

Sum:

```text id="ml140"
7
```

Now:

```text id="ml141"
7 > 1
```

Update:

```text id="ml142"
maxSum = 7
answer = 2
```

Push:

```text id="ml143"
7, -8
```

---

# 🧩 Level 3

Queue:

```text id="ml144"
7, -8
```

Sum:

```text id="ml145"
-1
```

No update

---

# ✅ Final Answer

```text id="ml146"
2
```

---

# 🧠 Mental Model (REVISION KEY)

> “Process one BFS level → compute its sum”

---

# ⚡ Key BFS Insight

Queue always stores:

```text id="ml147"
next level nodes
```

---

# ⚡ Why `size` matters

Without `size`:

```text id="ml148"
cannot separate levels
```

---

# ⚡ Queue operations

```cpp id="ml149"
q.front()
q.pop()
```

---

# ❗ NOT

```cpp id="ml150"
q.top()
```

(`top()` belongs to stack/priority_queue)

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * queue → **O(width of tree)**

---

# ❗ Common Mistakes

* ❌ Using `maxSum = 0`
* ❌ Forgetting negative sums
* ❌ `size` outside while loop
* ❌ Forgetting to push children
* ❌ Using `top()` instead of `front()`

---

# 🧠 Patterns Learned

* BFS traversal
* Level-order processing
* Level aggregation problems

---

# 🏷 Tags

* Tree
* BFS
* Queue
* Level Order Traversal

---

# 🔥 Recognition Pattern

Use this approach when:

* Need level sums/averages
* Need level-wise processing
* Visibility or grouping by levels
