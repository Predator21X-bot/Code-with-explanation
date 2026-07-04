# LeetCode 11 - Container With Most Water

## Pattern
Two Pointers

---

# Recognition Signals

Look for these clues:

- Two ends of an array
- Maximize or minimize a value
- Width depends on indices
- Value depends on both ends
- Brute force compares every pair (O(n²))

➡️ Think about solving from both ends using Two Pointers.

---

# Clarifying Questions

### Input

- Integer array `height`
- Each element represents the height of a vertical line.

### Output

- Return the **maximum area** of water that can be contained.

### Formula

Area = min(leftHeight, rightHeight) × (right - left)

Where:

- Height = smaller of the two walls
- Width = distance between the walls

### Is the array sorted?

No.

### Can we sort it?

No.

Sorting changes the positions of the walls, which changes the width. Since width is part of the area formula, sorting destroys the original problem.

---

# Brute Force

Check every possible pair.

For every i:

- Compare with every j > i
- Compute area
- Update maximum

Time Complexity:

O(n²)

Space Complexity:

O(1)

---

# Optimization Journey

Observation:

Area depends on two things:

1. Width
2. Smaller height

Initially, choosing the first and last elements gives the maximum possible width.

As pointers move inward, the width always decreases.

Since width can never increase, the only way to obtain a larger area is to increase the limiting height.

The limiting height is always the **shorter wall**.

Therefore:

- If left wall is shorter → move left.
- If right wall is shorter → move right.

Moving the taller wall cannot help because the shorter wall still limits the water while the width decreases.

---

# Core Intuition

The shorter wall determines the maximum water that can be stored.

Keeping the shorter wall while decreasing the width can never produce a better answer.

The only chance to improve the area is to replace the shorter wall with a taller one.

---

# Algorithm

1. Place one pointer at the beginning.
2. Place one pointer at the end.
3. Compute the current area.
4. Update the maximum area.
5. Move the pointer with the smaller height.
6. Repeat until the pointers meet.

---

# Dry Run

Input:

height = [1,8,6,2,5,4,8,3,7]

| Left | Right | Heights | Width | Area | Max Area | Move |
|------|-------|---------|------:|-----:|---------:|------|
|0|8|1,7|8|8|8|Left++|
|1|8|8,7|7|49|49|Right--|
|1|7|8,3|6|18|49|Right--|
|1|6|8,8|5|40|49|Either pointer|
|...|...|...|...|...|49|Continue|

Final Answer:

49

---

# C++ Code

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {

        int left = 0;
        int right = height.size() - 1;

        int maxArea = 0;

        while (left < right) {

            int area = min(height[left], height[right]) * (right - left);

            maxArea = max(maxArea, area);

            if (height[left] < height[right])
                left++;
            else
                right--;
        }

        return maxArea;
    }
};
```

---

# Code Walkthrough

### left

Points to the left wall.

### right

Points to the right wall.

### area

Current container area.

### maxArea

Stores the best answer found so far.

### Pointer Movement

Move the pointer with the smaller height because it is the limiting wall.

---

# Complexity

Time Complexity:

O(n)

Reason:

Each pointer moves at most once across the array.

Space Complexity:

O(1)

---

# Why Two Pointers?

Although the array is **not sorted**, we still use Two Pointers because:

- We start with the maximum possible width.
- Width only decreases.
- The only controllable factor is the limiting height.
- Moving the shorter wall is the only move that might improve the area.

---

# Difference from Two Sum II

| Two Sum II | Container With Most Water |
|------------|---------------------------|
| Goal: Find one valid pair | Goal: Find maximum area |
| Stop when answer is found | Traverse until pointers meet |
| Pointer movement depends on current sum | Pointer movement depends on smaller height |
| Return immediately | Track the best answer throughout |

---

# Common Mistakes

❌ Using `abs(left - right)` instead of `right - left`.

❌ Using `max(height[left], height[right])` instead of `min(...)`.

❌ Moving the taller pointer.

❌ Sorting the array.

---

# Similar Problems

- Two Sum II
- Trapping Rain Water
- 3Sum
- Max Consecutive Ones

---

# Spy Memory Trigger

> **"Width always decreases. Improve the limiter."**

Since the width shrinks every step, your only hope of finding a better container is to replace the shorter wall with a taller one.
