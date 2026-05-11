# 🧩 Smallest Number in Infinite Set

### 🔗 Problem Link

https://leetcode.com/problems/smallest-number-in-infinite-set/

---

# 🧠 Problem Understanding

Initially infinite set contains:

```text id="sinf1"
1,2,3,4,5,6,7...
```

---

# 🧩 Operations

---

# 1️⃣ popSmallest()

Remove and return:

```text id="sinf2"
smallest available number
```

---

# 2️⃣ addBack(num)

Add number back ONLY IF:

```text id="sinf3"
it was removed earlier
```

---

# 🔍 Example

Initially:

```text id="sinf4"
1 2 3 4 5 ...
```

---

## popSmallest()

Returns:

```text id="sinf5"
1
```

Remaining:

```text id="sinf6"
2 3 4 5 ...
```

---

## popSmallest()

Returns:

```text id="sinf7"
2
```

Remaining:

```text id="sinf8"
3 4 5 ...
```

---

## addBack(1)

Set becomes:

```text id="sinf9"
1 3 4 5 ...
```

because:

```text id="sinf10"
1 was previously removed
```

---

## popSmallest()

Returns:

```text id="sinf11"
1
```

again.

---

# 🧠 Most Important Observation

We CANNOT actually store:

```text id="sinf12"
infinite numbers
```

❌ impossible

---

# 🔥 Key Insight

Suppose we already popped:

```text id="sinf13"
1 2 3
```

Then next untouched smallest naturally becomes:

```text id="sinf14"
4
```

We can simply track this using:

```cpp id="sinf15"
current
```

---

# 🧠 Final Idea

We need TWO things:

---

# 1️⃣ Pointer for untouched numbers

```cpp id="sinf16"
int current;
```

Tracks:

```text id="sinf17"
next untouched natural number
```

Example:

```text id="sinf18"
current = 4
```

means:

```text id="sinf19"
4,5,6... are naturally available
```

---

# 2️⃣ Min Heap for added-back numbers

```cpp id="sinf20"
priority_queue<
    int,
    vector<int>,
    greater<int>
> pq;
```

Stores:

```text id="sinf21"
numbers added back
```

---

# 🧠 Why Min Heap?

Because we always need:

```text id="sinf22"
smallest added-back number quickly
```

Min heap keeps:

```text id="sinf23"
smallest element at top
```

---

# ❗ One More Problem — Duplicates

Suppose:

```cpp id="sinf24"
addBack(1);
addBack(1);
```

Heap becomes:

```text id="sinf25"
1,1
```

❌ wrong

---

# 🔥 So we ALSO need

```cpp id="sinf26"
unordered_set<int> s;
```

to track:

```text id="sinf27"
which numbers already exist in heap
```

---

# 🧠 Final Data Structures

```cpp id="sinf28"
int current;

priority_queue<
    int,
    vector<int>,
    greater<int>
> pq;

unordered_set<int> s;
```

---

# 🎯 Constructor

Initially smallest available number is:

```text id="sinf29"
1
```

So:

```cpp id="sinf30"
current = 1;
```

---

# 🔥 popSmallest()

MOST IMPORTANT FUNCTION

---

# 🧩 Case 1 — Heap NOT Empty

Example:

```text id="sinf31"
heap = [1]
current = 4
```

Even though natural sequence starts at 4:

```text id="sinf32"
1 is smaller
```

So added-back numbers get priority.

---

# ✅ Steps

```cpp id="sinf33"
int x = pq.top();
pq.pop();

s.erase(x);

return x;
```

---

# 🧩 Case 2 — Heap Empty

Then smallest naturally is:

```text id="sinf34"
current
```

---

# ✅ Return and move forward

```cpp id="sinf35"
return current++;
```

---

# 🔥 addBack(num)

We should add back ONLY IF:

```text id="sinf36"
num was removed earlier
```

---

# 🧠 How to know this?

If:

```cpp id="sinf37"
num < current
```

then number was already popped before.

---

# ❗ Also avoid duplicates

```cpp id="sinf38"
s.find(num) == s.end()
```

---

# ✅ Then insert

```cpp id="sinf39"
pq.push(num);
s.insert(num);
```

---

# 💻 Full Code

```cpp id="sinf40"
class SmallestInfiniteSet {
public:

    int current;

    priority_queue<
        int,
        vector<int>,
        greater<int>
    > pq;

    unordered_set<int> s;

    SmallestInfiniteSet() {
        current = 1;
    }

    int popSmallest() {

        // added-back numbers available
        if (!pq.empty()) {

            int x = pq.top();
            pq.pop();

            s.erase(x);

            return x;
        }

        // natural infinite sequence
        return current++;
    }

    void addBack(int num) {

        // only if removed earlier
        // and not already present
        if (num < current &&
            s.find(num) == s.end()) {

            pq.push(num);
            s.insert(num);
        }
    }
};
```

---

# 🔍 Step-by-Step Dry Run

---

# 🧩 Initial State

```text id="sinf41"
current = 1
heap = empty
```

---

# 🧩 popSmallest()

Heap empty.

Return:

```text id="sinf42"
1
```

Now:

```text id="sinf43"
current = 2
```

---

# 🧩 popSmallest()

Return:

```text id="sinf44"
2
```

Now:

```text id="sinf45"
current = 3
```

---

# 🧩 addBack(1)

Since:

```text id="sinf46"
1 < current
```

Insert into heap.

Heap:

```text id="sinf47"
[1]
```

---

# 🧩 popSmallest()

Heap NOT empty.

Return:

```text id="sinf48"
1
```

Heap becomes empty again.

---

# 🧠 Mental Model (REVISION KEY)

> “Store only exceptions, not infinite numbers”

---

# 🔥 MOST IMPORTANT INSIGHT

We NEVER store:

```text id="sinf49"
1,2,3,4,5... infinitely
```

Instead:

* `current` handles untouched sequence
* heap handles added-back exceptions

---

# ⚡ Why Min Heap?

Because we always need:

```text id="sinf50"
smallest added-back number first
```

---

# ⚡ Why Set?

To prevent:

```text id="sinf51"
duplicate insertions
```

---

# ⏱ Complexity

---

## popSmallest()

Heap operations:

```text id="sinf52"
O(log n)
```

---

## addBack()

Heap + set:

```text id="sinf53"
O(log n)
```

---

# ⚡ Space Complexity

Stores only:

* added-back numbers
* set entries

NOT infinite numbers.

---

# ❗ Common Mistakes

* ❌ Actually trying to store infinite numbers
* ❌ Forgetting duplicate prevention
* ❌ Accessing heap top when empty
* ❌ Forgetting added-back numbers get priority

---

# 🧠 Patterns Learned

* Heap + Set combination
* Data structure simulation
* Lazy infinite sequence handling

---

# 🏷 Tags

* Heap
* Priority Queue
* Hash Set
* Design Problem

---

# 🔥 Recognition Pattern

Use:

* heap → when smallest/largest repeatedly needed
* set → duplicate prevention
* pointer → lazy infinite simulation
