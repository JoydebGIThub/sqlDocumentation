# What is Clustered and Non-Clustered Index in SQL?
In SQL, "**indexes**" are used to speed up the retrieval of rows from a database table. They help in improving the performance of queries by allowing the database engine to quickly locate the required rows without scanning the entire table. There are two primary types of indexes in SQL: "**Clustered Index and Non-Clustered Index**".
- A **database index** is a data structure that improves the speed of data retrieval operations on a table at the cost of **additional space** and **slower writes** (inserts, updates, deletes).
```sql
CREATE INDEX idx_employees_lastname ON employees (last_name);
```
- Composite Index (multiple columns)
```sql
CREATE INDEX idx_orders_cust_date ON orders (customer_id, order_date);
```

