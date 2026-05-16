# Letter Combinations of a Phone Number

## Problem Summary

We are given a string of digits:

```cpp id="m1r8v4"
digits
```

Each digit maps to letters like old mobile phone keypad.

| Digit | Letters |
| ----- | ------- |
| 2     | abc     |
| 3     | def     |
| 4     | ghi     |
| 5     | jkl     |
| 6     | mno     |
| 7     | pqrs    |
| 8     | tuv     |
| 9     | wxyz    |

Goal:

```text id="q7m2k5"
Generate ALL possible letter combinations
```

by choosing:

```text id="x4m9r1"
ONE letter from each digit
```

---

# Example 1

## Input

```text id="t8m1k6"
"2"
```

Digit `2` maps to:

```text id="f5m7r2"
a b c
```

Output:

```text id="p2m8v4"
["a","b","c"]
```

---

# Example 2

## Input

```text id="v6m1r9"
"23"
```

Mappings:

```text id="m4r8k1"
2 -> abc
3 -> def
```

Possible combinations:

```text id="q9m2v7"
ad
ae
af
bd
be
bf
cd
ce
cf
```

Output:

```text id="x3m7r5"
["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

---

# Important Understanding

For EACH digit:

```text id="f8m1k2"
choose exactly ONE letter
```

Then combine all choices together.

---

# Key Observation

Each digit gives:

```text id="p5m9r1"
multiple choices
```

This forms a:

# Decision Tree

---

# Visual Tree

For input:

```text id="t2m7v4"
"23"
```

Decision tree:

```text id="n8m4r6"
          ""
       /   |   \
      a    b    c
    / | \ /|\  /|\
   d e f d e f d e f
```

Each root-to-leaf path forms one valid answer.

---

# Core Backtracking Idea

At every step:

```text id="r4m1k8"
choose one letter
move to next digit
```

When all digits are processed:

```text id="q7m5v2"
one complete combination is formed
```

---

# Why Backtracking?

Problem asks:

```text id="x1m8r4"
generate ALL combinations
```

Whenever you see:

* all combinations
* all possibilities
* every possible choice

think:

# Backtracking / Recursion

---

# Recursive State

At any recursive call we need:

## 1. Current index

Which digit are we processing?

Example:

```text id="m9r2k5"
index = 1
```

means second digit.

---

## 2. Current partial combination

Example:

```text id="v5m7r1"
"ad"
```

---

## 3. Result vector

Store completed answers.

---

## 4. Keypad mapping

To know available letters.

---

# Keypad Mapping

We store mapping as:

```cpp id="f2m8v6"
vector<string> keypad = {
    "", "", "abc", "def",
    "ghi", "jkl", "mno",
    "pqrs", "tuv", "wxyz"
};
```

---

# Why This Works

Index directly matches digit.

Example:

```cpp id="t4m1r9"
keypad[2]
```

gives:

```text id="q8m5r2"
"abc"
```

---

# Character to Integer Conversion

Digits are characters:

```text id="x6m1v7"
'2'
```

To convert into integer:

```cpp id="m3r8k1"
digits[index] - '0'
```

Example:

```cpp id="p7m2v4"
'2' - '0' = 2
```

---

# Base Case

When:

```cpp id="f4m9r2"
index == digits.length()
```

means:

```text id="q1m7k8"
all digits processed
```

Current combination is complete answer.

Store it:

```cpp id="v8m4r1"
ans.push_back(current);
```

---

# Recursive Step

Get letters for current digit:

```cpp id="n5m2v7"
string letters = keypad[digits[index] - '0'];
```

Loop through all possible letters:

```cpp id="x2m8r5"
for(char ch : letters)
```

For every letter:

1. choose letter
2. recurse to next digit

---

# Recursive Call

```cpp id="m6r1k9"
solve(
    index + 1,
    current + ch,
    digits,
    ans,
    keypad
);
```

---

# Why `current + ch` ?

Suppose:

```text id="t8m4v2"
current = "a"
ch = 'd'
```

Then:

```text id="q3m7r1"
"a" + 'd'
```

becomes:

```text id="v9m2k4"
"ad"
```

New partial combination.

---

# Important Backtracking Insight

We are basically doing:

```text id="f1m8r6"
choose
explore
choose
explore
...
```

This is Depth First Search (DFS) on decision tree.

---

# Dry Run

## Input

```text id="p4m7v2"
"23"
```

---

# Initial Call

```cpp id="x7m1r5"
solve(0, "")
```

Meaning:

```text id="m2r9k8"
processing first digit
current string empty
```

---

# Processing Digit '2'

Letters:

```text id="q5m8v1"
abc
```

Choose:

```text id="t1m7r4"
a
```

Recursive call:

```text id="f8m2k5"
solve(1, "a")
```

---

# Processing Digit '3'

Letters:

```text id="v4m9r1"
def
```

Choose:

```text id="n6m1v8"
d
```

Recursive call:

```text id="x5m2r7"
solve(2, "ad")
```

Now:

```text id="k4m9v2"
index == digits.length()
```

Store:

```text id="w8m1r3"
"ad"
```

---

# Continue Exploring

Then recursion tries:

```text id="q6m2v7"
ae
af
bd
be
bf
cd
ce
cf
```

---

# Empty Input Edge Case

Suppose:

```text id="t8m1k5"
digits = ""
```

Then answer should be:

```text id="f2m7r9"
[]
```

because no combinations exist.

---

# Final Code

```cpp id="m5r8k1"
class Solution {
public:

    void solve(
        int index,
        string current,
        string digits,
        vector<string>& ans,
        vector<string>& keypad
    ){

        // Base case
        if(index == digits.length()){
            ans.push_back(current);
            return;
        }

        // Current digit letters
        string letters = keypad[digits[index] - '0'];

        // Try every possible letter
        for(char ch : letters){

            solve(
                index + 1,
                current + ch,
                digits,
                ans,
                keypad
            );
        }
    }

    vector<string> letterCombinations(string digits) {

        vector<string> ans;

        // Edge case
        if(digits.empty())
            return ans;

        vector<string> keypad = {
            "", "", "abc", "def",
            "ghi", "jkl", "mno",
            "pqrs", "tuv", "wxyz"
        };

        // Start recursion
        solve(0, "", digits, ans, keypad);

        return ans;
    }
};
```

---

# Complexity Analysis

Suppose:

```text id="x7m4v2"
n = digits.length()
```

Each digit has about:

```text id="q9m1r6"
4 choices
```

Total combinations:

```text id="w4m8k2"
O(4^n)
```

---

# Time Complexity

```text id="p1m7r5"
O(4^n * n)
```

Why `* n`?

Because building each string takes time.

---

# Space Complexity

Recursive depth:

```text id="t6m2v8"
O(n)
```

Answer storage excluded.

---

# Common Mistakes

## Mistake 1

Forgetting empty input check.

Input:

```text id="r8m5k4"
""
```

should return:

```text id="x2m9r1"
[]
```

---

## Mistake 2

Placing recursive logic inside main function.

Recursive exploration belongs inside helper function.

---

## Mistake 3

Forgetting base case.

Without base case:

```text id="m1r8v6"
recursion never stops
```

---

## Mistake 4

Not converting digit character to integer.

Need:

```cpp id="q6m2v7"
digits[index] - '0'
```

---

# Recognition Pattern

This is classic:

# Combination Generation via Backtracking

---

# Important Mental Models

## Decision Tree

```text id="t8m1k5"
Every choice creates branches
```

---

## Backtracking

```text id="f2m7r9"
choose
explore
return
choose next
```

---

## DFS

Recursion explores one path deeply before returning.

---

# Biggest Takeaway

Backtracking is fundamentally:

```text id="m5r8k1"
systematically exploring all possibilities
```

using recursive decision trees.
