# 📌 Day 5: Apply Transform Over Each Element in Array

🔗 Platform: LeetCode
🔗 Problem: Apply Transform Over Each Element in Array

---

## 🧩 Problem Summary

Given:

* an array `arr`
* a function `fn`

Return a new array where:

```text
result[i] = fn(arr[i], i)
```

👉 You must NOT use built-in `map`

---

## 🧠 Intuition

We need to:

* iterate through array
* apply function on each element
* store results in new array

---

## ⚙️ Approach

1. Create empty array:

   ```javascript
   let result = [];
   ```

2. Loop through array:

   ```javascript
   for (let i = 0; i < arr.length; i++)
   ```

3. Apply function with **value + index**:

   ```javascript
   result.push(fn(arr[i], i));
   ```

4. Return result

---

## 💻 Code

```javascript
/**
 * @param {number[]} arr
 * @param {Function} fn
 * @return {number[]}
 */
var map = function(arr, fn) {
    let result = [];
    for (let i = 0; i < arr.length; i++) {
        result.push(fn(arr[i], i));
    }
    return result;
};
```

---

## ⏱ Complexity

* Time Complexity: **O(n)**
* Space Complexity: **O(n)**

---

## 📌 Key Learnings

* Arrays use `.length`, not `.size()`
* `push()` adds elements to array
* Functions can take multiple arguments
* Always read problem carefully (hidden detail: index)

---

## 🧱 Pattern Used

* Array traversal
* Transformation
* Higher-order function usage
