# 🧩 Create DataFrame from List

## 🔗 Problem
LeetCode: Create a DataFrame from List
Link : https://leetcode.com/problems/create-a-dataframe-from-list/description/?envType=study-plan-v2&envId=introduction-to-pandas&lang=pythondata

---

## 🧠 Intuition

- Pandas DataFrame is like a table (rows + columns)
- A 2D list naturally maps to rows
- Each inner list = one row
- We need to assign column names explicitly

---

## ⚙️ Approach

1. Use `pd.DataFrame()`
2. Pass the given 2D list directly
3. Provide column names using `columns=[...]`

---

## ✅ Code

```python
import pandas as pd

def createDataframe(student_data):
    return pd.DataFrame(student_data, columns=["student_id", "age"])
