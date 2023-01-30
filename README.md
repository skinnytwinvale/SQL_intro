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

