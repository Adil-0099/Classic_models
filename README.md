# 🧠 Classic Models SQL Analysis - Business Insights from SQL Queries

## 📌 Objective
This project uses advanced SQL queries on the Classic Models database to uncover valuable business insights related to customer behavior, revenue generation, employee performance, and product management.

---

## 📂 Dataset
- Classic Models (Sample Database)

---

## 🛠️ Tools Used
- SQL (MySQL or PostgreSQL)
- PowerPoint (for visual explanation)
- GitHub (project hosting)

---

## 🔍 Key SQL Queries and Their Business Impact

### 1️⃣ Customer Orders with Product Details
```sql
SELECT c.customername, c.phone, o.customernumber, p.productname, p.productline, 
       p.productdescription, p.quantityinstock, o.orderdate 
FROM products p
INNER JOIN orderdetails od ON p.productcode = od.productcode
INNER JOIN orders o ON o.ordernumber = od.ordernumber
INNER JOIN customers c ON c.customernumber = o.customernumber
ORDER BY p.quantityinstock;
```
**Insight**: Helps identify demand trends and manage inventory effectively. Also supports contacting customers with delivery or promotional updates.

---

### 2️⃣ Customers with More Than 5 Orders
```sql
SELECT c.customername, c.customernumber, COUNT(o.ordernumber) AS total_orders
FROM customers c
INNER JOIN orders o ON c.customernumber = o.customernumber
GROUP BY c.customername, c.customernumber
HAVING COUNT(o.ordernumber) > 5;
```
**Insight**: Identifies loyal customers to target with loyalty programs and premium services.

---

### 3️⃣ Total Revenue by Product Line
```sql
SELECT p.productline, SUM(od.quantityordered * od.priceeach) AS total_revenue
FROM productlines pl
INNER JOIN products p ON pl.productline = p.productline
INNER JOIN orderdetails od ON od.productcode = p.productcode
GROUP BY p.productline
ORDER BY total_revenue DESC;
```
**Insight**: Reveals which product lines generate the most revenue (e.g., Classic Cars), helping optimize marketing and stocking.

---

### 4️⃣ View: Total Quantity & Revenue per Product
```sql
CREATE VIEW view_revenue AS
SELECT p.productname, SUM(od.quantityordered) AS total_quantity,
       SUM(od.quantityordered * od.priceeach) AS total_revenue
FROM productlines pl
INNER JOIN products p ON pl.productline = p.productline
INNER JOIN orderdetails od ON od.productcode = p.productcode
GROUP BY p.productname;

SELECT * FROM view_revenue ORDER BY total_revenue DESC;
```
**Insight**: Allows company departments to access revenue data per product, enabling discount strategies on low-performing items.

---

### 5️⃣ Temporary Table: High-Value Customers
```sql
CREATE TEMPORARY TABLE temp_high_value_customer AS
SELECT c.customername, SUM(p.amount) AS total_amount
FROM customers c
INNER JOIN payments p ON c.customernumber = p.customernumber
GROUP BY c.customername;

SELECT * FROM temp_high_value_customer ORDER BY total_amount DESC;
```
**Insight**: Identifies financially significant customers to prioritize service, build trust, and manage stock clearance.

---

### 6️⃣ Top 5 Customers by Order Value
```sql
SELECT c.customername, SUM(od.quantityordered * od.priceeach) AS total_orders_revenue
FROM customers c
INNER JOIN orders o ON c.customernumber = o.customernumber
INNER JOIN orderdetails od ON od.ordernumber = o.ordernumber
GROUP BY c.customername
ORDER BY total_orders_revenue DESC
LIMIT 5;
```
**Insight**: Pinpoints top 5 customers for loyalty programs, exclusive offers, and relationship building.

---

### 7️⃣ Employees Who Haven’t Made Sales
```sql
SELECT CONCAT(e.firstname, " ", e.lastname) AS employee_name, e.employeenumber, o.ordernumber
FROM employees e
LEFT JOIN customers c ON e.employeenumber = c.salesRepEmployeeNumber
LEFT JOIN orders o ON o.customernumber = c.customernumber
WHERE c.customernumber IS NULL;
```
**Insight**: Helps monitor sales team effectiveness, identify training needs, or reassign underperforming employees.

---

## 📈 Business Impact Summary
- 🎯 Customer Segmentation for loyalty programs.
- 💰 Revenue Optimization by product line.
- 🧍‍♂️ Employee Performance analysis.
- 🧾 Sales Efficiency improvements.
- 📦 Inventory Management based on demand patterns.

---

## ✅ Conclusion
This SQL-based analysis provides powerful, actionable insights to improve operations, customer engagement, and revenue strategies at Classic Models. Each query not only answers a business question but also directly supports decision-making and growth.

---

> 💡 *This project demonstrates how SQL can be used not just for querying, but as a foundation for real-world business analysis and strategy.*
