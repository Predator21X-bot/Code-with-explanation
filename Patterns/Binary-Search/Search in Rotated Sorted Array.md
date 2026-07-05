# Binary Search – Search in Rotated Sorted Array (LC 33)

## Pattern

**Modified Binary Search**

---

# Investigation

### Inputs

* Rotated sorted array `nums`
* Integer `target`

### Output

* Return index of `target`
* Return `-1` if not found

### Constraints

* Array is originally sorted in ascending order.
* Rotated exactly once (possibly zero rotations).
* All elements are **unique**.
* Expected Time: **O(log n)**
* Extra Space: **O(1)**

---

# Key Observation

A rotated sorted array has **exactly one pivot**.

Example

```text
Original

1 2 3 4 5 6 7

Rotated

5 6 7 1 2 3 4
      ^
     Pivot
```

The pivot is the **only place where ascending order breaks**.

---

# Why Binary Search Works

When we split the array into two halves,

```text
left -------- mid -------- right
```

there is only **one pivot**.

Therefore,

> **At least one half is always completely sorted.**

Our goal is:

1. Find the sorted half.
2. Check whether the target lies inside it.
3. Discard the other half.

---

# How to Find the Sorted Half

## Left Half Sorted

```cpp
if (nums[left] <= nums[mid])
```

Example

```text
4 5 6 7 | 0 1 2
```

Since

```text
4 <= 7
```

there is no pivot in the left half.

So

```text
Left Half = Sorted
```

---

## Right Half Sorted

Otherwise,

```cpp
else
```

Example

```text
6 7 8 | 1 2 3 4
```

Since

```text
6 <= 1 ❌
```

the pivot lies in the left half.

Therefore,

```text
Right Half = Sorted
```

---

# The Golden Question

Once a half is sorted,

**never ask**

> Is the target on the other side?

Instead ask

> **Does the target belong inside the sorted half?**

---

# Left Half Logic

Suppose

```text
4 5 6 7 | 0 1 2
```

Target = **5**

Sorted range

```text
4 ---------- 7
```

Ask

```text
Is 5 inside [4,7] ?
```

Condition

```cpp
nums[left] <= target &&
target < nums[mid]
```

If true

```cpp
right = mid - 1;
```

Otherwise

```cpp
left = mid + 1;
```

---

# Right Half Logic

Suppose

```text
6 7 8 | 1 2 3 4
```

Target = **3**

Sorted range

```text
1 ---------- 4
```

Ask

```text
Is 3 inside [1,4] ?
```

Condition

```cpp
nums[mid] < target &&
target <= nums[right]
```

If true

```cpp
left = mid + 1;
```

Otherwise

```cpp
right = mid - 1;
```

---

# Why `<` instead of `<=`?

At the beginning of every iteration,

```cpp
if (nums[mid] == target)
    return mid;
```

So later,

```cpp
target != nums[mid]
```

Therefore

```cpp
target < nums[mid]
```

and

```cpp
nums[mid] < target
```

are sufficient.

---

# Algorithm

1. Initialize `left` and `right`.
2. While `left <= right`
3. Compute `mid`.
4. If `nums[mid] == target`, return `mid`.
5. Determine which half is sorted.
6. Check if the target lies inside the sorted half.
7. Keep the half containing the target.
8. Repeat.
9. Return `-1`.

---

# Production Pseudocode

```text
left = 0
right = n - 1

while left <= right

    mid = left + (right - left) / 2

    if nums[mid] == target
        return mid

    if left half is sorted

        if target lies in left half
            right = mid - 1
        else
            left = mid + 1

    else

        if target lies in right half
            left = mid + 1
        else
            right = mid - 1

return -1
```

---

# Production C++

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = static_cast<int>(nums.size()) - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return mid;

            // Left half is sorted
            if (nums[left] <= nums[mid]) {

                if (nums[left] <= target && target < nums[mid])
                    right = mid - 1;
                else
                    left = mid + 1;
            }
            // Right half is sorted
            else {

                if (nums[mid] < target && target <= nums[right])
                    left = mid + 1;
                else
                    right = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# Dry Run

Example

```text
nums = [4,5,6,7,0,1,2]
target = 1
```

### Iteration 1

```text
L           M           R

4 5 6 7 0 1 2
```

Left half sorted

```text
4 <= 7
```

Is target inside

```text
4 ... 7 ?
```

```text
4 <= 1 < 7

False
```

Move

```text
left = mid + 1
```

---

### Iteration 2

```text
0 1 2
```

Right half sorted.

Check range.

Target found.

---

# Complexity

**Time**

```text
O(log n)
```

Every iteration removes half the search space.

**Space**

```text
O(1)
```

Only pointers are used.

---

# Common Mistakes

❌ `mid = (left + right) / 2`

✔️

```cpp
mid = left + (right - left) / 2;
```

---

❌ Comparing

```cpp
nums[mid] >= nums[left] &&
nums[mid] < target
```

✔️ Compare **target**, not `mid`.

---

❌

```cpp
right = mid;
```

✔️

```cpp
right = mid - 1;
```

---

❌ Forgetting

```cpp
if (nums[mid] == target)
```

before checking the sorted half.

---

# Binary Search Template Learned

```cpp
while (left <= right) {

    int mid = left + (right - left) / 2;

    if (nums[mid] == target)
        return mid;

    if (nums[left] <= nums[mid]) {

        // Left half sorted

    } else {

        // Right half sorted

    }
}
```

---

# Interview Takeaway

> **A rotated sorted array has exactly one pivot. Therefore, in every iteration, one half is guaranteed to be sorted. Identify the sorted half, check if the target lies within its range, and discard the other half.**

---

### Sensei's Summary

The key lesson isn't the code—it's the invariant:

> **One half is always sorted, and a sorted half gives you a value range. Use that range to decide which half to discard.**

Once this invariant becomes second nature, problems like **Find Minimum in Rotated Sorted Array**, **Search in Rotated Sorted Array II (duplicates)**, and other rotated-array variants become much easier.
