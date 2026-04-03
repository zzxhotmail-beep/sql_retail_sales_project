# Retail Sales Analysis | SQL Portfolio Project

## Project Overview

This portfolio project analyzes retail sales data using PostgreSQL, simulating real-world data analyst workflows. The project covers the full analysis pipeline: database setup, data cleaning, exploratory data analysis (EDA), advanced SQL querying, and business-focused insights.

It demonstrates my ability to translate raw data into actionable business recommendations—critical skills for driving retail business growth as a Data Analyst.

## Core Objectives

- Design and implement a retail sales database to store transactional data efficiently.
- Perform data cleaning to ensure data accuracy and reliability.
- Conduct exploratory data analysis (EDA) to identify patterns and trends.
- Develop targeted SQL queries to answer business-critical questions.
- Present findings clearly and connect insights to business decision-making.

## Technical Skills Demonstrated

- **SQL (PostgreSQL)**: Database creation, table design, data population
- **Data Cleaning**: Handling null/missing values for data integrity
- **Exploratory Data Analysis (EDA)**: Aggregate functions, DISTINCT counts, and trend analysis
- **Advanced SQL Techniques**: Window functions (RANK), CTEs, CASE statements, GROUP BY, filtering
- **Business Acumen**: Translating business questions into SQL queries and deriving actionable insights

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `sql_project_p1`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE sql_project_p1;

DROP TABLE IF EXISTS retail_sales;
CREATE TABLE retail_sales
  (
	transactions_id INT PRIMARY KEY,
	sale_date	DATE,
	sale_time	TIME,
	customer_id	INT,
	gender	VARCHAR(15),
	age	INT,
	category VARCHAR(15),	
	quantiy	INT,
	price_per_unit	FLOAT,
	cogs	FLOAT,
	total_sale FLOAT
  );
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
SELECT COUNT(*) FROM retail_sales;
SELECT COUNT(DISTINCT customer_id) FROM retail_sales;
SELECT DISTINCT category FROM retail_sales;

SELECT * FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;

DELETE FROM retail_sales
WHERE 
    transactions_id IS NULL
    OR
    sale_date IS NULL
    OR 
    sale_time IS NULL
    OR
    gender IS NULL
    OR
    category IS NULL
    OR
    quantiy IS NULL
    OR
    cogs IS NULL
    OR
    total_sale IS NULL;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05'**:
```sql
SELECT * 
FROM retail_sales
where sale_date = '2022-11-05'
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
SELECT *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantiy >= 4
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category**:
```sql
SELECT 
    category,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY category
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category**:
```sql
SELECT 
    ROUND(AVG(age),2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000**:
```sql
SELECT * 
FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP BY 
    category,
    gender
ORDER BY category
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
SELECT 
       year,
       month,
       avg_sale
FROM 
(    
SELECT 
    EXTRACT(YEAR FROM sale_date) as year,
    EXTRACT(MONTH FROM sale_date) as month,
    AVG(total_sale) as avg_sale,
    RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC) as rank
FROM retail_sales
GROUP BY 1, 2
) as t1
WHERE rank = 1
ORDER BY 3 DESC
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales**:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY customer_id
ORDER BY total_sales DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retail_sales
GROUP BY category
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
WITH hourly_sale
AS
(
SELECT *,
    CASE
        WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning'
        WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
        ELSE 'Evening'
    END as shift
FROM retail_sales
)
SELECT 
    shift,
    COUNT(*) as total_orders    
FROM hourly_sale
GROUP BY shift
```

## Findings

- **Customer Demographics**: The dataset covers customers across different age groups, with sales distributed among various product categories, including Clothing and Beauty.
- **High-Value Transactions**: Several transactions exceed a total sale of 1000, highlighting the presence of premium purchases.
- **Sales Trends**: Monthly sales analysis reveals fluctuations, helping to identify peak seasons and periods of high activity.
- **Customer Insights**: The analysis identifies top-spending customers and the most popular product categories, providing actionable insights for business strategy.

## Conclusion

This project provides a thorough introduction to SQL for data analysts, encompassing database setup, data cleaning, exploratory data analysis, and business-focused SQL queries. The findings can inform business decisions by revealing sales patterns, customer behavior, and product performance trends.

## How to Run

1. Create the database and tables using sql_queries_p1.sql
2. Import the dataset (retail_sales.csv)
3. Execute queries to replicate analysis and generate insights

## Author - Zixuan Zhang

This project is part of my portfolio, showcasing the SQL skills essential for data analyst roles. 
- **LinkedIn**: [My Professional Profile](https://www.linkedin.com/in/zixuan-zhang-78ba38274)

## Notes

This project is part of my professional portfolio and demonstrates my ability to:
- Write efficient SQL queries
- Perform structured data analysis
- Transform raw data into actionable business insights


Thank you very much!
