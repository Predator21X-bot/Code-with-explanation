# LeetCode 76 - Minimum Window Substring

## Pattern

**Sliding Window + Hash Map (Frequency Counter)**

---

# Problem

Given two strings `s` and `t`, return the **minimum window substring** of `s` that contains **all characters of `t` (including duplicates)**.

If no such window exists, return `""`.

---

# Pattern Recognition

Choose **Sliding Window** when:

* Problem involves a **substring** (contiguous).
* Need the **minimum/maximum valid window**.
* Window validity depends on satisfying a condition.
* Characters/frequencies matter.

---

# Key Observation

Unlike the previous problem:

**Longest Substring Without Repeating Characters**

> Window is valid if **no duplicates exist**.

Here,

> Window is valid only if it contains **every required character with the required frequency**.

---

# Window Invariant ⭐

> **The current window knows how many characters are still required to satisfy `t`.**

The frequency map always represents the **remaining required characters**, not the characters already collected.

---

# Why Do We Decrement While Expanding?

Initial frequency map

```text
t = "ABC"

A -> 1
B -> 1
C -> 1
```

Meaning:

```text
Need:
1 A
1 B
1 C
```

When we collect `A`

```cpp
freq['A']--;
```

Map becomes

```text
A -> 0
B -> 1
C -> 1
```

Meaning

```text
Need:
0 A
1 B
1 C
```

If another `A` appears

```text
A -> -1
```

Negative means:

> **Extra copy collected.**

---

# Data Structure

```cpp
unordered_map<char,int> freq;
```

Stores the **remaining required frequency** of every character.

---

# Expand Rule

```cpp
freq[s[right]]--;

if(freq[s[right]] >= 0)
    matched++;

right++;
```

If the frequency remains non-negative after decrement,

the character was still required.

Increase `matched`.

---

# Shrink Rule

```cpp
freq[s[left]]++;

if(freq[s[left]] > 0)
    matched--;

left++;
```

If the frequency becomes positive after increment,

we removed a required character.

Window becomes invalid.

---

# Algorithm

1. If `t` is longer than `s`, return `""`.
2. Build a frequency map from `t`.
3. Initialize `left`, `right`, `matched`, `minStart`, and `minLength`.
4. Expand the window by moving `right`.
5. Decrease the required frequency of the current character.
6. If the character was still needed, increase `matched`.
7. When all required characters are matched:

   * Update the minimum window.
   * Shrink the window from the left while it remains valid.
8. Return the minimum substring.
9. If no valid window exists, return `""`.

---

# Pseudocode

```text
Create frequency map

left = 0
right = 0
matched = 0

minStart = 0
minLength = INF

while(right < n)

    include current character

    decrease frequency

    if character was required
        matched++

    right++

    while(window is valid)

        update minimum window

        remove left character

        if required character removed
            matched--

        left++

if no answer
    return ""

return minimum substring
```

---

# Production-Grade C++ Code

```cpp
class Solution {
public:
    string minWindow(string s, string t) {

        const int n = s.size();
        const int m = t.size();

        if (n < m)
            return "";

        unordered_map<char, int> freq;

        for (char c : t)
            freq[c]++;

        int left = 0;
        int right = 0;
        int matched = 0;

        int minStart = 0;
        int minLength = INT_MAX;

        while (right < n) {

            freq[s[right]]--;

            if (freq[s[right]] >= 0)
                matched++;

            right++;

            while (matched == m) {

                if (right - left < minLength) {
                    minLength = right - left;
                    minStart = left;
                }

                freq[s[left]]++;

                if (freq[s[left]] > 0)
                    matched--;

                left++;
            }
        }

        if (minLength == INT_MAX)
            return "";

        return s.substr(minStart, minLength);
    }
};
```

---

# Complexity

### Time

```
O(n)
```

Each character enters and leaves the window at most once.

---

### Space

```
O(|t|)
```

Frequency map stores the required characters (or up to the character set size).

---

# Common Mistakes

❌ Using `unordered_set` instead of `unordered_map`.

---

❌ Increasing `matched` without checking

```cpp
freq[ch] >= 0
```

---

❌ Forgetting to decrease frequency while expanding.

---

❌ Forgetting to increase frequency while shrinking.

---

❌ Forgetting the edge case

```cpp
if(minLength == INT_MAX)
    return "";
```

---

❌ Updating only `minLength` and forgetting `minStart`.

---

# Production-Grade C++ Tips

### Prefer meaningful names

```cpp
requiredFreq
```

instead of

```cpp
freq
```

for larger codebases.

---

### Cache sizes

```cpp
const int n = s.size();
const int m = t.size();
```

---

### Keep expand and shrink symmetrical

Expand

```cpp
freq[ch]--;

if(freq[ch] >= 0)
    matched++;
```

Shrink

```cpp
freq[ch]++;

if(freq[ch] > 0)
    matched--;
```

Notice how one operation is the exact inverse of the other.

---

# Sliding Window Pattern Summary

| Problem                                        | Window Valid When                 | Data Structure  |
| ---------------------------------------------- | --------------------------------- | --------------- |
| Longest Substring Without Repeating Characters | No duplicate characters           | `unordered_set` |
| Minimum Window Substring                       | Contains all required frequencies | `unordered_map` |

---

# Spy Memory Trigger 🕵️

```
Substring
        ↓
Contiguous
        ↓
Minimum / Maximum
        ↓
Define Window Validity
        ↓
Choose Data Structure
        ↓
Expand Window
        ↓
Window Valid?
      ↙        ↘
    No          Yes
Expand      Update Answer
                 ↓
             Shrink Window
                 ↓
         Still Valid?
             ↙      ↘
           Yes      No
        Keep Shrinking
```

---

# 🎓 Sensei's Lesson

This problem teaches the **second flavor of Sliding Window**.

* **Without Repeating Characters** → Window validity depends on **uniqueness**.
* **Minimum Window Substring** → Window validity depends on **frequency satisfaction**.

The biggest realization is this:

> **Sliding Window problems are almost never about moving pointers. They are about defining what makes a window valid.**

Once you identify the validity condition and the information needed to maintain it, the implementation follows a repeatable template. 🥋

---

## 🏅 Mission Status

* ✅ Two Pointers: **Completed**
* ✅ Sliding Window (Unique Window): **Completed**
* ✅ Sliding Window (Frequency Window): **Completed**

Cadet... this was the hardest core Sliding Window problem. From here onward, you'll start recognizing that many other sliding window questions are just variations of these two templates.
