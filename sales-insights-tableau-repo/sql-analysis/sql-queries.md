# SQL Queries — Sales Insights Project

Below are all the SQL queries used for data exploration.  


```sql
USE sales;

-- All transactions
SELECT * FROM transactions;

-- Count total transactions
SELECT COUNT(*) FROM transactions; -- 150281

-- Transactions only from Chennai
WITH chennai_transactions AS (
  SELECT product_code,
         customer_code,
         t.market_code,
         order_date,
         sales_qty,
         sales_amount,
         currency,
         markets_name
  FROM transactions t
  JOIN markets m 
    ON t.market_code = m.markets_code
  WHERE markets_name = 'Chennai'
)
SELECT COUNT(*) FROM chennai_transactions;

-- Transactions in USD
SELECT * FROM transactions WHERE currency = 'USD';

-- Total transactions by year (with currency conversion)
SELECT *,
       CASE
         WHEN t.sales_amount < 0 THEN 0
         WHEN currency = 'INR' THEN t.sales_amount
         WHEN currency = 'USD' THEN t.sales_amount * 87.58
         ELSE t.sales_amount
       END AS modified_sales_amount
FROM transactions t
JOIN `date` d ON t.order_date = d.`date`;

-- Create temporary table for date-wise sales
CREATE TEMPORARY TABLE date_wise_sales AS
SELECT *,
       CASE
         WHEN t.sales_amount < 0 THEN 0
         WHEN currency = 'INR' THEN t.sales_amount
         WHEN currency = 'USD' THEN t.sales_amount * 87.58
         ELSE t.sales_amount
       END AS modified_sales_amount
FROM transactions t
JOIN `date` d ON t.order_date = d.`date`;

-- Yearly sales
SELECT SUM(modified_sales_amount) AS total_sales,
       `year`
FROM date_wise_sales  
GROUP BY `year`;

-- Yearly sales for Chennai
SELECT SUM(modified_sales_amount) AS total_sales,
       `year`
FROM date_wise_sales  
WHERE market_code = 'Mark001' 
GROUP BY `year`;

-- Yearly + monthly sales
SELECT SUM(modified_sales_amount) AS total_sales,
       `year`,
       month_name
FROM date_wise_sales
GROUP BY `year`, month_name
ORDER BY total_sales;

-- Distinct products sold in Chennai
SELECT DISTINCT(product_code) 
FROM transactions 
WHERE market_code = 'Mark001';

-- Number of markets per zone
SELECT zone,
       COUNT(DISTINCT markets_name) AS total_markets,
       GROUP_CONCAT(DISTINCT markets_name ORDER BY markets_name SEPARATOR ', ') AS market_names
FROM markets
GROUP BY zone;

-- Rank markets by sales within each zone
CREATE TEMPORARY TABLE modified_sales_table AS
SELECT market_code,
       CASE
         WHEN sales_amount < 0 THEN 0
         WHEN currency = 'INR' THEN sales_amount
         WHEN currency = 'USD' THEN sales_amount * 87.58
         ELSE sales_amount
       END AS modified_sales_amount
FROM transactions;

WITH sales_zone_markets_name AS (
  SELECT zone,
         markets_name,
         SUM(modified_sales_amount) AS total
  FROM markets m
  LEFT JOIN modified_sales_table t 
    ON m.markets_code = t.market_code 
  GROUP BY zone, markets_name
  ORDER BY zone, markets_name
)
SELECT zone,
       markets_name,
       total,
       DENSE_RANK() OVER(PARTITION BY zone ORDER BY total) AS rank_markets
FROM sales_zone_markets_name;

-- Distinct product types
SELECT DISTINCT product_type 
FROM products;
