# 🟦 LeetCode 435 — Non-overlapping Intervals

**Difficulty:** Medium
**Pattern:** Greedy + Sorting
**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(1)` auxiliary space *(excluding sorting implementation)*

---

# 📌 Problem

Given a collection of intervals:

```cpp
[start, end]
```

return the **minimum number of intervals that must be removed** so that the remaining intervals are non-overlapping.

---

# Example

```text
Input:

[[1,2],[2,3],[3,4],[1,3]]
```

Sort first:

```text
[[1,2],[1,3],[2,3],[3,4]]
```

`[1,2]` and `[1,3]` overlap.

We remove:

```text
[1,3]
```

Remaining:

```text
[1,2]
[2,3]
[3,4]
```

These do not overlap.

Answer:

```text
1
```

---

# 💡 Key Greedy Insight

When two intervals overlap:

```text
[1,4]
[2,3]
```

we have to remove **one**.

Which one should we keep?

```text
[1,4] → ends at 4
[2,3] → ends at 3
```

Keep:

```text
[2,3]
```

because it ends earlier.

### Why?

An interval that ends earlier leaves **more room for future intervals**.

```text
Earlier end
     ↓
More remaining space
     ↓
More intervals can fit
     ↓
Fewer removals overall
```

This is the core greedy idea.

---

# ⭐ Greedy Rule

> **When two intervals overlap, remove the interval with the larger end time and keep the one with the smaller end time.**

---

# Why Sort?

We first sort the intervals:

```cpp
sort(intervals.begin(), intervals.end());
```

This sorts by:

```text
start → then end
```

Example:

```text
Before:

[2,3]
[1,4]
[1,2]

After:

[1,2]
[1,4]
[2,3]
```

Now we can process intervals from left to right.

---

# Overlap Condition

Suppose:

```text
Previous kept interval = [1,2]
Current interval        = [2,3]
```

They **do not overlap**.

Because they only touch at `2`.

Therefore:

```cpp
currentStart < previousEnd
```

is the overlap condition.

### Overlap

```cpp
currentStart < previousEnd
```

### No overlap

```cpp
currentStart >= previousEnd
```

---

# Important Difference from Merge Intervals

In **Merge Intervals**:

```text
Overlap
   ↓
Merge
```

Here:

```text
Overlap
   ↓
Remove one
```

And among the overlapping intervals:

```text
Keep the one with smaller end.
```

---

# Variables

We maintain:

```cpp
int count;
```

Number of intervals removed.

And:

```cpp
int previousEnd;
```

The end time of the interval we have decided to **keep**.

For the current interval:

```cpp
int currentStart = intervals[i][0];
int currentEnd = intervals[i][1];
```

---

# Algorithm

### Step 1 — Sort

```cpp
sort(intervals.begin(), intervals.end());
```

---

### Step 2 — Initialize

The first interval is our initial interval to keep.

```cpp
int previousEnd = intervals[0][1];
```

Initially:

```cpp
count = 0;
```

---

### Step 3 — Traverse from second interval

```cpp
for (int i = 1; i < intervals.size(); i++)
```

---

### Step 4 — Check overlap

```cpp
if (currentStart < previousEnd)
```

If true, the intervals overlap.

---

### Step 5 — Remove one

```cpp
count++;
```

We need to remove one of the two overlapping intervals.

---

### Step 6 — Keep the one ending earlier

```cpp
previousEnd = min(previousEnd, currentEnd);
```

This is the most important line.

It means:

> Keep the interval whose end time is smaller.

---

### Step 7 — No overlap

If:

```cpp
currentStart >= previousEnd
```

there is no conflict.

So the current interval becomes the new interval we're keeping:

```cpp
previousEnd = currentEnd;
```

---

# Complete Logic

```cpp
if (currentStart < previousEnd) {

    // Overlap
    count++;

    // Keep interval with smaller end
    previousEnd = min(previousEnd, currentEnd);

} else {

    // No overlap
    previousEnd = currentEnd;
}
```

---

# Dry Run

Input:

```text
[[1,2],[2,3],[3,4],[1,3]]
```

### Sort

```text
[1,2]
[1,3]
[2,3]
[3,4]
```

---

### Start

```text
previousEnd = 2
count = 0
```

---

### Current = `[1,3]`

```text
currentStart = 1
previousEnd = 2
```

Check:

```cpp
1 < 2
```

✅ Overlap.

Remove one:

```text
count = 1
```

Keep smaller end:

```cpp
previousEnd = min(2,3);
```

Therefore:

```text
previousEnd = 2
```

We keep:

```text
[1,2]
```

---

### Current = `[2,3]`

Check:

```cpp
2 < 2
```

❌ No overlap.

So:

```cpp
previousEnd = 3;
```

---

### Current = `[3,4]`

Check:

```cpp
3 < 3
```

❌ No overlap.

So:

```cpp
previousEnd = 4;
```

---

### Final

```text
count = 1
```

Answer:

```text
1
```

---

# Another Important Example

Consider:

```text
[1,4]
[2,3]
[3,5]
```

Initially:

```text
previousEnd = 4
```

### `[2,3]`

```cpp
2 < 4
```

Overlap.

Remove one:

```text
count = 1
```

Keep smaller end:

```cpp
previousEnd = min(4,3);
```

Therefore:

```text
previousEnd = 3
```

We effectively keep:

```text
[2,3]
```

---

### `[3,5]`

```cpp
3 < 3
```

False.

So they don't overlap.

```text
[2,3]
[3,5]
```

are allowed.

Final:

```text
count = 1
```

---

# C++ Solution

```cpp
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {

        if (intervals.empty())
            return 0;

        sort(intervals.begin(), intervals.end());

        int count = 0;
        int previousEnd = intervals[0][1];

        for (int i = 1; i < intervals.size(); i++) {

            int currentStart = intervals[i][0];
            int currentEnd = intervals[i][1];

            if (currentStart < previousEnd) {

                // Overlap → remove one interval
                count++;

                // Keep the interval that ends earlier
                previousEnd = min(previousEnd, currentEnd);

            } else {

                // No overlap
                previousEnd = currentEnd;
            }
        }

        return count;
    }
};
```

---

# 🧠 Why Does the Greedy Choice Work?

Suppose two intervals overlap:

```text
A = [1,4]
B = [2,3]
```

We must remove one.

If we keep `A`:

```text
ends at 4
```

If we keep `B`:

```text
ends at 3
```

Keeping `B` is always at least as good for future intervals because an interval ending at `3` leaves **more available space** than one ending at `4`.

Therefore:

```text
Overlap
   ↓
Keep smaller end
```

This is the greedy invariant.

---

# Common Mistakes

### ❌ Using `<=`

Wrong:

```cpp
if (currentStart <= previousEnd)
```

For this problem:

```text
[1,2]
[2,3]
```

do **not** overlap.

Correct:

```cpp
if (currentStart < previousEnd)
```

---

### ❌ Keeping the larger end

Wrong:

```cpp
previousEnd = max(previousEnd, currentEnd);
```

That is useful for **Merge Intervals**, not this problem.

Here we want:

```cpp
previousEnd = min(previousEnd, currentEnd);
```

because we want to keep the interval ending earlier.

---

### ❌ Updating with `currentStart`

Wrong:

```cpp
previousEnd = min(previousEnd, currentStart);
```

`previousEnd` must represent an **interval's end**, so compare:

```cpp
previousEnd
```

with:

```cpp
currentEnd
```

---

### ❌ Forgetting to increment `count`

Every overlap means one interval must be removed:

```cpp
count++;
```

---

### ❌ Starting from `i = 0`

We initialize using the first interval:

```cpp
int previousEnd = intervals[0][1];
```

Therefore traversal starts at:

```cpp
for (int i = 1; ...)
```

---

# Pattern Recognition

This is a classic **Greedy Interval Scheduling** problem.

When you see:

> **Remove the minimum number of intervals to eliminate overlap**

Think:

```text
Sort
 ↓
Find overlap
 ↓
Remove one
 ↓
Keep interval with smaller end
 ↓
Continue
```

---

# Comparison With Previous Interval Problems

| Problem                   | Overlap Handling            | Main Tool        |
| ------------------------- | --------------------------- | ---------------- |
| Merge Intervals           | Merge them                  | Sorting          |
| Meeting Rooms II          | Track simultaneous meetings | Min Heap         |
| Non-overlapping Intervals | Remove one                  | Greedy + Sorting |

The important distinction:

```text
Merge Intervals
→ Keep BOTH by merging

Meeting Rooms II
→ Keep BOTH and count rooms

Non-overlapping Intervals
→ Remove ONE
```

---

# Complexity

### Time

Sorting:

```text
O(n log n)
```

Traversal:

```text
O(n)
```

Overall:

```text
O(n log n)
```

### Space

We only use:

```cpp
count
previousEnd
```

So auxiliary space is:

```text
O(1)
```

excluding the sorting implementation.

---

# 🧠 Interview Cheat Sheet

Remember these three things:

### 1. Sort

```cpp
sort(intervals.begin(), intervals.end());
```

### 2. Overlap

```cpp
currentStart < previousEnd
```

### 3. Greedy choice

```cpp
count++;
previousEnd = min(previousEnd, currentEnd);
```

### Mental Model

> **If two intervals overlap, throw away the one that ends later. The earlier-ending interval gives us the most room for everything that comes next.**
