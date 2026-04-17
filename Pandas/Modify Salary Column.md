# 🧩 Modify Salary Column

## 🔗 Problem
LeetCode: Modify Salary Column

---

## 🧠 Intuition
- We need to update an existing column (`salary`)
- Apply an operation to all rows
- No need to create a new column

---

## ⚙️ Approach
- Access column using `df["salary"]`
- Modify it using assignment
- Apply vectorized operation (multiply by 2)
- Return updated DataFrame

---

## ✅ Code

```python
import pandas as pd

def modifySalaryColumn(employees: pd.DataFrame) -> pd.DataFrame:
    employees["salary"] = 2 * employees["salary"]
    return employees
