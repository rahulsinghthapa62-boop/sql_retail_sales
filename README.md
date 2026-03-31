# Retail Sales Analysis SQL Project

## Project Overview

This project showcases my SQL analysis of retail sales data, focusing on data cleaning, exploratory data analysis (EDA), and solving business-driven questions. By developing this end-to-end SQL workflow, I have demonstrated my ability to transform raw sales records into actionable insights, utilizing core data analyst techniques to support strategic decision-making.

## Objectives

1. **Set up a retail sales database**: Create and populate a retail sales database with the provided sales data.
2. **Data Cleaning**: Identify and remove any records with missing or null values.
3. **Exploratory Data Analysis (EDA)**: Perform basic exploratory data analysis to understand the dataset.
4. **Business Analysis**: Use SQL to answer specific business questions and derive insights from the sales data.

## Project Structure

### 1. Database Setup

- **Database Creation**: The project starts by creating a database named `retail_sales_analysis`.
- **Table Creation**: A table named `retail_sales` is created to store the sales data. The table structure includes columns for transaction ID, sale date, sale time, customer ID, gender, age, product category, quantity sold, price per unit, cost of goods sold (COGS), and total sale amount.

```sql
CREATE DATABASE retail_sales;

create table retail_sales(
	transactions_id	int primary key,
	sale_date	date,
	sale_time	time,
	customer_id	int,
	gender	varchar(15),
	age	int,
	category varchar(15)	,
	quantity	int,
	price_per_unit	numeric(10,2),
	cogs	numeric(10,2),
	total_sale numeric(10,2)
);
```

### 2. Data Exploration & Cleaning

- **Record Count**: Determine the total number of records in the dataset.
- **Customer Count**: Find out how many unique customers are in the dataset.
- **Category Count**: Identify all unique product categories in the dataset.
- **Null Value Check**: Check for any null values in the dataset and delete records with missing data.

```sql
select count(*) as total_sales from retail_sales;
select count(distinct customer_id) as unique_id from retail_sales;;
select distinct category from retail_sales;

select * from retail_sales
where transactions_id is null
or
sale_date is null
or
sale_time is null
or
gender is null
or
category is null
or
quantity is null
or
cogs is null
or
total_sale is null;

delete  from retail_sales
where transactions_id is null
or
sale_date is null
or
sale_time is null
or
gender is null
or
category is null
or
quantity is null
or
cogs is null
or
total_sale is null;
```

### 3. Data Analysis & Findings

The following SQL queries were developed to answer specific business questions:

1. **Write a SQL query to retrieve all columns for sales made on '2022-11-05**:
```sql
select * from retail_sales
where sale_date = '2022-11-05';
```

2. **Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022**:
```sql
select * from retail_sales
	where category ILIKE 'clothing' 
	and
	quantity >=4
	and
	to_char(sale_date,'YYYY-MM')='2022-11' ;
```

3. **Write a SQL query to calculate the total sales (total_sale) for each category.**:
```sql
select category,sum(total_sale) as net_Sales from retail_sales
group by 1
order by 2;
```

4. **Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.**:
```sql
select round(avg(age),2) from retail_Sales
where category ilike 'beauty';
```

5. **Write a SQL query to find all transactions where the total_sale is greater than 1000.**:
```sql
select * from retail_sales 
where total_sale > 1000;
```

6. **Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.**:
```sql
select 
	category,
	gender,
	count(transactions_id)
from retail_sales
group by category,gender;
```

7. **Write a SQL query to calculate the average sale for each month. Find out best selling month in each year**:
```sql
select * from (
	select 
		extract(month from sale_date) as month,
		extract(year from sale_date) as year,
		avg(total_sale) as total_sale, 
		rank() over(partition by extract(year from sale_date)
					order by avg(total_sale) desc ) as rank
	from retail_sales
		group by 1,2) as t1
		where rank = 1 ;
```

8. **Write a SQL query to find the top 5 customers based on the highest total sales **:
```sql
select 
	customer_id,
	sum(total_sale) as total_sale 
from retail_sales
	group by 1 
	order by 2 desc
	limit 5 ;
```

9. **Write a SQL query to find the number of unique customers who purchased items from each category.**:
```sql
select 
	category,
	count(distinct customer_id) as cnt_unq_cust 
from retail_sales
	group by 1;
```

10. **Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)**:
```sql
with hourly_sale as(
	select *,
		case 
			when extract(hour from sale_time) <12 then 'Morning'
			when extract(hour from sale_time) between 12 and 17 then 'afternoon'
			else 'evening'
		end as shift
	from retail_sales)

select shift,count(transactions_id) from hourly_sale
group by 1;
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

## Conclusion

This project serves as a comprehensive introduction to SQL for data analysts, covering database setup, data cleaning, exploratory data analysis, and business-driven SQL queries. The findings from this project can help drive business decisions by understanding sales patterns, customer behavior, and product performance.
