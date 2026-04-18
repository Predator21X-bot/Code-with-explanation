# 🧩 Rename Columns

## 🔗 Problem
LeetCode: Rename Columns

---

## 🧠 Intuition
- We need to change column names
- Pandas provides a direct method for renaming
- Multiple columns can be renamed at once using a mapping

---

## ⚙️ Approach
- Use: `df.rename(columns={...})`
- Provide mapping → `{old_name: new_name}`
- Return the updated DataFrame

---

## ✅ Code

```python
import pandas as pd

def renameColumns(students: pd.DataFrame) -> pd.DataFrame:
    return students.rename(columns={
        "id": "student_id",
        "first": "first_name",
        "last": "last_name",
        "age": "age_in_years"
    })
