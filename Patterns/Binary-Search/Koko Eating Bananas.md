# Koko Eating Bananas (LeetCode 875)

## Pattern

**Binary Search on Answer (Parametric Search)**

Unlike classic Binary Search, we are **not searching an index**.

We are searching for the **minimum valid eating speed**.

---

# Investigation

### Input

* `piles[]` → Number of bananas in each pile.
* `h` → Total hours available.

### Output

Return the **minimum integer eating speed `k`** such that Koko finishes all bananas within `h` hours.

### Constraints

* `1 <= piles.length <= 10^4`
* `1 <= piles[i] <= 10^9`
* `piles.length <= h <= 10^9`

---

# Edge Cases

* Only one pile.
* `h == piles.length` (Koko has exactly one hour per pile.)
* Very large pile sizes.
* All piles have the same size.
* Maximum constraints.

---

# Special Properties

* Array is **not sorted**.
* Duplicates are allowed.
* Order of piles does **not** matter.
* Koko eats from **only one pile per hour**.
* If a pile finishes early, she waits until the next hour.
* We need the **minimum** valid eating speed.

---

# Key Observation

Possible eating speeds range from

```text
1 → max(piles)
```

Example

```text
piles = [3,6,7,11]

Possible speeds

1 2 3 4 5 6 7 8 9 10 11
```

For every speed, we can determine whether it is **valid**.

Example (`h = 8`)

| Speed | Hours | Valid |
| ----: | ----: | :---: |
|     1 |    27 |   ❌   |
|     2 |    15 |   ❌   |
|     3 |    10 |   ❌   |
|     4 |     8 |   ✅   |
|     5 |     8 |   ✅   |
|     6 |     6 |   ✅   |
|     7 |     5 |   ✅   |
|     8 |     5 |   ✅   |

Notice the pattern

```text
❌ ❌ ❌ ✅ ✅ ✅ ✅
```

There is a **single boundary**.

We need the **first valid speed**.

This monotonic property allows Binary Search.

---

# Search Space

We are **not** searching the array.

We are searching the answer.

```text
left = 1
right = max(piles)
```

---

# Feasibility Function

For a candidate speed `mid`,

calculate total hours required.

For every pile,

```text
hours += ceil(pile / mid)
```

If

```text
hours <= h
```

the speed is **valid**.

Otherwise,

the speed is **too slow**.

---

# Ceiling Division Formula

Instead of

```cpp
ceil((double)pile / speed)
```

we can use pure integer arithmetic:

```cpp
(pile + speed - 1) / speed
```

General formula:

```text
ceil(a / b)
=
(a + b - 1) / b
```

Example

```text
pile = 7
speed = 4

(7 + 4 - 1) / 4

10 / 4

2
```

No floating point operations are required.

---

# Algorithm

1. Find the maximum pile.
2. Search the speed range `[1, maxPile]`.
3. Compute the middle speed.
4. Calculate the total hours needed at that speed.
5. If the speed is valid (`hours <= h`), search for a smaller valid speed.
6. Otherwise, search for a larger speed.
7. Return the minimum valid speed.

---

# Production Pseudocode

```text
left = 1
right = maximum element in piles

while left < right

    mid = left + (right - left) / 2

    hours = 0

    for every pile

        hours += ceil(pile / mid)

    if hours <= h
        right = mid

    else
        left = mid + 1

return left
```

---

# Production C++

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1;
        int right = *max_element(piles.begin(), piles.end());

        while (left < right) {
            int mid = left + (right - left) / 2;

            long long hours = 0;

            for (int pile : piles) {
                hours += (pile + mid - 1) / mid;
                // Equivalent:
                // hours += ceil((double)pile / mid);
            }

            if (hours <= h)
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }
};
```

---

# Dry Run

Example

```text
piles = [3,6,7,11]
h = 8
```

Search Space

```text
1 ------------------ 11
```

### Iteration 1

```text
mid = 6
```

Hours

```text
3  -> 1
6  -> 1
7  -> 2
11 -> 2

Total = 6
```

```text
6 <= 8
```

Valid

Search smaller speed

```text
right = mid
```

---

### Iteration 2

```text
mid = 3
```

Hours

```text
3  -> 1
6  -> 2
7  -> 3
11 -> 4

Total = 10
```

```text
10 > 8
```

Too slow

Search larger speed

```text
left = mid + 1
```

---

Eventually

```text
left == right == 4
```

Return

```text
4
```

---

# Complexity

### Time

Finding maximum pile

```text
O(n)
```

Binary Search

```text
O(log(maxPile))
```

Each iteration checks every pile

```text
O(n)
```

Overall

```text
O(n × log(maxPile))
```

---

### Space

```text
O(1)
```

---

# Common Mistakes

### ❌ Binary searching the array

```cpp
left = 0;
right = piles.size() - 1;
```

✅ Correct

```cpp
left = 1;
right = maxPile;
```

---

### ❌ Integer division

```cpp
hours += pile / speed;
```

Wrong because

```text
7 / 4 = 1
```

Need

```cpp
hours += (pile + speed - 1) / speed;
```

---

### ❌ Using `int` for hours

Maximum total hours can exceed `int`.

Use

```cpp
long long hours = 0;
```

---

### ❌ Returning `mid`

`mid` changes every iteration.

Return

```cpp
return left;
```

---

# Binary Search on Answer Template

```cpp
while (left < right) {

    int mid = left + (right - left) / 2;

    if (valid(mid))
        right = mid;
    else
        left = mid + 1;
}

return left;
```

The only thing that changes from problem to problem is the **`valid(mid)`** function.

---

# Interview Takeaway

> **Whenever a problem asks for the minimum or maximum feasible answer, define the answer search space, build a feasibility (`valid`) function, and binary search the first valid answer. Koko Eating Bananas is the classic introduction to Binary Search on Answer.**
