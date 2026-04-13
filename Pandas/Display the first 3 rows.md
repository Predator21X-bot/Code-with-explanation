# 🧩 Select First Rows

## 🔗 Problem
LeetCode: Select First Rows

---

## 🧠 Intuition

- Often we need to quickly preview the dataset
- Pandas provides a built-in function to get top rows
- We only need the **first N rows**

---

## ⚙️ Approach

1. Use `.head()` method on DataFrame
2. Pass the number of rows required → `.head(n)`
3. Return the result

---

## ✅ Code

```python
import pandas as pd

def selectFirstRows(employees: pd.DataFrame):
    return employees.head(3)
