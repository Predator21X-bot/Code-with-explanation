# 🧩 Merge Strings Alternately

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/merge-strings-alternately/](https://leetcode.com/problems/merge-strings-alternately/)

---

## 🧠 Problem Understanding

We are given two strings `word1` and `word2`.

We need to merge them by:

* Taking characters **alternately**
* Starting from `word1`

If one string finishes early:

* Append the remaining characters of the other string

---

## 🔍 Key Observations

* This is a **two-pointer problem**
* We traverse both strings **simultaneously**
* Once one string ends → append remaining part

---

## 🧪 Examples

### Example 1

```cpp
Input:
word1 = "abc"
word2 = "pqr"

Output:
"apbqcr"
```

### Example 2

```cpp
Input:
word1 = "ab"
word2 = "pqrs"

Output:
"apbqrs"
```

### Example 3

```cpp
Input:
word1 = "abcd"
word2 = "pq"

Output:
"apbqcd"
```

---

## 🐢 Brute Force Idea

* Use extra checks at each step to decide which character to pick
* Could simulate alternating using conditionals

👉 Not efficient or clean

### ⏱ Complexity

* Time: O(n + m)
* Space: O(n + m)

---

## ⚡ Optimized Approach (Two Pointers)

### 💡 Idea

1. Initialize two pointers:

   * `i = 0` for `word1`
   * `j = 0` for `word2`

2. While both have characters:

   * Add `word1[i]`
   * Add `word2[j]`
   * Increment both

3. Append remaining characters:

   * If `word1` not finished → add rest
   * If `word2` not finished → add rest

---

## 💻 Code

```cpp
class Solution {
public:
    string mergeAlternately(string word1, string word2) {
        int n = word1.size(), m = word2.size();
        string result;
        result.reserve(n + m);

        int i = 0, j = 0;

        while (i < n && j < m) {
            result.push_back(word1[i]);
            result.push_back(word2[j]);
            i++;
            j++;
        }

        while (i < n) {
            result.push_back(word1[i]);
            i++;
        }

        while (j < m) {
            result.push_back(word2[j]);
            j++;
        }

        return result;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n + m)**
* Space: **O(n + m)**

---

## ⚠️ Edge Cases

* One string is empty
* Both strings are empty
* One string much longer than the other

---

## ❗ Common Mistakes

* Using wrong loop condition (`j < n` instead of `j < m`)
* Traversing backwards (`i--`) instead of forward
* Using `append()` incorrectly for single characters
* Not handling remaining characters properly

---

## 🧠 Patterns Learned

* Two pointers on strings
* Merging/interleaving technique
* Handling leftover elements

---

## 🏷 Tags

* Strings
* Two Pointers
* Simulation

---

## 🧠 When to Use This

* When merging two sequences alternately
* When both inputs need to be processed simultaneously
* When order matters but no sorting is required

---

## 🔥 Mental Model (Revision Key)

> “Take turns → when one ends → dump the rest”

---

## 🚀 Takeaway

* Classic **two-pointer merge pattern**
* Always think:

  > “process together → then handle leftovers”
