### DISTINCT
1. Find distinct values in the table.
2. `SELECT DISTINCT(col_name) FROM table_name;`
3. GROUP BY can also be used for the same
4. “`Select col_name from table GROUP BY col_name;`” same output as above DISTINCT query.
5. SQL is smart enough to realise that if you are using GROUP BY and not using any aggregation function, then
you mean “DISTINCT”.

### GROUP BY HAVING
1. Out of the categories made by GROUP BY, we would like to know only particular thing (cond).
2. Similar to WHERE.
3. `Select COUNT(cust_id), country from customer GROUP BY country HAVING COUNT(cust_id) > 50;`
   
### WHERE vs HAVING
1. Both have same function of filtering the row base on certain conditions.
2. WHERE clause is used to filter the rows from the table based on specified condition
3. HAVING clause is used to filter the rows from the groups based on the specified condition.
4. HAVING is used after GROUP BY while WHERE is used before GROUP BY clause.
5.  If you are using HAVING, GROUP BY is necessary.
6.  WHERE can be used with SELECT, UPDATE & DELETE keywords while GROUP BY used with SELECT.

---
### CONSTRAINTS (DDL)
1. `Primary Key`
   * PK is not null, unique and only one per table.
  ```sql
   CREATE TABLE ORDER (
   id INT PRIMARY KEY,
   delivery_date DATE,
   order_placed_date DATE,
   PRIMARY KEY(id)
   );
   ```
2. `Foreign Key`
   * FK refers to PK of other table.
   * Each relation can having any number of FK.
   ```sql
   CREATE TABLE ORDER (
   id INT PRIMARY KEY,
   delivery_date DATE,
   order_placed_date DATE,
   cust_id INT,
   FOREIGN KEY (cust_id) REFERENCES customer(id)
   );
   ```
3. `UNIQUE`
   * Unique, can be null, table can have multiple unique atributes.
   ```sql
   CREATE TABLE customer (
   …
   email VARCHAR(1024) UNIQUE,
   …
   );
   ```
4. `CHECK`
   * “age_check”, can also avoid this, MySQL generates name of constraint automatically
    ```sql
    CREATE TABLE customer (
    …
    CONSTRAINT age_check CHECK (age > 12),
    …
    );
    ```
   
5. `DEFAULT`
   * Set default value of the column.
   ```sql
    CREATE TABLE account (
   …
   saving-rate DOUBLE NOT NULL DEFAULT 4.25,
   …
   );
   ```
#### Note : An attribute can be PK and FK both in a table.
---

### ALTER OPERATIONS
* Changes schema
1. `ADD`
   1. Add new column.
   2. `ALTER TABLE table_name ADD new_col_name datatype ADD new_col_name_2 datatype;`
   3. e.g., ALTER TABLE customer ADD age INT NOT NULL;
2. `MODIFY`
   1. Change datatype of an attribute.
   2.  `ALTER TABLE table-name MODIFY col-name col-datatype;`
   3.  E.g., VARCHAR TO CHAR
    ALTER TABLE customer MODIFY name CHAR(1024);
3. `CHANGE COLUMN`
   1. Rename column name.
   2. `ALTER TABLE table-name CHANGE COLUMN old-col-name new-col-name new-col-datatype;`
   3. e.g., ALTER TABLE customer CHANGE COLUMN name customer-name VARCHAR(1024);
4. `DROP COLUMN`
   1. Drop a column completely.
   2. `ALTER TABLE table-name DROP COLUMN col-name;`
   3. e.g., ALTER TABLE customer DROP COLUMN middle-name;
5. `RENAME`
   1.  Rename table name itself.
   2.  `ALTER TABLE table-name RENAME TO new-table-name;`
   3.  e.g., ALTER TABLE customer RENAME TO customer-details;

---

### DATA MANIPULATION LANGUAGE (DML)
1. `INSERT`
   1. `INSERT INTO table-name(col1, col2, col3) VALUES (v1, v2, v3), (val1, val2, val3);`
2. `UPDATE`
   1. `UPDATE table-name SET col1 = 1, col2 = ‘abc’ WHERE id = 1;`
   2. Update multiple rows e.g.,
      1. UPDATE student SET standard = standard + 1;
   3. ON UPDATE CASCADE
      1. Can be added to the table while creating constraints. Suppose there is a situation where we have two tables such that primary key of one table is the foreign key for another table. if we update the primary key of the first table then using the ON UPDATE CASCADE foreign key of the second table automatically get updated.
3. `DELETE`
   1. `DELETE FROM table-name WHERE id = 1;`
   2. `DELETE FROM table-name;` //all rows will be deleted.
   3. `DELETE CASCADE` - (to overcome DELETE constraint of Referential constraints)
    What would happen to child entry if parent table’s entry is deleted?
    ```sql
    CREATE TABLE ORDER (
    order_id int PRIMARY KEY,
    delivery_date DATE,
    cust_id INT
    FOREIGN KEY(cust_id) REFERENCES customer(id) ON DELETE CASCADE
        );
    ```
   4. `ON DELETE NULL` - (can FK have null values?)
   ```sql
   CREATE TABLE ORDER (
   order_id int PRIMARY KEY,
   delivery_date DATE,
   cust_id INT,
   FOREIGN KEY(cust_id) REFERENCES customer(id) ON DELETE SET NULL
   );
   ```
4. `REPLACE`
   1. Primarily used for already present tuple in a table.
   2. As UPDATE, using REPLACE with the help of WHERE clause in PK, then that row will be replaced.
   3. As INSERT, if there is no duplicate data new tuple will be inserted.
   4. `REPLACE INTO student (id, class) VALUES(4, 3);`
   5. `REPLACE INTO table SET col1 = val1, col2 = val2;`

---
