# Pizza Hutt Sales Dashboard (Excel + PostgreSQL)

## Project Overview
This project analyzes **Pizza Hut’s sales performance** using data stored in **PostgreSQL** and visualized in **Microsoft Excel**.  
The dashboard provides insights into **sales trends, product performance, and customer ordering patterns** through interactive visuals and KPIs.

---

## Tech Stack
- Database: PostgreSQL
- Data Cleaning: Power Query Editor  
- Visualization Tool: Microsoft Excel  
- Data Connection: Excel Power Query (connected to PostgreSQL)  
- Languages/Queries: SQL (PostgreSQL dialect)

---

## Database Schema

### Table: `pizza_sales`
```sql
CREATE TABLE pizza_sales 
(
    pizza_id INT PRIMARY KEY,
    order_id INT, 
    pizza_name_id VARCHAR(30),
    quantity INT,
    order_date DATE,
    order_time TIME,
    unit_price NUMERIC(4,2),
    total_price NUMERIC(4,2),
    pizza_size VARCHAR(5),
    pizza_category VARCHAR(20),
    pizza_ingredients VARCHAR(100),
    pizza_name VARCHAR(50)
);


KPI REQUIREMENTS 

--1. Total Revenue:--

SELECT 
SUM(total_price) AS Total_Revenue
FROM PIZZA_SALES 

--2. Average Order Value:--

SELECT 
SUM(total_price) / COUNT(DISTINCT(order_id)) as Avg_order_value
FROM PIZZA_SALES 

--3. Total Pizzas Sold:--

SELECT 
SUM(quantity) AS Total_Pizzas_Sold
FROM PIZZA_SALES 

--4. Total Orders: --

SELECT 
COUNT(DISTINCT(order_id)) AS Total_Orders
FROM PIZZA_SALES 

--5.Average Pizzas Per Order:--

SELECT 
ROUND(SUM(quantity) *1.0 / COUNT(DISTINCT(order_id)),2)
FROM PIZZA_SALES 

--6.Daily Trend for Total Orders--

SELECT 
TO_CHAR(order_date,'Day ') AS Day,
COUNT(DISTINCT(order_id)) AS Total_orders
FROM PIZZA_SALES 
GROUP BY Day
ORDER BY Total_orders DESC

--7.Hourly Trend for Orders -- 

SELECT 
EXTRACT(HOUR FROM order_time) AS Hourly,
COUNT(DISTINCT(order_id)) AS Total_orders
FROM PIZZA_SALES
GROUP BY Hourly
ORDER BY Total_orders ASC

--8. % of Sales by Pizza Category--

SELECT 
  pizza_category,
  ROUND(SUM(total_price), 2) AS total_revenue,
  ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS pct
FROM pizza_sales
GROUP BY pizza_category

--9. % of Sales by Pizza Size --

SELECT 
	pizza_size,
	ROUND(SUM(total_price),2) AS  total_revenue,
	ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS pct
FROM pizza_sales
GROUP BY pizza_size

--10.Total Pizzas Sold by Pizza Category--

SELECT 
pizza_category,
SUM(quantity) AS Total_pizzas_Sold
FROM pizza_sales
GROUP BY pizza_category

--11.Top 5 Best Sellers by Total Pizzas Sold--

SELECT 
pizza_name,
SUM(quantity) AS Total_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Sold DESC LIMIT 5


--12.Bottom 5 Best Sellers by Total Pizzas Sold--

SELECT 
pizza_name,
SUM(quantity) AS Total_Sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY Total_Sold ASC LIMIT 5



