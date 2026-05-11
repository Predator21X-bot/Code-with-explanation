# 🧩 Kth Largest Element in an Array

### 🔗 Problem Link

https://leetcode.com/problems/kth-largest-element-in-an-array/

---

# 🧠 Problem Understanding

Given:

* integer array `nums`
* integer `k`

👉 Goal:

```text id="kln1"
find kth largest element
```

NOT:

```text id="kln2"
kth distinct element
```

Duplicates also count.

---

# 🔍 Example 1

```text id="kln3"
nums = [3,2,1,5,6,4]
k = 2
```

Sorted descending:

```text id="kln4"
6,5,4,3,2,1
```

---

# ✅ 2nd largest

```text id="kln5"
5
```

---

# 🔍 Example 2

```text id="kln6"
nums = [3,2,3,1,2,4,5,5,6]
k = 4
```

Sorted descending:

```text id="kln7"
6,5,5,4,3,3,2,2,1
```

---

# ✅ 4th largest

```text id="kln8"
4
```

---

# 🧠 Brute Force Idea

Sort array in descending order.

Then:

```cpp id="kln9"
return nums[k-1];
```

---

# 💻 Sorting Solution

```cpp id="kln10"
sort(nums.begin(), nums.end(), greater<int>());

return nums[k-1];
```

---

# ⏱ Complexity

Sorting:

```text id="kln11"
O(n log n)
```

---

# 🧠 Better Idea

We do NOT need:

```text id="kln12"
fully sorted array
```

We only care about:

```text id="kln13"
top k largest elements
```

---

# 🔥 Best Approach — Min Heap

---

# 🧠 What is Heap?

Heap is a:

```text id="kln14"
Complete Binary Tree
```

implemented internally using:

```text id="kln15"
array/vector
```

---

# 🌳 Heap Types

---

## Max Heap

```text id="kln16"
largest element at top
```

---

## Min Heap

```text id="kln17"
smallest element at top
```

---

# 🧠 Why Min Heap here?

We want to maintain:

```text id="kln18"
k largest elements only
```

---

# 🔥 Key Insight

If heap size becomes:

```text id="kln19"
greater than k
```

remove:

```text id="kln20"
smallest element
```

because it can never become kth largest anymore.

---

# 🧠 Heap ALWAYS stores

```text id="kln21"
k largest elements seen so far
```

---

# 🔥 MOST IMPORTANT INSIGHT

In min heap:

```text id="kln22"
smallest among top-k elements stays at top
```

That element is exactly:

```text id="kln23"
kth largest
```

---

# 🔍 Example Dry Run

```text id="kln24"
nums = [3,2,1,5,6,4]
k = 2
```

---

# 🧩 Add 3

Heap:

```text id="kln25"
[3]
```

---

# 🧩 Add 2

Heap:

```text id="kln26"
[2,3]
```

---

# 🧩 Add 1

Heap:

```text id="kln27"
[1,3,2]
```

Size > 2

Remove smallest:

```text id="kln28"
1
```

Heap:

```text id="kln29"
[2,3]
```

---

# 🧩 Add 5

Heap:

```text id="kln30"
[2,3,5]
```

Remove smallest:

```text id="kln31"
2
```

Heap:

```text id="kln32"
[3,5]
```

---

# 🧩 Add 6

Heap:

```text id="kln33"
[3,5,6]
```

Remove:

```text id="kln34"
3
```

Heap:

```text id="kln35"
[5,6]
```

---

# 🧩 Add 4

Heap:

```text id="kln36"
[4,6,5]
```

Remove:

```text id="kln37"
4
```

Heap:

```text id="kln38"
[5,6]
```

---

# ✅ Final Answer

```cpp id="kln39"
pq.top() = 5
```

---

# 🧠 Priority Queue Syntax

---

# ✅ Max Heap (default)

```cpp id="kln40"
priority_queue<int> pq;
```

Largest element stays at top.

---

# ✅ Min Heap

```cpp id="kln41"
priority_queue<
    int,
    vector<int>,
    greater<int>
> pq;
```

Smallest element stays at top.

---

# 🧠 Meaning of syntax

---

## `int`

Heap stores integers.

---

## `vector<int>`

Internal container used by heap.

---

## `greater<int>`

Comparator deciding:

```text id="kln42"
smaller element gets higher priority
```

👉 creates MIN heap.

---

# 🧠 Heap Internal Structure

Heap is internally:

```text id="kln43"
Complete Binary Tree
```

stored in:

```text id="kln44"
array/vector
```

---

# 🔍 Example

Heap:

```text id="kln45"
        10
       /  \
      5    8
     / \
    2   3
```

Stored as:

```text id="kln46"
[10,5,8,2,3]
```

---

# ⚡ Heap Property

---

## Max Heap

```text id="kln47"
parent >= children
```

---

## Min Heap

```text id="kln48"
parent <= children
```

---

# ❗ Heap is NOT BST

Heap only guarantees:

```text id="kln49"
parent-child ordering
```

NOT full sorting.

---

# ⚡ Heap Operations

---

## Insert

```cpp id="kln50"
pq.push(x);
```

---

## Top element

```cpp id="kln51"
pq.top();
```

---

## Remove top

```cpp id="kln52"
pq.pop();
```

---

# 💻 Full Code

```cpp id="kln53"
class Solution {
public:

    int findKthLargest(vector<int>& nums, int k) {

        // min heap
        priority_queue<
            int,
            vector<int>,
            greater<int>
        > pq;

        for (int num : nums) {

            pq.push(num);

            // keep only k elements
            if (pq.size() > k) {
                pq.pop();
            }
        }

        return pq.top();
    }
};
```

---

# 🧠 Mental Model (REVISION KEY)

> “Keep only top-k largest elements”

---

# 🔥 Key Heap Insight

Min heap top always represents:

```text id="kln54"
weakest element among current top-k
```

which becomes:

```text id="kln55"
kth largest overall
```

---

# ⏱ Complexity

---

## Heap size always

```text id="kln56"
k
```

---

## Each insertion/removal

```text id="kln57"
O(log k)
```

---

# ✅ Total Complexity

```text id="kln58"
O(n log k)
```

Better than sorting:

```text id="kln59"
O(n log n)
```

---

# ⚡ Space Complexity

Heap stores only:

```text id="kln60"
k elements
```

So:

```text id="kln61"
O(k)
```

---

# ❗ Common Mistakes

* ❌ Using max heap unnecessarily
* ❌ Forgetting to maintain heap size k
* ❌ Confusing kth largest with kth distinct
* ❌ Thinking heap is fully sorted

---

# 🧠 Patterns Learned

* Top-k problems
* Min heap maintenance
* Partial sorting
* Priority queue usage

---

# 🏷 Tags

* Heap
* Priority Queue
* Sorting
* Top-K Problems

---

# 🔥 Recognition Pattern

Use min heap when:

```text id="kln62"
need to maintain largest k elements
```

Use max heap when:

```text id="kln63"
need quick access to largest element
```
