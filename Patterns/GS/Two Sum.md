Absolutely ❤️ We'll come back to **Median of Two Sorted Arrays** tomorrow.

# 🟦 LeetCode 1 — Two Sum

**Difficulty:** Easy
**Pattern:** Hash Map
**Time Complexity:** `O(n)`
**Space Complexity:** `O(n)`

---

## 📌 Problem

Given an array `nums` and an integer `target`, return the **indices of two numbers** whose sum equals `target`.

```cpp
nums[i] + nums[j] == target
```

You cannot use the same element twice.

### Example

```text
nums = [2,7,11,15]
target = 9
```

```text
2 + 7 = 9
```

Therefore:

```cpp
[0,1]
```

---

# 💡 Core Idea — Complement

For every number:

```cpp
nums[i]
```

ask:

> What number do I need to reach `target`?

That number is the **complement**:

```cpp
int complement = target - nums[i];
```

Example:

```text
target = 9
current = 2

complement = 9 - 2
           = 7
```

So we need to check:

> Have we already seen `7`?

---

# ⭐ Hash Map

Use:

```cpp
unordered_map<int, int> mp;
```

Store:

```text
value → index
```

For example:

```text
3 → 0
2 → 1
```

Then when we encounter `4`:

```text
target = 6

complement = 6 - 4
           = 2
```

`2` is already in the map at index `1`.

Therefore:

```cpp
return {1, 2};
```

---

# Algorithm

### 1. Create Hash Map

```cpp
unordered_map<int, int> mp;
```

---

### 2. Traverse the array

```cpp
for (int i = 0; i < nums.size(); i++)
```

---

### 3. Calculate complement

```cpp
int complement = target - nums[i];
```

---

### 4. Check whether complement exists

```cpp
if (mp.count(complement))
```

If yes:

```cpp
return {mp[complement], i};
```

---

### 5. Store current number

If the complement isn't found:

```cpp
mp[nums[i]] = i;
```

---

# ✅ Complete Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {

        unordered_map<int, int> mp;

        for (int i = 0; i < nums.size(); i++) {

            int complement = target - nums[i];

            if (mp.count(complement)) {
                return {mp[complement], i};
            }

            mp[nums[i]] = i;
        }

        return {};
    }
};
```

---

# 🧪 Dry Run

Input:

```text
nums = [3,2,4]
target = 6
```

### `i = 0`

```text
current = 3
complement = 6 - 3 = 3
```

`3` isn't in the map.

Store:

```text
3 → 0
```

---

### `i = 1`

```text
current = 2
complement = 6 - 2 = 4
```

`4` isn't in the map.

Store:

```text
3 → 0
2 → 1
```

---

### `i = 2`

```text
current = 4
complement = 6 - 4 = 2
```

`2` **is** in the map:

```text
2 → 1
```

Therefore:

```cpp
return {1, 2};
```

---

# ⚠️ Why We Check Before Inserting

The order is:

```cpp
if (mp.count(complement))
    return {mp[complement], i};

mp[nums[i]] = i;
```

not:

```cpp
mp[nums[i]] = i;

if (mp.count(complement))
```

Checking first ensures we don't accidentally use the **same element twice**.

---

# ❌ Why Sorting + Two Pointers Isn't the Best Approach Here

You tried:

```cpp
sort(nums.begin(), nums.end());
```

The two-pointer logic itself works for finding the values, but sorting changes their positions.

Example:

```text
Original:

nums = [3,2,4]

index:  0 1 2
value:  3 2 4
```

After sorting:

```text
[2,3,4]
```

Two pointers find:

```text
2 + 4 = 6
```

but their sorted positions are:

```text
0,2
```

while their **original indices** were:

```text
1,2
```

The problem asks for original indices.

Therefore, Hash Map is the cleaner solution.

---

# 🧠 Pattern Recognition

Whenever you see:

> Find two elements satisfying a target relationship.

Think:

```text
Current element
      ↓
Calculate what is needed
      ↓
Check Hash Map
      ↓
Found?
  ↓       ↓
 YES      NO
  ↓        ↓
return    store
```

For Two Sum specifically:

```cpp
int complement = target - nums[i];
```

---

# 🔑 Interview Cheat Sheet

Memorize this pattern:

```cpp
unordered_map<int, int> mp;

for (int i = 0; i < nums.size(); i++) {

    int complement = target - nums[i];

    if (mp.count(complement)) {
        return {mp[complement], i};
    }

    mp[nums[i]] = i;
}
```

### Mental Model

> **For every number, calculate the number needed to reach the target, and check whether we've already seen it.**

That's the core **Hash Map complement pattern** you'll see repeatedly in DSA.
