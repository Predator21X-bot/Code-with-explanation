# LeetCode 167 - Two Sum II (Input Array Is Sorted)

## Pattern
Two Pointers

---

# Recognition Signals

Whenever you notice:

- Sorted array
- Find a pair
- Return indices
- Only one valid solution
- O(1) extra space preferred

➡️ Think **Two Pointers**.

---

# Clarifying Questions (Interview)

Before writing any code, clarify:

1. What are the inputs?
   - Sorted integer array `numbers`
   - Integer `target`

2. What should be returned?
   - **1-based indices** of the two numbers.

3. Is the array sorted?
   - Yes, ascending order.

4. Can the same element be used twice?
   - No.

5. Is there always one solution?
   - Yes.

---

# Brute Force

Check every possible pair.

Algorithm:

For every element

- Check every element after it.
- If their sum equals target, return the indices.

### Complexity

Time : O(n²)

Space : O(1)

### Bottleneck

Most pairs are checked unnecessarily.

---

# Optimization Journey

### Observation 1

The array is already sorted.

Example

2 7 11 15

Target = 9

Suppose

Left = 2

Right = 15

Current Sum = 17

We need a smaller sum.

Question:

Which pointer should move?

Move Left?

7 + 15 = 22 ❌

Sum increased.

Move Right?

2 + 11 = 13 ✅

Sum decreased.

Because the array is sorted,

moving the **right pointer** always decreases the sum.

---

Now suppose

Current Sum = 5

Need a larger sum.

Move Right?

Impossible.

Move Left?

The left value becomes larger.

The sum increases.

Therefore

If sum < target

→ left++

If sum > target

→ right--

---

# Core Intuition

The sorted order makes pointer movement predictable.

- Moving the left pointer always increases (or keeps) the sum.
- Moving the right pointer always decreases (or keeps) the sum.

Every comparison eliminates impossible pairs.

We never revisit them.

This reduces the search space after every iteration.

---

# Algorithm

1. Initialize two pointers.

   left = 0

   right = n - 1

2. While left < right

   Compute current sum.

3. If sum == target

   Return {left + 1, right + 1}

4. If sum < target

   Move left++

5. Else

   Move right--

---

# Dry Run

Input

numbers = [2,7,11,15]

target = 9

| Left | Right | Values | Sum | Action |
|------|-------|--------|-----|--------|
|0|3|2,15|17|Move Right|
|0|2|2,11|13|Move Right|
|0|1|2,7|9|Return|

Output

{1,2}

---

# C++ Code

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {

        int left = 0;
        int right = numbers.size() - 1;

        while (left < right) {

            int sum = numbers[left] + numbers[right];

            if (sum == target)
                return {left + 1, right + 1};

            if (sum < target)
                left++;
            else
                right--;
        }

        return {};
    }
};
```

---

# Code Walkthrough

### left

Starts from the smallest element.

### right

Starts from the largest element.

### sum

Represents the current candidate pair.

### sum == target

Return the required **1-based indices**.

### sum < target

Current sum is too small.

Increase it by moving the left pointer.

### sum > target

Current sum is too large.

Decrease it by moving the right pointer.

---

# Complexity

Time Complexity

O(n)

Reason:

Each pointer moves at most **n** times.

Space Complexity

O(1)

Only two variables are used.

---

# Why Two Pointers?

Because the array is sorted.

The sorted property guarantees:

- Left → Right increases values.
- Right → Left decreases values.

Without sorting, this guarantee disappears and Two Pointers is no longer valid.

---

# Common Mistakes

❌ Returning 0-based indices.

❌ Forgetting the array is already sorted.

❌ Moving the wrong pointer.

❌ Using a HashMap when O(1) extra space is preferred.

---

# Pattern Recognition Summary

| Observation | Meaning |
|------------|---------|
| Sorted array | Two Pointers becomes possible |
| Need one pair | Search from both ends |
| Sum too small | Move left pointer |
| Sum too large | Move right pointer |
| O(1) space | Avoid HashMap |

---

# Similar Problems

- Remove Duplicates from Sorted Array
- Container With Most Water
- 3Sum
- 4Sum
- Valid Palindrome

---

# Spy Memory Trigger 🕶️

> **"Sorted + Pair = Two Pointers."**

The moment you see a sorted array and need to find a pair, place one pointer at each end and let the sorted order guide every move.
