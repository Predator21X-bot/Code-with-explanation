# LeetCode 3 - Longest Substring Without Repeating Characters

## Pattern

**Sliding Window + Hash Set**

---

## Problem

Given a string `s`, return the length of the **longest substring** without repeating characters.

---

## Pattern Recognition

Choose **Sliding Window** when:

* The problem involves a **contiguous substring/subarray**.
* We need the **longest** or **shortest** valid window.
* A window can become **invalid** and later **valid** by moving pointers.

---

## Key Observation

A valid window contains **only unique characters**.

Whenever a duplicate character enters the window, it becomes invalid and must be shrunk until the duplicate is removed.

---

## Window Invariant

> **The current window always contains unique characters.**

Every iteration maintains this invariant.

---

## Data Structure

```cpp
unordered_set<char> window;
```

Used for O(1) lookup.

Operations:

* `insert()`
* `erase()`
* `count()`

---

## Algorithm

1. Initialize `left`, `right`, and `maxLength`.
2. Maintain an `unordered_set` representing the current window.
3. Expand the window by moving `right`.
4. If `s[right]` is not present:

   * Insert it.
   * Update `maxLength`.
   * Move `right`.
5. Otherwise:

   * Remove characters from the left until `s[right]` is no longer present.
6. Continue until `right` reaches the end.
7. Return `maxLength`.

---

## Pseudocode

```text
left = 0
right = 0
maxLength = 0

declare unordered_set

while(right < n)

    if(current character not present)
        insert character
        update answer
        right++

    else
        while(character already exists)
            erase left character
            left++

return maxLength
```

---

## C++ Solution

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {

        int left = 0;
        int right = 0;
        int maxLength = 0;

        unordered_set<char> window;

        while (right < s.size()) {

            if (!window.count(s[right])) {

                window.insert(s[right]);
                maxLength = max(maxLength, right - left + 1);
                right++;

            } else {

                while (window.count(s[right])) {
                    window.erase(s[left]);
                    left++;
                }
            }
        }

        return maxLength;
    }
};
```

---

## Complexity

**Time:** `O(n)`

* Each character enters and leaves the window at most once.

**Space:** `O(min(n, 256))`

* Stores only unique characters in the current window.

---

# Sliding Window Recognition Checklist

* ✅ Contiguous substring/subarray
* ✅ Longest/Shortest valid window
* ✅ Window validity condition exists
* ✅ Expand when valid
* ✅ Shrink until valid
* ✅ Maintain window information using HashSet/HashMap

---

# Common Mistakes

❌ Shrinking based on `s[left]` instead of checking whether `s[right]` still exists.

```cpp
while(window.count(s[right])) {
    window.erase(s[left]);
    left++;
}
```

---

❌ Forgetting to update the answer **only after** a valid expansion.

---

❌ Using a `for` loop for `right` when the window controls movement.

A `while` loop better represents the expand/shrink process.

---

# Production-Grade C++ Tips

```cpp
// Prefer descriptive names
maxLength
```

instead of

```cpp
maxLen
```

---

```cpp
// Cleaner lookup
window.count(ch)
```

instead of

```cpp
window.find(ch) == window.end()
```

---

Declare variables separately for readability.

```cpp
int left = 0;
int right = 0;
int maxLength = 0;
```

---

# Pattern Memory Trigger 🧠

```
Substring/Subarray
        ↓
Contiguous
        ↓
Need Longest/Shortest
        ↓
Can Window Become Invalid?
        ↓
YES
        ↓
Sliding Window
        ↓
What Information Must I Maintain?
        ↓
HashSet / HashMap
        ↓
Expand → Invalid? → Shrink → Expand
```

---

## 🎯 Takeaway

The hardest part of Sliding Window is **not moving pointers**—it's identifying **what makes the window invalid** and **what information must be maintained**. Once those two questions are answered, the implementation follows a repeatable template.
