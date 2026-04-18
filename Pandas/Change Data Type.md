# 🧩 Change Data Type

## 🔗 Problem
LeetCode: Change Data Type

---

## 🧠 Intuition
- Column `grade` is stored as float
- We need to convert it to integer
- Pandas provides a method for type conversion

---

## ⚙️ Approach
- Use: `.astype(type)`
- Apply on the `grade` column
- Assign back to the same column
- Return updated DataFrame

---

## ✅ Code

```python
import pandas as pd

def changeDatatype(students: pd.DataFrame) -> pd.DataFrame:
    students["grade"] = students["grade"].astype(int)
    return students
