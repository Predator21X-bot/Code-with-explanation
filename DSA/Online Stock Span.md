# Online Stock Span

## Problem Summary

We receive stock prices one-by-one.

For every incoming price:

```cpp id="m1r8v4"
price
```

return:

# stock span

---

# What Is Stock Span?

Span means:

# number of consecutive previous days

(including current day)

where:

previousPrice\le currentPrice

---

# Example

Prices:

```text id="q7m2k5"
[100,80,60,70,60,75,85]
```

Spans:

```text id="x4m9r1"
[1,1,1,2,1,4,6]
```

---

# Why?

For:

```text id="t8m1k6"
75
```

previous smaller/equal prices are:

```text id="f5m7r2"
60
70
60
```

plus current day itself.

Total span:

```text id="p2m8v4"
4
```

---

# MOST Important Insight

We need:

# consecutive previous smaller/equal elements

This leads to:

# monotonic stack

---

# Important Relationship

Daily Temperatures asked:

```text id="v6m1r9"
next greater element
```

Stock Span asks:

```text id="m4r8k1"
previous greater element
```

Very important connection.

---

# VERY Important Clarification

There is NO special STL structure called:

```text id="q9m2v7"
monotonic stack
```

It is simply:

```cpp id="x3m7r5"
stack<...>
```

with special maintenance logic.

---

# Why Called Monotonic?

Because stack is maintained in:

* increasing
  OR
* decreasing order

using popping rules.

---

# In This Problem

We maintain:

# monotonic decreasing stack

of prices.

---

# Why Decreasing?

Because once a larger price arrives:

```text id="f8m1k2"
smaller/equal prices become useless
```

They can never block future spans again.

---

# What Should Stack Store?

We need:

* current price
* already computed span

So stack stores:

```cpp id="p5m9r1"
(price, span)
```

using:

```cpp id="t2m7v4"
pair<int,int>
```

---

# Stack Declaration

```cpp id="n8m4r6"
stack<pair<int,int>> st;
```

---

# Meaning Of Pair

```text id="r4m1k8"
(first, second)
```

represents:

```text id="q7m5v2"
(price, span)
```

---

# Example

```text id="x1m8r4"
(70,2)
```

means:

* price = 70
* span = 2

---

# MOST Important Optimization

Instead of recounting previous days repeatedly:

```text id="m9r2k5"
we reuse previously computed spans
```

Very important.

---

# Main Logic

For every incoming price:

---

# Step 1 — Initialize Span

Current day itself counts.

```cpp id="v5m7r1"
int span = 1;
```

---

# Step 2 — Pop Smaller/Equal Prices

While:

currentPrice\ge stackTopPrice

current price absorbs previous spans.

---

# Why?

Because those smaller/equal prices belong inside current span.

---

# Condition

```cpp id="f2m8v6"
while(
    !st.empty()
    &&
    price >= st.top().first
)
```

---

# Why `.first`?

Because:

```cpp id="t4m1r9"
.first = price
```

---

# Step 3 — Add Previous Span

```cpp id="q8m5r2"
span += st.top().second;
```

---

# Why `.second`?

Because:

```cpp id="x6m1v7"
.second = span
```

---

# Step 4 — Pop

```cpp id="m3r8k1"
st.pop();
```

because smaller/equal price becomes useless.

---

# Step 5 — Push Current Pair

```cpp id="p7m2v4"
st.push({price, span});
```

---

# Step 6 — Return Span

```cpp id="f4m9r2"
return span;
```

---

# Example Dry Run

Prices:

```text id="q1m7k8"
[100,80,60,70,60,75]
```

---

# Day 1 → 100

Span:

```text id="v8m4r1"
1
```

Push:

```text id="n5m2v7"
(100,1)
```

Stack:

```text id="x2m8r5"
[(100,1)]
```

---

# Day 2 → 80

Check:

80\ge100

FALSE.

Push:

```text id="m6r1k9"
(80,1)
```

Stack:

```text id="t8m4v2"
[(100,1),(80,1)]
```

---

# Day 3 → 60

Push directly.

Stack:

```text id="q3m7r1"
[(100,1),(80,1),(60,1)]
```

---

# Day 4 → 70

Compare:

70\ge60

TRUE.

Pop `(60,1)`.

Add span:

```text id="v9m2k4"
1 + 1 = 2
```

---

# Compare Again

70\ge80

FALSE.

Push:

```text id="f1m8r6"
(70,2)
```

Stack:

```text id="p4m7v2"
[(100,1),(80,1),(70,2)]
```

---

# Important Observation

Stack prices remain:

# decreasing

```text id="x7m1r5"
100
80
70
```

That is why this becomes:

# monotonic decreasing stack

---

# Internal Structure Comparison

---

# Normal Stack

Elements:

```text id="m2r9k8"
[100,80,60,70]
```

Normal stack stores:

```text id="q5m8v1"
[100,80,60,70]
```

No ordering rule.

---

# Monotonic Decreasing Stack

Using popping condition:

```cpp id="t1m7r4"
while(current >= top)
```

Final stack becomes:

```text id="f8m2k5"
[100,80,70]
```

because 60 gets absorbed by 70.

---

# MOST Important Difference

Normal stack:

```text id="v4m9r1"
stores everything
```

Monotonic stack:

```text id="n6m1v8"
stores only useful unresolved barriers
```

---

# Complete Optimal Solution

```cpp id="x5m2r7"
class StockSpanner {
public:

    stack<pair<int,int>> st;

    StockSpanner() {

    }

    int next(int price) {

        int span = 1;

        while(
            !st.empty()
            &&
            price >= st.top().first
        ){

            span += st.top().second;

            st.pop();
        }

        st.push({price, span});

        return span;
    }
};
```

---

# Complexity Analysis

Each element:

* pushed once
* popped once

Total:

```text id="k4m9v2"
O(n)
```

---

# Space Complexity

Stack storage:

```text id="w8m1r3"
O(n)
```

---

# Important Pattern Learned

This is classic:

# Previous Greater Element Pattern

using:

# monotonic decreasing stack

---

# Similar Problems

* Daily Temperatures
* Next Greater Element
* Largest Rectangle in Histogram
* Trapping Rain Water
* Remove K Digits

---

# Common Mistakes

## Mistake 1

Using:

```cpp id="q6m2v7"
>
```

instead of:

```cpp id="t8m1k5"
>=
```

Equal prices should also be included in span.

---

## Mistake 2

Storing only prices.

Need spans too for optimization.

---

## Mistake 3

Recomputing span manually every time.

Monotonic stack avoids repeated counting.

---

## Mistake 4

Thinking monotonic stack is separate STL structure.

It is just:

```cpp id="f2m7r9"
normal stack
```

with special popping rules.

---

# Recognition Pattern

Whenever problem asks:

* previous greater
* consecutive smaller values
* spans/ranges
* nearest larger barrier

think:

# monotonic stack

---

# Important Mental Models

## Pop Means

```text id="m5r8k1"
current price absorbs previous span
```

---

# Push Means

```text id="x7m4v2"
current price becomes future barrier
```

---

# Monotonic Property

Stack order maintained manually using popping logic.

---

# Biggest Takeaway

Monotonic stack works because:

# smaller useless elements are removed immediately

allowing efficient span/range computation in:

```text id="q9m1r6"
O(n)
```

time.
