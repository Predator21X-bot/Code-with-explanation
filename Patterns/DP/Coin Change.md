# LeetCode 322 — Coin Change (GitHub Notes)

---

# 🔹 Problem

You are given an array of coin denominations `coins` and an integer `amount`.

Return the **minimum number of coins** needed to make up the given amount.

If it is impossible to make the amount, return `-1`.

You have an **infinite supply** of each coin.

---

# 🔹 Examples

### Example 1

```text
Input:
coins = [1,2,5]
amount = 11

Output:
3

Explanation:
11 = 5 + 5 + 1
```

---

### Example 2

```text
Input:
coins = [2]
amount = 3

Output:
-1
```

---

### Example 3

```text
Input:
coins = [1]
amount = 0

Output:
0
```

---

# 🔹 Pattern Recognition

Ask yourself:

* Need the **minimum**?
* Current answer depends on **smaller amounts**?
* Can reuse the same coin unlimited times?

This is a classic **1D Dynamic Programming (Unbounded Knapsack)** problem.

---

# 🔹 DP State

```cpp
dp[i]
```

represents

> **Minimum number of coins required to make amount `i`.**

---

# 🔹 Base Case

```cpp
dp[0] = 0;
```

Reason:

It takes **0 coins** to make amount **0**.

---

# 🔹 DP Initialization

```cpp
vector<int> dp(amount + 1, amount + 1);
```

Why `amount + 1`?

The maximum possible valid answer is `amount` (using `amount` coins of denomination `1`).

So,

```text
amount + 1
```

acts as **Infinity (Impossible)**.

It is preferred over `INT_MAX` because:

* avoids integer overflow while doing

```cpp
dp[i - coin] + 1
```

* no extra condition is required

---

# 🔹 Transition

Suppose we are computing

```cpp
dp[i]
```

Try every coin as the **last coin used**.

If

```cpp
coin <= i
```

then

```cpp
candidate = dp[i - coin] + 1
```

Take the minimum among all candidates.

Transition:

```cpp
dp[i] = min(dp[i], dp[i - coin] + 1);
```

---

# 🔹 Why does this work?

Example:

```text
coins = [1,2,5]
amount = 4
```

Possible last coins:

Using coin **1**

```text
dp[3] + 1 = 3
```

Using coin **2**

```text
dp[2] + 1 = 2
```

Using coin **5**

```text
Not possible
```

Minimum:

```text
dp[4] = 2
```

---

# 🔹 DP Table Dry Run

Example:

```text
coins = [1,2,5]
amount = 6
```

Initial:

```text
Index : 0 1 2 3 4 5 6

dp    : 0 7 7 7 7 7 7
```

(`7 = amount + 1`)

---

### i = 1

```text
coin 1

dp[1] = dp[0] + 1 = 1
```

```text
0 1 7 7 7 7 7
```

---

### i = 2

```text
coin 1 -> dp[1]+1 = 2

coin 2 -> dp[0]+1 = 1
```

```text
0 1 1 7 7 7 7
```

---

### i = 3

```text
coin 1 -> 2

coin 2 -> 2
```

```text
0 1 1 2 7 7 7
```

---

### i = 4

```text
coin 1 -> 3

coin 2 -> 2
```

```text
0 1 1 2 2 7 7
```

---

### i = 5

```text
coin 1 -> 3

coin 2 -> 3

coin 5 -> 1
```

```text
0 1 1 2 2 1 7
```

---

### i = 6

```text
coin 1 -> 2

coin 2 -> 3

coin 5 -> 2
```

Final DP

```text
0 1 1 2 2 1 2
```

Answer:

```text
dp[6] = 2
```

---

# 🔹 Algorithm

For every amount:

```cpp
for (int i = 1; i <= amount; i++)
```

Try every coin:

```cpp
for (int coin : coins)
```

If coin can be used:

```cpp
if (coin <= i)
```

Update answer:

```cpp
dp[i] = min(dp[i], dp[i - coin] + 1);
```

---

# 🔹 Why outer loop is Amount?

Because

```cpp
dp[i]
```

depends on

```cpp
dp[i - coin]
```

which is always a **smaller amount**.

Hence compute

```text
dp[1]

↓

dp[2]

↓

dp[3]

↓

...

↓

dp[amount]
```

This ensures every dependency is already computed.

---

# 🔹 Why inner loop is Coins?

For one amount there are multiple choices.

Example:

```text
amount = 11

Last coin = 1

Last coin = 2

Last coin = 5
```

We must try every coin and choose the minimum.

---

# 🔹 Edge Cases

### Amount is 0

```text
coins = [1]

amount = 0

Answer = 0
```

---

### Impossible Case

```text
coins = [2]

amount = 3

Answer = -1
```

---

### Single Coin

```text
coins = [5]

amount = 20

Answer = 4
```

---

# 🔹 Complexity

### Time Complexity

Outer loop:

```text
amount
```

Inner loop:

```text
number of coins
```

Overall:

```text
O(amount × number_of_coins)
```

---

### Space Complexity

Only one DP array:

```cpp
dp[amount + 1]
```

Therefore

```text
O(amount)
```

---

# 🔹 C++ Solution (Annotated)

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {

        // dp[i] = minimum coins needed to make amount i
        vector<int> dp(amount + 1, amount + 1);

        // Base case
        dp[0] = 0;

        // Compute answer for every amount
        for (int i = 1; i <= amount; i++) {

            // Try every coin
            for (int coin : coins) {

                // Coin can contribute
                if (coin <= i) {

                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        // Impossible case
        return dp[amount] == amount + 1 ? -1 : dp[amount];
    }
};
```

---

# 🔑 Key Takeaways

* **DP State:** `dp[i] = minimum coins needed to make amount i`
* **Base Case:** `dp[0] = 0`
* **Initialization:** `amount + 1` acts as **Infinity**
* **Transition:** `dp[i] = min(dp[i], dp[i - coin] + 1)`
* **Process amounts in increasing order** because current state depends on smaller amounts.
* **Try every coin** because each coin is a possible last choice.
* **Pattern:** 1D Dynamic Programming + Unbounded Knapsack.

---

## 📌 Interview Checklist

* ✅ Recognized as Dynamic Programming.
* ✅ Defined DP state correctly.
* ✅ Explained why `dp[0] = 0`.
* ✅ Explained why `amount + 1` is used instead of `INT_MAX`.
* ✅ Derived the recurrence instead of memorizing it.
* ✅ Explained why we iterate amounts from `1` to `amount`.
* ✅ Handled the impossible case by returning `-1`.
* ✅ Complexity:

  * **Time:** `O(amount × number_of_coins)`
  * **Space:** `O(amount)`
