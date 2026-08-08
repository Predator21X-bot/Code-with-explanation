# 🟦 LeetCode 1 — Two Sum

**Difficulty:** Easy
**Pattern:** Hash Map / Two Pointers
**Time Complexity:** `O(n)` with Hash Map
**Space Complexity:** `O(n)`

---

# 📌 Problem

Given an array of integers `nums` and an integer `target`, return the **indices of the two numbers** such that:

```cpp
nums[i] + nums[j] == target
```

Each input has exactly one solution, and you cannot use the same element twice.

---

# Example

```text
nums = [2,7,11,15]
target = 9
```

We need:

```text
2 + 7 = 9
```

So return:

```cpp
[0,1]
```

---

# 💡 Key Insight

For every number:

```cpp
nums[i]
```

we need to find:

```cpp
target - nums[i]
```

This is called the **complement**.

For example:

```text
target = 9
current = 2

complement = 9 - 2
           = 7
```

So the question becomes:

> **Have we already seen `7`?**

A Hash Map lets us answer this in `O(1)` average time.

---

# ⭐ Hash Map Approach

We store:

```text
value → index
```

Example:

```text
nums = [3,2,4]
target = 6
```

Process:

```text
3 → index 0
2 → index 1
4 → index 2
```

When we reach `4`:

```text
complement = 6 - 4
           = 2
```

`2` is already in the map at index `1`.

Therefore:

```cpp
return {1,2};
```

---

# Algorithm

### Step 1

Create a Hash Map:

```cpp
unordered_map<int,int> mp;
```

---

### Step 2

Traverse the array.

```cpp
for (int i = 0; i < nums.size(); i++)
```

---

### Step 3

Calculate the complement.

```cpp
int complement = target - nums[i];
```

---

### Step 4

Check whether the complement already exists.

```cpp
if (mp.count(complement))
```

If yes:

```cpp
return {mp[complement], i};
```

---

### Step 5

Otherwise store the current number and its index.

```cpp
mp[nums[i]] = i;
```

---

# C++ Solution — Hash Map

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

```text
nums = [3,2,4]
target = 6
```

### `i = 0`

```text
nums[i] = 3

complement = 6 - 3
           = 3
```

`3` isn't in the map.

Store:

```text
3 → 0
```

---

### `i = 1`

```text
nums[i] = 2

complement = 6 - 2
           = 4
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
nums[i] = 4

complement = 6 - 4
           = 2
```

`2` exists:

```text
2 → 1
```

Therefore:

```cpp
return {1,2};
```

---

# ⚠️ Why Your Two-Pointer Code Had a Problem

You wrote:

```cpp
sort(nums.begin(), nums.end());
```

Sorting itself is fine for the **two-pointer technique**, but the problem asks for the **original indices**.

Example:

```text
nums = [3,2,4]
```

Original indices:

```text
3 → 0
2 → 1
4 → 2
```

After sorting:

```text
2 → 0
3 → 1
4 → 2
```

Now if two pointers find:

```text
2 + 4 = 6
```

you would return:

```cpp
{0,2}
```

But that's wrong for the original array.

The correct indices are:

```cpp
{1,2}
```

because the original array was:

```text
index:  0  1  2
value:  3  2  4
             ↑  ↑
```

---

# Two-Pointer Approach

Two pointers **can still be used**, but we need to preserve the original index.

Store:

```cpp
(value, originalIndex)
```

For:

```text
nums = [3,2,4]
```

create:

```text
(3,0)
(2,1)
(4,2)
```

Sort:

```text
(2,1)
(3,0)
(4,2)
```

Now two pointers can operate on the sorted values while still having access to the original indices.

---

# C++ — Two Pointer Version

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {

        vector<pair<int, int>> arr;

        for (int i = 0; i < nums.size(); i++) {
            arr.push_back({nums[i], i});
        }

        sort(arr.begin(), arr.end());

        int i = 0;
        int j = arr.size() - 1;

        while (i < j) {

            int sum = arr[i].first + arr[j].first;

            if (sum == target) {
                return {arr[i].second, arr[j].second};
            }
            else if (sum < target) {
                i++;
            }
            else {
                j--;
            }
        }

        return {};
    }
};
```

---

# Hash Map vs Two Pointers

| Approach     |         Time |  Space | Original Indices         |
| ------------ | -----------: | -----: | ------------------------ |
| Hash Map     |       `O(n)` | `O(n)` | Naturally preserved      |
| Two Pointers | `O(n log n)` | `O(n)` | Must explicitly preserve |

For the **classic Two Sum**, the Hash Map solution is generally preferred.

---

# 🧠 Pattern Recognition

When you see:

> Find two numbers whose sum equals a target.

Think:

```text
Current number
      ↓
Calculate complement
      ↓
Have I seen complement?
      ↓
   Yes → return indices
   No  → store current number
```

The core formula:

```cpp
int complement = target - nums[i];
```

---

# Common Mistakes

### ❌ Sorting the original array

```cpp
sort(nums.begin(), nums.end());
```

and then returning the two-pointer positions.

This loses the original indices.

---

### ❌ Checking after inserting

You generally want:

```cpp
if (mp.count(complement))
    return {mp[complement], i};

mp[nums[i]] = i;
```

This ensures you don't accidentally use the same element twice.

---

### ❌ Using `nums[i]` as the lookup

The lookup should be the **complement**:

```cpp
mp.count(target - nums[i])
```

not simply:

```cpp
mp.count(nums[i])
```

---

# ⭐ Interview Cheat Sheet

Remember:

```cpp
unordered_map<int, int> mp;

for (int i = 0; i < nums.size(); i++) {

    int complement = target - nums[i];

    if (mp.count(complement))
        return {mp[complement], i};

    mp[nums[i]] = i;
}
```

### Mental Model

> **For every number, calculate what number I need to complete the target, then check whether I've already seen it.**

That's the entire Hash Map pattern behind Two Sum.
