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
```

## Query 2 - Income Increase for Electronic Accessories by Branch
```sql
-- Calculate the income increase percentage for the product line 'Electronic accessories' in March compared to the average income in January and February for each branch
SELECT
    b."Branch_Name" AS branch_name,
    t1.product_line,
    (t1.total_gross_income - t2.avg_gross_income) / t2.avg_gross_income * 100 AS income_increase
FROM
    (SELECT
        s.branch_code,
        s.product_line,
        SUM(CASE WHEN strftime('%m', s."Date") = '03' THEN s.gross_income ELSE 0 END) AS total_gross_income
    FROM
        Sales s
    WHERE
        s.product_line = 'Electronic accessories' AND strftime('%m', s."Date") = '03'
    GROUP BY
        s.branch_code, s.product_line) t1
JOIN
    (SELECT
        s.branch_code,
        s.product_line,
        AVG(s.gross_income) AS avg_gross_income
    FROM
        Sales s
    WHERE
        s.product_line = 'Electronic accessories' AND strftime('%m', s."Date") IN ('01', '02')
    GROUP BY
        s.branch_code, s.product_line) t2
ON
    t1.branch_code = t2.branch_code AND t1.product_line = t2.product_line
JOIN
    branch b ON b."Branch_Code" = t1.branch_code
ORDER BY
    income_increase ASC;
```

## Query 3 - Total Income for a Specific Date Range
```sql
-- Calculate the total income for each product line within a specific date range
SELECT
    s.product_line,
    SUM(s.gross_income) AS total_income
FROM
    sales s
WHERE
    DATE(s."Date") >= DATE('2019-01-01') AND DATE(s."Date") <= DATE('2019-01-08')
GROUP BY
    s.product_line
ORDER BY
    total_income DESC
LIMIT 1;

```

## Query 4 - Occurrences of Sales by Time
```sql
-- Count the number of sales occurrences for each hour of the day in February on weekends (Saturday and Sunday)
SELECT
    strftime('%H:%M', s."Time") AS time,
    COUNT(*) AS no_of_occurrences
FROM
    Sales s
JOIN
    (
        SELECT
            strftime('%w', s."Date") AS weekday,
            strftime('%H', s."Time") AS hour,
            s."Date"
        FROM
            Sales s
        WHERE
            strftime('%m', s."Date") = '02' AND
            strftime('%w', s."Date") IN ('0', '6')
    ) t ON strftime('%w', s."Date") = t.weekday AND strftime('%H', s."Time") = t.hour
GROUP BY
    time
ORDER BY
    no_of_occurrences DESC
LIMIT 5;

```

## Query 5 - Highest Gross Income by Date, Product Line, and Branch
```sql
-- Retrieve the highest gross income for each date, product line, and branch combination within a specific date range
SELECT
    s."Date",
    s.product_line,
    b."Branch_Name",
    s.gross_income
FROM
    Sales s
JOIN
    branch b ON s."Branch_Code" = b."Branch_Code"
WHERE
    s.product_line IN ('Electronic accessories', 'Food and beverages') AND
    DATE(s."Date") >= DATE('2019-02-01') AND DATE(s."Date") <= DATE('2019-02-07')
GROUP BY
    s."Date",
    s.product_line,
    b."Branch_Name"
HAVING
    s.gross_income = (
        SELECT
            MAX(gross_income)
        FROM
            Sales
        WHERE
            product_line = s.product_line AND
            "Date" = s."Date"
    )
ORDER BY
    s."Date",
    s.product_line;

```

## Query 6 - Top Product Lines by Total Gross Income for Each Customer Type
```sql
-- Retrieve the top product lines by total gross income for each customer type
SELECT 
    sub.Customer_type, 
    sub.Product_line, 
    sub.total_gross_income, 
    sub.no_of_purchases, 
    sub.rank
FROM (
    SELECT
        s.Customer_type,
        s.Product_line,
        SUM(s.gross_income) AS total_gross_income,
        COUNT(*) AS no_of_purchases,
        RANK() OVER (PARTITION BY s.Customer_type ORDER BY SUM(s.gross_income) DESC) AS rank
    FROM
        Sales s
    JOIN
        branch b ON s.Branch_Code = b.Branch_Code
    WHERE
        s.Customer_type <> 'Unknown'
    GROUP BY
        s.Customer_type,
        s.Product_line
) AS sub
WHERE
    sub.rank <= 3
ORDER BY
    sub.Customer_type,
    sub.total_gross_income DESC
LIMIT 6;

```

## Query 7 - Branch Income Grade
```sql
-- Calculate the gross income for each branch and categorize them as 'high income' or 'low income' based on a threshold of 500
SELECT
    b.branch_name,
    s.customer_type,
    SUM(s.gross_income) AS gross_income,
    CASE
        WHEN SUM(s.gross_income) >= 500 THEN 'high income'
        ELSE 'low income'
    END AS income_grade
FROM
    Sales s
JOIN
    branch b ON s.branch_code = b.branch_code
WHERE
    s.customer_type IN ('Member', 'Normal')
GROUP BY
    b.branch_name,
    s.customer_type
ORDER BY
    b.branch_name,
    s.customer_type;

```
## Query 8 - Top Selling Product Line for a Specific Date Range
```sql
-- Calculate the total income for each product line during a specific date range and display the product line with the highest total income
SELECT
    s.product_line,
    SUM(s.gross_income) AS total_income
FROM
    sales s
WHERE
    DATE(s."Date") >= DATE('2019-01-01') AND DATE(s."Date") <= DATE('2019-01-08')
GROUP BY
    s.product_line
ORDER BY
    total_income DESC
LIMIT 1;

```

## Query 9 - Hourly Occurrences on Weekends in February
```sql

-- Count the occurrences of sales transactions during specific hours on Saturdays and Sundays in February
SELECT
    strftime('%H:%M', s."Time") AS time,
    COUNT(*) AS no_of_occurrences
FROM
    Sales s
JOIN
    (
        SELECT
            strftime('%w', s."Date") AS weekday,
            strftime('%H', s."Time") AS hour,
            s."Date"
        FROM
            Sales s
        WHERE
            strftime('%m', s."Date") = '02' AND
            strftime('%w', s."Date") IN ('0', '6')
    ) t ON strftime('%w', s."Date") = t.weekday AND strftime('%H', s."Time") = t.hour
GROUP BY
    time
ORDER BY
    no_of_occurrences DESC
LIMIT 5;
```

## Query 10 - Highest Gross Income for Specific Product Lines and Dates
```sql
-- Retrieve sales data for specific product lines and dates, grouping by date, product line, and branch name,
-- and filter for the highest gross income for each combination
SELECT
    s."Date",
    s.product_line,
    b."Branch_Name",
    s.gross_income
FROM
    Sales s
JOIN
    branch b ON s."Branch_Code" = b."Branch_Code"
WHERE
    s.product_line IN ('Electronic accessories', 'Food and beverages') AND
    DATE(s."Date") >= DATE('2019-02-01') AND DATE(s."Date") <= DATE('2019-02-07')
GROUP BY
    s."Date",
    s.product_line,
    b."Branch_Name"
HAVING
    s.gross_income = (
        SELECT
            MAX(gross_income)
        FROM
            Sales
        WHERE
            product_line = s.product_line AND
            "Date" = s."Date"
    )
ORDER BY
    s."Date",
    s.product_line;

```













