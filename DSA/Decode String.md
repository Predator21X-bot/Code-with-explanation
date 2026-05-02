# 🧩 Decode String

### 🔗 Problem Link

https://leetcode.com/problems/decode-string/

---

# 🧠 Problem Understanding

Given:

* Encoded string with pattern:

```text
k[substring]
```

👉 Means:

```text
repeat substring k times
```

---

## 🔍 Example

```text
Input:  "3[a2[c]]"
Output: "accaccacc"
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: What are we doing?

```text
Expand encoded patterns
```

---

## 🧩 Step 2: Why is it tricky?

👉 Because of **nesting**

```text
3[a2[c]]
```

* inner: `2[c] → cc`
* outer: `3[acc] → accaccacc`

---

## 🧠 Key challenge

👉 We must remember:

```text
previous string + repeat count
```

---

# ⚡ Approach: Stack + Simulation

---

## 🧠 Idea

> “Pause current state → process inner → resume”

---

## 🧩 Data structures

```cpp
stack<int> numStack;      // stores repeat counts
stack<string> strStack;   // stores previous strings
```

---

## 🧩 Variables

```cpp
string curr = "";
int num = 0;
```

---

# 💻 Code

```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<int> numStack;
        stack<string> strStack;

        string curr = "";
        int num = 0;

        for (char c : s) {
            if (isdigit(c)) {
                num = num * 10 + (c - '0');
            } 
            else if (c == '[') {
                numStack.push(num);
                strStack.push(curr);

                num = 0;
                curr = "";
            } 
            else if (c == ']') {
                string temp = curr;
                curr = strStack.top();
                strStack.pop();

                int repeat = numStack.top();
                numStack.pop();

                while (repeat--) {
                    curr += temp;
                }
            } 
            else {
                curr += c;
            }
        }

        return curr;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n × k)** (string expansion)
* Space: **O(n)**

---

# 🔥 Step-by-Step Example

```text
s = "3[a2[c]]"
```

---

## Step 1

```text
num = 3
push → numStack = [3], strStack = [""]
```

---

## Step 2

```text
curr = "a"
```

---

## Step 3

```text
num = 2
push → numStack = [3,2], strStack = ["","a"]
```

---

## Step 4

```text
curr = "c"
```

---

## Step 5

```text
']' → expand "c" × 2 → "cc"
curr = "a" + "cc" = "acc"
```

---

## Step 6

```text
']' → expand "acc" × 3 → "accaccacc"
```

---

# 🧠 Mental Model (REVISION KEY)

> “Stack stores context of nested decoding”

---

# ⚡ Pattern Breakdown

---

## 🔹 Digit

```cpp
num = num * 10 + (c - '0');
```

---

## 🔹 '['

```cpp
push current state
reset current
```

---

## 🔹 ']'

```cpp
pop state
repeat substring
merge
```

---

## 🔹 Character

```cpp
append to current string
```

---

# ❗ Common Mistakes

* ❌ Using only one stack
* ❌ Using `stack<char>`
* ❌ Not handling multi-digit numbers
* ❌ Forgetting to reset `num` and `curr`
* ❌ Incorrect order of operations

---

# 🧠 Patterns Learned

* Stack (nested structure)
* String building
* Simulation

---

# 🏷 Tags

* Stack
* String
* Simulation

---

# 🔥 Recognition Pattern

Use this when:

* Nested structures
* Encoded patterns
* Parentheses-like problems
