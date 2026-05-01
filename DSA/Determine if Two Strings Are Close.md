# 🧩 Determine if Two Strings Are Close

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/determine-if-two-strings-are-close/](https://leetcode.com/problems/determine-if-two-strings-are-close/)

---

# 🧠 Problem Understanding

Two strings are **close** if we can:

1. Rearrange characters
2. Swap frequencies between characters

---

## ❗ Important restriction

```text
We CANNOT introduce new characters
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Check same characters

---

### 🧠 Why?

> We cannot create new characters

---

### ❌ Example (fail immediately)

```text
word1 = "abc"
word2 = "abd"
```

👉 `c` cannot become `d`
👉 Impossible

```text
return false
```

---

### ✅ Condition

```text
Set(word1) == Set(word2)
```

---

---

## 🧩 Step 2: Count frequencies

---

### 🧠 Why?

We need:

```text
how many times each character appears
```

---

### Example

```text
word1 = "aab" → {a:2, b:1}
word2 = "bba" → {b:2, a:1}
```

---

---

## 🧩 Step 3: Compare frequency distribution

---

### 🧠 Key insight

We can:

```text
swap frequencies between characters
```

---

### ❌ Not required

```text
same character → same frequency ❌
```

---

### ✅ Required

```text
same multiset of frequencies
```

---

### Example (valid)

```text
word1 = "aab" → [2,1]
word2 = "bba" → [2,1]

sorted → [1,2] == [1,2] ✅
```

---

### ❌ Example (invalid)

```text
word1 = "aab" → [2,1]
word2 = "abc" → [1,1,1]

sorted → mismatch ❌
```

---

---

## 🧩 Step 4: Sorting frequencies

---

### 🧠 Why?

Order doesn’t matter:

```text
[2,1] == [1,2] after sorting
```

---

# ⚡ Approach: Hashing + Sorting

---

## 💻 Code

```cpp
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        if (word1.size() != word2.size()) return false;

        unordered_map<char,int> mp1, mp2;

        for (char c : word1) mp1[c]++;
        for (char c : word2) mp2[c]++;

        // Step 1: same characters
        for (auto &p : mp1) {
            if (mp2.find(p.first) == mp2.end()) return false;
        }

        // Step 2: extract frequencies
        vector<int> f1, f2;

        for (auto &p : mp1) f1.push_back(p.second);
        for (auto &p : mp2) f2.push_back(p.second);

        // Step 3: sort
        sort(f1.begin(), f1.end());
        sort(f2.begin(), f2.end());

        // Step 4: compare
        return f1 == f2;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n log n)** (due to sorting)
* Space: **O(1)** (since alphabet is limited)

---

# ❗ Common Mistakes

* ❌ Not checking character set equality
* ❌ Comparing maps directly
* ❌ Sorting map instead of values
* ❌ Thinking exact frequency match is needed

---

# 🧠 Patterns Learned

* Frequency counting
* Set equality
* Multiset comparison
* Transformation constraints

---

# 🏷 Tags

* Hashing
* Sorting
* Strings

---

# 🔥 Mental Model (REVISION KEY)

> “Same characters + same frequency distribution”

---

# ⚡ Key Insight (VERY IMPORTANT)

```text
Characters = fixed  
Frequencies = flexible
```

---

# 🧠 Real-world analogy

```text
People = characters  
Money = frequency  

You can redistribute money  
❌ but cannot add/remove people
```

---

# 🚀 Takeaway

* Always understand **allowed operations**
* Translate operations into **constraints**
* Solve based on constraints, not brute force
