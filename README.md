# Customer Sales Analysis Project



### Project overview

This data analysis project aims to provide insights into the sales performance of a Pizza company over the 2015 year. By analyzing various aspects of the sales data, we seek to identify trends, make data-driven recommendations, and gain a deeper understanding of the company's performance along with specific requirements by the client.
---

# `Ask`

### Business Problem Statement

In the fast-paced and competitive food industry, understanding sales performance is crucial for any business to thrive and grow. Our client, a renowned Pizza company, seeks to gain a comprehensive understanding of their sales performance for the fiscal year 2015. The objective of our Customer Sales Analysis project is to meticulously examine diverse facets within the sales data, uncover pertinent trends, and formulate actionable insights. Through this rigorous analysis, we aim to provide data-driven recommendations that will enable our client to make informed decisions and strategically plan for future growth and optimization. The challenge lies in effectively analyzing the vast amount of sales data to derive meaningful insights that can directly contribute to the company's bottom line and overall success.

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

---

# `Prepare`

### Data source & Integrity

The Primary dataset used for this analysis is the [pizza_sales.xlsx](https://1drv.ms/x/s!AjKhR_ndv-LThkg2pyCeKcz983VS?e=7x3veL) file, containing detailed infoprmation about various aspects of the sales made by the company.
this Data was primarily Stored in a Kaggle dataset and was downloaded. The Data is in long format and was initially very Dirty. The data is Relialbe, Credible and comprehensive. it has all the information required for us to answer the bussiness questions. Initially SQL was used to clean and get our required KPI's and tables to form charts.

---

# `Process`

### Tools Used

- Microsoft SQL Server :- Initial Cleaning, filtering & sorting and Data Validation.
- Microsoft Excel :- Data cleaning, Data analysis and Visualisations.

#### **Data Integrity Assurance:**
   - Regularly conducted data integrity checks.
   - Implemented checksums and validation routines.

#### **Verification of Data Readiness:**
   - Performed exploratory data analysis (EDA) for outliers.
   - Validated data against predefined documentation done through SQL.

#### **Documentation of Cleaning Process:**
   - Maintained a detailed data cleaning log.
   - Used version control for tracking changes.

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

#### G. Top 5 Best Sellers by Total Pizzas Sold
``` SQL
SELECT Top 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold DESC;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/26a5a17a-74ce-44bd-bc83-b770258d9ba9)

#### H. Bottom 5 Best Sellers by Total Pizzas Sold
``` SQL
SELECT TOP 5 pizza_name, SUM(quantity) AS Total_Pizza_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Pizza_Sold ASC;
```
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/0503be64-f8b7-41f4-8dae-f2838c1c4fdc)




### We will later use these query results for Data Validation in Excel before making each Pivot chart.
---

# `Analysis`

## Data Importing & formatting in Excel
After firing all our queries in Microsoft SQL server, we go ahead and import this data into Excel from Microsoft SQL Server for further analysis. 

## Pivots for EDA and KPI's

### A. KPI's 

![Screenshot 2024-02-04 154238](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/ed6d00f6-d258-4805-aaa0-74548ef420f5)

### B. Trends for total orders
1. Daily Orders

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/8e6f57dd-943d-410f-ae04-8ff4590a0305)  

Chart:

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/ae795616-6fac-441b-b8e3-afe08e58ce47)

2. Hourly Orders
   
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/80e65dfe-1838-429e-bad1-76772d605e50)

Chart:

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/e05532d8-c4a4-4a19-8419-6633beb7af25)

### Percentage of sales
1. % of Sales by pizza size

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/e04b8c4f-9ef4-4615-bcc3-831535ba04ff)

Chart: 

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/9f8865b8-ed2f-456e-929f-ed1cfcb44613)

2. % of sales by pizza name

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/3dafea61-3693-4571-8c9e-220499d6fd3d)

Chart: 

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/55983548-2548-4437-90a2-1c37d94ff580)

### Daily Trend For Orders
		
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/4c242a04-ce2c-4b68-b983-3d343dffbed9)


Chart: We had to bring the values out of this pivot to make a funnel Chart.
		
![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/f50da00a-024e-4e63-8b70-e4d6b0a5b16a)

### Top & Bottom five sellers

1. Top five best selling pizzas

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/8e56091e-500c-4938-b999-52f63f5a05ce)

Chart: 

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/cf171dab-a10f-4a1f-9563-18a8f514dc93)

2. Bottom five worst selling pizzas

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/7596c5a8-044d-4db1-92e4-320c772f0aee)

 Chart: 

 ![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/3911898f-94d6-4a93-9151-eeba7c497fba)

### Timeline slicer

![image](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/18d0c3ee-ceab-486f-a3cb-e44879320168)



## All these Pivot charts that you saw above are really in for some serious glow-up for the sharing phase 😙

---

# `Share`

## To present our findings to our Stakeholders a Dynamic Dashboard is prepared in Excel.


![Screenshot 2024-02-04 153637](https://github.com/Luckychoudharyy/Sales_Analysis_Project/assets/157785333/6eb0dfc8-6619-4403-86fb-126764967e41)


All the pivot charts were moulded and formatted to present our key findings in a very interactive manner.


# Key Findings From Analysis

- Orders are highest on weekends, Friday/saturday evenings.
  
- There are maximum Orders from 12 - 01 pm to 4 - 7pm.

- classic category contributes to the maximum sales and total orders.

- Large size pizza contributes to the maximum sales

- Classic Delux and Chicken pizza are the top sellers and revenue generators.

- The Brie carre pizza is the bottom in both orders and revenue. 

---

# `Act`

## Recommendations

- Optimize Staffing and Inventory: Given the peak times, ensure adequate staffing and inventory during these hours to handle the high volume of orders and maintain customer satisfaction.

- Promote Popular Items: Leverage the popularity of the Classic category and Large size pizzas in marketing campaigns to attract more customers.

- Bundle Offers: Consider creating bundle offers that include the top-selling pizzas to increase average order value.

- Re-evaluate Underperforming Product: For the Brie Carre pizza, consider revising its recipe or marketing strategy, or replacing it with a new product if it continues to underperform.

- Loyalty Program: Implement a loyalty program to encourage repeat orders, especially targeting customers who order during peak times and prefer the popular items.
  

---

 

