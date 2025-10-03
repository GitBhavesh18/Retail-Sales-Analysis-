
# Retail Sales Analysis SQL Project (Personal Portfolio)

## Project Overview


**Project Title**: Retail Sales Analysis (MySQL)  
**Author**: Bhavesh  
**Level**: Beginner/Portfolio  
**Database**: `retail_sales`

This is my personal project to practice and showcase SQL skills for data analysis. I worked with a real-world retail sales dataset, imported it into MySQL, and wrote SQL queries to extract business insights. The project covers database setup, data cleaning, exploratory analysis, and answering business questions using SQL. It demonstrates my ability to work hands-on with data and MySQL for analytics.

## Objectives

1. **Set up a MySQL database**: Create and populate a retail sales database using the provided CSV data.
2. **Data Cleaning**: Identify and remove records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Use SQL to understand the dataset and its structure.
4. **Business Analysis**: Write SQL queries to answer business questions and generate insights.

## Project Structure


### 1. Database Setup

- **Database Creation**: I created a MySQL database named `retail_sales`.
- **Table Creation**: The table structure matches the columns in the provided CSV file (`SQL - Retail Sales Analysis_utf .csv`).


```sql
CREATE DATABASE retail_sales;
USE retail_sales;
-- Table creation SQL based on CSV columns
-- (See actual table structure in your SQL script)
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
-- Check and remove nulls as needed
```


### 3. Data Analysis & Insights


I wrote and executed several SQL queries (see `sql_query_p1.sql`) to answer business questions, such as:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
SELECT *
FROM retail_sales
WHERE sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
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

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retail_sales
GROUP BY 1
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retail_sales
WHERE category = 'Beauty'
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
SELECT * FROM retail_sales
WHERE total_sale > 1000
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retail_sales
GROUP 
    BY 
    category,
    gender
ORDER BY 1
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
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retail_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
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


## Key Findings

- The dataset covers multiple product categories and customer demographics.
- Identified top-selling products, high-value transactions, and sales trends by month and shift.
- Segmented customers and found top spenders using SQL aggregation and filtering.


## Reports & Deliverables

- SQL script: `sql_query_p1.sql` (contains all analysis queries)
- Data file: `SQL - Retail Sales Analysis_utf .csv`
- This README summarizing the project and findings


## Conclusion

This project demonstrates my ability to work with real-world data in MySQL, perform data cleaning, write complex SQL queries, and extract actionable business insights. It is part of my data analytics portfolio.


## How to Use

1. Clone this repository.
2. Set up MySQL and create the `retail_sales` database.
3. Import the CSV data into a table matching the CSV columns.
4. Run the queries from `sql_query_p1.sql` in MySQL Workbench or your preferred client.
5. Explore and modify queries to gain further insights.


## Author

Bhavesh

This project is part of my personal portfolio. If you have questions or feedback, feel free to reach out!
