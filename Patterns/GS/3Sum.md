# 🟦 LeetCode 15 — 3Sum

**Difficulty:** Medium
**Pattern:** Sorting + Two Pointers
**Time Complexity:** `O(n²)`
**Space Complexity:** `O(1)` auxiliary space, excluding the output.

The core idea is to **sort the array, fix one number, and solve the remaining two-sum problem with two pointers**. Sorting also makes duplicate handling possible. ([LeetCode][1])

---

## 📌 Problem

Given an integer array, return **all unique triplets**:

```cpp
nums[i] + nums[j] + nums[k] == 0
```

No duplicate triplets should be returned. ([LeetCode][1])

### Example

```text
nums = [-1,0,1,2,-1,-4]
```

Output:

```text
[
    [-1,-1,2],
    [-1,0,1]
]
```

---

# 💡 Core Idea

Think of 3Sum as:

> **Fix one number → solve Two Sum for the remaining part.**

Suppose:

```text
a + b + c = 0
```

If we fix:

```text
a = nums[i]
```

then we need:

```text
b + c = -a
```

So we have reduced the problem to **Two Sum**.

But instead of using a Hash Map, we'll use **sorting + two pointers**. ([LeetCode][1])

---

# Step 1 — Sort the Array

```cpp
sort(nums.begin(), nums.end());
```

Example:

```text
Before:

[-1,0,1,2,-1,-4]

After:

[-4,-1,-1,0,1,2]
```

Sorting is extremely important because it gives us two advantages:

### 1. We can skip duplicates

```text
[-4,-1,-1,0,1,2]
    ↑
  duplicate
```

### 2. Two-pointer movement becomes predictable

If the sum is too small:

```text
sum < 0
```

move `left` right.

If the sum is too large:

```text
sum > 0
```

move `right` left. ([NeetCode][2])

---

# Step 2 — Fix `i`

We'll iterate:

```cpp
for (int i = 0; i < nums.size() - 2; i++)
```

For every `i`, treat:

```cpp
nums[i]
```

as the first number.

Then:

```cpp
int left = i + 1;
int right = nums.size() - 1;
```

So:

```text
             i       left             right
             ↓        ↓                 ↓
[-4, -1, -1, 0, 1, 2]
```

---

# Step 3 — Calculate the Sum

```cpp
int sum = nums[i] + nums[left] + nums[right];
```

Now there are three possibilities.

---

## Case 1 — `sum < 0`

Example:

```text
-1 + 0 + 2 = 1
```

Actually this is positive, so let's use:

```text
-4 + (-1) + 2 = -3
```

We need the sum to become **larger**.

Since the array is sorted:

```text
left →
```

increases the value.

Therefore:

```cpp
left++;
```

---

## Case 2 — `sum > 0`

Example:

```text
-1 + 1 + 2 = 2
```

We need the sum to become **smaller**.

Since the array is sorted:

```text
right ←
```

decreases the value.

Therefore:

```cpp
right--;
```

---

## Case 3 — `sum == 0`

We've found a valid triplet:

```cpp
ans.push_back({
    nums[i],
    nums[left],
    nums[right]
});
```

Then move **both**:

```cpp
left++;
right--;
```

Why both?

Because we've already found the current combination. Moving only one pointer would leave the other value fixed and can lead to unnecessary checks/duplicates. After finding a valid pair, both pointers move inward, then duplicates are skipped. ([Reddit][3])

---

# 🚨 The Most Important Part: Duplicates

This problem specifically asks for **unique triplets**. ([LeetCode][1])

Consider:

```text
[-1,-1,0,1]
```

If we use the first `-1`:

```text
-1 + 0 + 1 = 0
```

We get:

```text
[-1,0,1]
```

If we then use the second `-1` as `i`, we'd get:

```text
[-1,0,1]
```

again.

So we need:

```cpp
if (i > 0 && nums[i] == nums[i - 1])
    continue;
```

This means:

> If this is the same value as the previous `i`, don't use it as the starting point again.

---

# Why Do We Skip `i` Duplicates?

Suppose:

```text
[-1,-1,0,1]
```

First:

```text
i = 0
nums[i] = -1
```

We find:

```text
[-1,0,1]
```

Then:

```text
i = 1
nums[i] = -1
```

But this would generate the **same set of possible triplets** because the value is still `-1`.

Therefore:

```cpp
if (i > 0 && nums[i] == nums[i-1])
    continue;
```

---

# Duplicate `left` Values

Suppose:

```text
[-2,0,0,0,2,2]
```

We might find:

```text
[-2,0,2]
```

Then after:

```cpp
left++;
right--;
```

we could land on another `0`.

We don't want to add:

```text
[-2,0,2]
```

again.

So after finding a valid triplet:

```cpp
while (left < right &&
       nums[left] == nums[left - 1]) {
    left++;
}
```

This skips repeated values.

---

# Complete Solution

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {

        vector<vector<int>> ans;

        // Sort the array
        sort(nums.begin(), nums.end());

        int n = nums.size();

        // Fix the first element
        for (int i = 0; i < n - 2; i++) {

            // If nums[i] is positive,
            // remaining numbers are also positive
            // so sum can never become 0.
            if (nums[i] > 0)
                break;

            // Skip duplicate first elements
            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = n - 1;

            // Two pointer search
            while (left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                if (sum < 0) {

                    left++;

                } else if (sum > 0) {

                    right--;

                } else {

                    // Found a valid triplet
                    ans.push_back({
                        nums[i],
                        nums[left],
                        nums[right]
                    });

                    // Move both pointers
                    left++;
                    right--;

                    // Skip duplicate left values
                    while (left < right &&
                           nums[left] == nums[left - 1]) {
                        left++;
                    }
                }
            }
        }

        return ans;
    }
};
```

---

# 🧪 Dry Run

Let's use:

```text
nums = [-1,0,1,2,-1,-4]
```

After sorting:

```text
[-4,-1,-1,0,1,2]
```

---

## `i = 0`

```text
i
↓
[-4,-1,-1,0,1,2]
```

Set:

```text
left = 1
right = 5
```

Sum:

```text
-4 + (-1) + 2 = -3
```

Too small:

```cpp
left++;
```

Now:

```text
-4 + (-1) + 2 = -3
```

Again.

Eventually:

```text
-4 + 1 + 2 = -1
```

Still too small.

No valid triplet for `-4`.

---

## `i = 1`

```text
[-4, -1, -1, 0, 1, 2]
      ↑
      i
```

```text
left = 2
right = 5
```

Sum:

```text
-1 + (-1) + 2 = 0
```

Found:

```text
[-1,-1,2]
```

Add it.

Move:

```text
left++
right--
```

Now:

```text
left = 3
right = 4
```

Sum:

```text
-1 + 0 + 1 = 0
```

Found:

```text
[-1,0,1]
```

Add it.

---

## `i = 2`

Now:

```text
nums[2] = -1
nums[1] = -1
```

They're equal.

So:

```cpp
if (nums[i] == nums[i - 1])
    continue;
```

We skip it.

This prevents:

```text
[-1,-1,2]
[-1,0,1]
```

from being generated again.

---

# ⭐ Why `if (nums[i] > 0) break`?

After sorting:

```text
[-4,-2,-1,0,1,2]
```

Suppose:

```text
nums[i] = 1
```

Every number after it is:

```text
1,2,...
```

So:

```text
1 + positive + positive > 0
```

There is no way to make the sum `0`.

Therefore:

```cpp
if (nums[i] > 0)
    break;
```

is a valid optimization. Importantly, it must be `> 0`, **not `>= 0`**, because:

```text
[0,0,0]
```

is a valid answer. ([NeetCode][2])

---

# 🧠 Why Sorting Is So Powerful Here

Without sorting:

```text
[-1,0,1,2,-1,-4]
```

we can't confidently say:

```text
sum < 0 → left++
```

because we don't know whether moving `left` gives us a larger number.

After sorting:

```text
[-4,-1,-1,0,1,2]
```

we know:

```text
left++ → value increases
right-- → value decreases
```

Therefore:

```text
sum < 0
   ↓
need bigger
   ↓
left++

sum > 0
   ↓
need smaller
   ↓
right--
```

This is the core reason the two-pointer technique works. ([LeetCode][4])

---

# 🔥 Connection to Two Sum

You already did **Two Sum**.

Two Sum:

```text
target = 9

current = 2
needed = 7
```

Using a Hash Map:

```cpp
complement = target - nums[i];
```

3Sum is basically:

```text
Fix nums[i]
       ↓
target = -nums[i]
       ↓
Find two numbers whose sum = target
       ↓
Two Pointer
```

So:

```text
Two Sum
   ↓
Find 2 numbers

3Sum
   ↓
Fix 1 number
   ↓
Find remaining 2 numbers
```

That's a very important pattern for interviews.

---

# ⚠️ Common Mistakes

### 1. Forgetting to sort

```cpp
sort(nums.begin(), nums.end());
```

is essential for the two-pointer approach.

---

### 2. Not skipping duplicate `i`

Wrong:

```cpp
for (int i = 0; i < n; i++)
```

without duplicate handling.

Correct:

```cpp
if (i > 0 && nums[i] == nums[i - 1])
    continue;
```

---

### 3. Using `>= 0` for the early break

Wrong:

```cpp
if (nums[i] >= 0)
    break;
```

because:

```text
[0,0,0]
```

is valid.

Correct:

```cpp
if (nums[i] > 0)
    break;
```

---

### 4. Moving the wrong pointer

```text
sum < 0 → left++

sum > 0 → right--
```

Remember:

```text
LEFT  → bigger values
RIGHT ← smaller values
```

---

# 🔑 Interview Cheat Sheet

```cpp
sort(nums.begin(), nums.end());

for (int i = 0; i < n - 2; i++) {

    if (nums[i] > 0)
        break;

    if (i > 0 && nums[i] == nums[i - 1])
        continue;

    int left = i + 1;
    int right = n - 1;

    while (left < right) {

        int sum = nums[i] + nums[left] + nums[right];

        if (sum < 0) {
            left++;

        } else if (sum > 0) {
            right--;

        } else {

            ans.push_back({
                nums[i],
                nums[left],
                nums[right]
            });

            left++;
            right--;

            while (left < right &&
                   nums[left] == nums[left - 1]) {
                left++;
            }
        }
    }
}
```

---

# 🎯 Mental Model

> **Sort → Fix one number → Two pointers for the other two → Move based on the sum → Skip duplicates.**

```text
              FIX
               ↓
        nums[i] = a
               ↓
       Need b + c = -a
               ↓
      ┌───────────────┐
      │               │
    left            right
      →               ←
      │               │
      └───────┬───────┘
              ↓
           compare
            sum
         /    |    \
       <0     0     >0
       ↓      ↓      ↓
    left++  save   right--
```

**Pattern to remember:**
`3Sum = Sort + Fix one element + Two Sum with two pointers + Duplicate handling.` ([LeetCode][1])

[1]: https://leetcode.com/problems/3sum/solution/?utm_source=chatgpt.com "3Sum - LeetCode"
[2]: https://neetcode.io/solutions/3sum?utm_source=chatgpt.com "LeetCode 15 3Sum Solution & Explanation | NeetCode"
[3]: https://www.reddit.com/r/leetcode/comments/172a9ub?utm_source=chatgpt.com "3Sum Two Pointer solution confusion"
[4]: https://leetcode.doocs.org/en/lc/15/?utm_source=chatgpt.com "15. 3Sum - LeetCode Wiki"
