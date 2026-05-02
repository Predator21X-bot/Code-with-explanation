# 🧩 Equal Row and Column Pairs

### 🔗 Problem Link

https://leetcode.com/problems/equal-row-and-column-pairs/

---

# 🧠 Problem Understanding

Given:

* An `n x n` matrix `grid`

👉 Goal:
Find number of pairs `(i, j)` such that:

```
row i == column j
```

---

## 🔍 Example

```
grid =
[1 2 3]
[2 1 3]
[1 2 3]

Output = 2
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: What does equality mean?

For `(i, j)`:

```
grid[i][k] == grid[k][j] for all k
```

👉 Entire row must match entire column

---

## 🧩 Step 2: Brute force

* Compare each row with each column
* Each comparison = O(n)

```
Total = O(n³) ❌
```

---

## 🧩 Step 3: Optimization idea

> Convert repeated comparison → lookup problem

---

## 🧠 Key Insight

```
Store rows → compare columns using lookup
```

---

# ⚡ Approach 1: Using `map<vector<int>, int>` (Simple & Safe)

---

## 🧠 Why it works

* `map` uses **comparison (operator<)**
* `vector<int>` supports lexicographical comparison
* No hashing required

---

## 💻 Code

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        map<vector<int>, int> mp;

        // store rows
        for (auto &row : grid) {
            mp[row]++;
        }

        int n = grid.size();
        int answer = 0;

        // check columns
        for (int j = 0; j < n; j++) {
            vector<int> column;

            for (int i = 0; i < n; i++) {
                column.push_back(grid[i][j]);
            }

            if (mp.count(column)) {
                answer += mp[column];
            }
        }

        return answer;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n² log n)**
* Space: **O(n²)**

---

# ⚡ Approach 2: Using `unordered_map` + String Serialization (Optimal)

---

## 🧠 Why needed?

* `unordered_map` requires hashing
* `vector<int>` has no default hash

👉 Convert vector → string

---

## 🔹 Example

```
[1,2,3] → "1#2#3"
```

---

## 💻 Code

```cpp
class Solution {
public:
    string serialize(vector<int>& v) {
        string s;
        for (int x : v) {
            s += to_string(x) + "#";
        }
        return s;
    }

    int equalPairs(vector<vector<int>>& grid) {
        unordered_map<string, int> mp;

        // store rows
        for (auto &row : grid) {
            mp[serialize(row)]++;
        }

        int n = grid.size();
        int answer = 0;

        // check columns
        for (int j = 0; j < n; j++) {
            vector<int> column;

            for (int i = 0; i < n; i++) {
                column.push_back(grid[i][j]);
            }

            string key = serialize(column);

            if (mp.count(key)) {
                answer += mp[key];
            }
        }

        return answer;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n²)**
* Space: **O(n²)**

---

# ⚡ Approach 3: Using `unordered_map` + Custom Hash (Advanced)

---

## 🧠 Idea

Define a custom hash function for `vector<int>`

---

## 💻 Code

```cpp
struct VectorHash {
    size_t operator()(const vector<int>& v) const {
        size_t hash = 0;
        for (int x : v) {
            hash ^= hash * 31 + std::hash<int>()(x);
        }
        return hash;
    }
};

class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        unordered_map<vector<int>, int, VectorHash> mp;

        for (auto &row : grid) {
            mp[row]++;
        }

        int n = grid.size();
        int answer = 0;

        for (int j = 0; j < n; j++) {
            vector<int> column;

            for (int i = 0; i < n; i++) {
                column.push_back(grid[i][j]);
            }

            if (mp.count(column)) {
                answer += mp[column];
            }
        }

        return answer;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n²)**
* Space: **O(n²)**

---

# 🔥 map vs unordered_map (VERY IMPORTANT)

| Feature        | map            | unordered_map  |
| -------------- | -------------- | -------------- |
| Mechanism      | Tree (ordered) | Hash table     |
| Requirement    | comparison     | hash function  |
| Time           | O(log n)       | O(1) avg       |
| Vector support | ✅              | ❌ (needs hash) |

---

# 🧠 Patterns Learned

* Matrix → reduce to lookup
* Hashing complex structures
* Frequency mapping
* Avoiding repeated comparisons

---

# ❗ Common Mistakes

* ❌ Using unordered_map<vector<int>> directly
* ❌ Forgetting to build column vector
* ❌ Ignoring frequency count
* ❌ Comparing element-by-element repeatedly

---

# 🔥 Mental Model (REVISION KEY)

> “Store rows → match columns using hashing”

---

# 🚀 Takeaway

* Convert expensive comparisons → hash lookups
* Choose data structure based on:

  * hashing vs ordering
* For complex keys:

  * serialize OR custom hash
