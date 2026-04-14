# 🧩 Select Data

## 🔗 Problem
LeetCode: Select Data

---

## 🧠 Intuition

- We are not just selecting columns
- We also need to **filter rows based on a condition**
- Then extract specific columns from the filtered result

---

## ⚙️ Approach

1. Filter rows using a condition:
   - `students["student_id"] == 101`
2. Use this condition inside DataFrame indexing
3. Select required columns using `[["name", "age"]]`
4. Return the result

---

## ✅ Code

```python
import pandas as pd

def selectData(students: pd.DataFrame):
    return students[students["student_id"] == 101][["name", "age"]]
