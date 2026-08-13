# 🟦 LeetCode 153 — Find Minimum in Rotated Sorted Array

**Difficulty:** Medium
**Pattern:** Binary Search
**Time Complexity:** `O(log n)`
**Space Complexity:** `O(1)`

The important observation is that although the array is rotated, it was **originally sorted**, so it consists of two sorted portions. Because all values are unique, we can use binary search to determine which portion contains the minimum. ([LeetCode][1])

---

## 📌 Problem

Given a sorted array that has been rotated, find its minimum element.

Example:

```text
Original:
[0,1,2,4,5,6,7]

Rotated:
[4,5,6,7,0,1,2]
```

Answer:

```text
0
```

The problem requires an `O(log n)` solution. ([LeetCode][1])

---

# 💡 First Understand What "Rotated Sorted" Means

Take:

```text
[1,2,3,4,5]
```

Rotate it:

```text
[3,4,5,1,2]
```

Notice something:

```text
[3,4,5] → sorted
[1,2]   → sorted
```

There is just **one point where the sorted order breaks**:

```text
5 → 1
```

And the element immediately after that break is the minimum:

```text
        ↓
[3,4,5,1,2]
       ↑
      min
```

So our goal is essentially:

> **Use binary search to find which side contains that "break".**

---

# ⭐ The Key Observation

We compare:

```cpp
nums[mid]
```

with:

```cpp
nums[right]
```

Why `right`?

Because `nums[right]` tells us which sorted portion `mid` belongs to.

Consider:

```text
[4,5,6,7,0,1,2]
       ↑     ↑
      mid   right
       7     2
```

Here:

```text
nums[mid] > nums[right]
```

because:

```text
7 > 2
```

That tells us:

> The minimum **must be to the right of `mid`**.

So:

```cpp
left = mid + 1;
```

---

# 🧠 Why Does `nums[mid] > nums[right]` Mean Go Right?

Let's visualize:

```text
[4,5,6,7 | 0,1,2]
           ↑
         minimum
```

If:

```text
nums[mid] > nums[right]
```

then `mid` is sitting in the **left/high portion** of the rotated array.

Example:

```text
mid = 7
right = 2

7 > 2
```

The drop:

```text
7 → 0
```

must be somewhere **after `mid`**.

Therefore:

```cpp
left = mid + 1;
```

---

# ⭐ The Other Case

Suppose:

```text
[5,6,7,1,2,3,4]
     ↑       ↑
    mid    right
     1       4
```

Now:

```text
nums[mid] < nums[right]
```

because:

```text
1 < 4
```

This tells us:

> `mid` is already in the sorted right portion, so the minimum could be `mid` itself or somewhere to its left.

Therefore:

```cpp
right = mid;
```

### Notice:

We use:

```cpp
right = mid;
```

**not**

```cpp
right = mid - 1;
```

because `mid` itself could be the minimum.

---

# 🔥 The Two Conditions

This is the heart of the problem:

```cpp
if (nums[mid] > nums[right]) {
    left = mid + 1;
}
else {
    right = mid;
}
```

Think:

```text
nums[mid] > nums[right]
        ↓
 minimum is RIGHT of mid
        ↓
 left = mid + 1
```

Otherwise:

```text
nums[mid] < nums[right]
        ↓
 minimum is AT mid or LEFT
        ↓
 right = mid
```

Because all elements are unique, equality doesn't arise between `nums[mid]` and `nums[right]` unless they refer to the same index. ([LeetCode][1])

---

# 🧪 Dry Run — `[3,4,5,1,2]`

Let's really understand this one.

```text
nums = [3,4,5,1,2]
```

Initially:

```text
left = 0
right = 4
```

```text
[3,4,5,1,2]
 ↑       ↑
 L       R
```

---

## Iteration 1

```cpp
mid = left + (right-left)/2;
```

```text
mid = 2
```

So:

```text
[3,4,5,1,2]
       ↑   ↑
      mid right
       5   2
```

Compare:

```text
5 > 2
```

Therefore:

```cpp
left = mid + 1;
```

So:

```text
left = 3
```

Our search space becomes:

```text
[1,2]
 ↑ ↑
 L R
```

We eliminated:

```text
[3,4,5]
```

because the minimum cannot be there.

---

## Iteration 2

Now:

```text
left = 3
right = 4
```

Calculate:

```text
mid = 3
```

```text
[3,4,5,1,2]
       ↑   ↑
      mid right
       1   2
```

Compare:

```text
1 < 2
```

Therefore:

```cpp
right = mid;
```

So:

```text
right = 3
```

Now:

```text
left = 3
right = 3
```

Loop ends.

Return:

```cpp
nums[left]
```

which is:

```text
1
```

---

# ⭐ Why Does the Loop Use `left < right`?

We write:

```cpp
while (left < right)
```

because we're trying to reduce the search space until:

```text
left == right
```

At that point, only **one possible minimum** remains.

Example:

```text
left = 3
right = 3

[3,4,5,1,2]
       ↑
      min
```

So:

```cpp
return nums[left];
```

---

# 🧠 Why Not `while(left <= right)`?

You can design binary search that way, but this particular solution is cleaner with:

```cpp
while (left < right)
```

because we're not searching for an exact target.

We're narrowing down the **position of the minimum**.

Our invariant is:

> **The minimum is always somewhere in `[left, right]`.**

Each iteration shrinks this range until one index remains.

---

# ✅ C++ Solution

```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {

        int left = 0;
        int right = nums.size() - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right]) {

                // Minimum is to the right of mid
                left = mid + 1;

            } else {

                // Minimum is at mid or to the left
                right = mid;
            }
        }

        return nums[left];
    }
};
```

This is the standard `O(log n)` binary-search approach. ([LeetCode][1])

---

# 🧪 Example 2 — Already Sorted Array

Consider:

```text
[1,2,3,4,5]
```

There is technically no visible "drop"; the minimum is simply the first element.

```text
left = 0
right = 4

mid = 2
```

Compare:

```text
nums[mid] = 3
nums[right] = 5
```

```text
3 < 5
```

Therefore:

```cpp
right = mid;
```

Now:

```text
left = 0
right = 2
```

Again:

```text
mid = 1

nums[mid] = 2
nums[right] = 3
```

Again:

```text
2 < 3
```

So:

```text
right = 1
```

Eventually:

```text
left = right = 0
```

Return:

```text
1
```

Correct.

---

# 🔥 Why We Don't Compare `nums[mid]` With `nums[left]`

You might wonder:

> Why `nums[right]` specifically?

Because `right` gives us a reliable reference for the **right sorted portion**.

Consider:

```text
[4,5,6,7,0,1,2]
```

At:

```text
mid = 3
```

we have:

```text
nums[mid] = 7
nums[right] = 2
```

So:

```text
7 > 2
```

This immediately tells us:

```text
minimum is somewhere AFTER mid
```

The comparison with `right` directly tells us which side of the rotation we're on.

---

# 🚨 The Most Important `right = mid`

This is something you should remember for interviews.

When:

```cpp
nums[mid] < nums[right]
```

we know:

```text
minimum ∈ [left, mid]
```

So:

```cpp
right = mid;
```

Why not:

```cpp
right = mid - 1;
```

Because `mid` itself might be the minimum.

Example:

```text
[4,5,6,1,2,3]
       ↑
      mid
```

If:

```text
nums[mid] = 1
```

then `mid` **is the answer**.

So we must keep it:

```cpp
right = mid;
```

---

# 🧠 Binary Search Invariant

This is a great way to explain the solution in an interview:

> **At every iteration, I maintain that the minimum element lies within `[left, right]`.**

Then:

### If:

```cpp
nums[mid] > nums[right]
```

The minimum cannot be at or before `mid`, so:

```cpp
left = mid + 1;
```

### Otherwise:

```cpp
nums[mid] < nums[right]
```

The minimum can be `mid` or somewhere before it:

```cpp
right = mid;
```

Eventually:

```text
left == right
```

and that index contains the minimum.

---

# ⚠️ Common Mistakes

### ❌ Mistake 1

```cpp
if (nums[mid] > nums[right])
    right = mid;
```

Wrong direction.

If `mid > right`, we're in the **left/high portion**, so the minimum is to the **right**.

Correct:

```cpp
left = mid + 1;
```

---

### ❌ Mistake 2

```cpp
right = mid - 1;
```

when:

```cpp
nums[mid] < nums[right]
```

Wrong because `mid` itself could be the minimum.

Correct:

```cpp
right = mid;
```

---

### ❌ Mistake 3

Using:

```cpp
while (left <= right)
```

and trying to return `nums[mid]`.

This problem is about **shrinking the range to one candidate**, so:

```cpp
while (left < right)
```

with:

```cpp
return nums[left];
```

is much cleaner.

---

# 🎯 Interview Cheat Sheet

### Initialize

```cpp
int left = 0;
int right = nums.size() - 1;
```

### Binary search

```cpp
while (left < right) {

    int mid = left + (right - left) / 2;

    if (nums[mid] > nums[right]) {
        left = mid + 1;
    } else {
        right = mid;
    }
}
```

### Answer

```cpp
return nums[left];
```

### Complexity

```text
Time  → O(log n)
Space → O(1)
```

---

# 🔑 Mental Model

Don't memorize the code first. Remember this picture:

```text
        LEFT PART          RIGHT PART
      (larger values)    (smaller values)

[ 4  5  6  7 | 0  1  2 ]
                ↑
             MINIMUM
```

Then ask:

```text
Is nums[mid] > nums[right]?
```

### YES

```text
mid is in the larger/left portion

        ↓

minimum must be RIGHT of mid

        ↓

left = mid + 1
```

### NO

```text
mid is in the smaller/right portion

        ↓

minimum is AT mid or LEFT of mid

        ↓

right = mid
```

---

## ⭐ One-line takeaway

> **Compare `mid` with `right`: if `nums[mid] > nums[right]`, throw away the left half; otherwise keep `mid` and throw away the right half.**

This is the key binary-search pattern behind LeetCode 153. ([LeetCode][1])

[1]: https://leetcode.doocs.org/en/lc/153/?utm_source=chatgpt.com "153. Find Minimum in Rotated Sorted Array - LeetCode Wiki"
