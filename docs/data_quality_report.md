# Data Quality Report – Telco Customer Churn

**Date:** 06 Oct 2025  
**Analyst:** Sahana Mathad  
**Tool Used:** Power BI (Power Query Editor)

---

## 📊 1. Dataset Overview
| Metric | Value |
|---------|-------|
| Total Rows | 7,043 |
| Total Columns | 23 |

---

## 🧩 2. Missing or Null Values
| Check | Result |
|--------|---------|
| Columns with Missing/Null Values | 0 |
| Columns with Errors | 0 |
| Comment | All columns have 100% valid data based on full dataset profiling. |

---

## 🔁 3. Duplicate Rows
| Check | Result |
|--------|---------|
| Before Duplicate Removal | 7,043 rows |
| After Duplicate Removal | 7,043 rows |
| **Duplicates Found** | **0** |
| Comment | Dataset is free from duplicate records. |

---

## ⚠️ 4. Suspicious / Outlier Values
| Column | Rule Checked | Result | Comment |
|---------|----------------|---------|----------|
| Tenure | `= 0` | 11 records | Possible new customers or missing tenure info |
| MonthlyCharges | `> 150` | 0 | All values within expected range |
| Gender | Blank | 0 | No missing gender values |

---

## ✅ 5. Data Quality Summary
- Dataset is **clean and consistent**.  
- No duplicates or missing fields detected.  
- Minor check needed on 11 customers with `Tenure = 0` (could be new customers).  
- Data is ready for **further transformation and modeling** in Power BI.

---

**Next Step:** Proceed to KPI creation and churn visualization.
