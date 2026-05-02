# 🧩 Asteroid Collision

### 🔗 Problem Link

https://leetcode.com/problems/asteroid-collision/

---

# 🧠 Problem Understanding

Given:

* Array `asteroids`

👉 Rules:

* Positive → moving right ➡️
* Negative → moving left ⬅️

👉 Collision happens when:

```text
right-moving asteroid meets left-moving asteroid
```

---

## 🔍 Example

```text
asteroids = [5, 10, -5]
Output = [5, 10]
```

---

# 🧠 Step-by-Step Thinking (IMPORTANT 🔥)

---

## 🧩 Step 1: When does collision happen?

```text
st.top() > 0 AND current < 0
```

👉 Means:

* stack asteroid → right
* current asteroid → left
  👉 moving toward each other

---

## ❌ No collision cases

```text
[-2, 3] → move away ❌  
[-2, -3] → same direction ❌  
[2, 3] → same direction ❌
```

---

## 🧩 Step 2: What happens during collision?

Compare sizes:

---

### Case 1: Stack smaller

```text
[3, -5]
→ 3 destroyed
→ -5 continues
```

---

### Case 2: Equal

```text
[5, -5]
→ both destroyed
```

---

### Case 3: Stack larger

```text
[10, -5]
→ -5 destroyed
```

---

# ⚡ Approach: Stack + Simulation

---

## 🧠 Idea

* Use stack to store alive asteroids
* Process each asteroid one by one
* Resolve collisions immediately

---

# 💻 Code

```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        stack<int> st;

        for (int a : asteroids) {
            bool destroyed = false;

            while (!st.empty() && st.top() > 0 && a < 0) {

                if (abs(st.top()) < abs(a)) {
                    st.pop(); // stack destroyed
                } 
                else if (abs(st.top()) == abs(a)) {
                    st.pop(); // both destroyed
                    destroyed = true;
                    break;
                } 
                else {
                    destroyed = true; // current destroyed
                    break;
                }
            }

            if (!destroyed) {
                st.push(a);
            }
        }

        vector<int> result;

        while (!st.empty()) {
            result.push_back(st.top());
            st.pop();
        }

        reverse(result.begin(), result.end());

        return result;
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
One asteroid can collide multiple times
```

---

## Example

```text
[10, 2, -5]

→ 2 destroyed  
→ then 10 vs -5
```

👉 That’s why we use `while`, not `if`

---

# 🧠 Mental Model (REVISION KEY)

> “Current asteroid fights stack until stable”

---

# ⚡ Simulation Flow

```text
for each asteroid:
    while collision possible:
        resolve collision
    if survives:
        push to stack
```

---

# 🧠 Patterns Learned

* Stack (LIFO)
* Simulation
* Repeated collision handling

---

# 🏷 Tags

* Stack
* Simulation
* Array

---

# ❗ Common Mistakes

* ❌ Using `if` instead of `while`
* ❌ Pushing everything first
* ❌ Not handling chain collisions
* ❌ Ignoring direction condition

---

# 🔥 Recognition Pattern

Use this approach when:

* Objects interact with previous ones
* Chain reactions possible
* Need to simulate step-by-step
