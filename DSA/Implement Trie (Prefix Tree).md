# Implement Trie (Prefix Tree)

## Problem Summary

We must implement a:

# Trie (Prefix Tree)

supporting operations:

* insert(word)
* search(word)
* startsWith(prefix)

---

# What Is A Trie?

Trie is a:

# tree-based data structure for strings

especially useful for:

* prefixes
* autocomplete
* dictionary search
* word suggestions

---

# MOST Important Idea

Strings with:

```text id="m1r8v4"
common prefixes
```

share the same path.

---

# Example

Insert:

```text id="q7m2k5"
"app"
"apple"
```

Trie:

```text id="x4m9r1"
root
 |
 a
 |
 p
 |
 p*
 |
 l
 |
 e*
```

`*` means:

```text id="t8m1k6"
word ends here
```

---

# Important Observation

Prefix:

```text id="f5m7r2"
"app"
```

is stored only once.

This is why Trie is efficient.

---

# Core Operations

---

# 1. insert(word)

Add word into trie.

---

# 2. search(word)

Check if COMPLETE word exists.

---

# 3. startsWith(prefix)

Check whether prefix path exists.

---

# MOST Important TrieNode Structure

Each node stores:

---

# 1. Children Links

Possible next characters.

Usually:

```cpp id="p2m8v4"
TrieNode* children[26];
```

---

# Why 26?

Because lowercase English letters:

```text id="v6m1r9"
a → z
```

---

# Meaning

```text id="m4r8k1"
children[0]
```

represents:

```text id="q9m2v7"
'a'
```

---

# children[1]

represents:

```text id="x3m7r5"
'b'
```

and so on.

---

# 2. End Of Word Marker

```cpp id="f8m1k2"
bool isEnd;
```

---

# Why Needed?

Suppose trie contains:

```text id="p5m9r1"
"apple"
```

Searching:

```text id="t2m7v4"
"app"
```

Path exists.

BUT:

```text id="n8m4r6"
"app" may not be complete word
```

Need:

```text id="r4m1k8"
isEnd
```

to distinguish.

---

# TrieNode Definition

```cpp id="q7m5v2"
class TrieNode {
public:

    TrieNode* children[26];

    bool isEnd;

    TrieNode() {

        for(int i = 0; i < 26; i++){

            children[i] = nullptr;
        }

        isEnd = false;
    }
};
```

---

# Why `nullptr`?

Means:

```text id="x1m8r4"
path does not exist yet
```

---

# Trie Structure

Trie stores:

```cpp id="m9r2k5"
TrieNode* root;
```

which is starting node.

---

# Trie Constructor

```cpp id="v5m7r1"
Trie() {

    root = new TrieNode();
}
```

---

# INSERT OPERATION

Suppose inserting:

```text id="f2m8v6"
"cat"
```

---

# Step 1 — Start At Root

```cpp id="t4m1r9"
TrieNode* node = root;
```

---

# Step 2 — Traverse Characters

```cpp id="q8m5r2"
for(char ch : word)
```

---

# Step 3 — Convert Character To Index

```cpp id="x6m1v7"
int index = ch - 'a';
```

---

# Example

```text id="m3r8k1"
'c' - 'a' = 2
```

---

# Step 4 — Create Path If Missing

```cpp id="p7m2v4"
if(node->children[index] == nullptr){

    node->children[index] =
    new TrieNode();
}
```

---

# Why?

Path does not exist yet.

So create node.

---

# Step 5 — Move Forward

```cpp id="f4m9r2"
node = node->children[index];
```

---

# Step 6 — Mark Word End

After processing all characters:

```cpp id="q1m7k8"
node->isEnd = true;
```

Very important.

---

# Complete Insert Function

```cpp id="v8m4r1"
void insert(string word) {

    TrieNode* node = root;

    for(char ch : word){

        int index = ch - 'a';

        if(node->children[index] == nullptr){

            node->children[index] =
            new TrieNode();
        }

        node = node->children[index];
    }

    node->isEnd = true;
}
```

---

# SEARCH OPERATION

Goal:

Check whether COMPLETE word exists.

---

# Step 1 — Start At Root

```cpp id="n5m2v7"
TrieNode* node = root;
```

---

# Step 2 — Traverse Characters

```cpp id="x2m8r5"
for(char ch : word)
```

---

# Step 3 — Find Index

```cpp id="m6r1k9"
int index = ch - 'a';
```

---

# Step 4 — Missing Path?

```cpp id="t8m4v2"
if(node->children[index] == nullptr){

    return false;
}
```

---

# Why?

Word path does not exist.

---

# Step 5 — Move Forward

```cpp id="q3m7r1"
node = node->children[index];
```

---

# Step 6 — Final Check

After traversal:

```cpp id="v9m2k4"
return node->isEnd;
```

---

# Why `isEnd`?

Path existence alone is insufficient.

Need complete word ending.

---

# Complete Search Function

```cpp id="f1m8r6"
bool search(string word) {

    TrieNode* node = root;

    for(char ch : word){

        int index = ch - 'a';

        if(node->children[index] == nullptr){

            return false;
        }

        node = node->children[index];
    }

    return node->isEnd;
}
```

---

# startsWith OPERATION

Very similar to search.

Difference:

```text id="p4m7v2"
no need to check isEnd
```

If prefix path exists:

```text id="x7m1r5"
return true
```

---

# Complete startsWith Function

```cpp id="m2r9k8"
bool startsWith(string prefix) {

    TrieNode* node = root;

    for(char ch : prefix){

        int index = ch - 'a';

        if(node->children[index] == nullptr){

            return false;
        }

        node = node->children[index];
    }

    return true;
}
```

---

# Complete Final Solution

```cpp id="q5m8v1"
class TrieNode {
public:

    TrieNode* children[26];

    bool isEnd;

    TrieNode() {

        for(int i = 0; i < 26; i++){

            children[i] = nullptr;
        }

        isEnd = false;
    }
};

class Trie {
public:

    TrieNode* root;

    Trie() {

        root = new TrieNode();
    }

    void insert(string word) {

        TrieNode* node = root;

        for(char ch : word){

            int index = ch - 'a';

            if(node->children[index] == nullptr){

                node->children[index] =
                new TrieNode();
            }

            node = node->children[index];
        }

        node->isEnd = true;
    }

    bool search(string word) {

        TrieNode* node = root;

        for(char ch : word){

            int index = ch - 'a';

            if(node->children[index] == nullptr){

                return false;
            }

            node = node->children[index];
        }

        return node->isEnd;
    }

    bool startsWith(string prefix) {

        TrieNode* node = root;

        for(char ch : prefix){

            int index = ch - 'a';

            if(node->children[index] == nullptr){

                return false;
            }

            node = node->children[index];
        }

        return true;
    }
};
```

---

# Complexity Analysis

Suppose word length = `L`

---

# Insert

```text id="t1m7r4"
O(L)
```

---

# Search

```text id="f8m2k5"
O(L)
```

---

# startsWith

```text id="v4m9r1"
O(L)
```

---

# Space Complexity

Depends on total characters inserted.

Worst case:

```text id="n6m1v8"
O(total characters)
```

---

# Important Pattern Learned

Trie is extremely useful for:

* prefix search
* autocomplete
* dictionary lookup
* word games

---

# Similar Problems

* Word Search II
* Design Add and Search Words
* Replace Words
* Maximum XOR Using Trie
* Stream of Characters

---

# Common Mistakes

## Mistake 1

Forgetting:

```cpp id="x5m2r7"
isEnd
```

Without it:

```text id="k4m9v2"
cannot distinguish prefix vs complete word
```

---

## Mistake 2

Using:

```cpp id="w8m1r3"
children[26]
```

without initializing to `nullptr`.

Leads to garbage pointers.

---

## Mistake 3

Forgetting to move pointer:

```cpp id="q6m2v7"
node = node->children[index];
```

inside loop.

---

## Mistake 4

Confusing:

```text id="t8m1k5"
search
```

with:

```text id="f2m7r9"
startsWith
```

Search requires:

```cpp id="m5r8k1"
isEnd == true
```

startsWith only requires path existence.

---

# Recognition Pattern

Whenever problem involves:

* prefixes
* many string queries
* autocomplete
* dictionary words

think:

# Trie

---

# Important Mental Models

## TrieNode Represents

```text id="x7m4v2"
one character position
```

---

# Path From Root Represents

```text id="q9m1r6"
a prefix/string
```

---

# Shared Prefixes

Stored only once.

Huge optimization.

---

# Biggest Takeaway

Trie transforms strings into:

# navigable character paths

allowing extremely efficient prefix operations.
