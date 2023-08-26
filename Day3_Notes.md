### Order of Execution
* How to read execution plan in SQL Server right to left?
* To read the SQL Execution Plan correctly, you should know first that the flow of the execution is starting from the right to the left and from the top to the bottom, with the last operator at the left, which is the SELECT operator in most queries, contains the final result of the query.


### JOINING TABLES
![](2023-08-25-20-51-47.png)
* All RDBMS are relational in nature, we refer to other tables to get meaningful outcomes.
* FK are used to do reference to other table.
1. `INNER JOIN`
   1. Returns a resultant table that has matching values from both the tables or all the tables.
   ```sql
   SELECT column-list FROM table1 INNER JOIN table2 ON condition1
   INNER JOIN table3 ON condition2
   …;
   ```
2. `Alias in MySQL (AS)`
   2. Aliases in MySQL is used to give a temporary name to a table or a column in a table for the purpose of
   a particular query. It works as a nickname for expressing the tables or column names. It makes the query short
   and neat.
   ```sql
   SELECT col_name AS alias_name FROM table_name;
   SELECT col_name1, col_name2,... FROM table_name AS alias_name;
   ```

3. `OUTER JOIN`
   1. `LEFT JOIN`
      1. This returns a resulting table that all the data from left table and the matched data from the right table.
      2. SELECT columns FROM table LEFT JOIN table2 ON Join_Condition;
   2. `RIGHT JOIN`
      1. This returns a resulting table that all the data from right table and the matched data from the left table.
      2.  SELECT columns FROM table RIGHT JOIN table2 ON join_cond;
   3.  `FULL JOIN`
       1.  This returns a resulting table that contains all data when there is a match on left or right table data.
       2.  Emulated in MySQL using LEFT and RIGHT JOIN.
       3.  LEFT JOIN UNION RIGHT JOIN.
       4.  SELECT columns FROM table1 as t1 LEFT JOIN table2 as t2 ON t1.id = t2.id
4. `UNION`
    SELECT columns FROM table1 as t1 RIGHT JOIN table2 as t2 ON t1.id = t2.id;
    ![](2023-08-25-20-59-20.png)
    ![](2023-08-25-20-59-46.png)


5.  `UNION ALL`
    Can also be used this will duplicate values as well while UNION gives unique values.
    ![](2023-08-25-21-00-20.png)
   
6. `CROSS JOIN`
   1. This returns all the cartesian products of the data present in both tables. Hence, all possible variations
   are reflected in the output.
   2. Used rarely in practical purpose.
   3. Table-1 has 10 rows and table-2 has 5, then resultant would have 50 rows.
   4. SELECT column-lists FROM table1 CROSS JOIN table2;
   
7. `SELF JOIN`
   1. It is used to get the output from a particular table when the same table is joined to itself.
   2. Used very less.
   3. Emulated using INNER JOIN.
   4. SELECT columns FROM table as t1 INNER JOIN table as t2 ON t1.id = t2.id;
   ![](2023-08-25-20-54-19.png)
   ![](2023-08-25-20-55-31.png)
   
8. `Join without using join keywords.`
   1. SELECT * FROM table1, table2 WHERE condition;
   2. e.g., SELECT artist_name, album_name, year_recordedFROM artist, albumWHERE artist.id = album.artist_id

---

#### Discuss a common pitfall with using JOIN
* There are different types of JOINs. We may need to implement a particular type of JOIN to answer our question correctly. As you're already aware, if you don't specify the conditional expression, your SQL query will not fail. It will return a CROSS JOIN. If you go on to perform your analysis on the CROSS JOIN, you'll get an incorrect result. You'll also get incorrect results when you don't join your tables with the appropriate conditional expressions.

* You'll also get incorrect results if you incorrectly filter your JOIN with the WHERE clause. Incorrectly filtering can result in an unintended type of JOIN. For example, a LEFT JOIN can be transformed to an INNER JOIN with an incorrect WHERE clause.

```sql
-- LEFT OUTER JOIN of the students and student_contact

SELECT *
FROM students
LEFT JOIN student_contact
    ON students.student_id = student_contact.student_id;
```

* Logical errors are introduced into our program this way. Logical errors do not return error messages, making them difficult to detect — especially when we're working with large tables.

---

```sql
-- create a students table:

CREATE TABLE students (
    student_id INTEGER PRIMARY KEY,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL
);

INSERT INTO students VALUES (1, "Mary", "Wilson");
INSERT INTO students VALUES (2, "Tim", "Ben");
INSERT INTO students VALUES (3, "Alice", "Robinson");
INSERT INTO students VALUES (4, "Reece", "Bells");

-- create a student_contact table:

CREATE TABLE student_contact (
    student_id INTEGER PRIMARY KEY,
    email_address TEXT NOT NULL,
    FOREIGN KEY (student_id) REFERENCES students(student_id)
);

INSERT INTO student_contact VALUES (1, "mary.wilson@school.edu");
INSERT INTO student_contact VALUES (2, "tim.ben@school.edu");
INSERT INTO student_contact VALUES (3, "alice.robinson@school.edu");

-- -----------------------------------------------------------------------------------------------------
-- create a staff table:

CREATE TABLE staff (
    staff_id INTEGER PRIMARY KEY,
    first_name TEXT NOT NULL,
    last_name TEXT NOT NULL
);

INSERT INTO staff VALUES (1, "Ada", "Lovelace");
INSERT INTO staff VALUES (2, "Adam ", "Smith");
INSERT INTO staff VALUES (3, "Nikolo", "Tesla");
INSERT INTO staff VALUES (4, "Ada", "Tesla");

-- create a staff_contact table:

CREATE TABLE staff_contact (
    staff_id INTEGER PRIMARY KEY,
    email_address TEXT NOT NULL,
    FOREIGN KEY (staff_id) REFERENCES staff(staff_id)
);

INSERT INTO staff_contact VALUES (1, "ada.lovelace@school.edu");
INSERT INTO staff_contact VALUES (2, "adam.smith@school.edu");
INSERT INTO staff_contact VALUES (3, "nikolo.tesla@school.edu");
INSERT INTO staff_contact VALUES (4, "ada.tesla@school.edu");
-- ----------------------------------------------------------------------------------------------------
CREATE TABLE ashi (
    student_id INTEGER PRIMARY KEY,
    email_address TEXT NOT NULL,
    FOREIGN KEY (student_id) REFERENCES students(student_id)
);

INSERT INTO ashi VALUES (1, "mary.wilson@school.edu");
INSERT INTO ashi VALUES (2, "tim.ben@school.edu");
INSERT INTO ashi VALUES (3, "alice.robinson@school.edu");
-- -----------------------------------------------------------------------------------------------------

SELECT * FROM staff_contact;
SELECT * FROM student_contact;

-- -----------------------------------------------------------------------------------------------------
-- DEFAULT CARTESIAN JOIN : y + z columns and m x n rows.
SELECT * FROM staff_contact, student_contact;
SELECT * FROM student_contact, staff_contact;

-- -----------------------------------------------------------------------------------------------------
-- INNER JOIN without conditional expression
SELECT * FROM staff_contact INNER JOIN student_contact;
SELECT * FROM student_contact INNER JOIN staff_contact ORDER BY staff_id LIMIT 5;

-- -----------------------------------------------------------------------------------------------------
-- LEFT OUTER JOIN

SELECT *
FROM student_contact as A
LEFT OUTER JOIN staff_contact as B
ON A.student_id = B.staff_id;

-- -----------------------------------------------------------------------------------------------------
-- RIGHT OUTER JOIN

SELECT *
FROM student_contact as A
RIGHT OUTER JOIN staff_contact as B
ON A.student_id = B.staff_id;

-- -----------------------------------------------------------------------------------------------------
-- FULL OUTER JOIN

SELECT *
FROM student_contact
FULL JOIN staff_contact;

SELECT *
FROM student_contact
FULL JOIN staff_contact
ON student_contact.student_id = staff_contact.staff_id;

-- --------------------------------------------------------------------------------------------------------
-- NATURAL JOIN has in-built equality conditional

SELECT *
FROM student_contact
NATURAL JOIN staff_contact;

SELECT *
FROM student_contact
NATURAL JOIN ashi;

-- --------------------------------------------------------------------------------------------------------
-- Repeated Columns : INNER JOIN of the students and student_contact with USING

SELECT *
FROM students
INNER JOIN student_contact;

SELECT *
FROM students
INNER JOIN student_contact
    USING (student_id);

-- NATURAL JOIN instead of `INNER JOIN ... USING`

SELECT *
FROM students
NATURAL JOIN student_contact;

-- INNER JOIN list column names in SELECT and using ALIASES

SELECT
    s.student_id,
    s.first_name,
    s.last_name,
    sc.email_address
FROM students AS s
INNER JOIN student_contact AS sc
    ON s.student_id = sc.student_id;
    
-- --------------------------------------------------------------------------
-- Nested Joins

SELECT *
FROM students
LEFT JOIN student_contact
    USING (student_id)
LEFT JOIN ashi
    USING (student_id);
    
-- ------------------------------------------------------------------------
-- Joins + Filtering
SELECT *
FROM students
LEFT JOIN student_contact
    USING (student_id)
WHERE student_contact.student_id IS NULL;

-- --------------------------------------------------------------------------
-- Display Columns neatly

SELECT students.first_name || " " || students.last_name AS students_name
FROM students
LEFT JOIN student_contact
    USING (student_id)
LEFT JOIN ashi
    USING (student_id);


```



---

Q. Return the email, first_name, and last_name columns on the customer table for the subset where the genre is Jazz. We want to connect the customer table to the genre table. The schema will be helpful here. In the schema, we'll need to join the customer, invoice, invoice_line, track, and genre tables to get the information we want.

```sql
SELECT
    DISTINCT customer.email,
    customer.first_name || " " || customer.last_name AS full_name
FROM customer
INNER JOIN genre
    ON genre.genre_id = track.genre_id
WHERE genre.name = 'Jazz'
ORDER BY 1;
```

---
---

1. **Question**: List the users who have made more than 100 posts.

   **Solution**:

   ```sql

   SELECT user_id, COUNT(*) as post_count

   FROM posts

   GROUP BY user_id

   HAVING post_count > 100;

   ```

 

2. **Question**: Calculate the total number of reactions (likes, hearts, etc.) for each post.

   **Solution**:

   ```sql

   SELECT p.post_id, p.title, COUNT(r.reaction_id) as total_reactions

   FROM posts p

   LEFT JOIN reactions r ON p.post_id = r.post_id

   GROUP BY p.post_id, p.title;

   ```

 

4. **Question**: Determine the average number of comments per post for each user.

   **Solution**:

   ```sql

   SELECT p.user_id, AVG(c.comment_count) as avg_comments_per_post

   FROM (

       SELECT user_id, post_id, COUNT(*) as comment_count

       FROM comments

       GROUP BY user_id, post_id

   ) c

   JOIN posts p 
   
   ON c.post_id = p.post_id

   GROUP BY p.user_id;

   ```

 

5. **Question**: List the users who have accepted the most friend requests.

   **Solution**:

   ```sql

   SELECT user_id, COUNT(*) as accepted_requests

   FROM friend_requests

   WHERE status = 'accepted'

   GROUP BY user_id

   ORDER BY accepted_requests DESC

   LIMIT 10;

   ```





### Question Practice Link : https://www.w3resource.com/sql-exercises/sql-joins-exercises.php