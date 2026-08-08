# 🟦 LeetCode 55 - Jump Game

**Difficulty:** Medium
**Pattern:** Greedy
**Time Complexity:** `O(n)`
**Space Complexity:** `O(1)`

---

# 📌 Problem

Given an integer array:

```cpp
nums[i]
```

where `nums[i]` represents the **maximum number of steps** you can jump forward from index `i`.

Return:

```cpp
true
```

if you can reach the **last index**, otherwise return:

```cpp
false
```

---

# Examples

### Example 1

```text
nums = [2,3,1,1,4]

Output = true
```

Possible path:

```text
0 → 1 → 4
```

Because:

```text
nums[0] = 2
nums[1] = 3
```

From index `1`, we can reach index `4`.

---

### Example 2

```text
nums = [3,2,1,0,4]

Output = false
```

Eventually we reach:

```text
index = 3
nums[3] = 0
```

We cannot move beyond index `3`, so index `4` cannot be reached.

---

# 💡 Key Insight

Instead of thinking:

> **"From the current index, where can I jump?"**

we think backwards:

> **"Which index can reach my current goal?"**

Start with the last index as the goal.

```cpp
int goal = nums.size() - 1;
```

Then scan from **right to left**.

---

# Greedy Idea

Suppose:

```text
nums = [2,3,1,1,4]

Index:  0  1  2  3  4
Value:  2  3  1  1  4
                         ↑
                       Goal
```

Initially:

```text
goal = 4
```

Check index `3`:

```cpp
3 + nums[3]
= 3 + 1
= 4
```

So index `3` can reach the goal.

Update:

```cpp
goal = 3;
```

Now the problem becomes:

> Can we reach index `3`?

---

Check index `2`:

```cpp
2 + nums[2]
= 2 + 1
= 3
```

It can reach the goal.

```cpp
goal = 2;
```

---

Check index `1`:

```cpp
1 + nums[1]
= 1 + 3
= 4
```

Since:

```cpp
4 >= 2
```

it can reach the goal.

```cpp
goal = 1;
```

---

Check index `0`:

```cpp
0 + nums[0]
= 0 + 2
= 2
```

Since:

```cpp
2 >= 1
```

it can reach the goal.

```cpp
goal = 0;
```

Now:

```cpp
goal == 0
```

Therefore, the last index is reachable.

---

# ⭐ Greedy Condition

At every index `i`, check:

```cpp
i + nums[i] >= goal
```

If true:

```cpp
goal = i;
```

Meaning:

> **Index `i` can reach my current goal, so I can make `i` my new goal.**

---

# Why Don't We Return False Immediately?

Suppose:

```text
[2,3,1,1,4]
```

Maybe one index cannot reach the current goal, but an earlier index might.

Therefore:

```cpp
if (i + nums[i] >= goal)
    goal = i;
```

If the condition is false, we simply continue checking earlier indices.

We only know the final answer after scanning the entire array.

---

# Important Clarification

We're scanning **backwards**, but the jumps themselves are always **forward**.

For example:

```text
goal = 4

Check index 3:

Can index 3 jump FORWARD to 4?
```

We are not actually jumping from `4 → 3`.

We're simply examining potential starting points from right to left.

The condition remains:

```cpp
i + nums[i] >= goal
```

---

# Algorithm

### Step 1

Set the last index as the initial goal.

```cpp
int goal = nums.size() - 1;
```

### Step 2

Traverse from the second-last index toward index `0`.

```cpp
for (int i = nums.size() - 2; i >= 0; i--)
```

### Step 3

Check whether index `i` can reach the current goal.

```cpp
if (i + nums[i] >= goal)
```

### Step 4

If yes, move the goal.

```cpp
goal = i;
```

### Step 5

At the end:

```cpp
return goal == 0;
```

If the goal reaches `0`, there is a valid path from the starting index to the last index.

---

# C++ Solution

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {

        int goal = nums.size() - 1;

        for (int i = nums.size() - 2; i >= 0; i--) {

            if (i + nums[i] >= goal) {
                goal = i;
            }
        }

        return goal == 0;
    }
};
```

---

# Dry Run - `true`

```text
nums = [2,3,1,1,4]
```

| `i` | `nums[i]` | `i + nums[i]` | `goal` before | Can reach? | New goal |
| --: | --------: | ------------: | ------------: | :--------: | -------: |
|   3 |         1 |             4 |             4 |      ✅     |        3 |
|   2 |         1 |             3 |             3 |      ✅     |        2 |
|   1 |         3 |             4 |             2 |      ✅     |        1 |
|   0 |         2 |             2 |             1 |      ✅     |        0 |

Final:

```cpp
goal == 0
```

Answer:

```text
true
```

---

# Dry Run - `false`

```text
nums = [3,2,1,0,4]
```

Initial:

```cpp
goal = 4;
```

| `i` | `nums[i]` | `i + nums[i]` | `goal` | Can reach? |
| --: | --------: | ------------: | -----: | :--------: |
|   3 |         0 |             3 |      4 |      ❌     |
|   2 |         1 |             3 |      4 |      ❌     |
|   1 |         2 |             3 |      4 |      ❌     |
|   0 |         3 |             3 |      4 |      ❌     |

Goal never moves:

```cpp
goal = 4;
```

Therefore:

```cpp
goal != 0
```

Answer:

```text
false
```

---

# Common Mistakes

### ❌ Moving the goal by one

Wrong:

```cpp
goal--;
```

Correct:

```cpp
goal = i;
```

Why?

Because we found a **specific index `i`** that can reach the goal.

---

### ❌ Returning false when one index fails

Wrong:

```cpp
if (i + nums[i] < goal)
    return false;
```

A particular index failing does **not** mean the entire problem is impossible.

Keep checking earlier indices.

---

### ❌ Thinking we're jumping backwards

We're only **scanning backwards**.

The actual jump being checked is:

```text
i → goal
```

which is forward.

---

### ❌ Using DP unnecessarily

A DP solution is possible, but unnecessary.

We only need to know:

> **Can some earlier index reach the current goal?**

That can be maintained with one variable.

---

# Complexity

### Time

We scan the array once:

```text
O(n)
```

### Space

Only one variable:

```text
O(1)
```

---

# Pattern Recognition

This is a **Greedy** problem.

The greedy invariant is:

> `goal` represents the **leftmost index currently known to be able to reach the last index**.

Every time we find:

```cpp
i + nums[i] >= goal
```

we move the goal left:

```cpp
goal = i;
```

At the end:

```cpp
goal == 0
```

means the starting position can reach the destination.

---

# 🧠 Interview Memory Trick

Don't memorize the code.

Remember these **3 lines**:

```cpp
int goal = nums.size() - 1;

if (i + nums[i] >= goal)
    goal = i;

return goal == 0;
```

Mental model:

```text
Last index
    ↓
Who can reach it?
    ↓
Make that index the new goal
    ↓
Who can reach the new goal?
    ↓
Keep moving goal left
    ↓
Did goal reach 0?
```

**If yes → `true`**
**If no → `false`**

---

# Related Problems

* LeetCode 45 — Jump Game II
* LeetCode 1306 — Jump Game III
* LeetCode 1345 — Jump Game IV
* LeetCode 1871 — Jump Game VII

The most important follow-up is **Jump Game II**, where instead of asking *"Can I reach the end?"*, we ask for the **minimum number of jumps**.
