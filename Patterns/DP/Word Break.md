# 🟦 LeetCode 139 - Word Break

**Difficulty:** Medium
**Pattern:** Dynamic Programming (1D DP - Prefix DP)
**Time Complexity:** `O(n² × L)` *(where `L` is the average substring length due to `substr()`)*
**Space Complexity:** `O(n + m)` *(DP array + HashSet)*

---

# 📌 Problem

Given a string `s` and a dictionary of strings `wordDict`, determine if `s` can be segmented into **one or more dictionary words**.

### Important Points

* Words **can be reused** multiple times.
* Every character of the string must belong to **exactly one** dictionary word.
* Return **true** if **at least one valid segmentation** exists.

---

# Examples

### Example 1

```cpp
Input:
s = "leetcode"
wordDict = ["leet","code"]

Output:
true

Explanation:
leetcode = leet + code
```

---

### Example 2

```cpp
Input:
s = "applepenapple"
wordDict = ["apple","pen"]

Output:
true

Explanation:
apple + pen + apple
```

---

### Example 3

```cpp
Input:
s = "catsandog"
wordDict = ["cats","dog","sand","and","cat"]

Output:
false
```

Possible splits:

```text
cat | sand | og    ❌
cats | and | og    ❌
```

`"og"` is not a dictionary word.

---

# 💡 Key Observation

Instead of asking

> Can the whole string be segmented?

Ask a smaller question:

> **Can the first `i` characters be segmented?**

This naturally leads to Dynamic Programming.

---

# DP State

Define

```cpp
dp[i]
```

as

> **Can the first `i` characters of the string be segmented into dictionary words?**

Examples

```text
s = "leetcode"

dp[0] -> ""
dp[1] -> "l"
dp[2] -> "le"
dp[3] -> "lee"
dp[4] -> "leet"
...
dp[8] -> "leetcode"
```

Notice:

```cpp
dp[n]
```

represents the **entire string**.

---

# Base Case

Empty string is always considered successfully segmented.

Therefore,

```cpp
dp[0] = true;
```

Initialize

```cpp
vector<bool> dp(n + 1, false);
dp[0] = true;
```

---

# Why is `dp[0] = true`?

Suppose

```text
s = "apple"
wordDict = ["apple"]
```

When checking `"apple"`,

we need to verify:

```text
"" | apple
```

Everything before `"apple"` is the empty string.

Therefore,

```cpp
dp[0]
```

must be true.

Otherwise even the first word could never be matched.

---

# DP Transition

For every prefix ending at `i`,

try every possible split position.

```text
0 .... j | j .... i
```

If

* prefix before `j` is valid

AND

* substring `j → i-1` exists in dictionary

then current prefix is valid.

Transition:

```cpp
if (dp[j] && wordSet.count(s.substr(j, i - j))) {
    dp[i] = true;
}
```

---

# Dry Run

```text
s = "leetcode"

wordDict = ["leet","code"]
```

Initially

```text
dp

T F F F F F F F F
```

---

### Compute dp[4]

Try all splits

```text
"" | leet
```

```cpp
dp[0] == true
```

and

```text
"leet"
```

exists.

Therefore

```cpp
dp[4] = true;
```

DP

```text
T F F F T F F F F
```

---

### Compute dp[8]

Try splits

```text
"" | leetcode
```

❌

```text
l | eetcode
```

❌

```text
le | etcode
```

❌

```text
lee | tcode
```

❌

```text
leet | code
```

Now

```cpp
dp[4] == true
```

and

```text
code
```

exists.

Therefore

```cpp
dp[8] = true;
```

Answer

```cpp
return dp[n];
```

---

# Why do we check every split?

For every position

```cpp
j
```

we ask

> What if the last word starts from here?

Example

```text
catsanddog
```

Possible valid segmentations

```text
cat | sand | dog

cats | and | dog
```

Both are valid.

Hence we must try **every split position**.

---

# Why can we use `break`?

Suppose while computing

```cpp
dp[7]
```

we find

```text
cat | sand
```

which is valid.

Now

```cpp
dp[7] = true;
```

Should we still check

```text
cats | and
```

No.

Because the problem asks

> **Does at least one valid segmentation exist?**

The answer is already

```text
YES
```

Another valid segmentation does not change the answer.

Hence

```cpp
if (dp[j] && wordSet.count(s.substr(j, i - j))) {
    dp[i] = true;
    break;
}
```

The `break` only exits the **inner loop**.

The outer loop continues computing future prefixes.

---

# Why convert vector to HashSet?

Instead of

```cpp
vector<string>
```

use

```cpp
unordered_set<string> wordSet(wordDict.begin(), wordDict.end());
```

Reason

Dictionary lookup becomes

```text
Vector
O(m)

HashSet
O(1)
```

which significantly improves performance.

---

# Algorithm

1. Convert dictionary into HashSet.
2. Create DP array of size `n + 1`.
3. Initialize

```cpp
dp[0] = true;
```

4. For every prefix `i`

   * Try every split position `j`

   * If

```cpp
dp[j]
```

is true

AND

```cpp
s.substr(j, i - j)
```

exists in dictionary

then

```cpp
dp[i] = true;
break;
```

5. Return

```cpp
dp[n];
```

---

# C++ Solution

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {

        int n = s.length();

        unordered_set<string> wordSet(wordDict.begin(), wordDict.end());

        vector<bool> dp(n + 1, false);
        dp[0] = true;

        for (int i = 1; i <= n; i++) {

            for (int j = 0; j < i; j++) {

                if (dp[j] && wordSet.count(s.substr(j, i - j))) {

                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[n];
    }
};
```

---

# Complexity Analysis

### Time Complexity

Outer loop

```text
O(n)
```

Inner loop

```text
O(n)
```

Creating substring

```cpp
s.substr(...)
```

takes

```text
O(L)
```

where `L` is the substring length.

Overall

```text
O(n² × L)
```

In the worst case,

```text
L = O(n)
```

so worst-case complexity becomes

```text
O(n³)
```

> **Interview Note:** Many interviewers accept `O(n²)` assuming substring lookup is treated as constant or optimized. In C++, be aware that `substr()` copies characters, making the strict worst-case `O(n³)`.

---

### Space Complexity

DP array

```text
O(n)
```

HashSet

```text
O(m)
```

Overall

```text
O(n + m)
```

where `m` is the total number of dictionary words.

---

# Common Mistakes

### ❌ Setting `dp[0] = false`

Wrong.

Empty string is always segmentable.

Correct

```cpp
dp[0] = true;
```

---

### ❌ Forgetting HashSet

Searching the vector every time makes lookups expensive.

Use

```cpp
unordered_set<string>
```

---

### ❌ Returning `dp[n-1]`

Wrong.

`dp[i]` represents the first `i` characters.

Entire string is

```cpp
dp[n]
```

---

### ❌ Forgetting `break`

The answer remains correct without `break`, but unnecessary checks increase runtime.

---

### ❌ Misunderstanding `break`

`break` exits **only the inner loop**.

The outer loop still computes

```text
dp[i+1], dp[i+2], ...
```

---

# Pattern Recognition

This is a **Prefix DP** problem.

Whenever you see:

* Split a string
* Partition into valid words
* Check if a prefix is valid
* Can the string be segmented?

Think:

```cpp
dp[i]
```

=

> **Can the first `i` characters satisfy the condition?**

---

# Similar Problems

* LeetCode 140 – Word Break II
* LeetCode 472 – Concatenated Words
* LeetCode 91 – Decode Ways
* LeetCode 97 – Interleaving String
* LeetCode 132 – Palindrome Partitioning II

---

# Interview Takeaways

* DP state should clearly represent a **prefix**, not an index.
* `dp[0] = true` acts as the starting point for building valid prefixes.
* Try every possible split position `j`.
* Use a HashSet for efficient dictionary lookups.
* Since this is an **existence problem (true/false)**, stop checking further splits once one valid segmentation is found (`break`).
* Return `dp[n]`, which represents whether the **entire string** can be segmented.
