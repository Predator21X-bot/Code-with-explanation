# Edit Distance

## Problem Summary

We are given two strings:

```cpp id="m1r8v4"
word1
word2
```

We must convert:

```text id="q7m2k5"
word1 → word2
```

using minimum operations.

---

# Allowed Operations

We can perform:

---

# 1. Insert

Example:

```text id="x4m9r1"
cat → chat
```

Insert:

```text id="t8m1k6"
h
```

---

# 2. Delete

Example:

```text id="f5m7r2"
cart → cat
```

Delete:

```text id="p2m8v4"
r
```

---

# 3. Replace

Example:

```text id="v6m1r9"
cat → cut
```

Replace:

```text id="m4r8k1"
a → u
```

---

# Goal

Find:

# minimum operations

needed.

---

# Example

```text id="q9m2v7"
word1 = "horse"
word2 = "ros"
```

Answer:

```text id="x3m7r5"
3
```

Possible operations:

```text id="f8m1k2"
horse → rorse
rorse → rose
rose → ros
```

---

# MOST Important Insight

At every pair of characters:

```text id="p5m9r1"
word1[i]
word2[j]
```

we decide:

* match
* insert
* delete
* replace

---

# Important DP Definition

Define:

```text id="t2m7v4"
dp[i][j]
```

as:

# minimum operations needed to convert:

```text id="n8m4r6"
first i characters of word1
into
first j characters of word2
```

Very important definition.

---

# Why 2D DP?

State depends on TWO variables:

* position in word1
* position in word2

So:

# multidimensional DP

---

# DP Table Size

We create:

```cpp id="r4m1k8"
vector<vector<int>> dp(
    m + 1,
    vector<int>(n + 1, 0)
);
```

---

# Why `m+1` and `n+1`?

Because we must also represent:

```text id="q7m5v2"
empty string states
```

Very important.

---

# Understanding Base Cases

This is the MOST important conceptual part.

---

# First Row Meaning

When:

```text id="x1m8r4"
i = 0
```

we are converting:

```text id="m9r2k5"
empty string
→
first j chars of word2
```

---

# Example

Suppose:

```text id="v5m7r1"
word2 = "ros"
```

---

# dp[0][0]

```text id="f2m8v6"
"" → ""
```

Need:

```text id="t4m1r9"
0 operations
```

So:

dp[0][0]=0

---

# dp[0][1]

```text id="q8m5r2"
"" → "r"
```

Need:

```text id="x6m1v7"
1 insertion
```

So:

dp[0][1]=1

---

# dp[0][2]

```text id="m3r8k1"
"" → "ro"
```

Need:

* insert r
* insert o

Total:

```text id="p7m2v4"
2
```

So:

dp[0][2]=2

---

# Therefore First Row

Becomes:

```text id="f4m9r2"
0 1 2 3 ...
```

---

# General Formula

dp[0][j]=j

because converting:

```text id="q1m7k8"
empty string → j chars
```

needs:

```text id="v8m4r1"
j insertions
```

---

# First Column Meaning

When:

```text id="n5m2v7"
j = 0
```

we convert:

```text id="x2m8r5"
first i chars of word1
→
empty string
```

---

# Example

Suppose:

```text id="m6r1k9"
word1 = "horse"
```

---

# dp[1][0]

```text id="t8m4v2"
"h" → ""
```

Need:

```text id="q3m7r1"
1 deletion
```

So:

dp[1][0]=1

---

# dp[2][0]

```text id="v9m2k4"
"ho" → ""
```

Need:

* delete h
* delete o

Total:

```text id="f1m8r6"
2
```

So:

dp[2][0]=2

---

# Therefore First Column

Becomes:

```text id="p4m7v2"
0
1
2
3
...
```

---

# General Formula

dp[i][0]=i

because converting:

```text id="x7m1r5"
i chars → empty string
```

needs:

```text id="m2r9k8"
i deletions
```

---

# MOST Important DP Insight

Base cases are:

# foundation states

All future DP cells depend on them.

---

# Character Match Case

Suppose:

```text id="q5m8v1"
word1[i-1] == word2[j-1]
```

Example:

```text id="t1m7r4"
'a' == 'a'
```

No operation needed.

So:

dp[i][j]=dp[i-1][j-1]

---

# Why?

Current characters already same.

Just solve remaining smaller problem.

---

# Character Mismatch Case

Suppose:

```text id="f8m2k5"
word1[i-1] != word2[j-1]
```

We try all 3 operations.

---

# Operation 1 — Insert

Insert character into word1.

1+dp[i][j-1]

---

# Why?

One insertion performed.

Now solve smaller conversion.

---

# Operation 2 — Delete

Delete current character from word1.

1+dp[i-1][j]

---

# Operation 3 — Replace

Replace current character.

1+dp[i-1][j-1]

---

# Take Minimum

We want minimum operations.

So:

dp[i][j]=1+\min(insert,delete,replace)

---

# Visual Example

Suppose:

```text id="v4m9r1"
word1 = "cat"
word2 = "cut"
```

---

# Compare

```text id="n6m1v8"
c == c
```

No operation.

---

# Compare

```text id="x5m2r7"
a != u
```

Try:

* insert
* delete
* replace

Best:

```text id="k4m9v2"
replace
```

---

# Complete DP Solution

```cpp id="w8m1r3"
class Solution {
public:

    int minDistance(
        string word1,
        string word2
    ) {

        int m = word1.length();
        int n = word2.length();

        vector<vector<int>> dp(
            m + 1,
            vector<int>(n + 1, 0)
        );

        // First row
        for(int j = 0; j <= n; j++){

            dp[0][j] = j;
        }

        // First column
        for(int i = 0; i <= m; i++){

            dp[i][0] = i;
        }

        // Fill DP table
        for(int i = 1; i <= m; i++){

            for(int j = 1; j <= n; j++){

                // Characters match
                if(word1[i - 1] == word2[j - 1]){

                    dp[i][j] =
                    dp[i - 1][j - 1];
                }

                // Characters differ
                else{

                    dp[i][j] =
                    1 +
                    min({
                        dp[i][j - 1],     // insert
                        dp[i - 1][j],     // delete
                        dp[i - 1][j - 1]  // replace
                    });
                }
            }
        }

        return dp[m][n];
    }
};
```

---

# Complexity Analysis

## Time Complexity

Every DP state computed once.

```text id="q6m2v7"
O(m × n)
```

---

## Space Complexity

DP table size:

```text id="t8m1k5"
O(m × n)
```

---

# Important DP Pattern Learned

This is classic:

# String Transformation DP

Very important interview category.

---

# Similar Problems

* Longest Common Subsequence
* Delete Operation for Two Strings
* Distinct Subsequences
* Shortest Common Supersequence
* Regular Expression Matching

---

# Common Mistakes

## Mistake 1

Incorrect base cases.

Remember:

```text id="f2m7r9"
empty → string = insert all
string → empty = delete all
```

---

## Mistake 2

Using:

```text id="m5r8k1"
dp[i][j]
```

without clearly defining its meaning.

Always define DP state first.

---

## Mistake 3

Forgetting `m+1` and `n+1`.

Need extra row/column for empty-string states.

---

## Mistake 4

Confusing insert/delete directions.

Think carefully:

```text id="x7m4v2"
what transformation is happening?
```

---

# Recognition Pattern

Whenever problem asks:

* convert one string into another
* minimum operations
* edits on strings

think:

# Edit Distance DP

---

# Important Mental Models

## Base States

First row/column represent:

```text id="q9m1r6"
simplest possible conversions
```

---

## Match Case

Characters already same.

```text id="w4m8k2"
no operation needed
```

---

## Mismatch Case

Try:

```text id="p1m7r5"
insert
delete
replace
```

Choose minimum future cost.

---

# Biggest Takeaway

Dynamic Programming works by:

```text id="t6m2v8"
building larger answers
from already solved smaller subproblems
```

and Edit Distance is one of the purest examples of that idea.
