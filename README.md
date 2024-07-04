# Northwind Database Exploratory Analysis

## Introduction
This project involves an analysis of the Northwind database using PostgreSQL, focusing on SQL fundamentals, aggregation functions, and exploratory data analysis. The Northwind database is a sample schema representing a small business ERP system, including customers, orders, inventory, purchasing, suppliers, shipping, employees, and accounting.

## Database Overview
The Northwind database includes 12 tables, each serving different aspects of the business. Key tables include:

- Suppliers: Information about suppliers and vendors.
- Customers: Details of customers.
- Employees: Employee information.
- Products: Product details.
- Shippers: Information about shippers.
- Orders and Order_Details: Sales order transactions.
- Categories: Product categories.
- Employee_Territories: Mapping between employees and territories.
- Territories: Territory descriptions.
- Region: Region descriptions.
- US_States: Details of US states.

You can obtain the Northwind database for PostgreSQL from this GitHub repository:
https://github.com/pthom/northwind_psql

## Entity-Relationship Diagram

![ER Diagram - Northwind Database](https://github.com/shinilkumars/Northwind-DB-SQL-Exploratory-Analysis/assets/173347067/4e0c036f-28fe-4855-b779-d5687141ed6c)

## Exploratory Data Analysis (EDA)

### 1. Products for Discount (discount offered for products with unit price >=10)
```sql
SELECT product_id
FROM PRODUCTS
WHERE unit_price >= 10;
```
66 products are eligible for discounts (price >= $10).

### 2. **Categories with More Than 500 Units in Stock**
```sql
SELECT category_id, SUM(units_in_stock) AS total_units_in_stock
FROM PRODUCTS
GROUP BY category_id
HAVING SUM(units_in_stock) > 500
ORDER BY category_id;
```
Category IDs 1, 2, and 8 have more than 500 units in stock.

### 3. Order Count per Customer
```sql
SELECT customer_id, COUNT(order_id) AS count_of_orders
FROM ORDERS
GROUP BY customer_id
ORDER BY count_of_orders DESC;
```
Top 3 customers by order count:
- SAVEA (31 orders)
- ERNSH (30 orders)
- QUICK (28 orders)

### 4. Product Pricing Analysis
```sql
SELECT AVG(unit_price) as avg_price, MIN(unit_price) as min_price, MAX(unit_price) as max_price 
FROM products;
```
- Average price: $28.83
- Minimum price: $2.50
- Maximum price: $263.50

### 5. Order Trends Over Time
```sql
SELECT EXTRACT(YEAR FROM order_date) as year, EXTRACT(MONTH FROM order_date) as month, COUNT(*) as order_count
FROM orders
GROUP BY year, month
ORDER BY year, month;
```
Order counts show an increasing trend from 1996 to 1998, with the highest count in March and April 1998 (73 and 74 orders respectively).

### 6. Customer Segmentation
```sql
SELECT customer_id, COUNT(*) as order_count, SUM(freight) as total_freight
FROM orders
GROUP BY customer_id
ORDER BY order_count DESC
LIMIT 5;
```
Top 5 customers by order count and total freight:
- SAVEA (31 orders, $6683.70 freight)
- ERNSH (30 orders, $6205.39 freight)
- QUICK (28 orders, $5605.63 freight)
- HUNGO (19 orders, $2755.24 freight)
- FOLKO (19 orders, $1678.08 freight)

### 7. Top-Selling Products
```sql
SELECT p.product_name, p.category_id, SUM(od.quantity) as total_quantity_sold
FROM order_details od
JOIN products p ON od.product_id = p.product_id
GROUP BY p.product_id, p.product_name, p.category_id
ORDER BY total_quantity_sold DESC
LIMIT 5;
```
Top 5 best-selling products:
- Camembert Pierrot (Category 4, 1577 units)
- Raclette Courdavault (Category 4, 1496 units)
- Gorgonzola Telino (Category 4, 1397 units)
- Gnocchi di nonna Alice (Category 5, 1263 units)
- Pavlova (Category 3, 1158 units)

### 8. Geographic Sales Distribution
```sql
SELECT ship_country, COUNT(*) as order_count
FROM orders
GROUP BY ship_country
ORDER BY order_count DESC
LIMIT 5;
```
Top 5 countries by order count:
- Germany (122 orders)
- USA (122 orders)
- Brazil (83 orders)
- France (77 orders)
- UK (56 orders)

### 9. Employee Sales Performance
```sql
SELECT e.employee_id, e.first_name, e.last_name, COUNT(o.order_id) as order_count
FROM employees e
LEFT JOIN orders o ON e.employee_id = o.employee_id
GROUP BY e.employee_id, e.first_name, e.last_name
ORDER BY order_count DESC
LIMIT 5;
```
Top 5 employees by order count:
- Margaret Peacock (156 orders)
- Janet Leverling (127 orders)
- Nancy Davolio (123 orders)
- Laura Callahan (104 orders)
- Andrew Fuller (96 orders)  

## Tool Used
PostgreSQL

## Key Learning

- SQL fundamentals (SELECT, WHERE, GROUP BY, HAVING, ORDER BY)
- Data aggregation and analysis
- Joining tables for complex queries
- PostgreSQL-specific features
- Exploratory data analysis techniques
- Database Schema Analysis for interpreting multi-table relationships in a business context.
- Deriving business insights from data

## Conclusion
This project provides valuable insights into the Northwind database, revealing patterns in product sales, customer behavior, employee performance, and geographical distribution of orders. The analysis demonstrates the power of SQL in extracting meaningful business intelligence from relational databases.
