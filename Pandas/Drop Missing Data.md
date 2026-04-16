# 🧩 Drop Missing Data

## 🔗 Problem
LeetCode: Drop Missing Data

---

## 🧠 Intuition
- Some rows contain missing values (`None` / `NaN`)
- We need to remove those rows
- Only consider missing values in a specific column (`name`)

---

## ⚙️ Approach
- Use: `df.dropna()`
- Specify column using `subset=["name"]`
- Return the filtered DataFrame

---

## ✅ Code

```python
import pandas as pd

def dropMissingData(students: pd.DataFrame) -> pd.DataFrame:
    return students.dropna(subset=["name"])
