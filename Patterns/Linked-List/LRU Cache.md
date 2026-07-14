# 146. LRU Cache (Medium)

**Problem Link:** https://leetcode.com/problems/lru-cache/

---

# Pattern Recognition

### Pattern
- **HashMap + Doubly Linked List**
- **Design Data Structure**

### Why?

The cache must support:

- `get(key)` → O(1)
- `put(key, value)` → O(1)

A single data structure cannot achieve both requirements.

---

# Problem Statement

Design an LRU (Least Recently Used) Cache that supports:

- `get(key)`
- `put(key, value)`

If the cache reaches its capacity:

- Remove the **Least Recently Used (LRU)** item.

Whenever a key is accessed or updated, it becomes the **Most Recently Used (MRU)**.

---

# Intuition

Imagine a queue of recently used items.

```text
Head (MRU) ---------------------- Tail (LRU)

Most Recently Used      Least Recently Used
```

Whenever we access an item:

Move it to the front.

Whenever the cache is full:

Remove the node at the back.

---

# Why not use only a HashMap?

HashMap provides:

- O(1) lookup
- O(1) insertion

Example

```text
{
1 : 100
2 : 200
3 : 300
}
```

Question:

Which key is the least recently used?

Impossible to determine.

HashMap stores **key → value**, not usage order.

---

# Why not use only a Doubly Linked List?

Suppose

```text
Head <-> 5 <-> 2 <-> 7 <-> Tail
```

Finding

```cpp
get(2)
```

requires traversing the list.

Worst Case:

```text
O(n)
```

Not acceptable.

---

# Combining Both Data Structures

HashMap

```text
key
 ↓
Node*
```

The HashMap stores:

```cpp
unordered_map<int, Node*>
```

This gives O(1) access to any node.

The Doubly Linked List maintains the order of usage.

---

# Data Structure

```text
HashMap

key
 ↓
Node*
        │
        ▼

DummyHead
    ⇅
Most Recently Used
    ⇅
...
    ⇅
Least Recently Used
    ⇅
DummyTail
```

---

# Node Structure

Each node stores:

```cpp
class Node {

public:

    int key;

    int value;

    Node* prev;

    Node* next;
};
```

### Why store the key?

Suppose the least recently used node is removed.

We must erase it from the HashMap.

```cpp
mp.erase(node->key);
```

Without storing the key, we would not know which entry to erase.

---

# Why Dummy Head & Dummy Tail?

Instead of

```text
NULL
```

we maintain

```text
DummyHead <-------> DummyTail
```

Advantages:

- No null pointer checks.
- No special handling for first or last node.
- Every insertion and deletion becomes identical.

---

# Helper Functions

## remove(node)

Removes a node from the DLL.

Before

```text
A <-> Node <-> B
```

After

```text
A <---------> B
```

Implementation

```cpp
node->prev->next = node->next;
node->next->prev = node->prev;
```

---

## insert(node)

Always inserts immediately after Dummy Head.

Before

```text
DummyHead <-> A
```

After inserting X

```text
DummyHead <-> X <-> A
```

Implementation

```cpp
node->next = head->next;

node->prev = head;

head->next->prev = node;

head->next = node;
```

---

# Constructor

Initialize the dummy nodes.

```cpp
head = new Node(0,0);

tail = new Node(0,0);

head->next = tail;

tail->prev = head;
```

Initially

```text
DummyHead <-------> DummyTail
```

---

# get(key)

## Case 1

Key does not exist.

Return

```cpp
-1
```

---

## Case 2

Key exists.

Steps

1. Find node using HashMap.
2. Remove it from its current position.
3. Insert it at the front.
4. Return its value.

Algorithm

```text
Find

↓

Remove

↓

Insert at Front

↓

Return Value
```

---

# put(key, value)

There are three cases.

---

## Case 1

Key already exists.

Steps

1. Update value.
2. Remove node.
3. Insert node at front.

---

## Case 2

Key doesn't exist and cache is NOT full.

Steps

1. Create node.
2. Insert at front.
3. Store in HashMap.

---

## Case 3

Key doesn't exist and cache IS full.

Steps

1. Find LRU node (`tail->prev`).
2. Remove it from DLL.
3. Remove from HashMap.
4. Delete node.
5. Create new node.
6. Insert at front.
7. Store in HashMap.

---

# Complete Algorithm

## get(key)

```text
Does key exist?

No
    Return -1

Yes
    Remove node
    Insert node at front
    Return value
```

---

## put(key,value)

```text
Does key exist?

Yes

    Update value

    Move node to front

No

    Is cache full?

    Yes

        Remove LRU

        Erase from HashMap

    Create new node

    Insert at front

    Store in HashMap
```

---

# Dry Run

Capacity = 2

```
put(1,1)

Head <-> 1 <-> Tail
```

```
put(2,2)

Head <-> 2 <-> 1 <-> Tail
```

```
get(1)
```

Move 1 to front

```text
Head <-> 1 <-> 2 <-> Tail
```

```
put(3,3)
```

Cache full.

Remove LRU

```text
2
```

Insert 3

```text
Head <-> 3 <-> 1 <-> Tail
```

---

# Code

```cpp
class LRUCache {

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

    unordered_map<int, Node*> mp;

    int capacity;

    Node* head;
    Node* tail;

    void remove(Node* node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void insert(Node* node) {

        node->next = head->next;

        node->prev = head;

        head->next->prev = node;

        head->next = node;
    }

public:

    LRUCache(int capacity) {

        this->capacity = capacity;

        head = new Node(0, 0);
        tail = new Node(0, 0);

        head->next = tail;
        tail->prev = head;
    }

    int get(int key) {

        if (mp.find(key) == mp.end())
            return -1;

        Node* node = mp[key];

        remove(node);
        insert(node);

        return node->value;
    }

    void put(int key, int value) {

        if (mp.find(key) != mp.end()) {

            Node* node = mp[key];

            node->value = value;

            remove(node);
            insert(node);

            return;
        }

        if (mp.size() == capacity) {

            Node* lru = tail->prev;

            remove(lru);

            mp.erase(lru->key);

            delete lru;
        }

        Node* node = new Node(key, value);

        insert(node);

        mp[key] = node;
    }
};
```

---

# Complexity

| Operation | Time |
|-----------|------|
| get() | O(1) |
| put() | O(1) |

Space

```text
O(capacity)
```

---

# Key Takeaways

- A HashMap alone cannot maintain usage order.
- A Doubly Linked List alone cannot find nodes in O(1).
- Combining both provides O(1) `get()` and `put()`.
- Store **key + value** inside each node.
- Use **dummy head** and **dummy tail** to eliminate edge cases.
- Every successful `get()` and `put()` moves the node to the **front (MRU)**.
- When full, always remove `tail->prev`, the **Least Recently Used (LRU)** node.

---

# Similar Problems

- LFU Cache
- Design Browser History
- All O(1) Data Structure
- Insert Delete GetRandom O(1)
- Design Circular Deque
