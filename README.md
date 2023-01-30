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
