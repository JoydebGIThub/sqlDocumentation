# SQL Querys
## PIVOT
The PIVOT clause in Oracle SQL is a powerful feature introduced in Oracle 11g that allows you to transform rows into columns, essentially creating a cross-tabulation of your data. This can make your data easier to read, summarize, and analyze.
| Product | Region | SalesAmount |
|---------|--------|-------------|
| Laptop  | East   | 3000        |
| Tablet  | West   | 1500        |
| Laptop  | West   | 2000        |
| Phone   | East   | 1000        |
| Tablet  | East   | 1200        |
| Phone   | West   | 1800        |

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
| Product | East_Sales | West_Sales |
|---------|------------|------------|
| Laptop  | 3000       | 2000       |
| Phone   | 1000       | 1800       |
| Tablet  | 1200       | 1500       |

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
| Product | East_Sales | West_Sales |
|---------|------------|------------|
| Laptop  | 3000       | 2000       |
| Monitor | 0          | 0          |
| Phone   | 1000       | 1800       |
| Tablet  | 1200       | 1500       |
***
***
## LISTAGG
Ah, "LISTAGG!" It's a fantastic Oracle SQL function for aggregating strings from multiple rows into a single string, often separated by a specified delimiter. Think of it as the opposite of splitting a string; it combines them. This is incredibly useful for generating comma-separated lists, creating concatenated values for reporting, or preparing data for other applications.
### Core Functionality:
The LISTAGG function takes two main arguments:
1. measure_expr: This is the column or expression whose values you want to concatenate. It must resolve to a string data type (or a data type that can be implicitly converted to a string).
2. delimiter: This is the string that will separate the concatenated values. It's a single-quoted string.

| department_id | employee_name |
|---------------|----------------|
| 10            | John Doe       |
| 20            | Jane Smith     |
| 10            | Peter Jones    |
| 20            | Alice Brown    |
| 10            | Bob Williams   |
#### We want to get a list of employee names for each department, separated by commas.
```sql
SELECT
    department_id,
    LISTAGG(employee_name, ', ') WITHIN GROUP (ORDER BY employee_name) AS employee_list
FROM
    employees
GROUP BY
    department_id;
```
#### Output:
| department_id | employee_list                          |
|---------------|----------------------------------------|
| 10            | Bob Williams, John Doe, Peter Jones    |
| 20            | Alice Brown, Jane Smith                |

### Concatenating Products Ordered by Customer

| customer_id | order_id | product_name |
|-------------|----------|--------------|
| 1           | 101      | Laptop       |
| 2           | 102      | Tablet       |
| 1           | 103      | Mouse        |
| 2           | 104      | Keyboard     |
| 1           | 105      | Monitor      |

#### We want to get a list of products ordered by each customer.
```sql
SELECT
    customer_id,
    LISTAGG(product_name, '; ') WITHIN GROUP (ORDER BY order_id) AS ordered_products
FROM
    orders
GROUP BY
    customer_id;
```
#### Output:
| customer_id | ordered_products         |
|-------------|--------------------------|
| 1           | Laptop; Mouse; Monitor   |
| 2           | Tablet; Keyboard         |







