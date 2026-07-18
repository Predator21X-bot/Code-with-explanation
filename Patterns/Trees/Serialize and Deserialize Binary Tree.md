# Serialize and Deserialize Binary Tree (LeetCode 297)

## Problem Statement

Design an algorithm to serialize and deserialize a binary tree.

- **Serialize:** Convert a binary tree into a string.
- **Deserialize:** Convert that string back into the original binary tree.

The deserialized tree must have the **exact same structure** as the original.

---

## Pattern Recognition

### Key Observation

To rebuild a tree, we need:

- The value of every node.
- The exact position of every missing child.

If we don't store `null` nodes, different trees can produce the same serialized string.

Example:

```
    1
     \
      2
```

Without null markers:

```
1,2
```

This is indistinguishable from:

```
    1
   /
  2
```

Therefore, we **must** store null children.

---

## Intuition

This problem is essentially the reverse of itself.

### Serialization

Traverse the tree using **Level Order Traversal (BFS)**.

For every node:

- Store its value.
- If the node is null, store `"null"`.
- Push both left and right children into the queue.

This preserves the exact structure.

Example:

```
      1
     / \
    2   3
       / \
      4   5
```

Serialized String:

```
1,2,3,null,null,4,5,null,null,null,null,
```

---

### Deserialization

Reverse the serialization process.

1. Split the string into individual values.
2. Create the root.
3. Traverse level-by-level using a queue.
4. For every parent:
   - Read the next value → left child.
   - Read the next value → right child.
   - Create nodes only if the value is not `"null"`.

Since serialization stored nodes in BFS order, deserialization rebuilds them in the same order.

---

# Dry Run

Tree:

```
      1
     / \
    2   3
       / \
      4   5
```

Serialized:

```
1,2,3,null,null,4,5,null,null,null,null,
```

Split into:

```
["1","2","3","null","null","4","5","null","null","null","null"]
```

Create root:

```
Queue:

1
```

Index = 1

---

### Process Parent = 1

Read:

```
2
3
```

Create:

```
      1
     / \
    2   3
```

Queue:

```
2
3
```

---

### Process Parent = 2

Read:

```
null
null
```

No children created.

Queue:

```
3
```

---

### Process Parent = 3

Read:

```
4
5
```

Create:

```
      3
     / \
    4   5
```

Queue:

```
4
5
```

Tree reconstructed successfully.

---

# Algorithm

## Serialization

1. If tree is empty, return empty string.
2. Push root into queue.
3. While queue is not empty:
   - Pop node.
   - If node is null:
     - Append `"null,"`
   - Else:
     - Append node value.
     - Push left child.
     - Push right child.
4. Return the serialized string.

---

## Deserialization

1. If string is empty, return `nullptr`.
2. Split string by commas into a vector.
3. Create root from first value.
4. Push root into queue.
5. Maintain an index pointing to the next unread value.
6. While queue is not empty:
   - Pop parent.
   - Read next value:
     - If not `"null"`, create left child.
   - Read next value:
     - If not `"null"`, create right child.
   - Push newly created children.
7. Return root.

---

# Code

```cpp
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {

        if (!root)
            return "";

        queue<TreeNode*> q;
        string ans;

        q.push(root);

        while (!q.empty()) {

            TreeNode* node = q.front();
            q.pop();

            if (node == nullptr) {
                ans += "null,";
            }
            else {
                ans += to_string(node->val) + ",";

                q.push(node->left);
                q.push(node->right);
            }
        }

        return ans;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {

        if (data.empty())
            return nullptr;

        vector<string> nodes;

        stringstream ss(data);
        string temp;

        while (getline(ss, temp, ',')) {
            nodes.push_back(temp);
        }

        TreeNode* root = new TreeNode(stoi(nodes[0]));

        queue<TreeNode*> q;
        q.push(root);

        int i = 1;

        while (!q.empty() && i < nodes.size()) {

            TreeNode* parent = q.front();
            q.pop();

            if (nodes[i] != "null") {

                parent->left = new TreeNode(stoi(nodes[i]));
                q.push(parent->left);
            }

            i++;

            if (i < nodes.size() && nodes[i] != "null") {

                parent->right = new TreeNode(stoi(nodes[i]));
                q.push(parent->right);
            }

            i++;
        }

        return root;
    }
};
```

---

# Complexity Analysis

### Serialization

- **Time:** `O(n)`
- **Space:** `O(n)`

---

### Deserialization

- **Time:** `O(n)`
- **Space:** `O(n)`

---

# Key Takeaways

- Serialization converts a tree into a string.
- Deserialization reconstructs the original tree from that string.
- **Null markers are essential** to preserve tree structure.
- BFS naturally processes nodes level-by-level, making reconstruction straightforward.
- Serialization and deserialization are mirror operations:
  - **Serialize:** Pop → Write → Push children.
  - **Deserialize:** Pop → Read → Create children.
- A queue is used in both processes to maintain level-order traversal.
- Overall complexity is **O(n)** time and **O(n)** space.
