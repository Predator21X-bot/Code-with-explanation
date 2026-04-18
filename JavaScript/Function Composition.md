# 📌 Day 8: Function Composition

🔗 Platform: LeetCode
🔗 Problem: Function Composition

---

## 🧩 Problem Summary

Given:

* an array of functions `functions`

Return a function such that:

```text
fn(x) = functions[0](functions[1](...functions[n-1](x)))
```

👉 Functions are applied **right to left**

---

## 🧠 Intuition

We:

* receive an input `x`
* apply last function first
* keep updating `x`
* return final result

👉 This combines multiple functions into one

---

## ⚙️ Approach

1. Return a function that takes `x`
2. Loop from end to start:

   ```javascript
   for (let i = functions.length - 1; i >= 0; i--)
   ```
3. Update `x`:

   ```javascript
   x = functions[i](x);
   ```
4. Return final `x`

---

## 💻 Code

```javascript
/**
 * @param {Function[]} functions
 * @return {Function}
 */
var compose = function(functions) {
    return function(x) {
        for (let i = functions.length - 1; i >= 0; i--) {
            x = functions[i](x);
        }
        return x;
    }
};
```

---

## ⏱ Complexity

* Time Complexity: **O(n)**
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Functions can be passed and returned
* Execution order matters (right → left)
* Arrays are 0-indexed
* Avoid early return inside loops

---

## 🧱 Pattern Used

* Function composition
* Loop traversal (reverse)
* Functional programming
