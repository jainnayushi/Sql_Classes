# SQL
* Structured Query Language, used to access and manipulate data.

## CRUD operations
* SQL used CRUD operations to communicate with DB.
* CREATE - execute INSERT statements to insert new tuple into the relation.
* READ - Read data already in the relations.
* UPDATE - Modify already inserted data in the relation.
* DELETE - Delete specific data point/tuple/row or multiple rows.

SQL is not DB, is a query language.
   
## What is RDBMS? 
* (Relational Database Management System)
* Software that enable us to implement designed relational model.
* e.g., MySQL, MS SQL, Oracle, IBM etc.
* Table/Relation is the simplest form of data storage object in R-DB.
* MySQL is open-source RDBMS, and it uses SQL for all CRUD operations
* MySQL used client-server model, where client is CLI or frontend that used services provided by MySQL server.

## Difference between SQL and MySQL
* SQL is Structured Query language used to perform CRUD operations in R-DB
* While MySQL is a RDBMS used to store, manage and administrate DB (provided by itself) using SQL.

---

## SQL DATA TYPES (Ref: https://www.w3schools.com/sql/sql_datatypes.asp)
Image
1. In SQL DB, data is stored in the form of tables.
2. Data can be of different types, like INT, CHAR etc.
3. Size: TINY < SMALL < MEDIUM < INT < BIGINT.
4. Variable length Data types e.g., VARCHAR, are better to use as they occupy space equal to the actual data size.
5. Values can also be unsigned e.g., INT UNSIGNED.

--- 

## Types of SQL commands:
1. `DDL (data definition language)`: defining relation schema.
   1. CREATE: create table, DB, view.
   2. ALTER TABLE: modification in table structure. e.g, change column datatype or add/remove columns.
   3. DROP: delete table, DB, view.
   4. TRUNCATE: remove all the tuples from the table.
   5. RENAME: rename DB name, table name, column name etc.
2. `DRL/DQL (data retrieval language / data query language)`: retrieve data from the tables.
   1. SELECT
3. `DML (data modification language)`: use to perform modifications in the DB
   1. INSERT: insert data into a relation
   2. UPDATE: update relation data.
   3. DELETE: delete row(s) from the relation.
4. `DCL (Data Control language)`: grant or revoke authorities from user.
   1. GRANT: access privileges to the DB
   2. REVOKE: revoke user access privileges.
5. `TCL (Transaction control language)`: to manage transactions done in the DB
    1.  START TRANSACTION: begin a transaction
    2.  COMMIT: apply all the changes and end transaction
    3.  ROLLBACK: discard changes and end transaction
    4.  SAVEPOINT: checkout within the group of transactions in which to rollback.
---

## MANAGING DB (DDL)
1. Creation of DB
   1. CREATE DATABASE IF NOT EXISTS db-name;
   2. USE db-name; //need to execute to choose on which DB CREATE TABLE etc commands will be executed. //make switching between DBs possible.
   3. DROP DATABASE IF EXISTS db-name; //dropping database.
   4. SHOW DATABASES; //list all the DBs in the server.
   5. SHOW TABLES; //list tables in the selected DB.

--- 
## DATA RETRIEVAL LANGUAGE (DRL)
#### SELECT
1. Syntax: SELECT <set of column names> FROM <table_name>;
2. Order of execution from RIGHT to LEFT.
3. Q. Can we use SELECT keyword without using FROM clause?
   1. Yes, using DUAL Tables.
   2. Dual tables are dummy tables created by MySQL, help users to do certain obvious actions without referring to user defined tables.
   3. e.g., SELECT 55 + 11;
        SELECT now();
        SELECT ucase(); etc.
#### WHERE
   1. Reduce rows based on given conditions.
   2. E.g., SELECT * FROM customer WHERE age > 18;
#### BETWEEN
   1. SELECT * FROM customer WHERE age between 0 AND 100;
   2. In the above e.g., 0 and 100 are inclusive.
   3. [0,100]
#### IN
   1.  Reduces OR conditions;
   2.  e.g., SELECT * FROM officers WHERE officer_name IN ('Lakshay', ‘Maharana Pratap', ‘Deepika’);
#### AND/OR/NOT
   1.  AND: WHERE cond1 AND cond2
   2.  OR: WHERE cond1 OR cond2
   3.  NOT: WHERE col_name NOT IN (1,2,3,4);
#### IS NULL
    e.g., SELECT * FROM customer WHERE prime_status IS NULL;

#### Pattern Searching / Wildcard (‘%’, ‘_’)
   1.  ‘%’, any number of character from 0 to n. Similar to ‘*’ asterisk in regex.
   2.  ‘_’, only one character.
   3.  SELECT * FROM customer WHERE name LIKE ‘%p_’;
   
#### ORDER BY
   1.  Sorting the data retrieved using WHERE clause.
   2.  ORDER BY <column-name> DESC;
   3.  DESC = Descending and ASC = Ascending
   4.  e.g., SELECT * FROM customer ORDER BY name DESC;
   
#### GROUP BY
   1. GROUP BY Clause is used to collect data from multiple records and group the result by one or more column. It is
   generally used in a SELECT statement.
   2. Groups into category based on column given.
   3. SELECT `c1, c2, c3` FROM sample_table WHERE cond GROUP BY `c1, c2, c3`.
   4. All the column names mentioned after SELECT statement shall be repeated in GROUP BY, in order to successfully
   execute the query.
   5. Used with aggregation functions to perform various actions.
      1. COUNT()
      2. SUM()
      3. AVG()
      4. MIN()
      5. MAX()
---

#### Data for reference

```sql
CREATE TABLE  demo.AGENTS2 
(	
	AGENT_CODE INT NOT NULL PRIMARY KEY, 
	AGENT_NAME CHAR(40), 
	WORKING_AREA CHAR(35), 
	COMMISSION VARCHAR(100), 
	PHONE_NO CHAR(15), 
	COUNTRY VARCHAR(25) 
 );

INSERT INTO AGENTS2 VALUES ('71', 'Ramasundar', 'Bangalore', '0.15', '077-25814763', NULL);
INSERT INTO AGENTS2 VALUES ('7', 'Ramasundar', 'Bangalore', '0.15', '077-25814763', '');
INSERT INTO AGENTS2 VALUES ('3', 'Alex ', 'London', '0.13', '075-12458969', '');
INSERT INTO AGENTS2 VALUES ('8', 'Alford', 'New York', '0.12', '044-25874365', '');
INSERT INTO AGENTS2 VALUES ('11', 'Ravi Kumar', 'Bangalore', '0.15', '077-45625874', '');
INSERT INTO AGENTS2 VALUES ('10', 'Santakumar', 'Chennai', '0.14', '007-22388644', '');
INSERT INTO AGENTS2 VALUES ('12', 'Lucida', 'San Jose', '0.12', '044-52981425', '');
INSERT INTO AGENTS2 VALUES ('5', 'Anderson', 'Brisban', '0.13', '045-21447739', '');
INSERT INTO AGENTS2 VALUES ('1', 'Subbarao', 'Bangalore', '0.14', '077-12346674', '');
INSERT INTO AGENTS2 VALUES ('2', 'Mukesh', 'Mumbai', '0.11', '029-12358964', '');
INSERT INTO AGENTS2 VALUES ('6', 'McDen', 'London', '0.15', '078-22255588', '');
INSERT INTO AGENTS2 VALUES ('4', 'Ivan', 'Torento', '0.15', '008-22544166', '');
INSERT INTO AGENTS2 VALUES ('9', 'Benjamin', 'Hampshair', '0.11', '008-22536178', '');
INSERT INTO AGENTS2 VALUES ('18', 'Subbarao', 'Bangalore', '0.30', '077-12346674', '');
```