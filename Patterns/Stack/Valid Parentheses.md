# Valid Parentheses

**LeetCode:** [https://leetcode.com/problems/valid-parentheses/](https://leetcode.com/problems/valid-parentheses/)

**Difficulty:** Easy

**Pattern:** Stack (LIFO)

---

# Problem Statement

Given a string `s` containing only the characters:

* `(`
* `)`
* `{`
* `}`
* `[`
* `]`

Determine whether the string is valid.

A valid string must satisfy:

1. Every opening bracket must be closed by the same type of bracket.
2. Brackets must be closed in the correct order.
3. Every closing bracket must have a corresponding opening bracket.

---

# Pattern Recognition

This is a **Stack** problem.

### Why?

Whenever a closing bracket appears, it must match the **most recent unmatched opening bracket**.

Stack follows **LIFO (Last In First Out)**, which perfectly models this behavior.

---

# Intuition

* Traverse the string from left to right.
* If an opening bracket is found, push it onto the stack.
* If a closing bracket is found:

  * If the stack is empty, return `false`.
  * Otherwise, compare it with the stack's top.
  * If they match, pop the stack.
  * Otherwise return `false`.
* At the end, if the stack is empty, the string is valid.

---

# Algorithm

1. Create an empty stack.
2. Traverse every character in the string.
3. If it is an opening bracket, push it into the stack.
4. Otherwise (closing bracket):

   * If the stack is empty, return `false`.
   * Check whether the current closing bracket matches the opening bracket at the top.
   * If it matches, pop the stack.
   * Otherwise return `false`.
5. After traversal, return whether the stack is empty.

---

# Pseudocode

```text
Create an empty stack

For every character c

    If c is an opening bracket
        Push into stack

    Else

        If stack is empty
            Return false

        If stack top matches current bracket
            Pop stack
        Else
            Return false

Return stack is empty
```

---

# Dry Run

### Input

```text
s = "([])"
```

| Character | Stack | Action      |
| --------- | ----- | ----------- |
| (         | (     | Push        |
| [         | ( [   | Push        |
| ]         | (     | Match & Pop |
| )         | Empty | Match & Pop |

Stack becomes empty.

Answer:

```text
true
```

---

# Edge Cases

### Case 1

```text
s = "("
```

No closing bracket.

Answer:

```text
false
```

---

### Case 2

```text
s = ")"
```

Stack is empty when a closing bracket appears.

Answer:

```text
false
```

---

### Case 3

```text
s = "([)]"
```

Top is `[` but current bracket is `)`.

Mismatch.

Answer:

```text
false
```

---

### Case 4

```text
s = ""
```

Nothing to match.

Answer:

```text
true
```

(Logically valid, though LeetCode constraints start from length ≥ 1.)

---

# Why do we check `st.empty()` before `st.top()`?

Consider:

```text
")"
```

The first character is a closing bracket.

The stack is empty.

Calling

```cpp
st.top();
```

on an empty stack is invalid and results in undefined behavior.

Therefore:

```cpp
if(st.empty())
    return false;
```

This also correctly handles cases where a closing bracket appears before any opening bracket.

---

# C++ Solution

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;

        for (char c : s) {

            if (c == '(' || c == '{' || c == '[') {
                st.push(c);
            }
            else {

                if (st.empty())
                    return false;

                if ((st.top() == '(' && c == ')') ||
                    (st.top() == '{' && c == '}') ||
                    (st.top() == '[' && c == ']')) {

                    st.pop();
                }
                else {
                    return false;
                }
            }
        }

        return st.empty();
    }
};
```

---

# Complexity Analysis

**Time Complexity**

```
O(n)
```

Each character is pushed and popped at most once.

**Space Complexity**

```
O(n)
```

In the worst case, every character is an opening bracket.

---

# Key Takeaways

* Use a **Stack** whenever you need to match the **most recent unmatched element**.
* Always check `stack.empty()` before accessing `stack.top()`.
* Push all opening brackets.
* Closing brackets must match the current stack top.
* The string is valid **only if the stack is empty at the end**.

---

# Similar Problems

* Daily Temperatures
* Remove All Adjacent Duplicates in String
* Decode String
* Simplify Path
* Basic Calculator
* Largest Rectangle in Histogram
