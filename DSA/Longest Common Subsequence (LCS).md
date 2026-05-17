# Longest Common Subsequence (LCS)

## Problem Summary

We are given two strings:

```cpp id="m1r8v4"
text1
text2
```

We must find:

# length of the longest common subsequence

between them.

---

# Important Term — Subsequence

A subsequence means:

```text id="q7m2k5"
characters kept in same order
```

but:

```text id="x4m9r1"
not necessarily contiguous
```

---

# Example

String:

```text id="t8m1k6"
"abcde"
```

Valid subsequences:

```text id="f5m7r2"
"ace"
"abd"
"abc"
"ae"
```

---

# Invalid Subsequence

```text id="p2m8v4"
"ca"
```

because order changes.

---

# Common Subsequence

A subsequence present in:

```text id="v6m1r9"
BOTH strings
```

---

# Example

```text id="m4r8k1"
text1 = "abcde"
text2 = "ace"
```

Common subsequences:

```text id="q9m2v7"
"a"
"ac"
"ace"
```

Longest:

```text id="x3m7r5"
"ace"
```

Length:

```text id="f8m1k2"
3
```

---

# Goal

Return:

```text id="p5m9r1"
length only
```

NOT the actual subsequence.

---

# MOST Important Observation

At every pair of characters:

```text id="t2m7v4"
text1[i]
text2[j]
```

we have choices.

---

# Case 1 — Characters Match

Example:

```text id="n8m4r6"
'a' == 'a'
```

Then this character CAN become part of subsequence.

So:

```text id="r4m1k8"
1 + solve remaining strings
```

---

# Case 2 — Characters Do NOT Match

Example:

```text id="q7m5v2"
'b' != 'c'
```

Then we must skip one character.

Choices:

* skip character from first string
  OR
* skip character from second string

Take better answer.

---

# Important DP Definition

Define:

```text id="x1m8r4"
dp[i][j]
```

as:

# length of LCS between:

```text id="m9r2k5"
first i characters of text1
and
first j characters of text2
```

Very important definition.

---

# Why Extra Row and Column?

We create:

```cpp id="v5m7r1"
vector<vector<int>> dp(
    m + 1,
    vector<int>(n + 1, 0)
);
```

Why?

Because:

```text id="f2m8v6"
empty string cases
```

must also exist.

---

# Important Base Case Concept

In recursion we write:

```cpp id="t4m1r9"
if(i == m || j == n)
    return 0;
```

But in:

# bottom-up DP

base cases are represented by:

```text id="q8m5r2"
extra row and extra column initialized to 0
```

---

# Meaning of Last Row

```text id="x6m1v7"
dp[m][j]
```

means:

```text id="m3r8k1"
text1 exhausted
```

So LCS length:

```text id="p7m2v4"
0
```

---

# Meaning of Last Column

```text id="f4m9r2"
dp[i][n]
```

means:

```text id="q1m7k8"
text2 exhausted
```

Again:

```text id="v8m4r1"
0
```

---

# Match Recurrence

If:

```text id="n5m2v7"
text1[i-1] == text2[j-1]
```

then:

dp[i][j]=1+dp[i-1][j-1]

---

# Why?

Matching character contributes:

```text id="x2m8r5"
1
```

plus best subsequence before those characters.

---

# Non-Match Recurrence

If characters differ:

dp[i][j]=\max(dp[i-1][j],dp[i][j-1])

---

# Why max()?

We try:

* skipping character from text1
  OR
* skipping character from text2

Take longer subsequence.

---

# Important Indexing Insight

We use:

```text id="m6r1k9"
i-1
j-1
```

because DP table shifted by one due to extra row/column.

---

# Visual Example

Suppose:

```text id="t8m4v2"
text1 = "abc"
text2 = "ac"
```

---

# DP Table Size

```text id="q3m7r1"
4 x 3
```

because:

```text id="v9m2k4"
m+1
n+1
```

---

# Initial Table

```text id="f1m8r6"
0 0 0
0 0 0
0 0 0
0 0 0
```

---

# Fill dp[1][1]

Compare:

```text id="p4m7v2"
'a' == 'a'
```

Match.

dp[1][1]=1+dp[0][0]=1

Table:

```text id="x7m1r5"
0 0 0
0 1 0
0 0 0
0 0 0
```

---

# Fill dp[1][2]

Compare:

```text id="m2r9k8"
'a' vs 'c'
```

No match.

dp[1][2]=\max(dp[0][2],dp[1][1])=1

---

# Continue Filling

Final table:

```text id="q5m8v1"
0 0 0
0 1 1
0 1 1
0 1 2
```

Answer:

```text id="t1m7r4"
2
```

Subsequence:

```text id="f8m2k5"
"ac"
```

---

# Complete Bottom-Up DP Solution

```cpp id="v4m9r1"
class Solution {
public:

    int longestCommonSubsequence(
        string text1,
        string text2
    ) {

        int m = text1.length();
        int n = text2.length();

        vector<vector<int>> dp(
            m + 1,
            vector<int>(n + 1, 0)
        );

        for(int i = 1; i <= m; i++){

            for(int j = 1; j <= n; j++){

                // Characters match
                if(text1[i - 1] == text2[j - 1]){

                    dp[i][j] =
                    1 + dp[i - 1][j - 1];
                }

                // Characters differ
                else{

                    dp[i][j] =
                    max(
                        dp[i - 1][j],
                        dp[i][j - 1]
                    );
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

We compute every DP state once.

```text id="n6m1v8"
O(m × n)
```

---

## Space Complexity

DP table size:

```text id="x5m2r7"
O(m × n)
```

---

# Space Optimization

Observe recurrence:

Current state depends only on:

* previous row
* current row left value

Can optimize to:

```text id="k4m9v2"
O(n)
```

space using 1D DP.

---

# Important DP Pattern Learned

This is classic:

# Two String DP

Very important interview category.

---

# Similar Problems

* Edit Distance
* Distinct Subsequences
* Longest Palindromic Subsequence
* Shortest Common Supersequence
* Delete Operation for Two Strings

---

# Common Mistakes

## Mistake 1

Writing recursive base case inside bottom-up loops.

Wrong:

```cpp id="w8m1r3"
if(i == m || j == n)
    return 0;
```

In bottom-up DP:

```text id="q6m2v7"
base cases are table initialization
```

---

## Mistake 2

Creating DP table of size:

```text id="t8m1k5"
m x n
```

Need:

```text id="f2m7r9"
(m+1) x (n+1)
```

for empty-string states.

---

## Mistake 3

Starting loops from 0 while using:

```cpp id="m5r8k1"
text1[i-1]
```

This causes invalid indexing.

Correct:

```cpp id="x7m4v2"
start from 1
```

---

## Mistake 4

Confusing substring vs subsequence.

Subsequence:

```text id="q9m1r6"
does NOT need contiguous characters
```

---

# Recognition Pattern

Whenever problem involves:

* two strings
* matching characters
* subsequences

think:

# String Dynamic Programming

---

# Important Mental Models

## Match or Skip

At every character pair:

```text id="w4m8k2"
match them
OR
skip one character
```

---

## State Meaning

Always clearly define:

```text id="p1m7r5"
What exactly does dp[i][j] represent?
```

Most DP mistakes happen here.

---

## Bottom-Up Base Cases

Recursive:

```cpp id="t6m2v8"
if(...) return 0;
```

Bottom-up equivalent:

```text id="r8m5k4"
extra initialized row/column
```

---

# Biggest Takeaway

Dynamic Programming is fundamentally:

```text id="x2m9r1"
breaking a large problem into overlapping smaller subproblems
```

and reusing already solved states efficiently.
