# Minimum Number of Arrows to Burst Balloons

## Problem Summary

Each balloon is represented by:

```cpp id="m1r8v4"
[start, end]
```

meaning balloon exists across that x-axis interval.

---

# Arrow Rule

If we shoot an arrow at position:

```text id="q7m2k5"
x
```

then ALL balloons satisfying:

start\le x\le end

will burst.

---

# Goal

Find:

# minimum number of arrows

needed to burst ALL balloons.

---

# MOST Important Insight

If intervals overlap:

# one arrow can burst all overlapping balloons

Very important greedy observation.

---

# Example

Suppose balloons:

```text id="x4m9r1"
[1,6]
[2,8]
[7,12]
```

---

# First Two Overlap

Common region:

```text id="t8m1k6"
2 → 6
```

Shoot one arrow anywhere inside overlap.

Both balloons burst.

---

# Third Balloon

Does not overlap.

Needs another arrow.

Total:

```text id="f5m7r2"
2 arrows
```

---

# MOST Important Greedy Idea

Always shoot arrow at:

# smallest possible ending point

Exactly same greedy philosophy as interval scheduling.

---

# Why?

Earlier ending position leaves:

```text id="p2m8v4"
maximum chance of hitting future balloons
```

---

# IMPORTANT First Step

Sort balloons by:

# ending coordinate

ascending.

---

# Sorting

```cpp id="v6m1r9"
sort(
    points.begin(),
    points.end(),
    [](vector<int>& a, vector<int>& b){

        return a[1] < b[1];
    }
);
```

---

# Why `a[1]`?

Because:

```text id="m4r8k1"
index 1 = interval end
```

---

# Important Mental Model

After sorting:

```text id="q9m2v7"
earliest ending balloon is best greedy choice
```

---

# Main Greedy Logic

Shoot first arrow at:

```text id="x3m7r5"
end of first balloon
```

Maintain:

```cpp id="f8m1k2"
arrowPos
```

representing:

# current arrow position

---

# Another Important Variable

Maintain:

```cpp id="p5m9r1"
arrows
```

representing:

# total arrows used

---

# Initialization

First balloon always needs one arrow.

```cpp id="t2m7v4"
int arrows = 1;
```

Shoot at:

```cpp id="n8m4r6"
points[0][1]
```

So:

```cpp id="r4m1k8"
int arrowPos = points[0][1];
```

---

# Traversing Remaining Balloons

For every balloon:

```text id="q7m5v2"
[start, end]
```

compare:

```text id="x1m8r4"
start
```

with:

```text id="m9r2k5"
arrowPos
```

---

# Case 1 — Current Arrow Can Burst Balloon

If:

start\le arrowPos

then current balloon overlaps current arrow position.

No new arrow needed.

---

# Why?

Arrow already lies inside balloon interval.

So balloon bursts automatically.

---

# Case 2 — Balloon Cannot Be Reached

If:

start>arrowPos

then current arrow cannot burst this balloon.

Need NEW arrow.

---

# Update

```cpp id="v5m7r1"
arrows++;
```

Move arrow to:

```cpp id="f2m8v6"
current end
```

So:

```cpp id="t4m1r9"
arrowPos = end;
```

---

# Example Dry Run

Balloons:

```text id="q8m5r2"
[10,16]
[2,8]
[1,6]
[7,12]
```

---

# Step 1 — Sort By End

```text id="x6m1v7"
[1,6]
[2,8]
[7,12]
[10,16]
```

---

# Step 2 — First Arrow

Shoot at:

```text id="m3r8k1"
6
```

```text id="p7m2v4"
arrows = 1
```

---

# Next Balloon

```text id="f4m9r2"
[2,8]
```

Check:

2\le6

Already burst.

---

# Next Balloon

```text id="q1m7k8"
[7,12]
```

Check:

7>6

Need new arrow.

```text id="v8m4r1"
arrows = 2
```

Move arrow to:

```text id="n5m2v7"
12
```

---

# Next Balloon

```text id="x2m8r5"
[10,16]
```

Check:

10\le12

Already burst.

---

# Final Answer

```text id="m6r1k9"
2 arrows
```

---

# Complete Optimal Solution

```cpp id="t8m4v2"
class Solution {
public:

    int findMinArrowShots(
        vector<vector<int>>& points
    ) {

        sort(
            points.begin(),
            points.end(),
            [](vector<int>& a, vector<int>& b){

                return a[1] < b[1];
            }
        );

        int arrows = 1;

        int arrowPos = points[0][1];

        for(int i = 1; i < points.size(); i++){

            int start = points[i][0];
            int end = points[i][1];

            // Need new arrow
            if(start > arrowPos){

                arrows++;

                arrowPos = end;
            }
        }

        return arrows;
    }
};
```

---

# Complexity Analysis

## Sorting

```text id="q3m7r1"
O(n log n)
```

---

# Traversal

```text id="v9m2k4"
O(n)
```

---

# Total Time Complexity

```text id="f1m8r6"
O(n log n)
```

---

# Space Complexity

```text id="p4m7v2"
O(1)
```

(ignoring sorting stack space)

---

# Important Pattern Learned

This is another classic:

# Interval Greedy Problem

Very important interview category.

---

# Similar Problems

* Non-overlapping Intervals
* Meeting Rooms
* Merge Intervals
* Insert Interval
* Meeting Rooms II

---

# Common Mistakes

## Mistake 1

Sorting by:

```text id="x7m1r5"
start time
```

instead of:

```text id="m2r9k8"
end time
```

Greedy proof depends on smallest end first.

---

## Mistake 2

Using:

```cpp id="q5m8v1"
>=
```

instead of:

```cpp id="t1m7r4"
>
```

Remember:

If:

start=arrowPos

current arrow STILL bursts balloon.

---

## Mistake 3

Updating arrow position unnecessarily.

Arrow moves ONLY when:

```text id="f8m2k5"
new non-overlapping balloon appears
```

---

# Recognition Pattern

Whenever problem involves:

* overlapping intervals
* minimum resources/groups
* interval coverage

think:

# sort by end time

---

# Important Mental Models

## One Arrow Represents

```text id="v4m9r1"
one overlapping interval group
```

---

# Greedy Goal

Maximize balloons burst per arrow.

---

# Earlier Ending Is Better

Smaller ending intervals maximize future overlap potential.

---

# Biggest Takeaway

The heart of interval greedy problems is:

# choosing the earliest finishing interval/resource

to maximize future flexibility.
