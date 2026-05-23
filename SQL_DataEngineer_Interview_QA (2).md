# 🗄️ SQL Interview Questions & Answers
### JSW Data Engineer Interview Preparation — Friday

---
## 📋 Table of Contents
> Click on any question to jump directly to its answer

- [Q1. What is SQL and why is it important for Data Engineers?](#q1)
- [Q2. What are the different types of SQL commands?](#q2)
- [Q3. What is the difference between WHERE and HAVING?](#q3)
- [Q4. What is the difference between DELETE, TRUNCATE and DROP?](#q4)
- [Q5. What are different types of JOINs in SQL?](#q5)
- [Q6. What is the difference between UNION and UNION ALL?](#q6)
- [Q7. What is the difference between PRIMARY KEY and FOREIGN KEY?](#q7)
- [Q8. What is the difference between DISTINCT and GROUP BY?](#q8)
- [Q9. What are Constraints in SQL?](#q9)
- [Q10. What is the difference between CHAR, VARCHAR and TEXT?](#q10)
- [Q11. What is NULL in SQL and how to handle it?](#q11)
- [Q12. What is the order of SQL query execution?](#q12)
- [Q13. What are Window Functions? (Very Important)](#q13)
- [Q14. What is the difference between RANK, DENSE_RANK and ROW_NUMBER?](#q14)
- [Q15. What is a CTE (Common Table Expression)?](#q15)
- [Q16. What is a Subquery?](#q16)
- [Q17. What are Indexes in SQL and why are they important?](#q17)
- [Q18. What is Normalization? Explain 1NF, 2NF, 3NF.](#q18)
- [Q19. What is Denormalization and when to use it?](#q19)
- [Q20. What is the difference between OLTP and OLAP?](#q20)
- [Q21. What is a View in SQL?](#q21)
- [Q22. What is a Materialized View?](#q22)
- [Q23. What are Stored Procedures and Functions?](#q23)
- [Q24. What are Transactions in SQL?](#q24)
- [Q25. What is the difference between IN, EXISTS and ANY?](#q25)
- [Q26. What is a Recursive CTE?](#q26)
- [Q27. What are Slowly Changing Dimensions (SCD)?](#q27)
- [Q28. What is Partitioning in SQL?](#q28)
- [Q29. What is Query Optimization in SQL?](#q29)
- [Q30. What is EXPLAIN / EXPLAIN ANALYZE in PostgreSQL?](#q30)
- [Q31. What is UPSERT in SQL? (INSERT + UPDATE)](#q31)
- [Q32. What is a Star Schema and Snowflake Schema?](#q32)
- [Q33. What is the difference between Clustered and Non-Clustered Index?](#q33)
- [Q34. How do you find duplicate records in SQL?](#q34)
- [Q35. How do you find the second highest salary?](#q35)
- [Q36. How do you calculate running total and moving average?](#q36)
- [Q37. What is PIVOT in SQL?](#q37)
- [Q38. How do you handle NULL values in SQL joins and aggregations?](#q38)
- [Q39. What is PostgreSQL and how is it different from MySQL?](#q39)
- [Q40. What are common PostgreSQL data types?](#q40)
- [Q41. How do you work with JSON data in PostgreSQL?](#q41)
- [Q42. What is the difference between JSON and JSONB in PostgreSQL?](#q42)
- [Q43. How do you create and use Sequences in PostgreSQL?](#q43)
- [Q44. What are Common PostgreSQL Functions for Data Engineering?](#q44)
- [Q45. What is a Trigger in SQL?](#q45)
- [Q46. What is Connection Pooling in PostgreSQL?](#q46)
- [Q47. What is VACUUM in PostgreSQL?](#q47)
- [Q48. Write a query to find Top 5 products by sales in each region.](#q48)
- [Q49. Write a query to calculate Month-over-Month (MoM) sales growth.](#q49)
- [Q50. Write a query to find customers who placed orders in both 2023 and 2024.](#q50)
- [Q51. Write a query to detect data quality issues — null values count per column.](#q51)
- [Q52. Write a query to find orders where amount is above the average for their region.](#q52)
- [Q53. Write a query to create a daily sales report with cumulative total.](#q53)
- [Q54. Write a query to find products that were never ordered.](#q54)
- [Q55. Write a query to flag outlier records (sales > 2 standard deviations from mean).](#q55)
- [Q56. Write a query to implement data versioning (SCD Type 2 insert).](#q56)
- [Q57. Write a query to transpose rows to columns (PIVOT).](#q57)
- [Q58. Write a query to find gaps in sequential data (missing order IDs).](#q58)
- [Q59. What is the difference between INTERSECT, EXCEPT and UNION?](#q59)
- [Q60. Write a query to load data with UPSERT — insert new, update existing.](#q60)
- [Q61. What is the difference between CHAR and VARCHAR?](#q61)
- [Q62. What is a Composite Primary Key?](#q62)
- [Q63. What is Pattern Matching in SQL?](#q63)
- [Q64. What is a Correlated Subquery?](#q64)
- [Q65. What is an Anti-Join in SQL?](#q65)
- [Q66. What is a Cross Join in SQL?](#q66)
- [Q67. What is a Cursor in SQL?](#q67)
- [Q68. What are Transaction Isolation Levels in SQL?](#q68)
- [Q69. What is a Deadlock and how to handle it?](#q69)
- [Q70. What is a Database Snapshot?](#q70)
- [Q71. What is Dynamic SQL and when to use it?](#q71)
- [Q72. What is the difference between Horizontal and Vertical Partitioning?](#q72)
- [Q73. What is the difference between Database Sharding and Partitioning?](#q73)
- [Q74. What is a Bitmap Index vs B-Tree Index?](#q74)
- [Q75. What is SQL Injection and how to prevent it?](#q75)
- [Q76. What are Temporary Tables in SQL?](#q76)
- [Q77. What is a Sequence in SQL?](#q77)
- [Q78. What are the ACID Properties of a Transaction?](#q78)
- [Q79. What is the CASE statement in SQL?](#q79)
- [Q80. How to delete duplicate rows keeping only one?](#q80)
- [Q61. What is a Composite Primary Key?](#q61)
- [Q62. What is a Cross Join in SQL?](#q62)
- [Q63. What is an Anti-Join in SQL?](#q63)
- [Q64. What is a Correlated Subquery?](#q64)
- [Q65. What is the difference between EXISTS and IN?](#q65)
- [Q66. What is the CASE Statement in SQL?](#q66)
- [Q67. What is Pattern Matching in SQL?](#q67)
- [Q68. What is a Cursor in SQL?](#q68)
- [Q69. What are ACID Properties in SQL?](#q69)
- [Q70. What are Transaction Isolation Levels?](#q70)
- [Q71. What is a Deadlock in SQL?](#q71)
- [Q72. What is Database Sharding?](#q72)
- [Q73. What is Dynamic SQL?](#q73)
- [Q74. What is SQL Injection and how to prevent it?](#q74)
- [Q75. What is Horizontal vs Vertical Partitioning?](#q75)
- [Q76. What is a Sequence in SQL?](#q76)
- [Q77. What is a Temporary Table in SQL?](#q77)
- [Q78. What are Scalar Functions in SQL?](#q78)
- [Q79. What is the difference between COUNT(*) and COUNT(column)?](#q79)
- [Q80. What is the COALESCE and NULLIF Function?](#q80)


---

## 🔵 SECTION 1: SQL BASICS (Q1–Q12)

---

<a id="q1"></a>
### 🔷 Q1. What is SQL and why is it important for Data Engineers?

> **What is SQL:**
> - SQL = Structured Query Language
> - Used to store, retrieve, update and delete data in relational databases
> - Works with databases like PostgreSQL, MySQL, SQL Server, SQLite
> - Industry standard — every data engineer must know SQL
>
> **Why Data Engineers use SQL:**
> - Write ETL pipelines and data transformations
> - Query and filter large datasets efficiently
> - Validate data quality — check nulls, duplicates, outliers
> - Create aggregations, reports and summaries for dashboards
> - Join multiple tables to build denormalized views
> - Manage database schema — create tables, indexes, views
> - Optimize slow queries for better pipeline performance

---

<a id="q2"></a>
### 🔷 Q2. What are the different types of SQL commands?

| Type | Full Form | Commands | Use |
|---|---|---|---|
| DDL | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Create/modify table structure |
| DML | Data Manipulation Language | SELECT, INSERT, UPDATE, DELETE | Work with data |
| DCL | Data Control Language | GRANT, REVOKE | Control permissions |
| TCL | Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT | Manage transactions |

---

<a id="q3"></a>
### 🔷 Q3. What is the difference between WHERE and HAVING?

| | WHERE | HAVING |
|---|---|---|
| Used with | Filters rows before grouping | Filters groups after GROUP BY |
| Works on | Individual rows | Aggregated results |
| Can use aggregate functions | ❌ No | ✅ Yes |

```sql
-- WHERE — filter before grouping
SELECT region, SUM(sales)
FROM orders
WHERE year = 2024           -- filters rows first
GROUP BY region;

-- HAVING — filter after grouping
SELECT region, SUM(sales)
FROM orders
GROUP BY region
HAVING SUM(sales) > 100000; -- filters groups after aggregation
```

---

<a id="q4"></a>
### 🔷 Q4. What is the difference between DELETE, TRUNCATE and DROP?

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| What | Removes specific rows | Removes all rows | Removes entire table |
| WHERE clause | ✅ Yes | ❌ No | ❌ No |
| Rollback possible | ✅ Yes | ❌ No | ❌ No |
| Speed | Slow (logs each row) | Fast | Fast |
| Structure kept | ✅ Yes | ✅ Yes | ❌ No |

---

<a id="q5"></a>
### 🔷 Q5. What are different types of JOINs in SQL?

```sql
-- INNER JOIN — only matching rows from both tables
SELECT o.order_id, c.name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;

-- LEFT JOIN — all rows from left + matching from right (NULL if no match)
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;

-- RIGHT JOIN — all rows from right + matching from left
SELECT c.name, o.order_id
FROM customers c
RIGHT JOIN orders o ON c.id = o.customer_id;

-- FULL OUTER JOIN — all rows from both tables
SELECT c.name, o.order_id
FROM customers c
FULL OUTER JOIN orders o ON c.id = o.customer_id;

-- SELF JOIN — join table with itself
SELECT a.name as employee, b.name as manager
FROM employees a
JOIN employees b ON a.manager_id = b.id;
```

---

<a id="q6"></a>
### 🔷 Q6. What is the difference between UNION and UNION ALL?

| | UNION | UNION ALL |
|---|---|---|
| Duplicates | Removes duplicate rows | Keeps all rows including duplicates |
| Speed | Slower (checks duplicates) | Faster |
| Use | When you need unique results | When duplicates are okay or expected |

```sql
-- UNION — unique rows only
SELECT city FROM customers
UNION
SELECT city FROM suppliers;

-- UNION ALL — all rows including duplicates
SELECT city FROM customers
UNION ALL
SELECT city FROM suppliers;
```

---

<a id="q7"></a>
### 🔷 Q7. What is the difference between PRIMARY KEY and FOREIGN KEY?

> - **PRIMARY KEY** — uniquely identifies each row in a table. Cannot be NULL. Only one per table.
> - **FOREIGN KEY** — links one table to another. References the PRIMARY KEY of another table. Used to maintain relationships.

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,      -- primary key
    name VARCHAR(100)
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INT REFERENCES customers(id),  -- foreign key
    amount FLOAT
);
```

---

<a id="q8"></a>
### 🔷 Q8. What is the difference between DISTINCT and GROUP BY?

> Both remove duplicates but work differently:
> - **DISTINCT** — removes duplicate rows in SELECT result
> - **GROUP BY** — groups rows to apply aggregate functions

```sql
-- DISTINCT — get unique cities
SELECT DISTINCT city FROM customers;

-- GROUP BY — count customers per city
SELECT city, COUNT(*) as total
FROM customers
GROUP BY city;
```

---

<a id="q9"></a>
### 🔷 Q9. What are Constraints in SQL?

> Constraints are rules applied on columns to ensure data integrity.

| Constraint | What it does |
|---|---|
| NOT NULL | Column cannot be empty |
| UNIQUE | All values must be different |
| PRIMARY KEY | NOT NULL + UNIQUE — identifies each row |
| FOREIGN KEY | Links to primary key of another table |
| CHECK | Value must satisfy a condition |
| DEFAULT | Sets a default value if none is provided |

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary FLOAT CHECK (salary > 0),
    department VARCHAR(50) DEFAULT 'General'
);
```

---

<a id="q10"></a>
### 🔷 Q10. What is the difference between CHAR, VARCHAR and TEXT?

| | CHAR(n) | VARCHAR(n) | TEXT |
|---|---|---|---|
| Length | Fixed length | Variable, max n | Unlimited |
| Storage | Always uses n bytes | Uses only what is needed | Uses only what is needed |
| Use | Fixed size data (phone numbers) | Names, emails | Long descriptions |
| Speed | Slightly faster | Slightly slower | Slowest |

---

<a id="q11"></a>
### 🔷 Q11. What is NULL in SQL and how to handle it?

> NULL means no value / unknown — it is NOT zero or empty string.

```sql
-- Check for NULL
SELECT * FROM orders WHERE discount IS NULL;
SELECT * FROM orders WHERE discount IS NOT NULL;

-- Replace NULL with a value
SELECT name, COALESCE(discount, 0) as discount FROM orders;

-- NULLIF — returns NULL if two values are equal
SELECT NULLIF(salary, 0) FROM employees;  -- returns NULL if salary = 0

-- NULL in calculations — any math with NULL gives NULL
SELECT 100 + NULL;  -- Result: NULL
```

---

<a id="q12"></a>
### 🔷 Q12. What is the order of SQL query execution?

> SQL does NOT execute in the order you write it. Actual execution order:

```
1. FROM       — which table
2. JOIN       — combine tables
3. WHERE      — filter rows
4. GROUP BY   — group rows
5. HAVING     — filter groups
6. SELECT     — pick columns
7. DISTINCT   — remove duplicates
8. ORDER BY   — sort results
9. LIMIT      — limit rows
```

> **Important:** This is why you CANNOT use SELECT aliases in WHERE clause — WHERE runs before SELECT!

---

## 🟡 SECTION 2: INTERMEDIATE SQL (Q13–Q25)

---

<a id="q13"></a>
### 🔷 Q13. What are Window Functions? (Very Important)

> Window functions perform calculations across related rows without collapsing them like GROUP BY.
> They use `OVER()` clause.

```sql
-- RANK — rank sales per region
SELECT name, region, sales,
    RANK() OVER (PARTITION BY region ORDER BY sales DESC) as rank
FROM sales;

-- Running total
SELECT date, sales,
    SUM(sales) OVER (ORDER BY date) as running_total
FROM sales;

-- Previous row value (LAG)
SELECT date, sales,
    LAG(sales, 1) OVER (ORDER BY date) as prev_day_sales
FROM sales;

-- Next row value (LEAD)
SELECT date, sales,
    LEAD(sales, 1) OVER (ORDER BY date) as next_day_sales
FROM sales;

-- Row number
SELECT name, sales,
    ROW_NUMBER() OVER (ORDER BY sales DESC) as row_num
FROM sales;
```

---

<a id="q14"></a>
### 🔷 Q14. What is the difference between RANK, DENSE_RANK and ROW_NUMBER?

| Function | Description | Example (100, 90, 90, 80) |
|---|---|---|
| RANK() | Same rank for ties, leaves gaps | 1, 2, 2, 4 |
| DENSE_RANK() | Same rank for ties, no gaps | 1, 2, 2, 3 |
| ROW_NUMBER() | Unique number for every row, no ties | 1, 2, 3, 4 |

---

<a id="q15"></a>
### 🔷 Q15. What is a CTE (Common Table Expression)?

> CTE is a temporary named result set created using `WITH` keyword. Makes complex queries clean and readable.

```sql
-- Without CTE — messy nested query
SELECT name FROM customers
WHERE id IN (SELECT customer_id FROM orders WHERE amount > 5000);

-- With CTE — clean and readable
WITH high_value_orders AS (
    SELECT customer_id
    FROM orders
    WHERE amount > 5000
)
SELECT name
FROM customers
WHERE id IN (SELECT customer_id FROM high_value_orders);

-- Multiple CTEs
WITH
monthly_sales AS (
    SELECT DATE_TRUNC('month', order_date) as month, SUM(amount) as total
    FROM orders
    GROUP BY 1
),
ranked_months AS (
    SELECT month, total,
           RANK() OVER (ORDER BY total DESC) as rank
    FROM monthly_sales
)
SELECT * FROM ranked_months WHERE rank <= 3;
```

---

<a id="q16"></a>
### 🔷 Q16. What is a Subquery?

> A query written inside another query.

```sql
-- Subquery in WHERE
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Subquery in FROM (inline view)
SELECT region, avg_sales
FROM (
    SELECT region, AVG(sales) as avg_sales
    FROM orders
    GROUP BY region
) as regional_avg
WHERE avg_sales > 50000;

-- Correlated subquery — references outer query
SELECT name, salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary)
    FROM employees e2
    WHERE e2.department = e1.department  -- references outer query
);
```

---

<a id="q17"></a>
### 🔷 Q17. What are Indexes in SQL and why are they important?

> An Index speeds up data retrieval — like an index in a book. Without index, database scans every row (slow). With index, it jumps directly (fast).

```sql
-- Create index
CREATE INDEX idx_orders_region ON orders(region);

-- Create composite index (multiple columns)
CREATE INDEX idx_orders_region_date ON orders(region, order_date);

-- Unique index
CREATE UNIQUE INDEX idx_email ON customers(email);

-- Drop index
DROP INDEX idx_orders_region;
```

> **When to use:** Columns used frequently in WHERE, JOIN and ORDER BY.
> **When NOT to use:** Columns with too many updates — index slows down INSERT/UPDATE.

---

<a id="q18"></a>
### 🔷 Q18. What is Normalization? Explain 1NF, 2NF, 3NF.

> **What is Normalization:**
> - Process of organizing a database to reduce redundancy and improve data integrity
> - Divides large tables into smaller tables and links them with relationships
> - Goal: store each piece of data only once
>
> **1NF — First Normal Form:**
> - Each column must have atomic (single, indivisible) values
> - No repeating groups or arrays in a column
> - Each row must be unique
> - Bad: `skills = "Python, SQL, Java"` → Good: separate row for each skill
>
> **2NF — Second Normal Form:**
> - Must satisfy all 1NF rules first
> - Every non-key column must depend on the FULL primary key
> - No partial dependency allowed
> - Bad: If primary key is (order_id, product_id) but product_name only depends on product_id — that's partial dependency
>
> **3NF — Third Normal Form:**
> - Must satisfy all 2NF rules first
> - No transitive dependency — non-key column should not depend on another non-key column
> - Bad: `employee_id → zip_code → city` (city depends on zip, not directly on employee_id)
> - Good: Move zip_code and city to a separate table

---

<a id="q19"></a>
### 🔷 Q19. What is Denormalization and when to use it?

> **What is Denormalization:**
> - Intentionally adding redundancy back into a normalized database
> - Opposite of normalization — merges tables to reduce the need for JOINs
>
> **Why use it:**
> - Improves read/query performance significantly
> - Fewer JOINs needed — queries run faster
> - Simpler queries for analysts and BI tools
>
> **When to use:**
> - Data warehouses and OLAP systems
> - Reporting databases and dashboards
> - When read speed is more important than storage cost
> - When data is rarely updated
>
> **Trade-offs:**
> - More storage space needed
> - Data updates are harder — same data exists in multiple places
> - Risk of data inconsistency if not managed carefully
>
> **Examples:**
> - Storing customer_name directly in orders table (instead of joining with customers)
> - Pre-computing monthly totals and storing them

---

<a id="q20"></a>
### 🔷 Q20. What is the difference between OLTP and OLAP?

| | OLTP | OLAP |
|---|---|---|
| Full form | Online Transaction Processing | Online Analytical Processing |
| Purpose | Day-to-day operations | Analytics and reporting |
| Operations | INSERT, UPDATE, DELETE | SELECT, aggregations |
| Data | Current, real-time | Historical, large volume |
| Schema | Normalized (3NF) | Denormalized (Star/Snowflake) |
| Example | Order placing, banking | Sales dashboards, BI reports |

---

<a id="q21"></a>
### 🔷 Q21. What is a View in SQL?

> A View is a virtual table based on a SELECT query. It does not store data — it shows data from the original table dynamically.

```sql
-- Create view
CREATE VIEW east_region_sales AS
SELECT order_id, customer_name, amount, order_date
FROM orders
WHERE region = 'East';

-- Use view like a table
SELECT * FROM east_region_sales WHERE amount > 1000;

-- Drop view
DROP VIEW east_region_sales;
```

> **Benefits:** Simplifies complex queries, provides security (hide sensitive columns), reusable.

---

<a id="q22"></a>
### 🔷 Q22. What is a Materialized View?

> A Materialized View stores the actual query result physically — unlike a regular View which runs the query every time.

| | View | Materialized View |
|---|---|---|
| Stores data | ❌ No (runs query each time) | ✅ Yes (stored snapshot) |
| Speed | Slower | Faster |
| Data freshness | Always fresh | Needs manual REFRESH |
| Use | Simple queries | Heavy aggregation queries |

```sql
-- Create materialized view
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT DATE_TRUNC('month', order_date) as month, SUM(amount) as total
FROM orders
GROUP BY 1;

-- Refresh when data updates
REFRESH MATERIALIZED VIEW monthly_sales;
```

---

<a id="q23"></a>
### 🔷 Q23. What are Stored Procedures and Functions?

```sql
-- Stored Procedure — executes a set of SQL statements
CREATE OR REPLACE PROCEDURE update_salary(emp_id INT, new_salary FLOAT)
LANGUAGE plpgsql AS $$
BEGIN
    UPDATE employees SET salary = new_salary WHERE id = emp_id;
    COMMIT;
END;
$$;

-- Call procedure
CALL update_salary(101, 75000);

-- Function — returns a value
CREATE OR REPLACE FUNCTION get_total_sales(reg VARCHAR)
RETURNS FLOAT LANGUAGE plpgsql AS $$
BEGIN
    RETURN (SELECT SUM(amount) FROM orders WHERE region = reg);
END;
$$;

-- Use function
SELECT get_total_sales('East');
```

---

<a id="q24"></a>
### 🔷 Q24. What are Transactions in SQL?

> A Transaction is a group of SQL statements that execute together — either ALL succeed or ALL fail (Atomicity).

```sql
BEGIN;

UPDATE accounts SET balance = balance - 5000 WHERE id = 1;  -- debit
UPDATE accounts SET balance = balance + 5000 WHERE id = 2;  -- credit

-- If everything is fine
COMMIT;

-- If something goes wrong — undo everything
ROLLBACK;
```

> **ACID Properties — Very Important:**
>
> **A — Atomicity:**
> - All statements in a transaction succeed or all fail together
> - No partial updates — if transfer fails halfway, everything rolls back
>
> **C — Consistency:**
> - Database moves from one valid state to another valid state
> - All rules, constraints and triggers are respected
>
> **I — Isolation:**
> - Concurrent transactions do not interfere with each other
> - Each transaction sees a consistent snapshot of data
>
> **D — Durability:**
> - Once COMMIT is done, changes are permanent
> - Even if server crashes after COMMIT, data is not lost

---

<a id="q25"></a>
### 🔷 Q25. What is the difference between IN, EXISTS and ANY?

```sql
-- IN — checks if value is in a list
SELECT name FROM customers
WHERE city IN ('Mumbai', 'Delhi', 'Bengaluru');

-- EXISTS — checks if subquery returns any rows (faster for large data)
SELECT name FROM customers c
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.customer_id = c.id
);

-- ANY — true if any comparison in subquery is true
SELECT name, salary FROM employees
WHERE salary > ANY (SELECT salary FROM employees WHERE dept = 'IT');
```

---

## 🟠 SECTION 3: ADVANCED SQL (Q26–Q38)

---

<a id="q26"></a>
### 🔷 Q26. What is a Recursive CTE?

> A Recursive CTE calls itself repeatedly — useful for hierarchical data like org charts or category trees.

```sql
-- Employee hierarchy (who reports to whom)
WITH RECURSIVE emp_hierarchy AS (
    -- Base case: top-level manager
    SELECT id, name, manager_id, 1 as level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive case: find employees under each manager
    SELECT e.id, e.name, e.manager_id, h.level + 1
    FROM employees e
    JOIN emp_hierarchy h ON e.manager_id = h.id
)
SELECT * FROM emp_hierarchy ORDER BY level;
```

---

<a id="q27"></a>
### 🔷 Q27. What are Slowly Changing Dimensions (SCD)?

> SCD is a concept in data warehousing to handle changes in dimension data over time.

| Type | What it does | Example |
|---|---|---|
| SCD Type 1 | Overwrite old value — no history | Update customer's city directly |
| SCD Type 2 | Add a new row with version tracking — keeps full history | Add new row with `start_date`, `end_date`, `is_current` flag |
| SCD Type 3 | Add a new column for previous value | Add `previous_city` column |

> **Most common in interviews:** SCD Type 2

```sql
-- SCD Type 2 example table
CREATE TABLE customers_scd2 (
    id SERIAL PRIMARY KEY,
    customer_id INT,
    name VARCHAR,
    city VARCHAR,
    start_date DATE,
    end_date DATE,
    is_current BOOLEAN
);
```

---

<a id="q28"></a>
### 🔷 Q28. What is Partitioning in SQL?

> Partitioning divides a large table into smaller physical pieces based on a column value — improves query performance.

```sql
-- Range partitioning by year
CREATE TABLE orders (
    order_id INT,
    order_date DATE,
    amount FLOAT
) PARTITION BY RANGE (EXTRACT(YEAR FROM order_date));

CREATE TABLE orders_2023 PARTITION OF orders
    FOR VALUES FROM (2023) TO (2024);

CREATE TABLE orders_2024 PARTITION OF orders
    FOR VALUES FROM (2024) TO (2025);
```

> **Benefits:** Queries on specific partition only scan that partition — much faster.

---

<a id="q29"></a>
### 🔷 Q29. What is Query Optimization in SQL?

> Writing SQL queries that run faster and use less resources.

**Key tips:**

```sql
-- ❌ BAD — function on column prevents index use (full table scan)
SELECT * FROM orders WHERE YEAR(order_date) = 2024;

-- ✅ GOOD — index-friendly range query
SELECT * FROM orders
WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01';

-- ❌ BAD — SELECT * fetches unnecessary columns
SELECT * FROM orders WHERE region = 'East';

-- ✅ GOOD — select only needed columns
SELECT order_id, amount FROM orders WHERE region = 'East';

-- ❌ BAD — using subquery that runs for each row (correlated)
SELECT name FROM customers c
WHERE (SELECT COUNT(*) FROM orders WHERE customer_id = c.id) > 5;

-- ✅ GOOD — use JOIN instead
SELECT c.name FROM customers c
JOIN (
    SELECT customer_id FROM orders
    GROUP BY customer_id HAVING COUNT(*) > 5
) o ON c.id = o.customer_id;
```

---

<a id="q30"></a>
### 🔷 Q30. What is EXPLAIN / EXPLAIN ANALYZE in PostgreSQL?

> `EXPLAIN` shows the query execution plan — how PostgreSQL plans to run your query.
> `EXPLAIN ANALYZE` actually runs the query and shows real execution time.

```sql
EXPLAIN SELECT * FROM orders WHERE region = 'East';
-- Shows: Seq Scan or Index Scan, cost estimate

EXPLAIN ANALYZE SELECT * FROM orders WHERE region = 'East';
-- Shows: actual time, rows, plan
```

> Look for:
> - **Seq Scan** — full table scan (slow, may need index)
> - **Index Scan** — using index (fast)
> - **Hash Join / Nested Loop** — join strategies

---

<a id="q31"></a>
### 🔷 Q31. What is UPSERT in SQL? (INSERT + UPDATE)

> UPSERT inserts a new row if it doesn't exist, or updates it if it does — very useful in data pipelines to avoid duplicates.

```sql
-- PostgreSQL UPSERT using ON CONFLICT
INSERT INTO products (id, name, price)
VALUES (101, 'Laptop', 75000)
ON CONFLICT (id)
DO UPDATE SET
    name = EXCLUDED.name,
    price = EXCLUDED.price;

-- Insert and ignore if already exists
INSERT INTO products (id, name, price)
VALUES (101, 'Laptop', 75000)
ON CONFLICT (id) DO NOTHING;
```

---

<a id="q32"></a>
### 🔷 Q32. What is a Star Schema and Snowflake Schema?

| | Star Schema | Snowflake Schema |
|---|---|---|
| Structure | One fact table + multiple dimension tables (not normalized) | One fact table + normalized dimension tables |
| Joins needed | Fewer joins | More joins |
| Query speed | Faster | Slower |
| Storage | More redundancy | Less redundancy |
| Use | Simple BI reports | Complex data warehouses |

> **Fact table** — stores measurable data (sales amount, quantity)
> **Dimension table** — stores descriptive data (customer name, product, date)

---

<a id="q33"></a>
### 🔷 Q33. What is the difference between Clustered and Non-Clustered Index?

| | Clustered Index | Non-Clustered Index |
|---|---|---|
| Storage | Physically sorts and stores rows in index order | Separate structure, points to rows |
| Count per table | Only 1 allowed | Multiple allowed |
| Speed | Faster for range queries | Slightly slower |
| Default | PRIMARY KEY is clustered by default | All other indexes |

---

<a id="q34"></a>
### 🔷 Q34. How do you find duplicate records in SQL?

```sql
-- Find duplicates
SELECT email, COUNT(*) as count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- Delete duplicates — keep only one (using ROW_NUMBER)
WITH duplicates AS (
    SELECT id,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) as rn
    FROM customers
)
DELETE FROM customers
WHERE id IN (
    SELECT id FROM duplicates WHERE rn > 1
);
```

---

<a id="q35"></a>
### 🔷 Q35. How do you find the second highest salary?

```sql
-- Method 1 — using LIMIT and OFFSET
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;

-- Method 2 — using subquery
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 3 — using DENSE_RANK (best approach)
SELECT salary FROM (
    SELECT salary,
           DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM employees
) ranked
WHERE rnk = 2;
```

---

<a id="q36"></a>
### 🔷 Q36. How do you calculate running total and moving average?

```sql
-- Running total of sales
SELECT order_date, amount,
    SUM(amount) OVER (ORDER BY order_date) as running_total
FROM orders;

-- Moving average (last 3 days)
SELECT order_date, amount,
    AVG(amount) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) as moving_avg_3day
FROM orders;
```

---

<a id="q37"></a>
### 🔷 Q37. What is PIVOT in SQL?

> PIVOT converts row values into column headers — turns vertical data into horizontal.

```sql
-- Sales by region per year using CASE
SELECT
    year,
    SUM(CASE WHEN region = 'East' THEN sales ELSE 0 END) as East,
    SUM(CASE WHEN region = 'West' THEN sales ELSE 0 END) as West,
    SUM(CASE WHEN region = 'North' THEN sales ELSE 0 END) as North
FROM orders
GROUP BY year;
```

---

<a id="q38"></a>
### 🔷 Q38. How do you handle NULL values in SQL joins and aggregations?

```sql
-- NULL in LEFT JOIN — right side columns are NULL when no match
SELECT c.name, COALESCE(o.amount, 0) as amount
FROM customers c
LEFT JOIN orders o ON c.id = o.customer_id;

-- NULL in aggregations — SUM, AVG, COUNT ignore NULLs automatically
SELECT AVG(salary) FROM employees;       -- ignores NULL salaries
SELECT COUNT(salary) FROM employees;     -- does NOT count NULLs
SELECT COUNT(*) FROM employees;          -- counts ALL rows including NULLs

-- Replace NULL in GROUP BY
SELECT COALESCE(region, 'Unknown') as region, COUNT(*)
FROM orders
GROUP BY region;
```

---

## 🔴 SECTION 4: PostgreSQL SPECIFIC (Q39–Q47)

---

<a id="q39"></a>
### 🔷 Q39. What is PostgreSQL and how is it different from MySQL?

| | PostgreSQL | MySQL |
|---|---|---|
| Type | Object-relational database | Relational database |
| ACID | Fully ACID compliant | Depends on engine |
| JSON support | Excellent (JSONB) | Basic |
| Window functions | ✅ Full support | ✅ Support |
| Data types | More types (arrays, hstore, UUID) | Standard types |
| Use case | Complex queries, analytics, data engineering | Web apps, simpler use cases |

---

<a id="q40"></a>
### 🔷 Q40. What are common PostgreSQL data types?

```sql
-- Numeric
INT, BIGINT, SERIAL (auto increment), FLOAT, NUMERIC(10,2)

-- Text
VARCHAR(n), TEXT, CHAR(n)

-- Date/Time
DATE, TIME, TIMESTAMP, TIMESTAMPTZ (with timezone)

-- Boolean
BOOLEAN (true/false)

-- JSON
JSON, JSONB (binary JSON — faster for querying)

-- Arrays
INTEGER[], TEXT[]

-- UUID
UUID
```

---

<a id="q41"></a>
### 🔷 Q41. How do you work with JSON data in PostgreSQL?

```sql
-- Store JSON data
CREATE TABLE events (
    id SERIAL PRIMARY KEY,
    data JSONB
);

INSERT INTO events(data) VALUES
('{"user": "Raj", "action": "login", "city": "Mumbai"}');

-- Query JSON field
SELECT data->>'user' as username FROM events;
SELECT data->>'city' as city FROM events WHERE data->>'action' = 'login';

-- Check if key exists
SELECT * FROM events WHERE data ? 'city';
```

---

<a id="q42"></a>
### 🔷 Q42. What is the difference between JSON and JSONB in PostgreSQL?

| | JSON | JSONB |
|---|---|---|
| Storage | Stored as text, preserves whitespace | Stored as binary, compressed |
| Speed | Slower to query | Faster to query |
| Indexing | Cannot be indexed | Can be indexed (GIN index) |
| Use | When original format must be preserved | When querying JSON fields frequently |

---

<a id="q43"></a>
### 🔷 Q43. How do you create and use Sequences in PostgreSQL?

> A Sequence auto-generates unique numbers — used for primary keys.

```sql
-- SERIAL is a shortcut that creates a sequence automatically
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,  -- auto generates 1, 2, 3...
    amount FLOAT
);

-- Manual sequence
CREATE SEQUENCE order_seq START 1000 INCREMENT 1;
SELECT NEXTVAL('order_seq');  -- gets next value
SELECT CURRVAL('order_seq');  -- gets current value
```

---

<a id="q44"></a>
### 🔷 Q44. What are Common PostgreSQL Functions for Data Engineering?

```sql
-- String functions
UPPER('hello')            -- 'HELLO'
LOWER('HELLO')            -- 'hello'
TRIM('  hello  ')         -- 'hello'
SUBSTRING('hello', 1, 3)  -- 'hel'
CONCAT('Raj', ' Kumar')   -- 'Raj Kumar'
REPLACE('hello', 'l', 'r') -- 'herro'
LENGTH('hello')           -- 5

-- Date functions
NOW()                                    -- current timestamp
CURRENT_DATE                             -- today's date
DATE_TRUNC('month', order_date)          -- first day of month
DATE_PART('year', order_date)            -- extract year
order_date + INTERVAL '7 days'           -- add 7 days
AGE(end_date, start_date)                -- difference between dates

-- Conditional
COALESCE(col1, col2, 'default')          -- first non-null value
NULLIF(value, 0)                         -- returns NULL if value = 0
CASE WHEN x > 0 THEN 'positive' ELSE 'negative' END
```

---

<a id="q45"></a>
### 🔷 Q45. What is a Trigger in SQL?

> A Trigger is a function that automatically runs when a specific event (INSERT, UPDATE, DELETE) happens on a table.

```sql
-- Create a function for the trigger
CREATE OR REPLACE FUNCTION update_timestamp()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Create the trigger
CREATE TRIGGER set_updated_at
BEFORE UPDATE ON orders
FOR EACH ROW
EXECUTE FUNCTION update_timestamp();
```

> **Data Engineering use:** Auto-updating `updated_at` timestamps, logging changes, data versioning.

---

<a id="q46"></a>
### 🔷 Q46. What is Connection Pooling in PostgreSQL?

> Connection Pooling manages multiple database connections efficiently — instead of opening a new connection for every request.
>
> - Opening a DB connection is expensive (slow)
> - Connection pool keeps a set of connections ready to reuse
> - Tools: **PgBouncer** (most popular), SQLAlchemy connection pool
>
> **Data Engineering use:** Python pipelines that process many records — always use connection pooling.

```python
from sqlalchemy import create_engine

# SQLAlchemy creates a connection pool automatically
engine = create_engine(
    'postgresql://user:pass@localhost/mydb',
    pool_size=5,        # 5 connections in pool
    max_overflow=10     # allow 10 more if needed
)
```

---

<a id="q47"></a>
### 🔷 Q47. What is VACUUM in PostgreSQL?

> PostgreSQL uses MVCC (Multi-Version Concurrency Control) — when rows are updated or deleted, old versions are kept temporarily.
>
> `VACUUM` cleans up these dead rows and reclaims storage.

```sql
-- Basic vacuum
VACUUM orders;

-- Full vacuum — more aggressive, locks table
VACUUM FULL orders;

-- Analyze — updates query planner statistics
VACUUM ANALYZE orders;
```

> PostgreSQL runs AutoVacuum automatically in the background. Manual VACUUM needed for very large tables.

---

## 🟢 SECTION 5: PRACTICAL DATA ENGINEERING QUERIES (Q48–Q60)

---

<a id="q48"></a>
### 🔷 Q48. Write a query to find Top 5 products by sales in each region.

```sql
SELECT region, product, total_sales
FROM (
    SELECT region, product,
           SUM(amount) as total_sales,
           RANK() OVER (PARTITION BY region ORDER BY SUM(amount) DESC) as rnk
    FROM orders
    GROUP BY region, product
) ranked
WHERE rnk <= 5;
```

---

<a id="q49"></a>
### 🔷 Q49. Write a query to calculate Month-over-Month (MoM) sales growth.

```sql
WITH monthly AS (
    SELECT DATE_TRUNC('month', order_date) as month,
           SUM(amount) as total_sales
    FROM orders
    GROUP BY 1
)
SELECT
    month,
    total_sales,
    LAG(total_sales) OVER (ORDER BY month) as prev_month_sales,
    ROUND(
        (total_sales - LAG(total_sales) OVER (ORDER BY month)) * 100.0
        / LAG(total_sales) OVER (ORDER BY month), 2
    ) as mom_growth_pct
FROM monthly
ORDER BY month;
```

---

<a id="q50"></a>
### 🔷 Q50. Write a query to find customers who placed orders in both 2023 and 2024.

```sql
SELECT customer_id
FROM orders
WHERE EXTRACT(YEAR FROM order_date) = 2023

INTERSECT

SELECT customer_id
FROM orders
WHERE EXTRACT(YEAR FROM order_date) = 2024;
```

---

<a id="q51"></a>
### 🔷 Q51. Write a query to detect data quality issues — null values count per column.

```sql
SELECT
    COUNT(*) - COUNT(customer_id) as null_customer_id,
    COUNT(*) - COUNT(amount)      as null_amount,
    COUNT(*) - COUNT(region)      as null_region,
    COUNT(*) - COUNT(order_date)  as null_order_date
FROM orders;
```

---

<a id="q52"></a>
### 🔷 Q52. Write a query to find orders where amount is above the average for their region.

```sql
SELECT order_id, region, amount
FROM orders o
WHERE amount > (
    SELECT AVG(amount)
    FROM orders
    WHERE region = o.region
);

-- Better approach using window function
SELECT order_id, region, amount
FROM (
    SELECT order_id, region, amount,
           AVG(amount) OVER (PARTITION BY region) as region_avg
    FROM orders
) t
WHERE amount > region_avg;
```

---

<a id="q53"></a>
### 🔷 Q53. Write a query to create a daily sales report with cumulative total.

```sql
SELECT
    order_date,
    SUM(amount) as daily_sales,
    SUM(SUM(amount)) OVER (ORDER BY order_date) as cumulative_sales,
    COUNT(*) as total_orders
FROM orders
GROUP BY order_date
ORDER BY order_date;
```

---

<a id="q54"></a>
### 🔷 Q54. Write a query to find products that were never ordered.

```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id
WHERE o.product_id IS NULL;
```

---

<a id="q55"></a>
### 🔷 Q55. Write a query to flag outlier records (sales > 2 standard deviations from mean).

```sql
SELECT order_id, amount,
    CASE
        WHEN amount > AVG(amount) OVER() + 2 * STDDEV(amount) OVER()
             THEN 'High Outlier'
        WHEN amount < AVG(amount) OVER() - 2 * STDDEV(amount) OVER()
             THEN 'Low Outlier'
        ELSE 'Normal'
    END as status
FROM orders;
```

---

<a id="q56"></a>
### 🔷 Q56. Write a query to implement data versioning (SCD Type 2 insert).

```sql
-- Mark old record as inactive
UPDATE customers_scd2
SET end_date = CURRENT_DATE, is_current = FALSE
WHERE customer_id = 101 AND is_current = TRUE;

-- Insert new version
INSERT INTO customers_scd2 (customer_id, name, city, start_date, end_date, is_current)
VALUES (101, 'Raj Kumar', 'Mumbai', CURRENT_DATE, '9999-12-31', TRUE);
```

---

<a id="q57"></a>
### 🔷 Q57. Write a query to transpose rows to columns (PIVOT).

```sql
-- Sales per region per year
SELECT
    year,
    SUM(CASE WHEN region = 'East'  THEN amount ELSE 0 END) as East,
    SUM(CASE WHEN region = 'West'  THEN amount ELSE 0 END) as West,
    SUM(CASE WHEN region = 'North' THEN amount ELSE 0 END) as North,
    SUM(CASE WHEN region = 'South' THEN amount ELSE 0 END) as South
FROM orders
GROUP BY year
ORDER BY year;
```

---

<a id="q58"></a>
### 🔷 Q58. Write a query to find gaps in sequential data (missing order IDs).

```sql
SELECT order_id + 1 as missing_id
FROM orders o1
WHERE NOT EXISTS (
    SELECT 1 FROM orders o2
    WHERE o2.order_id = o1.order_id + 1
)
AND order_id < (SELECT MAX(order_id) FROM orders);
```

---

<a id="q59"></a>
### 🔷 Q59. What is the difference between INTERSECT, EXCEPT and UNION?

```sql
-- UNION — all rows from both (removes duplicates)
SELECT city FROM customers UNION SELECT city FROM suppliers;

-- INTERSECT — only rows present in BOTH
SELECT city FROM customers INTERSECT SELECT city FROM suppliers;

-- EXCEPT — rows in first query but NOT in second
SELECT city FROM customers EXCEPT SELECT city FROM suppliers;
```

---

<a id="q60"></a>
### 🔷 Q60. Write a query to load data with UPSERT — insert new, update existing.

```sql
-- Bulk UPSERT for data pipeline
INSERT INTO products (product_id, name, price, updated_at)
SELECT product_id, name, price, NOW()
FROM staging_products
ON CONFLICT (product_id)
DO UPDATE SET
    name       = EXCLUDED.name,
    price      = EXCLUDED.price,
    updated_at = EXCLUDED.updated_at;
```

---

## 🟣 SECTION 6: EXTRA IMPORTANT SQL CONCEPTS (Q61–Q80)

---

<a id="q61"></a>
### 🔷 Q61. What is a Composite Primary Key?

> - A primary key made of **two or more columns together**
> - Used when one column alone cannot uniquely identify a row
> - Both columns combined must be unique
>
> ```sql
> CREATE TABLE order_items (
>     order_id   INT,
>     product_id INT,
>     quantity   INT,
>     PRIMARY KEY (order_id, product_id)  -- composite primary key
> );
> -- No single column is unique, but both together are
> ```

---

<a id="q62"></a>
### 🔷 Q62. What is a Cross Join in SQL?

> - Returns **Cartesian product** — every row of Table A paired with every row of Table B
> - No join condition needed
> - Result size = rows(A) × rows(B)
>
> ```sql
> -- 5 products × 4 regions = 20 combinations
> SELECT p.product_name, r.region_name
> FROM products p
> CROSS JOIN regions r;
> ```
>
> **Use cases:**
> - Generate all combinations — dates × products, sizes × colors
> - Create test data with all possible combinations
>
> **Difference from INNER JOIN:**
> - INNER JOIN = only matching rows (filtered)
> - CROSS JOIN = ALL combinations (no filter)

---

<a id="q63"></a>
### 🔷 Q63. What is an Anti-Join in SQL?

> - Returns rows from Table A that have **NO matching rows** in Table B
> - "In A but NOT in B"
> - Done using LEFT JOIN + WHERE IS NULL or NOT EXISTS
>
> ```sql
> -- Find customers who never placed an order
>
> -- Method 1: LEFT JOIN + IS NULL
> SELECT c.name
> FROM customers c
> LEFT JOIN orders o ON c.id = o.customer_id
> WHERE o.customer_id IS NULL;
>
> -- Method 2: NOT EXISTS (faster for large data)
> SELECT c.name
> FROM customers c
> WHERE NOT EXISTS (
>     SELECT 1 FROM orders o WHERE o.customer_id = c.id
> );
>
> -- Method 3: NOT IN (careful with NULLs!)
> SELECT name FROM customers
> WHERE id NOT IN (SELECT customer_id FROM orders);
> ```

---

<a id="q64"></a>
### 🔷 Q64. What is a Correlated Subquery?

> - A subquery that **references columns from the outer query**
> - Re-evaluated for EACH row of the outer query — can be slow
>
> ```sql
> -- Find employees earning above their department average
> SELECT name, salary, department_id
> FROM employees e
> WHERE salary > (
>     SELECT AVG(salary)
>     FROM employees e2
>     WHERE e2.department_id = e.department_id  -- references outer query
> );
> ```
>
> | | Regular Subquery | Correlated Subquery |
> |---|---|---|
> | Runs | Once | Once per outer row |
> | References | Independent | Depends on outer query |
> | Speed | Faster | Slower on large data |

---

<a id="q65"></a>
### 🔷 Q65. What is the difference between EXISTS and IN?

> ```sql
> -- IN — checks if value is in a list
> SELECT name FROM customers
> WHERE id IN (SELECT customer_id FROM orders);
>
> -- EXISTS — checks if any row is returned by subquery
> SELECT name FROM customers c
> WHERE EXISTS (
>     SELECT 1 FROM orders o WHERE o.customer_id = c.id
> );
> ```
>
> | | IN | EXISTS |
> |---|---|---|
> | Works with | A list or subquery result | A correlated subquery |
> | NULL handling | NOT IN fails if subquery returns NULL | NOT EXISTS works safely with NULLs |
> | Speed | Good for small lists | Better for large data with indexes |
>
> **Rule of thumb:** Use EXISTS for large tables — it stops at first match

---

<a id="q66"></a>
### 🔷 Q66. What is the CASE Statement in SQL?

> - SQL's way to add **if-else logic** inside a query
> - Returns different values based on conditions
>
> ```sql
> -- Classify salary levels
> SELECT name, salary,
>     CASE
>         WHEN salary > 100000 THEN 'High'
>         WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium'
>         ELSE 'Low'
>     END AS salary_level
> FROM employees;
>
> -- Count by category using CASE
> SELECT
>     COUNT(CASE WHEN region = 'East' THEN 1 END) as east_count,
>     COUNT(CASE WHEN region = 'West' THEN 1 END) as west_count
> FROM orders;
> ```

---

<a id="q67"></a>
### 🔷 Q67. What is Pattern Matching in SQL?

> - Used to search for a pattern in text using `LIKE`
>
> ```sql
> -- % = any number of characters
> SELECT * FROM customers WHERE name LIKE 'Raj%';   -- starts with Raj
> SELECT * FROM customers WHERE name LIKE '%Kumar';  -- ends with Kumar
> SELECT * FROM customers WHERE name LIKE '%ish%';   -- contains ish
>
> -- _ = exactly one character
> SELECT * FROM products WHERE code LIKE 'A_1';  -- A, any char, 1
>
> -- NOT LIKE
> SELECT * FROM customers WHERE email NOT LIKE '%gmail%';
>
> -- PostgreSQL — case insensitive
> SELECT * FROM customers WHERE name ILIKE 'raj%';
> ```

---

<a id="q68"></a>
### 🔷 Q68. What is a Cursor in SQL?

> - A cursor lets you **process rows one at a time** instead of as a set
> - Used when you need to loop through results row by row
>
> ```sql
> DECLARE cur CURSOR FOR
>     SELECT id, salary FROM employees;
>
> OPEN cur;
>
> FETCH NEXT FROM cur INTO @id, @salary;
>
> WHILE @@FETCH_STATUS = 0
> BEGIN
>     -- process each row
>     PRINT @salary;
>     FETCH NEXT FROM cur INTO @id, @salary;
> END
>
> CLOSE cur;
> DEALLOCATE cur;
> ```
>
> **Important for interviews:**
> - Cursors are **slow** — avoid in Data Engineering
> - Always prefer set-based operations (single SQL query)
> - Use cursors only when row-by-row logic is absolutely needed

---

<a id="q69"></a>
### 🔷 Q69. What are ACID Properties in SQL?

> **ACID = Atomicity + Consistency + Isolation + Durability**
>
> - **Atomicity** — All steps in a transaction succeed OR all fail together. No partial updates.
>   - Example: Bank transfer — debit + credit must both succeed or both rollback
>
> - **Consistency** — Database moves from one valid state to another. All rules and constraints are respected.
>   - Example: Inventory count cannot go below zero
>
> - **Isolation** — Concurrent transactions don't interfere with each other. Each transaction sees a consistent snapshot.
>   - Example: Two users booking same seat — one must wait
>
> - **Durability** — Once committed, changes are permanent even if server crashes.
>   - Example: Confirmed order stays even after power failure

---

<a id="q70"></a>
### 🔷 Q70. What are Transaction Isolation Levels?

> Controls how much one transaction can see of another's uncommitted changes.
>
> | Isolation Level | Dirty Read | Non-Repeatable Read | Phantom Read |
> |---|---|---|---|
> | Read Uncommitted | ✅ Possible | ✅ Possible | ✅ Possible |
> | Read Committed | ❌ No | ✅ Possible | ✅ Possible |
> | Repeatable Read | ❌ No | ❌ No | ✅ Possible |
> | Serializable | ❌ No | ❌ No | ❌ No |
>
> - **Dirty Read** — reading uncommitted data from another transaction
> - **Non-Repeatable Read** — same row gives different values in same transaction
> - **Phantom Read** — new rows appear between two reads in same transaction
>
> ```sql
> -- Set isolation level in PostgreSQL
> BEGIN;
> SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
> -- your queries here
> COMMIT;
> ```
>
> **Default in PostgreSQL = Read Committed**

---

<a id="q71"></a>
### 🔷 Q71. What is a Deadlock in SQL?

> - Deadlock = two transactions waiting for each other's locked resources — neither can proceed
>
> ```
> Transaction A holds lock on Table 1, waiting for Table 2
> Transaction B holds lock on Table 2, waiting for Table 1
> → Both wait forever = Deadlock!
> ```
>
> **How database handles it:**
> - Automatically detects the deadlock
> - Kills one transaction (victim) and rolls it back
> - Other transaction proceeds
>
> **How to prevent deadlocks:**
> - Always access tables in the same order in all transactions
> - Keep transactions short — commit quickly
> - Use proper indexes — reduces lock duration
> - Use lower isolation levels where possible
>
> **Deadlock vs Livelock:**
> - Deadlock = transactions blocked, no progress
> - Livelock = transactions keep responding to each other but still no progress

---

<a id="q72"></a>
### 🔷 Q72. What is Database Sharding?

> - Sharding = splitting a large database into smaller **independent databases** across multiple servers
> - Each shard holds a subset of data
>
> ```
> Without Sharding:         With Sharding:
> One big DB                DB Shard 1 (users 1-1M)
> All 10M users             DB Shard 2 (users 1M-2M)
>                           DB Shard 3 (users 2M-3M)
> ```
>
> | | Sharding | Partitioning |
> |---|---|---|
> | Where | Data split across multiple databases/servers | Data split within one database |
> | Purpose | Horizontal scaling across machines | Performance within one machine |
> | Complexity | High | Medium |
> | Example | DB per region | Table split by year |
>
> **When to use sharding:**
> - Data is too large for one server
> - Need to scale writes across multiple machines
> - High traffic applications (millions of users)

---

<a id="q73"></a>
### 🔷 Q73. What is Dynamic SQL?

> - SQL that is built and executed **at runtime** — not fixed at write time
>
> ```sql
> -- PostgreSQL dynamic SQL using EXECUTE
> DO $$
> DECLARE
>     table_name TEXT := 'orders';
>     query TEXT;
> BEGIN
>     query := 'SELECT COUNT(*) FROM ' || table_name;
>     EXECUTE query;
> END $$;
>
> -- SQL Server dynamic SQL
> DECLARE @sql NVARCHAR(MAX)
> SET @sql = 'SELECT * FROM ' + @TableName
> EXEC sp_executesql @sql;
> ```
>
> **Advantages:**
> - Flexible — table name or columns not known until runtime
> - Can build different queries based on conditions
>
> **Risks — Very Important:**
> - **SQL Injection** — if user input is added to query without sanitization
> - Harder to debug — query built at runtime
> - May not use cached execution plans — slower
>
> **Prevention:** Always use parameterized queries — never concatenate raw user input

---

<a id="q74"></a>
### 🔷 Q74. What is SQL Injection and how to prevent it?

> - SQL injection = attacker inserts malicious SQL code into input fields to manipulate queries
>
> ```sql
> -- DANGEROUS — string concatenation with user input
> query = "SELECT * FROM users WHERE name = '" + username + "'"
> -- If username = "'; DROP TABLE users; --"
> -- Query becomes: SELECT * FROM users WHERE name = ''; DROP TABLE users; --'
>
> -- SAFE — parameterized query (always use this)
> cursor.execute("SELECT * FROM users WHERE name = %s", (username,))
> ```
>
> **Prevention methods:**
> - Use **parameterized queries / prepared statements** — most important
> - Validate and sanitize all user inputs
> - Use least-privilege DB accounts — no DROP, limited access
> - Never show raw SQL errors to end users
> - Use ORM (SQLAlchemy) which auto-handles parameterization

---

<a id="q75"></a>
### 🔷 Q75. What is Horizontal vs Vertical Partitioning?

> | | Horizontal Partitioning | Vertical Partitioning |
> |---|---|---|
> | Splits | Rows of a table | Columns of a table |
> | Same columns | ✅ Yes — different rows | ❌ No — different columns |
> | Example | Orders 2023 in one table, Orders 2024 in another | Frequently used columns in one table, rarely used in another |
> | Use case | Time-series, region-based data | Tables with too many columns |
>
> ```sql
> -- Horizontal: split orders by year
> CREATE TABLE orders_2023 PARTITION OF orders
> FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');
>
> -- Vertical: split rarely used big columns
> -- Main table: id, name, price, region (frequently queried)
> -- Detail table: id, description, specifications (rarely queried)
> ```

---

<a id="q76"></a>
### 🔷 Q76. What is a Sequence in SQL?

> - A database object that generates **unique sequential numbers** automatically
> - Used for primary key generation or any auto-incrementing column
>
> ```sql
> -- Create sequence
> CREATE SEQUENCE order_seq START 1000 INCREMENT 1;
>
> -- Use in INSERT
> INSERT INTO orders (id, amount)
> VALUES (NEXTVAL('order_seq'), 5000);
>
> -- Get current value
> SELECT CURRVAL('order_seq');
>
> -- SERIAL in PostgreSQL = shortcut that auto-creates sequence
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,   -- auto-generates 1, 2, 3...
>     amount FLOAT
> );
> ```
>
> **Sequence vs Identity column:**
> - Sequence can be shared across multiple tables
> - Sequence can be customized — start value, increment, min, max
> - Identity column is tied to one table only

---

<a id="q77"></a>
### 🔷 Q77. What is a Temporary Table in SQL?

> - A table that exists only for the **duration of a session or transaction**
> - Automatically deleted when session ends
> - Useful for storing intermediate results in complex pipelines
>
> ```sql
> -- Create temporary table
> CREATE TEMP TABLE temp_sales AS
> SELECT region, SUM(amount) as total
> FROM orders
> WHERE order_date >= '2024-01-01'
> GROUP BY region;
>
> -- Use it in next step
> SELECT * FROM temp_sales WHERE total > 100000;
>
> -- Auto-deleted when session ends
> -- Or drop manually:
> DROP TABLE temp_sales;
> ```
>
> | | Temporary Table | CTE |
> |---|---|---|
> | Exists | For entire session | Only during single query |
> | Reusable | ✅ Multiple queries | ❌ Only in one query |
> | Indexed | ✅ Can add indexes | ❌ No |
> | Best for | Multi-step pipelines | Readable complex queries |

---

<a id="q78"></a>
### 🔷 Q78. What are Scalar Functions in SQL?

> - Functions that take one value and return one value
> - Used for formatting, converting or computing on a single value
>
> ```sql
> -- String functions
> SELECT UPPER('hello')           -- 'HELLO'
> SELECT LOWER('HELLO')           -- 'hello'
> SELECT LENGTH('hello')          -- 5
> SELECT TRIM('  hello  ')        -- 'hello'
> SELECT SUBSTRING('hello', 1, 3) -- 'hel'
> SELECT REPLACE('hello', 'l', 'r') -- 'herro'
>
> -- Numeric functions
> SELECT ROUND(3.14159, 2)        -- 3.14
> SELECT ABS(-50)                  -- 50
> SELECT CEIL(4.2)                 -- 5
> SELECT FLOOR(4.9)                -- 4
>
> -- Date functions
> SELECT NOW()                     -- current timestamp
> SELECT CURRENT_DATE              -- today's date
> SELECT EXTRACT(YEAR FROM NOW())  -- 2024
> ```

---

<a id="q79"></a>
### 🔷 Q79. What is the difference between COUNT(*) and COUNT(column)?

> ```sql
> -- COUNT(*) — counts ALL rows including NULLs
> SELECT COUNT(*) FROM employees;     -- returns 100 (all rows)
>
> -- COUNT(column) — counts only NON-NULL values
> SELECT COUNT(salary) FROM employees; -- returns 95 (if 5 salaries are NULL)
>
> -- COUNT(DISTINCT column) — counts unique non-null values
> SELECT COUNT(DISTINCT department) FROM employees; -- returns 5 unique departments
> ```
>
> | | COUNT(*) | COUNT(column) | COUNT(DISTINCT column) |
> |---|---|---|---|
> | Counts | All rows | Non-null values only | Unique non-null values |
> | NULLs | ✅ Included | ❌ Ignored | ❌ Ignored |

---

<a id="q80"></a>
### 🔷 Q80. What is the COALESCE and NULLIF Function?

> **COALESCE — returns first non-NULL value:**
> ```sql
> SELECT COALESCE(NULL, NULL, 'Default') -- 'Default'
> SELECT COALESCE(discount, 0)           -- replaces NULL discount with 0
>
> -- Real use: show full name — use whatever is available
> SELECT COALESCE(first_name, second_name, last_name) as name
> FROM employees;
> ```
>
> **NULLIF — returns NULL if two values are equal:**
> ```sql
> SELECT NULLIF(salary, 0)   -- returns NULL if salary = 0 (avoid division by zero)
>
> -- Safe division using NULLIF
> SELECT total_sales / NULLIF(total_orders, 0) as avg_order_value
> FROM summary;
> -- If total_orders = 0, returns NULL instead of error
> ```

---

## 🟣 SECTION 6: EXTRA IMPORTANT TOPICS (Q61–Q80)

---

<a id="q61"></a>
### 🔷 Q61. What is the difference between CHAR and VARCHAR?

| | CHAR | VARCHAR |
|---|---|---|
| Storage | Fixed length — always uses n bytes | Variable length — uses only what needed |
| Padding | Pads unused space with spaces | No padding |
| Speed | Slightly faster (fixed size) | Slightly slower |
| Use case | Fixed size data — phone numbers, codes | Names, emails, descriptions |

```sql
name CHAR(10)       -- "Raj" stored as "Raj       " (7 spaces padded)
name VARCHAR(10)    -- "Raj" stored as "Raj" (no padding)
```

---

<a id="q62"></a>
### 🔷 Q62. What is a Composite Primary Key?

> **Definition:**
> - A primary key made up of two or more columns together
> - Used when no single column can uniquely identify a row
> - Combination of all columns must be unique
>
> ```sql
> CREATE TABLE order_items (
>     order_id    INT,
>     product_id  INT,
>     quantity    INT,
>     PRIMARY KEY (order_id, product_id)  -- composite primary key
> );
> -- order_id alone is not unique, product_id alone is not unique
> -- but order_id + product_id together is unique
> ```
>
> **When to use:**
> - Junction/bridge tables in many-to-many relationships
> - When business rule requires combination to be unique

---

<a id="q63"></a>
### 🔷 Q63. What is Pattern Matching in SQL?

> **LIKE operator:**
> - `%` — matches any sequence of characters (including none)
> - `_` — matches exactly one character
>
> ```sql
> -- Names starting with 'R'
> SELECT * FROM employees WHERE name LIKE 'R%';
>
> -- Names ending with 'ar'
> SELECT * FROM employees WHERE name LIKE '%ar';
>
> -- Names with exactly 5 characters
> SELECT * FROM employees WHERE name LIKE '_____';
>
> -- Names containing 'aj' anywhere
> SELECT * FROM employees WHERE name LIKE '%aj%';
>
> -- NOT LIKE — exclude matches
> SELECT * FROM employees WHERE name NOT LIKE 'A%';
> ```
>
> **PostgreSQL extras:**
> - `ILIKE` — case-insensitive match
> - `~` — regex match
> - `~*` — case-insensitive regex match
>
> ```sql
> SELECT * FROM employees WHERE name ILIKE 'raj%';  -- matches Raj, RAJ, raj
> SELECT * FROM employees WHERE email ~ '^[a-z]+@gmail\.com$';  -- regex
> ```

---

<a id="q64"></a>
### 🔷 Q64. What is a Correlated Subquery?

> **Definition:**
> - A subquery that references columns from the outer (parent) query
> - Re-evaluated for every row of the outer query — can be slow
> - Different from a regular subquery which runs only once
>
> ```sql
> -- Regular subquery — runs once
> SELECT * FROM employees
> WHERE salary > (SELECT AVG(salary) FROM employees);
>
> -- Correlated subquery — runs for EACH employee row
> -- Find employees earning above their department's average
> SELECT name, salary, department_id
> FROM employees e
> WHERE salary > (
>     SELECT AVG(salary)
>     FROM employees e2
>     WHERE e2.department_id = e.department_id  -- references outer query
> );
> ```
>
> **Performance tip:**
> - Correlated subqueries are slow on large tables
> - Rewrite using window functions for better performance
>
> ```sql
> -- Better approach using window function
> SELECT name, salary, department_id
> FROM (
>     SELECT name, salary, department_id,
>            AVG(salary) OVER (PARTITION BY department_id) as dept_avg
>     FROM employees
> ) t
> WHERE salary > dept_avg;
> ```

---

<a id="q65"></a>
### 🔷 Q65. What is an Anti-Join in SQL?

> **Definition:**
> - An Anti-Join returns rows from the LEFT table that have NO matching row in the RIGHT table
> - Opposite of a regular JOIN
> - Common ways to write it: `LEFT JOIN ... WHERE right.id IS NULL` or `NOT EXISTS`
>
> ```sql
> -- Find customers who have NEVER placed an order
>
> -- Method 1: LEFT JOIN + IS NULL
> SELECT c.customer_id, c.name
> FROM customers c
> LEFT JOIN orders o ON c.customer_id = o.customer_id
> WHERE o.customer_id IS NULL;
>
> -- Method 2: NOT EXISTS (preferred — handles NULLs better)
> SELECT c.customer_id, c.name
> FROM customers c
> WHERE NOT EXISTS (
>     SELECT 1 FROM orders o
>     WHERE o.customer_id = c.customer_id
> );
>
> -- Method 3: NOT IN (careful with NULLs!)
> SELECT customer_id, name FROM customers
> WHERE customer_id NOT IN (SELECT customer_id FROM orders);
> ```
>
> **Important:** Avoid `NOT IN` if subquery can return NULL — it returns no rows at all. Prefer `NOT EXISTS`.

---

<a id="q66"></a>
### 🔷 Q66. What is a Cross Join in SQL?

> **Definition:**
> - Returns the Cartesian product of two tables — every row of Table A paired with every row of Table B
> - Result size = rows(A) × rows(B)
> - No join condition needed
>
> ```sql
> -- 3 products × 4 regions = 12 combinations
> SELECT p.product_name, r.region_name
> FROM products p
> CROSS JOIN regions r;
>
> -- Use case: generate all date + product combinations for a report
> SELECT d.date, p.product_id
> FROM date_dimension d
> CROSS JOIN products p;
> ```
>
> **vs INNER JOIN:**
> - CROSS JOIN = all combinations (no condition)
> - INNER JOIN = only matching rows (with condition)

---

<a id="q67"></a>
### 🔷 Q67. What is a Cursor in SQL?

> **Definition:**
> - A cursor retrieves and processes rows from a result set one row at a time
> - Used when you need to process data row-by-row (not as a set)
> - Generally slower than set-based operations — avoid when possible
>
> ```sql
> -- PostgreSQL cursor example
> DO $$
> DECLARE
>     emp_cursor CURSOR FOR SELECT id, name, salary FROM employees;
>     emp_row employees%ROWTYPE;
> BEGIN
>     OPEN emp_cursor;
>     LOOP
>         FETCH emp_cursor INTO emp_row;
>         EXIT WHEN NOT FOUND;
>         -- Process each row
>         RAISE NOTICE 'Employee: %, Salary: %', emp_row.name, emp_row.salary;
>     END LOOP;
>     CLOSE emp_cursor;
> END $$;
> ```
>
> **Cursor types:**
> - **STATIC** — snapshot, does not reflect changes during iteration
> - **DYNAMIC** — reflects all changes while cursor is open
> - **FORWARD_ONLY** — can only move forward, not backward
>
> **Best practice:** Avoid cursors — use set-based SQL, window functions or CTEs instead. Cursors are slow and resource-heavy.

---

<a id="q68"></a>
### 🔷 Q68. What are Transaction Isolation Levels in SQL?

> **What is Isolation:**
> - Isolation controls how much one transaction can see of another transaction's uncommitted changes
> - Higher isolation = more data safety, but slower (more locking)
> - Lower isolation = faster, but risk of reading dirty/inconsistent data
>
> **4 Isolation Levels (from lowest to highest):**
>
> | Level | Dirty Read | Non-Repeatable Read | Phantom Read |
> |---|---|---|---|
> | Read Uncommitted | ✅ Possible | ✅ Possible | ✅ Possible |
> | Read Committed | ❌ No | ✅ Possible | ✅ Possible |
> | Repeatable Read | ❌ No | ❌ No | ✅ Possible |
> | Serializable | ❌ No | ❌ No | ❌ No |
>
> **Definitions:**
> - **Dirty Read** — reading data that another transaction hasn't committed yet
> - **Non-Repeatable Read** — same row gives different values in same transaction
> - **Phantom Read** — same query returns different number of rows in same transaction
>
> ```sql
> -- Set isolation level in PostgreSQL
> BEGIN;
> SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
> SELECT * FROM orders WHERE region = 'East';
> COMMIT;
> ```
>
> **Default:** Most databases use `READ COMMITTED` by default. PostgreSQL default = READ COMMITTED.

---

<a id="q69"></a>
### 🔷 Q69. What is a Deadlock and how to handle it?

> **What is a Deadlock:**
> - Two or more transactions are waiting for each other to release a lock — circular wait
> - Neither can proceed — database must kill one to break the cycle
>
> ```
> Transaction A: holds lock on Table1, waiting for Table2
> Transaction B: holds lock on Table2, waiting for Table1
> → Deadlock! Neither can proceed
> ```
>
> **Deadlock vs Blocking:**
>
> | | Blocking | Deadlock |
> |---|---|---|
> | What | One transaction waits for another to release a lock | Two transactions wait for each other (circular) |
> | Resolution | Resolves automatically when lock is released | Needs database to kill one transaction |
> | Active? | Waiting transaction is paused | Both transactions are active but stuck |
>
> **How to prevent deadlocks:**
> - Always acquire locks in the same order across all transactions
> - Keep transactions short — do less work per transaction
> - Use proper indexes — reduces lock scope
> - Use lower isolation levels where possible
> - Use `SELECT ... FOR UPDATE SKIP LOCKED` to skip locked rows
>
> ```sql
> -- Always lock tables in same order to prevent deadlocks
> -- Bad: Transaction A locks orders then customers
> --      Transaction B locks customers then orders = deadlock risk
> -- Good: Both lock orders first, then customers = no deadlock
> ```

---

<a id="q70"></a>
### 🔷 Q70. What is a Database Snapshot?

> **Definition:**
> - A read-only, static view of a database at a specific point in time
> - Shows what the data looked like at the moment of snapshot creation
>
> **Uses:**
> - **Reporting** — analysts query snapshot without affecting live database
> - **Backup/Recovery** — restore data to a specific point if something goes wrong
> - **Testing** — test changes against stable data without touching production
>
> ```sql
> -- SQL Server example
> CREATE DATABASE MySnapshot ON
> (
>     NAME = MyDatabase_Data,
>     FILENAME = 'C:\Snapshots\MyDB_snap.ss'
> )
> AS SNAPSHOT OF MyDatabase;
>
> -- Query the snapshot
> SELECT * FROM MySnapshot.dbo.orders;
> ```
>
> **PostgreSQL equivalent:** Time Travel (see BigQuery Time Travel) or `pg_dump` for point-in-time backups

---

<a id="q71"></a>
### 🔷 Q71. What is Dynamic SQL and when to use it?

> **Definition:**
> - SQL code that is constructed and executed at runtime as a string
> - Used when table name, column name or conditions are not known until runtime
>
> ```sql
> -- PostgreSQL dynamic SQL using EXECUTE
> DO $$
> DECLARE
>     table_name TEXT := 'orders';
>     sql_query TEXT;
> BEGIN
>     sql_query := 'SELECT COUNT(*) FROM ' || table_name;
>     EXECUTE sql_query;
> END $$;
>
> -- Python example using parameterized query (SAFE way)
> import psycopg2
> cursor.execute(
>     "SELECT * FROM orders WHERE region = %s AND date = %s",
>     (region, date)  -- parameters passed separately — prevents SQL injection
> )
> ```
>
> **Advantages:**
> - Flexible — can work with dynamic table/column names
> - Handles multiple scenarios in one code block
>
> **Risks:**
> - **SQL Injection** — if user input is concatenated directly into SQL string
> - **Hard to debug** — errors appear at runtime, not at compile time
> - **Performance** — may not use cached query plans
>
> **Best practice:** Always use parameterized queries (`%s`, `$1`) — never concatenate user input directly into SQL.

---

<a id="q72"></a>
### 🔷 Q72. What is the difference between Horizontal and Vertical Partitioning?

| | Horizontal Partitioning | Vertical Partitioning |
|---|---|---|
| How | Splits rows into separate partitions | Splits columns into separate tables |
| Same columns? | ✅ Yes — all partitions have same columns | ❌ No — each part has different columns |
| Use case | Time-based or region-based data splitting | Separate rarely-used/large columns |
| Example | Orders split by year — 2022/2023/2024 | Store large TEXT/BLOB columns separately |
| Benefit | Queries scan only relevant rows | Queries fetch only needed columns faster |

```sql
-- Horizontal: same structure, different rows
-- orders_2022 table: all 2022 orders
-- orders_2023 table: all 2023 orders
-- orders_2024 table: all 2024 orders

-- Vertical: split columns
-- employees_basic: (id, name, email, department)  -- frequently accessed
-- employees_details: (id, bio, photo, documents)  -- rarely accessed
```

---

<a id="q73"></a>
### 🔷 Q73. What is the difference between Database Sharding and Partitioning?

| | Partitioning | Sharding |
|---|---|---|
| Location | Data stays in one database | Data split across multiple databases/servers |
| Purpose | Better performance and manageability | Horizontal scaling across servers |
| Visibility | Still one logical table | Separate independent databases |
| Complexity | Low | High |
| Use case | Large tables, time-series data | Massive scale — millions of users |
| Example | Orders table split by year | User database split by region — Asia DB, Europe DB |

> **Simple rule:**
> - Partitioning = splitting within one database
> - Sharding = splitting across multiple databases/servers

---

<a id="q74"></a>
### 🔷 Q74. What is a Bitmap Index vs B-Tree Index?

| | Bitmap Index | B-Tree Index |
|---|---|---|
| Structure | Uses bitmaps (bit arrays) | Balanced tree structure |
| Best for | Low-cardinality columns (few unique values) | High-cardinality columns (many unique values) |
| Examples | Gender (M/F), Status (Active/Inactive) | Customer ID, Email, Order ID |
| Operations | Fast for AND, OR, NOT conditions | Fast for range queries, exact lookups |
| Updates | Slow — bit arrays need rebuilding | Fast for updates |
| Use in | OLAP/data warehouse systems | OLTP systems |

```sql
-- B-Tree index (default) — for high cardinality
CREATE INDEX idx_order_id ON orders(order_id);

-- Bitmap index (Oracle/PostgreSQL GIN/BRIN)
-- Used internally for low-cardinality columns in OLAP queries
```

---

<a id="q75"></a>
### 🔷 Q75. What is SQL Injection and how to prevent it?

> **What is SQL Injection:**
> - An attack where malicious SQL code is inserted into a query via user input
> - Attacker can read, modify or delete database data
>
> ```python
> # ❌ VULNERABLE — string concatenation
> query = "SELECT * FROM users WHERE name = '" + user_input + "'"
> # If user types: ' OR '1'='1
> # Query becomes: SELECT * FROM users WHERE name = '' OR '1'='1'
> # Returns ALL users! 😱
>
> # ✅ SAFE — parameterized query
> cursor.execute("SELECT * FROM users WHERE name = %s", (user_input,))
> ```
>
> **Prevention methods:**
> - Use parameterized queries / prepared statements — always
> - Validate and sanitize all user inputs
> - Use least privilege database accounts — no DROP, no admin
> - Use stored procedures that don't build dynamic SQL
> - Never expose raw error messages to users

---

<a id="q76"></a>
### 🔷 Q76. What are Temporary Tables in SQL?

> **Definition:**
> - Tables that exist only for the duration of a session or transaction
> - Auto-deleted when session ends
> - Used for storing intermediate results in complex queries
>
> ```sql
> -- Create temporary table
> CREATE TEMP TABLE temp_sales AS
> SELECT region, SUM(amount) as total
> FROM orders
> WHERE year = 2024
> GROUP BY region;
>
> -- Use it in further queries
> SELECT * FROM temp_sales WHERE total > 100000;
>
> -- Automatically dropped when session ends
> -- Or drop manually
> DROP TABLE IF EXISTS temp_sales;
> ```
>
> **Local vs Global Temporary Tables (SQL Server):**
> - `#TempTable` — local, only visible in current session
> - `##TempTable` — global, visible to all sessions
>
> **vs CTE:**
> - CTE exists only for one query
> - Temp table exists for entire session — can be used in multiple queries

---

<a id="q77"></a>
### 🔷 Q77. What is a Sequence in SQL?

> **Definition:**
> - A database object that generates unique sequential numbers automatically
> - Used to create unique primary key values
>
> ```sql
> -- Create a sequence
> CREATE SEQUENCE order_seq
> START WITH 1000
> INCREMENT BY 1
> MINVALUE 1000
> MAXVALUE 999999
> NO CYCLE;
>
> -- Use sequence
> INSERT INTO orders (order_id, amount)
> VALUES (NEXTVAL('order_seq'), 5000);
>
> SELECT CURRVAL('order_seq');  -- current value
> SELECT NEXTVAL('order_seq');  -- next value
>
> -- SERIAL shortcut (PostgreSQL) — auto-creates sequence
> CREATE TABLE orders (
>     id SERIAL PRIMARY KEY,  -- auto-increments: 1, 2, 3...
>     amount FLOAT
> );
> ```
>
> **Sequence vs SERIAL vs IDENTITY:**
> - `SERIAL` — shortcut that creates a sequence automatically (older PostgreSQL)
> - `GENERATED ALWAYS AS IDENTITY` — SQL standard, recommended in PostgreSQL 10+
> - Custom `SEQUENCE` — most flexible, can be shared across tables

---

<a id="q78"></a>
### 🔷 Q78. What are the ACID Properties of a Transaction?

> **ACID = Atomicity + Consistency + Isolation + Durability**
>
> **A — Atomicity:**
> - Transaction is all or nothing — either all steps succeed or none do
> - If step 3 of 5 fails, steps 1 and 2 are rolled back
> - Example: Bank transfer — debit + credit must both succeed or both fail
>
> **C — Consistency:**
> - Database moves from one valid state to another
> - All rules, constraints and triggers must still hold after transaction
> - Example: Account balance cannot go below zero
>
> **I — Isolation:**
> - Concurrent transactions do not see each other's uncommitted changes
> - Each transaction appears to run alone
> - Example: Two people booking last seat — one will get it, not both
>
> **D — Durability:**
> - Once committed, data is permanent — survives crashes, restarts
> - Written to disk/WAL log before confirming commit
> - Example: Confirmed order stays even if server restarts immediately after

---

<a id="q79"></a>
### 🔷 Q79. What is the CASE statement in SQL?

> **Definition:**
> - SQL's conditional logic — like if-else in programming
> - Returns different values based on conditions
>
> ```sql
> -- Searched CASE
> SELECT name, salary,
>     CASE
>         WHEN salary > 100000 THEN 'High'
>         WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium'
>         ELSE 'Low'
>     END AS salary_level
> FROM employees;
>
> -- Simple CASE
> SELECT name,
>     CASE department_id
>         WHEN 1 THEN 'Engineering'
>         WHEN 2 THEN 'Sales'
>         WHEN 3 THEN 'HR'
>         ELSE 'Other'
>     END AS department_name
> FROM employees;
>
> -- CASE in aggregation — conditional count
> SELECT
>     COUNT(CASE WHEN status = 'completed' THEN 1 END) as completed,
>     COUNT(CASE WHEN status = 'pending'   THEN 1 END) as pending,
>     COUNT(CASE WHEN status = 'failed'    THEN 1 END) as failed
> FROM orders;
> ```

---

<a id="q80"></a>
### 🔷 Q80. How to delete duplicate rows keeping only one?

> **Most common interview question:**
>
> ```sql
> -- Method 1: Using ROW_NUMBER (most common)
> DELETE FROM employees
> WHERE id IN (
>     SELECT id FROM (
>         SELECT id,
>                ROW_NUMBER() OVER (PARTITION BY name, email ORDER BY id) as rn
>         FROM employees
>     ) t
>     WHERE rn > 1  -- keep row 1, delete all others
> );
>
> -- Method 2: Using self-join (MySQL style)
> DELETE e1
> FROM employees e1
> JOIN employees e2
>   ON e1.name = e2.name
>  AND e1.id > e2.id;  -- keep the one with smaller id
>
> -- PostgreSQL: Using CTID (physical row identifier)
> DELETE FROM employees
> WHERE ctid NOT IN (
>     SELECT MIN(ctid)
>     FROM employees
>     GROUP BY name, email
> );
>
> -- Find duplicates first (always verify before deleting)
> SELECT name, email, COUNT(*) as count
> FROM employees
> GROUP BY name, email
> HAVING COUNT(*) > 1;
> ```

---

## 💡 FINAL QUICK REVISION

**SQL Basics:**
> ✅ DDL = CREATE/ALTER/DROP | DML = SELECT/INSERT/UPDATE/DELETE
> ✅ WHERE = filter rows | HAVING = filter groups after GROUP BY
> ✅ DELETE = specific rows + rollback | TRUNCATE = all rows fast | DROP = removes table
> ✅ SQL execution order: FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT

**Joins & Set Operations:**
> ✅ INNER = matching only | LEFT = all left + matching right | FULL OUTER = all rows
> ✅ CROSS JOIN = Cartesian product — every row × every row (no condition)
> ✅ Anti-Join = LEFT JOIN + WHERE IS NULL = rows in A but NOT in B
> ✅ UNION = removes duplicates | UNION ALL = keeps duplicates (faster)
> ✅ INTERSECT = rows in both | EXCEPT = rows in first but not second

**Subqueries & Filtering:**
> ✅ Correlated subquery = references outer query, runs once per row (slow)
> ✅ EXISTS = stops at first match, faster for large data
> ✅ NOT EXISTS = safer than NOT IN when NULLs possible
> ✅ CASE WHEN = if-else logic inside queries
> ✅ LIKE = pattern matching | % = any chars | _ = one char | ILIKE = case insensitive

**Advanced:**
> ✅ Window functions use OVER() — don't collapse rows like GROUP BY
> ✅ RANK (gaps) | DENSE_RANK (no gaps) | ROW_NUMBER (always unique)
> ✅ LAG = previous row | LEAD = next row | SUM OVER = running total
> ✅ CTE = WITH clause — makes complex queries readable
> ✅ Materialized View = stores result | View = runs query each time
> ✅ Temp Table = lives for session | CTE = lives for one query only

**Transactions & Integrity:**
> ✅ ACID = Atomicity, Consistency, Isolation, Durability
> ✅ Isolation levels: Read Uncommitted → Read Committed → Repeatable Read → Serializable
> ✅ Deadlock = two transactions waiting for each other — DB kills one to resolve
> ✅ SQL Injection prevention = always use parameterized queries, never string concat

**Data Engineering SQL:**
> ✅ UPSERT = ON CONFLICT DO UPDATE — avoid duplicates in pipelines
> ✅ EXPLAIN ANALYZE = check query performance (Seq Scan = slow, Index Scan = fast)
> ✅ Indexes speed up WHERE, JOIN, ORDER BY — slow down INSERT/UPDATE
> ✅ Sharding = split DB across servers | Partitioning = split within one DB
> ✅ COALESCE = first non-null | NULLIF = return null if equal (avoid divide by zero)
> ✅ COUNT(*) = all rows including NULLs | COUNT(col) = non-null values only
> ✅ SCD Type 2 = add new row with start_date, end_date, is_current flag
> ✅ VACUUM = clean dead rows in PostgreSQL

**Your Experience to Mention:**
> ✅ "At DataWeave I wrote optimized PostgreSQL queries for data validation and reporting"
> ✅ "I handled data versioning and scheduling issues using Python and SQL"

---

*All the best Rajkishor! Python + SQL + Tableau = JSW Data Engineer ready! 🎯*
