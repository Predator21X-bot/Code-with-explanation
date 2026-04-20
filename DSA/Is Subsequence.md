# 🧩 Is Subsequence

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/is-subsequence/](https://leetcode.com/problems/is-subsequence/)

---

# 🧠 Problem Understanding

Given:

* `s` → smaller string
* `t` → larger string

👉 Check:

> Is `s` a subsequence of `t`?

---

## 🧠 Definition

> A subsequence means:

```text
Characters appear in order, but not necessarily contiguous
```

---

## 🔍 Example

```text
s = "abc"
t = "ahbgdc"
→ TRUE
```

```text
s = "axc"
t = "ahbgdc"
→ FALSE
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: What are we doing?

👉 Trying to match all characters of `s` inside `t` in order

---

## 🧩 Step 2: Use two pointers

| Pointer | Role       |
| ------- | ---------- |
| `i`     | tracks `s` |
| `j`     | scans `t`  |

---

## 🧩 Step 3: Movement logic

```text
if s[i] == t[j]:
    i++   (match found)

always:
    j++   (keep scanning t)
```

---

## 🧩 Step 4: When do we succeed?

👉 If:

```cpp
i == s.length()
```

---

# ⚡ Approach: Two Pointers

---

## 💻 Code

```cpp id="code"
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int i = 0, j = 0;

        while (i < s.length() && j < t.length()) {
            if (s[i] == t[j]) {
                i++;
            }
            j++;
        }

        return i == s.length();
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)** (length of `t`)
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Forgetting to increment `j` always
* ❌ Using wrong loop condition
* ❌ Not checking `i == s.length()` at end
* ❌ Trying to match both pointers blindly

---

# 🧠 Patterns Learned

* Two-pointer scanning
* Subsequence matching
* Greedy consumption

---

# 🏷 Tags

* String
* Two Pointers

---

# 🔥 Mental Model (REVISION KEY)

> “Scan `t` and try to consume `s`”

---

# ⚡ Recognition Pattern

Use this when:

* Matching order matters
* Continuity doesn’t matter
* One string is smaller

---

# 🧠 Template (Reusable)

```text
i = 0
for j in t:
    if s[i] == t[j]:
        i++
return i == len(s)
```

---

# 🚀 Variations (IMPORTANT)

---

## 🔹 Multiple queries (advanced)

If many `s` queries on same `t`:
👉 Preprocess `t` using hashmap + binary search

---

## 🔹 Delete characters problem

👉 Similar pattern:

> remove characters to make subsequence

---

## 🔹 LCS (advanced version)

👉 When:

* order matters
* AND optimal length needed

---

# 🧠 BFS vs Two Pointers (clarity)

| Problem type      | Approach     |
| ----------------- | ------------ |
| grid / graph      | BFS / DFS    |
| sequence matching | two pointers |

---

# 🚀 Takeaway

* Always identify:

  * **what to match**
  * **where to scan**
