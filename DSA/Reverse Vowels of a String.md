# 🧩 Reverse Vowels of a String

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/reverse-vowels-of-a-string/](https://leetcode.com/problems/reverse-vowels-of-a-string/)

---

## 🧠 Problem Understanding

We are given a string `s`.

👉 Goal:

* Reverse only the **vowels** in the string
* Keep all other characters in the same position

---

## 🔍 Key Observations

* Only vowels matter: `a, e, i, o, u` (both cases)
* Non-vowel characters remain unchanged
* Order of vowels should be reversed

---

## 🧠 Core Idea (Two Pointers)

* Use two pointers:

  * `i` → start
  * `j` → end

---

### 💡 Algorithm

1. Move `i` forward until it finds a vowel
2. Move `j` backward until it finds a vowel
3. Swap both vowels
4. Move both pointers
5. Repeat until `i < j`

---

## 💻 Code (Optimal)

```cpp
class Solution {
public:
    string reverseVowels(string s) {
        int i = 0;
        int j = s.length() - 1;
        string v = "aeiouAEIOU";

        while (i < j) {
            if (v.find(s[i]) == string::npos)
                i++;
            else if (v.find(s[j]) == string::npos)
                j--;
            else {
                swap(s[i], s[j]);
                i++;
                j--;
            }
        }

        return s;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

## ⚠️ Edge Cases

* No vowels
* All vowels
* Mixed case
* Single character

---

## ❗ Common Mistakes (VERY IMPORTANT)

### ❌ Using multiple `if` instead of `else if`

```cpp
if (...)
if (...)
if (...)
```

👉 Leads to:

* Multiple pointer movements in one iteration
* Skipped characters
* Incorrect swaps

---

### ✅ Correct approach

```cpp
if (...)
else if (...)
else ...
```

👉 Only ONE action per iteration

---

### ❌ Wrong condition

```cpp
find(...) == string::npos   // means NOT vowel
```

👉 Don’t confuse:

* `== npos` → not vowel
* `!= npos` → vowel

---

### ❌ Re-checking unnecessary conditions

👉 Once inside `else`, both are vowels — no need to check again

---

## 🧠 Patterns Learned

* Two-pointer technique
* String manipulation
* Conditional pointer movement

---

## 🏷 Tags

* Strings
* Two Pointers

---

## 🧠 When to Use This

* When reversing/filtering specific characters
* When working from both ends of string
* When in-place modification is needed

---

## 🔥 Mental Model (Revision Key)

> “Skip non-vowels → swap vowels → move inward”

---

## ⚡ Key Insight

* Control flow (`if / else if / else`) is **critical**
* Pointer movement must be **controlled carefully**

---

## 🚀 Takeaway

* Two-pointer problems = pointer movement + conditions
* Clean logic > complex conditions

---

## ✅ Status

* Solved optimally
* Two-pointer pattern understood
* Edge cases handled
