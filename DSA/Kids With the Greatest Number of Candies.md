# 🧩 Kids With the Greatest Number of Candies

### 🔗 Problem Link

LeetCode: [https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies/)

---

## 🧠 Problem Understanding

We are given:

* An array `candies[]` where each element represents candies a kid has
* An integer `extraCandies`

For each kid:

* Check if giving all `extraCandies` makes them have **maximum candies among all kids**

Return:

* A boolean array where:

  * `true` → can become greatest
  * `false` → cannot

---

## 🔍 Key Observations

* We don’t need to modify the array ❌
* We only **compare hypothetically** ✅
* The important value is:

  > **maximum element of the array**

---

## 🧠 Core Idea

1. Find `maxCandies` from array
2. For each kid:

   * Check:

     ```
     candies[i] + extraCandies >= maxCandies
     ```
3. Store result in boolean array

---

## 🧪 Examples

### Example 1

```cpp id="k0k1qa"
Input:
candies = [2,3,5,1,3], extraCandies = 3

Output:
[true,true,true,false,true]
```

---

## 🐢 Brute Force Idea

* For each kid:

  * Add extra candies
  * Compare with all other elements

### ⏱ Complexity

* Time: O(n²)
* Space: O(1)

---

## ⚡ Optimized Approach

### 💡 Idea

* Precompute maximum once
* Compare each element with it

---

## 💻 Code

```cpp id="v7b6lw"
class Solution {
public:
    vector<bool> kidsWithCandies(vector<int>& candies, int extraCandies) {
        int n = candies.size();
        vector<bool> result(n);

        int maxCandies = *(max_element(candies.begin(), candies.end()));

        for (int i = 0; i < n; i++) {
            result[i] = (candies[i] + extraCandies >= maxCandies);
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

## ⚠️ Edge Cases

* All elements equal
* Only one kid
* Large `extraCandies`
* Empty array (edge but usually not in constraints)

---

## ❗ Common Mistakes (Important)

* Using `max_element` incorrectly
  ❌ `int max = max_element(...)`
  ✅ `int max = *(max_element(...))`

* Confusing iterator vs value

* Using `push_back` with pre-sized vector

* Forgetting `>=` (equality matters)

---

## 🧠 Patterns Learned

* Precompute max/min for optimization
* Avoid repeated scanning
* Boolean condition simplification

---

## 🏷 Tags

* Arrays
* Simulation
* Greedy (comparison-based)

---

## 🧠 When to Use This

* When checking condition against **global max/min**
* When repeated comparisons can be optimized
* When per-element decision depends on global property

---

## 🔥 Mental Model (Revision Key)

> “Find max → compare each → build result”

---

## 🚀 Takeaway

* Always think:

  > “Can I precompute something to avoid repeated work?”
