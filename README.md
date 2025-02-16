# Retail Sales Analysis SQL Project

## Project Overview

**Project Title**: Retail Sales Analysis  
**Database**: `p1_retail_db`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `Retail Sales Analysis Project`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
-- SQL Retail Sales Analysis
CREATE DATABASE Retail Sales Analysis Project;


-- Table Creation
CREATE TABLE retail_sales
(
	transactions_id INT PRIMARY KEY,
	sale_date DATE,
	sale_time TIME,
	customer_id INT,
	gender VARCHAR(15),
	age INT,
	category VARCHAR(15),
	quantity INT,
	price_per_unit FLOAT,
	cogs FLOAT,
	total_sale FLOAT
);
```

### 2. Data Exploration & Cleaning
```sql
--Viewing Data after importing
SELECT * 
FROM retail_sales;
LIMIT 10;

SELECT COUNT(*) 
FROM retail_sales;


--Data Cleaning
--Checking NULL values
SELECT * 
FROM retail_sales
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
	quantity IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;


-- Deleting NULL rows
DELETE 
FROM retail_sales 
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
	quantity IS NULL
	OR
	cogs IS NULL
	OR
	total_sale IS NULL;

--Data Exploration 
--How many sales we have?
SELECT 
	COUNT(*) as total_sale 
FROM retail_sales;

--How many distinct customer we have?
SELECT 
	COUNT(DISTINCT customer_id) as customers 
FROM retail_sales;

--How many categories we have?
SELECT 
	DISTINCT category FROM 
retail_sales;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Retrieve all columns for sales made on 2022-11-05**:
```sql
SELECT * 
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Retrieve all sales from 'Clothing' category where quantity sold is greater than equal to 4 for Novemeber 2022**:
```sql
SELECT 
  *
FROM retail_sales
WHERE 
    category = 'Clothing'
    AND 
    TO_CHAR(sale_date, 'YYYY-MM') = '2022-11'
    AND
    quantity >= 4
```

3. **Calculate total sales for each category**:
```sql
SELECT 
	category, 
	SUM(total_sale) as net_sale, 
	COUNT (*) as total_orders 
FROM retail_sales
GROUP BY 1;
```

4. **Calculate average age of customers who bought item from 'Beauty' category**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Show transactions where total_sale is greater than 1000**:
```sql
SELECT *
FROM retail_sales
WHERE total_sale > 1000;

```

6. **Calculate transactions made by each gender in each category**:
```sql
SELECT 
	category,
	gender,
	COUNT(*) as total_transactions
FROM retail_sales
GROUP BY 1 , 2
ORDER BY 1;
```

7. **Check the average sale for each month and find the best month in each year**:
```sql
SELECT 
	year,
	month,
	avg
FROM
(
	SELECT 
		EXTRACT(YEAR FROM sale_date) AS year,
		EXTRACT(MONTH FROM sale_date) AS month,
		AVG(total_sale) AS avg,
		RANK() OVER(PARTITION BY EXTRACT(YEAR FROM sale_date) ORDER BY AVG(total_sale) DESC)
	FROM retail_sales
	GROUP BY 1 , 2
) as q
WHERE rank = 1;
```

8. **Get the top 5 customers based on highest total sales**:
```sql
SELECT 
	customer_id,
	SUM(total_sale) as purchase
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5;
```

9. **Find unique customers for each category**:
```sql
SELECT 
	category,
	COUNT(DISTINCT customer_id) as unique_customer
FROM retail_sales
GROUP BY 1;
```

10. **Create shifts based on time and find the number of orders in each shift**:
```sql
WITH hourly_sales
AS
(
	SELECT *,
		CASE
			WHEN EXTRACT(HOUR FROM sale_time) < 12 THEN 'Morning' 
			WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon' 
			ELSE 'Evening'
		END AS shift
	FROM retail_sales
)
SELECT 
	shift,
	COUNT(*) as total_sales
FROM hourly_sales
GROUP BY shift;
```

## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.


