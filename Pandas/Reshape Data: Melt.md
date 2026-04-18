# 🧩 Reshape Data: Melt

## 🔗 Problem
LeetCode: Reshape Data - Melt

---

## 🧠 Intuition
- Data is in wide format (multiple columns for similar data)
- We need to convert it into long format
- Each row should represent:
  - a product
  - a specific quarter
  - its sales

---

## 🔄 Concept

### Wide Format
| product | quarter_1 | quarter_2 | quarter_3 | quarter_4 |
|---------|----------|----------|----------|----------|

### Long Format
| product | quarter   | sales |
|---------|----------|-------|

---

## ⚙️ Approach
- Use: `df.melt()`
- `id_vars` → columns to keep fixed (`product`)
- Remaining columns → converted into rows
- Rename:
  - column names → `quarter`
  - values → `sales`

---

## ✅ Code

```python
import pandas as pd

def meltTable(report: pd.DataFrame) -> pd.DataFrame:
    return report.melt(
        id_vars="product",
        var_name="quarter",
        value_name="sales"
    )
