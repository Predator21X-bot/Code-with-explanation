# 🟦 LeetCode 121 — Best Time to Buy and Sell Stock

**Difficulty:** Easy
**Pattern:** Array + Greedy / Dynamic Programming
**Core Pattern:** **Single-pass minimum tracking**
**Time Complexity:** `O(n)`
**Space Complexity:** `O(1)`

This problem is a foundational stock problem and is commonly categorized around **single-pass minimum tracking**; it also forms the basis for the more advanced stock DP/state-machine problems. ([LeetCode][1])

---

## 📌 Problem

Given:

```cpp
prices[i]
```

where `prices[i]` is the stock price on day `i`.

You can:

* Buy **once**
* Sell **once**
* You must **buy before selling**

Return the maximum profit.

If no profit is possible, return:

```text
0
```

Example:

```text
prices = [7,1,5,3,6,4]
```

Best transaction:

```text
Buy at 1
Sell at 6

Profit = 6 - 1 = 5
```

Answer:

```text
5
```

---

# 💡 Core Idea

At every day, ask:

> **"If I sell today, what is the maximum profit I could make?"**

If today's price is:

```cpp
prices[i]
```

then the best possible buying price before today is the **minimum price we've seen so far**.

So:

```text
profit = currentPrice - minimumPrice
```

Then keep track of the maximum profit.

---

# ⭐ The Two Variables

We only need:

```cpp
int minPrice;
int maxProfit;
```

### `minPrice`

```text
The cheapest price at which we could have bought so far.
```

### `maxProfit`

```text
The maximum profit we could have made so far.
```

---

# 🧠 Why Do We Need `minPrice`?

Consider:

```text
prices = [7,1,5,3,6,4]
```

As we move through the array:

```text
7
↓
minimum = 7
```

Then:

```text
1
↓
minimum = 1
```

Now when we reach:

```text
5
```

we ask:

```text
If I sell today at 5,
what is the best buying price?
```

We've already seen:

```text
1
```

Therefore:

```text
profit = 5 - 1
       = 4
```

Later:

```text
6 - 1 = 5
```

So maximum profit becomes `5`.

---

# 🔥 The Key Formula

For every price:

```cpp
profit = prices[i] - minPrice;
```

Then:

```cpp
maxProfit = max(maxProfit, profit);
```

And before that, update:

```cpp
minPrice = min(minPrice, prices[i]);
```

So the basic logic is:

```cpp
minPrice = min(minPrice, prices[i]);

int profit = prices[i] - minPrice;

maxProfit = max(maxProfit, profit);
```

---

# 🧪 Dry Run

Let's take:

```text
prices = [7,1,5,3,6,4]
```

Initially:

```cpp
minPrice = prices[0] = 7;
maxProfit = 0;
```

---

### Day 0 — Price = 7

```text
minPrice = min(7,7)
         = 7

profit = 7 - 7
       = 0

maxProfit = max(0,0)
          = 0
```

State:

```text
minPrice = 7
maxProfit = 0
```

---

### Day 1 — Price = 1

```text
minPrice = min(7,1)
         = 1
```

Profit:

```text
1 - 1 = 0
```

State:

```text
minPrice = 1
maxProfit = 0
```

This is important:

> We don't make a profit by selling today. But we have found a much better buying price for future days.

---

### Day 2 — Price = 5

Minimum:

```text
minPrice = min(1,5)
         = 1
```

Profit:

```text
5 - 1 = 4
```

Update:

```text
maxProfit = max(0,4)
          = 4
```

State:

```text
minPrice = 1
maxProfit = 4
```

---

### Day 3 — Price = 3

```text
minPrice = min(1,3)
         = 1
```

Profit:

```text
3 - 1 = 2
```

Maximum remains:

```text
maxProfit = 4
```

---

### Day 4 — Price = 6

```text
minPrice = 1
```

Profit:

```text
6 - 1 = 5
```

Therefore:

```text
maxProfit = 5
```

---

### Day 5 — Price = 4

Profit:

```text
4 - 1 = 3
```

Maximum remains:

```text
5
```

Final:

```text
minPrice = 1
maxProfit = 5
```

Answer:

```text
5
```

---

# ✅ C++ Solution

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {

        int minPrice = prices[0];
        int maxProfit = 0;

        for (int i = 1; i < prices.size(); i++) {

            // Best buying price so far
            minPrice = min(minPrice, prices[i]);

            // Profit if we sell today
            int profit = prices[i] - minPrice;

            // Best profit so far
            maxProfit = max(maxProfit, profit);
        }

        return maxProfit;
    }
};
```

---

# 🔥 Even Simpler Way to Think About It

Imagine you're walking through the prices:

```text
7 → 1 → 5 → 3 → 6 → 4
```

You're carrying two pieces of information:

```text
💰 Cheapest price I've seen = minPrice

📈 Best profit I've seen = maxProfit
```

Whenever you see a new price:

### First:

```text
"Is this the cheapest price I've seen?"

→ update minPrice
```

### Then:

```text
"If I sell today, how much could I make?"

→ current price - minPrice
```

### Finally:

```text
"Is that my best profit so far?"

→ update maxProfit
```

---

# ⚠️ Important Ordering

You may see the code written as:

```cpp
minPrice = min(minPrice, prices[i]);

maxProfit = max(
    maxProfit,
    prices[i] - minPrice
);
```

That's fine.

But conceptually, it's useful to understand that `minPrice` represents a **buying price from today or earlier**.

For example:

```text
prices = [7,1,5]
```

At `5`:

```text
minPrice = 1
```

So:

```text
5 - 1 = 4
```

The buy happens on an earlier day, so the transaction is valid.

---

# 🧠 Why Not Just Find Global Minimum and Maximum?

This is a common mistake.

Suppose:

```text
prices = [7,6,4,3,1]
```

Global minimum:

```text
1
```

Global maximum:

```text
7
```

You might calculate:

```text
7 - 1 = 6
```

But that's **invalid** because you would have to buy at `1` and then sell at `7`, meaning you'd sell **before** you bought.

The order matters:

```text
BUY → SELL
```

not:

```text
SELL → BUY
```

That's why we track the minimum **while traversing from left to right**.

---

# 🧪 Example Where Order Matters

```text
prices = [10,8,6,4,2]
```

The stock only decreases.

We keep finding cheaper prices:

```text
10 → 8 → 6 → 4 → 2
```

But every possible profit is negative.

We don't want negative profit.

So:

```cpp
maxProfit = 0;
```

remains:

```text
0
```

Answer:

```text
0
```

---

# 🔑 Why `maxProfit = 0`?

Because the problem allows us to **not make a transaction**.

So if:

```text
selling price < buying price
```

we simply don't trade.

Therefore:

```cpp
maxProfit = 0;
```

is the natural initial value.

---

# 🚀 Alternative DP Interpretation

You can also understand this as a very small DP/state problem.

At each day, we essentially maintain:

```text
minimum buying cost so far
maximum profit so far
```

This is why this problem often appears under both **Dynamic Programming** and **Greedy** classifications. ([LeetCode][2])

The optimized state is just:

```text
minPrice
maxProfit
```

instead of maintaining an entire DP array.

---

# 🔥 Connection to Your Previous DP Learning

You were recently learning:

```text
Minimum Path Sum
```

where we had:

```cpp
dp[i][j]
```

Here, we don't need a whole array.

Why?

Because the information we need from the past can be summarized by just:

```cpp
minPrice
maxProfit
```

This is an important DP optimization idea:

> **If the future only depends on a small amount of information from the past, we don't necessarily need to store the entire DP table.**

---

# ⚠️ Common Mistakes

### ❌ Mistake 1: Sort the prices

Don't do:

```cpp
sort(prices.begin(), prices.end());
```

Sorting destroys the chronological order.

We need:

```text
BUY before SELL
```

---

### ❌ Mistake 2: Use global min/max

As discussed:

```text
min → max
```

must happen in that order.

---

### ❌ Mistake 3: Reset `minPrice`

Don't do:

```cpp
minPrice = prices[i];
```

every iteration.

We want:

```cpp
minPrice = min(minPrice, prices[i]);
```

because we're remembering the cheapest price **seen so far**.

---

# 🎯 Interview Cheat Sheet

### State

```cpp
minPrice
```

> Cheapest buying price seen so far.

```cpp
maxProfit
```

> Maximum profit seen so far.

### Transition

```cpp
minPrice = min(minPrice, prices[i]);

int profit = prices[i] - minPrice;

maxProfit = max(maxProfit, profit);
```

### Complexity

```text
Time  → O(n)
Space → O(1)
```

---

# 🧠 The Mental Model

```text
prices
  ↓
Scan left → right
  ↓
Track cheapest price
  ↓
"What if I sell TODAY?"
  ↓
current price - cheapest price
  ↓
Track maximum profit
```

Or simply:

> **Buy at the cheapest price seen so far, and at every day ask what profit I'd get if I sold today. Keep the maximum.**

This **single-pass minimum-tracking pattern** is worth remembering because it becomes the foundation for more advanced stock problems such as Stock II, Stock III, cooldown, and transaction-fee variants. ([LeetCode][1])

[LeetCode 121 — Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/?utm_source=chatgpt.com)

[1]: https://leetcode.com/discuss/post/7452139/25-array-problems-to-revise-before-inter-8l98/?utm_source=chatgpt.com "25 Array Problems To Revise Before Interviews - Discuss - LeetCode"
[2]: https://leetcode.com/discuss/post/7474246/top-120-most-frequently-asked-amazon-sde-imwy/?utm_source=chatgpt.com "Top 120 Most Frequently Asked Amazon SDE-1 Interview Questions (2022–2025) - Discuss - LeetCode"
