# SQL Querys
## PIVOT
The PIVOT clause in Oracle SQL is a powerful feature introduced in Oracle 11g that allows you to transform rows into columns, essentially creating a cross-tabulation of your data. This can make your data easier to read, summarize, and analyze.
```csv
Product	Region	SalesAmount
Laptop	East	3000
Tablet	West	1500
Laptop	West	2000
Phone	East	1000
Tablet	East	1200
Phone	West	1800
```
### You want to pivot this data to see the total sales amount for each product in each region. Here's the SQL query:
```sql
SELECT *
FROM (
    SELECT Product, Region, SalesAmount
    FROM sales
)
PIVOT (
    SUM(SalesAmount)
    FOR Region IN ('East' AS East_Sales, 'West' AS West_Sales)
)
ORDER BY Product;
```
#### Output:
```csv
Product	East_Sales	West_Sales
Laptop	3000	      2000
Phone	  1000	      1800
Tablet	1200	      1500
```
- Handling Nulls: If there are no sales for a particular product in a region, the corresponding cell in the pivoted result will be NULL. You can use functions like NVL() to replace these nulls with a specific value (e.g., 0).
```sql
SELECT 
Product,
NVL(East_Sales, 0) AS East_Sales,
NVL(West_Sales, 0) AS West_Sales
FROM (
    SELECT Product, Region, SalesAmount
    FROM sales
)
PIVOT (
    SUM(SalesAmount)
    FOR Region IN (
      'East' AS East_Sales,
      'West' AS West_Sales
    )
)
ORDER BY Product;
```
#### Output:
```csv
Product	East_Sales	West_Sales
Laptop	3000	      2000
Monitor	0	          0
Phone	  1000	      1800
Tablet	1200	      1500
```
