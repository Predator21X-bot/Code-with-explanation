# 📌 Day 6: Filter Elements from Array

🔗 Platform: LeetCode
🔗 Problem: Filter Elements from Array

---

## 🧩 Problem Summary

Given:

* an array `arr`
* a function `fn`

Return a new array containing only elements where:

```text
fn(arr[i], i) === true
```

---

## 🧠 Intuition

We need to:

* iterate through array
* check condition using `fn`
* include only elements that satisfy condition

👉 This is called **filtering**

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

3. Apply condition:

   ```javascript
   if (fn(arr[i], i)) {
       result.push(arr[i]);
   }
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
var filter = function (arr, fn) {
    let result = [];
    for (let i = 0; i < arr.length; i++) {
        if (fn(arr[i], i)) {
            result.push(arr[i]);
        }
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

* `.length` is a property, not a function
* Filter only keeps elements where condition is true
* Functions returning boolean are commonly used as filters
* Similar structure to map, but conditional

---

## 🧱 Pattern Used

* Array traversal
* Conditional filtering
* Higher-order function
