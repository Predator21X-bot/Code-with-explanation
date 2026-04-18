# 🧩 Concatenate Tables

## 🔗 Problem
LeetCode: Concatenate Tables

---

## 🧠 Intuition
- We need to combine two DataFrames
- Both have same columns
- Stack rows vertically

---

## ⚙️ Approach
- Use: `pd.concat()`
- Pass DataFrames as list → `[df1, df2]`
- Use `ignore_index=True` to reset index
- Return combined DataFrame

---

## ✅ Code

```python
import pandas as pd

def concatenateTables(df1: pd.DataFrame, df2: pd.DataFrame) -> pd.DataFrame:
    return pd.concat([df1, df2], ignore_index=True)
