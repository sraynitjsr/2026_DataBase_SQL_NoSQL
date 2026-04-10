# SQL Practice Questions with Answers

## How to Use This Guide
1. Go to [DB Fiddle](https://www.db-fiddle.com/) or [SQLite Online](https://sqliteonline.com/)
2. Copy the **Setup Code** for each section
3. Try solving the questions yourself first
4. Check your answer against the solution
5. Study the explanation to understand why it works

---

## Section 1: Basic SELECT and WHERE

### Setup Code
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary INT,
    hire_date DATE
);

INSERT INTO employees VALUES
    (1, 'Rahul Kumar', 'Engineering', 75000, '2023-01-15'),
    (2, 'Priya Sharma', 'Marketing', 65000, '2023-03-20'),
    (3, 'Amit Patel', 'Engineering', 80000, '2022-11-10'),
    (4, 'Sneha Reddy', 'Sales', 55000, '2024-02-01'),
    (5, 'Ravi Verma', 'Engineering', 70000, '2023-06-15'),
    (6, 'Anjali Singh', 'HR', 60000, '2023-08-01'),
    (7, 'Vikram Joshi', 'Sales', 58000, '2023-09-12'),
    (8, 'Neha Gupta', 'Marketing', 62000, '2024-01-05');
```

---

### Q1: Select All Employees
**Question:** Display all information about all employees.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT * FROM employees;
```

**Explanation:** The `*` selects all columns from the table.

**Expected Output:**
```
id | name          | department   | salary | hire_date
---+---------------+--------------+--------+------------
1  | Rahul Kumar   | Engineering  | 75000  | 2023-01-15
2  | Priya Sharma  | Marketing    | 65000  | 2023-03-20
... (all 8 rows)
```
</details>

---

### Q2: Select Specific Columns
**Question:** Display only the name and salary of all employees.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, salary FROM employees;
```

**Explanation:** Specify column names separated by commas to select only those columns.

**Expected Output:**
```
name          | salary
--------------+--------
Rahul Kumar   | 75000
Priya Sharma  | 65000
... (all 8 rows)
```
</details>

---

### Q3: Filter by Department
**Question:** Find all employees who work in the Engineering department.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT * FROM employees
WHERE department = 'Engineering';
```

**Explanation:** The `WHERE` clause filters rows based on a condition.

**Expected Output:**
```
id | name         | department  | salary | hire_date
---+--------------+-------------+--------+------------
1  | Rahul Kumar  | Engineering | 75000  | 2023-01-15
3  | Amit Patel   | Engineering | 80000  | 2022-11-10
5  | Ravi Verma   | Engineering | 70000  | 2023-06-15
```
</details>

---

### Q4: Filter by Salary Range
**Question:** Find all employees with salary greater than 65,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, department, salary FROM employees
WHERE salary > 65000;
```

**Explanation:** Comparison operators (`>`, `<`, `>=`, `<=`, `=`, `!=`) filter numeric data.

**Expected Output:**
```
name         | department  | salary
-------------+-------------+--------
Rahul Kumar  | Engineering | 75000
Amit Patel   | Engineering | 80000
Ravi Verma   | Engineering | 70000
```
</details>

---

### Q5: Multiple Conditions with AND
**Question:** Find all employees in Engineering department with salary above 70,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, salary FROM employees
WHERE department = 'Engineering' AND salary > 70000;
```

**Explanation:** `AND` requires both conditions to be true.

**Expected Output:**
```
name        | salary
------------+--------
Rahul Kumar | 75000
Amit Patel  | 80000
```
</details>

---

### Q6: Multiple Conditions with OR
**Question:** Find all employees who work in Sales OR Marketing.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, department FROM employees
WHERE department = 'Sales' OR department = 'Marketing';
```

**Alternative using IN:**
```sql
SELECT name, department FROM employees
WHERE department IN ('Sales', 'Marketing');
```

**Explanation:** `OR` requires at least one condition to be true. `IN` is cleaner for multiple values.

**Expected Output:**
```
name          | department
--------------+------------
Priya Sharma  | Marketing
Sneha Reddy   | Sales
Vikram Joshi  | Sales
Neha Gupta    | Marketing
```
</details>

---

## Section 2: Pattern Matching and Sorting

### Setup Code
Use the same employees table from Section 1.

---

### Q7: Pattern Matching - Starts With
**Question:** Find all employees whose names start with 'R'.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name FROM employees
WHERE name LIKE 'R%';
```

**Explanation:** `LIKE` with `%` wildcard matches patterns. `R%` means starts with R.

**Expected Output:**
```
name
-------------
Rahul Kumar
Ravi Verma
```
</details>

---

### Q8: Pattern Matching - Contains
**Question:** Find all employees whose names contain 'Sharma'.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name FROM employees
WHERE name LIKE '%Sharma%';
```

**Explanation:** `%Sharma%` matches 'Sharma' anywhere in the string.

**Expected Output:**
```
name
-------------
Priya Sharma
```
</details>

---

### Q9: Sorting - Ascending Order
**Question:** Show all employees sorted by salary from lowest to highest.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, salary FROM employees
ORDER BY salary ASC;
```

**Explanation:** `ORDER BY` sorts results. `ASC` is ascending (lowest to highest).

**Expected Output:**
```
name          | salary
--------------+--------
Sneha Reddy   | 55000
Vikram Joshi  | 58000
Anjali Singh  | 60000
Neha Gupta    | 62000
Priya Sharma  | 65000
Ravi Verma    | 70000
Rahul Kumar   | 75000
Amit Patel    | 80000
```
</details>

---

### Q10: Sorting - Descending Order
**Question:** Show employees sorted by hire date, newest first.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, hire_date FROM employees
ORDER BY hire_date DESC;
```

**Explanation:** `DESC` is descending order (newest/highest first).

**Expected Output:**
```
name          | hire_date
--------------+------------
Sneha Reddy   | 2024-02-01
Neha Gupta    | 2024-01-05
Vikram Joshi  | 2023-09-12
... (oldest last)
```
</details>

---

### Q11: Limiting Results
**Question:** Show the top 3 highest-paid employees.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, salary FROM employees
ORDER BY salary DESC
LIMIT 3;
```

**Explanation:** Combine `ORDER BY DESC` with `LIMIT` to get top results.

**Expected Output:**
```
name        | salary
------------+--------
Amit Patel  | 80000
Rahul Kumar | 75000
Ravi Verma  | 70000
```
</details>

---

## Section 3: Aggregate Functions

### Setup Code
Use the same employees table from Section 1.

---

### Q12: Count All Employees
**Question:** How many total employees are there?

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT COUNT(*) AS total_employees FROM employees;
```

**Explanation:** `COUNT(*)` counts all rows. `AS` creates a column alias.

**Expected Output:**
```
total_employees
----------------
8
```
</details>

---

### Q13: Average Salary
**Question:** What is the average salary of all employees?

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT AVG(salary) AS average_salary FROM employees;
```

**Explanation:** `AVG()` calculates the mean of numeric values.

**Expected Output:**
```
average_salary
---------------
65625
```
</details>

---

### Q14: Maximum and Minimum
**Question:** Find the highest and lowest salaries.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```

**Explanation:** `MAX()` and `MIN()` return the highest and lowest values.

**Expected Output:**
```
highest_salary | lowest_salary
----------------+---------------
80000          | 55000
```
</details>

---

### Q15: Sum of Salaries
**Question:** What is the total salary expense for the company?

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT SUM(salary) AS total_expense FROM employees;
```

**Explanation:** `SUM()` adds all numeric values.

**Expected Output:**
```
total_expense
-------------
525000
```
</details>

---

## Section 4: GROUP BY and HAVING

### Setup Code
Use the same employees table from Section 1.

---

### Q16: Count by Department
**Question:** How many employees are in each department?

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

**Explanation:** `GROUP BY` groups rows with same values. `COUNT(*)` counts within each group.

**Expected Output:**
```
department  | employee_count
------------+----------------
Engineering | 3
Marketing   | 2
Sales       | 2
HR          | 1
```
</details>

---

### Q17: Average Salary by Department
**Question:** What is the average salary in each department?

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    department, 
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
ORDER BY avg_salary DESC;
```

**Explanation:** Aggregate functions work on grouped data. Added `ORDER BY` to sort results.

**Expected Output:**
```
department  | avg_salary
------------+------------
Engineering | 75000
Marketing   | 63500
HR          | 60000
Sales       | 56500
```
</details>

---

### Q18: Filter Groups with HAVING
**Question:** Show departments with average salary greater than 60,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    department, 
    AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 60000;
```

**Explanation:** `HAVING` filters groups (after `GROUP BY`). `WHERE` filters rows (before `GROUP BY`).

**Expected Output:**
```
department  | avg_salary
------------+------------
Engineering | 75000
Marketing   | 63500
```
</details>

---

### Q19: Complex Grouping
**Question:** Show departments with more than 1 employee and their total salary expense.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    department,
    COUNT(*) AS employee_count,
    SUM(salary) AS total_expense
FROM employees
GROUP BY department
HAVING COUNT(*) > 1
ORDER BY total_expense DESC;
```

**Explanation:** Multiple aggregates and `HAVING` with `COUNT()` to filter groups.

**Expected Output:**
```
department  | employee_count | total_expense
------------+----------------+--------------
Engineering | 3              | 225000
Marketing   | 2              | 127000
Sales       | 2              | 113000
```
</details>

---

## Section 5: JOINS (Multi-Table Queries)

### Setup Code
```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    salary INT
);

CREATE TABLE departments (
    id INT PRIMARY KEY,
    dept_name VARCHAR(50),
    location VARCHAR(50)
);

INSERT INTO employees VALUES
    (1, 'Rahul Kumar', 1, 75000),
    (2, 'Priya Sharma', 2, 65000),
    (3, 'Amit Patel', 1, 80000),
    (4, 'Sneha Reddy', 3, 55000),
    (5, 'Ravi Verma', 1, 70000),
    (6, 'Anjali Singh', NULL, 60000);  -- No department assigned

INSERT INTO departments VALUES
    (1, 'Engineering', 'Bangalore'),
    (2, 'Marketing', 'Mumbai'),
    (3, 'Sales', 'Delhi'),
    (4, 'Finance', 'Pune');  -- No employees yet
```

---

### Q20: INNER JOIN
**Question:** Show employee names with their department names (only employees who have departments).

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    employees.name,
    departments.dept_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.id;
```

**Alternative with aliases:**
```sql
SELECT 
    e.name,
    d.dept_name
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id;
```

**Explanation:** `INNER JOIN` returns only matching rows from both tables.

**Expected Output:**
```
name          | dept_name
--------------+-------------
Rahul Kumar   | Engineering
Priya Sharma  | Marketing
Amit Patel    | Engineering
Sneha Reddy   | Sales
Ravi Verma    | Engineering
```
Note: Anjali Singh is excluded (no department)
</details>

---

### Q21: LEFT JOIN
**Question:** Show all employees with their department names (include employees without departments).

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    e.name,
    d.dept_name
FROM employees e
LEFT JOIN departments d
ON e.department_id = d.id;
```

**Explanation:** `LEFT JOIN` returns all rows from left table (employees), with NULL for non-matching right table rows.

**Expected Output:**
```
name          | dept_name
--------------+-------------
Rahul Kumar   | Engineering
Priya Sharma  | Marketing
Amit Patel    | Engineering
Sneha Reddy   | Sales
Ravi Verma    | Engineering
Anjali Singh  | NULL
```
</details>

---

### Q22: RIGHT JOIN
**Question:** Show all departments with employee counts (include departments with no employees).

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    d.dept_name,
    COUNT(e.id) AS employee_count
FROM employees e
RIGHT JOIN departments d
ON e.department_id = d.id
GROUP BY d.dept_name;
```

**Explanation:** `RIGHT JOIN` returns all departments. `COUNT(e.id)` counts non-NULL employee IDs.

**Expected Output:**
```
dept_name   | employee_count
------------+----------------
Engineering | 3
Marketing   | 1
Sales       | 1
Finance     | 0
```
</details>

---

### Q23: JOIN with WHERE
**Question:** Show employees in the Engineering department with their location.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    e.name,
    e.salary,
    d.dept_name,
    d.location
FROM employees e
INNER JOIN departments d
ON e.department_id = d.id
WHERE d.dept_name = 'Engineering';
```

**Explanation:** Combine `JOIN` with `WHERE` to filter joined results.

**Expected Output:**
```
name        | salary | dept_name   | location
------------+--------+-------------+-----------
Rahul Kumar | 75000  | Engineering | Bangalore
Amit Patel  | 80000  | Engineering | Bangalore
Ravi Verma  | 70000  | Engineering | Bangalore
```
</details>

---

### Q24: JOIN with Aggregation
**Question:** Show each department's name, location, and average employee salary.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    d.dept_name,
    d.location,
    AVG(e.salary) AS avg_salary,
    COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e
ON d.id = e.department_id
GROUP BY d.dept_name, d.location
ORDER BY avg_salary DESC;
```

**Explanation:** Join departments with employees, then group by department to calculate averages.

**Expected Output:**
```
dept_name   | location  | avg_salary | employee_count
------------+-----------+------------+----------------
Engineering | Bangalore | 75000      | 3
Marketing   | Mumbai    | 65000      | 1
Sales       | Delhi     | 55000      | 1
Finance     | Pune      | NULL       | 0
```
</details>

---

## Section 6: Subqueries

### Setup Code
Use the employees table from Section 1.

---

### Q25: Subquery in WHERE
**Question:** Find employees who earn more than the average salary.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Explanation:** Inner query calculates average first, outer query uses that value in comparison.

**Expected Output:**
```
name        | salary
------------+--------
Rahul Kumar | 75000
Amit Patel  | 80000
Ravi Verma  | 70000
```
</details>

---

### Q26: Subquery with IN
**Question:** Find employees who work in departments located in specific cities (using subquery).

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
-- Using employees and departments from Section 5
SELECT name, salary
FROM employees
WHERE department_id IN (
    SELECT id FROM departments 
    WHERE location = 'Bangalore'
);
```

**Explanation:** Subquery returns department IDs, main query finds employees in those departments.

**Expected Output:**
```
name        | salary
------------+--------
Rahul Kumar | 75000
Amit Patel  | 80000
Ravi Verma  | 70000
```
</details>

---

### Q27: Subquery in SELECT
**Question:** Show each employee's salary and the difference from average salary.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    name,
    salary,
    salary - (SELECT AVG(salary) FROM employees) AS diff_from_avg
FROM employees
ORDER BY diff_from_avg DESC;
```

**Explanation:** Subquery in SELECT calculates a value for each row.

**Expected Output:**
```
name          | salary | diff_from_avg
--------------+--------+---------------
Amit Patel    | 80000  | 14375
Rahul Kumar   | 75000  | 9375
Ravi Verma    | 70000  | 4375
Priya Sharma  | 65000  | -625
... (others below average)
```
</details>

---

## Section 7: Advanced Challenges

### Setup Code
```sql
CREATE TABLE students (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    age INT
);

CREATE TABLE courses (
    id INT PRIMARY KEY,
    course_name VARCHAR(100),
    credits INT
);

CREATE TABLE enrollments (
    id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    grade CHAR(1)
);

INSERT INTO students VALUES
    (1, 'Aarav', 20),
    (2, 'Diya', 21),
    (3, 'Arjun', 19),
    (4, 'Isha', 22),
    (5, 'Vihaan', 20);

INSERT INTO courses VALUES
    (1, 'Database Systems', 3),
    (2, 'Web Development', 4),
    (3, 'Data Structures', 3),
    (4, 'Machine Learning', 4);

INSERT INTO enrollments VALUES
    (1, 1, 1, 'A'),
    (2, 1, 2, 'B'),
    (3, 2, 1, 'A'),
    (4, 2, 3, 'A'),
    (5, 3, 2, 'C'),
    (6, 3, 3, 'B'),
    (7, 4, 1, 'B'),
    (8, 4, 4, 'A'),
    (9, 5, 2, 'A');
```

---

### Q28: Three-Table JOIN
**Question:** Show student names, course names, and their grades.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    s.name AS student_name,
    c.course_name,
    e.grade
FROM students s
INNER JOIN enrollments e ON s.id = e.student_id
INNER JOIN courses c ON e.course_id = c.id
ORDER BY s.name, c.course_name;
```

**Explanation:** Chain multiple JOINs to connect three tables.

**Expected Output:**
```
student_name | course_name        | grade
-------------+--------------------+-------
Aarav        | Database Systems   | A
Aarav        | Web Development    | B
Arjun        | Data Structures    | B
Arjun        | Web Development    | C
Diya         | Data Structures    | A
Diya         | Database Systems   | A
Isha         | Database Systems   | B
Isha         | Machine Learning   | A
Vihaan       | Web Development    | A
```
</details>

---

### Q29: Find Students with All A Grades
**Question:** Find students who got 'A' grade in all their courses.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT s.name
FROM students s
WHERE s.id NOT IN (
    SELECT student_id
    FROM enrollments
    WHERE grade != 'A'
);
```

**Alternative solution:**
```sql
SELECT s.name
FROM students s
INNER JOIN enrollments e ON s.id = e.student_id
GROUP BY s.id, s.name
HAVING MIN(e.grade) = 'A' AND MAX(e.grade) = 'A';
```

**Explanation:** First solution finds students who have no non-A grades. Second uses MIN/MAX of grades.

**Expected Output:**
```
name
------
Diya
Vihaan
```
</details>

---

### Q30: Most Popular Course
**Question:** Find the course with the most enrollments.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
SELECT 
    c.course_name,
    COUNT(e.id) AS enrollment_count
FROM courses c
LEFT JOIN enrollments e ON c.id = e.course_id
GROUP BY c.id, c.course_name
ORDER BY enrollment_count DESC
LIMIT 1;
```

**Explanation:** Count enrollments per course, sort descending, take top 1.

**Expected Output:**
```
course_name       | enrollment_count
------------------+------------------
Web Development   | 3
```
</details>

---

## Section 8: Data Modification Practice

### Q31: Update Records
**Question:** Give a 10% salary raise to all Engineering employees.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
-- First, let's see current salaries
SELECT name, salary FROM employees WHERE department = 'Engineering';

-- Update with 10% increase
UPDATE employees
SET salary = salary * 1.10
WHERE department = 'Engineering';

-- Verify the change
SELECT name, salary FROM employees WHERE department = 'Engineering';
```

**Explanation:** `UPDATE` modifies existing rows. Always use `WHERE` to avoid updating all rows!

**Before:**
```
name        | salary
------------+--------
Rahul Kumar | 75000
Amit Patel  | 80000
Ravi Verma  | 70000
```

**After:**
```
name        | salary
------------+--------
Rahul Kumar | 82500
Amit Patel  | 88000
Ravi Verma  | 77000
```
</details>

---

### Q32: Delete Records
**Question:** Remove all employees with salary less than 60,000.

<details>
<summary>Click to see answer</summary>

**Solution:**
```sql
-- First, see who will be deleted
SELECT name, salary FROM employees WHERE salary < 60000;

-- Delete them
DELETE FROM employees WHERE salary < 60000;

-- Verify deletion
SELECT COUNT(*) FROM employees;
```

**Explanation:** `DELETE` removes rows. **ALWAYS** use `WHERE` or you'll delete everything!

**Important:** Run SELECT first to verify what will be deleted!
</details>

---

## Tips for Practice

### 1. Always Test Before Modifying
```sql
-- ❌ WRONG: Direct modification
UPDATE employees SET salary = 100000;  -- Updates EVERYTHING!

-- ✅ CORRECT: Test first
SELECT * FROM employees WHERE department = 'Sales';  -- See what matches
UPDATE employees SET salary = 100000 WHERE department = 'Sales';
```

### 2. Use Aliases for Readability
```sql
-- Without aliases (harder to read)
SELECT employees.name, departments.dept_name FROM employees...

-- With aliases (cleaner)
SELECT e.name, d.dept_name FROM employees e...
```

### 3. Comment Your Complex Queries
```sql
-- Find departments with above-average salaries
SELECT 
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE salary > 50000  -- Filter low-paid employees first
GROUP BY department
HAVING AVG(salary) > (SELECT AVG(salary) FROM employees)
ORDER BY avg_salary DESC;
```

### 4. Break Complex Problems into Steps
For complicated queries:
1. Write simple query first
2. Add one Join/Filter/Aggregate at a time
3. Test after each addition
4. Combine when all parts work

---

## Next Steps

After completing these questions:
1. ✅ Practice on [LeetCode SQL](https://leetcode.com/problemset/database/)
2. ✅ Try [HackerRank SQL](https://www.hackerrank.com/domains/sql)
3. ✅ Build a small project (library system, store inventory)
4. ✅ Learn about indexes and query optimization
5. ✅ Study Window Functions and CTEs for advanced queries

**Keep practicing! SQL mastery comes from repetition and problem-solving!** 💪
