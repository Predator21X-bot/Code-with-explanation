# 84. Largest Rectangle in Histogram (Hard)

**Problem Link:** https://leetcode.com/problems/largest-rectangle-in-histogram/

---

# Pattern Recognition

### Pattern
- **Monotonic Increasing Stack**

### Why?

For every bar, we want to know:

- Previous Smaller Element
- Next Smaller Element

These two boundaries determine the maximum width over which the current bar can extend.

A monotonic increasing stack allows us to efficiently find these boundaries in **O(n)**.

---

# Intuition

For every bar:

- Height is fixed.
- Width is determined by how far the bar can expand left and right before hitting a **smaller** bar.

Whenever we encounter a **smaller** bar:

- The taller bars before it can no longer expand.
- Their maximum possible rectangle is finalized.
- Pop them and calculate their area.

---

# Stack Invariant

The stack always stores **indices** of bars in **increasing order of heights**.

Example:

```text
Heights: [2, 5, 6]

Stack:
2
5
6
```

Every new height is **greater than or equal** to the previous one.

---

# Algorithm

1. Traverse the histogram from left to right.
2. Store **indices** in the stack.
3. While current height is **smaller** than the stack top:
   - Pop the bar.
   - Calculate its maximum possible rectangle.
   - Update maximum area.
4. Push current index.
5. After traversal, process all remaining bars.
6. Return maximum area.

---

# Width Formula

After popping a bar:

Two cases are possible.

### Case 1: Stack becomes empty

No smaller element exists on the left.

```cpp
width = currentIndex;
```

---

### Case 2: Stack is not empty

Previous smaller element exists.

```cpp
width = currentIndex - st.top() - 1;
```

---

# Cleanup Width

After traversing the entire array:

### Stack empty after pop

```cpp
width = n;
```

---

### Stack not empty

```cpp
width = n - st.top() - 1;
```

---

# Why check `st.empty()` again after popping?

We already checked:

```cpp
while (!st.empty())
```

But inside the loop we do:

```cpp
st.pop();
```

The pop operation may make the stack empty.

Example:

```text
Before pop:

Stack:
[0]
```

After:

```text
Stack:
[]
```

Calling

```cpp
st.top()
```

now causes a runtime error.

Hence:

```cpp
int width = st.empty() ? i : i - st.top() - 1;
```

---

# Code

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {

        int maxArea = 0;
        stack<int> st;

        for (int i = 0; i < heights.size(); i++) {

            while (!st.empty() && heights[i] < heights[st.top()]) {

                int height = heights[st.top()];
                st.pop();

                int width = st.empty()
                                ? i
                                : i - st.top() - 1;

                maxArea = max(maxArea, height * width);
            }

            st.push(i);
        }

        while (!st.empty()) {

            int height = heights[st.top()];
            st.pop();

            int width = st.empty()
                            ? heights.size()
                            : heights.size() - st.top() - 1;

            maxArea = max(maxArea, height * width);
        }

        return maxArea;
    }
};
```

---

# Complexity

- **Time:** O(n)
- **Space:** O(n)

---

# Key Takeaways

- Store **indices**, not heights.
- Stack is **monotonically increasing**.
- Pop when current height is **smaller**.
- When popping:
  - Height = popped bar.
  - Width = distance between previous smaller and next smaller.
- Every bar is pushed once and popped once → **O(n)**.

---

# Similar Problems

- Daily Temperatures
- Next Greater Element
- Next Smaller Element
- Sum of Subarray Minimums
- Maximal Rectangle
