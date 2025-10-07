# 🏬 Superstores Orders Data Analysis  
**Author:** Tushar Kanojiya  
📧 **Contact:** [tkanojiya83@gmail.com](mailto:tkanojiya83@gmail.com)

---

## 📘 Table of Contents

1. [Project Overview](#project-overview)  
2. [Data Source](#data-source)  
3. [SQL Workflow](#sql-workflow)  
4. [SQL Queries (Basic, Intermediate, Advanced)](#sql-queries-basic-intermediate-advanced)  
5. [Power BI Workflow (Step-by-Step)](#power-bi-workflow-step-by-step)  
6. [Key Insights](#key-insights)  
7. [How to Run](#how-to-run)  
8. [Folder Structure](#folder-structure)  
9. [Future Enhancements](#future-enhancements)  
10. [Contact](#contact)

---

## 🧾 Project Overview

This project involves analyzing a **Superstores orders dataset** (sales, profit, discounts, regions, products) using **SQL** for data processing and **Power BI** for visualization.  
The aim is to derive insights such as top selling categories, regional performance, profit margins, and trends over time.

---

## 📂 Data Source

The dataset includes (but is not limited to):

- Order ID, Order Date, Ship Date  
- Product ID, Product Name, Category, Sub-Category  
- Sales amount, Quantity, Discount, Profit  
- Customer ID, Customer Segment  
- Region, State, City, Ship Mode  

The data is provided in CSV/Excel format and loaded into a SQL database for further processing.

---

## 🧠 SQL Workflow

### 🪜 Steps Performed in SQL

1. **Data Import**  
   Import the raw CSV data into staging tables in SQL (e.g. using `LOAD DATA`, `BULK INSERT`, or SQL import wizard).

2. **Data Cleaning**  
   - Remove duplicates, handle NULL values  
   - Standardize text (trim spaces, uniform casing)  
   - Convert date strings to proper `DATE` / `DATETIME` types

3. **Data Transformation**  
   - Derive new columns (e.g. `Order_Month`, `Order_Year`, `Profit_Margin = Profit / Sales`)  
   - Use `CASE WHEN` transformations (for categorization)  
   - Join lookup tables if any (e.g. product / category metadata)

4. **Data Analysis**  
   - Use aggregations, window functions, ranking, and conditional logic to produce insights  
   - Prepare summary tables/views to feed into Power BI

---

## 🧩 SQL Queries (Basic, Intermediate, Advanced)

### 🟢 Basic Queries

| Purpose | SQL Example | Concepts / Functions | Description |
|---|---|---|---|
| Total Sales & Profit | ```sql SELECT SUM(Sales) AS Total_Sales, SUM(Profit) AS Total_Profit FROM orders; ``` | `SUM()` | Get overall sales and profit figures |
| Count Orders By Region | ```sql SELECT Region, COUNT(*) AS Num_Orders FROM orders GROUP BY Region; ``` | `COUNT()`, `GROUP BY` | Shows how many orders each region placed |
| Average Discount | ```sql SELECT AVG(Discount) AS Avg_Discount FROM orders; ``` | `AVG()` | Computes average discount across all orders |

### 🟡 Intermediate Queries

| Purpose | SQL Example | Concepts / Functions | Description |
|---|---|---|---|
| Sales by Category | ```sql SELECT Category, SUM(Sales) AS Sales_By_Category FROM orders GROUP BY Category ORDER BY Sales_By_Category DESC; ``` | `GROUP BY`, `ORDER BY` | Which product category brings highest revenue |
| Monthly Sales Trend | ```sql SELECT YEAR(Order_Date) AS Yr, MONTH(Order_Date) AS Mo, SUM(Sales) AS Monthly_Sales FROM orders GROUP BY YEAR(Order_Date), MONTH(Order_Date) ORDER BY Yr, Mo; ``` | `YEAR()`, `MONTH()`, `GROUP BY` | Shows how sales change month to month |
| Profit by State | ```sql SELECT State, SUM(Profit) AS State_Profit FROM orders GROUP BY State ORDER BY State_Profit DESC; ``` | `SUM()`, `ORDER BY` | Which states contribute most profit |

### 🔴 Advanced Queries

| Purpose | SQL Example | Concepts / Functions | Description |
|---|---|---|---|
| Top 5 Customers by Profit | ```sql SELECT Customer_ID, SUM(Profit) AS Total_Profit FROM orders GROUP BY Customer_ID ORDER BY Total_Profit DESC LIMIT 5; ``` | `LIMIT`, `ORDER BY` | Finds top profit-generating customers |
| Product Sales Ranking | ```sql SELECT Product_Name, RANK() OVER (ORDER BY SUM(Sales) DESC) AS Sales_Rank, SUM(Sales) AS Total_Sales FROM orders GROUP BY Product_Name; ``` | `RANK()`, `OVER()` | Rank products by total sales |
| Category Profit Margin | ```sql SELECT Category, (SUM(Profit) * 1.0 / SUM(Sales)) * 100 AS Profit_Margin_Percent FROM orders GROUP BY Category ORDER BY Profit_Margin_Percent DESC; ``` | `SUM()`, division, `GROUP BY` | Calculates profit margin percentage per category |
| Monthly YoY Growth | ```sql WITH monthly AS ( SELECT YEAR(Order_Date) AS Yr, MONTH(Order_Date) AS Mo, SUM(Sales) AS SalesAmt FROM orders GROUP BY YEAR(Order_Date), MONTH(Order_Date) ) SELECT m1.Yr, m1.Mo, m1.SalesAmt, m2.SalesAmt AS PrevYearSales, (m1.SalesAmt - m2.SalesAmt) / m2.SalesAmt * 100 AS YoY_Growth FROM monthly m1 JOIN monthly m2 ON m1.Mo = m2.Mo AND m1.Yr = m2.Yr + 1; ``` | CTEs, `JOIN`, arithmetic operations | Compare monthly sales growth year over year |

---

## 📊 Power BI Workflow (Step-by-Step)

1. **Data Connection**  
   - Connect Power BI to your SQL database (or import cleaned tables).  
   - Load tables such as `orders`, `products`, `customers`, `regions`.

2. **Power Query / Data Cleaning**  
   - Cleanse data: remove nulls, duplicates, correct data types  
   - Create calculated columns (e.g. `Order Month`, `Year`, `Profit Margin`)  
   - Filter or trim unnecessary columns

3. **Data Modeling**  
   - Define relationships: e.g. `orders → products`, `orders → customers`, `orders → regions`  
   - Use star schema for performance and clarity

4. **DAX Measures**  
   - **Total Sales** = `SUM(orders[Sales])`  
   - **Total Profit** = `SUM(orders[Profit])`  
   - **Profit Margin %** = `DIVIDE([Total Profit], [Total Sales]) * 100`  
   - **Average Discount** = `AVERAGE(orders[Discount])`  
   - **Top Product Sales** using `RANKX()` measure  

5. **Visualization & Report Design**  
   - **KPI Cards**: total sales, profit, margin  
   - **Bar / Column Charts**: sales by category / state / product  
   - **Line Charts**: sales / profit trends over time  
   - **Map Visuals**: show geographic performance  
   - **Slicers / Filters**: by year, region, category  

6. **Formatting & Interactivity**  
   - Apply consistent color themes, data labels, tooltips  
   - Add bookmarks or navigation between pages  
   - Enable tooltips to show additional metrics

7. **Publish / Share**  
   - Publish to Power BI Service  
   - Set up refresh schedules if connecting to live data  
   - Share dashboards or embed in web portals

---

## 📈 Key Insights (Potential / Sample)

- **Top Categories & Products:** Identify which product categories or items drive the highest sales and profit.  
- **Regional Performance:** See which states or regions outperform or underperform.  
- **Profit Margin Analysis:** Some categories may have high sales but low margins — useful to optimize.  
- **Seasonal Trends:** Detect months or quarters where sales spike or dip.  
- **Customer Value:** A small subset of customers may be generating a large portion of profit.  
- **Growth Patterns:** Year-over-year or month-over-month growth metrics to assess trends.

---

## 🧰 How to Run

1. Clone the repository:  
   ```bash
   git clone https://github.com/Tushar-kanojiya-4002/Superstores-orders-data.git
