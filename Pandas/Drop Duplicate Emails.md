# 🧩 Drop Duplicate Emails

## 🔗 Problem
LeetCode: Drop Duplicate Emails

---

## 🧠 Intuition
- Remove duplicate rows based on a specific column (`email`)
- Keep only the first occurrence

---

## ⚙️ Approach
- Use: `df.drop_duplicates()`
- Specify column using `subset="email"`
- Return the result (do NOT modify in-place)

---

## ✅ Code

```python
import pandas as pd

def dropDuplicateEmails(customers: pd.DataFrame) -> pd.DataFrame:
    return customers.drop_duplicates(subset="email")
