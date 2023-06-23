# SQL Statements

## Query 1 - Total Gross Income and Income Difference by City
```sql
-- Calculate the total gross income for the month of February and previous month (January) for each city
-- Also, calculate the difference in gross income between February and January
SELECT
    s.city_code,
    2 AS month,
    SUM(CASE WHEN strftime('%m', s."Date") = '02' THEN s.gross_income ELSE 0 END) AS total_gross_income,
    SUM(CASE WHEN strftime('%m', s."Date") = '01' THEN s.gross_income ELSE 0 END) AS previous_month_income,
    SUM(CASE WHEN strftime('%m', s."Date") = '02' THEN s.gross_income ELSE 0 END) - SUM(CASE WHEN strftime('%m', s."Date") = '01' THEN s.gross_income ELSE 0 END) AS diff_gross_income
FROM
    sales s
WHERE
    strftime('%m', s."Date") IN ('01', '02')  -- Filter for January and February
GROUP BY
    s.city_code
ORDER BY
    diff_gross_income DESC
LIMIT 1;


