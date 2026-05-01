# 🧩 Unique Number of Occurrences

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/unique-number-of-occurrences/](https://leetcode.com/problems/unique-number-of-occurrences/)

---

# 🧠 Problem Understanding

Given:

* Array `arr`

👉 Goal:

```text
Check if frequency of each number is UNIQUE
```

---

## 🔍 Example

```text
arr = [1,2,2,1,1,3]
freq = {1:3, 2:2, 3:1}
→ all counts unique → true
```

```text
arr = [1,2]
freq = {1:1, 2:1}
→ duplicate count → false
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Count frequency

👉 Use hashmap:

```cpp
unordered_map<int,int> freq;
```

---

## 🧩 Step 2: Store counts

```cpp
for (int num : arr) {
    freq[num]++;
}
```

---

## 🧩 Step 3: Check uniqueness

👉 Use set:

```cpp
unordered_set<int> seen;
```

---

## 🧩 Step 4: Detect duplicates

```cpp
if (seen.find(count) != seen.end())
    return false;
```

---

# ⚡ Approach: Hashing (Map + Set)

---

## 💻 Code

```cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int, int> freq;
        unordered_set<int> seen;

        for (int num : arr) {
            freq[num]++;
        }

        for (auto &p : freq) {
            int count = p.second;

            if (seen.find(count) != seen.end()) {
                return false;
            }

            seen.insert(count);
        }

        return true;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# ❗ Common Mistakes

* ❌ Using array instead of map
* ❌ Forgetting to check duplicates before insert
* ❌ Using map instead of set for uniqueness
* ❌ Comparing keys instead of values

---

# 🧠 Patterns Learned

* Frequency counting
* Duplicate detection
* Map + Set combination

---

# 🏷 Tags

* Array
* Hashing
* Map
* Set

---

# 🔥 Mental Model (REVISION KEY)

> “Map counts → Set ensures uniqueness”

---

# ⚡ Set vs Map (IMPORTANT)

| Structure       | Purpose          |
| --------------- | ---------------- |
| `unordered_map` | count frequency  |
| `unordered_set` | check duplicates |

---

# 🧠 Template (Reusable)

```text
1. count frequencies using map
2. for each frequency:
       if already in set → return false
       else insert
3. return true
```
