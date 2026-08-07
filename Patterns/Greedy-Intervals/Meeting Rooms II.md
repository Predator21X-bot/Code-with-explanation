# 🟦 LeetCode 253 - Meeting Rooms II

**Difficulty:** Medium
**Pattern:** Sorting + Greedy + Min Heap (Priority Queue)
**Time Complexity:** `O(n log n)`
**Space Complexity:** `O(n)`

---

# 📌 Problem

Given a list of meeting intervals

```cpp
[start, end]
```

Return the **minimum number of meeting rooms** required so that all meetings can take place without conflicts.

---

# Examples

### Example 1

```cpp
Input:

[[0,30],[5,10],[15,20]]

Output:

2
```

Visualization

```text
Room 1

0----------------30

Room 2

     5------10

Room 2 (reused)

            15------20
```

Need **2 rooms**.

---

### Example 2

```cpp
Input:

[[7,10],[2,4]]

Output:

1
```

Visualization

```text
2----4


        7----10
```

No overlap.

Only **1 room**.

---

# 💡 Key Observation

Unlike **Merge Intervals**, we **do not merge meetings**.

Instead, we ask:

> **Can the current meeting reuse an existing room?**

To answer this, we only need to know:

> **Which meeting ends the earliest?**

---

# Why Sort?

Sort meetings by **start time**.

```cpp
sort(intervals.begin(), intervals.end());
```

After sorting,

```text
Every next meeting starts at the same or a later time.
```

Now we process meetings from left to right.

---

# Why Min Heap?

Suppose the current rooms have meetings ending at

```text
30
10
25
40
18
```

Which room should we check first?

The room ending at

```text
10
```

because it becomes free the earliest.

A **Min Heap** always gives the smallest end time.

```cpp
priority_queue<
    int,
    vector<int>,
    greater<int>
> pq;
```

---

# What does the Heap store?

The heap stores only

```cpp
meeting end times
```

Not

```cpp
[start,end]
```

Why?

Because to reuse a room, we only compare

```cpp
Current meeting start
```

with

```cpp
Earliest meeting end
```

The meeting start time of an occupied room is no longer useful.

---

# Algorithm

### Step 1

Sort meetings.

```cpp
sort(intervals.begin(), intervals.end());
```

---

### Step 2

Create a Min Heap.

```cpp
priority_queue<int, vector<int>, greater<int>> pq;
```

---

### Step 3

Insert the first meeting's end time.

```cpp
pq.push(intervals[0][1]);
```

One room is occupied.

---

### Step 4

Process remaining meetings.

Current meeting

```cpp
start = intervals[i][0];

end = intervals[i][1];
```

---

## Case 1 : Room Available

If

```cpp
start >= pq.top()
```

Example

```text
Current Meeting

15------20

Earliest room ends

10
```

Since

```cpp
15 >= 10
```

the room is free.

Reuse it.

```cpp
pq.pop();
```

---

## Case 2 : Allocate / Reuse Room

Regardless of whether we reused a room or allocated a new one,

the current meeting occupies a room until

```cpp
end
```

So

```cpp
pq.push(end);
```

Notice

```cpp
pq.push(end);
```

is **outside** the `if`.

```cpp
if (start >= pq.top()) {
    pq.pop();
}

pq.push(end);
```

Reason

* If a room was free → we reused it.
* Otherwise → we allocated a new room.

In both cases, the current meeting occupies a room until `end`.

---

# Dry Run

Input

```cpp
[[0,30],[5,10],[15,20]]
```

After sorting

```text
[[0,30],[5,10],[15,20]]
```

---

### Meeting 1

```text
[0,30]
```

Heap

```text
30
```

Rooms

```text
1
```

---

### Meeting 2

```text
[5,10]
```

Check

```cpp
5 >= 30
```

False

Need another room.

Push

```cpp
10
```

Heap

```text
10
30
```

Rooms

```text
2
```

---

### Meeting 3

```text
[15,20]
```

Check

```cpp
15 >= 10
```

True

Room becomes free.

```cpp
pq.pop();
```

Heap

```text
30
```

Current meeting now occupies that room.

```cpp
pq.push(20);
```

Heap

```text
20
30
```

Rooms

```text
2
```

Return

```cpp
pq.size()

= 2
```

---

# Why do we return `pq.size()`?

Each element in the heap represents

> **One room currently occupied.**

Heap

```text
20
30
```

means

```text
Room 1 → occupied till 20

Room 2 → occupied till 30
```

Heap size

```text
2
```

equals the number of rooms required.

---

# Why not maintain a separate room counter?

The heap already tracks it.

Every

```cpp
pq.push(...)
```

means

```text
One room is occupied.
```

Every

```cpp
pq.pop()
```

means

```text
One room became free.
```

Therefore

```cpp
pq.size()
```

always equals the number of occupied rooms.

---

# C++ Solution

```cpp
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {

        if (intervals.empty())
            return 0;

        sort(intervals.begin(), intervals.end());

        priority_queue<int, vector<int>, greater<int>> pq;

        // First meeting occupies one room
        pq.push(intervals[0][1]);

        // Process remaining meetings
        for (int i = 1; i < intervals.size(); i++) {

            int start = intervals[i][0];
            int end = intervals[i][1];

            // Earliest room becomes free
            if (start >= pq.top()) {
                pq.pop();
            }

            // Current meeting occupies a room
            pq.push(end);
        }

        return pq.size();
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

### Heap Operations

For each meeting

* Push → `O(log n)`
* Pop → `O(log n)` (if needed)

Total

```text
O(n log n)
```

---

### Overall Time Complexity

```text
O(n log n)
```

---

### Space Complexity

Heap stores at most

```text
n
```

meeting end times.

```text
O(n)
```

---

# Common Mistakes

### ❌ Forgetting to sort

Always sort by **start time** before processing.

---

### ❌ Using Max Heap

Wrong

```cpp
priority_queue<int>
```

Need

```cpp
priority_queue<int,
               vector<int>,
               greater<int>>
```

because we need the **earliest ending meeting**.

---

### ❌ Storing whole intervals in heap

Wrong

```cpp
[start,end]
```

We only need

```cpp
end
```

---

### ❌ Writing

```cpp
if(start > pq.top())
```

Wrong.

Correct

```cpp
if(start >= pq.top())
```

because

```text
Meeting ends at 10

Next starts at 10
```

can reuse the same room.

---

### ❌ Putting

```cpp
pq.push(end);
```

inside the `if`

Wrong.

Current meeting always occupies a room.

Push must happen every iteration.

---

### ❌ Returning

```cpp
pq.top()
```

Wrong.

Need

```cpp
pq.size()
```

---

# Pattern Recognition

This belongs to the **Intervals + Min Heap** pattern.

Whenever the problem asks:

* Minimum meeting rooms
* Number of resources needed
* Earliest available resource
* Scheduling with overlaps

Think

```text
Sort

↓

Min Heap

↓

Track earliest finishing resource
```

---

# Difference from Merge Intervals

| Merge Intervals             | Meeting Rooms II                    |
| --------------------------- | ----------------------------------- |
| Merge overlapping intervals | Count overlapping meetings          |
| One current merged interval | Multiple active meetings            |
| Update merged interval      | Track end time of every active room |
| Greedy + Sorting            | Greedy + Sorting + Min Heap         |

---

# Similar Problems

* LeetCode 252 – Meeting Rooms
* LeetCode 56 – Merge Intervals
* LeetCode 57 – Insert Interval
* LeetCode 435 – Non-overlapping Intervals
* LeetCode 759 – Employee Free Time
* LeetCode 2402 – Meeting Rooms III

---

# Interview Takeaways

* Sort meetings by **start time**.
* Use a **Min Heap** to store **meeting end times**.
* Compare the current meeting's **start** with the **smallest end time**.
* If the earliest room is free (`start >= pq.top()`), pop it and reuse that room.
* Always push the current meeting's end time because the meeting occupies a room.
* The heap size at the end represents the **minimum number of meeting rooms required**.
