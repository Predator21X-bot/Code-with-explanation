# 📌 Day 9: Return Length of Arguments Passed

🔗 Platform: LeetCode
🔗 Problem: Return Length of Arguments Passed

---

## 🧩 Problem Summary

Write a function that returns the number of arguments passed to it.

---

## 🧠 Intuition

We need:

* a way to capture all arguments
* count how many are passed

👉 JavaScript provides **rest parameters** for this

---

## 🔥 Key Concept: Rest Parameters

```javascript
function fn(...args) {
    // args is an array
}
```

👉 `...args` collects all arguments into an array

---

## ⚙️ Approach

1. Use rest parameter:

   ```javascript
   function(...args)
   ```

2. Return length:

   ```javascript
   return args.length;
   ```

---

## 💻 Code

```javascript
/**
 * @param {...(null|boolean|number|string|Array|Object)} args
 * @return {number}
 */
var argumentsLength = function(...args) {
    return args.length;
};
```

---

## ⏱ Complexity

* Time Complexity: **O(1)**
* Space Complexity: **O(n)** (args array)

---

## 📌 Key Learnings

* `...args` collects arguments into array
* `.length` gives number of elements
* Functions can take variable number of arguments
