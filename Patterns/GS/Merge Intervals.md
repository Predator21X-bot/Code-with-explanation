# 🟦 LeetCode 56 — Merge Intervals

**Difficulty:** Medium
**Pattern:** Intervals + Sorting + Greedy
**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(n)` for the output

The key idea is to **sort intervals by their start time**, then scan from left to right and merge an interval whenever it overlaps with the last merged interval. ([LeetCode][1])

---

## 📌 Problem

Given intervals:

```cpp
intervals = [[1,3],[2,6],[8,10],[15,18]]
```

Merge all overlapping intervals.

Output:

```cpp
[[1,6],[8,10],[15,18]]
```

Because:

```text
[1,3]
   [2,6]
```

overlap, so they become:

```text
[1,6]
```

Also, `[1,4]` and `[4,5]` **do overlap**, so the condition is `currentStart <= previousEnd`. ([LeetCode][1])

---

# 💡 Core Idea

The most important observation is:

> **Sort intervals by their starting point first.**

Example:

```text
Before:
[8,10]
[1,3]
[15,18]
[2,6]

After sorting:
[1,3]
[2,6]
[8,10]
[15,18]
```

Now we can process them from left to right.

Why?

Because after sorting, the next interval always starts at or after the previous interval's start. This means we only need to compare the current interval with the **last merged interval**. ([NeetCode][2])

---

# ⭐ Two Cases

Suppose our answer currently contains:

```text
[1,6]
```

and the current interval is:

```text
[4,8]
```

### Case 1 — Overlap

```text
currentStart <= previousEnd
```

Here:

```text
4 <= 6
```

So they overlap:

```text
[1---------6]
    [4---------8]
```

Merge them:

```text
[1---------8]
```

Therefore:

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

---

### Case 2 — No overlap

Suppose:

```text
ans.back() = [1,6]
current    = [8,10]
```

Check:

```text
8 <= 6 ❌
```

So:

```text
[1------6]   [8------10]
```

They are separate.

Therefore:

```cpp
ans.push_back(intervals[i]);
```

---

# 🧠 Why `ans.back()`?

This is important.

```cpp
ans.back()
```

means:

> **The last interval currently present in `ans`.**

For example:

```cpp
ans = {
    {1,6},
    {8,10}
};
```

Then:

```cpp
ans.back()
```

is:

```text
[8,10]
```

So:

```cpp
ans.back()[1]
```

is:

```text
10
```

the end of the most recently merged interval.

---

# 🧪 Dry Run

Input:

```text
[[1,3],[2,6],[8,10],[15,18]]
```

Already sorted.

### Start

Push the first interval:

```text
ans = [[1,3]]
```

---

### Current = `[2,6]`

Compare:

```text
currentStart = 2
previousEnd  = 3
```

```text
2 <= 3 ✅
```

Overlap.

Update:

```cpp
ans.back()[1] = max(3,6);
```

So:

```text
ans = [[1,6]]
```

---

### Current = `[8,10]`

Compare:

```text
8 <= 6 ❌
```

No overlap.

Push it:

```text
ans = [
    [1,6],
    [8,10]
]
```

---

### Current = `[15,18]`

Compare:

```text
15 <= 10 ❌
```

No overlap.

Push:

```text
ans = [
    [1,6],
    [8,10],
    [15,18]
]
```

Final answer:

```text
[[1,6],[8,10],[15,18]]
```

---

# 🚨 Why `max()` when merging?

Suppose:

```text
ans.back() = [1,10]
current    = [2,6]
```

They overlap.

We **must not** change the end to `6`.

That would incorrectly shrink the interval:

```text
[1,6] ❌
```

The combined interval should remain:

```text
[1,10]
```

So:

```cpp
ans.back()[1] = max(10,6);
```

gives:

```text
10
```

---

### Another example

```text
ans.back() = [1,5]
current    = [3,10]
```

We need:

```text
[1,10]
```

Therefore:

```cpp
max(5,10) = 10
```

So the rule is:

> **When intervals overlap, keep the same start and extend the end as far as necessary.**

```cpp
ans.back()[1] = max(ans.back()[1], intervals[i][1]);
```

---

# ✅ C++ Solution

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {

        // Sort by starting point
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> ans;

        // Add first interval
        ans.push_back(intervals[0]);

        // Process remaining intervals
        for (int i = 1; i < intervals.size(); i++) {

            // Overlap
            if (intervals[i][0] <= ans.back()[1]) {

                // Extend the current merged interval
                ans.back()[1] =
                    max(ans.back()[1], intervals[i][1]);

            } else {

                // No overlap
                ans.push_back(intervals[i]);
            }
        }

        return ans;
    }
};
```

This is the standard `sort + scan` solution. ([WalkCCC][3])

---

# 🔥 The Part You Should Memorize

```cpp
if (intervals[i][0] <= ans.back()[1]) {
    
    ans.back()[1] =
        max(ans.back()[1], intervals[i][1]);

} else {
    
    ans.push_back(intervals[i]);
}
```

Translate it into English:

```text
Does current start <= previous merged end?
             ↓
          YES
             ↓
          OVERLAP
             ↓
     Extend previous end
```

Otherwise:

```text
          NO
           ↓
      SEPARATE
           ↓
   Add new interval
```

---

# 🧠 Why Do We Compare With `ans.back()` Instead of `intervals[i-1]`?

This is **very important**.

Consider:

```text
[1,10]
[2,3]
[4,5]
```

After processing:

```text
ans = [[1,10]]
```

Now `[4,5]` overlaps with `[1,10]`.

If you only compared with the immediately previous original interval:

```text
[2,3]
```

you would incorrectly think:

```text
4 > 3
```

and conclude there's no overlap.

But `[4,5]` **does** overlap with the merged interval `[1,10]`.

Therefore we compare with:

```cpp
ans.back()
```

because it represents the **entire merged range so far**.

This is one of the most important ideas in this problem.

---

# ⚠️ Common Mistake

Don't write:

```cpp
ans.back()[1] = max(
    ans.back()[1],
    intervals[i][0]
);
```

We need the **current interval's end**:

```cpp
intervals[i][1]
```

So:

```cpp
ans.back()[1] =
    max(ans.back()[1], intervals[i][1]);
```

---

# 🔑 Pattern Recognition

Whenever you see:

> Given intervals and merge overlapping ones.

Immediately think:

```text
SORT BY START
      ↓
SCAN LEFT → RIGHT
      ↓
Compare current START
with merged END
      ↓
   ┌──────┴──────┐
 OVERLAP       NO OVERLAP
   ↓                ↓
 EXTEND            PUSH
```

### Formula

```cpp
currentStart <= mergedEnd
```

→ overlap

```cpp
mergedEnd = max(mergedEnd, currentEnd)
```

→ merge

Otherwise:

```cpp
ans.push_back(current)
```

→ separate interval.

---

# 🎯 Interview Cheat Sheet

### 1. Sort

```cpp
sort(intervals.begin(), intervals.end());
```

### 2. Start answer

```cpp
ans.push_back(intervals[0]);
```

### 3. Check overlap

```cpp
intervals[i][0] <= ans.back()[1]
```

### 4. Merge

```cpp
ans.back()[1] =
    max(ans.back()[1], intervals[i][1]);
```

### 5. Separate

```cpp
ans.push_back(intervals[i]);
```

### Complexity

```text
Sorting → O(n log n)
Scanning → O(n)

Total → O(n log n)
```

---

## 🧠 One-line mental model

> **Sort by start → keep the last merged interval → if the next start is within its end, extend it; otherwise start a new interval.**

This is the same **Merge Intervals** problem you practiced earlier, so the key thing now is to be able to derive the condition and `ans.back()` logic rather than just memorize the code.

[1]: https://leetcode.com/problems/merge-intervals/solution/?utm_source=chatgpt.com "Merge Intervals - LeetCode"
[2]: https://neetcode.io/solutions/merge-intervals?utm_source=chatgpt.com "LeetCode 56 Merge Intervals Solution & Explanation | NeetCode"
[3]: https://walkccc.me/LeetCode/problems/56/?utm_source=chatgpt.com "56. Merge Intervals - LeetCode Solutions"
