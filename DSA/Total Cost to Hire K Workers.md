# Total Cost to Hire K Workers

## Problem Summary

We are given:

```cpp id="tn1r8u"
costs[]
```

Each value represents:

```text id="b1cv9m"
cost to hire a worker
```

We must hire exactly:

```text id="rj9k4a"
k workers
```

with minimum total cost.

---

# Important Restriction

At any moment, we can ONLY hire from:

```text id="x7r6nd"
first candidates workers
OR
last candidates workers
```

We cannot directly hire workers from the middle.

---

# Example

## Input

```text id="9q2k5s"
costs = [5,3,8,2,7,4]
candidates = 2
```

Initially available workers:

Front side:

```text id="m3y5cz"
5, 3
```

Back side:

```text id="c0fwj7"
7, 4
```

So selectable workers are:

```text id="p1e5k0"
5,3,7,4
```

NOT:

```text id="e3e8qe"
8 or 2
```

because they are in the middle.

---

# Hiring Process

Suppose we hire:

```text id="ytf6wr"
3
```

from left side.

Now front window moves ahead.

Front workers become:

```text id="5yyx7x"
5,8
```

Back remains:

```text id="f7tmxp"
7,4
```

Current selectable workers:

```text id="c7m4g4"
5,8,7,4
```

This process continues until:

```text id="k8h8n4"
k workers are hired
```

---

# Key Observation

At every step we need:

```text id="uxajb9"
minimum cost available worker
```

among current candidates.

This strongly suggests:

# Min Heap

because heap efficiently supports:

```text id="yw1ht5"
repeated minimum extraction
```

---

# Core Idea

Heap stores:

```text id="3pxrgg"
currently available workers
```

NOT already hired workers.

This is the MOST important understanding.

---

# Why Store Index Too?

We store:

```cpp id="k8hmsf"
{cost, index}
```

inside heap.

Why?

Because after hiring someone:

```text id="5w9srm"
we must refill from same side
```

Index helps identify:

```text id="yxg10m"
did worker come from left side or right side?
```

---

# Heap Type

```cpp id="33b0h0"
priority_queue<
    pair<int,int>,
    vector<pair<int,int>>,
    greater<pair<int,int>>
> pq;
```

---

# Important Bonus

Pair comparison automatically handles:

## First compare

```text id="q6gwr9"
cost
```

## Then compare

```text id="g0ep9w"
index
```

This perfectly matches problem rule:

```text id="yb7q3g"
if costs are same,
choose smaller index
```

No extra comparator needed.

---

# Two Pointer Setup

We maintain:

```cpp id="l1k6o5"
left
right
```

These represent:

```text id="4df1dq"
next available worker
from left and right sides
```

---

# Initial Filling

Initially insert:

## First `candidates` workers

from left side.

---

## Last `candidates` workers

from right side.

---

# Important Edge Case

Always check:

```cpp id="r7zhj3"
left <= right
```

Otherwise same worker may be inserted twice.

---

# Example

```text id="n4pr98"
costs = [1,2,3]
candidates = 2
```

Without overlap check:

```text id="0k4mwc"
worker at index 1 may be inserted twice
```

---

# Main Hiring Loop

Repeat exactly:

```text id="jrm9we"
k times
```

Each iteration:

---

## Step 1

Get cheapest worker:

```cpp id="7hbrwv"
auto [cost, index] = pq.top();
pq.pop();
```

---

## Step 2

Add to answer:

```cpp id="9t3w0z"
ans += cost;
```

---

## Step 3

Determine worker side.

Condition:

```cpp id="4h2o6g"
index < left
```

means:

```text id="ngv39w"
worker came from left side
```

because left pointer already moved ahead.

Otherwise:

```text id="l4m4w4"
worker came from right side
```

---

## Step 4

Refill from same side.

### Left refill

```cpp id="d5tfyr"
pq.push({costs[left], left});
left++;
```

---

### Right refill

```cpp id="7g2j6l"
pq.push({costs[right], right});
right--;
```

---

# Mental Model

Think of problem as:

```text id="4rxt7n"
Two hiring gates:
left gate and right gate
```

At every step:

```text id="rzm8z2"
hire cheapest visible worker
```

Then:

```text id="xtp7q8"
open next worker from same gate
```

---

# Complete Dry Run

## Input

```text id="0hqcmn"
costs = [5,3,8,2,7,4]
k = 3
candidates = 2
```

---

# Initial Heap

Insert from left:

```text id="xqqw6z"
{5,0}
{3,1}
```

Insert from right:

```text id="5gpxrd"
{7,4}
{4,5}
```

Heap contains:

```text id="4d0d7f"
3,4,5,7
```

---

# Hiring 1

Pop:

```text id="m7ykmj"
3
```

Answer:

```text id="9q2dqs"
3
```

Worker came from left.

Insert next left worker:

```text id="3my5hm"
8
```

---

# Hiring 2

Heap now:

```text id="8z89tp"
4,5,7,8
```

Pop:

```text id="b0wwgh"
4
```

Answer:

```text id="w9pkr9"
7
```

Worker came from right.

Insert next right worker:

```text id="nffg0q"
2
```

---

# Hiring 3

Heap now:

```text id="x2f4vz"
2,5,7,8
```

Pop:

```text id="3y9q1w"
2
```

Answer:

```text id="0gjxrf"
9
```

---

# Final Answer

```text id="vwz3we"
9
```

---

# Final Code

```cpp id="8c6x2u"
class Solution {
public:
    long long totalCost(vector<int>& costs, int k, int candidates) {

        int left = 0;
        int right = costs.size() - 1;

        long long ans = 0;

        priority_queue<
            pair<int,int>,
            vector<pair<int,int>>,
            greater<pair<int,int>>
        > pq;

        // Initial left candidates
        for(int i = 0; i < candidates && left <= right; i++){
            pq.push({costs[left], left});
            left++;
        }

        // Initial right candidates
        for(int i = 0; i < candidates && left <= right; i++){
            pq.push({costs[right], right});
            right--;
        }

        // Hire k workers
        for(int i = 0; i < k; i++){

            auto [cost, index] = pq.top();
            pq.pop();

            ans += cost;

            // Worker came from left side
            if(index < left){

                if(left <= right){
                    pq.push({costs[left], left});
                    left++;
                }

            }
            // Worker came from right side
            else{

                if(left <= right){
                    pq.push({costs[right], right});
                    right--;
                }

            }
        }

        return ans;
    }
};
```

---

# Complexity Analysis

## Heap Size

At most:

```text id="9t5p4f"
2 * candidates
```

workers inside heap.

---

# Time Complexity

Each push/pop:

```text id="myy1s4"
O(log candidates)
```

Total operations:

```text id="9rjlwm"
k
```

So:

```text id="t6nj0w"
O(k log candidates)
```

---

# Space Complexity

Heap stores:

```text id="e7r98r"
O(candidates)
```

workers.

---

# Common Mistakes

## Mistake 1

Using heap size based on `k`.

Wrong because:

```text id="5p3n83"
heap stores available workers
NOT hired workers
```

---

## Mistake 2

Forgetting overlap check:

```cpp id="t4m9rh"
left <= right
```

Can cause duplicate insertion.

---

## Mistake 3

Only storing cost in heap.

Need index too for:

```text id="a9fj6z"
side detection
```

---

## Mistake 4

Wrong refill logic.

After hiring:

```text id="cfh7f2"
must refill from SAME side
```

---

# Recognition Pattern

This is a classic:

```text id="8vz8vy"
Two Pointers + Min Heap
```

problem.

---

# Important Mental Models

## Heap

```text id="mqy7kw"
Current hiring pool
```

---

## Left Pointer

```text id="f1n2m6"
Next available left worker
```

---

## Right Pointer

```text id="8gx4v7"
Next available right worker
```

---

# Biggest Takeaway

The hardest insight is:

```text id="8xv8ah"
Heap stores currently selectable workers
```

Once that clicks:

the implementation becomes manageable.
