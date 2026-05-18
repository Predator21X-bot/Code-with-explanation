# Search Suggestions System

## Problem Summary

We are given:

```cpp id="m1r8v4"
products
```

and:

```cpp id="q7m2k5"
searchWord
```

As user types each character of `searchWord`:

```text id="x4m9r1"
show at most 3 lexicographically smallest products
```

matching current prefix.

---

# Example

```text id="t8m1k6"
products =
["mobile","mouse","moneypot","monitor","mousepad"]

searchWord = "mouse"
```

---

# User Types

## Prefix `"m"`

Suggestions:

```text id="f5m7r2"
mobile
moneypot
monitor
```

---

# Prefix `"mo"`

Suggestions:

```text id="p2m8v4"
mobile
moneypot
monitor
```

---

# Prefix `"mou"`

Suggestions:

```text id="v6m1r9"
mouse
mousepad
```

---

# Final Output

```text id="m4r8k1"
[
 ["mobile","moneypot","monitor"],
 ["mobile","moneypot","monitor"],
 ["mouse","mousepad"],
 ["mouse","mousepad"],
 ["mouse","mousepad"]
]
```

---

# MOST Important Observation

Every growing prefix needs:

# its own suggestion list

So return type becomes:

```cpp id="q9m2v7"
vector<vector<string>>
```

---

# MOST Important Insight

If products are:

# sorted lexicographically first

then first inserted products automatically become:

```text id="x3m7r5"
smallest suggestions
```

Very important simplification.

---

# Core Idea

Use:

# Trie (Prefix Tree)

Each Trie node stores:

```text id="f8m1k2"
top 3 suggestions for that prefix
```

---

# TrieNode Structure

Each node stores:

---

# 1. Children

```cpp id="p5m9r1"
TrieNode* children[26];
```

---

# Why 26?

Lowercase English letters:

```text id="t2m7v4"
a → z
```

---

# Meaning

```text id="n8m4r6"
children[0]
```

represents:

```text id="r4m1k8"
'a'
```

---

# 2. Suggestions List

```cpp id="q7m5v2"
vector<string> suggestions;
```

Stores:

```text id="x1m8r4"
at most 3 products
```

for current prefix.

---

# TrieNode Definition

```cpp id="m9r2k5"
class TrieNode {
public:

    TrieNode* children[26];

    vector<string> suggestions;

    TrieNode() {

        for(int i = 0; i < 26; i++){

            children[i] = nullptr;
        }
    }
};
```

---

# Why Initialize To nullptr?

Means:

```text id="v5m7r1"
path does not exist yet
```

---

# Important Mental Model

Each Trie node represents:

# one prefix

and stores:

```text id="f2m8v6"
best 3 products for that prefix
```

---

# VERY Important First Step

Sort products.

```cpp id="t4m1r9"
sort(products.begin(), products.end());
```

---

# Why Sort?

Suppose sorted products:

```text id="q8m5r2"
mobile
moneypot
monitor
mouse
mousepad
```

First inserted suggestions automatically become:

# lexicographically smallest

No extra sorting needed later.

---

# INSERT OPERATION

Suppose inserting:

```text id="x6m1v7"
"mouse"
```

---

# Step 1 — Start From Root

```cpp id="m3r8k1"
TrieNode* node = root;
```

---

# Step 2 — Traverse Characters

```cpp id="p7m2v4"
for(char ch : word)
```

---

# Step 3 — Convert Character To Index

```cpp id="f4m9r2"
int index = ch - 'a';
```

---

# Example

```text id="q1m7k8"
'm' - 'a' = 12
```

---

# Step 4 — Create Child If Missing

```cpp id="v8m4r1"
if(node->children[index] == nullptr){

    node->children[index] =
    new TrieNode();
}
```

---

# Step 5 — Move Forward

```cpp id="n5m2v7"
node = node->children[index];
```

---

# Step 6 — Store Suggestion

Only keep:

# top 3 products

Condition:

```cpp id="x2m8r5"
if(node->suggestions.size() < 3)
```

Then:

```cpp id="m6r1k9"
node->suggestions.push_back(word);
```

---

# Why Works?

Because products already sorted.

First 3 inserted automatically become smallest 3.

---

# Complete Insert Function

```cpp id="t8m4v2"
void insert(string word) {

    TrieNode* node = root;

    for(char ch : word){

        int index = ch - 'a';

        if(node->children[index] == nullptr){

            node->children[index] =
            new TrieNode();
        }

        node = node->children[index];

        if(node->suggestions.size() < 3){

            node->suggestions.push_back(word);
        }
    }
}
```

---

# SEARCH PROCESS

As user types:

```text id="q3m7r1"
m
mo
mou
...
```

Traverse trie.

At every node:

```text id="v9m2k4"
collect suggestions vector
```

---

# Important Case

Suppose prefix path disappears.

Then:

```text id="f1m8r6"
all remaining answers become empty lists
```

---

# Main Search Logic

```cpp id="p4m7v2"
TrieNode* node = root;

for(char ch : searchWord){

    int index = ch - 'a';

    if(node != nullptr){

        node = node->children[index];
    }

    if(node != nullptr){

        ans.push_back(node->suggestions);
    }
    else{

        ans.push_back({});
    }
}
```

---

# Complete Solution

```cpp id="x7m1r5"
class TrieNode {
public:

    TrieNode* children[26];

    vector<string> suggestions;

    TrieNode() {

        for(int i = 0; i < 26; i++){

            children[i] = nullptr;
        }
    }
};

class Solution {
public:

    TrieNode* root = new TrieNode();

    void insert(string word) {

        TrieNode* node = root;

        for(char ch : word){

            int index = ch - 'a';

            if(node->children[index] == nullptr){

                node->children[index] =
                new TrieNode();
            }

            node = node->children[index];

            if(node->suggestions.size() < 3){

                node->suggestions.push_back(word);
            }
        }
    }

    vector<vector<string>> suggestedProducts(
        vector<string>& products,
        string searchWord
    ) {

        sort(products.begin(), products.end());

        for(string product : products){

            insert(product);
        }

        vector<vector<string>> ans;

        TrieNode* node = root;

        for(char ch : searchWord){

            int index = ch - 'a';

            if(node != nullptr){

                node = node->children[index];
            }

            if(node != nullptr){

                ans.push_back(node->suggestions);
            }
            else{

                ans.push_back({});
            }
        }

        return ans;
    }
};
```

---

# Complexity Analysis

Suppose:

* `N` = number of products
* `L` = average product length

---

# Sorting

```text id="m2r9k8"
O(N log N)
```

---

# Trie Insert

```text id="q5m8v1"
O(N × L)
```

---

# Search

```text id="t1m7r4"
O(searchWord length)
```

---

# Space Complexity

Trie storage:

```text id="f8m2k5"
O(total characters)
```

---

# Important Pattern Learned

This is classic:

# Prefix Query Optimization

Very important Trie category.

---

# Similar Problems

* Implement Trie
* Word Search II
* Replace Words
* Stream of Characters
* Maximum XOR Using Trie

---

# Common Mistakes

## Mistake 1

Forgetting to sort products first.

Without sorting:

```text id="v4m9r1"
suggestions may not be lexicographically smallest
```

---

## Mistake 2

Storing ALL products at every node.

Need only:

```text id="n6m1v8"
top 3
```

---

## Mistake 3

Not handling missing path correctly.

After path disappears:

```text id="x5m2r7"
remaining answers are empty lists
```

---

## Mistake 4

Forgetting:

```cpp id="k4m9v2"
node = node->children[index];
```

inside traversal.

---

# Recognition Pattern

Whenever problem asks:

* autocomplete
* search suggestions
* prefix queries
* dictionary recommendations

think:

# Trie

---

# Important Mental Models

## Trie Node Represents

```text id="w8m1r3"
one prefix
```

---

# Suggestions Stored At Node

Represent:

```text id="q6m2v7"
best answers for that prefix
```

---

# Products Sorted First

Makes suggestion storage extremely easy.

---

# Biggest Takeaway

Trie allows us to:

# preprocess prefix answers once

so future suggestion queries become extremely fast.
