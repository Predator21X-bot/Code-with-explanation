# Daily Temperatures

## Problem Summary

We are given:

```cpp id="m1r8v4"
temperatures[i]
```

For each day, find:

# how many days until a warmer temperature

If no warmer future day exists:

```text id="q7m2k5"
answer = 0
```

---

# Example

```text id="x4m9r1"
temperatures =
[73,74,75,71,69,72,76,73]
```

Output:

```text id="t8m1k6"
[1,1,4,2,1,1,0,0]
```

---

# Why?

For:

```text id="f5m7r2"
73
```

next warmer temperature:

```text id="p2m8v4"
74
```

comes after:

```text id="v6m1r9"
1 day
```

---

# MOST Important Insight

For every index:

```text id="m4r8k1"
find next greater element on right
```

This is classic:

# Next Greater Element Pattern

---

# Brute Force Approach

For every day:

scan right side until warmer temperature found.

---

# Complexity

```text id="q9m2v7"
O(n²)
```

Too slow.

---

# Optimization Idea

We need fast access to:

# future warmer temperatures

This leads to:

# stack

---

# VERY Important Clarification

There is NO separate STL structure called:

```text id="x3m7r5"
monotonic stack
```

It is simply:

```cpp id="f8m1k2"
stack<int>
```

---

# Then Why Called Monotonic Stack?

Because OUR LOGIC maintains stack in:

* increasing order
  OR
* decreasing order

---

# In This Problem

We maintain:

# monotonic decreasing stack

of temperatures.

---

# MOST Important Insight

The stack itself is normal.

The:

```cpp id="p5m9r1"
while(...)
```

condition makes it monotonic.

---

# Stack Declaration

```cpp id="t2m7v4"
stack<int> st;
```

Completely normal stack.

---

# Why Store Indices Instead Of Temperatures?

Because answer needs:

futureIndex-currentIndex

days difference.

---

# If We Stored Only Temperatures

Suppose stack contains:

```text id="n8m4r6"
73
```

and current temperature:

```text id="r4m1k8"
74
```

We know warmer temperature exists.

BUT:

```text id="q7m5v2"
we do not know previous position
```

Cannot calculate distance.

---

# Therefore Stack Stores

# indices

---

# Then We Can Access

---

# Temperature

Using:

```cpp id="x1m8r4"
temperatures[st.top()]
```

---

# AND Index

Using:

```cpp id="m9r2k5"
st.top()
```

---

# Important Monotonic Property

Stack temperatures remain:

# decreasing

from bottom → top.

---

# Why?

Because current warmer temperature removes smaller previous temperatures.

---

# Main Logic

Traverse temperatures left → right.

For current temperature:

```text id="v5m7r1"
temperatures[i]
```

---

# Compare With Stack Top

While:

```cpp id="f2m8v6"
temperatures[i] >
temperatures[st.top()]
```

we found warmer day for stack top index.

---

# Why?

Current day is FIRST warmer future day for that index.

---

# Then

Store answer:

```cpp id="t4m1r9"
ans[prevIndex] =
i - prevIndex;
```

---

# Pop It

```cpp id="q8m5r2"
st.pop();
```

because answer already found.

---

# Finally Push Current Index

```cpp id="x6m1v7"
st.push(i);
```

because current day still needs future warmer answer.

---

# VERY Important Mental Model

Stack contains:

# unresolved indices

still waiting for their next greater element.

---

# Monotonic Stack vs Normal Stack

---

# Normal Stack

Push everything normally.

Example elements:

```text id="m3r8k1"
[30,40,35,50,20,25]
```

Final stack:

```text id="p7m2v4"
[30,40,35,50,20,25]
```

No ordering guarantee.

---

# Monotonic Decreasing Stack

Using popping condition:

```cpp id="f4m9r2"
while(current > stackTop)
```

Final stack becomes:

```text id="q1m7k8"
[50,25]
```

because smaller resolved elements get removed.

---

# MOST Important Difference

Normal stack:

```text id="v8m4r1"
stores history
```

Monotonic stack:

```text id="n5m2v7"
stores only useful unresolved candidates
```

---

# Internal Working Example

Temperatures:

```text id="x2m8r5"
[30,40,35,50,20,25]
```

---

# i = 0 → 30

Stack:

```text id="m6r1k9"
[30]
```

---

# i = 1 → 40

40>30

Pop 30.

Push 40.

Stack:

```text id="t8m4v2"
[40]
```

---

# i = 2 → 35

35>40

False.

Push 35.

Stack:

```text id="q3m7r1"
[40,35]
```

Still decreasing.

---

# i = 3 → 50

50>35

Pop 35.

Then:

50>40

Pop 40.

Push 50.

Stack:

```text id="v9m2k4"
[50]
```

---

# i = 4 → 20

Push 20.

Stack:

```text id="f1m8r6"
[50,20]
```

---

# i = 5 → 25

25>20

Pop 20.

Push 25.

Final stack:

```text id="p4m7v2"
[50,25]
```

Still decreasing.

---

# MOST Important Observation

At ALL times:

stack temperatures remain:

# decreasing

That is why it is called:

# monotonic decreasing stack

---

# Leftover Stack Elements

At end of traversal:

indices still in stack NEVER found warmer future day.

Their answers remain:

```text id="x7m1r5"
0
```

---

# Why Automatically 0?

Because answer array initialized as:

```cpp id="m2r9k8"
vector<int> ans(n, 0);
```

---

# Complete Optimal Solution

```cpp id="q5m8v1"
class Solution {
public:

    vector<int> dailyTemperatures(
        vector<int>& temperatures
    ) {

        int n = temperatures.size();

        vector<int> ans(n, 0);

        stack<int> st;

        for(int i = 0; i < n; i++){

            while(
                !st.empty()
                &&
                temperatures[i] >
                temperatures[st.top()]
            ){

                int prevIndex = st.top();

                st.pop();

                ans[prevIndex] =
                i - prevIndex;
            }

            st.push(i);
        }

        return ans;
    }
};
```

---

# Complexity Analysis

## Time Complexity

Every index:

* pushed once
* popped once

Total:

```text id="t1m7r4"
O(n)
```

---

# Space Complexity

Stack storage:

```text id="f8m2k5"
O(n)
```

---

# Important Pattern Learned

This is classic:

# Next Greater Element Pattern

using:

# Monotonic Stack

---

# Similar Problems

* Next Greater Element I
* Next Greater Element II
* Stock Span Problem
* Largest Rectangle in Histogram
* Trapping Rain Water

---

# Common Mistakes

## Mistake 1

Storing temperatures instead of indices.

Need indices for:

```text id="v4m9r1"
distance calculation
```

---

## Mistake 2

Using:

```cpp id="n6m1v8"
if
```

instead of:

```cpp id="x5m2r7"
while
```

Current temperature may resolve MANY previous indices.

---

## Mistake 3

Forgetting to push current index after popping.

Current index still needs future answer.

---

## Mistake 4

Thinking monotonic stack is separate STL structure.

It is just:

```cpp id="k4m9v2"
normal stack<int>
```

with special maintenance rules.

---

# Recognition Pattern

Whenever problem asks:

* next greater
* next warmer
* next larger
* nearest greater neighbor

think:

# monotonic stack

---

# Important Mental Models

## Pop Means

```text id="w8m1r3"
answer found
```

---

# Push Means

```text id="q6m2v7"
still unresolved
```

---

# Monotonic Property

Stack order maintained manually using popping rules.

---

# Biggest Takeaway

Monotonic stack is:

# a stack pattern

where special push/pop logic maintains sorted order internally

allowing nearest greater/smaller queries in:

```text id="t8m1k5"
O(n)
```

time.
