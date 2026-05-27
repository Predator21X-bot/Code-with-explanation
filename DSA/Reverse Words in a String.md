# 🧩 Reverse Words in a String

## 🔗 Problem Link

LeetCode: [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/?utm_source=chatgpt.com)

---

# 📌 Problem Statement

Given a string `s`, reverse the order of words.

A word is defined as a sequence of non-space characters.

Additionally:

* Remove leading spaces
* Remove trailing spaces
* Remove multiple spaces between words

---

# 🧠 Example

```cpp
Input:  "  the sky   is blue  "

Output: "blue is sky the"
```

---

# 🧠 Intuition

We need to:

1. Extract valid words
2. Ignore unnecessary spaces
3. Reverse word order

Instead of manually parsing spaces, we can use:

```cpp
stringstream
```

which automatically extracts words cleanly.

---

# ⚡ Approach 1 — Using `stringstream`

---

# 🧠 What is `stringstream`?

`stringstream` is a C++ class that allows us to treat a string like an input stream.

It works similarly to:

```cpp
cin
```

but reads data from a string instead of keyboard input.

---

# ✅ Header File

```cpp
#include <sstream>
```

---

# 🧠 Syntax

```cpp
stringstream ss(stringName);
```

Example:

```cpp
string s = "hello world";

stringstream ss(s);
```

Now `ss` can extract words one by one.

---

# 🧠 How Extraction Works

```cpp
ss >> word
```

This means:

👉 Extract next word from stream into variable `word`

It automatically:

* skips leading spaces
* skips multiple spaces
* stops at space

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

# ✅ Output

```cpp
hello
world
```

---

# 🧠 Why is `word` String Used?

```cpp
string word;
```

We need a temporary variable to store each extracted word.

Example:

Input:

```cpp
"the sky is blue"
```

Iterations:

| Extraction | word stores |
| ---------- | ----------- |
| 1          | `"the"`     |
| 2          | `"sky"`     |
| 3          | `"is"`      |
| 4          | `"blue"`    |

---

# ⚡ Algorithm (StringStream Solution)

---

## ✅ Step-by-Step Algorithm

### Step 1

Create stringstream object

```cpp
stringstream ss(s);
```

---

### Step 2

Extract words one by one

```cpp
while (ss >> word)
```

---

### Step 3

Add current word at front of result

```cpp
result = word + " " + result;
```

This reverses the order.

---

### Step 4

Return final answer

---

# ✅ Code

```cpp
class Solution {
public:

    string reverseWords(string s) {

        // Step 1: Create stringstream
        stringstream ss(s);

        // Stores extracted word
        string word;

        // Final answer
        string result = "";

        // Step 2: Extract words one by one
        while (ss >> word) {

            // First word
            if (result.empty()) {
                result = word;
            }

            // Add current word at front
            else {
                result = word + " " + result;
            }
        }

        // Step 3: Return result
        return result;
    }
};
```

---

# 🧠 How This Code Works Internally

---

## Input

```cpp
"  the sky   is blue  "
```

---

# 🔹 Initial State

```cpp
result = ""
```

---

# 🔹 Iteration 1

```cpp
word = "the"
```

Since result is empty:

```cpp
result = "the"
```

---

# 🔹 Iteration 2

```cpp
word = "sky"
```

Now:

```cpp
result = "sky the"
```

---

# 🔹 Iteration 3

```cpp
word = "is"
```

Now:

```cpp
result = "is sky the"
```

---

# 🔹 Iteration 4

```cpp
word = "blue"
```

Now:

```cpp
result = "blue is sky the"
```

---

# ✅ Final Answer

```cpp
"blue is sky the"
```

---

# 🧠 Why `result.empty()` is Used?

For first word:

```cpp
result = word;
```

Otherwise:

```cpp
result = word + " " + result;
```

This avoids extra space at beginning/end.

---

# ❗ Problem in This Solution

This line:

```cpp
result = word + " " + result;
```

creates new strings repeatedly.

So time complexity becomes:

```cpp
O(n²)
```

---

# ⚡ Better Optimized Solution

Instead of adding at front repeatedly:

1. Store words in vector
2. Traverse vector backwards

---

# ⚡ Optimized Algorithm

---

## ✅ Step 1

Extract all words using stringstream

---

## ✅ Step 2

Store words in vector

Example:

```cpp
["the", "sky", "is", "blue"]
```

---

## ✅ Step 3

Traverse vector from back

```cpp
blue
is
sky
the
```

---

## ✅ Step 4

Build final answer

---

# ✅ Optimized Code

```cpp
class Solution {
public:

    string reverseWords(string s) {

        // Create stream
        stringstream ss(s);

        // Temporary word storage
        string word;

        // Store all words
        vector<string> words;

        // Extract words
        while (ss >> word) {
            words.push_back(word);
        }

        // Final result
        string result = "";

        // Traverse backwards
        for (int i = words.size() - 1; i >= 0; i--) {

            result += words[i];

            // Add space between words
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

# 🔹 Vector Stores

```cpp
["the", "sky", "is", "blue"]
```

---

# 🔹 Reverse Traversal

| i | Add  |
| - | ---- |
| 3 | blue |
| 2 | is   |
| 1 | sky  |
| 0 | the  |

---

# ✅ Final Output

```cpp
"blue is sky the"
```

---

# ⏱ Complexity Analysis

| Approach            | Time  | Space |
| ------------------- | ----- | ----- |
| Front Concatenation | O(n²) | O(n)  |
| Vector + Reverse    | O(n)  | O(n)  |

---

# ⚡ Most Optimized In-place Solution

---

# 🧠 Algorithm

1. Remove extra spaces
2. Reverse complete string
3. Reverse each word individually

---

# ✅ Example

Input:

```cpp
"the sky is blue"
```

---

## Reverse whole string

```cpp
"eulb si yks eht"
```

---

## Reverse each word

```cpp
"blue is sky the"
```

---
Yes 😄 you are absolutely right.
Your notes currently explain the **most optimized approach algorithm**, but the **actual code is missing**. 

You should add the complete in-place optimized code after:

```cpp
# ⚡ Most Optimized In-place Solution
```

---

# ✅ Add This Section to Your Notes

## ✅ Most Optimized In-place Code

```cpp
class Solution {
public:

    // Reverse characters between left and right
    void reverseRange(string &s, int left, int right) {

        while (left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }

    string reverseWords(string s) {

        int n = s.length();

        // -----------------------------
        // Step 1: Remove Extra Spaces
        // -----------------------------

        string temp = "";

        int i = 0;

        while (i < n) {

            // Skip spaces
            while (i < n && s[i] == ' ') {
                i++;
            }

            // Add word characters
            while (i < n && s[i] != ' ') {
                temp += s[i];
                i++;
            }

            // Skip extra spaces
            while (i < n && s[i] == ' ') {
                i++;
            }

            // Add single space if more words exist
            if (i < n) {
                temp += ' ';
            }
        }

        s = temp;

        n = s.length();

        // -----------------------------
        // Step 2: Reverse Whole String
        // -----------------------------

        reverseRange(s, 0, n - 1);

        // -----------------------------
        // Step 3: Reverse Each Word
        // -----------------------------

        int start = 0;

        for (int end = 0; end <= n; end++) {

            // Word end found
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

# 🧠 How This Optimized Solution Works

---

# ✅ Step 1 — Remove Extra Spaces

Input:

```cpp
"  the sky   is blue  "
```

After cleanup:

```cpp
"the sky is blue"
```

We:

* remove leading spaces
* remove trailing spaces
* reduce multiple spaces to one

---

# ✅ Step 2 — Reverse Entire String

```cpp
"eulb si yks eht"
```

Entire sentence reversed.

---

# ✅ Step 3 — Reverse Each Word

Reverse:

```cpp
"eulb" -> "blue"
"si"   -> "is"
"yks"  -> "sky"
"eht"  -> "the"
```

Final:

```cpp
"blue is sky the"
```

---

# 🧠 Why This is Most Optimized

Because:

* each character visited limited times
* no repeated front concatenation
* no expensive copying repeatedly

---

# ⏱ Complexity

| Complexity | Value |
| ---------- | ----- |
| Time       | O(n)  |
| Space      | O(1)  |

---

# 🔥 Important Concept

This line:

```cpp
reverseRange(s, 0, n - 1);
```

reverses:

```cpp
"the sky is blue"
```

into:

```cpp
"eulb si yks eht"
```

Then we fix individual words.

This is a VERY common interview trick.

# ✅ Time Complexity

```cpp
O(n)
```

---

# ✅ Space Complexity

```cpp
O(1)
```

---

# ❗ Common Mistakes

---

## ❌ Forgetting Extra Spaces

Input:

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

## ❌ Misunderstanding `stringstream`

It extracts:

* word by word
* not character by character

---

# 🧠 Important Concepts Learned

* String Parsing
* `stringstream`
* Reversal Techniques
* Two Pointers
* Space Handling
* String Manipulation

---

# 🔥 Interview Tip

If interviewer asks:

---

## ✅ Easy Readable Solution

Use:

```cpp
stringstream
```

---

## ✅ Best Optimized Solution

Use:

* reverse whole string
* reverse individual words
* two pointers

---

# 🚀 Final Revision Line

> “Extract words cleanly using stringstream → reverse order → build answer”
