# LeetCode 15 - 3Sum

## Pattern

Two Pointers (Outer Loop + Two Sum II)

---

# Recognition Signals

Whenever you see:

- Find triplets
- Sum = 0 (or any target)
- Multiple answers
- Unique triplets
- Values matter (not indices)

Think:

> Sort + Fix one value + Two Pointers

---

# Clarifying Questions

### Input

Vector of integers.

### Output

Return all unique triplets.

```cpp
vector<vector<int>>
```

### Return indices?

❌ No.

Return values.

### Multiple answers?

✅ Yes.

### Can input contain duplicates?

✅ Yes.

### Can output contain duplicate triplets?

❌ No.

---

# Brute Force

Three nested loops.

Try every triplet.

Check if sum == 0.

Time Complexity:

O(n³)

Space:

O(1)

Too slow.

---

# Optimization Journey

Observation:

If we fix one number,

then

```
x + ? + ? = 0
```

becomes

```
? + ? = -x
```

Now the remaining problem is simply

**Two Sum II**

---

# Core Intuition

Instead of searching for 3 numbers simultaneously,

fix one number,

then search for the remaining two numbers using Two Pointers.

---

# Invariant ⭐

Fix one unique value.

Use Left and Right pointers to search for all pairs that sum to

```
-target
```

Skip duplicate values to avoid repeated triplets.

---

# Pointer Responsibilities

## Fixed Pointer (i)

Represents the first element of the triplet.

Moves through the array.

Skips duplicate fixed values.

---

## Left Pointer

Searches for the second element.

Moves right when current sum is too small.

---

## Right Pointer

Searches for the third element.

Moves left when current sum is too large.

---

# Algorithm

1. Sort the array.
2. Create an empty answer vector.
3. Traverse every element as the fixed value.
4. Skip duplicate fixed values.
5. Set target = -nums[i].
6. Left = i + 1.
7. Right = n - 1.
8. While Left < Right:
    - Compute current sum.
    - If sum == target:
        - Store triplet.
        - Skip duplicate Left values.
        - Skip duplicate Right values.
        - Move both pointers.
    - Else if sum < target:
        - Move Left.
    - Else:
        - Move Right.
9. Return the result.

---

# Pseudocode

```
Sort array

Create result

For each element

    Skip duplicate fixed values

    target = -current value

    left = current + 1

    right = last index

    While left < right

        sum = left value + right value

        If sum == target

            Store triplet

            Skip duplicate left values

            Skip duplicate right values

            left++

            right--

        Else if sum < target

            left++

        Else

            right--
```

---

# Dry Run

Sorted

```
[-4,-1,-1,0,1,2]
```

Fix

```
-1
```

Target

```
1
```

Left = -1

Right = 2

```
-1 + 2 = 1
```

Triplet

```
[-1,-1,2]
```

Continue

Left -> 0

Right -> 1

```
0 + 1 = 1
```

Triplet

```
[-1,0,1]
```

---

# C++ Code

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {

        sort(nums.begin(), nums.end());

        vector<vector<int>> result;

        for (int i = 0; i < nums.size(); i++) {

            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int target = -nums[i];

            int left = i + 1;
            int right = nums.size() - 1;

            while (left < right) {

                int sum = nums[left] + nums[right];

                if (sum == target) {

                    result.push_back(
                        {nums[i], nums[left], nums[right]});

                    while (left < right &&
                           nums[left] == nums[left + 1])
                        left++;

                    while (left < right &&
                           nums[right] == nums[right - 1])
                        right--;

                    left++;
                    right--;

                }
                else if (sum < target) {

                    left++;

                }
                else {

                    right--;

                }
            }
        }

        return result;
    }
};
```

---

# Complexity

Sorting

O(n log n)

Outer Loop

O(n)

Inner Two Pointer

O(n)

Overall

O(n²)

Space

O(1)

(ignoring output vector)

---

# Common Mistakes

❌ Returning target instead of nums[i].

❌ Forgetting to sort.

❌ Comparing duplicate with next value instead of previous.

❌ Forgetting to skip duplicate fixed values.

❌ Forgetting to skip duplicate Left and Right values.

❌ Returning after first triplet.

---

# Pattern Summary

Two Sum II

↓

Container

↓

Remove Duplicates

↓

3Sum

3Sum is simply:

Outer Loop

+

Two Sum II

+

Duplicate Handling

---

# Memory Trigger

"Fix one. Solve Two Sum. Skip duplicates."

---

# Sensei's Lesson ⭐

Whenever a problem asks for

k numbers,

try fixing one number.

Reduce it to

(k-1)-Sum.

Problem reduction is one of the most powerful techniques in DSA.
