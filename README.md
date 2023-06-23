# Supermarket-sales
Supermarket Sales Analysis

This repository contains SQL queries and a dataset analysis for a large multinational company that operates a chain of hypermarkets and retail companies. The provided dataset, Dataset_supermarket_sales, includes information about sales transactions, customer types, products, branches, and more. The goal is to generate reports and answer key business questions using SQL queries.

Dataset Description
The dataset consists of the following attributes:

Invoice id: Computer-generated sales slip invoice identification number.
Branch: Branch of supermarkets.
City: Location of supermarkets.
Customer type: Type of customers, recorded as "Members" for customers using a member card and "Normal" for customers without a member card.
Gender: Gender type of the customer.
Product line: General item categorization groups, including "Electronic accessories", "Fashion accessories", "Food and beverages", "Health and beauty", "Home and lifestyle", and "Sports and travel".
Unit price: Price of each product in dollars ($).
Quantity: Number of products purchased by the customer.
Tax: 5% tax fee for customer buying.
Total: Total price including tax.
Date: Date of purchase (Record available from January 2019 to March 2019).
Time: Purchase time (10 am to 9 pm).
Payment: Payment method used by the customer for the purchase (Cash, Credit card, or E-wallet).
COGS: Cost of goods sold.
Gross margin percentage: Gross margin percentage.
Gross income: Gross income.
Rating: Customer stratification rating on their overall shopping experience, on a scale of 1 to 10.
Business Questions
The following business questions are addressed using SQL queries:

Data Quality Issues: The data quality issues present in the dataset are identified and discussed. These include missing date entries, inconsistency in reporting formats, inconsistent values for gender and customer type, invalid city and branch codes, and inconsistent capitalization.
Data Cleaning Approach: The preferred approach for data cleaning is discussed, with Excel being chosen due to its user-friendly interface and suitability for small to medium-sized datasets. Steps for data cleaning are provided, including fixing missing values, correcting dates, handling incorrect city and branch codes, filling membership blanks, and adding calculated fields for total and income.
Data Cleaning Steps: Detailed steps for data cleaning are outlined, covering the identification of misleading data, filling missing values, manual correction of dates, handling incorrect codes, filling membership blanks, adding calculated fields, and rechecking the data to ensure its suitability for analysis.
Queries and Analysis
To analyze the dataset and answer business questions, SQL queries are provided. The queries cover a range of tasks such as retrieving sales information, calculating totals and averages, identifying top performers, and analyzing customer ratings. Each query is accompanied by a description of its purpose and the expected results.

The queries address the following tasks:

Importing the data into the target database.
Retrieving sales information by branch and product line.
Analyzing sales trends by month and product line.
Calculating the average gross income and quantity sold by product line.
Analyzing the performance of branches based on gross income and quantity sold.
Identifying the top-selling products in each branch.
Analyzing the distribution of customers by type and gender.
Identifying the top customers by gross income.
Analyzing the distribution of ratings by product line.
Identifying the top-rated products.
Analyzing the income grade by branch and customer type.
Analyzing the quantity sold and difference from previous and next days for specific product lines.
Analyzing the average income and quantity sold by city and customer type.
Analyzing the total income and quantity sold on weekends by product line and city.
Conclusion
This repository provides SQL queries and analysis for a supermarket sales dataset, addressing key business questions and providing insights into sales performance, product trends, customer behavior, and more. The SQL queries can be used to generate reports and analyze the provided dataset or similar datasets in the future.
