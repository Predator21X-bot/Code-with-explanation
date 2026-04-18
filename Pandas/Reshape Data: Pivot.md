# 🧩 Reshape Data: Pivot

## 🔗 Problem
LeetCode: Reshape Data - Pivot

---

## 🧠 Intuition
- Data is in long format (multiple rows per entity)
- We need to convert it into wide format
- Each row → represents a month
- Each column → represents a city

---

## ⚙️ Approach
- Use: `df.pivot()`
- Set:
  - `index` → rows (`month`)
  - `columns` → new columns (`city`)
  - `values` → cell values (`temperature`)
- Return reshaped DataFrame

---

## ✅ Code

```python
import pandas as pd

def pivotTable(weather: pd.DataFrame) -> pd.DataFrame:
    return weather.pivot(
        index="month",
        columns="city",
        values="temperature"
    )
