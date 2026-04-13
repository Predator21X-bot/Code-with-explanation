# 🧩 DataFrame Size

## 🔗 Problem
LeetCode: DataFrame Size

---

## 🧠 Intuition

- A Pandas DataFrame has:
  - Rows
  - Columns
- We need to return both as a list → `[rows, columns]`

---

## ⚙️ Approach

1. Use `.shape` attribute on DataFrame
   - Returns a tuple → `(rows, columns)`
2. Convert tuple to list using `list()`
3. Return the result

---

## ✅ Code

```python
import pandas as pd

def getDataframeSize(players: pd.DataFrame):
    return list(players.shape)
