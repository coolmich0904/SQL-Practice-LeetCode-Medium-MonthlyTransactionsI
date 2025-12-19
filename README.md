# LeetCode: Monthly Transactions I (Oracle SQL)

## Problem Description
The goal is to find the number of transactions and their total amounts, as well as the number of approved transactions and their total amounts, for each month and country.

## SQL Solution
```sql
SELECT 
    TO_CHAR(trans_date, 'YYYY-MM') AS month,
    country,
    COUNT(id) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM Transactions
GROUP BY TO_CHAR(trans_date, 'YYYY-MM'), country;
```
## Key Learning Points
### 1.Logical query Processing Order
  : In Oracle SQL, the execution order is (프원그해셀오)
  FROM --> WHERE --> GROUP BY --> HAVING --> SELECT --> ORDER BY
 
### 2. Group By Alias Restriction
  - The Issue: Since the `GROUP BY` clause is processed **before** the `SELECT` clause, the alias `month` is not yet defined during the grouping stage.
- The Solution: Therefore, the full expression `TO_CHAR(trans_date, 'YYYY-MM')` must be repeated in the `GROUP BY` clause to avoid errors.
- Personal Note: I learned this by troubleshooting a "column not found"

- Conditional Aggregation
  : I used the "CASE WHEN" statement within the "SUM" function to perform conditional aggregation. This approach is highly compatibloe across different Oracle versions compared to the newer "Filter" clause
  


