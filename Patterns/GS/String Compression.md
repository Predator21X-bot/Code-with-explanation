# 🟦 LeetCode 443 — String Compression

**Difficulty:** Medium
**Pattern:** Two Pointers + In-Place Array Manipulation
**Time Complexity:** `O(n)`
**Space Complexity:** `O(1)`

The problem asks us to compress **consecutive groups** in-place. For example, `["a","a","b","b","c","c","c"]` becomes `["a","2","b","2","c","3"]`, and counts of 10 or more are written as multiple digits. ([LeetCode][1])

---

# 📌 Problem

Given:

```text
chars = ["a","a","b","b","c","c","c"]
```

Compress consecutive repeating characters.

Output:

```text
["a","2","b","2","c","3"]
```

Return the **new length**:

```text
6
```

The compression must happen **in-place** and use `O(1)` extra space. ([LeetCode][1])

---

# 💡 Core Idea

We need to do two things:

1. **Read** the original array and identify groups.
2. **Write** the compressed result back into the same array.

This naturally gives us **two pointers**:

```text
read pointer  → finds the next group
write pointer → writes compressed result
```

Think:

```text
READ
 ↓
[a a a b b c c c]
                ↓
WRITE
 ↓
[a 3 b 2 c 3]
```

This is an **in-place two-pointer** technique. ([NeetCode][2])

---

# ⭐ Two Pointers

We'll use:

```cpp
int i = 0;   // start of current group
int k = 0;   // write position
```

For every group, we need to find where it ends.

Use another pointer:

```cpp
int j = i;
```

and move `j` while characters are the same.

```cpp
while (j < n && chars[j] == chars[i]) {
    j++;
}
```

After this:

```text
group = chars[i ... j-1]
```

and its count is:

```cpp
int count = j - i;
```

---

# 🧠 Example

Suppose:

```text
chars = [a,a,a,b,b,c]
```

Initially:

```text
i = 0
```

`chars[i] = 'a'`.

Move `j`:

```text
j = 0 → a
j = 1 → a
j = 2 → a
j = 3 → b ❌
```

So:

```text
i = 0
j = 3
```

Count:

```cpp
count = j - i;
       = 3 - 0
       = 3;
```

Therefore the group is:

```text
aaa
```

and we write:

```text
a3
```

---

# Step 1 — Write the Character

For every group, first write the character:

```cpp
chars[k++] = chars[i];
```

For:

```text
aaa
```

we write:

```text
a
```

---

# Step 2 — Write the Count

Only write the count if:

```cpp
count > 1
```

Because:

```text
a
```

should remain:

```text
a
```

not:

```text
a1
```

The problem explicitly specifies that a group of length 1 is represented by only the character. ([LeetCode][1])

---

# 🚨 Important: Count Can Have Multiple Digits

Suppose:

```text
aaaaaaaaaaaa
```

12 `a`s.

We need:

```text
a12
```

not:

```text
a12
```

as one character — obviously `1` and `2` are separate characters in the array.

So:

```text
a12
 ↓
'a', '1', '2'
```

The easiest way in C++ is:

```cpp
string countStr = to_string(count);
```

Then:

```cpp
for (char c : countStr) {
    chars[k++] = c;
}
```

This handles:

```text
2
10
12
100
...
```

automatically. The problem specifically requires counts of 10 or greater to be split into individual characters. ([LeetCode][1])

---

# Step 3 — Move to Next Group

After processing:

```text
chars[i ... j-1]
```

we need to start from:

```cpp
i = j;
```

Because `j` is the first position belonging to the **next group**.

Example:

```text
[a,a,a,b,b,c]

i = 0
j = 3
```

We've processed:

```text
[a,a,a]
```

So:

```cpp
i = 3;
```

Now:

```text
chars[3] = b
```

and we process the next group.

---

# 🧪 Complete Dry Run

Input:

```text
[a,a,b,b,c,c,c]
```

### Group 1

```text
i = 0
```

Find group:

```text
[a,a]
```

Count:

```text
2
```

Write:

```text
chars[0] = 'a'
chars[1] = '2'
```

Now:

```text
[a,2,b,b,c,c,c]
```

---

### Group 2

```text
i = 2
```

Group:

```text
[b,b]
```

Count:

```text
2
```

Write starting at `k = 2`:

```text
[a,2,b,2,c,c,c]
```

---

### Group 3

```text
i = 4
```

Group:

```text
[c,c,c]
```

Count:

```text
3
```

Write:

```text
[a,2,b,2,c,3,c]
```

Final relevant portion:

```text
[a,2,b,2,c,3]
```

Returned length:

```text
6
```

Everything after index `5` is irrelevant, as specified by the problem. ([LeetCode][1])

---

# ✅ C++ Solution

```cpp
class Solution {
public:
    int compress(vector<char>& chars) {

        int n = chars.size();

        int i = 0;  // read/group pointer
        int k = 0;  // write pointer

        while (i < n) {

            // Find the end of the current group
            int j = i;

            while (j < n && chars[j] == chars[i]) {
                j++;
            }

            // Count of current group
            int count = j - i;

            // Write the character
            chars[k++] = chars[i];

            // Write count only if > 1
            if (count > 1) {

                string countStr = to_string(count);

                for (char c : countStr) {
                    chars[k++] = c;
                }
            }

            // Move to next group
            i = j;
        }

        return k;
    }
};
```

---

# 🔥 The Most Important Part

There are **three different indices/ideas** here:

```text
i → where the current group starts

j → where the current group ends

k → where we write the compressed result
```

For:

```text
[a,a,a,b,b]
```

we have:

```text
i = 0
j = 3
k = 0
```

because:

```text
[a,a,a] [b,b]
 ↑     ↑
 i     j
```

We write:

```text
a3
```

at:

```text
chars[k]
```

Then:

```text
k = 2
i = 3
```

and process `bb`.

---

# 🧠 Why Can We Overwrite the Array?

This is the clever part.

The compressed representation can never be longer than the original array.

For example:

```text
aaa → a3
```

3 characters → 2 characters.

```text
aaaaaaaaaaaa → a12
```

12 characters → 3 characters.

And:

```text
a → a
```

1 → 1.

Therefore, our `k` write pointer never needs to move beyond the unread data in a problematic way. This makes the in-place approach safe. ([NeetCode][2])

---

# ⚠️ Common Mistake — Writing `count` Directly

Don't do:

```cpp
chars[k++] = count;
```

because `chars` stores `char`.

For example:

```text
count = 12
```

We need:

```text
'1'
'2'
```

not the numeric value `12`.

That's why:

```cpp
string countStr = to_string(count);
```

and then:

```cpp
for (char c : countStr)
    chars[k++] = c;
```

---

# ⚠️ Common Mistake — Writing `1`

For:

```text
[a]
```

the result should be:

```text
[a]
```

NOT:

```text
[a,1]
```

Therefore:

```cpp
if (count > 1)
```

is important.

---

# 🧠 Pattern Recognition

When you see:

> Modify an array in-place while reading its original contents.

Think:

```text
READ POINTER
     ↓
Understand/process original data
     ↓
WRITE POINTER
     ↓
Overwrite array in-place
```

This is a useful **read/write two-pointer pattern**.

---

# Complexity

Let:

```text
n = chars.size()
```

Each character is processed a constant number of times.

### Time

```text
O(n)
```

### Extra Space

We only use a few variables.

```text
O(1)
```

Strictly speaking, `to_string(count)` creates a tiny temporary string whose length is at most a few digits under the given constraints; the intended algorithm is still considered constant auxiliary space for this problem. The core in-place algorithm uses constant extra workspace. ([LeetCode][1])

---

# 🔑 Interview Cheat Sheet

### Identify a group

```cpp
int j = i;

while (j < n && chars[j] == chars[i])
    j++;
```

### Get frequency

```cpp
int count = j - i;
```

### Write character

```cpp
chars[k++] = chars[i];
```

### Write count only when needed

```cpp
if (count > 1) {
    string s = to_string(count);

    for (char c : s)
        chars[k++] = c;
}
```

### Move to next group

```cpp
i = j;
```

### Return compressed length

```cpp
return k;
```

---

# 🎯 Mental Model

```text
        FIND GROUP
             ↓
     ┌───────────────┐
     │ aaa           │
     └───────────────┘
             ↓
        count = 3
             ↓
          WRITE
             ↓
           a3
             ↓
        i = next group
```

The simplest way to remember the problem:

> **`i` reads groups, `j` finds the group's size, and `k` writes the compressed result.**

This is the main **in-place two-pointer pattern** to take away from LeetCode 443. ([NeetCode][2])

[1]: https://leetcode.com/problems/string-compression/submissions/1341080133/?utm_source=chatgpt.com "String Compression - LeetCode"
[2]: https://neetcode.io/solutions/string-compression?utm_source=chatgpt.com "LeetCode 443 String Compression Solution & Explanation | NeetCode"
