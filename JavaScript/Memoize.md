# 📌 Day 11: Memoize

🔗 Platform: LeetCode
🔗 Problem: Memoize

---

## 🧩 Problem Summary

Given a function `fn`, return a memoized version of it.

👉 If called again with same arguments:

* return cached result
* do NOT recompute

---

## 🧠 Intuition

We need:

* a way to store previous results
* reuse them for same inputs

👉 This avoids unnecessary computation

---

## 🔥 Key Concept: Memoization

👉 Memoization =
**Store results of function calls and reuse them**

---

## ⚙️ Approach

1. Create a cache object:

   ```javascript
   let cache = {};
   ```

2. Return a function:

   ```javascript
   return function(...args)
   ```

3. Convert arguments into key:

   ```javascript
   let key = args.join(",");
   ```

4. Check cache:

   ```javascript
   if (key in cache)
   ```

5. If not present:

   * compute result
   * store in cache

---

## 💻 Code

```javascript
/**
 * @param {Function} fn
 * @return {Function}
 */
function memoize(fn) {
    let cache = {};
    return function (...args) {
        let key = args.join(",");
        if (key in cache) {
            return cache[key];
        }

        let result = fn(...args);
        cache[key] = result;

        return result;
    }
}
```

---

## ⏱ Complexity

* Time Complexity:

  * First call → depends on `fn`
  * Repeated calls → **O(1)**

* Space Complexity: **O(n)** (cache storage)

---

## 📌 Key Learnings

* Closures allow persistent storage (`cache`)
* Objects can be used as key-value storage
* Avoid recomputation using caching
* Argument serialization (`join`) helps create keys
