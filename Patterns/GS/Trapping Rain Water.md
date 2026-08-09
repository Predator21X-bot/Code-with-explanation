# 🟦 LeetCode 42 — Trapping Rain Water

**Difficulty:** Hard
**Pattern:** Two Pointers
**Time Complexity:** `O(n)`
**Space Complexity:** `O(1)`

---

# 📌 Problem

Given an array where:

```cpp
height[i]
```

represents the height of a vertical bar, calculate how much rainwater can be trapped between the bars.

Example:

```text
height = [0,1,0,2,1,0,1,3,2,1,2,1]
```

Output:

```text
6
```

---

# 💡 Core Idea

Water can be trapped at index `i` only when there are boundaries on **both sides**.

For every position:

```text
water[i] =
min(leftMax, rightMax) - height[i]
```

where:

```text
leftMax  = highest bar to the left
rightMax = highest bar to the right
```

### Why `min()`?

Suppose:

```text
leftMax  = 5
rightMax = 3
```

Water cannot rise above `3`, because the right side would overflow.

Therefore:

```text
water level = min(5,3)
            = 3
```

If the current bar has height `1`:

```text
water = 3 - 1
      = 2
```

---

# 🧠 Brute Force Approach

For every index `i`:

1. Find the maximum height on the left.
2. Find the maximum height on the right.
3. Calculate:

```cpp
min(leftMax, rightMax) - height[i]
```

But finding left/right maximum every time takes:

```text
O(n²)
```

We can do better.

---

# ⭐ Two Pointer Approach

Use two pointers:

```cpp
int left = 0;
int right = height.size() - 1;
```

And maintain:

```cpp
int leftMax = 0;
int rightMax = 0;
```

Visualize:

```text
left →                         ← right

[ ... ... ... ... ... ... ... ... ]
```

Instead of calculating the maximum on both sides for every index, we maintain the maximum seen so far.

---

# 🔑 Most Important Insight

Suppose:

```text
leftMax = 5
rightMax = 3
```

The limiting side is the **right side**.

Why?

Because:

```text
water level = min(leftMax, rightMax)
            = 3
```

So we can safely calculate water on the side with the **smaller maximum boundary**.

### Rule

```text
if leftMax < rightMax
    process left

else
    process right
```

---

# Another Way to Think About It

At any point:

```text
leftMax < rightMax
```

We already know the right side has a wall at least as high as `rightMax`.

Therefore, for the left position, the right boundary is **not the limiting factor**.

The left boundary is.

So we can safely calculate:

```cpp
leftMax - height[left]
```

Similarly, if:

```text
rightMax <= leftMax
```

we process the right side:

```cpp
rightMax - height[right]
```

---

# Algorithm

### Step 1

Initialize pointers:

```cpp
int left = 0;
int right = height.size() - 1;
```

---

### Step 2

Initialize maximum boundaries:

```cpp
int leftMax = 0;
int rightMax = 0;
```

---

### Step 3

While pointers haven't crossed:

```cpp
while (left <= right)
```

---

### Step 4

If the left boundary is smaller:

```cpp
if (height[left] <= height[right])
```

Process the left side.

Update:

```cpp
leftMax = max(leftMax, height[left]);
```

Otherwise:

```cpp
water += leftMax - height[left];
```

Then move:

```cpp
left++;
```

---

### Step 5

Otherwise process the right side.

Update:

```cpp
rightMax = max(rightMax, height[right]);
```

Otherwise:

```cpp
water += rightMax - height[right];
```

Then:

```cpp
right--;
```

---

# C++ Solution

```cpp
class Solution {
public:
    int trap(vector<int>& height) {

        int left = 0;
        int right = height.size() - 1;

        int leftMax = 0;
        int rightMax = 0;

        int water = 0;

        while (left <= right) {

            if (height[left] <= height[right]) {

                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }

                left++;

            } else {

                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }

                right--;
            }
        }

        return water;
    }
};
```

---

# 🧪 Dry Run

Consider the simpler example:

```text
height = [3,1,2]
```

Initially:

```text
left = 0
right = 2

leftMax = 0
rightMax = 0

water = 0
```

---

### Step 1

```text
height[left] = 3
height[right] = 2
```

Since:

```text
3 > 2
```

process the right side.

```text
rightMax = max(0,2)
         = 2
```

Move:

```text
right--
```

Now:

```text
right = 1
```

---

### Step 2

Now:

```text
height[left] = 3
height[right] = 1
```

Again:

```text
3 > 1
```

Process right.

`1 < rightMax`:

```text
water += 2 - 1
       = 1
```

Final:

```text
water = 1
```

Correct.

---

# Why Do We Compare `height[left]` and `height[right]`?

This is the subtle part.

We need to know which side is the **limiting boundary**.

If:

```text
height[left] <= height[right]
```

the right side currently has a wall at least as high as the left side.

Therefore we can safely process the left side.

Otherwise:

```text
height[right] < height[left]
```

the right side is the limiting side, so process the right.

---

# ⚠️ Important Distinction

There are two closely related ways you may see this solution written.

### Version 1 — Compare current heights

```cpp
if (height[left] <= height[right])
```

This is the version above.

### Version 2 — Compare current maximum boundaries

```cpp
if (leftMax <= rightMax)
```

Both can be used with carefully maintained invariants, but **don't mix the logic between the two implementations**.

For your interview preparation, I'd recommend mastering the first version because the reasoning is easier to trace:

```text
Smaller current boundary
        ↓
Process that side
        ↓
Maintain its maximum
        ↓
Calculate trapped water
```

---

# Common Mistakes

### ❌ Using `max()` instead of `min()`

Wrong:

```cpp
water = max(leftMax, rightMax) - height[i];
```

Correct:

```cpp
water = min(leftMax, rightMax) - height[i];
```

Water is limited by the **shorter boundary**.

---

### ❌ Moving the wrong pointer

If the left side is the limiting side:

```cpp
left++;
```

If the right side is limiting:

```cpp
right--;
```

---

### ❌ Forgetting to update `leftMax` / `rightMax`

If the current bar is taller:

```cpp
leftMax = max(leftMax, height[left]);
```

or:

```cpp
rightMax = max(rightMax, height[right]);
```

---

### ❌ Calculating negative water

If:

```text
height[i] > leftMax
```

there is no water.

Instead, this bar becomes the new boundary.

That's why we do:

```cpp
if (height[left] >= leftMax)
    leftMax = height[left];
else
    water += leftMax - height[left];
```

---

# 🧠 Pattern Recognition

When you see:

> Calculate something based on the maximum/minimum boundary on both sides.

Think:

```text
Left boundary + Right boundary
          ↓
      Two Pointers
          ↓
   Maintain leftMax
   Maintain rightMax
          ↓
 Process the limiting side
```

---

# Complexity

### Time

Each pointer moves across the array once:

```text
O(n)
```

### Space

Only a few variables:

```text
O(1)
```

---

# 🔥 Interview Cheat Sheet

Remember these four variables:

```cpp
int left = 0;
int right = n - 1;

int leftMax = 0;
int rightMax = 0;
```

Core idea:

```text
             Water
               ↓
      min(leftMax, rightMax)
               ↓
         - height[i]
```

Two-pointer rule:

```text
Left side is limiting
        ↓
Process left
        ↓
left++

Right side is limiting
        ↓
Process right
        ↓
right--
```

### Mental Model

> **Water at a position is determined by the shorter wall around it. With two pointers, always process the side whose boundary is smaller, while maintaining the maximum wall seen on each side.**
