# 📌 Day 7: Array Reduce Transformation

🔗 Platform: LeetCode
🔗 Problem: Array Reduce Transformation

---

## 🧩 Problem Summary

Given:

* array `nums`
* function `fn`
* initial value `init`

Return a single value by applying:

```text
acc = fn(acc, current)
```

for all elements.

---

## 🧠 Intuition

We:

* start with `init`
* combine it with each element
* keep updating result

👉 Final result is **one value**

---

## ⚙️ Approach

1. Initialize accumulator:

   ```javascript
   let acc = init;
   ```

2. Loop through array:

   ```javascript
   for (let i = 0; i < nums.length; i++)
   ```

3. Update accumulator:

   ```javascript
   acc = fn(acc, nums[i]);
   ```

4. Return result

---

## 💻 Code

```javascript
/**
 * @param {number[]} nums
 * @param {Function} fn
 * @param {number} init
 * @return {number}
 */
var reduce = function(nums, fn, init) {
    let acc = init;
    for (let i = 0; i < nums.length; i++) {
        acc = fn(acc, nums[i]);
    }
    return acc;
};
```

---

## ⏱ Complexity

* Time Complexity: **O(n)**
* Space Complexity: **O(1)**

---

## 📌 Key Learnings

* Reduce returns a **single value**
* Always start with `init`
* Accumulator keeps updating
* Works for sum, product, max, etc.

---

## 🧱 Pattern Used

* Accumulation
* Iterative combination
* Functional programming
