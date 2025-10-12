# 📘 Data Dictionary — Customer Churn Prediction & Retention Dashboard

### 📅 Last Updated: October 2025  
**Author:** Sahana M  
**Project Type:** Academic / Self-Learning Project  
**Dataset Source:** [Telco Customer Churn – Kaggle](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)  
**Tools Used:** Power BI | Power Query | DAX | Python | Excel  

---

## 1️⃣ Dataset Overview
- **Domain:** Telecommunications – Customer Analytics  
- **Total Records:** 7,043 customers  
- **Target Variable:** `Churn` (Yes / No)  
- **Granularity:** Each row represents a unique customer account with demographic, service, and billing information.  
- **Objective:** Predict customer churn, analyze key drivers, and identify high-value at-risk customers for retention campaigns.  
- **Data Period:** Historical snapshot (no explicit date column).  
- **File Used:** `WA_Fn-UseC_-Telco-Customer-Churn.csv`

---

## 2️⃣ Raw Table Schema

| Column Name | Data Type | Description | Example | Transformation / Notes |
|--------------|------------|--------------|----------|-------------------------|
| `customerID` | String | Unique identifier for each customer | 7590-VHVEG | Used as primary key |
| `gender` | Categorical | Gender of the customer | Female | Encoded as binary (0/1) for modeling |
| `SeniorCitizen` | Integer (0/1) | Indicates if customer is a senior citizen | 0 | Retained as numeric flag |
| `Partner` | Categorical (Yes/No) | Whether customer has a partner | Yes | Converted to binary flag |
| `Dependents` | Categorical (Yes/No) | Whether customer has dependents | No | Converted to binary flag |
| `tenure` | Integer | Number of months the customer has stayed | 34 | Used for cohort and loyalty analysis |
| `PhoneService` | Categorical (Yes/No) | Whether customer has phone service | Yes | Binary conversion |
| `MultipleLines` | Categorical | Multiple phone lines | No phone service | Encoded into grouped Boolean |
| `InternetService` | Categorical | Internet connection type | Fiber optic | One-hot encoded for model |
| `OnlineSecurity` | Categorical (Yes/No) | Whether customer has online security | No | Converted to binary flag |
| `OnlineBackup` | Categorical (Yes/No) | Whether customer has online backup | Yes | Binary flag |
| `DeviceProtection` | Categorical (Yes/No) | Whether customer has device protection | Yes | Binary flag |
| `TechSupport` | Categorical (Yes/No) | Whether customer has tech support | No | Binary flag |
| `StreamingTV` | Categorical (Yes/No) | Whether customer has streaming TV | Yes | Binary flag |
| `StreamingMovies` | Categorical (Yes/No) | Whether customer has streaming movies | Yes | Binary flag |
| `Contract` | Categorical | Type of contract | Month-to-month | Key churn driver |
| `PaperlessBilling` | Categorical (Yes/No) | Paperless billing preference | Yes | Binary flag |
| `PaymentMethod` | Categorical | Payment method | Electronic check | Grouped into Digital / Non-digital |
| `MonthlyCharges` | Float | Monthly service charges | 70.35 | Used for revenue-at-risk calculation |
| `TotalCharges` | Float | Total amount charged to date | 1397.47 | Converted to numeric (some blanks imputed) |
| `Churn` | Categorical (Yes/No) | Whether customer left (target variable) | Yes | Encoded (1 = Churned, 0 = Retained) |

---

## 3️⃣ Engineered Fields (Created in Power Query / Python)

| Derived Field | Logic / Formula | Purpose |
|----------------|----------------|----------|
| `TenureGroup` | Binned `tenure` → (0–12, 13–24, 25–48, 49+) | Used for cohort & trend analysis |
| `ServicesCount` | Count of “Yes” across service columns | Indicates customer engagement level |
| `RevenueAtRisk` | If `Churn = Yes`, then `MonthlyCharges`, else 0 | Total potential monthly revenue loss |
| `Predicted_Probability` | Model output between 0 and 1 | Probability of churn (from Python model) |
| `Predicted_Label` | If `Predicted_Probability ≥ 0.5` → 1 else 0 | Predicted churn category |
| `RetentionScore` | `Predicted_Probability * MonthlyCharges` | Combines churn risk & customer value |
| `HighValueCustomer` | If `MonthlyCharges ≥ 70` & `tenure ≥ 12` | Used to flag key accounts |
| `ChurnFlag` | If `Churn = "Yes"` then 1 else 0 | Used for DAX-based metrics |

---

## 4️⃣ Data Quality Summary

| Check | Finding | Action Taken |
|--------|----------|---------------|
| Missing Values | `TotalCharges` had blanks | Imputed using median value |
| Duplicates | 0 | None removed |
| Outliers | A few high `MonthlyCharges` (>120) | Valid — premium plans |
| Inconsistent Categories | “No internet service” / “No phone service” entries | Merged & standardized |
| Imbalance | 26.5% churned vs 73.5% retained | Handled during ML phase (SMOTE or weighting) |
| Data Types | `TotalCharges` imported as text | Converted to float |

---

## 5️⃣ Power BI Data Model Relationships

| Table | Relationship | Cardinality | Usage |
|--------|---------------|--------------|--------|
| `Customer_Main` | `customerID` → base table | 1:1 | Primary data source |
| `Predictions` | `customerID` → `Predicted_Probability` | 1:1 (Left Join) | For model integration |
| `Revenue` | `customerID` → `MonthlyCharges` | 1:1 | Used for revenue KPIs |
| `Lookup_ContractType` | `Contract` → `ContractType` | Many-to-One | For slicers & grouping |

---

## 6️⃣ DAX Measures (Power BI Calculations)

| Measure Name | Formula / Definition | Purpose |
|---------------|----------------------|----------|
| **Churn Rate** | `DIVIDE(CALCULATE(COUNT(Customer[customerID]), Customer[Churn] = "Yes"), COUNT(Customer[customerID]))` | % of customers who churned |
| **Revenue At Risk** | `SUMX(FILTER(Customer, Customer[Churn]="Yes"), Customer[MonthlyCharges])` | Total monthly revenue lost |
| **Total Revenue** | `SUM(Customer[MonthlyCharges])` | Total monthly revenue base |
| **Revenue At Risk %** | `DIVIDE([Revenue At Risk],[Total Revenue],0)` | Portion of total revenue lost |
| **HighValueAtRisk** | `CALCULATE(COUNTROWS(Customer), FILTER(Customer, Customer[Predicted_Probability] ≥ 0.7 && Customer[MonthlyCharges] ≥ 70))` | Count of high-value at-risk customers |
| **Average Tenure** | `AVERAGE(Customer[tenure])` | Mean customer tenure |
| **RetentionScore** | `AVERAGEX(Customer, Customer[Predicted_Probability] * Customer[MonthlyCharges])` | Weighted retention KPI |
| **Precision@K** | Custom TOPN-based DAX using K parameter | Model accuracy for top K customers |
| **AdjustedChurnRate** | `[Churn Rate] * (1 - 'Discount%'[Discount % Value]/100)` | What-if scenario metric |
| **ServicesPerCustomer** | `AVERAGE(Customer[ServicesCount])` | Average services subscribed per user |

---

## 7️⃣ KPI Definitions

| KPI | Description | Business Meaning |
|------|--------------|------------------|
| **Churn Rate** | % of customers who left | Overall retention health |
| **Revenue at Risk (₹)** | Monthly revenue loss due to churn | Quantifies business impact |
| **Precision@K** | % of correctly predicted churners in top-K | Model performance metric |
| **Retention Score** | Risk × Value composite | Prioritization metric |
| **Average Tenure (months)** | Mean tenure across customers | Loyalty indicator |
| **High-Value at Risk (Count)** | # of profitable customers likely to churn | Target retention list |

---

## 8️⃣ Data Validation Checks (Post-Dashboard)

| Validation | Result | Notes |
|-------------|---------|-------|
| Slicer behavior | ✅ Correctly filters dependent visuals |
| Measure consistency | ✅ Matches Python outputs |
| Refresh test | ✅ Completed successfully |
| Tooltip functionality | ✅ Displays probability trend correctly |
| Export (Top-N) | ✅ CSV export validated |

---

## 9️⃣ Version History

| Version | Date | Description |
|----------|------|-------------|
| v1.0 | Sept 2025 | Initial import & cleaning in Power Query |
| v1.1 | Oct 2025 | Added derived columns & DAX measures |
| v1.2 | Oct 2025 | Completed dashboard visuals & model integration |
| v1.3 | Oct 2025 | Documentation & GitHub publishing |

---

⭐ *End of Document — Data Dictionary (Customer Churn Prediction & Retention Dashboard)*  
