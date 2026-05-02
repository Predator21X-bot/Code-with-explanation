# 🧩 Dota2 Senate

### 🔗 Problem Link

https://leetcode.com/problems/dota2-senate/

---

# 🧠 Problem Understanding

Given:

* String `senate`
* `'R'` → Radiant
* `'D'` → Dire

---

## 🎯 Rules

Each senator:

```text
can ban one opponent senator
```

👉 Banned senator:

```text
removed permanently ❌
```

👉 Winning senator:

```text
comes back in next round ✅
```

---

## 🎯 Goal

```text
Predict which party will win
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: Key observation

👉 Order matters:

```text
earlier index → acts first
```

---

## 🧩 Step 2: Cyclic behavior

👉 Game continues in rounds:

```text
winner goes back into queue
```

---

# ⚡ Approach: Two Queues + Simulation

---

## 🧠 Idea

* Store positions (indices), not characters
* Maintain two queues:

  * Radiant queue
  * Dire queue

---

## 🧩 Step 3: Initialize queues

```cpp
queue<int> queueR, queueD;

for (int i = 0; i < senate.size(); i++) {
    if (senate[i] == 'R') queueR.push(i);
    else queueD.push(i);
}
```

---

## 🧩 Step 4: Simulate

```cpp
while (!queueR.empty() && !queueD.empty()) {
    int r = queueR.front(); queueR.pop();
    int d = queueD.front(); queueD.pop();

    if (r < d) {
        queueR.push(r + n);
    } else {
        queueD.push(d + n);
    }
}
```

---

## 🧩 Step 5: Return winner

```cpp
return queueR.empty() ? "Dire" : "Radiant";
```

---

# 💻 Full Code

```cpp
class Solution {
public:
    string predictPartyVictory(string senate) {
        queue<int> queueR, queueD;
        int n = senate.size();

        for (int i = 0; i < n; i++) {
            if (senate[i] == 'R') queueR.push(i);
            else queueD.push(i);
        }

        while (!queueR.empty() && !queueD.empty()) {
            int r = queueR.front(); queueR.pop();
            int d = queueD.front(); queueD.pop();

            if (r < d) {
                queueR.push(r + n);
            } else {
                queueD.push(d + n);
            }
        }

        return queueR.empty() ? "Dire" : "Radiant";
    }
};
```

---

# ⏱ Complexity

* Time: **O(n)**
* Space: **O(n)**

---

# 🔥 Key Insight (VERY IMPORTANT)

```text
Store indices to maintain order
```

---

# 🧠 Why `+n`?

👉 Simulates next round:

```text
current round: [0,1,2,...,n-1]
next round:   [n,n+1,n+2,...]
```

---

# 🔍 Example

```text
senate = "RDD"
index  =  0 1 2
```

---

## Step-by-step

```text
R(0) vs D(1) → R wins → reinsert at 3
```

```text
queueR = [3]
queueD = [2]
```

```text
D(2) vs R(3) → D wins → reinsert at 5
```

```text
queueD = [5]
```

👉 Dire wins

---

# 🧠 Mental Model (REVISION KEY)

> “Earlier index acts first → survives → re-enters next round”

---

# ⚡ Pattern Breakdown

---

## 🔹 Store positions

👉 maintains order

---

## 🔹 Compare front elements

👉 decide who acts first

---

## 🔹 Push winner back

👉 simulate next round

---

# ❗ Common Mistakes

* ❌ Using `queue<char>` instead of `queue<int>`
* ❌ Not storing indices
* ❌ Using `+1` instead of `+n`
* ❌ Not understanding cyclic rounds
* ❌ Incorrect return condition

---

# 🧠 Patterns Learned

* Queue
* Simulation
* Greedy (earlier wins)
