# Using SQL Databases

SQL (Structured Query Language) databases are relational database management systems that store data in structured tables with predefined schemas. They use SQL for querying and manipulating data.

## When to Use SQL Databases

SQL databases are ideal for applications that require:
- **Structured data** with a fixed schema
- **ACID transactions** (Atomicity, Consistency, Isolation, Durability) for data integrity
- **Complex queries** involving joins, aggregations, and relationships
- **Data consistency** and referential integrity
- **Transactional applications** like banking, e-commerce, or inventory systems

## Popular SQL Databases

- **MySQL**: Open-source, widely used for web applications
- **PostgreSQL**: Advanced open-source database with extensive features
- **SQLite**: Lightweight, file-based database for embedded systems
- **Oracle Database**: Enterprise-grade commercial database
- **Microsoft SQL Server**: Microsoft's relational database platform

## Basic SQL Commands

```sql
-- Create a table
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);

-- Insert data
INSERT INTO users (id, name, email) VALUES (1, 'John Doe', 'john@example.com');

-- Query data
SELECT * FROM users WHERE id = 1;

-- Update data
UPDATE users SET name = 'Jane Doe' WHERE id = 1;

-- Delete data
DELETE FROM users WHERE id = 1;
```

## Advantages

- **ACID Compliance**: Ensures reliable transactions
- **Mature Technology**: Well-established with extensive tooling
- **Strong Consistency**: Immediate data consistency across queries
- **Rich Query Language**: Powerful SQL for complex operations
- **Data Integrity**: Constraints and relationships prevent data corruption

## Disadvantages

- **Scalability Limitations**: Vertical scaling can be expensive
- **Rigid Schema**: Schema changes require careful planning
- **Performance Overhead**: ACID properties can impact performance for high-volume writes
- **Less Flexible**: Not ideal for unstructured or rapidly changing data