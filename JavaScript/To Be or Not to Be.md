# 📌 Day 3: To Be Or Not To Be

🔗 Platform: LeetCode
🔗 Problem: To Be Or Not To Be

---

## 🧩 Problem Summary

Implement a function `expect(val)` that returns an object with two methods:

* `toBe(val2)`
* `notToBe(val2)`

### Behavior:

* `toBe(val2)`

  * returns `true` if `val === val2`
  * otherwise throws `"Not Equal"`

* `notToBe(val2)`

  * returns `true` if `val !== val2`
  * otherwise throws `"Equal"`

---

## 🧠 Intuition

We need:

* A function that returns an **object**
* That object contains **methods**
* Each method performs a **comparison**
* If condition fails → **throw error**

---

## 🔥 Key Concept 1: Objects with Methods

JavaScript objects can store functions:

```javascript
const obj = {
    greet: function() {
        return "Hello";
    }
}
```

👉 These functions are called **methods**

---

## 🔥 Key Concept 2: Error Handling

To throw errors in JavaScript:

```javascript
throw new Error("message");
```

👉 This:

* stops execution
* signals something went wrong

---

## ⚙️ Approach

1. Create function `expect(val)`
2. Return an object with two methods:

   * `toBe(val2)`
   * `notToBe(val2)`
3. In `toBe`:

   * check `val === val2`
   * else throw `"Not Equal"`
4. In `notToBe`:

   * check `val !== val2`
   * else throw `"Equal"`

---

## 💻 Code

```javascript
/**
 * @param {string} val
 * @return {Object}
 */
var expect = function (val) {
    return {
        toBe: function (val2) {
            if (val === val2) {
                return true;
            } else {
                throw new Error("Not Equal");
            }
        },
        notToBe: function (val2) {
            if (val !== val2) {
                return true;
            } else {
                throw new Error("Equal");
            }
        }
    }
};

/**
 * expect(5).toBe(5); // true
 * expect(5).notToBe(5); // throws "Equal"
 */
```

---

## ⏱ Complexity

* Time Complexity: **O(1)**
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Objects can hold **methods (functions inside objects)**
* Use `===` for strict equality
* Use `!==` for strict inequality
* Throw errors using:

  ```javascript
  throw new Error("message");
  ```

---

## 🧱 Pattern Used

* Object with methods
* Conditional logic
* Error handling
