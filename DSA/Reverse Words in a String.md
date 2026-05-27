# 🧩 Reverse Words in a String

## 🔗 Problem Link

LeetCode: [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/?utm_source=chatgpt.com)

---

# 📌 Problem Statement

Given a string `s`, reverse the order of the words.

A **word** is defined as a sequence of non-space characters.

Additionally:

* Remove leading spaces
* Remove trailing spaces
* Reduce multiple spaces between words to a single space

---

# 🧠 Examples

## Example 1

```cpp
Input:  "the sky is blue"
Output: "blue is sky the"
```

---

## Example 2

```cpp
Input:  "  hello world  "
Output: "world hello"
```

---

## Example 3

```cpp
Input:  "a good   example"
Output: "example good a"
```

---

# 🔍 Key Observations

* Words are separated by spaces
* Input may contain:

  * Leading spaces
  * Trailing spaces
  * Multiple spaces between words
* Final answer must contain:

  * Exactly one space between words
  * No extra spaces

---

# ⚡ Approach 1 — Using `stringstream` (Clean & Easy)

---

# 🧠 Core Idea

1. Extract words using `stringstream`
2. Build answer in reverse order

---

# 📚 What is `stringstream`?

`stringstream` is a class in C++ used to treat a string like an input/output stream.

It works similarly to `cin`.

---

## ✅ Header File

```cpp
#include <sstream>
```

---

# 🧠 Why Use It?

It automatically:

* Splits words
* Ignores multiple spaces
* Makes parsing strings easier

---

# ✅ Example

```cpp
#include <iostream>
#include <sstream>
using namespace std;

int main() {
    string s = "  hello   world  ";

    stringstream ss(s);

    string word;

    while (ss >> word) {
        cout << word << endl;
    }
}
```

---

## ✅ Output

```cpp
hello
world
```

---

# 🔥 Important Behavior

```cpp
ss >> word
```

automatically:

* skips leading spaces
* skips multiple spaces
* extracts one word at a time

This is why `stringstream` is perfect for this problem.

---

# ✅ Solution Using `stringstream`

```cpp
class Solution {
public:
    string reverseWords(string s) {

        stringstream ss(s);

        string word;
        string result = "";

        while (ss >> word) {

            if (result.empty()) {
                result = word;
            }
            else {
                result = word + " " + result;
            }
        }

        return result;
    }
};
```

---

# 🧠 Dry Run

Input:

```cpp
"  the sky   is blue  "
```

---

## Step-by-Step

| Extracted Word | Result              |
| -------------- | ------------------- |
| the            | `"the"`             |
| sky            | `"sky the"`         |
| is             | `"is sky the"`      |
| blue           | `"blue is sky the"` |

---

# ⏱ Complexity Analysis

## Time Complexity

```cpp
O(n²)
```

Why?

Because:

```cpp
result = word + " " + result;
```

creates a new string every iteration.

---

## Space Complexity

```cpp
O(n)
```

---

# ❗ Drawback of This Approach

Repeated string concatenation at the front is expensive.

---

# ⚡ Approach 2 — Optimized Solution (O(n))

---

# 🧠 Core Idea

Instead of adding words at the front repeatedly:

1. Extract all words
2. Store them in a vector
3. Traverse vector backwards
4. Build final answer efficiently

---

# ✅ Optimized Code

```cpp
class Solution {
public:
    string reverseWords(string s) {

        stringstream ss(s);

        vector<string> words;
        string word;

        // Extract words
        while (ss >> word) {
            words.push_back(word);
        }

        string result = "";

        // Build reversed sentence
        for (int i = words.size() - 1; i >= 0; i--) {

            result += words[i];

            if (i != 0) {
                result += " ";
            }
        }

        return result;
    }
};
```

---

# 🧠 Dry Run

Input:

```cpp
"the sky is blue"
```

---

## Step 1: Store Words

```cpp
["the", "sky", "is", "blue"]
```

---

## Step 2: Traverse Backwards

```cpp
"blue"
"blue is"
"blue is sky"
"blue is sky the"
```

---

# ⏱ Complexity Analysis

## Time Complexity

```cpp
O(n)
```

Each character processed a constant number of times.

---

## Space Complexity

```cpp
O(n)
```

for storing words.

---

# ⚡ Approach 3 — In-place Optimal Two Pointer Solution

---

# 🧠 Core Idea

1. Remove extra spaces
2. Reverse entire string
3. Reverse each individual word

---

# 🔥 Why This Works

Suppose:

```cpp
"the sky is blue"
```

After reversing whole string:

```cpp
"eulb si yks eht"
```

Now reverse each word:

```cpp
"blue is sky the"
```

Done ✅

---

# ✅ Fully Optimized Code

```cpp
class Solution {
public:

    void reverseRange(string &s, int left, int right) {

        while (left < right) {
            swap(s[left++], s[right--]);
        }
    }

    string cleanSpaces(string s) {

        int n = s.length();

        string result = "";

        int i = 0;

        while (i < n) {

            // Skip spaces
            while (i < n && s[i] == ' ') {
                i++;
            }

            // Add word
            while (i < n && s[i] != ' ') {
                result += s[i];
                i++;
            }

            // Skip extra spaces
            while (i < n && s[i] == ' ') {
                i++;
            }

            // Add single space if needed
            if (i < n) {
                result += ' ';
            }
        }

        return result;
    }

    string reverseWords(string s) {

        // Step 1: Remove extra spaces
        s = cleanSpaces(s);

        int n = s.length();

        // Step 2: Reverse whole string
        reverseRange(s, 0, n - 1);

        // Step 3: Reverse each word
        int start = 0;

        for (int end = 0; end <= n; end++) {

            if (end == n || s[end] == ' ') {

                reverseRange(s, start, end - 1);

                start = end + 1;
            }
        }

        return s;
    }
};
```

---

# 🧠 Dry Run

Input:

```cpp
"  the sky is blue  "
```

---

## Step 1 — Remove Spaces

```cpp
"the sky is blue"
```

---

## Step 2 — Reverse Whole String

```cpp
"eulb si yks eht"
```

---

## Step 3 — Reverse Individual Words

```cpp
"blue is sky the"
```

---

# ⏱ Complexity Analysis

## Time Complexity

```cpp
O(n)
```

---

## Space Complexity

```cpp
O(1)
```

(ignoring output string)

---

# ⚠️ Common Mistakes

---

## ❌ Forgetting Multiple Spaces

```cpp
"a   good   example"
```

Need:

```cpp
"example good a"
```

---

## ❌ Extra Space at End

Wrong:

```cpp
"blue is sky the "
```

Correct:

```cpp
"blue is sky the"
```

---

## ❌ Using Front Concatenation Excessively

```cpp
result = word + result;
```

This causes repeated copying.

---

## ❌ Forgetting Empty String Cases

Input:

```cpp
"     "
```

Output should be:

```cpp
""
```

---

# 🧠 Important Concepts Learned

---

## ✅ String Parsing

Extracting meaningful tokens from strings.

---

## ✅ `stringstream`

Useful for:

* parsing words
* splitting strings
* handling spaces automatically

---

## ✅ Two Pointer Technique

Used for:

* reversing
* trimming spaces
* in-place operations

---

## ✅ String Reversal Pattern

Very common interview technique.

---

# 🏷 Tags

* Strings
* Two Pointers
* StringStream
* Parsing
* In-place Algorithms

---

# 🔥 Interview Tips

If interviewer asks:

---

## ✅ “Simple readable solution?”

Use:

```cpp
stringstream
```

---

## ✅ “Most optimized solution?”

Use:

* reverse whole string
* reverse individual words
* two pointers

---

# 🚀 Final Takeaway

| Approach                   | Time  | Space | Difficulty |
| -------------------------- | ----- | ----- | ---------- |
| Stringstream + prepend     | O(n²) | O(n)  | Easy       |
| Vector + reverse traversal | O(n)  | O(n)  | Medium     |
| In-place reverse           | O(n)  | O(1)  | Hard       |

---

# 🧠 Revision One-Liner

> “Clean spaces → reverse whole string → reverse each word”
