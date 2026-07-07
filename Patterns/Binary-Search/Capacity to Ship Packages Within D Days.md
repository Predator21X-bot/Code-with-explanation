# Capacity to Ship Packages Within D Days (LeetCode 1011)

## Problem
Given an array `weights` where `weights[i]` represents the weight of the `i-th` package and an integer `days`, return the **minimum ship capacity** required to ship all packages within the given number of days.

**Important Constraints**
- Packages must be shipped **in order**.
- Packages **cannot be split**.
- The goal is to minimize the ship's capacity.

---

# Pattern Recognition

This is a **Binary Search on Answer** problem.

Although the array is **not sorted**, the **answer space is sorted**.

Suppose the capacities are:

```
Capacity

10   11   12   13   14   15
 ❌    ❌    ✅    ✅    ✅    ✅
```

If a capacity works, every larger capacity will also work.

We need to find the **first valid capacity**.

---

# Search Space

### Minimum possible capacity

The ship must at least carry the heaviest package.

```cpp
left = max(weights)
```

### Maximum possible capacity

The ship carries every package in one day.

```cpp
right = sum(weights)
```

So our search space becomes

```text
[max(weights), sum(weights)]
```

---

# Key Observation

Binary Search is **not performed on the array**.

It is performed on the possible ship capacities.

For every capacity (`mid`), we ask:

> Can I ship all packages within the given number of days?

This is our **valid()** function.

---

# Valid(mid)

Treat `mid` as the ship's capacity.

Simulate loading packages while maintaining their order.

Initialize:

```cpp
daysRequired = 1;
currentLoad = 0;
```

Traverse every package.

### If package fits

```cpp
currentLoad += weight;
```

### Otherwise

Start a new day.

```cpp
daysRequired++;
currentLoad = weight;
```

At the end,

```
daysRequired <= days
```

means the capacity is valid.

---

# Why daysRequired = 1?

We always begin shipping on **Day 1**.

Example:

```
weights = [3,2,2]
capacity = 10
```

Everything fits on Day 1.

Answer:

```
daysRequired = 1
```

If initialized to 0, we would incorrectly return 0 days.

Think of it as:

```
Open Day 1
↓

Start loading packages
↓

Whenever capacity exceeds,
start Day 2, Day 3...
```

---

# Binary Search Logic

If

```cpp
daysRequired <= days
```

Current capacity works.

Try a smaller capacity.

```cpp
right = mid;
```

Else

Capacity is too small.

Increase it.

```cpp
left = mid + 1;
```

---

# Algorithm

1. Find the maximum package weight.
2. Find the total sum of all weights.
3. Set:
   - `left = max(weights)`
   - `right = sum(weights)`
4. While `left < right`
   - Calculate `mid`.
   - Simulate shipping using capacity = `mid`.
   - Calculate `daysRequired`.
   - If `daysRequired <= days`
     - `right = mid`
   - Else
     - `left = mid + 1`
5. Return `left`.

---

# Production C++ Solution

```cpp
class Solution {
public:
    int shipWithinDays(vector<int>& weights, int days) {
        int left = *max_element(weights.begin(), weights.end());

        long long right = 0;
        for (int weight : weights)
            right += weight;

        while (left < right) {
            int mid = left + (right - left) / 2;

            int daysRequired = 1;
            int currentLoad = 0;

            for (int weight : weights) {
                if (currentLoad + weight > mid) {
                    daysRequired++;
                    currentLoad = weight;
                } else {
                    currentLoad += weight;
                }
            }

            if (daysRequired <= days)
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }
};
```

---

# Complexity Analysis

Let

- `n = weights.size()`
- `S = sum(weights)`

Binary Search range:

```
[max(weights), sum(weights)]
```

Iterations:

```
O(log S)
```

Each iteration scans the entire array.

```
O(n)
```

Overall Time Complexity

```
O(n log S)
```

Space Complexity

```
O(1)
```

---

# Common Mistakes

### ❌ Using `weights[mid]`

`mid` represents **capacity**, not an array index.

Correct:

```cpp
if (currentLoad + weight > mid)
```

---

### ❌ Updating Binary Search inside the for-loop

Wrong:

```cpp
for (...) {
    ...

    if(daysRequired <= days)
        right = mid;
}
```

The entire simulation must finish before deciding.

Correct:

```cpp
for (...) {
    ...
}

if(daysRequired <= days)
    right = mid;
else
    left = mid + 1;
```

---

### ❌ Initializing `daysRequired = 0`

Shipping starts on Day 1.

Correct:

```cpp
daysRequired = 1;
```

---

### ❌ Using `left = 1`

The ship cannot carry less than the heaviest package.

Correct:

```cpp
left = max(weights);
```

---

# Binary Search on Answer Template

```cpp
left = minimum possible answer;
right = maximum possible answer;

while (left < right) {
    mid = left + (right - left) / 2;

    if (valid(mid))
        right = mid;
    else
        left = mid + 1;
}

return left;
```

Only the **valid(mid)** function changes from one problem to another.

- Koko Eating Bananas → Calculate required hours.
- Capacity to Ship Packages → Calculate required days.
- Future Binary Search on Answer problems → Define a new validity function.
