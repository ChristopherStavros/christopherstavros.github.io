---
layout: post
title:  "PostgreSQL Syntax - Cheatsheet"
date:   2019-01-23 12:42:30 -0501
categories: blog
author: Stavros
---

A quick guide to PostreSQL syntax.

# Resources

[Official Documentation](https://www.postgresql.org/docs/10/index.html)

# Some basics

### Quotes

- In Postgres, "double quotes" are used for names of tables and fields, while 'single quotes' are used for string constants

### Comments

```sql
/* Block comment */

-- Single line comment
```

# SQL - DML

## SELECT

Select all rows and columns from a table

```sql
SELECT * FROM customers
```

Select all rows and only specific columns from a table

```sql
SELECT id, last_name
FROM customers
```

## WHERE

Use the 'WHERE' clause to filter returned rows

```sql
SELECT id, last_name 
FROM customers
WHERE last_name='Adam' OR last_name='Watson'
```

## AS

Give a friendly name to a column or table

```sql
SELECT customers.id AS "custID"
FROM customers
```

## LIMIT

Limit the number of rows returned

```sql
SELECT customers.first_name, customers.last_name 
FROM customers 
LIMIT 1;
```

## UPDATE

Update specific values

```sql
UPDATE items
SET price = 4.00
WHERE id = 3;
```

## DELETE rows

Delete rows that meet specific criteria

```sql
DELETE FROM items WHERE id=4;
```

## LIKE

Use LIKE for more advanced filtering

**NOTE:** This is taxing to the server so it should be avoided where possible

### Using underscores to denote the number of characters

```sql
SELECT * FROM customers
WHERE last_name LIKE '____'  /* returns last names with 4 chars*/
```

### Using the wildcard symbol '%'

```sql
SELECT * FROM customers
WHERE last_name LIKE '%'
/* returns all rows */
```

```sql
SELECT * FROM customers
WHERE last_name LIKE 'S%' 
/* returns last names that start with a capital S */
```

```sql
SELECT * FROM customers
WHERE last_name LIKE '%a%' 
/* returns last names that contain an lower case a */
```

### Combining wildcards and underscores

```sql
SELECT * FROM customers
WHERE last_name LIKE '%t_'
/* returns last names where 't' is the second to last character */
```

## JOINS

- JOINs treat rows of data as if they were Sets
- We can perform set operations on the tables
- JOINs are fairly quick and do not caue a major performance hit

### INNER JOIN

- Set intersection is the elements common to two sets
- INNER JOIN is similar to **set intersection**
- INNER JOIN selects rows from table 1 and table2 where they match the selecting column

    ```sql
    SELECT * FROM Customers
    INNER JOIN Orders
    ON Customers.ID = Orders.Customer_ID
    ```

### LEFT JOIN

- This selects all rows from the table1 (on the left), and the rows from table2 (on the right) **if they match**
- If they don't match, the data for the right table is blank

    ```sql
    SELECT * FROM Customers
    LEFT JOIN Orders
    ON Customer.ID = Orders.Customer_ID
    ```

### RIGHT JOIN

- Opposite of LEFT JOIN
- This selects all rows from the table2 (on the right), and the rows from table1 (on the left) **if they match**
- If they don't match, the data for the left table is blank

    ```sql
    SELECT * FROM Customers
    RIGHT JOIN Orders
    ON Customer.ID = Orders.Customer_ID
    ```

### FULL JOIN

- This selects all rows from both tables, matching them if there is a match on the selected column

    ```sql
    SELECT * FROM Customers
    FULL JOIN Orders
    ON Customer.ID = Orders.Customer_ID
    ```

## AGGREGATE FUNCTIONS

**NOTE:** when grouping data using GROUP BY,  some columns may become obsolete as a result of the grouping, however you can still that data using aggregate functions such as COUNT or SUM

### GROUP BY and COUNT

Count all purchases by a customer

```sql
SELECT COUNT(id) AS purchases, customer_id FROM purchases
GROUP BY customer_id;

--OR

SELECT customers.first_name, customers.last_name, COUNT(purchases.id)
FROM customers
LEFT JOIN purchases ON customers.id = purchases.customer_id
GROUP BY customers.id;
```

### SUM

```sql
SELECT customers.first_name, customers.last_name, COUNT(items.name), SUM(items.price)
FROM items
INNER JOIN purchases ON items.id = purchases.item_id
INNER JOIN customers ON purchases.customer_id = customers.id
GROUP BY customers.id
```

```sql
SELECT SUM(items.price)
FROM purchases
INNER JOIN items ON purchases.item_id = items.id
```

### ORDER BY and LIMIT

```sql
SELECT customers.first_name, customers.last_name, COUNT(items.name), SUM(items.price) AS "total_spent"
FROM items
INNER JOIN purchases ON items.id = purchases.item_id
INNER JOIN customers ON purchases.customer_id = customers.id
GROUP BY customers.id
ORDER BY total_spent DESC
LIMIT 1
```

# SQL - DDL

## CREATE DATABASE

```sql
CREATE DATABASE steezcorp
    WITH 
    OWNER = postgres
    ENCODING = 'UTF8'
    LC_COLLATE = 'English_United States.1252'
    LC_CTYPE = 'English_United States.1252'
    TABLESPACE = pg_default
    CONNECTION LIMIT = -1;
```

## CREATE (and DROP) TABLE

Very basic table

```sql
DROP TABLE IF EXISTS public.users;

CREATE TABLE public.users (
    id INTEGER PRIMARY KEY,
    name CHARACTER varying(100) NOT NULL
)

-- OR
DROP TABLE IF EXISTS public.users;

CREATE TABLE public.users (
    id INTEGER,
    name CHARACTER varying(100) NOT NULL,
    CONSTRAINT users_id_pkey PRIMARY KEY (id) -- users_id_pkey is just the 'name' of the constraint
)
```

## FOREIGN KEY

```sql
DROP TABLE IF EXISTS public.videos;

CREATE TABLE public.videos (
    id INTEGER PRIMARY KEY,
    user_id INTEGER REFERENCES public.users, -- FOREIGN KEY
    name CHARACTER VARYING(255) NOT NULL
);
```

## INSERT data

```sql
INSERT INTO public.users --this works if data is inserted into ALL columns in the table
VALUES (1, 'hans_gruber');

--OR

INSERT INTO public.users(id, name)
VALUES (1, 'hans_gruber');
```