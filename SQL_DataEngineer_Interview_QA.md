# 🗄️ SQL Interview Questions & Answers
### JSW Data Engineer Interview Preparation — Friday

---

## 🔵 SECTION 1: SQL BASICS (Q1–Q12)

---

**Q1. What is SQL and why is it important for Data Engineers?**

> SQL (Structured Query Language) is used to store, retrieve, manipulate and manage data in relational databases.
>
> Data Engineers use SQL to:
> - Write data pipelines and transformations
> - Query large datasets from PostgreSQL, MySQL, SQL Server
> - Validate and clean data
> - Build reports and aggregations for dashboards

---

**Q2. What are the different types of SQL commands?**

| Type | Full Form | Commands | Use |
|---|---|---|---|
| DDL | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Create/modify table structure |
| DML | Data Manipulation Language | SELECT, INSERT, UPDATE, DELETE | Work with data |
| DCL | Data Control Language | GRANT, REVOKE | Control permissions |
| TCL | Transaction Control Language | COMMIT, ROLLBACK, SAVEPOINT | Manage transactions |

---

**Q3. What is the difference between WHERE and HAVING?**

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

**Q4. What is the difference between DELETE, TRUNCATE and DROP?**

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| What | Removes specific rows | Removes all rows | Removes entire table |
| WHERE clause | ✅ Yes | ❌ No | ❌ No |
| Rollback possible | ✅ Yes | ❌ No | ❌ No |
| Speed | Slow (logs each row) | Fast | Fast |
| Structure kept | ✅ Yes | ✅ Yes | ❌ No |

---

**Q5. What are different types of JOINs in SQL?**

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

**Q6. What is the difference between UNION and UNION ALL?**

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

**Q7. What is the difference between PRIMARY KEY and FOREIGN KEY?**

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

**Q8. What is the difference between DISTINCT and GROUP BY?**

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

**Q9. What are Constraints in SQL?**

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

**Q10. What is the difference between CHAR, VARCHAR and TEXT?**

| | CHAR(n) | VARCHAR(n) | TEXT |
|---|---|---|---|
| Length | Fixed length | Variable, max n | Unlimited |
| Storage | Always uses n bytes | Uses only what is needed | Uses only what is needed |
| Use | Fixed size data (phone numbers) | Names, emails | Long descriptions |
| Speed | Slightly faster | Slightly slower | Slowest |

---

**Q11. What is NULL in SQL and how to handle it?**

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

**Q12. What is the order of SQL query execution?**

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

**Q13. What are Window Functions? (Very Important)**

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

**Q14. What is the difference between RANK, DENSE_RANK and ROW_NUMBER?**

| Function | Description | Example (100, 90, 90, 80) |
|---|---|---|
| RANK() | Same rank for ties, leaves gaps | 1, 2, 2, 4 |
| DENSE_RANK() | Same rank for ties, no gaps | 1, 2, 2, 3 |
| ROW_NUMBER() | Unique number for every row, no ties | 1, 2, 3, 4 |

---

**Q15. What is a CTE (Common Table Expression)?**

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

**Q16. What is a Subquery?**

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

**Q17. What are Indexes in SQL and why are they important?**

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

**Q18. What is Normalization? Explain 1NF, 2NF, 3NF.**

> Normalization is organizing a database to reduce duplicate data and improve data integrity.

> **1NF (First Normal Form)**
> - Each column has only one value (atomic)
> - No repeating groups
> - Bad: `skills = "Python, SQL, Java"` → Good: separate rows for each skill

> **2NF (Second Normal Form)**
> - Must be in 1NF
> - Every non-key column must depend on the FULL primary key (no partial dependency)

> **3NF (Third Normal Form)**
> - Must be in 2NF
> - No transitive dependency — non-key column should not depend on another non-key column
> - Bad: `zip_code → city` (city depends on zip, not on primary key)

---

**Q19. What is Denormalization and when to use it?**

> Denormalization is intentionally adding redundancy to a database to improve read performance.
>
> - Normalization = less storage, slower reads (need JOINs)
> - Denormalization = more storage, faster reads (no JOINs needed)
>
> Used in: Data warehouses, analytical databases where fast reading is more important than storage.

---

**Q20. What is the difference between OLTP and OLAP?**

| | OLTP | OLAP |
|---|---|---|
| Full form | Online Transaction Processing | Online Analytical Processing |
| Purpose | Day-to-day operations | Analytics and reporting |
| Operations | INSERT, UPDATE, DELETE | SELECT, aggregations |
| Data | Current, real-time | Historical, large volume |
| Schema | Normalized (3NF) | Denormalized (Star/Snowflake) |
| Example | Order placing, banking | Sales dashboards, BI reports |

---

**Q21. What is a View in SQL?**

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

**Q22. What is a Materialized View?**

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

**Q23. What are Stored Procedures and Functions?**

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

**Q24. What are Transactions in SQL?**

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

> **ACID Properties:**
> - **A**tomicity — all or nothing
> - **C**onsistency — data remains valid
> - **I**solation — transactions don't interfere with each other
> - **D**urability — committed data is permanent

---

**Q25. What is the difference between IN, EXISTS and ANY?**

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

**Q26. What is a Recursive CTE?**

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

**Q27. What are Slowly Changing Dimensions (SCD)?**

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

**Q28. What is Partitioning in SQL?**

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

**Q29. What is Query Optimization in SQL?**

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

**Q30. What is EXPLAIN / EXPLAIN ANALYZE in PostgreSQL?**

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

**Q31. What is UPSERT in SQL? (INSERT + UPDATE)**

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

**Q32. What is a Star Schema and Snowflake Schema?**

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

**Q33. What is the difference between Clustered and Non-Clustered Index?**

| | Clustered Index | Non-Clustered Index |
|---|---|---|
| Storage | Physically sorts and stores rows in index order | Separate structure, points to rows |
| Count per table | Only 1 allowed | Multiple allowed |
| Speed | Faster for range queries | Slightly slower |
| Default | PRIMARY KEY is clustered by default | All other indexes |

---

**Q34. How do you find duplicate records in SQL?**

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

**Q35. How do you find the second highest salary?**

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

**Q36. How do you calculate running total and moving average?**

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

**Q37. What is PIVOT in SQL?**

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

**Q38. How do you handle NULL values in SQL joins and aggregations?**

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

**Q39. What is PostgreSQL and how is it different from MySQL?**

| | PostgreSQL | MySQL |
|---|---|---|
| Type | Object-relational database | Relational database |
| ACID | Fully ACID compliant | Depends on engine |
| JSON support | Excellent (JSONB) | Basic |
| Window functions | ✅ Full support | ✅ Support |
| Data types | More types (arrays, hstore, UUID) | Standard types |
| Use case | Complex queries, analytics, data engineering | Web apps, simpler use cases |

---

**Q40. What are common PostgreSQL data types?**

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

**Q41. How do you work with JSON data in PostgreSQL?**

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

**Q42. What is the difference between JSON and JSONB in PostgreSQL?**

| | JSON | JSONB |
|---|---|---|
| Storage | Stored as text, preserves whitespace | Stored as binary, compressed |
| Speed | Slower to query | Faster to query |
| Indexing | Cannot be indexed | Can be indexed (GIN index) |
| Use | When original format must be preserved | When querying JSON fields frequently |

---

**Q43. How do you create and use Sequences in PostgreSQL?**

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

**Q44. What are Common PostgreSQL Functions for Data Engineering?**

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

**Q45. What is a Trigger in SQL?**

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

**Q46. What is Connection Pooling in PostgreSQL?**

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

**Q47. What is VACUUM in PostgreSQL?**

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

**Q48. Write a query to find Top 5 products by sales in each region.**

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

**Q49. Write a query to calculate Month-over-Month (MoM) sales growth.**

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

**Q50. Write a query to find customers who placed orders in both 2023 and 2024.**

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

**Q51. Write a query to detect data quality issues — null values count per column.**

```sql
SELECT
    COUNT(*) - COUNT(customer_id) as null_customer_id,
    COUNT(*) - COUNT(amount)      as null_amount,
    COUNT(*) - COUNT(region)      as null_region,
    COUNT(*) - COUNT(order_date)  as null_order_date
FROM orders;
```

---

**Q52. Write a query to find orders where amount is above the average for their region.**

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

**Q53. Write a query to create a daily sales report with cumulative total.**

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

**Q54. Write a query to find products that were never ordered.**

```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id
WHERE o.product_id IS NULL;
```

---

**Q55. Write a query to flag outlier records (sales > 2 standard deviations from mean).**

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

**Q56. Write a query to implement data versioning (SCD Type 2 insert).**

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

**Q57. Write a query to transpose rows to columns (PIVOT).**

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

**Q58. Write a query to find gaps in sequential data (missing order IDs).**

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

**Q59. What is the difference between INTERSECT, EXCEPT and UNION?**

```sql
-- UNION — all rows from both (removes duplicates)
SELECT city FROM customers UNION SELECT city FROM suppliers;

-- INTERSECT — only rows present in BOTH
SELECT city FROM customers INTERSECT SELECT city FROM suppliers;

-- EXCEPT — rows in first query but NOT in second
SELECT city FROM customers EXCEPT SELECT city FROM suppliers;
```

---

**Q60. Write a query to load data with UPSERT — insert new, update existing.**

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

## 💡 FINAL QUICK REVISION

**SQL Basics:**
> ✅ DDL = CREATE/ALTER/DROP | DML = SELECT/INSERT/UPDATE/DELETE
> ✅ WHERE = filter rows | HAVING = filter groups after GROUP BY
> ✅ DELETE = specific rows + rollback | TRUNCATE = all rows fast | DROP = removes table
> ✅ SQL execution order: FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT

**Joins & Set Operations:**
> ✅ INNER = matching only | LEFT = all left + matching right | FULL OUTER = all rows
> ✅ UNION = removes duplicates | UNION ALL = keeps duplicates (faster)
> ✅ INTERSECT = rows in both | EXCEPT = rows in first but not second

**Advanced:**
> ✅ Window functions use OVER() — don't collapse rows like GROUP BY
> ✅ RANK (gaps) | DENSE_RANK (no gaps) | ROW_NUMBER (always unique)
> ✅ LAG = previous row | LEAD = next row | SUM OVER = running total
> ✅ CTE = WITH clause — makes complex queries readable
> ✅ Materialized View = stores result | View = runs query each time

**Data Engineering SQL:**
> ✅ UPSERT = ON CONFLICT DO UPDATE — avoid duplicates in pipelines
> ✅ EXPLAIN ANALYZE = check query performance (Seq Scan = slow, Index Scan = fast)
> ✅ Indexes speed up WHERE, JOIN, ORDER BY — slow down INSERT/UPDATE
> ✅ Star Schema = denormalized, fast reads | Snowflake Schema = normalized
> ✅ SCD Type 2 = add new row with start_date, end_date, is_current flag
> ✅ VACUUM = clean dead rows in PostgreSQL
> ✅ COALESCE = first non-null value | NULLIF = return null if equal

**Your Experience to Mention:**
> ✅ "At DataWeave I wrote optimized PostgreSQL queries for data validation and reporting"
> ✅ "I handled data versioning and scheduling issues using Python and SQL"

---

*All the best Rajkishor! Python + SQL + Tableau = JSW Data Engineer ready! 🎯*
