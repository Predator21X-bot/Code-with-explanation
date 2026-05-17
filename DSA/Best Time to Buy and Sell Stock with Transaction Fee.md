# Best Time to Buy and Sell Stock with Transaction Fee

## Problem Summary

We are given:

```cpp id="m1r8v4"
prices[i]
```

Meaning:

```text id="q7m2k5"
stock price on day i
```

We can:

* buy stock
* sell stock
* do nothing

---

# Important Rule

Every time we SELL:

```text id="x4m9r1"
pay transaction fee
```

---

# Goal

Find:

# maximum profit

possible.

---

# Important Observation

At every day we are in ONE of two states.

---

# State 1 — Holding Stock

Meaning:

```text id="t8m1k6"
we currently own stock
```

Possible actions:

* keep holding
* sell

---

# State 2 — Not Holding Stock

Meaning:

```text id="f5m7r2"
we currently own NO stock
```

Possible actions:

* buy
* skip

---

# MOST Important DP Idea

Track:

# best profit in each state

---

# DP State Definitions

---

# hold

Define:

```text id="p2m8v4"
hold
```

as:

# maximum profit while currently holding stock

---

# cash

Define:

```text id="v6m1r9"
cash
```

as:

# maximum profit while NOT holding stock

---

# Understanding Why `hold` Is Negative

This is the MOST important concept.

Suppose:

```text id="m4r8k1"
price = 5
```

If we buy stock:

```text id="q9m2v7"
we spend money
```

So profit becomes:

```text id="x3m7r5"
-5
```

because money left wallet.

---

# Example

Start:

```text id="f8m1k2"
cash = 0
```

Buy stock at:

```text id="p5m9r1"
5
```

Now:

```text id="t2m7v4"
hold = -5
```

because we spent 5.

---

# Important Mental Model

`hold` does NOT mean:

* stock value
* stock price

It means:

# current net profit while holding stock

---

# Initial States

On day 0:

---

# If Holding Stock

We must have bought it.

So:

```cpp id="n8m4r6"
hold = -prices[0];
```

---

# If Not Holding Stock

Profit is:

```cpp id="r4m1k8"
cash = 0;
```

---

# State Transitions

At every day:

```text id="q7m5v2"
buy
sell
or do nothing
```

---

# Updating `hold`

Two choices.

---

# Choice 1 — Keep Holding

Remain in holding state:

```text id="x1m8r4"
prevHold
```

---

# Choice 2 — Buy Today

Previously we had cash:

```text id="m9r2k5"
prevCash - prices[i]
```

because buying costs money.

---

# Transition

hold=\max(prevHold,prevCash-prices[i])

---

# Updating `cash`

Again two choices.

---

# Choice 1 — Keep Current Cash

```text id="v5m7r1"
prevCash
```

---

# Choice 2 — Sell Today

If previously holding stock:

```text id="f2m8v6"
prevHold + prices[i] - fee
```

Subtract fee because selling incurs fee.

---

# Transition

cash=\max(prevCash,prevHold+prices[i]-fee)

---

# Why Use Previous Variables?

Very important.

Suppose we update:

```text id="t4m1r9"
hold first
```

Then:

```text id="q8m5r2"
cash update may accidentally use NEW hold
```

which is incorrect.

---

# DP Rule

Every current state must depend ONLY on:

```text id="x6m1v7"
previous day's states
```

So store:

```cpp id="m3r8k1"
prevHold
prevCash
```

before updating.

---

# Example Dry Run

## Input

```text id="p7m2v4"
prices = [1,3,2,8,4,9]
fee = 2
```

---

# Day 0

```text id="f4m9r2"
hold = -1
cash = 0
```

---

# Day 1 (price = 3)

Sell possible:

```text id="q1m7k8"
-1 + 3 - 2 = 0
```

No improvement.

---

# Day 3 (price = 8)

Sell:

```text id="v8m4r1"
-1 + 8 - 2 = 5
```

Now:

```text id="n5m2v7"
cash = 5
```

---

# Day 5 (price = 9)

Final profit improves further.

Answer:

```text id="x2m8r5"
8
```

---

# Complete Optimized Solution

```cpp id="m6r1k9"
class Solution {
public:

    int maxProfit(
        vector<int>& prices,
        int fee
    ) {

        int hold = -prices[0];

        int cash = 0;

        for(int i = 1; i < prices.size(); i++){

            int prevHold = hold;
            int prevCash = cash;

            // Buy or continue holding
            hold =
            max(
                prevHold,
                prevCash - prices[i]
            );

            // Sell or continue not holding
            cash =
            max(
                prevCash,
                prevHold + prices[i] - fee
            );
        }

        return cash;
    }
};
```

---

# Why Final Answer Is `cash`

At end:

```text id="t8m4v2"
holding stock means unsold stock
```

Profit incomplete.

So best final profit must be:

```text id="q3m7r1"
not holding stock
```

Hence:

```cpp id="v9m2k4"
return cash;
```

---

# Complexity Analysis

## Time Complexity

Single traversal through prices.

```text id="f1m8r6"
O(n)
```

---

## Space Complexity

Only variables used.

```text id="p4m7v2"
O(1)
```

---

# Important DP Pattern Learned

This is:

# State Machine Dynamic Programming

Very important interview category.

---

# Similar Problems

* Best Time to Buy and Sell Stock I
* Best Time to Buy and Sell Stock II
* Stock with Cooldown
* Stock with K Transactions
* Stock with Transaction Fee

---

# Common Mistakes

## Mistake 1

Thinking:

```text id="x7m1r5"
hold = stock price
```

Wrong.

`hold` means:

```text id="m2r9k8"
current net profit while holding stock
```

---

## Mistake 2

Initializing:

```cpp id="q5m8v1"
hold = 0;
```

Wrong.

Buying costs money.

Correct:

```cpp id="t1m7r4"
hold = -prices[0];
```

---

## Mistake 3

Updating states directly without previous variables.

Wrong:

```cpp id="f8m2k5"
hold = ...
cash = ...
```

because second update may use already-modified state.

---

## Mistake 4

Forgetting transaction fee during sell.

Correct sell formula:

prevHold+prices[i]-fee

---

# Recognition Pattern

Whenever stock problem involves:

* buying
* selling
* cooldown
* fees
* transactions

think:

# State DP

---

# Important Mental Models

## Buying

```text id="v4m9r1"
spending money
```

so profit decreases.

---

## Selling

```text id="n6m1v8"
earning money
```

so profit increases.

---

## State Machine

At every day:

```text id="x5m2r7"
holding stock
OR
not holding stock
```

---

# Biggest Takeaway

Many stock DP problems reduce to:

```text id="k4m9v2"
tracking optimal profit across states
```

and updating those states day-by-day efficiently.
