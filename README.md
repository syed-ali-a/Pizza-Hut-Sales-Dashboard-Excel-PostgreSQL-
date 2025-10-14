# Pizza Hut Sales Dashboard (Excel + PostgreSQL)

## Project Overview
This project analyzes **Pizza Hut’s sales performance** using data stored in **PostgreSQL** and visualized in **Microsoft Excel**.  
The dashboard provides insights into **sales trends, product performance, and customer ordering patterns** through interactive visuals and KPIs.

---

## Tech Stack
- Database:PostgreSQL  
- Visualization Tool: Microsoft Excel
- Data Cleaning : Power Query Editor
- Data Connection: Excel Power Query (connected to PostgreSQL)  
- Languages/Queries:SQL (PostgreSQL dialect)

---

##  Database Schema

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
Key Performance Indicators (KPIs)
KPI	Description	SQL Query
Total Revenue	Total revenue generated from all pizza sales	SELECT SUM(total_price) FROM pizza_sales;
Average Order Value	Average sales value per order	SELECT SUM(total_price) / COUNT(DISTINCT(order_id)) FROM pizza_sales;
Total Pizzas Sold	Total quantity of pizzas sold	SELECT SUM(quantity) FROM pizza_sales;
Total Orders	Total number of unique orders	SELECT COUNT(DISTINCT(order_id)) FROM pizza_sales;
Average Pizzas Per Order	Average number of pizzas per order	SELECT ROUND(SUM(quantity)*1.0/COUNT(DISTINCT(order_id)),2) FROM pizza_sales;

Trend Analysis
Daily Trend for Total Orders
sql
Copy code
SELECT 
  TO_CHAR(order_date, 'Day') AS day,
  COUNT(DISTINCT(order_id)) AS total_orders
FROM pizza_sales
GROUP BY day
ORDER BY total_orders DESC;
Hourly Trend for Orders
sql
Copy code
SELECT 
  EXTRACT(HOUR FROM order_time) AS hour,
  COUNT(DISTINCT(order_id)) AS total_orders
FROM pizza_sales
GROUP BY hour
ORDER BY total_orders ASC;
Category and Size Insights
% of Sales by Pizza Category
sql
Copy code
SELECT 
  pizza_category,
  ROUND(SUM(total_price), 2) AS total_revenue,
  ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS pct
FROM pizza_sales
GROUP BY pizza_category;
% of Sales by Pizza Size
sql
Copy code
SELECT 
  pizza_size,
  ROUND(SUM(total_price), 2) AS total_revenue,
  ROUND(SUM(total_price) * 100.0 / (SELECT SUM(total_price) FROM pizza_sales), 2) AS pct
FROM pizza_sales
GROUP BY pizza_size;
Best & Worst Performers
Top 5 Best Sellers
sql
Copy code
SELECT 
  pizza_name,
  SUM(quantity) AS total_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_sold DESC
LIMIT 5;
Bottom 5 Sellers
sql
Copy code
SELECT 
  pizza_name,
  SUM(quantity) AS total_sold
FROM pizza_sales
GROUP BY pizza_name
ORDER BY total_sold ASC
LIMIT 5;
Dashboard Insights (Excel Visualization)
🔹 Busiest Days & Times
Peak sales occur on Friday and Saturday evenings.

Most orders are placed between 12 PM to 1 PM.

🔹 Sales by Category & Size
Classic category and Large size pizzas contribute the most to total revenue.

🔹 Best Sellers
Classic Deluxe and Chicken pizzas are the top performers.

🔹 Worst Seller
Brie Carre Pizza has the lowest sales and revenue.

Dashboard Preview

How to Use
Import the dataset into PostgreSQL.

Run the provided SQL queries to verify the KPIs.

Connect Excel to PostgreSQL using Power Query or ODBC Connection.

Load the query results and create Pivot Tables and Charts.

Use Excel Slicers for month-wise or category-based filtering.

Learnings & Skills Demonstrated
SQL for data extraction and analytics

Excel Power Query and Power Pivot

KPI dashboard creation

Data modeling and aggregation

Business insights and data storytelling
