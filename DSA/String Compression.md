# 🧩 String Compression

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/string-compression/](https://leetcode.com/problems/string-compression/)

---

## 🧠 Problem Understanding

Given a character array `chars`, compress it **in-place** such that:

* Consecutive repeating characters are replaced with:

```text
char + count
```

### ⚠️ Rules:

* Only **consecutive groups** matter
* If count = 1 → write only character
* Modify array in-place
* Return new length

---

## 🔍 Example

```text
Input:  ["a","a","b","b","c","c","c"]
Output: ["a","2","b","2","c","3"]
```

---

## 🧠 Core Idea

Process the array in **groups of same characters**

---

## ⚡ Approach: Two Pointers (Read + Write)

---

### 🧠 Pointers

| Pointer | Role                              |
| ------- | --------------------------------- |
| `i`     | start of group (read)             |
| `j`     | moves to end of group             |
| `k`     | write pointer (compressed result) |

---

## 🧠 Algorithm

1. Start from index `i`
2. Move `j` until character changes
3. Compute:

```cpp
count = j - i
```

4. Write:

   * character
   * count (if >1)
5. Move:

```cpp
i = j
```

---

## 💻 Code

```cpp id="compression-code"
class Solution {
public:
    int compress(vector<char>& chars) {
        int i = 0, k = 0;
        int n = chars.size();

        while (i < n) {
            int j = i;

            // find group
            while (j < n && chars[j] == chars[i]) {
                j++;
            }

            int count = j - i;

            // write character
            chars[k++] = chars[i];

            // write count if >1
            if (count > 1) {
                string s = to_string(count);
                for (char c : s) {
                    chars[k++] = c;
                }
            }

            i = j;
        }

        return k;
    }
};
```

---

## ⏱ Complexity

* Time: **O(n)**
* Space: **O(1)** (in-place)

---

## 🧠 Dry Run

Input:

```text
[a, a, a, b, b]
```

Steps:

```text
aaa → a3
bb  → b2
```

Final array:

```text
[a, 3, b, 2]
```

Return:

```cpp
k = 4
```

---

## ❗ Common Mistakes

### ❌ Writing count before character

```text
3a ❌
```

### ✅ Correct

```text
a3 ✅
```

---

### ❌ Not resetting `j = i`

---

### ❌ Forgetting multi-digit count

```text
12 → must write '1','2'
```

---

### ❌ Thinking it’s O(n²)

👉 It’s **O(n)** because:

* `j` moves forward only once

---

## 🧠 Patterns Learned

* Two-pointer technique
* In-place array modification
* Group processing

---

## 🏷 Tags

* Arrays
* Two Pointers
* String

---

## 🧠 When to Use This

* When working with:

  * consecutive groups
  * compression problems
  * in-place modifications

---

## 🔥 Mental Model (Revision Key)

> “Read group → write char → write count → move ahead”

---

## ⚡ Key Insight

* Separate **read pointer** and **write pointer**
* Don’t delete — just overwrite

---

## 🚀 Takeaway

* Always think:

  > “Can I process data in groups?”
* Control pointer movement carefully
