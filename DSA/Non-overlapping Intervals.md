# Non-overlapping Intervals

## Problem Summary

We are given intervals:

```cpp id="m1r8v4"
[start, end]
```

We must remove:

# minimum number of intervals

so that remaining intervals become:

# non-overlapping

---

# What Is Overlapping?

Suppose:

```text id="q7m2k5"
[1,3]
[2,4]
```

These overlap because:

```text id="x4m9r1"
2 < 3
```

meaning second interval starts before first interval ends.

---

# Non-Overlapping Example

```text id="t8m1k6"
[1,2]
[2,3]
```

These do NOT overlap.

Touching is allowed.

---

# MOST Important Insight

Instead of thinking:

```text id="f5m7r2"
which intervals to remove
```

think:

# which intervals to KEEP

Very important greedy mindset.

---

# Goal Becomes

Keep:

# maximum number of non-overlapping intervals

Then:

removed=total-kept

---

# MOST Important Greedy Rule

Always keep interval with:

# smallest ending time

---

# Why?

Earlier ending intervals leave:

```text id="p2m8v4"
maximum space for future intervals
```

This is the core greedy proof.

---

# Example

Suppose:

```text id="v6m1r9"
[1,100]
[2,3]
[3,4]
```

If we keep:

```text id="m4r8k1"
[1,100]
```

almost everything else overlaps.

Bad choice.

---

# Better Choice

Keep:

```text id="q9m2v7"
[2,3]
```

because it ends early.

Then we can also keep:

```text id="x3m7r5"
[3,4]
```

---

# IMPORTANT First Step

Sort intervals by:

# ending value

ascending.

---

# Sorting

```cpp id="f8m1k2"
sort(
    intervals.begin(),
    intervals.end(),
    [](vector<int>& a, vector<int>& b){

        return a[1] < b[1];
    }
);
```

---

# Why `a[1]`?

Because:

```text id="p5m9r1"
index 1 = end value
```

---

# Important Mental Model

After sorting:

```text id="t2m7v4"
first compatible interval is always optimal
```

Classic greedy scheduling proof.

---

# Main Traversal Idea

Maintain:

```cpp id="n8m4r6"
lastEnd
```

representing:

# end value of previously kept interval

---

# Compare With Current Interval

For every interval:

```text id="r4m1k8"
currentStart
```

compare with:

```text id="q7m5v2"
lastEnd
```

---

# Case 1 — No Overlap

If:

currentStart\ge lastEnd

then intervals do NOT overlap.

Keep interval.

Update:

```cpp id="x1m8r4"
lastEnd = currentEnd;
```

---

# Case 2 — Overlap Exists

If:

currentStart<lastEnd

then overlap exists.

Need removal.

```cpp id="m9r2k5"
removed++;
```

---

# Why Remove Current Interval?

Because intervals are already sorted by:

```text id="v5m7r1"
smallest end time
```

The previous interval always ends earlier or equal.

Keeping earlier-ending interval is greedily optimal.

---

# Example Dry Run

Intervals:

```text id="f2m8v6"
[1,2]
[2,3]
[3,4]
[1,3]
```

---

# Step 1 — Sort By End

```text id="t4m1r9"
[1,2]
[2,3]
[1,3]
[3,4]
```

---

# Keep First Interval

```text id="q8m5r2"
[1,2]
```

Initialize:

```text id="x6m1v7"
lastEnd = 2
```

---

# Next Interval

```text id="m3r8k1"
[2,3]
```

Check:

2\ge2

No overlap.

Keep it.

Update:

```text id="p7m2v4"
lastEnd = 3
```

---

# Next Interval

```text id="f4m9r2"
[1,3]
```

Check:

1<3

Overlap exists.

Remove it.

```text id="q1m7k8"
removed = 1
```

---

# Next Interval

```text id="v8m4r1"
[3,4]
```

Check:

3\ge3

No overlap.

Keep it.

---

# Final Answer

```text id="n5m2v7"
1 interval removed
```

---

# Complete Optimal Solution

```cpp id="x2m8r5"
class Solution {
public:

    int eraseOverlapIntervals(
        vector<vector<int>>& intervals
    ) {

        sort(
            intervals.begin(),
            intervals.end(),
            [](vector<int>& a, vector<int>& b){

                return a[1] < b[1];
            }
        );

        int removed = 0;

        int lastEnd = intervals[0][1];

        for(int i = 1; i < intervals.size(); i++){

            int start = intervals[i][0];
            int end = intervals[i][1];

            // No overlap
            if(start >= lastEnd){

                lastEnd = end;
            }

            // Overlap
            else{

                removed++;
            }
        }

        return removed;
    }
};
```

---

# Complexity Analysis

## Sorting

```text id="m6r1k9"
O(n log n)
```

---

# Traversal

```text id="t8m4v2"
O(n)
```

---

# Total Time Complexity

```text id="q3m7r1"
O(n log n)
```

---

# Space Complexity

Sorting only.

```text id="v9m2k4"
O(1)
```

(ignoring sorting implementation stack space)

---

# Important Pattern Learned

This is classic:

# Interval Scheduling Greedy

One of the MOST important greedy patterns.

---

# Similar Problems

* Meeting Rooms
* Meeting Rooms II
* Merge Intervals
* Minimum Number of Arrows to Burst Balloons
* Insert Interval

---

# Common Mistakes

## Mistake 1

Sorting by:

```text id="f1m8r6"
start time
```

instead of:

```text id="p4m7v2"
end time
```

Greedy proof fails otherwise.

---

## Mistake 2

Using:

```cpp id="x7m1r5"
>
```

instead of:

```cpp id="m2r9k8"
>=
```

Touching intervals are allowed.

---

## Mistake 3

Updating `lastEnd` during overlap case.

We should keep:

```text id="q5m8v1"
smaller ending interval
```

which is already previous interval after sorting.

---

# Recognition Pattern

Whenever problem involves:

* intervals
* overlaps
* maximize compatible intervals

think:

# sort by end time

---

# Important Mental Models

## Earlier Finish Is Better

Smaller ending intervals leave more future options.

---

# Greedy Works Because

Locally optimal choice:

```text id="t1m7r4"
smallest ending interval
```

leads to globally optimal solution.

---

# Biggest Takeaway

The heart of interval greedy problems is:

# keeping intervals that finish earliest

to maximize room for future intervals.
