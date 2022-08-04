# Structured Query Language
Structured Query Language (SQL) Backend (BE) template

## Contents
- [Reference](#reference)
- [What is meant by SQL database?](#what-is-meant-by-sql-database)
- [What does Primary Key mean?](#what-does-primary-key-mean)
- [What is Foreign Key?](#what-is-foreign-key)
  - [ON DELETE SET NULL](#on-delete-set-null)
  - [ON DELETE CASCADE](#on-delete-cascade)
- [What are Constraints](#what-are-constraints)
- [Operators](#operators)
- [SQL Commands](#sql-commands)
  - [Create Database](#create-database)
  - [Delete Database](#delete-database)
  - [Create Table](#create-table)
  - [Details of a Table](#get-details-of-table)
  - [Delete Table](#delete-table)
  - [Add Column to a Table](#add-column-to-existant-table)
  - [Delete Column from a Table](#delete-column-from-existant-table)
  - [Insert a Record](#insert-a-record-in-table)
  - [Select a Record](#select-a-record-from-a-table)
  - [Update a Record](#update-a-record)
  - [Delete a Record](#delete-a-record)
  - Functions
    - [Count a Record](#count-the-records)
    - [Average of Column](#average-of-column)
    - [Sum of Column](#sum-of-column)
    - [Aggregation of Column](#aggregation-of-column)
  - Wild Cards
    - [Wild Card %](#wild-card)
    - [Wild Card _](#wild-card-1)
  - [Union](#union)
  - [Join](#join)
  - [Left Join](#left-join)
  - [Right Join](#right-join)
  - Nested Queries
    - [Example 1](#nested-query)
    - [Example 2](#nested-query-1)
    - [Example 3](#nested-query-2)
- [Creating a Company Database](https://www.youtube.com/watch?v=HXV3zeQKqGY&t=7717s)
  - Topics:
    - Construct this [database](https://www.mikedane.com/databases/sql/company-database.pdf) with queries.
    - Create a complex table using Foreign and Primary Keys
    - Practical demonstration of using SQL queries
- [Cheat Sheet](#cheat-sheet)
- [ER Diagrams](#er-diagrams)
  - [Cardianlity Relationships](#cardianlity-relationships)
  - [One to One Relationship](https://www.youtube.com/watch?v=s6MH7f3SnsY)
  - [One to Many Relationship](https://www.youtube.com/watch?v=rZxETdO_KUQ)
  - [Many to One Relationship](https://www.youtube.com/watch?v=rZxETdO_KUQ)
  - [Many to Many Relationship](https://www.youtube.com/watch?v=onR_sLhbZ4w)


## Reference
Learned the SQL database, schema, queries from [freeCodeCamp](https://www.mikedane.com/databases/sql/). And [youtube](https://www.youtube.com/watch?v=HXV3zeQKqGY&t) tutorials.

## What is meant by SQL database?
SQL stands for Structured Query Language. It's used for relational databases. A SQL database is a collection of tables that stores a specific set of structured data.


## What does Primary Key mean?
A primary key is a special relational database table column (or combination of columns) designated to uniquely identify each table record.

A primary key is used as a unique identifier to quickly parse data within the table. A table cannot have more than one primary key. A primary key’s main features are:
- It must contain a unique value for each row of data.
- Primary key has 2 constrains i.e., `UNIQUE` and `NOT NULL`.
- Every row must have a primary key value.


## What is Foreign Key?
- Why the need of foreign key exists ? [Youtube Explanation](https://www.youtube.com/watch?v=1E--RLhxJHE)
- Foreign Key takes the reference from Primary key of another table or the same table.
- When there is the relationship between two or more tables than at least 1 column should be same in both tables. And that same column is taking reference from another table i.e., at as a foreign key.
- The table that contains FK is termed as Referencing Table whereas the table that has PK is termed as Referenced Tabled.
- A foreign key can have different name than the primary key it comes from.
- The primary key used by the foreign key has a relationship of parent - child table.
- Foreign key don't have to be unique in fact, they often aren't.


### `ON DELETE SET NULL`
If we delete the record(s) from the referenced table. The record(s) in the referencing table is set to null.
```
Example,
CREATE alphascale.branch (
  branch_id INT PRIMARY KEY,
  branch_name VARCHAR(20),
  mgr_id INT,
  mgr_start_date DATE,
  FOREIGN KEY (mgr_id) REFERENCES employee(emp_id) ON DELETE SET NULL
);

DELETE FROM alphascale.employee
WHERE emp_id = 102;

SELECT * FROM alphascale.branch
-- this will set the mgr_id (of 102) equals to NULL
```


### `ON DELETE CASCADE`
- If we delete the record(s) from the referenced table. The record(s) in the referencing table will also be deleted.
- Usually when we have a COMPOSITE PRIMARY KEY or the PRIMARY KEY is itself a FOREIGN KEY we mostly used ON DELTE CASCADE otherwise if we do like ON DELETE SET NULL. Then ones the row delete, it sets the value as NULL, but PRIMARY KEY cannot be NULL.
```
Example,
CREATE alphascale.branch_supplier(
  branch_id INT,
  supplier_name VARCHAR(20),
  supplier_type VARCHAR(20),
  PRIMARY KEY (branch_id, supplier_name),
  FOREIGN KEY (branch_id) REFERENCES branch(branch_id) ON DELETE CASCADE
);

DELETE FROM alphascale.branch
WHERE branch_id = 2;

SELECT * FROM alphascale.branch_supplier;
```


## What are Constraints?
Constraints are the rules enforced on the data columns of a table. These are used to limit the type of data that can go into a table. This ensures the accuracy and reliability of the data in the database.

Constraints could be either on a column level or a table level. The column level constraints are applied only to one column, whereas the table level constraints are applied to the whole table. Following are some of the most commonly used constraints available in SQL.

- `NOT NULL` − Ensures that a column cannot have NULL value.
- `DEFAULT` − Provides a default value for a column when none is specified.
- `UNIQUE` − Ensures that all values in a column are different.
- `PRIMARY Key` − Uniquely identifies each record in a database table.
- `FOREIGN Key` − Uniquely identifies a record in any of the given database table.
- `CHECK` − The CHECK constraint ensures that all the values in a column satisfies certain conditions.
- `INDEX` − Used to create and retrieve data from the database very quickly.
- `AUTO_INCREMENT` - Auto-increment allows a unique number to be generated automatically when a new record is inserted into a table.


## Operators
| Operators | Description |
| --------- | ----------- |
| `--` | comments |
| `<` | less than |
| `>` | greater than |
| `=` | equals to |
| `<=` | less than or equals to |
| `>=` | greater than or equals to |
| `<>` | not equals to |
| `AND` | logical and operator |
| `OR` | logical or operator |


## SQL Commands

### Create database
> $ `CREATE DATABASE {database_name};`
```
Example,
CREATE DATABASE alphascale;
```


### Delete database
> $ `DROP DATABASE {database_name};`
```
Example,
DROP DATABASE {database_name};
```


### Create table
> $ `CREATE TABLE {database_name}.{table_name} ( {column} {data_type} );`
```
Example 1,
CREATE TABLE alphascale.student (
    student_id INT PRIMARY KEY,
    name VARCHAR(20),
    major VARCHAR(20)
);
```
```
Example 2,
CREATE TABLE alphascale.students(
	student_id INT,
	username VARCHAR(20) NOT NULL,            -- It cannot be NULL
	major VARCHAR(20) UNIQUE,                 -- It must be unique in each record
	PRIMARY KEY(student_id)                   -- Primary key can also be declared here
);
```
```
Example 3,
CREATE TABLE alphascale.students(
	student_id INT,
	username VARCHAR(20) NOT NULL,
	major VARCHAR(20) DEFAULT 'undecided',    -- set a default value here
	PRIMARY KEY(student_id)
);
```
```
Example 4,
CREATE TABLE alphascale.students(
	student_id INT AUTO_INCREMENT,            -- auto increment the primary key
	username VARCHAR(20) NOT NULL,
	major VARCHAR(20) DEFAULT 'undecided',
	PRIMARY KEY(student_id)
);
```


### Get details of table
> $ `DESCRIBE TABLE {database_name}.{table_name};`
```
Example,
DESCRIBE TABLE alphascale.student;
```


### Delete table
> $ `DROP TABLE {database_name}.{table_name};`
```
Example,
DROP TABLE alphascale.student;
```


### Add column to existant table
> $ `ALTER TABLE {database_name}.{table_name} ADD {column} {data_type};`
```
Example,
ALTER TABLE alphascale.student ADD gpa DECIMAL(3, 2);
```


### Delete column from existant table
> $ `ALTER TABLE {database_name}.{table_name} DROP {column};`
```
Example,
ALTER TABLE alphascale.student DROP gpa;
```


### Insert a record in table
> $ `INSERT INTO {database_name}.{table_name} VALUES({item}, {item}, {item});`
```
Example 1,
INSERT INTO alphascale.students VALUES(1, 'jack', 'biology');
```
```
Example 2,
INSERT INTO alphascale.students(student_id, name) VALUES(4, 'Clair');  -- It can be used in case of default value.
```
```
Example 3,
INSERT INTO alphascale.students VALUES (2, NULL, 'telecommunication');
```


### Select a record from a table
> $ `SELECT {condition} FROM {database_name}.{table_name}`
```
Example 1,
SELECT * FROM alphascale.students;            -- select every thing
```
```
Example 2,
SELECT username, major FROM alphascale.students;     -- select username and major column 
```
```
Example 3,
SELECT first_name AS forename, last_name AS surname  -- aliase first_name as forename
FROM alphascale.employee; 
```
```
Example 3,
SELECT username, major 
FROM alphascale.students
ORDER BY student_id DESC;                      -- order by descending order of student_id
```
```
Example 4,
SELECT * 
FROM alphascale.students
ORDER BY student_id ASC;
```
```
Example 5,
SELECT *
FROM alphascale.students
ORDER BY major;                               -- ordered in alphabatical order of major
```
```
Example 6,
SELECT *
FROM alphascale.students
ORDER BY major, student_id DESC;
```
```
Example 7,
SELECT *
FROM alphascale.students
LIMIT 2;                                      -- it will only show 2 records
```
```
Example 8,
SELECT *
FROM alphascale.students
ORDER BY major
LIMIT 2;
```
```
Example 9,
SELECT *
FROM alphascale.students
WHERE major = 'chemistry';
```
```
Example 10,
SELECT username, major
FROM alphascale.students
WHERE major = 'chemistry';
```
```
Example 11,
SELECT username, major
FROM alphascale.students
WHERE major = 'chemistry' OR major = 'biology';
```
```
Example 12,
SELECT *
FROM alphascale.students
WHERE major = 'chemistry' OR username = 'asad@example.com';
```
```
Example 13,
SELECT *
FROM alphascale.students
WHERE major IN ('biology', 'chemistry');         -- if major is bioloyg or chemistry select it
```
```
Example 14,
SELECT DISTINCT sex                              -- select distinct entries in column 'sex'
FROM alphascale.employee;
```


### Update a record
> $ `UPDATE {database_name}.{table_name} SET {column} = '{new_value}' WHERE {codition}`
```
Example 1,
UPDATE alphascale.students
SET major = 'Bio'
WHERE major = 'electronics';
```
```
Example 2,
UPDATE alphascale.students 
SET major = 'Bio'
WHERE major = 'electronics' OR major = 'computer science';
```
```
Example 3,
UPDATE alphascale.students 
SET username = 'example@example.com', major = 'default major'
WHERE student_id = 1;
```
```
Example 4,
UPDATE alphascale.students 
SET username = 'example@example.com';     -- update all records
```


### Delete a record
> $ `DELETE FROM {database_name}.{table_name} WHERE {codition}`
```
Example 1,
DELETE FROM alphascale.students
WHERE student_id = 1;
```
```
Example 2,
DELETE FROM alphascale.students
WHERE username = 'tom@example.com' AND major = 'default major';
```
```
Example 3,
DELETE FROM alphascale.students;          -- delete all records
```


### Count the records
> $ `SELECT COUNT({column}) FROM {database_name}.{table_name}`
```
Example,
SELECT COUNT(emp_id)
FROM alphascale.employee;
```


### Average of column
> $ `SELECT AVG({column} FROM {database_name}.{table_name}`
```
Example,
SELECT AVG(salary)
FROM alphascale.employee;
```


### Sum of column
> $ `SELECT SUM({column}) FROM {database_name}.{table_name}`
```
Example,
SELECT SUM(salary)
FROM alphascale.employee;
```


### Aggregation of column
> $ `SELECT COUNT({column}), {column} FROM {database_name}.{table_name} GROUP BY {column}`
```
Example 1,
SELECT COUNT(sex), sex
FROM alphascale.employee
GROUP BY sex;
```
```
Example 2,
SELECT SUM(total_sales), emp_id
FROM alphascale.works_with
GROUP BY emp_it;
```


### Wild card %
> $ `SELECT * FROM {database_name}.{table_name} WHERE {column} LIKE '{condition}'`
```
Example 1,
SELECT *
FROM alphascale.client
WHERE client_name LIKE '%LLC'         -- string ends with LLC
```
```
Example 2,
SELECT *
FROM alphascale.client
WHERE client_name LIKE '%Fe%'         -- {number of char}Fe{number of char}
```
```
Example 3,
SELECT *
FROM alphascale.employee
WHERE client_name LIKE '%-10-%'
```


### Wild card _
> $ `SELECT * FROM {database_name}.{table_name} WHERE {column} LIKE '{condition}'`
```
Example 1,
SELECT *
FROM alphascale.client
WHERE client_name LIKE '____-10%'         -- _ means only 1 char
```


### Union
> $ `SELECT ... UNION SELECT ...;`
```
Example 1,
SELECT first_name                   -- data type of first_name and branch_name should be same
FROM alphascale.employee            -- same number of columns
UNION
SELECT branch_name
FROM alphascale.branch;
```
```
Example 2,
SELECT first_name, emp_id           -- same number of columns
FROM alphascale.employee
UNION
SELECT branch_name, branch_id
FROM alphascale.branch;
```


### Join
> $ `SELECT ... FROM {database}.{table} JOIN {database}.{table} ON {condition}`
```
Example 1,
SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM alphascale.employee
JOIN alphascale.branch
ON employee.emp_id = branch.mgr_id;             -- Joins two or more columns into singel table
```


### Left Join
> $ `SELECT ... FROM {database}.{table} LEFT JOIN {database}.{table} ON {condition}`
```
Example 1,
SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM alphascale.employee
LEFT JOIN alphascale.branch
ON employee.emp_id = branch.mgr_id;             -- all rows of left, selected row of right
```


### Right Join
> $ `SELECT ... FROM {database}.{table} RIGHT JOIN {database}.{table} ON {condition}`
```
Example 1,
SELECT employee.emp_id, employee.first_name, employee.last_name, branch.branch_name
FROM alphascale.employee
RIGHT JOIN alphascale.branch
ON employee.emp_id = branch.mgr_id;             -- all rows of right, selected row of left
```


### Nested Query
> $ `SELECT ... FROM {database}.{table} WHERE {column} IN ( {second_query} );`
```
Example,
SELECT employee.emp_id, employee.first_name, employee.last_name, employee.salary
FROM alphascale.employee
WHERE emp_id IN (
	SELECT branch.mgr_id
	FROM alphascale.branch
);
```


### Nested Query
> $ `SELECT ... FROM {database}.{table} WHERE {column} IN ( {second_query} );`
```
Example,
SELECT employee.emp_id, employee.first_name, employee.last_name, employee.branch_id
FROM alphascale.employee
WHERE branch_id IN (
	SELECT DISTINCT branch_supplier.branch_id
	FROM alphascale.branch_supplier
);
```


### Nested Query
> $ `SELECT ... FROM {database}.{table} WHERE {column} IN ( {second_query} );`
```
Example,
SELECT employee.emp_id, employee.first_name
FROM alphascale.employee
WHERE employee.emp_id IN (
	SELECT works_with.emp_id
	FROM alphascale.works_with
	WHERE works_with.total_sales > 25000
);
```


## Cheat Sheet
| Description | Syntax 
| ----------- | ----------- |
| Create Database | `CREATE DATABASE {database_name};` |
| Delete Database | `DROP DATABASE {database_name};` |
| Create Table | `CREATE TABLE {database_name}.{table_name} ( {column} {data_type} );` |
| Details of a Table | `DESCRIBE TABLE {database_name}.{table_name};` |
| Delete Table | `DROP TABLE {database_name}.{table_name};` |
| Add Column to a Table | `ALTER TABLE {database_name}.{table_name} ADD {column} {data_type};` |
| Delete Column from a Table | `ALTER TABLE {database_name}.{table_name} DROP {column};` |
| Insert a Record | `INSERT INTO {database_name}.{table_name} VALUES({item}, {item}, {item});` |
| Select a Record | `SELECT * FROM {database_name}.{table_name}` |
| Update a Record | `UPDATE {database_name}.{table_name} SET {column} = '{new_value}' WHERE {codition}` |
| Delete a Record | `DELETE FROM {database_name}.{table_name} WHERE {codition}` |
| Count a Record | `SELECT COUNT({column}) FROM {database_name}.{table_name}` |
| Average of Column | `SELECT AVG({column} FROM {database_name}.{table_name}` |
| Sum of Column | `SELECT SUM({column}) FROM {database_name}.{table_name}` |
| Aggregation of Column | `SELECT COUNT({column}), {column} FROM {database_name}.{table_name} GROUP BY {column}` |
| Wild Card % | `SELECT * FROM {database_name}.{table_name} WHERE {column} LIKE '{condition}'` |
| Wild Card _ | `SELECT * FROM {database_name}.{table_name} WHERE {column} LIKE '{condition}'` |
| Union | `SELECT ... UNION SELECT ...;` |
| Join | `SELECT ... FROM {database}.{table} JOIN {database}.{table} ON {condition}` |
| Left Join | `SELECT ... FROM {database}.{table} LEFT JOIN {database}.{table} ON {condition}` |
| Right Join | `SELECT ... FROM {database}.{table} RIGHT JOIN {database}.{table} ON {condition}` |
| Nested Query | `SELECT ... FROM {database}.{table} WHERE {column} IN ( {second_query} );` |


## ER Diagrams
Entity Relationship Diagrams


## Cardianlity Relationships
Create a Cardianlity Relationship for [sample database](https://lucid.app/lucidchart/c432c7bd-083f-4e48-8a94-28b137ef8966/edit?page=0_0&invitationId=inv_d85cb984-79e7-4ddc-ac7f-0d8410fe439c#)

Studied the cardianlity relationship. Create notes in my diary.


