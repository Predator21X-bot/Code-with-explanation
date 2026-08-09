# 🟦 LeetCode 4 — Median of Two Sorted Arrays

We have two sorted arrays:

```cpp
nums1 = [1,3]
nums2 = [2]
```

Combined:

```text
[1,2,3]
```

Median:

```text
2
```

So answer = `2.0`.

---

# Step 1 — What is the median?

For an odd number of elements:

```text
[1,2,3]
```

median = middle element:

```text
2
```

For an even number:

```text
[1,2,3,4]
```

median is:

```text
(2 + 3) / 2 = 2.5
```

So normally we'd merge the two arrays and find the middle.

But there's a catch.

---

# 🚨 Why can't we simply merge?

If:

```text
nums1 = [1,3]
nums2 = [2]
```

we could merge them:

```text
[1,2,3]
```

But merging takes:

```text
O(m+n)
```

The problem requires:

```text
O(log(m+n))
```

So we're **not allowed to actually merge the arrays**.

---

# 💡 The Big Idea: Partition

Instead of constructing:

```text
[1,2,3]
```

we want to divide the two arrays into:

```text
LEFT | RIGHT
```

such that:

> Everything on the left is smaller than everything on the right.

For:

```text
nums1 = [1,3]
nums2 = [2]
```

we can partition them like:

```text
nums1: [1 | 3]
nums2: [2 | ]
```

Together:

```text
LEFT  = [1,2]
RIGHT = [3]
```

The median is then determined by the boundary between LEFT and RIGHT.

---

# ⭐ The Critical Condition

Suppose we partition:

```text
nums1: [ ... | ... ]
nums2: [ ... | ... ]
```

We want:

```text
max(left side) <= min(right side)
```

More specifically:

```text
left1 <= right2
```

and:

```text
left2 <= right1
```

These two conditions guarantee that the partition is correct.

---

# Let's use an example

```text
nums1 = [1,3]
nums2 = [2,4]
```

Total elements:

```text
4
```

We need:

```text
2 elements on the left
2 elements on the right
```

A valid partition:

```text
nums1: [1 | 3]
nums2: [2 | 4]

LEFT  = [1,2]
RIGHT = [3,4]
```

Check:

```text
left1 = 1
right1 = 3

left2 = 2
right2 = 4
```

Conditions:

```text
left1 <= right2
1 <= 4 ✅

left2 <= right1
2 <= 3 ✅
```

Therefore the partition is correct.

---

# How do we get the median?

Once the partition is correct:

```text
LEFT  = [1,2]
RIGHT = [3,4]
```

Because the total length is even:

```text
median = (largest LEFT + smallest RIGHT) / 2
```

Therefore:

```text
median = (2 + 3) / 2
       = 2.5
```

---

# Now the tricky part: Binary Search

We need to find the correct partition.

We could try every possible partition:

```text
nums1:

[] | [1,3]

[1] | [3]

[1,3] | []
```

But that would be linear.

Instead, we **binary search for the partition in one array**.

### Important trick:

We always binary search the **smaller array**.

```cpp
if (nums1.size() > nums2.size())
    swap(nums1, nums2);
```

Why?

Because then our binary search takes:

```text
O(log(min(m,n)))
```

which satisfies the required complexity.

---

# Partition Size

Suppose:

```text
m = nums1.size()
n = nums2.size()
```

Total elements:

```text
m + n
```

We want roughly half on the left.

So:

```cpp
half = (m + n + 1) / 2;
```

Why the `+1`?

It makes the left side contain the extra element when the total length is odd.

For:

```text
m+n = 5
```

we get:

```text
half = (5+1)/2
     = 3
```

So:

```text
LEFT  → 3 elements
RIGHT → 2 elements
```

The median will therefore be the largest element on the left.

---

# Visualize the Partition

Suppose:

```text
nums1 = [1,3]
nums2 = [2,4]
```

Choose:

```text
i = number of elements taken from nums1
```

Then:

```text
nums1: [ first i elements | remaining ]

nums2: [ first half-i elements | remaining ]
```

For example:

```text
i = 1
```

gives:

```text
nums1: [1 | 3]

nums2: [2 | 4]
```

So:

```text
left1  = 1
right1 = 3

left2  = 2
right2 = 4
```

---

# 🚨 Boundary Cases

This is where the code gets tricky.

What if we take **nothing** from `nums1`?

```text
nums1: [ | 1,3]
```

Then:

```text
left1
```

doesn't exist.

We represent it as:

```cpp
left1 = INT_MIN;
```

Similarly, if we take **everything** from `nums1`:

```text
nums1: [1,3 | ]
```

then:

```text
right1
```

doesn't exist.

We represent it as:

```cpp
right1 = INT_MAX;
```

So:

```cpp
int left1  = (i == 0) ? INT_MIN : nums1[i - 1];
int right1 = (i == m) ? INT_MAX : nums1[i];
```

Same for `nums2`.

---

# ⭐ How Binary Search Moves

Now suppose our partition is invalid.

We check:

```cpp
left1 > right2
```

That means:

```text
Something on nums1's LEFT
is too large.
```

So we took **too many elements from nums1**.

Therefore:

```text
Move partition LEFT
```

Binary search:

```cpp
high = i - 1;
```

---

The other case:

```cpp
left2 > right1
```

means:

```text
We didn't take enough elements from nums1.
```

So:

```text
Move partition RIGHT
```

Binary search:

```cpp
low = i + 1;
```

---

# 🧠 The Three Cases

Memorize this table:

| Condition        | Meaning             | Action           |
| ---------------- | ------------------- | ---------------- |
| `left1 > right2` | Too many from nums1 | Move left        |
| `left2 > right1` | Too few from nums1  | Move right       |
| Neither          | Correct partition   | Calculate median |

---

# Correct Partition

When:

```cpp
left1 <= right2
```

AND

```cpp
left2 <= right1
```

we have found the correct partition.

Then:

### Odd total length

```cpp
max(left1, left2)
```

### Even total length

```cpp
(max(left1, left2) + min(right1, right2)) / 2.0
```

---

# Full C++ Solution

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1,
                                  vector<int>& nums2) {

        // Always binary search the smaller array
        if (nums1.size() > nums2.size()) {
            swap(nums1, nums2);
        }

        int m = nums1.size();
        int n = nums2.size();

        int low = 0;
        int high = m;

        int half = (m + n + 1) / 2;

        while (low <= high) {

            int i = low + (high - low) / 2;
            int j = half - i;

            int left1  = (i == 0) ? INT_MIN : nums1[i - 1];
            int right1 = (i == m) ? INT_MAX : nums1[i];

            int left2  = (j == 0) ? INT_MIN : nums2[j - 1];
            int right2 = (j == n) ? INT_MAX : nums2[j];

            // Too many elements taken from nums1
            if (left1 > right2) {

                high = i - 1;

            // Too few elements taken from nums1
            } else if (left2 > right1) {

                low = i + 1;

            } else {

                // Correct partition

                if ((m + n) % 2 == 1) {

                    return max(left1, left2);

                } else {

                    return (max(left1, left2)
                          + min(right1, right2)) / 2.0;
                }
            }
        }

        return 0.0;
    }
};
```

---

# 🧠 The Entire Problem in One Picture

Think:

```text
nums1:       LEFT1 | RIGHT1
                  ↑
              partition i

nums2:       LEFT2 | RIGHT2
                  ↑
              partition j
```

We need:

```text
LEFT1 <= RIGHT2
LEFT2 <= RIGHT1
```

Then:

```text
       LEFT              RIGHT
   ┌─────────┐        ┌─────────┐
   │         │        │         │
   │  LEFT1  │        │ RIGHT1  │
   │  LEFT2  │        │ RIGHT2  │
   │         │        │         │
   └─────────┘        └─────────┘
        ↑                  ↑
     largest            smallest
      on left           on right
```

Median:

```text
Odd:
max(left1, left2)

Even:
(max(left1,left2) + min(right1,right2)) / 2
```

---

# 🔥 What You Should Remember for Interviews

Don't try to memorize the entire code.

Remember these **four ideas**:

### 1. Binary search the smaller array

```cpp
if (nums1.size() > nums2.size())
    swap(nums1, nums2);
```

### 2. Split both arrays into LEFT / RIGHT

```text
nums1 → left1 | right1
nums2 → left2 | right2
```

### 3. Correct partition condition

```cpp
left1 <= right2 && left2 <= right1
```

### 4. Fix the partition with binary search

```cpp
left1 > right2
    → move partition left

left2 > right1
    → move partition right
```

### Mental model

> **We're not finding the median directly. We're finding a partition where exactly half the elements are on the left and every left element is ≤ every right element. Once that partition is found, the median is sitting at its boundary.**
