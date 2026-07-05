# Maximum Sum of Distinct Subarrays With Length K

**LeetCode:** 2461
**Pattern:** Sliding Window (Fixed Size) + Frequency Map

---

# Problem Statement

Given an integer array `nums` and an integer `k`, find the **maximum sum** among all subarrays of length exactly `k` such that **all elements inside the subarray are distinct**.

If no valid subarray exists, return `0`.

---

# Pattern Recognition

This problem contains the following clues:

* We are working with **contiguous elements** → **Subarray**
* Window size is **fixed (k)** → **Fixed Sliding Window**
* Need to check **distinct elements** → **Frequency Map / HashMap**
* Need to maximize something → Maintain **maximum answer**

**Pattern**

```
Sliding Window
        ↓
 Fixed Size Window
        ↓
 Maintain Sum + Frequency Map
```

---

# Investigation

## Inputs

* Integer array `nums`
* Integer `k` representing window size

---

## Output

Return

```
Maximum Sum
```

among all valid windows.

Return

```
0
```

if no valid window exists.

---

## Constraints

* Window size is fixed.
* Array may contain duplicates.
* Numbers are positive.
* Order cannot change.
* Subarray must remain contiguous.

---

## Edge Cases

### 1.

```
k > nums.size()
```

Impossible.

Return

```
0
```

---

### 2.

```
No window contains all distinct elements.
```

Example

```
9 9 9 9
k = 2
```

Answer

```
0
```

---

### 3.

Entire array is unique.

```
1 2 3 4
k = 4
```

Answer

```
10
```

---

### 4.

Duplicates outside the window are allowed.

Only duplicates **inside the current window** matter.

---

# Brute Force

Generate every subarray of size `k`.

For every subarray

* compute its sum
* check whether every element is unique

Checking uniqueness requires another HashSet.

Time Complexity

```
O(n × k)
```

Too slow.

---

# Optimized Idea

Instead of recomputing every window,

slide the window one element at a time.

When sliding

```
Remove one element
Add one element
```

instead of recalculating everything.

Maintain

```
Current Window Sum
```

and

```
Frequency of every element
```

inside the current window.

---

# Key Observation

The window is valid **only if every element appears exactly once.**

Instead of checking every frequency every time,

observe that

```
frequency map size == window size
```

means

```
All elements are distinct.
```

Example

Window

```
5 4 2
```

Map

```
5 → 1
4 → 1
2 → 1
```

Map Size

```
3
```

Window Size

```
3
```

Valid.

---

Window

```
5 5 2
```

Map

```
5 → 2
2 → 1
```

Map Size

```
2
```

Window Size

```
3
```

Not valid.

This single comparison avoids checking every frequency.

---

# Sliding Window Workflow

```
Expand window

↓

Add current element
Update sum
Update frequency

↓

Window too large?

↓

Remove left element
Decrease frequency
Erase if frequency becomes 0

↓

Window size == k ?

↓

Check

freq.size() == k

↓

Update maximum answer
```

---

# Algorithm

### Step 1

Initialize

```
left = 0

windowSum = 0

maxSum = 0

frequency map
```

---

### Step 2

Move the right pointer.

For every element

```
windowSum += nums[right]

frequency++
```

---

### Step 3

If

```
window size > k
```

remove the left element.

```
windowSum -= nums[left]

frequency--

erase if frequency becomes 0

left++
```

---

### Step 4

Whenever

```
window size == k
```

check

```
frequency.size() == k
```

If true

```
maxSum = max(maxSum, windowSum)
```

---

### Step 5

Return

```
maxSum
```

---

# Production Pseudocode

```
Initialize

left = 0
windowSum = 0
maxSum = 0

Declare frequency map

For every right pointer

    Add nums[right] to windowSum

    Increase frequency

    If window size exceeds k

        Remove nums[left] from windowSum

        Decrease frequency

        If frequency becomes zero

            Erase element

        Move left

    If window size equals k

        If frequency map size equals k

            Update maximum sum

Return maximum sum
```

---

# Production Code

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {

        unordered_map<int, int> freq;

        int left = 0;

        long long windowSum = 0;
        long long maxSum = 0;

        for (int right = 0; right < nums.size(); right++) {

            windowSum += nums[right];
            freq[nums[right]]++;

            if (right - left + 1 > k) {

                windowSum -= nums[left];

                freq[nums[left]]--;

                if (freq[nums[left]] == 0)
                    freq.erase(nums[left]);

                left++;
            }

            if (right - left + 1 == k && freq.size() == k) {
                maxSum = max(maxSum, windowSum);
            }
        }

        return maxSum;
    }
};
```

---

# Dry Run

```
nums = [1,5,4,2,9,9,9]

k = 3
```

Window

```
[1,5,4]

sum = 10

Distinct ✔

max = 10
```

Slide

```
[5,4,2]

sum = 11

Distinct ✔

max = 11
```

Slide

```
[4,2,9]

sum = 15

Distinct ✔

max = 15
```

Slide

```
[2,9,9]

Duplicate ❌

Ignore
```

Slide

```
[9,9,9]

Duplicate ❌

Ignore
```

Answer

```
15
```

---

# Complexity

### Time

```
O(n)
```

Every element

* enters once
* leaves once

---

### Space

```
O(k)
```

The frequency map stores at most `k` distinct elements.

---

# Interview Explanation (30 seconds)

> "Since the subarray size is fixed, this is a fixed sliding window problem. I maintain the current window sum and a frequency map of elements inside the window. As I expand the window, I add the new element to both the sum and the map. If the window grows beyond `k`, I remove the leftmost element from the sum and update its frequency, erasing it if its count reaches zero. Whenever the window size is exactly `k`, I know the window is valid if the number of distinct keys in the frequency map equals `k`. In that case, I update the maximum sum. This gives an `O(n)` solution with `O(k)` extra space."

---

# Key Takeaways

* Fixed-size windows shrink **only when the window exceeds `k`**.
* Maintain the window incrementally instead of recomputing.
* `windowSum` should be `long long` because the sum can exceed `int`.
* Remove keys from the frequency map when their count becomes zero.
* For distinct-element fixed windows, `freq.size() == k` is an elegant validity check.
* This problem combines **two ideas**: fixed sliding window + frequency map, a pattern that appears frequently in coding interviews.
