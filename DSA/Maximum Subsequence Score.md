# Maximum Subsequence Score

## Problem Summary

We are given:

```cpp
nums1
nums2
```

We must choose exactly:

```text
k indices
```

Score is defined as:

```text
(sum of chosen nums1 values)
*
(minimum of chosen nums2 values)
```

Goal:

```text
maximize the score
```

---

# Key Confusion in Problem

The score is NOT:

```text
nums1[i] * nums2[i]
```

Instead:

1. Sum all chosen `nums1`
2. Find minimum among chosen `nums2`
3. Multiply them

---

# Example

Suppose chosen pairs are:

```text
(1,2)
(3,3)
(2,4)
```

Then:

## Sum of nums1

```text
1 + 3 + 2 = 6
```

## Minimum nums2

```text
min(2,3,4) = 2
```

## Final Score

```text
6 * 2 = 12
```

---

# Brute Force Idea

Try all possible subsequences of size `k`.

Problem:

```text
Too many combinations
```

Complexity becomes exponential.

Not feasible.

---

# Core Greedy Insight

The difficult part is:

We need BOTH:

```text
large nums1 sum
AND
large minimum nums2
```

These two goals conflict with each other.

---

# Big Observation

Suppose we FIX the minimum `nums2`.

Example:

```text
Assume minimum nums2 = 4
```

Then score becomes:

```text
(sum of nums1) * 4
```

Now:

```text
4 is fixed
```

So we only need to maximize:

```text
sum(nums1)
```

This simplifies the problem dramatically.

---

# Why Sorting Helps

We sort pairs by:

```text
nums2 descending
```

Store pairs as:

```cpp
{nums2, nums1}
```

Example:

```text
(2,4)
(3,3)
(1,2)
(3,1)
```

After sorting descending by nums2:

```text
(2,4)
(3,3)
(1,2)
(3,1)
```

becomes:

```text
(4,2)
(3,3)
(2,1)
(1,3)
```

---

# Important Mental Model

When iterating left → right:

```text
current nums2 automatically becomes the minimum
```

Why?

Because all future nums2 values are smaller.

So at each step:

```text
current nums2 = minimum nums2
```

for any chosen subsequence using current and previous elements.

---

# Heap Insight

Now the problem becomes:

```text
For this fixed minimum nums2,
find largest possible nums1 sum
```

We need to maintain:

```text
largest k nums1 values seen so far
```

Efficiently.

---

# Why Min Heap?

Suppose:

```text
k = 3
```

Current selected nums1 values:

```text
10, 8, 5
```

If new value arrives:

```text
12
```

We should keep:

```text
12, 10, 8
```

and remove:

```text
5
```

So we need fast access to:

```text
smallest selected nums1
```

That is exactly what min heap gives.

---

# Heap Mental Model

```text
Keep only top-k useful nums1 values
```

If heap size exceeds `k`:

```text
remove the weakest (smallest) nums1
```

---

# Complete Algorithm

## Step 1

Create pairs:

```cpp
{nums2[i], nums1[i]}
```

---

## Step 2

Sort descending:

```cpp
sort(pairs.rbegin(), pairs.rend());
```

---

## Step 3

Use min heap to maintain best `k` nums1 values.

---

## Step 4

Maintain running sum of elements currently inside heap.

---

## Step 5

If heap size exceeds `k`:

```cpp
sum -= pq.top();
pq.pop();
```

---

## Step 6

When heap size becomes exactly `k`:

```cpp
score = sum * current nums2
```

Update answer.

---

# Why This Works

Sorting guarantees:

```text
current nums2 = minimum nums2
```

Heap guarantees:

```text
largest possible nums1 sum
```

Together they produce optimal score.

---

# Dry Run

## Input

```text
nums1 = [1,3,3,2]
nums2 = [2,1,3,4]
k = 3
```

---

# Create Pairs

```text
(2,1)
(1,3)
(3,3)
(4,2)
```

---

# Sort Descending

```text
(4,2)
(3,3)
(2,1)
(1,3)
```

---

# Iteration 1

Take:

```text
(4,2)
```

Heap:

```text
[2]
```

Sum:

```text
2
```

Need size 3 → continue.

---

# Iteration 2

Take:

```text
(3,3)
```

Heap:

```text
[2,3]
```

Sum:

```text
5
```

Still size < 3.

---

# Iteration 3

Take:

```text
(2,1)
```

Heap:

```text
[1,2,3]
```

Sum:

```text
6
```

Heap size == 3

Score:

```text
6 * 2 = 12
```

Answer:

```text
12
```

---

# Iteration 4

Take:

```text
(1,3)
```

Heap after push:

```text
[1,2,3,3]
```

Remove smallest:

```text
1
```

Heap becomes:

```text
[2,3,3]
```

Sum:

```text
8
```

Score:

```text
8 * 1 = 8
```

Maximum remains:

```text
12
```

---

# Final Code

```cpp
class Solution {
public:
    long long maxScore(vector<int>& nums1, vector<int>& nums2, int k) {

        int n = nums1.size();

        vector<pair<int,int>> pairs;

        for(int i = 0; i < n; i++){
            pairs.push_back({nums2[i], nums1[i]});
        }

        sort(pairs.rbegin(), pairs.rend());

        priority_queue<int, vector<int>, greater<int>> minHeap;

        long long currentSum = 0;
        long long maxScore = 0;

        for(auto &[num2, num1] : pairs){

            currentSum += num1;
            minHeap.push(num1);

            if(minHeap.size() > k){
                currentSum -= minHeap.top();
                minHeap.pop();
            }

            if(minHeap.size() == k){
                maxScore = max(maxScore, currentSum * num2);
            }
        }

        return maxScore;
    }
};
```

---

# Complexity Analysis

## Sorting

```text
O(n log n)
```

---

## Heap Operations

Each insertion/removal:

```text
O(log k)
```

Total:

```text
O(n log k)
```

---

# Final Complexity

## Time

```text
O(n log n)
```

---

## Space

```text
O(n + k)
```

---

# Common Mistakes

## Mistake 1

Using:

```cpp
else if
```

instead of separate `if`.

Wrong because after removing element:

```text
heap size may become exactly k
```

and we still need to calculate answer.

---

## Mistake 2

Forgetting:

```cpp
sum -= pq.top()
```

before popping.

Heap and sum must always represent same elements.

---

## Mistake 3

Using max heap.

We need quick access to:

```text
smallest selected nums1
```

So min heap is required.

---

## Mistake 4

Using `int`.

Multiplication may overflow.

Use:

```cpp
long long
```

---

# Recognition Pattern

This is a classic pattern:

```text
Fix one parameter using sorting
Optimize another using heap
```

---

# Important Mental Models

## Sorting

```text
Current nums2 becomes minimum nums2
```

---

## Heap

```text
Keep only top-k useful nums1 values
```

---

## Overall Strategy

```text
Fix minimum nums2
Maximize nums1 sum
```

This single sentence captures the whole problem.
