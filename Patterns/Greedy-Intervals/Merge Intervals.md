# 🟦 LeetCode 56 - Merge Intervals

**Difficulty:** Medium
**Pattern:** Sorting + Greedy (Intervals)
**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(n)` *(excluding output, sorting may use additional stack space depending on implementation)*

---

# 📌 Problem

Given a collection of intervals

```cpp
[start, end]
```

merge all overlapping intervals and return the resulting list.

---

# Examples

### Example 1

```cpp
Input:

[[1,3],[2,6],[8,10],[15,18]]

Output:

[[1,6],[8,10],[15,18]]
```

Visualization

```text
1------3
   2-----------6

↓

1---------------6
```

---

### Example 2

```cpp
Input:

[[1,4],[4,5]]

Output:

[[1,5]]
```

Even though

```text
4 == 4
```

they are considered overlapping.

---

# 💡 Key Observation

There are only **two possibilities** while processing intervals.

### 1. They overlap

```text
1------3
   2-----------6
```

Merge them.

---

### 2. They don't overlap

```text
1------3

          8------10
```

Keep them separate.

---

# Why do we sort first?

Suppose input is

```cpp
[[8,10],[1,3],[2,6]]
```

Without sorting,

we compare

```text
Current

[8,10]

Next

[1,3]
```

Since

```cpp
1 <= 10
```

you may incorrectly think they overlap.

Sorting gives

```cpp
[[1,3],[2,6],[8,10]]
```

Now every comparison is meaningful.

---

# Sorting

Sort intervals by their starting time.

```cpp
sort(intervals.begin(), intervals.end());
```

After sorting,

```text
Current Start <= Next Start
```

always holds.

---

# Merge Condition

Suppose

```text
Current

[start1, end1]

Next

[start2, end2]
```

They overlap if

```cpp
start2 <= end1
```

or

```cpp
if(intervals[i][0] <= ans.back()[1])
```

---

# How do we merge?

Suppose

```text
Current

[1,6]

Next

[4,9]
```

Merged interval becomes

```text
[1,9]
```

Notice

Start remains

```text
1
```

End becomes

```text
max(6,9)
```

Hence

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

---

# Why don't we update the start?

Because after sorting,

```text
currentStart <= nextStart
```

always.

Therefore,

```text
currentStart
```

is already the minimum start.

Only the end may increase.

---

# Algorithm

### Step 1

Sort intervals.

```cpp
sort(intervals.begin(), intervals.end());
```

---

### Step 2

Create answer vector.

```cpp
vector<vector<int>> ans;
```

---

### Step 3

Insert the first interval.

```cpp
ans.push_back(intervals[0]);
```

This becomes the current merged interval.

---

### Step 4

Traverse remaining intervals.

---

## Case 1 : Overlap

Condition

```cpp
intervals[i][0] <= ans.back()[1]
```

Merge

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

---

## Case 2 : No Overlap

Simply add a new interval.

```cpp
ans.push_back(intervals[i]);
```

---

# Dry Run

Input

```cpp
[[1,3],[2,6],[8,10],[15,18]]
```

Initially

```text
ans = [[1,3]]
```

---

Next

```text
[2,6]
```

Overlap

```cpp
2 <= 3
```

Update

```text
ans = [[1,6]]
```

---

Next

```text
[8,10]
```

No overlap

```cpp
8 > 6
```

Push

```text
ans = [[1,6],[8,10]]
```

---

Next

```text
[15,18]
```

No overlap

Push

```text
ans = [[1,6],[8,10],[15,18]]
```

---

# C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        if (intervals.empty())
            return {};

        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        ans.push_back(intervals[0]);

        for (int i = 1; i < intervals.size(); i++) {

            if (intervals[i][0] <= ans.back()[1]) {

                ans.back()[1] =
                    max(ans.back()[1], intervals[i][1]);

            } else {

                ans.push_back(intervals[i]);
            }
        }

        return ans;
    }
};
```

---

# Complexity Analysis

### Sorting

```text
O(n log n)
```

---

### Traversal

```text
O(n)
```

---

### Overall Time

```text
O(n log n)
```

Sorting dominates the complexity.

---

### Space

Answer vector

```text
O(n)
```

---

# Common Mistakes

### ❌ Forgetting to sort

Without sorting,

the overlap check becomes invalid.

Always sort first.

---

### ❌ Wrong overlap condition

Wrong

```cpp
intervals[i][0] < ans.back()[1]
```

This misses

```text
[1,4]
[4,5]
```

Correct

```cpp
intervals[i][0] <= ans.back()[1]
```

---

### ❌ Updating the whole interval

Wrong

```cpp
ans.back() = max(...);
```

`ans.back()` is an interval (`vector<int>`), while `max(...)` returns an integer.

Correct

```cpp
ans.back()[1] =
max(ans.back()[1], intervals[i][1]);
```

---

### ❌ Updating the start

Wrong

```cpp
ans.back()[0] = ...
```

Never update the start after sorting.

Only update the end.

---

### ❌ Forgetting empty input

Always check

```cpp
if(intervals.empty())
    return {};
```

---

# Pattern Recognition

This belongs to the **Intervals Pattern**.

Whenever you see:

* Merge intervals
* Insert interval
* Meeting Rooms
* Meeting Rooms II
* Employee Free Time
* Non-overlapping intervals

Think

```text
Sort

↓

Compare neighbouring intervals

↓

Merge or Keep Separate
```

---

# Similar Problems

* LeetCode 57 – Insert Interval
* LeetCode 252 – Meeting Rooms
* LeetCode 253 – Meeting Rooms II
* LeetCode 435 – Non-overlapping Intervals
* LeetCode 759 – Employee Free Time

---

# Interview Takeaways

* Always **sort intervals by start time** before merging.
* Compare the **next interval's start** with the **current merged interval's end**.
* If they overlap, extend the end using:

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

* If they don't overlap, push the new interval into the answer.
* Because the intervals are sorted, the **start never changes**—only the **end** may need to be extended.
