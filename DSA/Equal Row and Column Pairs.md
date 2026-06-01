# Equal Row and Column Pairs

## Problem Link

[https://leetcode.com/problems/equal-row-and-column-pairs/](https://leetcode.com/problems/equal-row-and-column-pairs/)

---

# Problem Summary

Given an:

```cpp
n x n matrix grid
```

Find the number of pairs:

```text
(row i, column j)
```

such that:

```text
row i == column j
```

---

# Example

```cpp
grid =
[
 [3,2,1],
 [1,7,6],
 [3,2,1]
]
```

Rows:

```text
[3,2,1]
[1,7,6]
[3,2,1]
```

Columns:

```text
[3,1,3]
[2,7,2]
[1,6,1]
```

No matches.

Answer:

```text
0
```

---

# What Does Equality Mean?

For a row and column to be equal:

```text
Every element must match.
```

Example:

```text
[1,2,3]
=
[1,2,3]
```

✅ Equal

---

```text
[1,2,3]
=
[1,3,2]
```

❌ Not Equal

---

# Approach 1 — Brute Force

---

## Idea

Compare:

```text
every row
with
every column
```

---

# Step 1

Pick a row.

```text
Row 0
```

---

# Step 2

Build each column.

```text
Col 0
Col 1
Col 2
...
```

---

# Step 3

Compare element-by-element.

```cpp
for(int k=0;k<n;k++)
{
    if(row[k] != column[k])
        break;
}
```

---

# Complexity

Rows:

```text
n
```

Columns:

```text
n
```

Comparison:

```text
n
```

Total:

```text
O(n³)
```

❌ Too slow.

---

# Key Observation

The same row gets compared again and again.

Example:

```text
[3,2,1]
```

might be compared with:

```text
Col0
Col1
Col2
```

repeatedly.

Wasteful.

---

# Main Optimization Idea

Instead of:

```text
compare repeatedly
```

Do:

```text
store once
lookup many times
```

---

# Approach 2 — map<vector<int>, int>

(Simple & Interview Friendly)

---

# Core Idea

Store all rows inside a map.

---

Example:

Rows:

```text
[3,2,1]
[1,7,6]
[3,2,1]
```

Store:

```text
[3,2,1] → 2

[1,7,6] → 1
```

---

# Why Frequency?

Suppose:

```text
[3,2,1]
```

appears twice.

If a column equals:

```text
[3,2,1]
```

then BOTH rows match.

Answer should increase by:

```text
2
```

not:

```text
1
```

---

# Data Structure

```cpp
map<vector<int>, int> mp;
```

---

# What Does It Store?

```text
row → frequency
```

---

# Store Rows

```cpp
for(auto &row : grid)
{
    mp[row]++;
}
```

---

# Build Columns

For every column:

```cpp
vector<int> column;

for(int i=0;i<n;i++)
{
    column.push_back(grid[i][j]);
}
```

---

# Lookup

```cpp
answer += mp[column];
```

if column exists.

---

# Full Code

```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {

        map<vector<int>, int> mp;

        for(auto &row : grid)
        {
            mp[row]++;
        }

        int n = grid.size();
        int answer = 0;

        for(int j=0;j<n;j++)
        {
            vector<int> column;

            for(int i=0;i<n;i++)
            {
                column.push_back(grid[i][j]);
            }

            answer += mp[column];
        }

        return answer;
    }
};
```

---

# Why Does map Work With vector?

Because:

```cpp
vector<int>
```

supports:

```text
lexicographical comparison
```

and:

```cpp
map
```

only needs comparison.

---

# Complexity

Insertion:

```text
O(log n)
```

Lookup:

```text
O(log n)
```

Total:

```text
O(n² log n)
```

---

# Approach 3 — unordered_map + Serialization

(Optimal and Easier Than Custom Hash)

---

# Problem

Can we do:

```cpp
unordered_map<vector<int>, int>
```

?

❌ No.

Compilation error.

---

# Why?

unordered_map requires:

```text
hash function
```

for keys.

---

Built-in hash exists for:

```cpp
int
string
```

but NOT:

```cpp
vector<int>
```

---

# Solution

Convert vector into string.

---

Example

```text
[1,2,3]
```

becomes:

```text
"1#2#3"
```

---

# Why Add '#'

Without separator:

```text
[1,23]
```

and

```text
[12,3]
```

both become:

```text
123
```

Wrong.

---

Separator avoids collisions.

```text
1#23
12#3
```

Different strings.

---

# Serialization Function

```cpp
string serialize(vector<int>& v)
{
    string s;

    for(int x : v)
    {
        s += to_string(x) + "#";
    }

    return s;
}
```

---

# Store Rows

```cpp
unordered_map<string,int> mp;

for(auto &row : grid)
{
    mp[serialize(row)]++;
}
```

---

# Check Columns

```cpp
string key = serialize(column);

answer += mp[key];
```

---

# Full Code

```cpp
class Solution {
public:

    string serialize(vector<int>& v)
    {
        string s;

        for(int x : v)
        {
            s += to_string(x) + "#";
        }

        return s;
    }

    int equalPairs(vector<vector<int>>& grid) {

        unordered_map<string,int> mp;

        for(auto &row : grid)
        {
            mp[serialize(row)]++;
        }

        int n = grid.size();
        int answer = 0;

        for(int j=0;j<n;j++)
        {
            vector<int> column;

            for(int i=0;i<n;i++)
            {
                column.push_back(grid[i][j]);
            }

            answer += mp[serialize(column)];
        }

        return answer;
    }
};
```

---

# Complexity

Hash lookup:

```text
O(1)
```

average.

Total:

```text
O(n²)
```

✅ Better than map.

---

# Approach 4 — unordered_map + Custom Hash

(Advanced STL Knowledge)

---

# Goal

Use:

```cpp
unordered_map<vector<int>, int>
```

directly.

---

# Problem

vector has no hash.

---

# Solution

Create custom hash.

---

```cpp
struct VectorHash {

    size_t operator()(const vector<int>& v) const {

        size_t hash = 0;

        for(int x : v)
        {
            hash ^= hash * 31 +
                    std::hash<int>()(x);
        }

        return hash;
    }
};
```

---

# Map Declaration

```cpp
unordered_map<
    vector<int>,
    int,
    VectorHash
> mp;
```

---

# Rest Of Logic

Exactly same as Approach 2.

Store rows.

Build columns.

Lookup frequency.

---

# Full Code

```cpp
struct VectorHash {

    size_t operator()(const vector<int>& v) const {

        size_t hash = 0;

        for(int x : v)
        {
            hash ^= hash * 31 +
                    std::hash<int>()(x);
        }

        return hash;
    }
};

class Solution {
public:

    int equalPairs(vector<vector<int>>& grid) {

        unordered_map<
            vector<int>,
            int,
            VectorHash
        > mp;

        for(auto &row : grid)
        {
            mp[row]++;
        }

        int n = grid.size();
        int answer = 0;

        for(int j=0;j<n;j++)
        {
            vector<int> column;

            for(int i=0;i<n;i++)
            {
                column.push_back(grid[i][j]);
            }

            answer += mp[column];
        }

        return answer;
    }
};
```

---

# map vs unordered_map

| Feature             | map           | unordered_map       |
| ------------------- | ------------- | ------------------- |
| Internal Structure  | Balanced Tree | Hash Table          |
| Ordered             | ✅ Yes         | ❌ No                |
| Lookup              | O(log n)      | O(1) avg            |
| Needs Comparison    | ✅             | ❌                   |
| Needs Hash          | ❌             | ✅                   |
| vector<int> Support | ✅ Directly    | ❌ Needs Custom Hash |

---

# Interview Recommendation

If coding interview:

### Safest

```cpp
map<vector<int>, int>
```

Easy.

Readable.

Less bugs.

---

### Optimal

```cpp
unordered_map<string,int>
```

Good balance between speed and simplicity.

---

### Advanced

```cpp
unordered_map<vector<int>,int,VectorHash>
```

Shows STL and hashing knowledge.

---

# Pattern Learned

This problem teaches:

### Convert

```text
Repeated Comparisons
```

into

```text
Fast Lookups
```

---

# Recognition Pattern

Whenever you see:

```text
Compare large structures repeatedly
```

Think:

```text
Store structure in map/hashmap
Lookup instead of recomputing
```

---

# Revision Key

### Brute Force

```text
Row × Column × Element
```

```text
O(n³)
```

---

### map

```text
Store rows
Lookup columns
```

```text
O(n² log n)
```

---

### unordered_map + string

```text
Serialize vectors
Hash lookup
```

```text
O(n²)
```

---

### unordered_map + custom hash

```text
Hash vector directly
```

```text
O(n²)
```

### One-Line Memory Trick

> "Store every row as a key, build each column, and count how many matching rows already exist."
