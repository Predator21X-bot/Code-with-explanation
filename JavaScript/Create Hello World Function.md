# 📌 Day 1: Create Hello World Function

🔗 Platform: LeetCode
🔗 Problem: Create Hello World Function

---

## 🧩 Problem Summary

We need to create a function `createHelloWorld` that returns another function.
When the returned function is called, it should return:

```text
"Hello World"
```

---

## 🧠 Intuition

This problem introduces a key JavaScript concept:

👉 **Functions can return other functions**

So instead of returning a value directly, we:

1. Create an outer function
2. Return an inner function
3. That inner function returns the final result

---

## ⚙️ Approach

1. Define a function using function expression:

   ```javascript
   var createHelloWorld = function() {}
   ```

2. Inside it, return another function:

   ```javascript
   return function() {}
   ```

3. Inside the inner function, return `"Hello World"`

---

## 💻 Code

```javascript
/**
 * @return {Function}
 */
var createHelloWorld = function() {
    
    return function(...args) {
        return "Hello World";
    }
};

/**
 * const f = createHelloWorld();
 * f(); // "Hello World"
 */
```

---

## ⏱ Complexity

* Time Complexity: **O(1)**
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Functions in JavaScript are **first-class citizens**
* A function can:

  * be stored in variables
  * be passed as arguments
  * be returned from another function
* Introduction to **closures (basic idea)**

---

## 🧱 Pattern Used

* Function returning function
* Foundation for:

  * Closures
  * Callbacks
  * Functional programming
