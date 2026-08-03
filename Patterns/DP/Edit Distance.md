# 🟦 LeetCode 72 - Edit Distance

**Difficulty:** Hard
**Pattern:** Dynamic Programming (2D DP - String DP)
**Time Complexity:** `O(m × n)`
**Space Complexity:** `O(m × n)` *(Can be optimized to `O(n)`)*

---

# 📌 Problem

Given two strings

```cpp
word1
word2
```

Convert

```cpp
word1
```

into

```cpp
word2
```

using the **minimum number of operations**.

Allowed operations:

* Insert
* Delete
* Replace

Return the **minimum number of operations** required.

---

# Examples

### Example 1

```cpp
Input:

word1 = "horse"
word2 = "ros"

Output:

3
```

Possible operations

```text
horse
↓

replace h → r

rorse
↓

delete r

rose
↓

delete e

ros
```

Total operations = **3**

---

### Example 2

```cpp
Input:

word1 = "intention"
word2 = "execution"

Output:

5
```

---

# 💡 Key Observation

Whenever characters are different, we have exactly **3 choices**.

1. Insert
2. Delete
3. Replace

We choose the operation requiring the **minimum** edits.

---

# DP State

Define

```cpp
dp[i][j]
```

as

> **Minimum operations required to convert the first `i` characters of `word1` into the first `j` characters of `word2`.**

Examples

```text
word1 = horse

word2 = ros

dp[3][2]
```

means

Convert

```text
hor
```

into

```text
ro
```

---

# Why DP size is `(m+1) × (n+1)`?

Our state represents **prefix lengths**, not indices.

Need states

```text
0...m

0...n
```

because

```cpp
dp[0][j]
```

and

```cpp
dp[i][0]
```

represent empty prefixes.

Hence

```cpp
vector<vector<int>> dp(m + 1, vector<int>(n + 1));
```

---

# Base Cases

## Empty → Non-empty

```cpp
dp[0][j] = j;
```

Reason

```text
""

↓

abc
```

Need

```text
Insert a
Insert b
Insert c
```

Operations = 3

---

## Non-empty → Empty

```cpp
dp[i][0] = i;
```

Reason

```text
abc

↓

""
```

Need

```text
Delete a
Delete b
Delete c
```

Operations = 3

---

# Character Match

Suppose

```text
cat

cut
```

Current characters

```text
t == t
```

No operation required.

Simply move diagonally.

```cpp
dp[i][j] = dp[i-1][j-1];
```

---

# Character Mismatch

Suppose

```text
a != u
```

Now we have 3 choices.

---

## 1. Insert

Example

```text
at

↓

cat
```

Insert

```text
c
```

After insertion,

remaining problem becomes

```text
at

↓

at
```

Notice

* word1 remains same
* word2 becomes smaller

Hence

```cpp
dp[i][j-1]
```

---

## 2. Delete

Example

```text
cat

↓

at
```

Delete

```text
c
```

Remaining problem

```text
at

↓

at
```

Notice

* word1 becomes smaller
* word2 remains same

Hence

```cpp
dp[i-1][j]
```

---

## 3. Replace

Example

```text
cat

↓

cut
```

Replace

```text
a → u
```

Remaining problem

```text
c

↓

c
```

Notice

Both strings become smaller.

Hence

```cpp
dp[i-1][j-1]
```

---

# DP Transition

If characters match

```cpp
if(word1[i-1]==word2[j-1])
    dp[i][j]=dp[i-1][j-1];
```

Else

```cpp
dp[i][j]=1+min({
    dp[i][j-1],      // Insert
    dp[i-1][j],      // Delete
    dp[i-1][j-1]     // Replace
});
```

The extra `1` represents the cost of performing the chosen operation.

---

# Easy Way to Remember

Imagine standing at cell

```text
dp[i][j]
```

```text
        j-1      j
      +-------+-------+
i-1   |  ↖    |   ↑   |
      |Replace|Delete |
      +-------+-------+
i     |   ←   |   X   |
      |Insert |Current|
      +-------+-------+
```

### Left ←

Insert

```cpp
dp[i][j-1]
```

because **word2 becomes smaller**.

---

### Up ↑

Delete

```cpp
dp[i-1][j]
```

because **word1 becomes smaller**.

---

### Diagonal ↖

Replace (or Match)

```cpp
dp[i-1][j-1]
```

because **both strings become smaller**.

---

# Algorithm

1. Let

```cpp
m = word1.size();

n = word2.size();
```

2. Create DP table

```cpp
(m+1) × (n+1)
```

3. Initialize

```cpp
dp[0][j]=j;

dp[i][0]=i;
```

4. Fill table

If characters equal

```cpp
dp[i][j]=dp[i-1][j-1];
```

Else

```cpp
dp[i][j]=1+min(insert,delete,replace);
```

5. Return

```cpp
dp[m][n];
```

---

# C++ Solution

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {

        int m = word1.size();
        int n = word2.size();

        vector<vector<int>> dp(m + 1, vector<int>(n + 1));

        // Base cases
        for (int i = 0; i <= m; i++)
            dp[i][0] = i;

        for (int j = 0; j <= n; j++)
            dp[0][j] = j;

        // Fill DP table
        for (int i = 1; i <= m; i++) {

            for (int j = 1; j <= n; j++) {

                if (word1[i - 1] == word2[j - 1]) {

                    dp[i][j] = dp[i - 1][j - 1];

                } else {

                    dp[i][j] = 1 + min({
                        dp[i][j - 1],      // Insert
                        dp[i - 1][j],      // Delete
                        dp[i - 1][j - 1]   // Replace
                    });
                }
            }
        }

        return dp[m][n];
    }
};
```

---

# Dry Run (Small Example)

```text
word1 = "cat"
word2 = "cut"
```

At the mismatch:

```text
a != u
```

Three choices:

```text
Insert  → dp[i][j-1]

Delete  → dp[i-1][j]

Replace → dp[i-1][j-1]
```

Take the minimum and add **1**.

---

# Complexity Analysis

### Time Complexity

DP table size

```text
(m+1) × (n+1)
```

Every cell is computed once.

```text
O(m × n)
```

---

### Space Complexity

DP table

```text
O(m × n)
```

---

# Common Mistakes

### ❌ Using `m × n` DP table

Wrong.

Need an extra row and column for empty strings.

Correct

```cpp
vector<vector<int>> dp(m+1, vector<int>(n+1));
```

---

### ❌ Forgetting Base Cases

Always initialize

```cpp
dp[0][j]=j;

dp[i][0]=i;
```

---

### ❌ Comparing

```cpp
word1[i]
```

instead of

```cpp
word1[i-1]
```

Remember:

* `i` is the **prefix length**
* `i-1` is the **last character** of that prefix

---

### ❌ Forgetting `+1`

When characters differ, every operation costs **one edit**.

```cpp
1 + min(...)
```

---

### ❌ Confusing Insert/Delete Directions

Don't memorize arrows.

Instead ask:

> **After performing this operation, which string still has characters left?**

* **Insert** → `word2` becomes smaller → `dp[i][j-1]`
* **Delete** → `word1` becomes smaller → `dp[i-1][j]`
* **Replace** → both become smaller → `dp[i-1][j-1]`

---

# Pattern Recognition

This is a **2D String DP** problem.

Whenever you compare two strings and ask:

* Minimum operations
* Longest common subsequence
* Minimum deletions
* Matching prefixes
* Transform one string into another

Think:

```cpp
dp[i][j]
```

=

> Answer considering the **first `i` characters** of one string and the **first `j` characters** of the other.

---

# Similar Problems

* LeetCode 1143 – Longest Common Subsequence
* LeetCode 583 – Delete Operation for Two Strings
* LeetCode 712 – Minimum ASCII Delete Sum
* LeetCode 97 – Interleaving String
* LeetCode 115 – Distinct Subsequences

---

# Interview Takeaways

* Use a **2D DP table** because the state depends on prefixes of **two strings**.
* `dp[i][j]` stores the minimum edits to convert the first `i` characters of `word1` into the first `j` characters of `word2`.
* Base cases handle conversions involving the empty string:

  * `dp[0][j] = j` (insert all characters)
  * `dp[i][0] = i` (delete all characters)
* If characters match, move diagonally without any extra cost.
* If characters differ, choose the minimum among **Insert**, **Delete**, and **Replace**, then add `1` for the current operation.
* A reliable way to remember the recurrence is to ask: **"After performing this operation, which string becomes shorter?"**
