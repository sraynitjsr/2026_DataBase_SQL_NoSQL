# SQL Basics

## 1. What is SQL?
SQL (Structured Query Language) is used to interact with relational databases like MySQL, PostgreSQL, and SQLite.

---

## 2. Core Concepts

### Table
A table stores data in rows and columns.

### Row
A single record in a table.

### Column
A field/property of the data.

---

## 3. Basic Commands

### SELECT (Read Data)
```sql
SELECT * FROM students;
SELECT name, age FROM students;
```

### INSERT (Add Data)
```sql
INSERT INTO students (name, age)
VALUES ('Rahul', 21);
```

### UPDATE (Modify Data)
```sql
UPDATE students
SET age = 23
WHERE name = 'Asha';
```

### DELETE (Remove Data)
```sql
DELETE FROM students
WHERE name = 'Rahul';
```

---

## 4. Filtering Data

### WHERE Clause
```sql
SELECT * FROM students
WHERE age > 20;
```

### Comparison Operators
- `=`, `!=` (or `<>`), `>`, `<`, `>=`, `<=`

### Logical Operators
- `AND`, `OR`, `NOT`

```sql
SELECT * FROM students
WHERE age > 20 AND name = 'Asha';
```

### Other Important Operators

#### LIKE (Pattern Matching)
```sql
SELECT * FROM students
WHERE name LIKE 'A%';  -- Names starting with 'A'
WHERE name LIKE '%kumar';  -- Names ending with 'kumar'
WHERE name LIKE '%sh%';  -- Names containing 'sh'
```

###8. Aggregate Functions
```sql
SELECT COUNT(*) FROM students;  -- Count all rows
SELECT COUNT(email) FROM students;  -- Count non-null emails
SELECT AVG(age) FROM students;  -- Average age
SELECT SUM(age) FROM students;  -- Sum of ages
SELECT MIN(age) FROM students;  -- Minimum age
SELECT MAX(age) FROM students;  -- Maximum age
```

---

## 9. GROUP BY and HAVING

### GROUP BY
```sql
SELECT age, COUNT(*) AS count
FROM students
GROUP BY age;
```

### HAVING (Filter Groups)
`HAVING` is like `WHERE` but for grouped data:

```sql
SELECT age, COUNT(*) AS count
FROM students
GROUP BY age
HAVING COUNT(*) > 5;  -- Only ages with more than 5 students
```

**Note:** Use `WHERE` before `GROUP BY` to filter rows, and `HAVING` after to filter groups.

```sql
SELECT age, AVG(marks) AS avg_marks
FROM students
WHERE age > 18
GROUP BY age
HAVING AVG(marks) > 75;
```

---

## 10. Sorting and Limiting

### ORDER BY
```sql
SELECT * FROM students
ORDER BY age ASC;  -- Ascending
ORDER BY age DESC;  -- Descending
```

#### Multiple Columns
```sql
SELECT * FROM students
ORDER BY age DESC, name ASC;
```

### LIMIT (Pagination)
```sql
SELECT * FROM students
LIMIT 10;  -- First 10 records

SELECT * FROM students
LIMIT 10 OFFSET 20;  -- Records 21-30
```

### DISTINCT (Unique Values)
```sql
SELECT DISTINCT age FROM students;
```

---

## 6. Aliases
Use `AS` to rename columns or tables:

```sql
SELECT name AS student_name, age AS student_age
FROM students AS s;
```

---

## 7. Sorting
```sql
SELECT * FROM students
ORDER BY age ASC;
```
- `ASC` (ascending)
- `DESC` (descending)
10. Joins

### INNER JOIN
Returns only matching rows from both tables:

```sql
SELECT students.name, marks.score
FROM students
INNER JOIN marks
ON students.id = marks.student_id;
```

###12. Creating Tables

### Basic Table
```sql
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  age INT
);
```

### Table with Constraints
```sql
CREATE TABLE students (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(100) UNIQUE,
  age INT CHECK (age >= 18),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Common Constraints
- `PRIMARY KEY` - Unique identifier for each row
- `FOREIGN KEY` - Links to another table
- `NOT NULL` - Column cannot be empty
- `UNIQUE` - All values must be unique
- `DEFAULT` - Default value if none provided
- `CHECK` - Custom validation rule
- `AUTO_INCREMENT` - Automatically increments value

### Foreign Key Example
```sql
CREATE TABLE marks (
  id3. Complete Example
```sql
-- Create table with constraints
CREATE TABLE employees (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  department VARCHAR(50),
  salary INT CHECK (salary > 0),
  hire_date DATE DEFAULT CURRENT_DATE
);

-- Insert multiple records
INSERT INTO employees (name, department, salary) VALUES
  ('Ankit', 'Engineering', 50000),
  ('Priya', 'Marketing', 45000),
  ('4. Practice Tools
- [DB Fiddle](https://www.db-fiddle.com/) - Test SQL in multiple databases
- [SQLite Online](https://sqliteonline.com/) - Browser-based SQLite
- [LeetCode](https://leetcode.com/) - SQL practice problems
- [HackerRank SQL](https://www.hackerrank.com/domains/sql) - Interactive SQL challenges
- [SQLZoo](https://sqlzoo.net/) - Interactive SQL tutorials

---

## 15. SQL Query Execution Order
Understanding the order helps write better queries:

```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

Example:
```sql
SELECT department, AVG(salary) AS avg_salary    -- 5. Select columns
FROM employees                                   -- 1. Get data from table
WHERE salary > 30000                            -- 2. Filter rows
GROUP BY department                             -- 3. Group by department
HAVING AVG(salary) > 45000                      -- 4. Filter groups
ORDER BY avg_salary DESC                        -- 6. Sort results
LIMIT 5;                                        -- 7. Limit results
```

---

## 16. Common Mistakes to Avoid
1. **Using `SELECT *`** - Specify columns for better performance
2. **Forgetting WHERE in UPDATE/DELETE** - Can modify/delete all rows!
3. **Not using indexes** - Slow queries on large tables
4. **Using HAVING instead of WHERE** - Use WHERE to filter before grouping
5. **Case sensitivity** - Varies by database (MySQL is case-insensitive, PostgreSQL is case-sensitive)

---

## 17. Next Topics to Learn
- **Indexes** - Speed up queries
- **Views** - Saved queries
- **Stored Procedures** - Reusable SQL code
- **Triggers** - Automatic actions on events
- **Transactions** - COMMIT, ROLLBACK for data integrity
- **Normalization** - Database design best practices
- **Window Functions** - Advanced analytics (ROW_NUMBER, RANK, etc.)
- **CTEs (Common Table Expressions)** - WITH clause for complex queries
- **Performance Optimization** - Query optimization technique

## 14

## 13ns all rows from right table, matching rows from left (or NULL):

```sql
SELECT students.name, marks.score
FROM students
RIGHT JOIN marks
ON students.id = marks.student_id;
```

### FULL OUTER JOIN
Returns all rows from both tables (not supported in MySQL, use UNION):

```sql
SELECT students.name, marks.score
FROM students
FULL OUTER JOIN marks
ON students.id = marks.student_id;
```

---

## 11. Subqueries
A query inside another query:

### Subquery in WHERE
```sql
SELECT name FROM students
WHERE age > (SELECT AVG(age) FROM students);
```

### Subquery in FROM
```sql
SELECT * FROM (
  SELECT name, age FROM students WHERE age > 20
) AS older_students;
```

### Subquery with IN
```sql
SELECT name FROM students
WHERE id IN (SELECT student_id FROM marks WHERE score > 90);
```

---

## 12

## 7. GROUP BY
```sql
SELECT age, COUNT(*)
FROM students
GROUP BY age;
```

---

## 8. Joins

### INNER JOIN
```sql
SELECT students.name, marks.score
FROM students
INNER JOIN marks
ON students.id = marks.student_id;
```

---

## 9. Creating Tables
```sql
CREATE TABLE students (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  age INT
);
```

---

## 10. Example
```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  name VARCHAR(100),
  salary INT
);

INSERT INTO employees VALUES (1, 'Ankit', 50000);

SELECT * FROM employees WHERE salary > 40000;
```

---

## 11. Practice Tools
- [DB Fiddle](https://www.db-fiddle.com/)
- [SQLite Online](https://sqliteonline.com/)
- [LeetCode](https://leetcode.com/) (SQL problems)

---

## 12. Next Topics
- Indexes
- Normalization
- Transactions
- Advanced Joins
- Window Functions
