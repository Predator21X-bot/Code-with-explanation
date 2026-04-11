# 🧩 Greatest Common Divisor of Strings

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/greatest-common-divisor-of-strings/](https://leetcode.com/problems/greatest-common-divisor-of-strings/)

---

## 🧠 Problem Understanding

We are given two strings `str1` and `str2`.

We need to find the **largest string `x`** such that:

* `x` repeated multiple times forms `str1`
* `x` repeated multiple times forms `str2`

---

## 🔍 Key Observations

* We are NOT looking for the smallest unit ❌
* We need the **largest common repeating pattern** ✅

### 🔑 Important Property

If a valid divisor exists:

```
str1 + str2 == str2 + str1
```

👉 This ensures both strings are built from the same base pattern.

---

## 🧠 Core Idea

1. Check if:

   ```
   str1 + str2 == str2 + str1
   ```

   * If false → return `""`

2. If true:

   * Let `n = str1.length`, `m = str2.length`
   * Compute:

     ```
     len = gcd(n, m)
     ```
   * Answer = prefix of length `len`

---

## 🧪 Examples

### Example 1

```
Input:
str1 = "ABCABC"
str2 = "ABC"

Output:
"ABC"
```

### Example 2

```
Input:
str1 = "ABABAB"
str2 = "ABAB"

Output:
"AB"
```

### Example 3

```
Input:
str1 = "LEET"
str2 = "CODE"

Output:
""
```

---

## ⚡ Optimized Approach (GCD + String Trick)

### 💡 Idea

* Use string concatenation property to validate pattern
* Use GCD of lengths to find largest valid substring

---

### ⏱ Complexity

* Time: **O(n + m)**
* Space: **O(n + m)** (due to concatenation)

---

## 💻 Code

```cpp
class Solution {
public:
    string gcdOfStrings(string str1, string str2) {
        int n = str1.length();
        int m = str2.length();

        if (str1 + str2 == str2 + str1) {
            return str1.substr(0, gcd(n, m));
        }

        return "";
    }
};
```

---

## ⚠️ Edge Cases

* One string is empty
* No valid divisor exists
* Strings have different patterns

---

## ❗ Common Mistakes (Important)

* Thinking the answer is the **smallest unit** ❌
* Forgetting to check:

  ```
  str1 + str2 == str2 + str1
  ```
* Trying brute force unnecessarily

---

## 🧠 Patterns Learned

* Applying **GCD concept on string lengths**
* Using **string concatenation trick**
* Pattern recognition in strings

---

## 🔥 Mental Model (Revision Key)

> “If both strings share a base pattern → concatenations match → answer length = gcd → return prefix”

---

## 🚀 Takeaway

* Always check **structure first**, then apply math
* This is a classic example of:

  > **Math + String Pattern = Optimal Solution**
