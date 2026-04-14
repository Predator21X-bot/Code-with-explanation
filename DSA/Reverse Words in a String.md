# 🧩 Reverse Words in a String

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/reverse-words-in-a-string/](https://leetcode.com/problems/reverse-words-in-a-string/)

---

## 🧠 Problem Understanding

We are given a string `s`.

👉 Goal:

* Reverse the **order of words**
* Remove extra spaces:

  * Leading ❌
  * Trailing ❌
  * Multiple spaces → single space

---

## 🔍 Key Observations

* A word = **continuous sequence of non-space characters**
* Input may contain:

  * Multiple spaces
  * Leading/trailing spaces

---

# ⚡ Approach 1: StringStream + Build from Front (Recommended)

---

## 🧠 Core Idea

1. Use `stringstream` to extract words
2. Build result by adding each word **at the front**

---

## 💻 Code

```cpp id="z7v4b2"
class Solution {
public:
    string reverseWords(string s) {
        string result = "";
        stringstream ss(s);
        string word;

        while (ss >> word) {
            if (result.empty())
                result = word;
            else
                result = word + " " + result;
        }

        return result;
    }
};
```

---

## 🧠 How it Works

### 🔹 `stringstream`

```cpp id="f1v2m3"
while (ss >> word)
```

👉 Automatically:

* Ignores extra spaces
* Extracts only valid words

---

### 🔹 Building result

```cpp id="a9x8d7"
result = word + " " + result;
```

👉 Adds word **in front**
👉 Reverses order naturally

---

## 🧠 Dry Run Example

Input:

```cpp id="dry1"
"  the sky  is blue  "
```

Steps:

```cpp id="dry2"
"the"
"sky the"
"is sky the"
"blue is sky the"
```

---

## ⏱ Complexity

* Time: **O(n²)** (due to repeated string concatenation)
* Space: **O(n)**

---

# ⚡ Approach 2: In-place Optimization (Advanced)

---

## 🧠 Idea

1. Reverse entire string
2. Reverse each word
3. Remove extra spaces

---

## 💡 Why better?

* Avoids extra string copying
* Closer to **O(n)** time

---

## ⚠️ Use When

* Asked for **in-place / optimal solution**
* Performance matters

---

# ❗ Common Mistakes

* Forgetting to handle multiple spaces
* Adding extra space at end
* Misunderstanding `result.empty()`
* Trying to use `push_front` on string ❌

---

# 🧠 Patterns Learned

* String parsing
* Two-pointer vs parsing approaches
* Building result in reverse order

---

# 🏷 Tags

* Strings
* Two Pointers
* Parsing

---

# 🧠 When to Use This

* When reversing **order of tokens/words**
* When input contains irregular spacing

---

# 🔥 Mental Model (Revision Key)

> “Extract words → prepend → build reversed sentence”

---

# ⚡ Key Insights

* `stringstream` cleans input automatically
* Order of concatenation controls placement
* First word initializes result

---

# 🚀 Takeaway

* Clean solution > complex optimization
* Understand flow before optimizing
