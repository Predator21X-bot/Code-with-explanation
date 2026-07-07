# Find Peak Element (LeetCode 162)

## Pattern

**Binary Search on Slope**

Unlike traditional Binary Search, the array is **not sorted**.

We use the **direction of the slope** to eliminate half of the search space.

---

# Investigation

### Input

* Integer array `nums`

### Output

* Return the **index** of any peak element.

### Peak Element

A peak element is an element that is **strictly greater than both of its neighbors**.

If multiple peaks exist, return **any** one.

### Constraints

* `1 <= nums.length <= 1000`
* `nums[i] != nums[i+1]`
* Expected Time: **O(log n)**
* Extra Space: **O(1)**

---

# Edge Cases

* Single element array
* Peak at the first index
* Peak at the last index
* Strictly increasing array
* Strictly decreasing array

---

# Special Properties

* Array is **not sorted**
* Adjacent elements are **never equal**
* Order matters
* We compare **adjacent elements** to determine the slope.

---

# Key Observation

We are **not** searching on sorted values.

We are searching on the **direction of the slope**.

Compare only

```cpp
nums[mid]
```

with

```cpp
nums[mid + 1]
```

---

# Why Binary Search Works

Every comparison tells us where a peak **must exist**.

## Case 1 : Downhill

```text
5 4 3 2
```

or

```text
nums[mid] > nums[mid + 1]
```

We are descending.

A peak must exist on the **left**, including `mid`.

Move

```cpp
right = mid;
```

---

## Case 2 : Uphill

```text
1 2 3 4
```

or

```text
nums[mid] < nums[mid + 1]
```

We are climbing.

A peak must exist on the **right**.

Move

```cpp
left = mid + 1;
```

---

# Mental Model

Imagine climbing a mountain.

```
Going Up (↗)

Keep climbing.

Peak is ahead.

left = mid + 1
```

```
Going Down (↘)

You've already crossed the peak.

Peak is behind (or at mid).

right = mid
```

---

# Algorithm

1. Initialize `left` and `right`.
2. While `left < right`
3. Compute `mid`.
4. Compare `nums[mid]` and `nums[mid + 1]`.
5. If descending, move `right = mid`.
6. If ascending, move `left = mid + 1`.
7. Continue until `left == right`.
8. Return `left`.

---

# Production Pseudocode

```text
left = 0
right = n - 1

while left < right

    mid = left + (right - left) / 2

    if nums[mid] > nums[mid + 1]
        right = mid
    else
        left = mid + 1

return left
```

---

# Production C++

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int left = 0;
        int right = static_cast<int>(nums.size()) - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[mid + 1]) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        return left;
    }
};
```

---

# Dry Run

### Example

```text
nums = [1,2,3,1]
```

### Iteration 1

```text
L      M      R

1 2 3 1
```

```
nums[mid] > nums[mid+1]

3 > 1
```

Descending

```
right = mid
```

---

### Iteration 2

```text
L   M   R

1 2 3
```

```
nums[mid] < nums[mid+1]

2 < 3
```

Ascending

```
left = mid + 1
```

---

Now

```text
left == right == 2
```

Return

```text
2
```

---

# Complexity

**Time**

```text
O(log n)
```

Each iteration discards half of the search space.

**Space**

```text
O(1)
```

Only constant extra space is used.

---

# Common Mistakes

### ❌ Using

```cpp
while (left <= right)
```

✅ Correct

```cpp
while (left < right)
```

---

### ❌ Comparing with `left` or `right`

```cpp
nums[mid] > nums[right]
```

✅ Correct

```cpp
nums[mid] > nums[mid + 1]
```

---

### ❌ Writing

```cpp
right = mid - 1;
```

This may discard the peak.

✅ Correct

```cpp
right = mid;
```

because `mid` itself can be the peak.

---

### ❌ Returning `mid`

`mid` changes every iteration.

✅ Return

```cpp
return left;
```

because the loop ends when

```text
left == right
```

---

# Binary Search Template Learned

```cpp
while (left < right) {

    int mid = left + (right - left) / 2;

    if (nums[mid] > nums[mid + 1]) {
        right = mid;
    } else {
        left = mid + 1;
    }
}

return left;
```

---

# Interview Takeaway

> **This is Binary Search on the slope, not on sorted values. Every comparison with `nums[mid + 1]` tells us whether we're climbing or descending. That guarantees one half always contains a peak, allowing us to discard the other half while maintaining an `O(log n)` solution.**
