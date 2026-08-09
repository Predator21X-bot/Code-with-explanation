# 🟦 LeetCode 1086 — High Five

**Difficulty:** Easy
**Pattern:** Hash Map + Top-K
**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(n)`

---

## 📌 Problem

You are given scores in the form:

```cpp
[id, score]
```

Each student can have multiple scores.

For **each student**, calculate the **average of their top 5 scores**.

Return:

```text
[id, average]
```

sorted by student ID.

---

# Example

```text
scores = [
    [1,91],
    [1,92],
    [1,60],
    [1,96],
    [1,90],
    [1,95]
]
```

Student `1` has:

```text
91, 92, 60, 96, 90, 95
```

Top 5:

```text
96, 95, 92, 91, 90
```

Average:

```text
(96 + 95 + 92 + 91 + 90) / 5
= 464 / 5
= 92
```

Result:

```text
[[1,92]]
```

---

# 💡 Core Idea

We need to solve two separate problems:

### 1. Group scores by student

We need:

```text
student ID → scores
```

A Hash Map is natural:

```cpp
unordered_map<int, vector<int>> mp;
```

For every:

```cpp
[id, score]
```

we do:

```cpp
mp[id].push_back(score);
```

---

### 2. Find the top 5 scores

Once we have all scores for a student:

```text
[91,92,60,96,90,95]
```

we need:

```text
[96,95,92,91,90]
```

The simplest approach is to sort:

```cpp
sort(scores.begin(), scores.end(), greater<int>());
```

Then take the first 5.

---

# ⭐ Algorithm

```text
For every [id, score]:
        ↓
Store score in map[id]

For every student:
        ↓
Sort their scores in descending order
        ↓
Take first 5
        ↓
Calculate sum
        ↓
sum / 5
        ↓
Store [id, average]

Return results sorted by ID
```

---

# Step 1 — Group Scores

```cpp
unordered_map<int, vector<int>> mp;

for (auto& score : items) {
    int id = score[0];
    int value = score[1];

    mp[id].push_back(value);
}
```

Example:

```text
mp:

1 → [91,92,60,96,90,95]
2 → [93,99,98,97,92]
```

---

# Step 2 — Sort Each Student's Scores

```cpp
sort(mp[id].begin(), mp[id].end(), greater<int>());
```

After sorting:

```text
1 → [96,95,92,91,90,60]
```

Now the first five are automatically the top five.

---

# Step 3 — Calculate Top 5 Average

```cpp
int sum = 0;

for (int i = 0; i < 5; i++) {
    sum += scores[i];
}

int average = sum / 5;
```

Important:

The problem asks for an **integer average**, so integer division is sufficient.

For example:

```text
464 / 5 = 92
```

---

# Step 4 — Build Result

```cpp
ans.push_back({id, average});
```

---

# C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> highFive(vector<vector<int>>& items) {

        unordered_map<int, vector<int>> mp;

        // Group scores by student ID
        for (auto& item : items) {
            int id = item[0];
            int score = item[1];

            mp[id].push_back(score);
        }

        vector<vector<int>> ans;

        // Process each student
        for (auto& [id, scores] : mp) {

            // Sort scores in descending order
            sort(scores.begin(), scores.end(), greater<int>());

            // Sum top 5 scores
            int sum = 0;

            for (int i = 0; i < 5; i++) {
                sum += scores[i];
            }

            int average = sum / 5;

            ans.push_back({id, average});
        }

        // Sort result by student ID
        sort(ans.begin(), ans.end());

        return ans;
    }
};
```

---

# 🧪 Dry Run

Input:

```text
[
 [1,91],
 [1,92],
 [2,93],
 [1,96],
 [1,90],
 [2,99],
 [2,98],
 [2,97],
 [1,60],
 [2,92]
]
```

### Group by ID

```text
1 → [91,92,96,90,60]
2 → [93,99,98,97,92]
```

### Student 1

Sort:

```text
[96,92,91,90,60]
```

Top 5:

```text
96 + 92 + 91 + 90 + 60 = 429
```

Average:

```text
429 / 5 = 85
```

### Student 2

Sort:

```text
[99,98,97,93,92]
```

Average:

```text
(99+98+97+93+92) / 5
= 479 / 5
= 95
```

Result:

```text
[
 [1,85],
 [2,95]
]
```

---

# ⚠️ Why Do We Sort in Descending Order?

We need the **highest** five scores.

Therefore:

```cpp
sort(scores.begin(), scores.end(), greater<int>());
```

gives:

```text
Highest → Lowest
```

Example:

```text
[60,96,90,95,91]
```

becomes:

```text
[96,95,91,90,60]
```

So:

```cpp
scores[0]
scores[1]
scores[2]
scores[3]
scores[4]
```

are exactly the top 5.

---

# ⚠️ Why Sort the Final Answer?

`unordered_map` does **not guarantee ordering**.

So even if student IDs are:

```text
1, 2, 3
```

the map might give them in some arbitrary order.

Therefore:

```cpp
sort(ans.begin(), ans.end());
```

ensures:

```text
[1,...]
[2,...]
[3,...]
```

because vectors are sorted lexicographically, so the first element (`id`) is compared first.

---

# 🧠 Pattern Recognition

This problem teaches an important combination:

```text
Hash Map
   +
Top K
```

Whenever you see:

> Group information by some ID/key and then find the top/bottom K values for each group.

Think:

```text
key
 ↓
Hash Map
 ↓
collection of values
 ↓
sort / heap
 ↓
Top K
```

For this problem:

```text
Student ID
    ↓
Hash Map
    ↓
Scores
    ↓
Sort descending
    ↓
Top 5
    ↓
Average
```

---

# 🔥 Alternative: Min Heap

Because we only need the **top 5**, we don't actually need to sort every score.

We can maintain a **Min Heap of size 5**.

Why a Min Heap?

Suppose the top 5 currently are:

```text
[80,85,90,95,100]
```

The smallest of these is:

```text
80
```

If we see a new score:

```text
99
```

we replace `80`:

```text
[85,90,95,99,100]
```

So the Min Heap keeps the **5 largest scores** while allowing us to quickly remove the smallest among them.

---

## Min Heap Logic

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

For every score:

```cpp
pq.push(score);

if (pq.size() > 5) {
    pq.pop();
}
```

At the end:

```text
Heap contains exactly the top 5 scores.
```

Then calculate their sum.

---

# Sorting vs Min Heap

| Approach | Idea                          | Complexity   |
| -------- | ----------------------------- | ------------ |
| Sort     | Sort all scores, take first 5 | `O(k log k)` |
| Min Heap | Maintain only top 5           | `O(k log 5)` |

For this particular problem, **sorting is simpler** and completely reasonable because the constraints are small.

The Min Heap approach becomes more useful when:

> `k` is small but the number of scores is very large.

---

# 🧠 Interview Cheat Sheet

### Group:

```cpp
mp[id].push_back(score);
```

### Sort:

```cpp
sort(scores.begin(), scores.end(), greater<int>());
```

### Top 5:

```cpp
for (int i = 0; i < 5; i++)
    sum += scores[i];
```

### Average:

```cpp
sum / 5
```

### Sort final result:

```cpp
sort(ans.begin(), ans.end());
```

---

# 🔑 Mental Model

> **First group scores by student ID using a Hash Map. Then, for each student, sort their scores in descending order, take the top 5, calculate the average, and finally sort the results by student ID.**

### Pattern to remember

```text
Grouping problem
      ↓
Hash Map

Need Top K
      ↓
Sorting / Heap

Need ordered output
      ↓
Sort answer
```
