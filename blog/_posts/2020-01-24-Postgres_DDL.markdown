---
layout: post
title:  "PostgreSQL - DDL"
date:   2019-01-23 12:42:30 -0501
categories: blog
author: Stavros
---

A quick guide to PostreSQL DDL (Data Definition Language) syntax.

# Resources

[Official Documentation](https://www.postgresql.org/docs/10/index.html)

# CREATE DATABASE

```sql
CREATE DATABASE steezcorp;
```

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

# CREATE (and DROP) TABLE

```sql
CREATE TABLE public.users (
    id INTEGER PRIMARY KEY,
    name CHARACTER varying(100) NOT NULL
)

-- OR

CREATE TABLE public.users (
    id INTEGER,
    name CHARACTER varying(100) NOT NULL,
    CONSTRAINT users_id_pkey PRIMARY KEY (id) -- users_id_pkey is just the 'name' of the constraint
)

-- OR

DROP TABLE IF EXISTS employees;
CREATE TABLE employees(
    id SERIAL PRIMARY KEY,
    first_name text NOT NULL,
    last_name text NOT NULL
);
```

# DROP TABLE
### ... or DROP DATABASE, SEQUENCE, more

```sql
DROP TABLE public.users;

-- RESTRICT is appended by default

DROP TABLE public.users RESTRICT;
```

## Use DROP CASCADE to delete any foreign key relationships as well

```sql
DROP TABLE public.users CASCADE;
```

## DROP TABLE testing for existence

```sql
DROP TABLE IF EXISTS public.users;
```

# FOREIGN KEY

```sql
CREATE TABLE public.videos (
    id INTEGER PRIMARY KEY,
    user_id INTEGER REFERENCES public.users, -- FOREIGN KEY
    name CHARACTER VARYING(255) NOT NULL
);
```

# INSERT data

```sql
INSERT INTO public.users --this works if data is inserted into ALL columns in the table
VALUES (1, 'hans_gruber');

--OR

INSERT INTO public.users(id, name)
VALUES (1, 'hans_gruber');
```

# UPDATE values

Update specific values

```sql
UPDATE items
SET price = 4.00
WHERE id = 3;
```

# DELETE rows

Delete rows that meet specific criteria

```sql
DELETE FROM items WHERE id=4;
```

# CREATE SEQUENCE

### This is unique to Postgres - many other systems use "auto-increment"

```sql
CREATE SEQUENCE users_id_seq START 3; --replace default value with the sequence
```

# ALTER TABLE

```sql
ALTER TABLE public.users
ALTER COLUMN id
SET DEFAULT nextval('users_id_seq');

ALTER SEQUENCE users_id_seq OWNED BY public.users.id
```

# SERIAL data type

**NOTE** This creates a SEQUENCE starting at 1

```sql
CREATE TABLE test(
    id SERIAL PRIMARY KEY, -- Creates a sequences starting at 1
    name text -- same as CHARACTER VARYING(255) without the upper limit
)

INSERT INTO test(name) VALUES ('Steez')
```
