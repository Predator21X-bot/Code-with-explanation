# 🧩 Filter, Sort, and Select

## 🔗 Problem
LeetCode: Find Heavy Animals

---

## 🧠 Intuition
- Filter rows where `weight > 100`
- Sort filtered rows by `weight` in descending order
- Return only the `name` column

---

## ⚙️ Approach

1. Filter rows:
```python
df[df["weight"] > 100]
````

2. Sort rows:

```python
.sort_values(by="weight", ascending=False)
```

3. Select column:

```python
[["name"]]
```

4. Chain all steps together

---

## ✅ Code

```python
import pandas as pd

def findHeavyAnimals(animals: pd.DataFrame) -> pd.DataFrame:
    return (
        animals[animals["weight"] > 100]
        .sort_values(by="weight", ascending=False)[["name"]]
    )
```

---

## 🧪 Example

**Input**

| name     | weight |
| -------- | ------ |
| Elephant | 1200   |
| Dog      | 30     |
| Horse    | 380    |
| Cat      | 10     |

**Output**

| name     |
| -------- |
| Elephant |
| Horse    |

---

## 🚨 Common Mistakes

* ❌ Using `desc` instead of `ascending=False`
* ❌ Selecting from original DataFrame (`animals["name"]`)
* ❌ Using `["name"]` instead of `[["name"]]`
* ❌ Forgetting chaining

---

## 🧠 Key Points

* Filtering → `df[condition]`
* Sorting → `.sort_values()`
* Descending → `ascending=False`
* `[["col"]]` returns DataFrame
* Method chaining improves readability

---

## 🔥 Pattern

```python
df[df["col"] > value] \
    .sort_values(by="col", ascending=False)[["target_col"]]
```

---

## 📌 Variations

### Multiple conditions

```python
df[(df["weight"] > 100) & (df["age"] > 5)]
```

### Ascending sort

```python
df.sort_values(by="weight", ascending=True)
```

---

## 🧠 Clean Style (Recommended)

```python
(
    df[df["col"] > value]
    .sort_values(by="col", ascending=False)[["target_col"]]
)
```
