# 🧩 Fill Missing Data

## 🔗 Problem
LeetCode: Fill Missing Data

---

## 🧠 Intuition
- Some values in `quantity` column are missing (`None` / `NaN`)
- Instead of removing rows, we replace missing values
- Fill missing values with `0`

---

## ⚙️ Approach
- Use: `.fillna(value)`
- Apply on specific column (`quantity`)
- Assign back to the column
- Return updated DataFrame

---

## ✅ Code

```python
import pandas as pd

def fillMissingValues(products: pd.DataFrame) -> pd.DataFrame:
    products["quantity"] = products["quantity"].fillna(0)
    return products
