# 📌 Day 4: Counter II

🔗 Platform: LeetCode
🔗 Problem: Counter II

---

## 🧩 Problem Summary

Given an integer `init`, return an object with three methods:

* `increment()` → increases value by 1 and returns it
* `decrement()` → decreases value by 1 and returns it
* `reset()` → resets value to `init` and returns it

---

## 🧠 Intuition

We need:

* A variable that **persists across function calls**
* Multiple methods that **share and modify the same variable**

👉 This is achieved using a **closure**

---

## 🔥 Closure in this Problem

```javascript
let count = init;
```

👉 This variable:

* lives in outer function
* is shared by all methods
* keeps updating across calls

---

## ⚙️ Approach

1. Store initial value:

   ```javascript
   let count = init;
   ```

2. Return an object with methods:

   * `increment()` → `return ++count`
   * `decrement()` → `return --count`
   * `reset()`:

     * set `count = init`
     * return `count`

---

## 💻 Code

```javascript
/**
 * @param {integer} init
 * @return { increment: Function, decrement: Function, reset: Function }
 */
var createCounter = function(init) {
    let count = init;
    return {
        increment: function() {
            return ++count;
        },
        reset: function() {
            count = init;
            return count;
        },
        decrement: function() {
            return --count;
        }
    }
};

/**
 * const counter = createCounter(5)
 * counter.increment(); // 6
 * counter.reset(); // 5
 * counter.decrement(); // 4
 */
```

---

## ⏱ Complexity

* Time Complexity: **O(1)** per operation
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Closures allow multiple functions to share **same state**
* Avoid passing variables again as parameters (breaks closure)
* Object methods can work together on shared data
* Pre-increment (`++x`) updates before returning

---

## 🧱 Pattern Used

* Closure with shared state
* Object with multiple methods
* Stateful design
