## What is the difference between DBMS and RDBMS?
DBMS (Database Management System):- As the name only suggest it is the basic way to store data in files. Data will stored here in individual file with no connection between them.
RBDMS (Relational Database Management System):- Have some sort of Relation Between the Data. Example: MySQL

  Pros: 
  - 
  - Easy to create relationship between different pieces of data.
  - This allows faster data retrival
  - When you're dealing with large dataset
  - Support multiple users
  - high security
  - optimized for large amounts of data

## What is primary key and foreign key?
Primary Key: It's a column or set of column that can uniquely identify records in a table also the uniquely identifiable data can't be empty or null. I table can have one primary column
Foreign key: Helps us to connect data across tables, ensuring that records in one table can reference related information in another. A table can have multiple foreign key column and the data of that column can be duplicate or null. The foreign key of some table can be the primary key of some table

## What are constraints and their types?
Constraints: SQL is like rules the set up for Data in the tables. They help keep the data accurate and relible.
- Not Null: A column cannot be empty.
- Unique: No 2 value in a coulmn can be same.
- Primary key: Combination of Not null and unique.
- Foreign key: It connect two table
- Check: Helps to set a condition (Age> 18)
- default: Take a default value when no value is mentained.(Default value India)

## Explain DDL and DML commands in SQL
DDL: Data Defination Language
- Define the structure of the database by **creating, altering, droping, truncate, rename**.
- Don't deal with the actual data but just with the database's structure.
- Setting up and organizing the structure

DML: Data Manipulation Language
- Working with the actual data by **insert, update, delete**.
- Keep the data into the table upto date.
- Manage everything inside the table the data itself.

## What is the Difference Between Delete, Drop, and Truncate Statements?
Delete: 
- Removing a specific data from a table based on certain **conditions**.
- It let me choose what I want to delete from the table.
- We can roll back or undo the deletion in most cases.

Truncate:
- It clears out all the data from the table and just leave the empty table.
- It's fast but doesn't allow to roll back.

Drop:
- Remove the table itself from the database.
- Drop is permanent and can't be undone.

## Differentiate Between Group By and Order by clause.
Group By:
- Puting same satisfied conditional data in one 🪣bucket.
- It is tipically used with **aggregate function** like **Sum, Avg, Count**.
- It can get a summarize view or each satisfied condition.
- Grouping the data in a meaningful summary

Order By:
- Sorting rows in a particular order
- We sorting the entire table based on one or more columns.
- Arrange data in a order for view.
```sql
SELECT first_name, age FROM Customers order by age desc;

select item, avg(amount) from orders group by item;
```

## Explain the difference between where and having clauses
Where:
- Filtering before grouping
- Filter individual rows based on specific conditions like age or name

Having:
- filtering after grouping
- Use having when we want to filter groups created by the Group By clause based on Aggregate Results, Like counts and sums.
```sql
SELECT first_name, age
FROM Customers where age >=28;
select item, avg(amount) from orders group by item having avg(amount)> 300;
```

## What are aggregate functions in SQL?
Aggregate functions are like tools that help us perform some task on data like:
- Count():
  - It counts the number of rows or non-null values in a column
  - Count(*) gives us the total number or records.
```sql
select count(*) from orders;
```
- Sum():
  - It Adds up all the values in a Numeric column
  - Sum(column_name) gives as the total value of that column
```sql
select sum(amount) from orders;
```
- Avg()
  - Calculate the avarage of a numeric column
  - Avg(column_name) sum of all the data in a column divided by total number of rows.
```sql
select avg(amount) from orders;
```
- Min()
  - Finds the minimum values in a numeric column
  - Min(column_name) gives as the lowest value of a column
```sql
select min(amount) from orders;
```
- Max()
  - Finds the maximum values in a numeric column
  - Min(column_name) gives as the highest value of a column
```sql
select max(amount) from orders;
```

## Explain Indexing in SQL and What do you mean by clustered index?
**Indexing** in SQL is like creating an index in a book. It's a data structure that improves the speed of data retrieval operations on a database table. Instead of scanning the entire table, the database can use the index to quickly locate the rows containing the desired data. Think of it as a shortcut.
**Example**: Imagine a table of customer information with columns like CustomerID, Name, and City. Without an index, if you want to find all customers in 'Mumbai', the database would have to examine every single row. However, if you create an index on the City column, the database can quickly jump to the relevant entries.
#### Creating a Non-Clustered Index:
To create a non-clustered index on the City column, we would use the CREATE INDEX statement:
```sql
CREATE INDEX IX_City ON Customers (City);
```
**This creates a separate structure that contains the City values and pointers to the actual rows in the Customers table. This helps in quickly locating rows based on the city.**
#### Querying based on the Non-Clustered Index Column (City):
```sql
SELECT Name, CustomerID
FROM Customers
WHERE City = 'Mumbai';
```
- ***How the Non-Clustered Index might be used**: The database can use the IX_City index. This index contains a sorted list of City values, with pointers to the actual data rows (which are physically ordered by CustomerID in this case).

A **clustered index** is a special type of index that determines the physical order of the data in a table. Think of it as the main organization system for the entire table. There can be only one clustered index per table because the data can only be physically sorted in one way. Typically, the primary key of a table is used to define the clustered index. The leaf nodes of a clustered index are the actual data rows of the table.
**Example**: In our customer table, if CustomerID is the primary key and we create a clustered index on it, the rows in the table will be physically stored on disk in ascending order of CustomerID. This makes retrieving rows based on CustomerID very fast.
#### Creating a Clustered Index:
When you define a primary key constraint on a table in many database systems (like SQL Server by default), a clustered index is automatically created on the primary key column(s)
```sql
CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(255),
    City VARCHAR(255)
);
```
**In this case, the PRIMARY KEY constraint on CustomerID will typically result in a clustered index being created on the CustomerID column. The physical storage of the Customers table will be ordered based on the values in the CustomerID column.**
If you wanted to explicitly create a clustered index (perhaps on a different column if no primary key was initially defined, although this is less common and has implications), you might use syntax like this (the exact syntax can vary slightly depending on the database system):
```sql
CREATE CLUSTERED INDEX IX_CustomerID ON Customers (CustomerID);
```
#### Querying based on the Clustered Index Column (CustomerID):
```sql
SELECT Name, City
FROM Customers
WHERE CustomerID = 123;
```
- **How the Clustered Index might be used**: Since there's a clustered index on CustomerID, the database can directly go to the physical location of the row(s) with CustomerID = 123. This is very efficient because the data is physically sorted by CustomerID. It's like directly finding a book in a library that's perfectly organized by its unique ID.

#### Querying on a column without an index:
```sql
SELECT Name
FROM Customers
WHERE Name LIKE 'A%';
```
- **How indexes are not used (likely)**: Since there's no index on the Name column, the database will likely have to perform a full table scan. It will go through each and every row in the Customers table and check if the Name starts with 'A'. This is less efficient, especially for large tables.


