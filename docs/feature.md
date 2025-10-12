# Feature Engineering Summary – Telco Customer Churn

| Feature | Created In | Formula / Logic | Description / Purpose |
|----------|-------------|----------------|-----------------------|
| avg_monthly_spend | Power Query | TotalCharges / tenure | Calculates average monthly customer spend |
| num_services | Power Query | Count of active service subscriptions | Reflects customer engagement depth |
| has_support_calls_30d | Power Query | Simulated using customerID pattern | Indicator of recent support contact |
| on_contract_less_than_3m | Power Query | tenure < 3 → 1 | Identifies new, potentially unstable customers |
| AvgSpendPerService | DAX Measure | avg_monthly_spend ÷ num_services | Normalizes spend per service |
