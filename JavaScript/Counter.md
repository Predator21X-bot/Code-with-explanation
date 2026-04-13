# 📌 Day 2: Counter

🔗 Platform: LeetCode
🔗 Problem: Counter

---

## 🧩 Problem Summary

Given an integer `n`, return a function `counter` such that:

* The first call returns `n`
* Each subsequent call returns the next integer (`n + 1`, `n + 2`, ...)

---

## 🧠 Intuition

We need a way to:

* store a variable (`n`)
* update it across multiple function calls

👉 This is achieved using a **closure**

---

## 🔥 What is a Closure? (Important)

👉 Closure =
**A function that remembers variables from its outer scope**

Even after the outer function has finished execution, the inner function can still access those variables.

---

### ❌ Not a Closure

```javascript
function outer() {
    return function() {
        return 5;
    }
}
```

👉 Inner function does NOT use outer variables → **not a closure**

---

### ✅ Closure

```javascript
function outer() {
    let x = 10;
    return function() {
        return x;
    }
}
```

👉 Inner function uses `x` → **closure**

---

### 💡 In this problem

```javascript
var createCounter = function(n) {
    return function() {
        return n++;
    };
};
```

👉 Inner function remembers `n` → closure in action

---

## ⚙️ Approach

1. Take `n` as input in outer function
2. Return an inner function
3. Inside inner function:

   * return current `n`
   * increment `n` using `n++`

---

## 💻 Code

```javascript
/**
 * @param {number} n
 * @return {Function} counter
 */
var createCounter = function(n) {
    
    return function() {
        return n++;
    };
};

/** 
 * const counter = createCounter(10)
 * counter() // 10
 * counter() // 11
 * counter() // 12
 */
```

---

## ⏱ Complexity

* Time Complexity: **O(1)** per call
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Closures allow functions to **retain state**
* Variables in outer scope persist across function calls
* `n++` (post-increment):

  * returns current value
  * then increments

---

## 🧱 Pattern Used

* Closure
* Stateful function
* Function returning function
