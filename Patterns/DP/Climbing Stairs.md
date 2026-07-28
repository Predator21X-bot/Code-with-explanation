# LeetCode 70. Climbing Stairs

**Difficulty:** Easy  
**Pattern:** 1D Dynamic Programming → Fibonacci Pattern → Space Optimization

---

# Problem

You are climbing a staircase.

- Each time you can climb either **1 step** or **2 steps**.
- Return the total number of distinct ways to reach the top.

### Example

```
Input: n = 4

Output: 5
```

Ways:

```
1 + 1 + 1 + 1
1 + 1 + 2
1 + 2 + 1
2 + 1 + 1
2 + 2
```

---

# Intuition

To reach the **ith** stair, the last move could only be:

- A **1-step jump** from stair **i-1**
- A **2-step jump** from stair **i-2**

These are the only possibilities.

Therefore,

```
Ways(i)
=
Ways(i-1) + Ways(i-2)
```

This is exactly the Fibonacci recurrence.

---

# DP State

```
dp[i]
```

represents

> Number of ways to reach the **ith** stair.

---

# Recurrence Relation

```
dp[i] = dp[i-1] + dp[i-2]
```

### Why?

To reach stair `i`:

Case 1

```
(i-1) ---> i
      +1
```

Number of ways = `dp[i-1]`

Case 2

```
(i-2) -----> i
       +2
```

Number of ways = `dp[i-2]`

Since these two cases are mutually exclusive,

```
dp[i] = dp[i-1] + dp[i-2]
```

---

# Base Cases

```
dp[1] = 1
```

Only one way

```
1
```

---

```
dp[2] = 2
```

Ways are

```
1 + 1

2
```

---

# Bottom-Up DP

### Algorithm

1. Handle base cases.
2. Create DP array.
3. Fill using recurrence.
4. Return last value.

---

# Dry Run

```
n = 5
```

Initial

```
dp[1] = 1
dp[2] = 2
```

```
i = 3

dp[3] = dp[2] + dp[1]
      = 2 + 1
      = 3
```

```
i = 4

dp[4] = dp[3] + dp[2]
      = 3 + 2
      = 5
```

```
i = 5

dp[5] = dp[4] + dp[3]
      = 5 + 3
      = 8
```

Final DP

```
Index

1 2 3 4 5

Value

1 2 3 5 8
```

---

# DP Solution

```cpp
class Solution {
public:
    int climbStairs(int n) {

        if (n <= 2)
            return n;

        vector<int> dp(n + 1);

        dp[1] = 1;
        dp[2] = 2;

        for (int i = 3; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
};
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(n)
```

---

# Can We Optimize Space?

Observe the recurrence

```
dp[i] = dp[i-1] + dp[i-2]
```

At every iteration we only use

```
dp[i-1]

dp[i-2]
```

Older values are never used again.

So instead of storing the entire DP array, we only store the last two values.

---

# Space Optimized Solution

Maintain

```
prev2 = dp[i-2]

prev1 = dp[i-1]
```

Compute

```
current = prev1 + prev2
```

Update

```
prev2 = prev1

prev1 = current
```

Repeat.

---

# Dry Run (Optimized)

Initially

```
prev2 = 1

prev1 = 2
```

```
i = 3

current = 2 + 1 = 3

prev2 = 2
prev1 = 3
```

```
i = 4

current = 3 + 2 = 5

prev2 = 3
prev1 = 5
```

```
i = 5

current = 5 + 3 = 8

prev2 = 5
prev1 = 8
```

Answer

```
prev1 = 8
```

---

# Optimized Code

```cpp
class Solution {
public:
    int climbStairs(int n) {

        if (n <= 2)
            return n;

        int prev2 = 1;
        int prev1 = 2;

        for (int i = 3; i <= n; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
};
```

---

# Complexity (Optimized)

Time

```
O(n)
```

Space

```
O(1)
```

---

# Interview Follow-ups

### Why `vector<int> dp(n + 1)`?

Because we want

```
dp[1]
...
dp[n]
```

If the vector size is `n`, the valid indices are

```
0 ... n-1
```

and `dp[n]` would be out of bounds.

Using `n + 1` keeps the indexing intuitive:

```
dp[i]
```

means

> Ways to reach the ith stair.

---

### Why `if (n <= 2)`?

The answers for

```
n = 1
n = 2
```

are already known.

Returning them directly avoids creating the DP array unnecessarily.

---

# Pattern Recognition

Whenever you see

- Number of ways
- Count possible paths
- Current answer depends only on previous states

Think:

```
Dynamic Programming
```

If only a few previous states are required,

Always ask:

> Can I optimize the DP array into variables?

---

# Key Takeaways

✅ DP State

```
dp[i]
```

= Number of ways to reach stair `i`

---

✅ Recurrence

```
dp[i] = dp[i-1] + dp[i-2]
```

---

✅ Base Cases

```
dp[1] = 1

dp[2] = 2
```

---

✅ Time Complexity

```
O(n)
```

---

✅ Space Complexity

```
O(n)
```

Optimized

```
O(1)
```

---

# Similar Problems

- Fibonacci Number (LC 509)
- Min Cost Climbing Stairs (LC 746)
- House Robber (LC 198)
- Decode Ways (LC 91)
- Coin Change (LC 322)
- Combination Sum IV (LC 377)

These all build upon the same **1D Dynamic Programming** pattern.
