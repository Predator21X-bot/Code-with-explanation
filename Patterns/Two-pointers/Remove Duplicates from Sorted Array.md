# LeetCode 26 - Remove Duplicates from Sorted Array

## Pattern
Two Pointers (Fast & Slow / Read & Write)

---

# Recognition Signals

Look for these clues:

- Sorted array
- Remove duplicates
- Modify array in-place
- O(1) extra space
- Return the length of the unique elements

➡️ Think **Fast & Slow (Read & Write) Pointers**.

---

# Clarifying Questions

### Input

- Sorted integer array `nums`

### Output

- Return the number of unique elements `k`.
- Modify the first `k` elements of the array to contain only unique values.
- Elements after index `k - 1` are ignored.

### What does "In-place" mean?

Modify the given array without using another array or extra memory.

### Why is sorting important?

Sorting places duplicate values next to each other.

Example:

1 1 1 2 2 3 3

Instead of

1 2 1 3 2 1

This allows us to detect duplicates by comparing only adjacent unique values.

---

# Brute Force

Create another array.

Traverse the original array.

Store only unique values.

Copy them back.

Time Complexity:

O(n)

Space Complexity:

O(n)

Problem:

Uses extra memory.

---

# Optimization Journey

Observation:

We are **not deleting** duplicates.

We are simply **overwriting** duplicate positions with the next unique value.

Example:

nums = [1,1,2,2,3]

Initially

Write → 1

Read → second 1

Duplicate found.

Do nothing.

Read moves.

Read reaches 2.

New unique value discovered.

Move Write.

Copy 2.

Array becomes:

1 2 2 2 3

Continue.

---

# Core Intuition

This problem is not about removing duplicates.

It is about **building a compact unique array at the beginning of the same array**.

The Read pointer scans every element.

The Write pointer builds the answer.

---

# Pointer Responsibilities

### Read Pointer

- Visits every element exactly once.
- Checks whether the current element is a new unique value.

### Write Pointer

- Always points to the last unique element stored.
- Moves only when a new unique element is found.
- Writes the new unique value into the next available position.

---

# Algorithm

1. Handle empty array.
2. Place Write at index 0.
3. Read starts from index 1.
4. Traverse the array.
5. If current value is different from the last unique value:
    - Move Write.
    - Copy current value.
6. Return Write + 1.

---

# Dry Run

Input

nums = [1,1,2,2,3]

| Read | Write | Comparison | Action | Array |
|------|-------|------------|--------|-------|
|1|0|1 == 1|Ignore|1 1 2 2 3|
|2|0|2 != 1|write++, copy|1 2 2 2 3|
|3|1|2 == 2|Ignore|1 2 2 2 3|
|4|1|3 != 2|write++, copy|1 2 3 2 3|

Return:

3

The first 3 elements are the answer.

---

# C++ Code

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {

        if (nums.empty())
            return 0;

        int write = 0;

        for (int read = 1; read < nums.size(); read++) {

            if (nums[read] != nums[write]) {

                write++;
                nums[write] = nums[read];
            }
        }

        return write + 1;
    }
};
```

---

# Code Walkthrough

```cpp
int write = 0;
```

Write points to the last unique element stored.

---

```cpp
for(int read = 1; ...)
```

Read scans every remaining element.

---

```cpp
if(nums[read] != nums[write])
```

Did we discover a new unique value?

---

```cpp
write++;
```

Move to the next free position.

---

```cpp
nums[write] = nums[read];
```

Store the newly discovered unique value.

---

```cpp
return write + 1;
```

The number of unique elements.

---

# Complexity

Time Complexity

O(n)

Reason:

Each element is visited exactly once.

Space Complexity

O(1)

No extra array is used.

---

# Why Two Pointers?

This is a **Fast & Slow Pointer** problem.

Unlike Two Sum II and Container With Most Water:

- Both pointers move in the same direction.
- Read scans.
- Write builds.

---

# Difference from Previous Two Pointer Problems

| Problem | Pointer Decision |
|----------|------------------|
| Two Sum II | Move based on current sum |
| Container With Most Water | Move based on shorter height |
| Remove Duplicates | Move Write only when a new unique value is found |

---

# Common Mistakes

❌ Thinking we are deleting duplicates.

❌ Using an extra array.

❌ Moving the Write pointer for duplicates.

❌ Forgetting to return `write + 1`.

---

# Similar Problems

- Remove Element
- Move Zeroes
- Sort Colors
- Merge Sorted Array

---

# Pattern Template (Fast & Slow)

1. Assign responsibilities.

Read → scans.

Write → builds.

2. Traverse using Read.

3. If Read finds a valid element:
    - Move Write.
    - Copy.

4. Return the final Write position.

---

# Sensei's Lesson ⭐

When you struggle to convert an algorithm into code:

1. Give every pointer a responsibility.
2. Write the algorithm in English.
3. Translate one English sentence into one line of code.

Never invent the algorithm while typing.

---

# Memory Trigger

> **"Read discovers. Write builds."**

The Read pointer explores the array.

The Write pointer constructs the final answer.
