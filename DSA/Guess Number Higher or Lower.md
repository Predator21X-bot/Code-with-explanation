# Guess Number Higher or Lower

## Problem Summary

We must guess a hidden number between:

```text id="7b4r1m"
1 to n
```

We are given an API:

```cpp id="v3q8p6"
guess(num)
```

which returns:

| Return Value | Meaning                    |
| ------------ | -------------------------- |
| `-1`         | guessed number is too HIGH |
| `1`          | guessed number is too LOW  |
| `0`          | guessed correctly          |

Goal:

```text id="q9m2k5"
find the hidden number
```

---

# Important Understanding

We NEVER directly know the hidden number.

We can only get information through:

```cpp id="e6w9r3"
guess(mid)
```

The API tells us whether:

```text id="j7t4v8"
go left
or
go right
```

This is exactly what makes Binary Search possible.

---

# Why Binary Search Works

At every step:

```text id="r2x5m1"
half the search space becomes impossible
```

---

# Example

Suppose:

```text id="f8n6q4"
n = 10
hidden number = 6
```

---

# Step 1

Search range:

```text id="m1p7w2"
[1 ... 10]
```

Middle:

```text id="t5r8k9"
5
```

Call:

```cpp id="z4v6n1"
guess(5)
```

Returns:

```text id="w7q2m8"
1
```

Meaning:

```text id="g3k5r9"
5 is too small
```

So answer must lie in:

```text id="p9n4x6"
[6 ... 10]
```

Left half becomes useless.

---

# Step 2

Search range:

```text id="s8w2m4"
[6 ... 10]
```

Middle:

```text id="v1k9r7"
8
```

Call:

```cpp id="b7m5q1"
guess(8)
```

Returns:

```text id="n4r8x2"
-1
```

Meaning:

```text id="k2p6m9"
8 is too large
```

So answer must lie in:

```text id="x5t9r3"
[6 ... 7]
```

Right half becomes useless.

---

# Binary Search Mental Model

```text id="c4n7m1"
Continuously eliminate impossible half
```

Binary Search is NOT about arrays only.

It is about:

```text id="q7v1m5"
searching efficiently inside a valid range
```

---

# Recognition Pattern

Whenever problem contains:

* higher/lower
* too big/too small
* monotonic condition
* ordered search space

think:

# Binary Search

---

# Search Space

We maintain:

```cpp id="m9r3k7"
left = 1
right = n
```

because hidden number lies between:

```text id="r6x4p8"
1 to n
```

---

# Mid Formula

Use:

```cpp id="v8n2m6"
int mid = left + (right - left) / 2;
```

instead of:

```cpp id="x2q7m4"
(left + right) / 2
```

to avoid integer overflow.

---

# Decision Logic

## Case 1

```cpp id="z9k4m1"
guess(mid) == 0
```

Meaning:

```text id="j5n8r2"
correct answer found
```

Return:

```cpp id="h3v7m9"
mid
```

---

## Case 2

```cpp id="k8m2q5"
guess(mid) == 1
```

Meaning:

```text id="f6x9r4"
mid is too small
```

So answer lies on RIGHT side.

Update:

```cpp id="p7m4v1"
left = mid + 1;
```

---

## Case 3

```cpp id="s1k8m6"
guess(mid) == -1
```

Meaning:

```text id="y9r2m5"
mid is too large
```

So answer lies on LEFT side.

Update:

```cpp id="w4m7k2"
right = mid - 1;
```

---

# Important Binary Search Flow

At every step:

1. Compute middle
2. Ask API
3. Eliminate impossible half
4. Continue searching remaining half

---

# Complete Algorithm

## Step 1

Initialize search space:

```cpp id="t2m5v8"
left = 1
right = n
```

---

## Step 2

Run binary search:

```cpp id="g8r3m1"
while(left <= right)
```

---

## Step 3

Compute middle:

```cpp id="r4m9k7"
mid = left + (right - left) / 2
```

---

## Step 4

Call API:

```cpp id="x6m2r8"
result = guess(mid)
```

---

## Step 5

Adjust search space based on result.

---

# Dry Run

## Input

```text id="m5r8k1"
n = 10
hidden = 6
```

---

# Iteration 1

```text id="f2m9r7"
left = 1
right = 10
mid = 5
```

API:

```text id="z7m4k2"
guess(5) = 1
```

Too small.

Update:

```text id="j8r2m4"
left = 6
```

---

# Iteration 2

```text id="y5m8k3"
left = 6
right = 10
mid = 8
```

API:

```text id="k4m7r1"
guess(8) = -1
```

Too large.

Update:

```text id="s2m9k6"
right = 7
```

---

# Iteration 3

```text id="p6r2m8"
left = 6
right = 7
mid = 6
```

API:

```text id="n5m8r4"
guess(6) = 0
```

Correct answer found.

Return:

```text id="t7m2k5"
6
```

---

# Final Code

```cpp id="x9m4r7"
class Solution {
public:
    int guessNumber(int n) {

        int left = 1;
        int right = n;

        while(left <= right){

            int mid = left + (right - left) / 2;

            int result = guess(mid);

            if(result == 0){
                return mid;
            }
            else if(result == 1){
                left = mid + 1;
            }
            else{
                right = mid - 1;
            }
        }

        return -1;
    }
};
```

---

# Complexity Analysis

At every step:

```text id="m1r7k4"
half search space is removed
```

So total iterations:

```text id="f9m2r6"
log₂(n)
```

---

# Time Complexity

```text id="z4m8k1"
O(log n)
```

---

# Space Complexity

Only variables used:

```text id="r5m1k9"
left
right
mid
```

So:

```text id="v7m3r2"
O(1)
```

---

# Common Mistakes

## Mistake 1

Using:

```cpp id="t9m4k7"
while(guess(mid))
```

Wrong because binary search should depend on:

```text id="s4m8r1"
valid search range
```

Correct:

```cpp id="q2m7k5"
while(left <= right)
```

---

## Mistake 2

Calculating `mid` outside loop.

Wrong because:

```text id="n8m4r6"
left/right change every iteration
```

So mid must be recalculated each time.

---

## Mistake 3

Returning:

```cpp id="m5r9k2"
-1 or 1
```

Those values belong to API, not our answer.

We return:

```text id="j4m8r7"
the hidden number
```

---

## Mistake 4

Using:

```cpp id="w1m6k8"
(left + right) / 2
```

May overflow for large values.

Safer formula:

```cpp id="y7m2r5"
left + (right - left) / 2
```

---

# Recognition Pattern Learned

This is:

```text id="k6m3r9"
Binary Search on Numeric Range
```

NOT binary search on array.

Very important distinction.

---

# Important Mental Models

## Binary Search

```text id="p8m4r2"
Eliminate impossible half
```

---

## API Response

```text id="x3m7k1"
Direction indicator
```

API tells:

```text id="m9r5k4"
move left
or
move right
```

---

# Biggest Takeaway

Binary Search is fundamentally about:

```text id="f4m8r6"
shrinking search space efficiently
```

not about arrays specifically.
