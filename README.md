# SQL_intro

## Definitions

1. Database - a collection of records that can easily be updated, accessed and managed. Databases
are used to capture and analyze data on a more permanent basis.
2. Relatonal Database - a type of database that is structured so that relationships can be established among stored information.
3. SQL - Structured Query Language is the standardized language for communicating with and managing data in relational database. The acronym is pronounced like the word "sequel".
4. RDBMS - A Relational DataBase Management System is a database management system based on a "relational" model. This model was actually developed before the SQL language and is the basis for SQL and systems like MySQL, Postgres, Oracle, IBM DB2, Microsoft SQL Server and many more.
5. PostgreSQL - PostgreSQL or "postgres" is an RDBMS that is open source (free for everyone to use and contribute too!). Postgres powers some of the largest companies in the world.
6. Schema - the organization of data inside of a database. The database schema represents the collection and association of tables in a database.
7. Table - A series of columns and rows which store data inside of a database. An example of a table is "users" or "customers".
8. Column - A portion of a table which has a specific category and data type. If we had a table called "users", we could create columns for "username", "password", which would both be a variable amount of characters or text. Postgres has quite a few data types, which we will see later
9. Row / Record - Each row in a table represents a record stored. In our "users" table, we may have a row that looks like 1, "elie", "secret". Where 1 represents a unique id, "elie" represents the value of the "username" and "secret" represents the value of the "password".
10. psql - a command line program, which can be used to enter PostgreSQL queries directly, or executed from a file.

# CRUD

## PSQL Syntax

- \du - lists users
- \dt - lists tables
- \d+ table_name - list details about the table name \l - lists databases
- \c NAME_OF_DB - connect to a database

New databases can be created in two ways:
1. In psql type CREATE DATABASE name_of_db;
2. In the terminal type createdb name_of_db 

Existing databases can be removed in two ways:
1. In psql type DROP DATABASE name_of_db; - make sure you are not connected to that database or the command will not work
2. In the terminal type dropdb name_of_db

## Syntax Gotchas

1. The most important thing with SQL syntax is to end your statements with a SEMI-COLON ;. SQL will not understand when you have finished your statement unless it sees that.
2. You also MUST make sure to put all text strings in single quotes ', not double quotes. SQL views double quotes as a name of a table and single quotes as a string.

## DDL vs DML
When working with SQL, the commands you will be using fall into two major categories:

DDL - Data Definition Language - this refers to the SQL syntax and commands around creating,
modifying and deleting tables, columns and databases.

DML - Data Manipulation Language - this refers to the SQL syntax and commands around creating,
reading, modifying and deleting rows.

### Creating a table (DDL)

CREATE TABLE users (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);

\d+ users should show our newly-created users table with 3 columns: id, first_name, last_name
                    
In the example above, users is the name of the table we are creating. The id, first_name, and last_name are all columns in the users table. SERIAL and TEXT are examples of data types, which we will talk about in detail next. PRIMARY KEY is a constraint placed on the column.

## Data Types in Postgres
In relational databases like postgres, you must specify the type of data that you plan to store in a column. Here are the types in postgres:
- SERIAL - auto incrementing integer, perfect for IDs
- TEXT - pieces of text (equally as space efficient as VARCHAR)
- VARCHAR - a variable number of characters CHAR - a fixed number of characters BOOLEAN - a boolean
- INTEGER - an integer
- REAL - a floating point number, e.g., 3.141593
- DECIMAL, NUMERIC - floating point numbers that have user specified percision. MONEY - floating point numbers use for money
- ARRAY - an array (array types are rarely used)

## Constraints
Constraints are certain database table restrictions that are either implicitly or explicitly created via the database schema. Constraints are a key part of DDL. Violating a constraint will throw a database error and (usually) abort the intended operation.

Common constraints are:

- not null - when creating or updating a record, a column value cannot be made NULL. This constraint is requiring information; for example, a table of college students cannot have NULL for their college major.

CREATE TABLE table_name (
    id SERIAL PRIMARY KEY,
    last_name VARCHAR(50),
    first_name VARCHAR(50),
    major VARCHAR(50) NOT NULL
);

- unique - a column value for a record must be unique in its table. For example, consider a table of users with a phone_number column: no two users may have the same phone number, so a unique constraint is placed on that column.

CREATE TABLE table_name (
    id SERIAL PRIMARY KEY,
    last_name VARCHAR(50),
    first_name VARCHAR(50),
    phone_number VARCHAR(7) UNIQUE
    
- primary key - an identifier for a record which has both unique and not-null constraints. Primary keys are used internally by the RDBMS to reference rows. Good examples of primary keys include: drivers license number, social security number, auto-generated unique IDs such as a UUID, etc. Bad examples are things like email address, full name, or phone number.

CREATE TABLE table_name (id SERIAL PRIMARY KEY,
                    first_name TEXT,
                    last_name TEXT);
                    
- foreign key - we'll talk about foreign keys later when we talk about joins.
- check - an expression is provided that must evaluate truthy for the operation to proceed. For example, if you have a table of products for an online shopping site, you might put a check constraint on products to have price > 0.

CREATE TABLE table_name (
    product_no SERIAL PRIMARY KEY,
    name TEXT,
    price NUMERIC CHECK (price > 0)
);

### Adding a column in a table
ALTER TABLE users ADD COLUMN favorite_number INTEGER;

### Removing a column in a table
ALTER TABLE users DROP COLUMN favorite_number;

### Renaming columns in a table
ALTER TABLE users ADD COLUMN jobb TEXT;

ALTER TABLE users RENAME COLUMN jobb TO job;

\d+ table_name and we should see 'job' spelled correctly now

## Changing the datatype of a column in a table DDL
ALTER TABLE users ADD COLUMN favorite_number TEXT;

ALTER TABLE users ALTER COLUMN favorite_number SET DATA TYPE VARCHAR(100);

### Adding a constraint to a table
ALTER TABLE users ADD CONSTRAINT favorite_number NOT NULL;

## Creating and Modifying data (DML)
When we perform CRUD operations on our rows (not columns, tables or databases) we are using DML or Data Manipulation Language.

CRUD SQL
- Create INSERT
- Read   SELECT 
- Update UPDATE 
- Delete DELETE

### SELECT
--to select all rows and columns--

SELECT * FROM users;

--to select specific columns--

SELECT id, first_name FROM users;

--to select specific columns and rows--

SELECT id, first_name FROM users WHERE id=1;

### INSERT
To insert or add data to a table - we use the INSERT command

--start with the INSERT INTO commands and then specify a table(column1,
column2, ...) and VALUES for each column.

INSERT INTO users(first_name, last_name) VALUES ('Elie', 'Schoppik');

### UPDATE
To update a row or multiple rows we use the UPDATE command.

UPDATE users SET first_name = 'Elie'; -- will update all users

UPDATE users SET first_name = 'Elie' WHERE id = 1; -- will update a user with
an id of 1

### DELETE
To delete a row or multiple rows we use the DELETE FROM command.

DELETE FROM users; -- will delete all users

DELETE FROM users WHERE id=1; -- will delete a user with an id of 1

# Operators And Aggregates

By the end of this chapter, you should be able to:
- Use built in operators to write more complex queries 
- Use ORDER BY to categorize results
- Use built in aggregate functions to calculate data 
- Use GROUP BY to aggregate data into sub groups 
- Use CASE statements for custom conditional output

### WHERE

The building block for these operators is the WHERE clause which comes after any operation like SELECT, UPDATE or DELETE

<img width="306" src="https://user-images.githubusercontent.com/101606295/216842846-c7b6e07b-3d93-4afe-918f-9f5fda980c24.png">
<img width="306" src="https://user-images.githubusercontent.com/101606295/216842865-3d9a5e96-0593-496b-8282-f591eb2f1894.png">

### IN

To find multiple records we can search for multiple terms using IN

<img width="306" alt="Screenshot 2023-02-05 at 3 26 30 PM" src="https://user-images.githubusercontent.com/101606295/216843081-207f6dd0-810d-4208-9094-7fd57ec45b79.png">

### NOT IN

We can do the exact inverse of IN using NOT IN

<img width="306" alt="Screenshot 2023-02-05 at 3 28 42 PM" src="https://user-images.githubusercontent.com/101606295/216843163-c218ca63-6216-4368-915f-fa0fcef28f28.png">

### BETWEEN

To search for a field in a range, we can use BETWEEN x AND y.

<img width="306" alt="Screenshot 2023-02-05 at 3 30 02 PM" src="https://user-images.githubusercontent.com/101606295/216843218-7321698f-7852-46d7-b481-ef20e93bde7f.png">

### Arithmetic

SQL supports all kinds of arithmetic operators like <, <=, >=, > and !=.

<img width="306" alt="Screenshot 2023-02-05 at 3 31 18 PM" src="https://user-images.githubusercontent.com/101606295/216843282-f5fe7c9d-779c-425f-8f5b-1f6e17fbe5a9.png">

### AND

To check that multiple conditions are satisfied we can use the AND command.

<img width="306" alt="Screenshot 2023-02-05 at 3 32 32 PM" src="https://user-images.githubusercontent.com/101606295/216843343-6ae7e06a-9c65-44ca-9729-131777d60b78.png">

### OR

To check that at least one condition is satisfied we can use the OR command.

<img width="306" alt="Screenshot 2023-02-05 at 3 33 27 PM" src="https://user-images.githubusercontent.com/101606295/216843392-8f44c8ee-ad8c-4777-baff-0f2c8777d832.png">

### LIKE

To search for a term we can use the LIKE. The % denotes any possible character. LIKE is
case sensitive.
- FIND ALL PLAYERS WHOSE NAME STARTS WITH IS

<img width="306" alt="Screenshot 2023-02-05 at 3 34 08 PM" src="https://user-images.githubusercontent.com/101606295/216843484-2557ca97-a789-432c-a3fa-3d25bae5f041.png">

### ILIKE

ILIKE is similar to the LIKE command, but it is case insensitive.

<img width="306" alt="Screenshot 2023-02-05 at 3 35 43 PM" src="https://user-images.githubusercontent.com/101606295/216843924-66a8972a-f8f5-41a6-a4a8-08b32fce75e6.png">

### ORDER BY

If we want to order results in ascending or descending order we use the ORDER BY ASC_or_DESC command.

<img width="306" alt="Screenshot 2023-02-05 at 3 37 39 PM" src="https://user-images.githubusercontent.com/101606295/216844537-b164cb46-b715-49cd-8a42-4c860231b7f5.png">

## Changing Data Types

Commonly in SQL, we will want to output a certain data type for an operation, to convert one data type to another we can use the CAST command or use ::data_type

<img width="306" alt="Screenshot 2023-02-05 at 4 00 01 PM" src="https://user-images.githubusercontent.com/101606295/216845508-5ddcc00a-0d69-4be9-9c7c-c27fb135283f.png">

### COUNT

To count the number of occurances we use the COUNT function.

<img width="306" alt="Screenshot 2023-02-05 at 3 54 17 PM" src="https://user-images.githubusercontent.com/101606295/216845272-4e9e7eb4-8314-43ff-9bcc-62aaf5f3cf75.png">
<img width="306" alt="Screenshot 2023-02-05 at 3 55 04 PM" src="https://user-images.githubusercontent.com/101606295/216845305-70f3d8ae-421d-41c1-a336-3116b5936ed6.png">

### SUM

To figure out the sum we can use the SUM function and even round numbers using the ROUND function as well.

<img width="306" alt="Screenshot 2023-02-05 at 3 55 50 PM" src="https://user-images.githubusercontent.com/101606295/216845339-429703ad-5b27-4cee-9a58-cc1e2fa5337d.png">

### MIN

To find the minimum value in a data set we use the MIN function.

<img width="306" alt="Screenshot 2023-02-05 at 3 56 53 PM" src="https://user-images.githubusercontent.com/101606295/216845388-19c4ea94-d84c-41c8-ac58-594390f03b64.png">

### MAX

To find the maximum value in a data set we use the MAX function.

<img width="306" alt="Screenshot 2023-02-05 at 3 57 00 PM" src="https://user-images.githubusercontent.com/101606295/216845401-8022d73c-006d-48b1-ada0-a85aa8b94f9f.png">

### AVG

To find the average value in a data set we use the AVG function. We can attach the AS command to alias the column name.

<img width="306" alt="Screenshot 2023-02-05 at 3 57 08 PM" src="https://user-images.githubusercontent.com/101606295/216845411-35005f90-376e-4326-a346-66cf1790e452.png">


### GROUP BY

Now that we have seen a couple aggregate functions, lets take some information.

<img width="306" alt="Screenshot 2023-02-05 at 3 58 26 PM" src="https://user-images.githubusercontent.com/101606295/216845439-7d707da1-88a4-4167-b551-bb65ef964c2a.png">

### HAVING

When using a GROUP BY clause, we can not attach a WHERE if we want to be more selective. Instead we use the HAVING keyword to place condition on our GROUP BY command.

<img width="306" alt="Screenshot 2023-02-05 at 3 58 58 PM" src="https://user-images.githubusercontent.com/101606295/216845473-f0da4e20-732c-484c-89fd-95d13ea74593.png">

### DISTINCT

If we only want to find unique values in a column, we can use DISTINCT, we can also do this for pairs of columns separated by a comma.



### CASE

In SQL, we can use conditional logic to query our data and display custom results based off of the condition.








