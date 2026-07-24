# LeetCode 127 — Word Ladder

**Difficulty:** Hard
**Pattern:** Graph + Breadth First Search (BFS) + Shortest Path + Implicit Graph

---

# Problem Statement

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return the **length of the shortest transformation sequence** from `beginWord` to `endWord`.

Rules:

* Change only **one character** at a time.
* Every transformed word **must exist** in `wordList`.
* `beginWord` is **not** considered part of `wordList`.
* If no transformation exists, return **0**.

---

# Examples

### Example 1

```text
Input:
beginWord = "hit"
endWord = "cog"

wordList =
["hot","dot","dog","lot","log","cog"]

Output:
5

Sequence:
hit
↓
hot
↓
dot
↓
dog
↓
cog
```

---

### Example 2

```text
Input:
beginWord = "hit"
endWord = "cog"

wordList =
["hot","dot","dog","lot","log"]

Output:
0
```

Reason:

```text
cog is not present in the dictionary.
```

---

# Observations

* Every **word** can be treated as a **node**.
* Two words are connected if they differ by exactly **one character**.
* Every transformation has the **same cost (1)**.
* We need the **shortest path**.

This immediately suggests:

> **Breadth First Search (BFS)**

---

# Why BFS?

Whenever you see:

* Shortest path
* Unweighted graph
* Minimum number of operations
* Minimum transformations

Think:

> **BFS**

---

# Graph Representation

We **do not** build the graph explicitly.

Instead,

For every word,

Generate all possible words by changing one character at every position.

Example:

```text
hit

↓

ait
bit
cit
...
zit

↓

hat
hbt
...
hzt

↓

hia
hib
...
hiz
```

Only the generated words present in the dictionary are considered neighbors.

This is called an **Implicit Graph**.

---

# Algorithm

### Step 1

Store all dictionary words inside an `unordered_set`.

```cpp
unordered_set<string> dict(wordList.begin(), wordList.end());
```

Reason:

* O(1) average lookup
* O(1) deletion

---

### Step 2

If `endWord` is not present,

```cpp
if (!dict.count(endWord))
    return 0;
```

No valid path exists.

---

### Step 3

Initialize BFS.

```cpp
queue<string> q;

q.push(beginWord);

dict.erase(beginWord);

int level = 1;
```

Removing the word immediately prevents revisiting.

---

### Step 4

Process level by level.

```cpp
while (!q.empty())
{
    int size = q.size();

    while(size--)
    {

    }

    level++;
}
```

Every iteration of the outer loop corresponds to one transformation level.

---

### Step 5

Generate neighbors.

For every character position,

Try replacing it with every lowercase letter.

```cpp
for(int i = 0; i < word.length(); i++)
{
    string temp = word;

    for(char ch = 'a'; ch <= 'z'; ch++)
    {
        if(ch == word[i])
            continue;

        temp[i] = ch;

        ...
    }
}
```

---

### Step 6

If generated word exists,

```cpp
if(dict.count(temp))
{
    q.push(temp);
    dict.erase(temp);
}
```

Erase immediately to avoid duplicate processing.

---

### Step 7

If dequeued word equals `endWord`,

```cpp
if(word == endWord)
    return level;
```

Since BFS explores shortest paths first, this is guaranteed to be the minimum answer.

---

# Dry Run

```text
beginWord = hit

Queue:
hit

Level = 1
```

Process

```text
hit
```

Generate

```text
hot
```

Queue

```text
hot
```

Level

```text
2
```

---

Process

```text
hot
```

Generate

```text
dot
lot
```

Queue

```text
dot
lot
```

Level

```text
3
```

---

Process

```text
dot
lot
```

Generate

```text
dog
log
```

Queue

```text
dog
log
```

Level

```text
4
```

---

Process

```text
dog
```

Generate

```text
cog
```

Queue

```text
cog
```

Level

```text
5
```

---

Pop

```text
cog
```

Return

```text
5
```

---

# Why Remove From Set Immediately?

Suppose

```text
dot
```

can be reached from both

```text
hot

and

lot
```

If we don't erase immediately,

both will push

```text
dot
```

into the queue.

Result:

```text
dot
dot
dot
dot
```

Duplicate processing.

Removing immediately guarantees every word is visited **only once**.

---

# Complexity Analysis

Let

* **N** = Number of words
* **L** = Length of each word

For every word:

* Iterate over **L** positions.
* Try **26** letters.

Time:

```text
O(N × L × 26)
```

Since 26 is constant,

> **O(N × L)**

Space:

```text
Queue
+
HashSet
```

Both can store at most **N** words.

> **O(N)**

---

# C++ Solution

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {

        unordered_set<string> dict(wordList.begin(), wordList.end());

        if (!dict.count(endWord))
            return 0;

        queue<string> q;
        q.push(beginWord);

        dict.erase(beginWord);

        int level = 1;

        while (!q.empty()) {

            int size = q.size();

            while (size--) {

                string word = q.front();
                q.pop();

                if (word == endWord)
                    return level;

                for (int i = 0; i < word.length(); i++) {

                    string temp = word;

                    for (char ch = 'a'; ch <= 'z'; ch++) {

                        if (ch == word[i])
                            continue;

                        temp[i] = ch;

                        if (dict.count(temp)) {
                            q.push(temp);
                            dict.erase(temp);
                        }
                    }
                }
            }

            level++;
        }

        return 0;
    }
};
```

---

# Key Interview Takeaways

* Treat words as **graph nodes**.
* Build the graph **implicitly** by generating neighbors.
* **Unweighted shortest path ⇒ BFS**.
* Use `unordered_set` for **O(1)** lookup.
* Remove words from the dictionary **as soon as they are enqueued**.
* Process BFS **level by level** so the first time `endWord` is reached is guaranteed to be the shortest transformation sequence.

---

# Pattern Recognition

| Clue in Problem             | Pattern               |
| --------------------------- | --------------------- |
| Shortest transformation     | BFS                   |
| Unweighted graph            | BFS                   |
| One operation per move      | BFS                   |
| Generate neighboring states | Implicit Graph        |
| Avoid revisiting            | HashSet / Visited Set |

---

# Similar Problems

* LeetCode 752 — Open the Lock
* LeetCode 433 — Minimum Genetic Mutation
* LeetCode 1091 — Shortest Path in Binary Matrix
* LeetCode 127 — Word Ladder II (Hard)
* LeetCode 815 — Bus Routes
* LeetCode 909 — Snakes and Ladders

---

## 💡 Interview Tip

A good rule of thumb is:

> If a problem asks for the **minimum number of steps, moves, transformations, or operations**, and each move has the **same cost**, think **BFS** first. When the graph isn't explicitly given but can be generated from the current state (like changing one letter), it's an **implicit graph**, and generating neighbors on the fly is often the optimal approach.
