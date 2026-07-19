# Clone Graph (LeetCode 133)

## Problem Statement

Given a reference to a node in a **connected undirected graph**, return a **deep copy (clone)** of the graph.

Each node contains:

- `val`
- `vector<Node*> neighbors`

The cloned graph should:

- Have the same structure.
- Have the same values.
- Be completely independent of the original graph.

---

# Pattern Recognition

### Key Observation

The graph may contain **cycles**.

Example:

```
    1 ----- 2
    |       |
    |       |
    4 ----- 3
```

If we recursively clone nodes without remembering previously cloned nodes, we'll keep revisiting the same nodes forever.

### Pattern

```
Graph
   ↓
DFS Traversal
   ↓
Memoization (HashMap)
   ↓
Deep Copy
```

---

# Intuition

We start from any node.

For every node:

1. Create its clone.
2. Store the mapping:

```
Original Node  →  Cloned Node
```

3. Recursively clone all neighbors.
4. Connect the cloned neighbors to the cloned node.

If we encounter a node that has already been cloned, simply return the existing clone instead of creating another one.

---

# Why do we need a HashMap?

Consider the graph:

```
1 ----- 2
|       |
|       |
4 ----- 3
```

Suppose we start cloning node `1`.

```
clone(1)
```

While cloning node `2`, we reach node `1` again.

Without remembering previously cloned nodes:

```
clone(1)
    ↓
clone(2)
    ↓
clone(1)
    ↓
clone(2)
    ↓
...
```

This causes **infinite recursion**.

---

# HashMap

We store

```cpp
unordered_map<Node*, Node*> mp;
```

Meaning:

```
Original Node        Cloned Node

Node1 -------------> Node1'
Node2 -------------> Node2'
Node3 -------------> Node3'
Node4 -------------> Node4'
```

Whenever we revisit a node:

```cpp
if (mp.find(node) != mp.end())
    return mp[node];
```

We immediately return its clone.

---

# Why use `Node*` as the key?

Instead of

```cpp
unordered_map<int, Node*>
```

we use

```cpp
unordered_map<Node*, Node*>
```

### Reason

Node values are **not guaranteed to be unique**.

Example:

```
      (2)

     /   \

   (2)   (2)
```

If we use

```cpp
unordered_map<int, Node*>
```

all nodes with value `2` would share the same key.

Using the node pointer uniquely identifies every node.

---

# Why insert into the map BEFORE recursion?

We create the clone:

```cpp
Node* clone = new Node(node->val);
```

Immediately store:

```cpp
mp[node] = clone;
```

before visiting neighbors.

This prevents cycles.

If a recursive call reaches this node again, it immediately returns the existing clone.

---

# Understanding the Neighbor Loop

Suppose the original graph is

```
    1
   / \
  2   4
```

Original node

```
Node1

neighbors

[Node2, Node4]
```

After creating the clone:

```cpp
Node* clone = new Node(node->val);
```

we have

```
Clone1

neighbors

[]
```

(empty vector)

Now iterate through every original neighbor:

```cpp
for (Node* neighbor : node->neighbors)
```

### First iteration

```
neighbor = Node2
```

Clone it:

```cpp
cloneGraph(neighbor)
```

returns

```
Node2'
```

Add it:

```cpp
clone->neighbors.push_back(Node2');
```

Now

```
Clone1

neighbors

[Node2']
```

---

### Second iteration

```
neighbor = Node4
```

Recursive call returns

```
Node4'
```

Add it

```cpp
clone->neighbors.push_back(Node4');
```

Now

```
Clone1

neighbors

[Node2', Node4']
```

The cloned node now has the exact same connections as the original.

---

# Why don't we push the original neighbor?

Incorrect:

```cpp
clone->neighbors.push_back(neighbor);
```

This creates

```
Clone1

↓

Original Node2
```

The cloned graph would point back into the original graph.

Instead we write

```cpp
clone->neighbors.push_back(cloneGraph(neighbor));
```

which adds

```
Clone1

↓

Clone2
```

This creates a true **deep copy**.

---

# Algorithm

## Base Cases

If the node is null:

```cpp
return nullptr;
```

If the node has already been cloned:

```cpp
return mp[node];
```

---

## Recursive Step

1. Create clone.

```cpp
Node* clone = new Node(node->val);
```

2. Store mapping.

```cpp
mp[node] = clone;
```

3. Clone every neighbor.

```cpp
for (Node* neighbor : node->neighbors)
{
    clone->neighbors.push_back(cloneGraph(neighbor));
}
```

4. Return clone.

---

# Dry Run

Graph

```
1 ----- 2
```

Start

```
cloneGraph(1)
```

Create

```
1'
```

Store

```
1 → 1'
```

Visit neighbor

```
2
```

Create

```
2'
```

Store

```
2 → 2'
```

Visit neighbor

```
1
```

Already exists in map.

Return

```
1'
```

Connect

```
2' → 1'
```

Return

```
2'
```

Connect

```
1' → 2'
```

Finished.

---

# Code

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;

    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }

    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }

    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
*/

class Solution {
public:
    unordered_map<Node*, Node*> mp;

    Node* cloneGraph(Node* node) {

        if (node == nullptr)
            return nullptr;

        if (mp.find(node) != mp.end())
            return mp[node];

        Node* clone = new Node(node->val);

        mp[node] = clone;

        for (Node* neighbor : node->neighbors) {
            clone->neighbors.push_back(cloneGraph(neighbor));
        }

        return clone;
    }
};
```

---

# Complexity Analysis

### Time Complexity

Each node is cloned exactly once.

Each edge is visited once.

```
O(V + E)
```

where

- `V` = number of vertices
- `E` = number of edges

---

### Space Complexity

HashMap:

```
O(V)
```

Recursive call stack:

```
O(V)
```

Worst case:

```
O(V)
```

---

# Key Takeaways

- Clone Graph is a **DFS + HashMap (Memoization)** problem.
- The graph may contain **cycles**, so naïve recursion can cause infinite recursion.
- Use `unordered_map<Node*, Node*>` to map **original nodes to cloned nodes**.
- Insert the mapping **before** recursively cloning neighbors.
- Always connect cloned nodes to **cloned neighbors**, never to original neighbors.
- `clone->neighbors` is initially an empty vector created by the `Node` constructor and is populated during DFS.
- If a node is already present in the map, return the existing clone immediately.
- Overall complexity is **O(V + E)** since each node and edge is processed only once.
