# 🧩 Path Sum III

### 🔗 Problem Link

https://leetcode.com/problems/path-sum-iii/

---

# 🧠 Problem Understanding

Given:

* Binary tree
* Target sum

👉 Goal:

```text id="1ps3"
Count number of downward paths
whose sum equals targetSum
```

---

# ❗ Important Rules

---

## Path must go:

```text id="2ps3"
parent → child
```

ONLY downward

---

## BUT path can:

```text id="3ps3"
start from ANY node
end at ANY node
```

---

# 🔍 Example

```text id="4ps3"
        10
       /  \
      5   -3
     / \    \
    3   2    11
   / \   \
  3  -2   1
```

Target:

```text id="5ps3"
8
```

---

## Valid paths

```text id="6ps3"
5 → 3
5 → 2 → 1
-3 → 11
```

👉 Answer:

```text id="7ps3"
3
```

---

# 🧠 Why this problem is tricky

Unlike normal Path Sum:

```text id="8ps3"
root → leaf
```

Here:

```text id="9ps3"
start and end are NOT fixed
```

---

# ⚡ Key Observation

At every node:

👉 We must ask:

```text id="10ps3"
“How many valid paths start from THIS node?”
```

AND repeat this for every node

---

# 🧠 Core Strategy

Use TWO DFS traversals

---

# ⚡ DFS 1

```text id="11ps3"
Choose every node as starting point
```

---

# ⚡ DFS 2

```text id="12ps3"
Count valid downward paths from that node
```

---

# 🧠 Main Idea in DFS 2

Track:

```text id="13ps3"
remaining target
```

---

# 🔍 Example

Target:

```text id="14ps3"
8
```

Path:

```text id="15ps3"
5 → 2 → 1
```

---

## Step-by-step

At `5`:

```text id="16ps3"
remaining = 8 - 5 = 3
```

At `2`:

```text id="17ps3"
remaining = 3 - 2 = 1
```

At `1`:

```text id="18ps3"
remaining = 1
```

👉 Current node completes target:

```cpp id="19ps3"
root->val == remaining
```

✅ Valid path found

---

# ⚡ Why equality works

We are NOT checking:

```text id="20ps3"
current node alone equals target ❌
```

We are checking:

```text id="21ps3"
current node completes remaining target ✅
```

---

# ⚡ Approach

---

# 🧩 DFS helper function

```cpp id="22ps3"
int countPaths(TreeNode* root, long long remaining)
```

👉 Counts:

```text id="23ps3"
valid paths starting from current node
```

---

# 🧩 Base case

```cpp id="24ps3"
if (!root) return 0;
```

---

# 🧩 Check current node

```cpp id="25ps3"
if (root->val == remaining) {
    count++;
}
```

---

# 🧩 Update remaining

```cpp id="26ps3"
remaining -= root->val;
```

---

# 🧩 Explore children

```cpp id="27ps3"
count += countPaths(root->left, remaining);
count += countPaths(root->right, remaining);
```

---

# ⚡ Second DFS

```cpp id="28ps3"
pathSum(root, target)
```

👉 Tries:

```text id="29ps3"
every node as starting node
```

---

# 💻 Full Code

```cpp id="30ps3"
class Solution {
public:

    int countPaths(TreeNode* root, long long remaining) {

        if (!root) return 0;

        int count = 0;

        // path completed
        if (root->val == remaining) {
            count++;
        }

        remaining -= root->val;

        count += countPaths(root->left, remaining);
        count += countPaths(root->right, remaining);

        return count;
    }

    int pathSum(TreeNode* root, int targetSum) {

        if (!root) return 0;

        return
            countPaths(root, targetSum)
            + pathSum(root->left, targetSum)
            + pathSum(root->right, targetSum);
    }
};
```

---

# 🔍 Recursion Flow

For every node:

```text id="31ps3"
1. Try path starting here
2. Explore left subtree
3. Explore right subtree
```

---

# 🧠 Mental Model (REVISION KEY)

> “At every node, explore all downward sums”

---

# ⚡ Key Insights

---

## 🔹 Remaining target logic

```text id="32ps3"
remaining = target - pathSumSoFar
```

---

## 🔹 Equality meaning

```text id="33ps3"
current node completes required sum
```

---

## 🔹 Why two DFS?

One DFS:

```text id="34ps3"
chooses starting node
```

Other DFS:

```text id="35ps3"
explores paths from it
```

---

# ⏱ Complexity

Worst case:

```text id="36ps3"
O(n²)
```

(skewed tree)

---

# ❗ Common Mistakes

* ❌ Assuming path starts at root
* ❌ Assuming path ends at leaf
* ❌ Using `< remaining` logic
* ❌ Forgetting second DFS
* ❌ Confusing remaining target meaning

---

# 🧠 Patterns Learned

* DFS recursion
* Path exploration
* Remaining target technique
* Multi-recursion pattern

---

# 🏷 Tags

* Tree
* DFS
* Recursion
* Path Sum

---

# 🔥 Recognition Pattern

Use this approach when:

* Paths can start anywhere
* Need all possible path sums
* Downward recursive exploration
