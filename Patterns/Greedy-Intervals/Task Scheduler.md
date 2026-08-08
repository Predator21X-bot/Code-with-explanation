# 🟦 LeetCode 621 — Task Scheduler

**Difficulty:** Medium
**Pattern:** Greedy + Max Heap + Queue
**Time Complexity:** `O(n log 26)` → effectively `O(n)` because there are only 26 task types
**Space Complexity:** `O(26)` → effectively `O(1)`

---

# 📌 Problem

Given a list of tasks represented by capital letters:

```cpp
tasks = ['A','A','A','B','B','B']
```

Each task takes exactly **1 unit of time**.

There is a non-negative integer `n` representing the **cooldown period**.

After executing a task, the **same task cannot be executed again for the next `n` intervals**.

Return the **minimum time intervals** required to execute all tasks.

---

# Example

```text
tasks = [A,A,A,B,B,B]
n = 2
```

We cannot do:

```text
A A ❌
```

because `A` needs a cooldown of 2.

Instead:

```text
Time:  1  2  3  4  5  6  7  8
Task:  A  B  _  A  B  _  A  B
```

Answer:

```text
8
```

`_` represents an idle interval.

---

# 💡 Key Insight

The most frequent tasks are the most difficult to schedule.

For example:

```text
A → 4
B → 2
C → 1
```

`A` appears most frequently, so we should prioritize it.

This leads to:

> **Always execute the currently available task with the highest remaining frequency.**

This naturally suggests a:

```cpp
priority_queue<int>
```

which is a **Max Heap**.

---

# Why Max Heap?

Suppose:

```text
A → 4
B → 3
C → 2
```

We want to execute:

```text
A
```

first because it has the highest frequency.

After executing A:

```text
A → 3
B → 3
C → 2
```

But A is now in cooldown, so we choose another available task.

The Max Heap always gives us the task with the highest remaining frequency.

---

# ⭐ We Need Two Data Structures

We need to track two categories of tasks.

### 1. Max Heap

Contains:

> **Currently available tasks**

```text
Max Heap
   ↓
Highest-frequency available task
```

---

### 2. Queue

Contains:

> **Tasks currently in cooldown**

Each queue element stores:

```text
(remaining frequency, time when available)
```

For example:

```cpp
{2, 5}
```

means:

```text
Task still has 2 occurrences remaining
Task becomes available at time 5
```

---

# Overall Flow

```text
             Available Tasks
                    ↓
                Max Heap
                    ↓
          Pick highest frequency
                    ↓
                 Execute
                    ↓
            Task still remaining?
               /           \
             Yes            No
              ↓
        Cooldown Queue
              ↓
       Cooldown complete
              ↓
          Max Heap again
```

---

# Step 1 — Count Frequencies

There are only 26 uppercase letters.

```cpp
vector<int> freq(26, 0);

for (char task : tasks) {
    freq[task - 'A']++;
}
```

Example:

```text
tasks = [A,A,A,B,B,B]
```

Frequency:

```text
A → 3
B → 3
```

---

# Step 2 — Create Max Heap

```cpp
priority_queue<int> pq;
```

Push every non-zero frequency:

```cpp
for (int f : freq) {
    if (f > 0)
        pq.push(f);
}
```

Heap conceptually:

```text
3
3
```

We only store the **frequency**, not the task character itself.

Why?

Because for scheduling, we only need to know:

> How many occurrences of this task are still remaining?

---

# Step 3 — Cooldown Queue

```cpp
queue<pair<int, int>> q;
```

Each element:

```text
{remaining frequency, available time}
```

Example:

```text
{2,4}
```

means:

```text
2 occurrences remaining
Available again at time 4
```

---

# Step 4 — Track Time

```cpp
int time = 0;
```

Each iteration represents one unit of time.

```cpp
time++;
```

---

# Step 5 — Execute Highest-Frequency Available Task

If the Max Heap isn't empty:

```cpp
if (!pq.empty()) {
```

Take the highest frequency:

```cpp
int remaining = pq.top();
pq.pop();
```

Execute one occurrence:

```cpp
remaining--;
```

---

# Step 6 — Put Task Into Cooldown

If the task still has occurrences remaining:

```cpp
if (remaining > 0) {
    q.push({remaining, time + n});
}
```

The task is temporarily unavailable.

---

# Step 7 — Bring Task Back From Cooldown

If the cooldown has finished:

```cpp
if (!q.empty() && q.front().second == time) {
    pq.push(q.front().first);
    q.pop();
}
```

Now the task becomes available again.

So it goes back into the Max Heap.

---

# Complete Algorithm

```text
1. Count frequency of each task.
2. Put frequencies into a Max Heap.
3. Create a cooldown Queue.
4. Increase time one unit at a time.
5. Pick the highest-frequency available task.
6. Execute it.
7. If it still has occurrences, put it into cooldown.
8. When cooldown finishes, move it back to the Max Heap.
9. Continue until both Heap and Queue are empty.
10. Return time.
```

---

# 🧪 Dry Run

Input:

```text
tasks = [A,A,A,B,B,B]
n = 2
```

Initial:

```text
Max Heap:

3
3
```

---

### Time 1

Take A:

```text
A → 3 → 2
```

A goes into cooldown.

```text
Queue:

{2,3}
```

---

### Time 2

A is unavailable.

Take B:

```text
B → 3 → 2
```

Queue:

```text
{2,3}
{2,4}
```

---

### Time 3

A becomes available.

Execute A:

```text
A → 2 → 1
```

---

### Time 4

B becomes available.

Execute B:

```text
B → 2 → 1
```

---

Eventually:

```text
Time:  1 2 3 4 5 6 7 8
Task:  A B A B A B _ _
```

Depending on the exact scheduling state, the total minimum is:

```text
8
```

The key idea is that **idle time is only necessary when there are no other available tasks to execute while the most frequent tasks are cooling down.**

---

# C++ Solution

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {

        // Count frequency of each task
        vector<int> freq(26, 0);

        for (char task : tasks) {
            freq[task - 'A']++;
        }

        // Max Heap: highest frequency first
        priority_queue<int> pq;

        for (int f : freq) {
            if (f > 0)
                pq.push(f);
        }

        // {remaining frequency, time when task becomes available}
        queue<pair<int, int>> q;

        int time = 0;

        while (!pq.empty() || !q.empty()) {

            time++;

            // Execute highest-frequency available task
            if (!pq.empty()) {

                int remaining = pq.top();
                pq.pop();

                remaining--;

                // Put task into cooldown
                if (remaining > 0) {
                    q.push({remaining, time + n});
                }
            }

            // Task finished cooldown
            if (!q.empty() && q.front().second == time) {
                pq.push(q.front().first);
                q.pop();
            }
        }

        return time;
    }
};
```

---

# ⭐ Important Timing Concept

If:

```text
n = 2
```

and we execute:

```text
A at time 1
```

then:

```text
Time 1 → A
Time 2 → cooldown
Time 3 → cooldown
Time 4 → A
```

So the next execution is:

```text
1 + n + 1 = 4
```

The cooldown queue therefore tracks when the task becomes available again.

---

# Why Use a Queue?

Tasks enter cooldown in chronological order.

If:

```text
Task A → available at 5
Task B → available at 7
Task C → available at 9
```

they naturally leave cooldown in the same order:

```text
5 → 7 → 9
```

Therefore a normal queue is sufficient.

---

# Why Don't We Put a Cooling Task Directly Back Into the Heap?

Suppose:

```text
A → 3 remaining
```

We execute A.

Now:

```text
A → 2 remaining
```

But A cannot immediately be selected again.

If we put it directly back into the Max Heap:

```text
A → 2
```

we might execute:

```text
A A
```

which violates the cooldown.

Therefore:

```text
Execute
   ↓
Cooldown Queue
   ↓
Cooldown finished
   ↓
Max Heap
```

---

# Common Mistakes

### ❌ Using a Min Heap

We want the task with the **highest frequency**.

Therefore:

```cpp
priority_queue<int>
```

not a min heap.

---

### ❌ Immediately reinserting the task

Wrong:

```cpp
pq.push(remaining);
```

immediately after execution.

The task must first enter cooldown.

---

### ❌ Forgetting idle time

If:

```text
No available task
```

we still need to advance:

```cpp
time++;
```

That represents an idle interval.

---

### ❌ Storing the task character unnecessarily

We can solve the problem using frequencies only:

```cpp
priority_queue<int> pq;
```

because the identity of the task doesn't matter once its frequency is known.

---

# Complexity

There are only **26 possible task types**.

Frequency counting:

```text
O(n)
```

Heap operations involve at most 26 elements:

```text
O(log 26)
```

Therefore overall:

```text
O(n log 26)
```

which is effectively:

```text
O(n)
```

Space:

```text
O(26)
```

which is effectively:

```text
O(1)
```

---

# 🧠 Pattern Recognition

When you see:

> Tasks + cooldown + minimize total execution time

Think:

```text
Frequency
    ↓
Max Heap
    ↓
Highest-frequency available task
    ↓
Cooldown Queue
    ↓
Available again
    ↓
Max Heap
```

---

# 🔑 Interview Cheat Sheet

Remember these three ideas:

### 1. Highest frequency first

```cpp
priority_queue<int> pq;
```

### 2. Cooling tasks cannot immediately return

```cpp
q.push({remaining, availableTime});
```

### 3. Move them back when cooldown finishes

```cpp
pq.push(q.front().first);
```

### Mental Model

> **Use the most frequent available task first, temporarily remove it during cooldown, and use other available tasks to fill that cooldown. If nothing is available, the CPU stays idle.**

---

## 🔥 Greedy Progression You've Covered

You now have a really useful set of greedy patterns:

```text
Jump Game
→ Track the furthest reachable goal

Merge Intervals
→ Merge overlapping ranges

Meeting Rooms II
→ Min Heap of earliest ending meetings

Non-overlapping Intervals
→ Keep the interval with the earliest end

Task Scheduler
→ Max Heap of highest-frequency available tasks
```

The important part is that these aren't five unrelated tricks — you're learning to identify **what information must be prioritized to make the greedy choice**.
