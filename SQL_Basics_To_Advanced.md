# SQL Basics To Advanced

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

-- Insert multiple rows
INSERT INTO students (name, age) VALUES
    ('Priya', 22),
    ('Amit', 20),
    ('Neha', 21);
```

### UPDATE (Modify Data)
```sql
UPDATE students
SET age = 23
WHERE name = 'Asha';

-- Update multiple columns
UPDATE students
SET age = 24, name = 'Asha Sharma'
WHERE name = 'Asha';
```

### DELETE (Remove Data)
```sql
DELETE FROM students
WHERE name = 'Rahul';

-- Delete with condition
DELETE FROM students
WHERE age < 18;
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

SELECT * FROM students
WHERE department = 'CS' OR department = 'IT';

SELECT * FROM students
WHERE NOT age < 18;
```

### Other Important Operators

#### LIKE (Pattern Matching)
```sql
SELECT * FROM students
WHERE name LIKE 'A%';  -- Names starting with 'A'

SELECT * FROM students
WHERE name LIKE '%kumar';  -- Names ending with 'kumar'

SELECT * FROM students
WHERE name LIKE '%sh%';  -- Names containing 'sh'

SELECT * FROM students
WHERE name LIKE 'R___l';  -- _ matches exactly one character
```

#### IN (Multiple Values)
```sql
SELECT * FROM students
WHERE age IN (20, 21, 22);

SELECT * FROM students
WHERE department IN ('CS', 'IT', 'ECE');
```

#### BETWEEN (Range)
```sql
SELECT * FROM students
WHERE age BETWEEN 18 AND 25;

SELECT * FROM students
WHERE hire_date BETWEEN '2023-01-01' AND '2023-12-31';
```

#### IS NULL / IS NOT NULL
```sql
SELECT * FROM students
WHERE email IS NULL;

SELECT * FROM students
WHERE phone IS NOT NULL;
```

---

## 5. Sorting and Limiting

### ORDER BY
```sql
SELECT * FROM students
ORDER BY age ASC;  -- Ascending (default)

SELECT * FROM students
ORDER BY age DESC;  -- Descending
```

#### Multiple Columns
```sql
SELECT * FROM students
ORDER BY age DESC, name ASC;  -- Sort by age first, then by name
```

### LIMIT and OFFSET (Pagination)
```sql
SELECT * FROM students
LIMIT 10;  -- First 10 records

SELECT * FROM students
LIMIT 10 OFFSET 20;  -- Skip first 20, get next 10 (records 21-30)

-- Alternative syntax (MySQL)
SELECT * FROM students
LIMIT 20, 10;  -- LIMIT offset, count
```

### DISTINCT (Unique Values)
```sql
SELECT DISTINCT age FROM students;  -- Unique ages only

SELECT DISTINCT department, location FROM employees;  -- Unique combinations
```

---

## 6. Aliases
Use `AS` to rename columns or tables for readability:

```sql
-- Column aliases
SELECT name AS student_name, age AS student_age
FROM students;

-- Table aliases (useful in JOINs)
SELECT s.name, s.age
FROM students AS s
WHERE s.age > 20;

-- AS is optional
SELECT name student_name, age student_age
FROM students s;
```

---

## 7. Aggregate Functions
```sql
SELECT COUNT(*) FROM students;  -- Count all rows
SELECT COUNT(email) FROM students;  -- Count non-null emails
SELECT AVG(age) FROM students;  -- Average age
SELECT SUM(salary) FROM employees;  -- Total sum
SELECT MIN(age) FROM students;  -- Minimum age
SELECT MAX(age) FROM students;  -- Maximum age
```

### Combine Aggregates
```sql
SELECT 
    COUNT(*) AS total_students,
    AVG(age) AS average_age,
    MIN(age) AS youngest,
    MAX(age) AS oldest
FROM students;
```

---

## 8. GROUP BY and HAVING

### GROUP BY
Groups rows with the same values:

```sql
SELECT age, COUNT(*) AS count
FROM students
GROUP BY age;

SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department;
```

### Multiple Column Grouping
```sql
SELECT department, location, COUNT(*) AS employee_count
FROM employees
GROUP BY department, location;
```

### HAVING (Filter Groups)
`HAVING` filters grouped data (use after `GROUP BY`):

```sql
SELECT age, COUNT(*) AS count
FROM students
GROUP BY age
HAVING COUNT(*) > 5;  -- Only ages with more than 5 students
```

### WHERE vs HAVING
- **WHERE** filters rows **before** grouping
- **HAVING** filters groups **after** grouping

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
WHERE salary > 30000  -- Filter individual employees first
GROUP BY department
HAVING AVG(salary) > 50000;  -- Then filter departments
```

---

## 9. Joins (Multi-Table Queries)

### INNER JOIN
Returns only matching rows from both tables:

```sql
SELECT students.name, marks.score
FROM students
INNER JOIN marks
ON students.id = marks.student_id;

-- With aliases (cleaner)
SELECT s.name, m.score
FROM students s
INNER JOIN marks m
ON s.id = m.student_id;
```

### LEFT JOIN (LEFT OUTER JOIN)
Returns all rows from left table + matching rows from right (NULL if no match):

```sql
SELECT s.name, m.score
FROM students s
LEFT JOIN marks m
ON s.id = m.student_id;
-- Shows all students, even those without marks
```

### RIGHT JOIN (RIGHT OUTER JOIN)
Returns all rows from right table + matching rows from left:

```sql
SELECT s.name, m.score
FROM students s
RIGHT JOIN marks m
ON s.id = m.student_id;
-- Shows all marks, even if student deleted
```

### FULL OUTER JOIN
Returns all rows from both tables (MySQL doesn't support this directly):

```sql
-- PostgreSQL syntax
SELECT s.name, m.score
FROM students s
FULL OUTER JOIN marks m
ON s.id = m.student_id;

-- MySQL alternative using UNION
SELECT s.name, m.score FROM students s LEFT JOIN marks m ON s.id = m.student_id
UNION
SELECT s.name, m.score FROM students s RIGHT JOIN marks m ON s.id = m.student_id;
```

### Multiple Joins
```sql
SELECT s.name, c.course_name, e.grade
FROM students s
INNER JOIN enrollments e ON s.id = e.student_id
INNER JOIN courses c ON e.course_id = c.id;
```

---

## 10. Subqueries
A query nested inside another query:

### Subquery in WHERE
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### Subquery with IN
```sql
SELECT name FROM students
WHERE id IN (
    SELECT student_id FROM marks WHERE score > 90
);
```

### Subquery in FROM
```sql
SELECT * FROM (
    SELECT name, age 
    FROM students 
    WHERE age > 20
) AS older_students
WHERE name LIKE 'A%';
```

### Subquery in SELECT
```sql
SELECT 
    name,
    salary,
    (SELECT AVG(salary) FROM employees) AS avg_salary,
    salary - (SELECT AVG(salary) FROM employees) AS difference
FROM employees;
```

### EXISTS (Check if Subquery Returns Results)
```sql
SELECT name FROM students s
WHERE EXISTS (
    SELECT 1 FROM enrollments e 
    WHERE e.student_id = s.id
);
```

---

## 11. Creating Tables

### Basic Table
```sql
CREATE TABLE students (
    id INT,
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
    department VARCHAR(50) DEFAULT 'Undeclared',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Common Constraints
- **PRIMARY KEY** - Unique identifier, cannot be NULL
- **FOREIGN KEY** - Links to another table, maintains referential integrity
- **NOT NULL** - Column must have a value
- **UNIQUE** - All values must be different
- **DEFAULT** - Default value if none provided
- **CHECK** - Custom validation rule
- **AUTO_INCREMENT** - Automatically generates sequential numbers

### Foreign Key Example
```sql
CREATE TABLE marks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT NOT NULL,
    subject VARCHAR(50),
    score INT CHECK (score BETWEEN 0 AND 100),
    FOREIGN KEY (student_id) REFERENCES students(id)
        ON DELETE CASCADE  -- Delete marks if student deleted
        ON UPDATE CASCADE  -- Update if student id changes
);
```

### Modify Existing Tables
```sql
-- Add column
ALTER TABLE students ADD COLUMN phone VARCHAR(15);

-- Drop column
ALTER TABLE students DROP COLUMN phone;

-- Modify column
ALTER TABLE students MODIFY COLUMN age INT NOT NULL;

-- Rename column
ALTER TABLE students RENAME COLUMN name TO full_name;

-- Add constraint
ALTER TABLE students ADD CONSTRAINT chk_age CHECK (age >= 18);

-- Drop table
DROP TABLE IF EXISTS students;
```

---

## 12. Views
A saved query that acts like a virtual table:

### Create View
```sql
CREATE VIEW engineering_students AS
SELECT id, name, age
FROM students
WHERE department = 'Engineering';

-- Use the view
SELECT * FROM engineering_students;
```

### Complex View with JOINs
```sql
CREATE VIEW student_performance AS
SELECT 
    s.name,
    s.department,
    AVG(m.score) AS avg_score,
    COUNT(m.id) AS total_exams
FROM students s
LEFT JOIN marks m ON s.id = m.student_id
GROUP BY s.id, s.name, s.department;

-- Query the view
SELECT * FROM student_performance
WHERE avg_score > 75;
```

### Manage Views
```sql
-- Update view
CREATE OR REPLACE VIEW engineering_students AS
SELECT id, name, age, email
FROM students
WHERE department = 'Engineering';

-- Drop view
DROP VIEW IF EXISTS engineering_students;
```

---

## 13. Transactions
Ensure data integrity with ACID properties:

### Basic Transaction
```sql
START TRANSACTION;  -- or BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;  -- Save changes
-- or ROLLBACK;  -- Undo changes if something went wrong
```

### Transaction with Error Handling
```sql
START TRANSACTION;

UPDATE inventory SET quantity = quantity - 5 WHERE product_id = 101;

-- Check if update was successful
SELECT quantity FROM inventory WHERE product_id = 101;

-- If quantity is negative, rollback
ROLLBACK;

-- If everything is good
COMMIT;
```

### Savepoints
```sql
START TRANSACTION;

INSERT INTO orders (customer_id, total) VALUES (1, 500);
SAVEPOINT order_created;

INSERT INTO order_items (order_id, product, quantity) VALUES (1, 'Laptop', 1);
SAVEPOINT items_added;

-- Oops, wrong quantity!
ROLLBACK TO items_added;  -- Go back to after order creation

INSERT INTO order_items (order_id, product, quantity) VALUES (1, 'Laptop', 2);

COMMIT;
```

---

## 14. Indexes
Speed up query performance on large tables:

### Why Use Indexes?
- **Without index**: Database scans every row (slow on large tables)
- **With index**: Database uses optimized data structure (fast lookups)

### Create Index
```sql
-- Single column index
CREATE INDEX idx_student_name ON students(name);

-- Multi-column index
CREATE INDEX idx_dept_salary ON employees(department, salary);

-- Unique index (ensures uniqueness + faster lookup)
CREATE UNIQUE INDEX idx_email ON students(email);
```

### Show Indexes
```sql
SHOW INDEXES FROM students;

-- or
SHOW INDEX FROM students;
```

### Drop Index
```sql
DROP INDEX idx_student_name ON students;
```

### When to Use Indexes
✅ **Use indexes on:**
- Primary keys (automatic)
- Foreign keys
- Columns in WHERE clauses
- Columns in JOIN conditions
- Columns in ORDER BY

❌ **Avoid indexes on:**
- Small tables (overhead not worth it)
- Columns with low cardinality (few unique values like gender)
- Frequently updated columns (slows down INSERT/UPDATE)

### Example: Performance Impact
```sql
-- Slow without index (scans 1 million rows)
SELECT * FROM employees WHERE email = 'john@example.com';

-- Fast with index (direct lookup)
CREATE INDEX idx_email ON employees(email);
SELECT * FROM employees WHERE email = 'john@example.com';
```

---

## 15. SQL Query Execution Order
Understanding the order helps write better queries:

```
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT
```

### Detailed Example
```sql
SELECT department, AVG(salary) AS avg_salary    -- 5. Select columns & calculate
FROM employees                                   -- 1. Get data from table
INNER JOIN departments ON ...                    -- 2. Join with other tables
WHERE salary > 30000                            -- 3. Filter individual rows
GROUP BY department                             -- 4. Group rows
HAVING AVG(salary) > 45000                      -- 6. Filter groups
ORDER BY avg_salary DESC                        -- 7. Sort results
LIMIT 5;                                        -- 8. Limit output
```

### Why This Matters
```sql
-- ❌ WRONG: Can't use alias in WHERE
SELECT name, salary * 12 AS annual_salary
FROM employees
WHERE annual_salary > 100000;  -- Error! annual_salary doesn't exist yet

-- ✅ CORRECT: Use original expression or subquery
SELECT name, salary * 12 AS annual_salary
FROM employees
WHERE salary * 12 > 100000;

-- ✅ ALSO CORRECT: Can use alias in HAVING
SELECT department, AVG(salary) AS avg_sal
FROM employees
GROUP BY department
HAVING avg_sal > 50000;  -- Works! Alias exists after SELECT
```

---

## 16. Common Mistakes to Avoid

### 1. Using SELECT *
```sql
-- ❌ Bad: Fetches all columns (slow, wastes bandwidth)
SELECT * FROM employees;

-- ✅ Good: Specify only needed columns
SELECT name, salary, department FROM employees;
```

### 2. Forgetting WHERE in UPDATE/DELETE
```sql
-- ❌ DANGER: Updates ALL rows!
UPDATE employees SET salary = 100000;

-- ✅ Safe: Always use WHERE
UPDATE employees SET salary = 100000 WHERE id = 5;

-- ⚠️ Best Practice: Check first!
SELECT * FROM employees WHERE id = 5;  -- Verify what will be updated
UPDATE employees SET salary = 100000 WHERE id = 5;
```

### 3. Not Using Indexes
```sql
-- Slow on million-row table
SELECT * FROM orders WHERE customer_email = 'user@example.com';

-- Create index for frequently searched columns
CREATE INDEX idx_customer_email ON orders(customer_email);
```

### 4. Using HAVING Instead of WHERE
```sql
-- ❌ Slow: Filters after grouping (processes all rows first)
SELECT department, COUNT(*) 
FROM employees
GROUP BY department
HAVING salary > 50000;

-- ✅ Fast: Filters before grouping (processes fewer rows)
SELECT department, COUNT(*)
FROM employees
WHERE salary > 50000
GROUP BY department;
```

### 5. N+1 Query Problem
```sql
-- ❌ Bad: Runs 1 + N queries (1 for students, N for each student's marks)
SELECT * FROM students;
-- Then for each student:
SELECT * FROM marks WHERE student_id = ?;

-- ✅ Good: Use JOIN (1 query total)
SELECT s.*, m.score
FROM students s
LEFT JOIN marks m ON s.id = m.student_id;
```

### 6. Not Handling NULL Properly
```sql
-- ❌ Wrong: NULL doesn't equal anything (not even NULL)
SELECT * FROM students WHERE email = NULL;

-- ✅ Correct: Use IS NULL
SELECT * FROM students WHERE email IS NULL;

-- ❌ Wrong: NULL in calculations gives NULL
SELECT name, salary * 12 AS annual FROM employees;  -- NULL salary = NULL annual

-- ✅ Better: Handle NULL explicitly
SELECT name, COALESCE(salary, 0) * 12 AS annual FROM employees;
```

### 7. Case Sensitivity Confusion
```sql
-- MySQL (case-insensitive by default)
SELECT * FROM students WHERE name = 'JOHN';  -- Matches 'john', 'John', 'JOHN'

-- PostgreSQL (case-sensitive)
SELECT * FROM students WHERE name = 'JOHN';  -- Only matches 'JOHN'
SELECT * FROM students WHERE LOWER(name) = 'john';  -- Case-insensitive search
```

---

## 17. Next Topics to Learn

### Beginner-Intermediate
✅ **You've learned the basics! Practice more with:**
- [LeetCode SQL](https://leetcode.com/problemset/database/) - 150+ problems
- [HackerRank SQL](https://www.hackerrank.com/domains/sql) - Structured learning path
- [SQLZoo](https://sqlzoo.net/) - Interactive tutorials

### Intermediate Topics
📚 **Ready for more? Learn these next:**

1. **Window Functions** (Advanced Analytics)
   ```sql
   -- ROW_NUMBER, RANK, DENSE_RANK
   SELECT name, salary,
          ROW_NUMBER() OVER (ORDER BY salary DESC) AS rank
   FROM employees;
   
   -- PARTITION BY (group analytics)
   SELECT department, name, salary,
          AVG(salary) OVER (PARTITION BY department) AS dept_avg
   FROM employees;
   ```

2. **CTEs (Common Table Expressions)**
   ```sql
   -- Cleaner than subqueries
   WITH high_earners AS (
       SELECT * FROM employees WHERE salary > 75000
   )
   SELECT department, COUNT(*) 
   FROM high_earners 
   GROUP BY department;
   ```

3. **Recursive Queries**
   ```sql
   -- Hierarchical data (org charts, categories)
   WITH RECURSIVE employee_hierarchy AS (
       SELECT id, name, manager_id, 1 AS level
       FROM employees WHERE manager_id IS NULL
       UNION ALL
       SELECT e.id, e.name, e.manager_id, eh.level + 1
       FROM employees e
       JOIN employee_hierarchy eh ON e.manager_id = eh.id
   )
   SELECT * FROM employee_hierarchy;
   ```

4. **Stored Procedures**
   ```sql
   -- Reusable SQL code
   DELIMITER //
   CREATE PROCEDURE GetHighEarners(IN min_salary INT)
   BEGIN
       SELECT * FROM employees WHERE salary >= min_salary;
   END //
   DELIMITER ;
   
   CALL GetHighEarners(75000);
   ```

5. **Triggers**
   ```sql
   -- Automatic actions on INSERT/UPDATE/DELETE
   CREATE TRIGGER audit_salary_changes
   AFTER UPDATE ON employees
   FOR EACH ROW
   BEGIN
       INSERT INTO salary_audit (employee_id, old_salary, new_salary, changed_at)
       VALUES (OLD.id, OLD.salary, NEW.salary, NOW());
   END;
   ```

6. **Database Normalization**
   - **1NF**: Atomic values, no repeating groups
   - **2NF**: No partial dependencies on composite keys
   - **3NF**: No transitive dependencies
   - **BCNF**: Every determinant is a candidate key

7. **Query Optimization**
   - Use `EXPLAIN` to analyze query execution
   - Optimize with proper indexes
   - Avoid `SELECT *` and `LIKE '%value%'`
   - Use `LIMIT` when possible
   - Partition large tables

8. **Database Design Patterns**
   - **One-to-Many**: User → Posts
   - **Many-to-Many**: Students ↔ Courses (requires junction table)
   - **One-to-One**: User → Profile
   - **Self-Referencing**: Employee → Manager (same table)

9. **JSON Support (Modern Databases)**
   ```sql
   -- Store and query JSON data (MySQL 5.7+, PostgreSQL)
   SELECT data->>'$.name' AS name
   FROM products
   WHERE data->>'$.category' = 'Electronics';
   ```

10. **Full-Text Search**
    ```sql
    -- Efficient text searching
    CREATE FULLTEXT INDEX idx_description ON products(description);
    SELECT * FROM products 
    WHERE MATCH(description) AGAINST('laptop gaming' IN NATURAL LANGUAGE MODE);
    ```

### Advanced Topics
🚀 **For experts:**
- **Partitioning & Sharding** - Scale to billions of rows
- **Replication** - High availability and load balancing
- **Query Profiling** - Deep performance analysis
- **Database Security** - SQL injection prevention, encryption
- **NoSQL Integration** - When to use SQL vs NoSQL
- **Data Warehousing** - OLAP, star schema, snowflake schema
- **ETL Processes** - Extract, Transform, Load

---

## 18. Quick Reference Card

### Query Template
```sql
SELECT column1, column2, aggregate_function(column3)
FROM table1
[INNER/LEFT/RIGHT] JOIN table2 ON table1.id = table2.foreign_id
WHERE condition
GROUP BY column1, column2
HAVING aggregate_condition
ORDER BY column1 [ASC|DESC]
LIMIT number OFFSET skip;
```

### Common Functions
```sql
-- String
CONCAT(str1, str2), UPPER(str), LOWER(str), SUBSTRING(str, start, length)
LENGTH(str), TRIM(str), REPLACE(str, find, replace)

-- Numeric
ROUND(num, decimals), CEIL(num), FLOOR(num), ABS(num), MOD(num, divisor)

-- Date/Time
NOW(), CURDATE(), CURTIME(), DATE_FORMAT(date, format)
DATEDIFF(date1, date2), DATE_ADD(date, INTERVAL value unit)

-- Conditional
IF(condition, true_value, false_value)
CASE WHEN condition THEN result ELSE default END
COALESCE(value1, value2, ...)  -- Returns first non-NULL
NULLIF(value1, value2)  -- Returns NULL if equal
```

### Remember
- Always use `WHERE` with `UPDATE`/`DELETE`
- Test with `SELECT` before modifying data
- Index frequently queried columns
- Use `EXPLAIN` to check query performance
- Backup before making schema changes

---

**Happy Querying! 🎯 Keep practicing and you'll master SQL in no time!**
