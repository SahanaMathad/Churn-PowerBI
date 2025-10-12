 Telco Customer Churn — Power BI Runbook
 Author: Sahana Mathad
 Date: 12 Oct 2025
 Tool: Microsoft Power BI Desktop
 Dataset: Telco Customer Churn Dataset


 1. Project Overview

 This Power BI project analyzes customer churn for a telecom company. The goal is to identify key
 churn drivers, estimate revenue at risk, and segment customers by retention risk and lifetime value.

 2. Data Model Design

 Star Schema using dim_customer, dim_date, and fact_billing tables. Relationships:
 dim_customer[CustomerID] → fact_billing[CustomerID]; dim_date[Date] →
 fact_billing[InvoiceDate]. All key fields hidden for cleaner visuals.

 3. Data Preparation

 Performed in Power Query: cleaned nulls, converted TotalCharges, created derived columns like
 ChurnFlag, TenureGroup, and NumServices. Added PredictedProbability_Col in DAX for simulated
 churn probability.

 4. Core Measures (DAX)

 Total Revenue = SUM(fact_billing[MonthlyCharges])
 Revenue At Risk = SUMX(FILTER(fact_billing, fact_billing[ChurnFlag] = 1),
 fact_billing[MonthlyCharges])
 Revenue At Risk % = DIVIDE([Revenue At Risk], [Total Revenue], 0)

 5. Pages Overview

 Page 1: Overview (KPIs, churn trends)
 Page 2: Drivers & Segments (contracts, services)
 Page 3: Cohort & Retention (heatmap, trends)
 Page 4: Financial (revenue at risk, what-if)
 Page 5: Retention Actions (scatter, top N customers).
 6. Navigation & Formatting
 Used collapsible overlay menu with bookmarks. Color theme: Blue #457B9D, Red #E63946, Green
 #2ECC71. Fonts: Segoe UI. Added page separators and aligned visuals consistently.

7. Key Insights

 • Churn Rate ~26% overall (highest in month-to-month contracts)
 • Fiber optic customers on e-check most at risk
 • Revenue at risk ≈ $1.5M monthly
 • Retention levers: longer contracts, small discounts
 • High-risk: high-charge, low-tenure customers.

 8. Deliverables

 Telco_Churn.pbix — Power BI report
 runbook.pdf — This guide
 data_dictionary.md — Dataset schema
 features.md — Engineered features
 Churn_analysis_Final.pdf — Presentation report.

<<<<<<< HEAD
 
=======
 
>>>>>>> 3e0a0257841b0602645dd6dfb146b08ea02ea5a5
