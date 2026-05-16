# Combination Sum III

## Problem Summary

We must choose:

```text id="m1r8v4"
exactly k numbers
```

from:

```text id="q7m2k5"
1 → 9
```

such that:

```text id="x4m9r1"
their sum = n
```

Return ALL valid combinations.

---

# Important Rules

## Rule 1

Allowed numbers are:

```text id="t8m1k6"
1 to 9
```

---

## Rule 2

Each number can be used:

```text id="f5m7r2"
ONLY ONCE
```

---

## Rule 3

Need EXACTLY:

```text id="p2m8v4"
k numbers
```

---

# Example 1

## Input

```text id="v6m1r9"
k = 3
n = 7
```

Need:

```text id="m4r8k1"
3 numbers whose sum = 7
```

Valid answer:

```text id="q9m2v7"
[1,2,4]
```

because:

```text id="x3m7r5"
1 + 2 + 4 = 7
```

---

# Example 2

## Input

```text id="f8m1k2"
k = 3
n = 9
```

Possible answers:

```text id="p5m9r1"
[1,2,6]
[1,3,5]
[2,3,4]
```

---

# Invalid Examples

## Repeated Numbers

```text id="t2m7v4"
[1,1,5]
```

Invalid because numbers cannot repeat.

---

## Wrong Size

```text id="n8m4r6"
[2,5]
```

Invalid because size is not `k`.

---

# Key Observation

Problem asks:

```text id="r4m1k8"
Generate ALL valid combinations
```

Whenever you see:

* all combinations
* all possibilities
* choose k elements

think:

# Backtracking

---

# Important Difference

This is:

# Combination Problem

NOT permutation.

Meaning:

```text id="q7m5v2"
[1,2,4]
```

and:

```text id="x1m8r4"
[2,1,4]
```

are considered SAME.

Order does NOT matter.

---

# Why Order Does Not Repeat

We only move FORWARD while choosing numbers.

This is controlled using:

```text id="m9r2k5"
start variable
```

---

# Core Backtracking Idea

At every step:

```text id="v5m7r1"
choose a number
explore further
undo choice
```

Classic backtracking cycle.

---

# Recursive State

At any recursive call we need:

## 1. Start Number

Example:

```text id="f2m8v6"
start = 4
```

means:

```text id="t4m1r9"
try only 4,5,6...
```

This avoids duplicates.

---

## 2. Current Combination

Example:

```text id="q8m5r2"
[1,2]
```

---

## 3. Current Sum

Example:

```text id="x6m1v7"
3
```

---

## 4. Result Vector

Stores all valid combinations.

---

# Important Pruning

Suppose:

```text id="m3r8k1"
sum > n
```

Can this path ever become valid?

❌ No.

Because all numbers are positive.

Adding more numbers only increases sum further.

So immediately:

```cpp id="p7m2v4"
return;
```

This is called:

# Pruning

Very important optimization.

---

# Base Cases

## Case 1 — Sum Too Large

```cpp id="f4m9r2"
if(sum > n)
    return;
```

---

## Case 2 — Already Picked k Numbers

```cpp id="q1m7k8"
if(current.size() == k)
```

Now:

### If sum == n

Valid combination found.

```cpp id="v8m4r1"
ans.push_back(current);
```

---

### Otherwise

Combination invalid.

Simply return.

---

# Why Return After Size == k?

Because:

```text id="n5m2v7"
we cannot pick more than k numbers
```

Problem requires EXACTLY k numbers.

---

# Recursive Loop

We try all possible next numbers:

```cpp id="x2m8r5"
for(int i = start; i <= 9; i++)
```

---

# Backtracking Cycle

## Step 1 — Choose

```cpp id="m6r1k9"
current.push_back(i);
```

---

## Step 2 — Explore

```cpp id="t8m4v2"
solve(
    i + 1,
    current,
    sum + i,
    k,
    n,
    ans
);
```

---

# Why `i + 1` ?

Because:

```text id="q3m7r1"
same number cannot be reused
```

and combinations move forward only.

---

# Example

Suppose current choice is:

```text id="v9m2k4"
3
```

Next recursion starts from:

```text id="f1m8r6"
4
```

So:

```text id="p4m7v2"
3 cannot appear again
```

---

## Step 3 — Undo Choice

```cpp id="x7m1r5"
current.pop_back();
```

This restores previous state.

This is the HEART of backtracking.

---

# Important Mental Model

Backtracking always follows:

```text id="m2r9k8"
choose
explore
undo
```

---

# Visual Tree

For:

```text id="q5m8v1"
k = 3
n = 7
```

Decision tree:

```text id="t1m7r4"
start
 ├── 1
 │    ├── 2
 │    │    ├── 3 -> sum=6
 │    │    ├── 4 -> sum=7 ✅
 │
 │    ├── 3
 │
 ├── 2
```

Every path is one possible combination.

---

# Dry Run

## Input

```text id="f8m2k5"
k = 3
n = 7
```

---

# Start

```text id="v4m9r1"
current = []
sum = 0
start = 1
```

---

# Choose 1

```text id="n6m1v8"
current = [1]
sum = 1
```

---

# Choose 2

```text id="x5m2r7"
current = [1,2]
sum = 3
```

---

# Choose 3

```text id="k4m9v2"
current = [1,2,3]
sum = 6
```

Size already:

```text id="w8m1r3"
3
```

But sum != 7.

Return.

---

# Backtrack

Remove:

```text id="q6m2v7"
3
```

Try:

```text id="t8m1k5"
4
```

Now:

```text id="f2m7r9"
current = [1,2,4]
sum = 7
```

Valid answer found.

Store it.

---

# Final Code

```cpp id="m5r8k1"
class Solution {
public:

    void solve(
        int start,
        vector<int>& current,
        int sum,
        int k,
        int n,
        vector<vector<int>>& ans
    ){

        // Pruning
        if(sum > n)
            return;

        // Base case
        if(current.size() == k){

            if(sum == n){
                ans.push_back(current);
            }

            return;
        }

        // Try all next numbers
        for(int i = start; i <= 9; i++){

            // Choose
            current.push_back(i);

            // Explore
            solve(
                i + 1,
                current,
                sum + i,
                k,
                n,
                ans
            );

            // Undo choice
            current.pop_back();
        }
    }

    vector<vector<int>> combinationSum3(int k, int n) {

        vector<vector<int>> ans;
        vector<int> current;

        solve(1, current, 0, k, n, ans);

        return ans;
    }
};
```

---

# Complexity Analysis

Only numbers:

```text id="x7m4v2"
1 → 9
```

are explored.

Worst-case recursion roughly explores:

```text id="q9m1r6"
O(2^9)
```

which is manageable.

---

# Time Complexity

Approx:

```text id="w4m8k2"
O(2^9)
```

---

# Space Complexity

Recursive depth:

```text id="p1m7r5"
O(k)
```

excluding answer storage.

---

# Common Mistakes

## Mistake 1

Forgetting:

```cpp id="t6m2v8"
current.pop_back();
```

Without undo step:

```text id="r8m5k4"
state becomes corrupted
```

---

## Mistake 2

Using:

```cpp id="x2m9r1"
solve(i, ...)
```

instead of:

```cpp id="m1r8v6"
solve(i + 1, ...)
```

This would allow number reuse.

---

## Mistake 3

Not pruning when:

```text id="q6m2v7"
sum > n
```

Leads to unnecessary recursion.

---

## Mistake 4

Confusing combinations with permutations.

Order does NOT matter here.

---

# Recognition Pattern

This is classic:

# Combination Backtracking

Pattern:

```text id="t8m1k5"
pick elements
move forward only
generate all valid combinations
```

---

# Important Mental Models

## Backtracking

```text id="f2m7r9"
choose
explore
undo
```

---

## DFS Exploration

Recursion explores one path deeply before returning.

---

## Pruning

Cut useless branches early.

---

# Biggest Takeaway

Backtracking is fundamentally:

```text id="m5r8k1"
systematic DFS exploration of decision trees
```

using recursive:

```text id="x7m4v2"
choose → explore → undo
```

cycles.
