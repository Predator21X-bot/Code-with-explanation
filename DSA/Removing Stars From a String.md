# 🧩 Removing Stars From a String

### 🔗 Problem Link

https://leetcode.com/problems/removing-stars-from-a-string/

---

# 🧠 Problem Understanding

Given:

* String `s` containing characters and `'*'`

👉 Rule:

```text
Each '*' removes the closest non-star character to its left
```

👉 Goal:
Return final string after all removals

---

## 🔍 Example

```text
s = "leet**cod*e"

Step-by-step:
l e e t * * → remove t, then e
→ l e

cod* → remove d

Final → "lecoe"
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Understand behavior

👉 When we see `*`:

```text
remove last added character
```

---

## 🧠 Key observation

```text
Last In → First Out (LIFO)
```

👉 This is exactly:

```text
STACK behavior
```

---

# ⚡ Approach 1: Using Stack

---

## 🧠 Idea

* Push characters
* On `*`, pop last character

---

## 💻 Code

```cpp
class Solution {
public:
    string removeStars(string s) {
        stack<char> st;

        for (char c : s) {
            if (c == '*') {
                st.pop();
            } else {
                st.push(c);
            }
        }

        string result = "";

        while (!st.empty()) {
            result += st.top();
            st.pop();
        }

        reverse(result.begin(), result.end());

        return result;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# ⚡ Approach 2: Using String as Stack (Optimal)

---

## 🧠 Idea

👉 String can act like stack:

* `push_back()` → push
* `pop_back()` → pop

---

## 💻 Code

```cpp
class Solution {
public:
    string removeStars(string s) {
        string result;

        for (char c : s) {
            if (c == '*') {
                result.pop_back();
            } else {
                result.push_back(c);
            }
        }

        return result;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# 🔥 Why Approach 2 is Better

| Feature     | Stack  | String          |
| ----------- | ------ | --------------- |
| Extra DS    | Yes    | No              |
| Code length | Longer | Short           |
| Performance | Same   | Slightly better |

---

# 🧠 Patterns Learned

* Stack (LIFO behavior)
* Simulation problems
* Using string as stack

---

# 🏷 Tags

* Stack
* String
* Simulation

---

# ❗ Common Mistakes

* ❌ Using `stack<string>` instead of `stack<char>`
* ❌ Returning original string
* ❌ Forgetting to rebuild string from stack
* ❌ Not handling order (reverse needed)

---

# 🔥 Mental Model (REVISION KEY)

> “Push characters → pop when star appears”

---

# ⚡ Recognition Pattern

Use stack when:

* Undo operations
* Remove previous element
* Matching patterns (parentheses etc.)
* LIFO behavior
