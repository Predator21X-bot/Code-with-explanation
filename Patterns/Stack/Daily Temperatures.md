# Daily Temperatures (LeetCode 739)

**Difficulty:** Medium
**Pattern:** Monotonic Stack (Next Greater Element)

---

# Problem Statement

Given an array `temperatures` where `temperatures[i]` represents the temperature on the `i-th` day, return an array `answer` such that:

* `answer[i]` = number of days until a warmer temperature.
* If no warmer temperature exists, `answer[i] = 0`.

### Example

```text
Input:
temperatures = [73,74,75,71,69,72,76,73]

Output:
[1,1,4,2,1,1,0,0]
```

---

# Restating the Problem

For every day:

* Look towards the **right**.
* Find the **first warmer temperature**.
* Return the number of days between them.
* If none exists, return `0`.

---

# Pattern Recognition

At first glance, we compare every element with all elements to its right.

Brute Force:

```text
For every temperature
    Traverse right
    Find first warmer temperature
```

Time Complexity:

```text
O(n²)
```

This is too slow.

Instead, notice:

* We only care about the **next warmer temperature**
* Elements that can never become an answer should be discarded.

This is exactly the **Monotonic Stack** pattern.

---

# Why Traverse Right → Left?

Suppose we are standing at index `i`.

We need information about the **future** (right side).

If we process from right to left:

* every future day has already been processed
* the stack already contains possible answers

So we always have the information we need.

---

# Main Observation

The stack stores **indices**, not temperatures.

Why?

Because the answer requires:

```cpp
days = nextIndex - currentIndex
```

If we stored only temperatures, we wouldn't know the index.

Temperature can always be accessed as:

```cpp
temperatures[stack.top()]
```

---

# Monotonic Stack Invariant

The stack always contains indices of temperatures in **strictly decreasing order of temperature** (from bottom to top).

Before processing a new temperature:

* Remove every temperature that is smaller than or equal to the current temperature.
* They can never be the next warmer day for the current element or for any element further left.

---

# Algorithm

1. Create an answer array initialized with `0`.
2. Create an empty stack to store indices.
3. Traverse the array from **right to left**.
4. While the stack is not empty and:

```cpp
temperatures[stack.top()] <= temperatures[i]
```

pop the stack.

5. If the stack is not empty:

```cpp
answer[i] = stack.top() - i;
```

6. Push the current index into the stack.
7. Return the answer array.

---

# Pseudocode

```cpp
answer = vector(n, 0)
stack<int> st

for i = n-1 to 0

    while stack not empty
          and temperatures[stack.top()] <= temperatures[i]
        stack.pop()

    if stack not empty
        answer[i] = stack.top() - i

    stack.push(i)

return answer
```

---

# C++ Solution

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {

        int n = temperatures.size();
        vector<int> answer(n, 0);
        stack<int> st;

        for (int i = n - 1; i >= 0; i--) {

            while (!st.empty() &&
                   temperatures[st.top()] <= temperatures[i]) {
                st.pop();
            }

            if (!st.empty())
                answer[i] = st.top() - i;

            st.push(i);
        }

        return answer;
    }
};
```

---

# Dry Run

```text
temperatures

73 74 75 71 69 72 76 73
```

Start from the right.

### i = 7 (73)

Stack = []

Answer = 0

Push 7

Stack:

```text
73
```

---

### i = 6 (76)

73 ≤ 76

Pop.

Stack empty.

Answer = 0

Push 6.

Stack:

```text
76
```

---

### i = 5 (72)

76 > 72

Next warmer day = index 6

Answer:

```text
6 - 5 = 1
```

Push 5.

Stack:

```text
76
72
```

---

### i = 4 (69)

72 > 69

Answer:

```text
5 - 4 = 1
```

Push 4.

Continue similarly.

Final answer:

```text
[1,1,4,2,1,1,0,0]
```

---

# Complexity Analysis

### Time Complexity

```text
O(n)
```

Reason:

* Every index is pushed once.
* Every index is popped at most once.

Total operations:

```text
Pushes = n
Pops = n

Total = O(2n) = O(n)
```

---

### Space Complexity

```text
O(n)
```

For the stack in the worst case.

---

# Interview Takeaways

* This is a **Monotonic Stack** problem.
* Traverse **right → left** because answers lie on the right.
* Store **indices**, not values.
* Pop all useless elements before answering.
* The stack top always represents the **nearest warmer temperature**.

---

# Similar Problems

* Next Greater Element I
* Next Greater Element II
* Stock Span
* Largest Rectangle in Histogram
* Trapping Rain Water (uses monotonic stack approach)
* Next Smaller Element
