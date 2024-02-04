# Bussiness Sales Analysis Project


#### Project overview

This data analysis project aims to provide insights into the sales performance of a Pizza company over the 2015 year. By analyzing various aspects of the sales data, we seek to identify trends, make data-driven recommendations, and gain a deeper understanding of the company's performance along with specific requirements by the client.


# Ask

### Problem Statement

#### KPI's Requirement

We need to analyze key indicators for our pizza sales data to gain insights into our business performance. Specifically, we want to calculate the following metrics:
1. Total Revenue: The sum of the total price of all pizza orders.
2. Average Order Value: The average amount spent per order, calculated by dividing the total revenue by the total number of orders.
3. Total Pizzas Sold: The sum of the quantities of all pizzas sold.
4. Total Orders: The total number of orders placed.
5. Average Pizzas Per Order: The average number of pizzas sold per order, calculated by dividing the total number of pizzas sold by the total number of orders.

#### Charts Requirement & Explainatory Data Analysis (EDA)

We would like to visualize various aspects of our pizza sales data to gain insights and understand key trends. We have identified the following requirements for creating charts:
- What is the daily trend for total orders?
- What is the hourly trend for total orders?
- What is the percentage of sales for each pizza category?
- What is the percentage of sales for each pizza size?
- How many total pizzas were sold for each pizza category?
- Which pizzas are the top 5 best sellers based on total pizzas sold?
- Which pizzas are the bottom 5 worst sellers based on total pizzas sold?


# Prepare

### Data source & Integrity

The Primary dataset used for this analysis is the "pizza_sales.xlsx" file, containing detailed infoprmation about various aspects of the sales made by the company.
this Data was primarily Stored in a Kaggle dataset and was downloaded. The Data is in long format and was initially very Dirty. The data is Relialbe, Credible and comprehensive. it has all the information required for us to answer the bussiness questions. Initially SQL was used to clean and get our required KPI's and tables to form charts.


# Process

### Tools Used

- Microsoft SQL Server :- Initial Cleaning, filtering & sorting and Data Validation.
- Micersoft Excel :- Data cleaning, Data analysis and Visualisations.

#### **Data Integrity Assurance:**
   - Regularly conducted data integrity checks.
   - Implemented checksums and validation routines.

#### **Verification of Data Readiness:**
   - Performed exploratory data analysis (EDA) for outliers.
   - Validated data against predefined documentation done through SQL.

#### **Documentation of Cleaning Process:**
   - Maintained a detailed data cleaning log.
   - Used version control for tracking changes.


# Analyse

### Pizza Sales Querries Used In The Project

#### A. KPI’s
1. Total Revenue:
``` SQL
SELECT SUM(total_price) AS Total_Revenue FROM pizza_sales;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/7ebae309-3a9d-4745-9cb8-c06d0c78f9c6)


2. Average Order Value
``` SQL
SELECT (SUM(total_price) / COUNT(DISTINCT order_id)) AS Avg_order_Value FROM pizza_sales;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/55cab2ca-7557-4140-a0f3-6e8d7aeba614)


3. Total Pizzas Sold
``` SQL
SELECT SUM(quantity) AS Total_pizza_sold FROM pizza_sales;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/dd200d58-a888-433f-8b13-b471dd7c7f5f)

4. Total Orders
```SQL
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/acbcfdd1-427a-4e9e-a1b4-61f49e716044)

5. Average Pizzas Per Order
``` SQL
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / 
CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) AS DECIMAL(10,2))
AS Avg_Pizzas_per_order
FROM pizza_sales;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/f4bf4a1a-82a0-4ace-af8d-ad700aa01e5f)

#### B. Daily Trend for Total Orders
``` SQL
SELECT DATENAME(DW, order_date) AS order_day, COUNT(DISTINCT order_id) AS total_orders 
FROM pizza_sales
GROUP BY DATENAME(DW, order_date);
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/4d10a322-7d0a-4f10-a1d3-67f7866939e2)

#### C. Hourly Trend for Orders
``` SQL
SELECT DATEPART(HOUR, order_time) as order_hours, COUNT(DISTINCT order_id) as total_orders
from pizza_sales
group by DATEPART(HOUR, order_time)
order by DATEPART(HOUR, order_time);
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/dbb8dbec-0359-434a-b796-cf5eebe614cf)

#### D. Percent of Sales by Pizza Category
``` SQL
SELECT pizza_category, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_category;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/166d30ff-2417-426a-8e42-a1b145469891)

#### E. Percent of Sales by Pizza Size
```SQL
SELECT pizza_size, CAST(SUM(total_price) AS DECIMAL(10,2)) as total_revenue,
CAST(SUM(total_price) * 100 / (SELECT SUM(total_price) from pizza_sales) AS DECIMAL(10,2)) AS PCT
FROM pizza_sales
GROUP BY pizza_size
ORDER BY pizza_size;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/5004cc9e-3fc6-404c-9daa-0c438df45def)

#### F. Total Pizzas Sold by Pizza Category
```SQL
SELECT pizza_category, SUM(quantity) as Total_Quantity_Sold
FROM pizza_sales
WHERE MONTH(order_date) = 2
GROUP BY pizza_category
ORDER BY Total_Quantity_Sold DESC;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/9e564bbd-0c43-4d8e-a14d-c87e721886a3)



  






