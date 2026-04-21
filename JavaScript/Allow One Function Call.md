# 📌 Day 10: Allow One Function Call

🔗 Platform: LeetCode
🔗 Problem: Allow One Function Call

---

## 🧩 Problem Summary

Given a function `fn`, return a new function that:

* can be called **only once**
* first call → returns result of `fn`
* subsequent calls → return `undefined`

---

## 🧠 Intuition

We need to:

* remember if function was already called
* block future calls

👉 This requires **persistent state across calls**

---

## 🔥 Key Concept: Closure

```javascript
let called = false;
```

👉 This variable:

* lives in outer function
* is remembered by inner function
* persists across calls

---

## ⚙️ Approach

1. Create a boolean flag:

   ```javascript
   let called = false;
   ```

2. Return a function:

   ```javascript
   return function(...args)
   ```

3. Inside:

   * If already called → return `undefined`
   * Else:

     * set `called = true`
     * call `fn(...args)` and return result

---

## 💻 Code

```javascript
/**
 * @param {Function} fn
 * @return {Function}
 */
var once = function(fn) {
    let called = false;
    return function(...args){
        if (called) {
            return undefined;
        }
        called = true;
        return fn(...args);
    }
};
```

---

## ⏱ Complexity

* Time Complexity: **O(1)** per call
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Closures allow variables to **persist across calls**
* Order matters: update state **before returning**
* `...args` allows flexible arguments
* Early return prevents further execution

---

## 🧱 Pattern Used

* Closure with state
* Conditional execution
* Function wrapping
