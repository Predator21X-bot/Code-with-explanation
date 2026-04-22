# 🧩 Maximum Number of Vowels in a Substring of Given Length

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)

---

# 🧠 Problem Understanding

Given:

* String `s`
* Integer `k`

👉 Goal:

> Find maximum number of **vowels** in any substring of size `k`

---

## 🔍 Example

```text
s = "abciiidef", k = 3  
Output = 3   ("iii")
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Identify pattern

👉 Substring of fixed size `k`
👉 Need max value over all such windows

---

## 🧠 Conclusion

> This is a **Fixed Sliding Window problem**

---

## 🧩 Step 2: What are we tracking?

👉 Instead of sum:

```text
count of vowels
```

---

## 🧩 Step 3: Vowel check

```cpp
bool isVowel(char c) {
    return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
}
```

---

# ⚡ Approach: Fixed Sliding Window

---

## 🧠 Core idea

> “Remove left, add right”

---

## 🧠 Window movement

```text
old window: [i ... i+k-1]  
new window: [i+1 ... i+k]
```

---

## 🧠 Update logic

```cpp
if (isVowel(s[i - k])) count--;
if (isVowel(s[i])) count++;
```

---

# 💻 Code

```cpp
class Solution {
public:
    bool isVowel(char c) {
        return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
    }

    int maxVowels(string s, int k) {
        int count = 0;
        int n = s.length();

        // first window
        for (int i = 0; i < k; i++) {
            if (isVowel(s[i])) count++;
        }

        int maxCount = count;

        // sliding window
        for (int i = k; i < n; i++) {
            if (isVowel(s[i - k])) count--;
            if (isVowel(s[i])) count++;

            maxCount = max(maxCount, count);
        }

        return maxCount;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)**

---

# ❗ Common Mistakes

* ❌ Returning `count` instead of `maxCount`
* ❌ Forgetting to initialize first window
* ❌ Wrong index (`i-k`)
* ❌ Not updating max inside loop

---

# 🧠 Patterns Learned

* Fixed sliding window
* Counting condition-based elements
* Reusing previous computation

---

# 🏷 Tags

* String
* Sliding Window

---

# 🔥 Mental Model (REVISION KEY)

> “Maintain count over window → update incrementally”

---

# ⚡ Sliding Window Types (BIG PICTURE)

---

## 🔹 Fixed Window (YOU JUST MASTERED)

```text
window size = k (constant)
```

### Examples:

* max average
* max vowels
* max sum

---

## 🔹 Variable Window (NEXT LEVEL 🔥)

```text
window expands/shrinks dynamically
```

### Examples:

* longest substring without repeating
* smallest subarray ≥ target

---

# 🧠 Template (Fixed Window)

```text
1. build first window
2. for each step:
   remove left
   add right
   update answer
```

---

# 🚀 Key Insight

> Sliding window = optimized brute force using overlap

---

# 🧠 Pattern Recognition

Use sliding window when:

* subarray / substring
* contiguous
* fixed or dynamic size
* need max/min/count

---

# 🚀 Takeaway

* Don’t recompute → reuse
* Track only what changes
* Always think:

  > what leaves, what enters
