 sql_retail_sales_project
 Project Overview

## Project Title: Retail Sales Analysis    
Database: `sql_porfollio_project1`

This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.

 ## Objectives

1. Set up a retail sales database: Create and populate a retail sales database with the provided sales data.
2. Data Cleaning: Identify and remove any records with missing or null values.
3. Exploratory Data Analysis (EDA): Perform basic exploratory data analysis to understand the dataset.
4. Business Analysis: Use SQL to answer specific business questions and derive insights from the sales data.

 Project Structure

 1. Database Setup

- Database Creation: The project starts by creating a database named `p1_retail_db`.
- Table Creation: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

'''sql
DROP TABLE IF EXISTS retail_sales;

CREATE TABLE retail_sales (
transactions_id	INT PRIMARY KEY,
sale_date DATE,
sale_time TIME,
customer_id	INT,
gender	VARCHAR(15),
age	INT,
category VARCHAR(15),	
quantiy	INT,
price_per_unit FLOAT,
cogs 	FLOAT,
total_sale FLOAT

);

-- Step 1 : Data Cleaning 
-- CHECKING FOR NULLS
SELECT *
FROM RETAIL_SALES
WHERE transactions_id is null
OR sale_date is null
OR sale_time is null
or customer_id is null
or gender is null
or category is null
or quantiy is null
or price_per_unit is null 
or cogs is null
or total_sale is null;

-- We have three rows with null values and there is no much details about them so lets delete them

DELETE FROM RETAIL_SALES
	WHERE transactions_id is null
		OR sale_date is null
		OR sale_time is null
		or customer_id is null
		or gender is null
		or category is null
		or quantiy is null
		or price_per_unit is null 
		or cogs is null
		or total_sale is null;

-- Step 2: Data Exploration
-- How many sales do we have?
SELECT
COUNT(transactions_id)
from retail_Sales; -- 1997 sales

-- How many unique customers do we have?
SELECT
COUNT(distinct customer_id) from retail_sales; -- We have 155 unique customers

-- What are categories we have?
SELECT
DISTINCT category FROM retail_Sales; -- Electronics , Clothing and Beauty

-- Step 3: Data Analysis and Business key problems 
-- Task 1 - Write a SQL query to retrieve all columns for sales made on '2022-11-05
SELECT *
FROM retail_Sales
WHERE sale_date = '2022-11-05';

/*Task 2 - Write a SQL query to retrieve all transactions where the category 
is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022 */

SELECT 
*
FROM retail_Sales 
WHERE category = 'Clothing' 
AND TO_CHAR(sale_date , 'YYYY-MM') = '2022-11'
AND quantiy >= 4 ;

-- Task 3 - Write a SQL query to calculate the total sales (total_sale) for each category.
SELECT
category,
sum(total_sale)
FROM retail_sales
GROUP BY category

/* Task 4 - Write a SQL query to find the average age of 
customers who purchased items from the 'Beauty' category. */

SELECT 
AVG(age)
FROM retail_sales
where category = 'Beauty'

-- Task 5 - Write a SQL query to find all transactions where the total_sale is greater than 1000.

SELECT
transactions_id
FROM retail_sales
WHERE total_Sale > 1000

/* Task 6 - Write a SQL query to find the total number of 
transactions (transaction_id) made by each gender in each category. */
SELECT
category,
gender,
count(transactions_id) 
from retail_sales
group by gender , category
order by category;

/* Task 7- Write a SQL query to calculate the average sale for each month. 
Find out best selling month in each year */
select
avg_sales,
sales_year,
sales_month
from
(
SELECT
avg(total_sale) avg_sales,
RANK() OVER( PARTITION BY DATE_PART('year',sale_date) ORDER BY avg(total_sale) desc)  as rank_sales,
DATE_PART('year',sale_date) as sales_year,
DATE_PART('month', sale_date) as sales_month
from retail_sales
group by DATE_PART('month',sale_date) , DATE_PART('year',sale_date)
) t
where rank_sales = 1 -- June 2022 and feb 2023 have highest avg sales 

-- Task 8 - Write a SQL query to find the top 5 customers based on the highest total sales

SELECT
customer_id,
sum(total_sale)
from retail_sales
group by customer_id
order by sum(total_sale) desc
limit 5;

-- Task - Write a SQL query to find the number of unique customers who purchased items from each category

SELECT
category,
COUNT(customer_id)
from retail_sales
group by category

-- Task 10 - Write a SQL query to create each shift and number of orders (
--Example Morning <12, Afternoon Between 12 & 17, Evening >17)
select
count(transactions_id),
CASE
WHEN sale_hour < 12 then 'Morning'
WHEN sale_hour BETWEEN 12 AND 17 THEN 'Afternoon'
ELSE 'Evening'
END AS Sale_Shift
from 
(
SELECT
transactions_id,
extract('hour' from sale_time) AS sale_hour
from retail_sales
) 
GROUP BY Sale_Shift

SELECT 
*
FROM retail_sales

'''
## Findings

- **Customer Demographics**: The dataset includes customers from various age groups, with sales distributed across different categories such as Clothing and Beauty.
- **High-Value Transactions**: Several transactions had a total sale amount greater than 1000, indicating premium purchases.
- **Sales Trends**: Monthly analysis shows variations in sales, helping identify peak seasons.
- **Customer Insights**: The analysis identifies the top-spending customers and the most popular product categories.

## Reports

- **Sales Summary**: A detailed report summarizing total sales, customer demographics, and category performance.
- **Trend Analysis**: Insights into sales trends across different months and shifts.
- **Customer Insights**: Reports on top customers and unique customer counts per category.

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.

