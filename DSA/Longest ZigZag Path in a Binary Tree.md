# 🧩 Longest ZigZag Path in a Binary Tree

### 🔗 Problem Link

https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/

---

# 🧠 Problem Understanding

A ZigZag path means:

```text id="zig101"
LEFT → RIGHT → LEFT → RIGHT
```

OR

```text id="zig102"
RIGHT → LEFT → RIGHT → LEFT
```

👉 Directions must alternate

---

# ❗ Important

Path length counts:

```text id="zig103"
edges, NOT nodes
```

---

# 🔍 Example

```text id="zig104"
        1
       /
      2
       \
        3
       /
      4
```

Path:

```text id="zig105"
1 → 2  (LEFT)
2 → 3  (RIGHT)
3 → 4  (LEFT)
```

✅ Perfect ZigZag

Length:

```text id="zig106"
3
```

---

# 🧠 Key Observation

At every node, we need:

```text id="zig107"
1. previous direction
2. current zigzag length
```

---

# ⚡ Core Idea

If directions alternate:

```text id="zig108"
continue zigzag
→ length + 1
```

---

If same direction repeats:

```text id="zig109"
restart zigzag
→ length = 1
```

---

# 🔥 Visual Intuition

```text id="zig110"
LEFT → RIGHT → LEFT ✅

LEFT → LEFT ❌ restart
```

---

# ⚡ DFS with State

We use recursion carrying:

```cpp id="zig111"
dfs(node, previousDirection, currentLength)
```

---

# 🧠 Parameters

---

## `node`

```text id="zig112"
current node
```

---

## `wentLeft`

```text id="zig113"
previous move direction
```

---

### `true`

```text id="zig114"
previous move was LEFT
```

---

### `false`

```text id="zig115"
previous move was RIGHT
```

---

## `length`

```text id="zig116"
current zigzag streak length
```

---

# ⚡ Global Answer

```cpp id="zig117"
int ans = 0;
```

Tracks:

```text id="zig118"
maximum zigzag length found
```

---

# ⚡ DFS Logic

---

# 🧩 Base case

```cpp id="zig119"
if (!node) return;
```

---

# 🧩 Update answer

```cpp id="zig120"
ans = max(ans, length);
```

---

# 🧩 Move LEFT

---

## If previous move was LEFT

```text id="zig121"
LEFT → LEFT
```

❌ ZigZag breaks

Restart:

```cpp id="zig122"
dfs(node->left, true, 1);
```

---

## Else

```text id="zig123"
RIGHT → LEFT
```

✅ Continue ZigZag

```cpp id="zig124"
dfs(node->left, true, length + 1);
```

---

# 🧩 Move RIGHT

---

## If previous move was RIGHT

```text id="zig125"
RIGHT → RIGHT
```

❌ Restart

```cpp id="zig126"
dfs(node->right, false, 1);
```

---

## Else

```text id="zig127"
LEFT → RIGHT
```

✅ Continue

```cpp id="zig128"
dfs(node->right, false, length + 1);
```

---

# 💻 Full Code

```cpp id="zig129"
class Solution {
public:

    int ans = 0;

    void dfs(TreeNode* node, bool wentLeft, int length) {

        if (!node) return;

        ans = max(ans, length);

        // go left
        if (node->left) {

            if (wentLeft) {
                dfs(node->left, true, 1); // restart
            } else {
                dfs(node->left, true, length + 1); // continue
            }
        }

        // go right
        if (node->right) {

            if (!wentLeft) {
                dfs(node->right, false, 1); // restart
            } else {
                dfs(node->right, false, length + 1); // continue
            }
        }
    }

    int longestZigZag(TreeNode* root) {

        dfs(root, true, 0);
        dfs(root, false, 0);

        return ans;
    }
};
```

---

# 🔍 Step-by-Step Dry Run

Tree:

```text id="zig130"
        1
       /
      2
       \
        3
       /
      4
```

---

# 🧩 Start

```cpp id="zig131"
dfs(1, true, 0)
```

---

## Move LEFT

```text id="zig132"
LEFT → LEFT
```

❌ Restart

```cpp id="zig133"
dfs(2, true, 1)
```

---

## Move RIGHT

```text id="zig134"
LEFT → RIGHT
```

✅ Continue

```cpp id="zig135"
dfs(3, false, 2)
```

---

## Move LEFT

```text id="zig136"
RIGHT → LEFT
```

✅ Continue

```cpp id="zig137"
dfs(4, true, 3)
```

---

# ✅ Final Answer

```text id="zig138"
3
```

---

# 🧠 Mental Model (REVISION KEY)

> “Alternate direction = continue, same direction = restart”

---

# ⚡ Why restart uses `1`

Because:

```text id="zig139"
current edge itself starts new zigzag
```

---

# ⚡ Why two initial DFS calls?

```cpp id="zig140"
dfs(root, true, 0);
dfs(root, false, 0);
```

Because:

```text id="zig141"
first move could be LEFT or RIGHT
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space:

  * recursion stack → **O(h)**

---

# ❗ Common Mistakes

* ❌ Confusing nodes vs edges
* ❌ Forgetting restart logic
* ❌ Not tracking previous direction
* ❌ Updating answer incorrectly

---

# 🧠 Patterns Learned

* DFS with state
* Direction tracking
* Recursive path continuation

---

# 🏷 Tags

* Tree
* DFS
* Recursion
* Dynamic State

---

# 🔥 Recognition Pattern

Use this approach when:

* Traversal depends on previous move
* Need path streak tracking
* Alternate-condition problems
