---
layout: post
title: PostgreSQL
date: 2023-05-4
categories: [Backend, PostgreSQL,Sql]
---

## SQL and Creating a Postgres Database

### what is SQL
 SQL is a language or syntax that allows us to interact with `SQL databases`. It is not a database itself, but rather a way of manipulating and querying data within a database. SQL allows us to perform operations such as `selecting`, `inserting`, `updating`, and `deleting` data, as well as `creating` and `modifying database structures such as tables, indexes, and views`. SQL is an important tool for anyone working with data, as it provides a standardized way of working with databases regardless of the specific database management system being used.


### Creating a database : 
To create a new database in PostgreSQL you need first to open psql  the PostgreSQL command line interface 

```sql
psql -U your_username
```
Once you are connected to a database, you can start entering SQL commands to interact with your data.

we can create our database with this command : 
```sql
CREATE DATABASE my_database;
```
### Connect to database:
After creating a database, you should connect with them to perform databases operations 
```sql
\c my_database
```

### Creating tables: 
After creating a database, you can create tables to store data. A table consists of columns and rows. Here's an example of how to create a table:
```sql
CREATE TABLE table_name (
    column1 datatype1,
    column2 datatype2,
    column3 datatype3,
    ...
);
```
Here's an example of creating a simple table and a brief explanation of some common data types:

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    age INTEGER,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10, 2),
    is_manager BOOLEAN
);
```
This creates a table called "employees" with six columns:

- `"id"` is a `SERIAL` column that serves as the `primary key` for the table. SERIAL is a shorthand way of specifying an auto-incrementing integer column.
- `"name"` is a `VARCHAR` column that can store up to 100 characters and cannot be NULL.
- `"age"` is an `INTEGER` column that can store whole numbers between -2147483648 and 2147483647.
- `"email"` is also a `VARCHAR` column that can store up to 100 characters, must be unique across all rows, and can be NULL.
- `"salary"` is a `DECIMAL` column that can store numbers with up to 10 digits total, 2 of which are to the right of the decimal point. This is commonly used for storing monetary values.
- "is_manager" is a `BOOLEAN` column that can store true or false values.

Here are some other common data types you might encounter in PostgreSQL:

- `TEXT`: Similar to VARCHAR, but can store larger amounts of text (up to 1 GB).
- `DATE`: Stores a date without a time component.
- `TIMESTAMP`: Stores a date and time.
- `TIME`: Stores a time of day without a date component.
- `ARRAY`: Stores an array (or list) of values of a particular data type. For example, you could have an array of integers, an array of strings, etc.
- `JSON`: Stores a JSON object or array.

### Some Common psql commands

- List all databases:
```sql
\l
```
- List all tables in the current database:
```sql
\dt
```
- Exit the PostgreSQL shell:
```sql
\q
```

### CRUD Operations 

CRUD stands for Create, Read, Update, and Delete, and these operations are the basic functions that can be performed on data in a database.

- `Create`: In SQL, you can create new records (also called rows or tuples) in a table using the INSERT statement. For example:
```sql
INSERT INTO employees (name, age, email, salary, is_manager)
VALUES ('John Doe', 35, 'john.doe@example.com', 50000.00, false);
```
- `Read`: To retrieve data from a table, you can use the SELECT statement. For example:

```sql
SELECT * FROM employees;
```
This will return all the rows and columns from the employees table. You can also specify which columns to return, and filter the results using conditions. For example:
```sql
SELECT name, age, email FROM employees
WHERE age > 30 AND is_manager = true;
```
This will return only the name, age, and email columns for employees who are over 30 years old and are managers.

- `Update`: To modify existing records in a table, you can use the UPDATE statement. For example:

```sql
UPDATE employees
SET salary = 55000.00, is_manager = true
WHERE name = 'John Doe';
```
This will update the salary and is_manager columns for the employee with the name 'John Doe'.

- `Delete`: To remove records from a table, you can use the DELETE statement. For example:

```sql
DELETE FROM employees
WHERE age > 60;
```
This will delete all employees who are over 60 years old.

### SQL Filters 

SQL filters are used to extract specific data from a database based on certain conditions or criteria.

Here are some examples of SQL filters:

1. Select all employees who are managers:

```sql
SELECT * FROM employees
WHERE is_manager = true;
```

2. Select all employees who are not managers and earn a salary greater than $50,000:

```sql
SELECT * FROM employees
WHERE is_manager = false AND salary > 50000;
```

3. Select all employees whose names start with the letter 'A':

```sql 
SELECT * FROM employees
WHERE name LIKE 'A%';
```
4. Select the top 5 highest earning employees:

```sql 
SELECT * FROM employees
ORDER BY salary DESC
LIMIT 5;
```
### Relating Tables with Foreign Keys

Relating tables with foreign keys is a technique used in relational database design to establish a relationship between two tables. `A foreign key is a column or a set of columns in one table that refers to the primary key of another table.` This establishes a link between the two tables, allowing data to be shared between them.

For example, consider two tables: `orders` and `customers`. The `orders` table contains information about customer orders, including the customer who placed the order. The customers table contains information about the customers themselves, such as their names, addresses, and phone numbers. To relate these two tables, we can create a foreign key on the orders table that references the primary key of the customers table:
```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    address VARCHAR(100),
    phone VARCHAR(20)
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers (id),
    order_date DATE NOT NULL,
    total DECIMAL(10, 2) NOT NULL
);
```

In this example, the `customer_id` column in the `orders` table is a foreign key that references the `id` column in the `customers` table. This creates a relationship between the two tables: each order is associated with a single customer.

When inserting data into the `orders` table, we must ensure that the `customer_id` value exists in the customers table. This prevents us from creating orders for non-existent customers. We can enforce this constraint using the REFERENCES keyword in the CREATE TABLE statement.

Using foreign keys, we can easily join the orders and customers tables to retrieve information about customers and their orders. For example, we can use the following query to retrieve the names of all customers who have placed orders:
```sql
SELECT DISTINCT customers.name
FROM customers
JOIN orders ON customers.id = orders.customer_id;
```


This SQL query retrieves the names of all customers who have placed orders by joining the customers and orders tables using a foreign key. The JOIN statement links the two tables together based on the customer_id column in the orders table and the id column in the customers table. The DISTINCT keyword ensures that each customer name is only displayed once in the result set.
