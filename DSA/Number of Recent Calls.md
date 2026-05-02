# 🧩 Number of Recent Calls

### 🔗 Problem Link

https://leetcode.com/problems/number-of-recent-calls/

---

# 🧠 Problem Understanding

You are given a class `RecentCounter` with function:

```cpp
int ping(int t);
```

👉 Each call represents a request at time `t`

---

## 🎯 Goal

Return number of requests in range:

```text
[t - 3000, t]
```

👉 Inclusive

---

## 🔍 Example

```text
ping(1)     → [1] → answer = 1
ping(100)   → [1,100] → answer = 2
ping(3001)  → [1,100,3001] → answer = 3
ping(3002)  → [100,3001,3002] → answer = 3
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Key observation

👉 Only recent calls matter:

```text
timestamps >= t - 3000
```

---

## 🧩 Step 2: What about old calls?

```text
timestamps < t - 3000 → useless → remove
```

---

# ⚡ Approach: Queue (Sliding Window)

---

## 🧠 Idea

* Store timestamps in queue
* Add new timestamp
* Remove outdated timestamps
* Return queue size

---

## 💻 Code

```cpp
class RecentCounter {
public:
    queue<int> q;

    RecentCounter() {}

    int ping(int t) {
        // add current timestamp
        q.push(t);

        // remove outdated timestamps
        while (!q.empty() && q.front() < t - 3000) {
            q.pop();
        }

        // return number of valid calls
        return q.size();
    }
};
```

---

# ⏱ Complexity

* Time: **O(1) amortized per call**
* Space: **O(n)**

---

# 🔥 Mental Model (REVISION KEY)

> “Queue always stores only valid recent timestamps”

---

# ⚡ Sliding Window Interpretation

👉 This is a **moving window problem**

```text
window = [t - 3000, t]
```

👉 Queue stores elements inside window

---

# 🧠 Why Queue?

| Operation    | Needed                |
| ------------ | --------------------- |
| push back    | add new timestamp     |
| pop front    | remove old timestamps |
| front access | check oldest          |

---

# ❗ Common Mistakes

* ❌ Using `push_front()`
* ❌ Using `pop_back()`
* ❌ Removing only once (use while loop)
* ❌ Not checking queue front
* ❌ Forgetting return value

---

# 🧠 Patterns Learned

* Queue
* Sliding window
* Time-based filtering

---

# 🏷 Tags

* Queue
* Sliding Window
* Design

---

# 🔥 Recognition Pattern

Use this when:

* “Recent / last X seconds”
* Time-based filtering
* Continuously incoming data
