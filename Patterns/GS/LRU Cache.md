# 🟦 LeetCode 146 — LRU Cache

**Difficulty:** Medium
**Pattern:** Hash Map + Doubly Linked List + Design
**Time Complexity:** `O(1)` for both `get()` and `put()`
**Space Complexity:** `O(capacity)`

The standard solution combines a **Hash Map** for `O(1)` key lookup with a **Doubly Linked List** for `O(1)` removal and movement of nodes. ([LeetCode][1])

---

# 📌 Problem

Design an LRU (Least Recently Used) cache that supports:

```cpp
get(key)
put(key, value)
```

When the cache reaches its capacity and we need to insert something new, we must remove the **least recently used** item. ([Code Review Stack Exchange][2])

Example:

```text
capacity = 2

put(1, 1)
put(2, 2)

get(1)

put(3, 3)
```

After `get(1)`, key `1` becomes recently used.

Therefore, when we insert `3`, key `2` gets removed:

```text
Cache:
1, 3

2 → evicted
```

---

# 🧠 First: What Does "Recently Used" Mean?

An item becomes **recently used** whenever we:

```text
get(key)
```

successfully access it, OR:

```text
put(key, value)
```

insert/update it.

So we need to maintain an ordering:

```text
LRU                         MRU
 ↓                           ↓
least recently used → most recently used
```

For example:

```text
[2] → [1] → [3]
 ↑                 ↑
LRU               MRU
```

Here:

```text
2 = least recently used
3 = most recently used
```

If we need to evict something:

```text
remove 2
```

---

# 🚨 Why Can't We Just Use a Hash Map?

A Hash Map gives us:

```cpp
map[key]
```

in approximately `O(1)`.

So `get(key)` is easy.

But the problem is **ordering**.

Suppose:

```text
map:

1 → 10
2 → 20
3 → 30
```

The map doesn't tell us:

> Which key was used least recently?

We need another data structure to maintain the order.

---

# 💡 Why Doubly Linked List?

A Doubly Linked List allows us to:

* remove a node in `O(1)`
* insert a node in `O(1)`
* move a node in `O(1)`

provided we already have a pointer to that node. ([NeetCode][3])

That's exactly what we need.

So:

```text
Hash Map
    +
Doubly Linked List
```

---

# ⭐ The Two Data Structures

We'll maintain:

```cpp
unordered_map<int, Node*> cache;
```

and:

```text
Doubly Linked List
```

The Hash Map stores:

```text
key → address of its Node
```

Example:

```text
cache:

1 → Node(1,10)
2 → Node(2,20)
3 → Node(3,30)
```

The linked list stores the **usage order**.

---

# 🔥 The Most Important Design Decision

Let's make:

```text
LEFT  = LRU
RIGHT = MRU
```

So:

```text
LRU                                      MRU
 ↓                                        ↓

LEFT → [1] ⇄ [2] ⇄ [3] ← RIGHT
```

When something is accessed:

```text
move it to RIGHT
```

When cache is full:

```text
remove node next to LEFT
```

This convention makes the code much easier to reason about. NeetCode's implementation similarly uses dummy left/right boundaries with the least recently used item near the left and most recently used near the right. ([NeetCode][3])

---

# ⭐ Why Do We Use Dummy Nodes?

We'll have:

```cpp
Node* left;
Node* right;
```

These are **dummy/sentinel nodes**.

Initially:

```text
left ⇄ right
```

After adding nodes:

```text
left ⇄ [1] ⇄ [2] ⇄ [3] ⇄ right
```

Here:

```text
left
 ↓
dummy boundary

right
 ↓
dummy boundary
```

They aren't actual cache entries.

They simply make insertion/removal easier because we don't need special cases for the first or last real node.

---

# 🧱 Node Structure

Each node needs:

```cpp
class Node {
public:
    int key;
    int value;

    Node* prev;
    Node* next;
};
```

Why do we store **both key and value**?

Because when we evict a node from the linked list, we also need to remove its key from the Hash Map.

For example:

```text
LRU node:

[key = 2, value = 20]
```

We need:

```cpp
cache.erase(2);
```

Therefore, the node must remember its key. This is an important implementation detail. ([NeetCode][3])

---

# 🧠 The Three Helper Operations

We'll create:

```cpp
remove(node)
```

```cpp
insert(node)
```

and use them to implement:

```cpp
get()
put()
```

---

# 1️⃣ Remove a Node

Suppose:

```text
A ⇄ B ⇄ C
```

and we want to remove `B`.

We need:

```text
A ⇄ C
```

So:

```cpp
node->prev->next = node->next;
node->next->prev = node->prev;
```

For `B`:

```text
B->prev = A
B->next = C
```

Therefore:

```cpp
A->next = C;
C->prev = A;
```

Done.

---

# 2️⃣ Insert at MRU Position

We want every newly used node immediately before `right`.

Suppose:

```text
left ⇄ A ⇄ B ⇄ right
```

Insert `C` at MRU:

```text
left ⇄ A ⇄ B ⇄ C ⇄ right
```

We need four pointer updates:

```cpp
node->prev = right->prev;
node->next = right;

right->prev->next = node;
right->prev = node;
```

This inserts `node` immediately before `right`.

---

# ⭐ Helper Functions

```cpp
void remove(Node* node) {

    node->prev->next = node->next;
    node->next->prev = node->prev;
}
```

And:

```cpp
void insert(Node* node) {

    node->prev = right->prev;
    node->next = right;

    right->prev->next = node;
    right->prev = node;
}
```

So:

```text
remove()
    ↓
take node out

insert()
    ↓
put node at MRU position
```

---

# 3️⃣ `get(key)`

What should `get()` do?

### If key doesn't exist

Return:

```cpp
-1
```

### If it exists

We need to:

1. Find node using Hash Map.
2. Remove it from its current position.
3. Insert it at MRU.
4. Return its value.

So:

```cpp
int get(int key) {

    if (cache.find(key) == cache.end()) {
        return -1;
    }

    Node* node = cache[key];

    remove(node);
    insert(node);

    return node->value;
}
```

---

# 🔥 Why Do We Move It During `get()`?

This is one of the most important things in LRU.

Suppose:

```text
capacity = 2

[1] ⇄ [2]
LRU     MRU
```

Now:

```cpp
get(1);
```

We're using key `1`.

Therefore `1` becomes **most recently used**.

So:

```text
Before:

[1] ⇄ [2]
 ↑       ↑
LRU     MRU
```

After:

```text
[2] ⇄ [1]
 ↑       ↑
LRU     MRU
```

If we don't move `1`, the cache would have incorrect usage information and could evict the wrong key later. ([NeetCode][3])

---

# 4️⃣ `put(key, value)`

There are two possibilities.

## Case A — Key already exists

Example:

```text
cache:

1 → 10
2 → 20
```

Call:

```cpp
put(1, 100);
```

We need:

```text
1 → 100
```

and because `1` was just used, it becomes MRU.

So:

```cpp
Node* node = cache[key];

remove(node);

node->value = value;

insert(node);
```

---

# Case B — Key Doesn't Exist

Suppose:

```text
capacity = 2

cache:
[1] ⇄ [2]
```

and:

```cpp
put(3, 30);
```

We're full.

So first remove the LRU node.

Because:

```text
left = dummy

left->next = LRU node
```

the LRU node is:

```cpp
Node* lru = left->next;
```

Then:

```cpp
remove(lru);
cache.erase(lru->key);
```

Now insert the new node:

```cpp
Node* node = new Node(3, 30);

cache[3] = node;

insert(node);
```

---

# 🧪 Complete Example

Capacity:

```text
2
```

### `put(1,1)`

```text
left ⇄ [1] ⇄ right
```

`1` is MRU.

---

### `put(2,2)`

```text
left ⇄ [1] ⇄ [2] ⇄ right
```

So:

```text
LRU              MRU
 ↓                ↓
 1                2
```

---

### `get(1)`

We access `1`.

Move it to MRU:

```text
left ⇄ [2] ⇄ [1] ⇄ right
```

Now:

```text
LRU = 2
MRU = 1
```

---

### `put(3,3)`

Cache is full.

LRU:

```text
2
```

Remove:

```text
left ⇄ [1] ⇄ right
```

Then insert `3`:

```text
left ⇄ [1] ⇄ [3] ⇄ right
```

So:

```text
1 → 1
3 → 3
```

and:

```cpp
get(2)
```

returns:

```text
-1
```

because `2` was evicted.

---

# ✅ Complete C++ Solution

```cpp
class LRUCache {
private:

    class Node {
    public:
        int key;
        int value;

        Node* prev;
        Node* next;

        Node(int k, int v) {
            key = k;
            value = v;
            prev = nullptr;
            next = nullptr;
        }
    };

    int capacity;

    // key -> node address
    unordered_map<int, Node*> cache;

    // Dummy nodes
    Node* left;   // LRU side
    Node* right;  // MRU side

    // Remove node from linked list
    void remove(Node* node) {

        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    // Insert node at MRU position
    void insert(Node* node) {

        node->prev = right->prev;
        node->next = right;

        right->prev->next = node;
        right->prev = node;
    }

public:

    LRUCache(int capacity) {

        this->capacity = capacity;

        left = new Node(0, 0);
        right = new Node(0, 0);

        left->next = right;
        right->prev = left;
    }

    int get(int key) {

        // Key doesn't exist
        if (cache.find(key) == cache.end()) {
            return -1;
        }

        Node* node = cache[key];

        // Mark as most recently used
        remove(node);
        insert(node);

        return node->value;
    }

    void put(int key, int value) {

        // Key already exists
        if (cache.find(key) != cache.end()) {

            Node* node = cache[key];

            remove(node);

            node->value = value;

            insert(node);

            return;
        }

        // Cache is full
        if (cache.size() == capacity) {

            Node* lru = left->next;

            remove(lru);

            cache.erase(lru->key);

            delete lru;
        }

        // Create new node
        Node* node = new Node(key, value);

        cache[key] = node;

        // New node is most recently used
        insert(node);
    }
};
```

---

# 🧠 Understand the Entire Architecture

The easiest way to remember the solution is:

```text
                  LRU CACHE
                     │
           ┌─────────┴─────────┐
           ↓                   ↓
       HASH MAP          DOUBLY LINKED LIST
           │                   │
           │                   │
       key → node          usage order
           │                   │
           ↓                   ↓
      O(1) lookup       O(1) remove/move
```

The Hash Map answers:

> **"Where is this key?"**

The Linked List answers:

> **"What is the usage order?"**

Together:

```text
Hash Map + Doubly Linked List
           ↓
        O(1) get
        O(1) put
```

That's the entire reason we need **both** data structures. ([LeetCode][1])

---

# 🔥 The Most Important Mapping

Remember this:

```text
LEFT                              RIGHT
 ↓                                  ↓
LRU                                MRU

dummy ⇄ LRU ⇄ ... ⇄ MRU ⇄ dummy
```

Then:

### `get(key)`

```text
Find node
   ↓
Remove node
   ↓
Insert at RIGHT
```

### `put(existing key)`

```text
Find node
   ↓
Update value
   ↓
Move to RIGHT
```

### `put(new key)`

```text
Cache full?
   ↓
YES → remove LEFT->next
   ↓
Insert new node before RIGHT
```

---

# ⚠️ Common Mistakes

### 1. Forgetting to update recency on `get()`

Wrong:

```cpp
return cache[key]->value;
```

That retrieves the value but doesn't make the key MRU.

Correct:

```cpp
remove(node);
insert(node);
```

([NeetCode][3])

---

### 2. Forgetting to erase the evicted key from the map

When removing the LRU node:

```cpp
Node* lru = left->next;
remove(lru);
```

is **not enough**.

You also need:

```cpp
cache.erase(lru->key);
```

Otherwise the Hash Map still points to the removed node.

---

### 3. Storing only the value in the node

Don't make:

```cpp
Node {
    int value;
}
```

You also need:

```cpp
int key;
```

because when you evict the LRU node, you need its key to erase it from the Hash Map. ([NeetCode][3])

---

### 4. Forgetting one of the four pointer updates

For insertion:

```cpp
node->prev = right->prev;
node->next = right;

right->prev->next = node;
right->prev = node;
```

All four are necessary.

For removal:

```cpp
node->prev->next = node->next;
node->next->prev = node->prev;
```

Both directions must be updated.

---

# 🎯 Interview Cheat Sheet

### Data Structures

```cpp
unordered_map<int, Node*> cache;
```

```text
key → node
```

and:

```text
Doubly Linked List
```

```text
LRU ⇄ ... ⇄ MRU
```

### `get`

```cpp
if (!cache.count(key))
    return -1;

Node* node = cache[key];

remove(node);
insert(node);

return node->value;
```

### `put`

Existing:

```cpp
remove(node);
node->value = value;
insert(node);
```

New + full:

```cpp
Node* lru = left->next;

remove(lru);
cache.erase(lru->key);
delete lru;
```

Then:

```cpp
Node* node = new Node(key, value);

cache[key] = node;
insert(node);
```

### Complexity

```text
get() → O(1)
put() → O(1)
Space → O(capacity)
```

---

# 🧠 Final Mental Model

Don't memorize 40 lines of code.

Memorize this picture:

```text
                    HASH MAP
                 key → Node*
                       │
                       ↓
LEFT                                              RIGHT
 │                                                  │
 ↓                                                  ↓
dummy ⇄ [LRU] ⇄ [ ... ] ⇄ [MRU] ⇄ dummy
             DOUBLY LINKED LIST
```

Then every operation follows naturally:

```text
GET
key → find node → remove → move to MRU → return value

PUT existing
key → find node → update → move to MRU

PUT new
if full → remove LRU → erase from map
        ↓
     create node
        ↓
     add to map
        ↓
     move to MRU
```

**The one sentence to remember:**

> **Hash Map gives me the node in `O(1)`; Doubly Linked List lets me change its position and evict the LRU node in `O(1)`.** ([LeetCode][1])

[1]: https://leetcode.doocs.org/en/lc/146/?utm_source=chatgpt.com "146. LRU Cache - LeetCode Wiki"
[2]: https://codereview.stackexchange.com/questions/206821/leetcode-146-lrucache-solution-in-java-doubly-linked-list-hashmap?rq=1&utm_source=chatgpt.com "programming challenge - Leetcode #146. LRUCache solution in Java (Doubly Linked List + HashMap) - Code Review Stack Exchange"
[3]: https://neetcode.io/solutions/lru-cache?utm_source=chatgpt.com "LeetCode 146 LRU Cache Solution & Explanation | NeetCode"
