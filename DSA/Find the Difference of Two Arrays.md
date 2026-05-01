# 🧩 Find the Difference of Two Arrays

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/find-the-difference-of-two-arrays/](https://leetcode.com/problems/find-the-difference-of-two-arrays/)

---

# 🧠 Problem Understanding

Given:

* Two arrays `nums1` and `nums2`

👉 Find:

```text
1. Elements in nums1 but NOT in nums2  
2. Elements in nums2 but NOT in nums1
```

👉 Output:

```text
[ list1, list2 ]
```

---

## 🔍 Example

```text
nums1 = [1,2,3]
nums2 = [2,4,6]

Output = [[1,3], [4,6]]
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: What do we need?

👉 Fast lookup:

```text
“Does element exist?”
```

---

## 🧠 Best data structure

```text
unordered_set
```

---

## 🧩 Step 2: Why set?

* Removes duplicates automatically
* O(1) lookup
* Cleaner than map

---

## 🧩 Step 3: Convert arrays to sets

```cpp
unordered_set<int> set1(nums1.begin(), nums1.end());
unordered_set<int> set2(nums2.begin(), nums2.end());
```

---

## 🧩 Step 4: Compare sets

👉 Elements in `set1` but not in `set2`:

```cpp
if (set2.find(x) == set2.end())
```

👉 Elements in `set2` but not in `set1`:

```cpp
if (set1.find(x) == set1.end())
```

---

# ⚡ Approach: Hashing (Set-based)

---

## 💻 Code

```cpp
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1(nums1.begin(), nums1.end());
        unordered_set<int> set2(nums2.begin(), nums2.end());

        vector<vector<int>> answer(2);

        for (int x : set1) {
            if (set2.find(x) == set2.end()) {
                answer[0].push_back(x);
            }
        }

        for (int x : set2) {
            if (set1.find(x) == set1.end()) {
                answer[1].push_back(x);
            }
        }

        return answer;
    }
};
```

---

# ⏱ Complexity

* Time: **O(n + m)**
* Space: **O(n + m)**

---

# ❗ Common Mistakes

* ❌ Using array indexing on set (`set[i]`)
* ❌ Forgetting to insert elements into set
* ❌ Using map when only existence is needed
* ❌ Wrong comparison (checking same set)
* ❌ Not initializing `vector<vector<int>>(2)`

---

# 🧠 Patterns Learned

* Hashing
* Set-based lookup
* Duplicate removal

---

# 🏷 Tags

* Array
* Hashing
* Set

---

# 🔥 Mental Model (REVISION KEY)

> “Convert to sets → compare existence”

---

# ⚡ Set vs Map (IMPORTANT)

| Structure       | Use                 |
| --------------- | ------------------- |
| `unordered_set` | existence           |
| `unordered_map` | frequency / mapping |

---

# 🧠 Template (Reusable)

```text
1. convert arrays to sets
2. for each element in set1:
       if not in set2 → add
3. for each element in set2:
       if not in set1 → add
```

---

# 🚀 Takeaway

* Always simplify:

  > “Do I need value or just presence?”
* If presence → use set
