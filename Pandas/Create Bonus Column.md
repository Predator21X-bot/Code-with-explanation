# 🧩 Create Bonus Column

## 🔗 Problem
LeetCode: Create Bonus Column

---

## 🧠 Intuition
- Create a new column based on an existing column
- Apply operation on entire column (no loops needed)

---

## ⚙️ Approach
- Use: `df["new_column"] = expression`
- Access existing column using `df["column_name"]`
- Apply vectorized operation

---

## ✅ Code

```python
import pandas as pd

def createBonusColumn(employees: pd.DataFrame) -> pd.DataFrame:
    employees["bonus"] = 2 * employees["salary"]
    return employees
