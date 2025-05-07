# 📊 Sales Insights: SQL + Power BI Project

This project demonstrates how to perform sales data analysis using **MySQL** and visualize it using **Power BI**. It covers database setup, SQL-based insights, and basic Power BI transformations.

---

## ⚙️ Prerequisites

- MySQL Server & MySQL Workbench installed locally
- Power BI Desktop
- `db_dump.sql` file (SQL dump of the database)

---

## 🏗️ Setup Instructions

### 1. Setup MySQL and Import the Database

1. Open **MySQL Workbench**.
2. Create a new schema:
   - Go to the **Schemas** tab.
   - Right-click → **Create Schema** → Name it `sales_insights`.
   - Click **Apply > Apply > Finish**.
3. Import the SQL dump:
   - Go to **File > Run SQL Script**.
   - Select `db_dump.sql`.
   - Set the default schema to `sales_insights`.
   - Click **Run**.

---

### 2. Connect MySQL to Power BI

1. Open **Power BI Desktop**.
2. Go to **Home > Get Data > MySQL Database**.
3. Enter:
   - **Server**: `localhost`
   - **Database**: `sales_insights`
4. Enter your MySQL credentials.
5. Select the required tables and load them.

---

## 🧠 SQL Queries for Data Insights

```sql
-- 1. Show all customer records
SELECT * FROM customers;

-- 2. Show total number of customers
SELECT COUNT(*) AS total_customers FROM customers;

-- 3. Show transactions for Chennai market (market code = 'Mark001')
SELECT * FROM transactions WHERE market_code = 'Mark001';

-- 4. Show distinct product codes sold in Chennai
SELECT DISTINCT product_code FROM transactions WHERE market_code = 'Mark001';

-- 5. Show transactions where currency is US Dollars
SELECT * FROM transactions WHERE TRIM(currency) = 'USD';

-- 6. Show transactions in 2020 (join with date table)
SELECT t.*, d.* 
FROM transactions t
JOIN date d ON t.order_date = d.date 
WHERE d.year = 2020;

-- 7. Show total revenue in 2020 (INR and USD only)
SELECT SUM(t.sales_amount) AS total_revenue_2020
FROM transactions t
JOIN date d ON t.order_date = d.date 
WHERE d.year = 2020 AND TRIM(t.currency) IN ('INR', 'USD');

-- 8. Show total revenue in January 2020
SELECT SUM(t.sales_amount) AS jan_2020_revenue
FROM transactions t
JOIN date d ON t.order_date = d.date 
WHERE d.year = 2020 AND d.month_name = 'January' 
AND TRIM(t.currency) IN ('INR', 'USD');

-- 9. Show total revenue in 2020 in Chennai
SELECT SUM(t.sales_amount) AS chennai_2020_revenue
FROM transactions t
JOIN date d ON t.order_date = d.date 
WHERE d.year = 2020 AND t.market_code = 'Mark001';

Formula to create norm_amount column in power query

= Table.AddColumn(#"Filtered Rows", "norm_amount",
    each if [currency] = "USD" or [currency] = "USD#(cr)" then [sales_amount] * 75 else [sales_amount], 
    type number)
