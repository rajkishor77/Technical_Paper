# 🗄️ SQL & PostgreSQL — Complete Study Notes
> **After reading this, no concept of SQL or PostgreSQL will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is SQL?](#1-what-is-sql)
2. [What is PostgreSQL?](#2-what-is-postgresql)
3. [SQL vs PostgreSQL vs MySQL](#3-sql-vs-postgresql-vs-mysql)
4. [Database Core Concepts](#4-database-core-concepts)
5. [Data Types](#5-data-types)
6. [DDL — Data Definition Language](#6-ddl--data-definition-language)
7. [DML — Data Manipulation Language](#7-dml--data-manipulation-language)
8. [DQL — Data Query Language](#8-dql--data-query-language)
9. [Joins — All Types](#9-joins--all-types)
10. [Aggregations & GROUP BY](#10-aggregations--group-by)
11. [Subqueries & CTEs](#11-subqueries--ctes)
12. [Window Functions](#12-window-functions)
13. [Indexes](#13-indexes)
14. [Normalization](#14-normalization)
15. [ACID Properties](#15-acid-properties)
16. [Transactions](#16-transactions)
17. [Locking Mechanisms](#17-locking-mechanisms)
18. [Isolation Levels](#18-isolation-levels)
19. [CAP Theorem](#19-cap-theorem)
20. [Triggers](#20-triggers)
21. [Views](#21-views)
22. [Stored Procedures & Functions](#22-stored-procedures--functions)
23. [Constraints](#23-constraints)
24. [PostgreSQL Specific Features](#24-postgresql-specific-features)
25. [PostgreSQL JSON & JSONB](#25-postgresql-json--jsonb)
26. [Performance & Query Optimization](#26-performance--query-optimization)
27. [PostgreSQL Configuration & Admin](#27-postgresql-configuration--admin)
28. [Complete SQL Commands Reference](#28-complete-sql-commands-reference)
29. [Complete psql CLI Reference](#29-complete-psql-cli-reference)
30. [Common Interview Questions](#30-common-interview-questions)

---

## 1. What is SQL?

SQL (**Structured Query Language**) is a standard language used to **create, read, update, and delete** data in relational databases.

### Simple Analogy 📊
> Think of a database as a **library**:
> - **Database** = The entire library building
> - **Table** = A specific bookshelf (e.g., "Science", "History")
> - **Row (Record)** = One book on that shelf
> - **Column (Field)** = A property of every book (title, author, year, ISBN)
> - **Primary Key** = The unique barcode on each book — no two books share it
> - **Foreign Key** = A reference slip inside a book pointing to another book
> - **Index** = The library's catalog — lets you find a book instantly without scanning every shelf
> - **Query** = Your request to the librarian: "Give me all science books published after 2020"
> - **Join** = "Give me books AND their author's biographies combined in one response"

### Key Points
- SQL is a **standard** — works with MySQL, PostgreSQL, SQLite, Oracle, SQL Server
- Designed in the **1970s** by IBM researchers (Edgar F. Codd's relational model)
- SQL is **declarative** — you say WHAT you want, the database figures out HOW to get it
- Organized into sub-languages: **DDL, DML, DQL, DCL, TCL**

### SQL Sub-Languages

| Sub-language | Full Form | Commands | Purpose |
|-------------|-----------|----------|---------|
| **DDL** | Data Definition Language | CREATE, ALTER, DROP, TRUNCATE | Define structure |
| **DML** | Data Manipulation Language | INSERT, UPDATE, DELETE | Modify data |
| **DQL** | Data Query Language | SELECT | Retrieve data |
| **DCL** | Data Control Language | GRANT, REVOKE | Permissions |
| **TCL** | Transaction Control Language | BEGIN, COMMIT, ROLLBACK, SAVEPOINT | Transactions |

---

## 2. What is PostgreSQL?

PostgreSQL is a **free, open-source, advanced relational database system** known for its robustness, standards compliance, and powerful features.

### Key Points
- First released in **1989** at UC Berkeley (as POSTGRES, then PostgreSQL)
- Written in **C**
- Known as the **"most advanced open-source relational database"**
- Fully **ACID compliant**
- Supports **JSON**, **arrays**, **full-text search**, **geospatial data** (PostGIS)
- Used by **Apple, Instagram, Spotify, Uber, Twitch, Reddit, GitHub**
- Handles both **relational AND non-relational** (JSON/JSONB) data

### PostgreSQL Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                     PostgreSQL Server (postmaster)                       │
│                                                                          │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────────────┐  │
│  │  Client Process  │  │  Client Process  │  │   Client Process     │  │
│  │  (your app/psql) │  │  (another app)   │  │   (pgAdmin)          │  │
│  └────────┬─────────┘  └────────┬─────────┘  └──────────┬───────────┘  │
│           │                     │                         │              │
│           └──────────────────── │ ────────────────────────┘             │
│                                 ▼                                        │
│                    ┌────────────────────────┐                           │
│                    │   Postmaster Process   │ ← accepts connections      │
│                    └────────────┬───────────┘                           │
│                                 │ forks                                  │
│                    ┌────────────▼───────────┐                           │
│                    │  Backend Process       │ ← one per client           │
│                    │  (query parser →       │                           │
│                    │   planner → executor)  │                           │
│                    └────────────┬───────────┘                           │
│                                 │                                        │
│  ┌──────────────┐  ┌────────────▼───────────┐  ┌──────────────────┐   │
│  │ Write-Ahead  │  │   Shared Buffer Cache  │  │   Background     │   │
│  │ Log (WAL)    │  │   (shared memory)      │  │   Workers        │   │
│  │ (durability) │  │                        │  │   (autovacuum,   │   │
│  └──────────────┘  └────────────┬───────────┘  │    checkpoint)   │   │
│                                 │               └──────────────────┘   │
│                    ┌────────────▼───────────┐                           │
│                    │   Data Files on Disk   │                           │
│                    │   (tables, indexes)    │                           │
│                    └────────────────────────┘                           │
└─────────────────────────────────────────────────────────────────────────┘
```

---

## 3. SQL vs PostgreSQL vs MySQL

| Feature | **PostgreSQL** | **MySQL** | **SQLite** |
|---------|---------------|-----------|------------|
| **Type** | Object-relational | Relational | Embedded relational |
| **ACID** | ✅ Full | ✅ (InnoDB) | ✅ |
| **JSON support** | ✅ Native JSONB (indexed) | ✅ Basic | ❌ No |
| **Full-text search** | ✅ Built-in | ✅ Basic | ❌ No |
| **Array type** | ✅ Native | ❌ No | ❌ No |
| **Window functions** | ✅ Full | ✅ (8.0+) | ✅ (3.25+) |
| **CTEs** | ✅ | ✅ (8.0+) | ✅ |
| **Geospatial** | ✅ PostGIS extension | ✅ Basic | ❌ No |
| **Replication** | ✅ Advanced | ✅ | ❌ No |
| **Max DB size** | Unlimited | 64TB | 281TB |
| **Performance** | Best for complex queries | Best for simple reads | Lightweight/embedded |
| **License** | PostgreSQL License (free) | GPL / Commercial | Public Domain |
| **Best for** | Complex apps, analytics | Web apps, simple CRUD | Mobile, embedded, testing |

---

## 4. Database Core Concepts

### 4.1 Table, Row, Column

```sql
-- A table is a 2D structure: rows (records) × columns (fields)
-- Example: employees table

-- id  | name    | salary  | dept_id | hired_at
-- ----+---------+---------+---------+----------
-- 1   | Alice   | 75000   | 3       | 2022-01-15
-- 2   | Bob     | 90000   | 1       | 2021-06-01
-- 3   | Carol   | 65000   | 3       | 2023-03-20
```

### 4.2 Primary Key
- **Uniquely identifies** every row in a table
- Cannot be NULL — must always have a value
- Only ONE primary key per table (can be composite)

```sql
-- Single column primary key
CREATE TABLE users (
    id SERIAL PRIMARY KEY,   -- auto-increment integer
    email VARCHAR(255) UNIQUE NOT NULL
);

-- Composite primary key (two columns together are unique)
CREATE TABLE order_items (
    order_id    INTEGER,
    product_id  INTEGER,
    quantity    INTEGER,
    PRIMARY KEY (order_id, product_id)  -- combination must be unique
);
```

### 4.3 Foreign Key
- A column that **references the primary key of another table**
- Enforces **referential integrity** — you can't reference a row that doesn't exist

```sql
CREATE TABLE orders (
    id         SERIAL PRIMARY KEY,
    user_id    INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total      DECIMAL(10, 2),
    created_at TIMESTAMP DEFAULT NOW()
);
```

### 4.4 Schema
- A **namespace** inside a database — groups related tables together
- PostgreSQL default schema: `public`

```sql
CREATE SCHEMA analytics;
CREATE TABLE analytics.reports (id SERIAL, name TEXT);
SELECT * FROM analytics.reports;
```

---

## 5. Data Types

### PostgreSQL Data Types

```sql
-- ─── Numeric ─────────────────────────────────────────────────────────────
SMALLINT          -- -32,768 to 32,767                      (2 bytes)
INTEGER / INT     -- -2,147,483,648 to 2,147,483,647        (4 bytes)
BIGINT            -- Very large integers                     (8 bytes)
SERIAL            -- Auto-increment INTEGER (1, 2, 3...)
BIGSERIAL         -- Auto-increment BIGINT
DECIMAL(p, s)     -- Exact precision: p=total digits, s=decimal places
NUMERIC(p, s)     -- Same as DECIMAL — use for money!
REAL              -- 6 decimal digits precision             (4 bytes)
DOUBLE PRECISION  -- 15 decimal digits precision            (8 bytes)

-- ─── Text ────────────────────────────────────────────────────────────────
CHAR(n)           -- Fixed-length string, padded with spaces
VARCHAR(n)        -- Variable-length string, max n characters
TEXT              -- Unlimited length string (preferred in PostgreSQL)

-- ─── Boolean ─────────────────────────────────────────────────────────────
BOOLEAN           -- TRUE, FALSE, NULL
                  -- Accepts: true/false, yes/no, on/off, 1/0, t/f, y/n

-- ─── Date & Time ─────────────────────────────────────────────────────────
DATE              -- '2024-01-15'
TIME              -- '14:30:00'
TIMESTAMP         -- '2024-01-15 14:30:00' (no timezone)
TIMESTAMPTZ       -- '2024-01-15 14:30:00+05:30' (with timezone) ← PREFERRED
INTERVAL          -- '3 days', '2 hours 30 minutes'

-- ─── JSON ────────────────────────────────────────────────────────────────
JSON              -- Stored as text, re-parsed each query
JSONB             -- Binary JSON, indexed, faster queries ← PREFERRED

-- ─── Arrays ──────────────────────────────────────────────────────────────
INTEGER[]         -- Array of integers: '{1,2,3}'
TEXT[]            -- Array of text: '{"red","green","blue"}'

-- ─── Special ─────────────────────────────────────────────────────────────
UUID              -- '550e8400-e29b-41d4-a716-446655440000'
BYTEA             -- Binary data (images, files)
INET              -- IP address: '192.168.1.1'
CIDR              -- IP network: '192.168.1.0/24'
POINT             -- Geometric point: '(1.5, 2.3)'
TSVECTOR          -- Full-text search document
TSQUERY           -- Full-text search query
ENUM              -- Custom enumerated type
```

---

## 6. DDL — Data Definition Language

### CREATE

```sql
-- ─── Create Database ──────────────────────────────────────────────────────
CREATE DATABASE myapp
    ENCODING 'UTF8'
    LC_COLLATE 'en_US.UTF-8'
    LC_CTYPE 'en_US.UTF-8';


-- ─── Create Table ─────────────────────────────────────────────────────────
CREATE TABLE employees (
    id            SERIAL          PRIMARY KEY,
    first_name    VARCHAR(100)    NOT NULL,
    last_name     VARCHAR(100)    NOT NULL,
    email         VARCHAR(255)    NOT NULL UNIQUE,
    phone         VARCHAR(20),
    salary        NUMERIC(10, 2)  NOT NULL DEFAULT 0,
    is_active     BOOLEAN         NOT NULL DEFAULT TRUE,
    dept_id       INTEGER         REFERENCES departments(id) ON DELETE SET NULL,
    manager_id    INTEGER         REFERENCES employees(id),   -- self-reference
    hired_at      DATE            NOT NULL DEFAULT CURRENT_DATE,
    created_at    TIMESTAMPTZ     NOT NULL DEFAULT NOW(),
    updated_at    TIMESTAMPTZ     NOT NULL DEFAULT NOW()
);


-- ─── Create Table with CHECK constraint ────────────────────────────────────
CREATE TABLE products (
    id          SERIAL PRIMARY KEY,
    name        VARCHAR(200) NOT NULL,
    price       NUMERIC(10, 2) NOT NULL CHECK (price >= 0),
    stock       INTEGER NOT NULL DEFAULT 0 CHECK (stock >= 0),
    category    VARCHAR(50) CHECK (category IN ('Electronics', 'Clothing', 'Books', 'Food')),
    created_at  TIMESTAMPTZ DEFAULT NOW()
);


-- ─── Create Table if not exists ────────────────────────────────────────────
CREATE TABLE IF NOT EXISTS users (
    id    SERIAL PRIMARY KEY,
    email TEXT   NOT NULL UNIQUE
);


-- ─── Create Table from another table ──────────────────────────────────────
CREATE TABLE employees_backup AS
    SELECT * FROM employees;                -- copies data too

CREATE TABLE employees_template AS
    SELECT * FROM employees WHERE 1=0;     -- copies structure only, no data
```

### ALTER

```sql
-- Add a column
ALTER TABLE employees ADD COLUMN middle_name VARCHAR(100);
ALTER TABLE employees ADD COLUMN profile_pic BYTEA;

-- Drop a column
ALTER TABLE employees DROP COLUMN middle_name;
ALTER TABLE employees DROP COLUMN IF EXISTS middle_name;

-- Rename a column
ALTER TABLE employees RENAME COLUMN phone TO phone_number;

-- Change column data type
ALTER TABLE employees ALTER COLUMN salary TYPE BIGINT;
ALTER TABLE employees ALTER COLUMN phone TYPE TEXT;

-- Set/Remove default value
ALTER TABLE employees ALTER COLUMN is_active SET DEFAULT TRUE;
ALTER TABLE employees ALTER COLUMN is_active DROP DEFAULT;

-- Add/Drop NOT NULL constraint
ALTER TABLE employees ALTER COLUMN phone SET NOT NULL;
ALTER TABLE employees ALTER COLUMN phone DROP NOT NULL;

-- Add UNIQUE constraint
ALTER TABLE employees ADD CONSTRAINT uq_employee_email UNIQUE (email);

-- Add CHECK constraint
ALTER TABLE products ADD CONSTRAINT chk_price_positive CHECK (price >= 0);

-- Drop a constraint
ALTER TABLE employees DROP CONSTRAINT uq_employee_email;

-- Rename a table
ALTER TABLE employees RENAME TO staff;

-- Add foreign key
ALTER TABLE orders ADD CONSTRAINT fk_orders_users
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
```

### DROP & TRUNCATE

```sql
-- Drop table (delete table + all data + indexes)
DROP TABLE employees;
DROP TABLE IF EXISTS employees;
DROP TABLE employees CASCADE;       -- also drops dependent objects (views, FKs)

-- Drop database
DROP DATABASE myapp;
DROP DATABASE IF EXISTS myapp;

-- TRUNCATE — delete ALL rows but keep the table structure
TRUNCATE TABLE employees;
TRUNCATE TABLE employees RESTART IDENTITY;  -- also reset auto-increment sequence
TRUNCATE TABLE employees CASCADE;           -- also truncate tables referencing this one

-- Difference: TRUNCATE vs DELETE
-- TRUNCATE: faster (no row-by-row logging), can't be rolled back in MySQL (can in PostgreSQL)
-- DELETE:   slower, logged row-by-row, can be rolled back, can use WHERE
```

---

## 7. DML — Data Manipulation Language

### INSERT

```sql
-- Insert one row
INSERT INTO employees (first_name, last_name, email, salary, dept_id)
VALUES ('Alice', 'Johnson', 'alice@company.com', 75000, 3);

-- Insert multiple rows at once (much faster than multiple single inserts)
INSERT INTO employees (first_name, last_name, email, salary, dept_id)
VALUES
    ('Bob',   'Smith',   'bob@company.com',   90000, 1),
    ('Carol', 'Davis',   'carol@company.com', 65000, 3),
    ('Dave',  'Wilson',  'dave@company.com',  80000, 2),
    ('Eve',   'Martinez','eve@company.com',   70000, 1);

-- Insert and return the created row (PostgreSQL specific)
INSERT INTO employees (first_name, last_name, email, salary)
VALUES ('Frank', 'Brown', 'frank@company.com', 72000)
RETURNING id, created_at;               -- returns the new row's id and timestamp

-- Insert from a SELECT (copy data between tables)
INSERT INTO employees_archive (first_name, last_name, email)
SELECT first_name, last_name, email
FROM employees
WHERE is_active = FALSE;

-- UPSERT — insert or update if conflict (PostgreSQL)
INSERT INTO employees (id, email, salary)
VALUES (1, 'alice@company.com', 80000)
ON CONFLICT (id)
    DO UPDATE SET salary = EXCLUDED.salary, updated_at = NOW();

-- UPSERT — insert or ignore if conflict
INSERT INTO employees (id, email, salary)
VALUES (1, 'alice@company.com', 80000)
ON CONFLICT (id)
    DO NOTHING;
```

### UPDATE

```sql
-- Update one field
UPDATE employees SET salary = 80000 WHERE id = 1;

-- Update multiple fields
UPDATE employees
SET
    salary     = 80000,
    is_active  = TRUE,
    updated_at = NOW()
WHERE id = 1;

-- Update using a calculation
UPDATE employees SET salary = salary * 1.10 WHERE dept_id = 3;   -- 10% raise for dept 3

-- Update with a subquery
UPDATE employees
SET dept_id = (SELECT id FROM departments WHERE name = 'Engineering')
WHERE email LIKE '%@tech.com';

-- Update with JOIN (PostgreSQL syntax)
UPDATE employees e
SET salary = e.salary * 1.15
FROM departments d
WHERE e.dept_id = d.id
  AND d.name = 'Sales';

-- Update and return updated rows
UPDATE employees
SET salary = salary + 5000
WHERE dept_id = 2
RETURNING id, first_name, salary;
```

### DELETE

```sql
-- Delete specific rows
DELETE FROM employees WHERE id = 5;
DELETE FROM employees WHERE is_active = FALSE;
DELETE FROM employees WHERE hired_at < '2020-01-01';

-- Delete with JOIN (PostgreSQL)
DELETE FROM employees e
USING departments d
WHERE e.dept_id = d.id
  AND d.name = 'Archived';

-- Delete and return deleted rows
DELETE FROM employees
WHERE is_active = FALSE
RETURNING id, first_name, last_name;

-- Delete ALL rows (use TRUNCATE instead for performance)
DELETE FROM employees;
```

---

## 8. DQL — Data Query Language

### SELECT — The Most Important Command

```sql
-- ─── Basic SELECT ─────────────────────────────────────────────────────────
SELECT * FROM employees;                          -- all columns (avoid in production)
SELECT id, first_name, last_name FROM employees;  -- specific columns
SELECT first_name, salary * 12 AS annual_salary FROM employees;  -- computed column
SELECT DISTINCT dept_id FROM employees;           -- remove duplicates
SELECT 'Hello World' AS greeting;                 -- no table needed

-- ─── WHERE clause ─────────────────────────────────────────────────────────
SELECT * FROM employees WHERE dept_id = 3;
SELECT * FROM employees WHERE salary > 70000;
SELECT * FROM employees WHERE salary BETWEEN 60000 AND 90000;
SELECT * FROM employees WHERE dept_id IN (1, 2, 3);
SELECT * FROM employees WHERE dept_id NOT IN (4, 5);
SELECT * FROM employees WHERE email LIKE '%@gmail.com';      -- ends with
SELECT * FROM employees WHERE first_name LIKE 'A%';          -- starts with
SELECT * FROM employees WHERE email ILIKE '%alice%';          -- case-insensitive LIKE (PostgreSQL)
SELECT * FROM employees WHERE first_name ~ '^Al';            -- regex match (PostgreSQL)
SELECT * FROM employees WHERE phone IS NULL;
SELECT * FROM employees WHERE phone IS NOT NULL;
SELECT * FROM employees WHERE is_active = TRUE AND salary > 70000;
SELECT * FROM employees WHERE dept_id = 1 OR dept_id = 2;
SELECT * FROM employees WHERE NOT is_active;

-- ─── ORDER BY ─────────────────────────────────────────────────────────────
SELECT * FROM employees ORDER BY salary DESC;                -- highest first
SELECT * FROM employees ORDER BY last_name ASC, salary DESC; -- multi-column sort
SELECT * FROM employees ORDER BY salary DESC NULLS LAST;    -- NULLs at the end

-- ─── LIMIT & OFFSET (Pagination) ──────────────────────────────────────────
SELECT * FROM employees LIMIT 10;                -- first 10 rows
SELECT * FROM employees LIMIT 10 OFFSET 20;     -- rows 21-30 (page 3, size 10)
-- Page N formula: OFFSET = (N - 1) * page_size

-- ─── CASE expression ──────────────────────────────────────────────────────
SELECT
    first_name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'Senior'
        WHEN salary >= 70000  THEN 'Mid-level'
        WHEN salary >= 50000  THEN 'Junior'
        ELSE 'Intern'
    END AS seniority_level
FROM employees;

-- CASE with search (simpler format)
SELECT
    dept_id,
    CASE dept_id
        WHEN 1 THEN 'Engineering'
        WHEN 2 THEN 'Sales'
        WHEN 3 THEN 'HR'
        ELSE 'Other'
    END AS department_name
FROM employees;

-- ─── String functions ──────────────────────────────────────────────────────
SELECT
    UPPER(first_name)                       AS name_upper,
    LOWER(email)                            AS email_lower,
    CONCAT(first_name, ' ', last_name)      AS full_name,
    first_name || ' ' || last_name          AS full_name2,   -- PostgreSQL concat operator
    LENGTH(email)                           AS email_length,
    TRIM('  hello  ')                       AS trimmed,
    LTRIM(first_name),                      -- trim left
    RTRIM(first_name),                      -- trim right
    SUBSTRING(email FROM 1 FOR 5)           AS email_start,  -- first 5 chars
    LEFT(first_name, 3),                    -- first 3 characters
    RIGHT(last_name, 4),                    -- last 4 characters
    REPLACE(email, '@company.com', '')      AS username,
    POSITION('@' IN email)                  AS at_sign_position,
    SPLIT_PART(email, '@', 1)              AS email_username,  -- PostgreSQL
    REPEAT('ha', 3)                         AS hahaha,         -- "hahaha"
    LPAD('5', 3, '0')                      AS padded           -- "005"
FROM employees;

-- ─── Number functions ─────────────────────────────────────────────────────
SELECT
    ROUND(salary / 12.0, 2)    AS monthly_salary,
    CEIL(3.2)                   AS ceiling,          -- 4
    FLOOR(3.9)                  AS floor_val,        -- 3
    ABS(-500)                   AS absolute,         -- 500
    MOD(17, 5)                  AS remainder,        -- 2
    POWER(2, 10)                AS power_of_2,       -- 1024
    SQRT(144)                   AS square_root,      -- 12
    TRUNC(3.99)                 AS truncated,        -- 3 (removes decimals)
    GREATEST(10, 20, 30)        AS greatest_val,     -- 30
    LEAST(10, 20, 30)           AS least_val,        -- 10
    RANDOM()                    AS rand_0_to_1       -- random between 0 and 1
FROM employees;

-- ─── Date functions ────────────────────────────────────────────────────────
SELECT
    NOW()                                   AS current_timestamp,
    CURRENT_DATE                            AS today,
    CURRENT_TIME                            AS time_now,
    EXTRACT(YEAR  FROM hired_at)            AS hire_year,
    EXTRACT(MONTH FROM hired_at)            AS hire_month,
    EXTRACT(DOW   FROM hired_at)            AS day_of_week,   -- 0=Sun, 6=Sat
    DATE_PART('year', hired_at)             AS year2,         -- same as EXTRACT
    AGE(hired_at)                           AS time_at_company,
    AGE(NOW(), hired_at)                    AS tenure,
    hired_at + INTERVAL '30 days'          AS probation_end,
    hired_at::DATE                          AS hired_date,
    TO_CHAR(hired_at, 'DD Mon YYYY')        AS formatted_date,  -- '15 Jan 2024'
    TO_CHAR(salary, 'FM$999,999.00')        AS formatted_salary, -- '$75,000.00'
    DATE_TRUNC('month', hired_at)           AS start_of_month
FROM employees;

-- ─── NULL handling ────────────────────────────────────────────────────────
SELECT
    COALESCE(phone, 'No phone')             AS phone_display,     -- first non-null
    COALESCE(phone, email, 'No contact')    AS contact,           -- tries phone first
    NULLIF(salary, 0)                       AS salary_or_null,    -- NULL if salary = 0
    NULLIF(dept_id, dept_id)               AS always_null
FROM employees;
```

---

## 9. Joins — All Types

```
Setup:
employees:   id | name    | dept_id
             ---|---------|--------
             1  | Alice   | 3
             2  | Bob     | 1
             3  | Carol   | NULL
             4  | Dave    | 99  (dept 99 doesn't exist)

departments: id | name
             ---|-------------
             1  | Engineering
             2  | Sales
             3  | HR
             5  | Marketing   (no employees in Marketing)
```

### All Join Types with Examples

```sql
-- ─── INNER JOIN — only matching rows from both tables ─────────────────────
SELECT e.name, d.name AS dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;
-- Result: Alice-HR, Bob-Engineering
-- Carol (NULL dept) and Dave (dept 99 not in departments) are EXCLUDED
-- Marketing (no employees) is EXCLUDED


-- ─── LEFT JOIN (LEFT OUTER JOIN) — ALL left rows + matching right ─────────
SELECT e.name, d.name AS dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
-- Result: Alice-HR, Bob-Engineering, Carol-NULL, Dave-NULL
-- ALL employees shown, departments are NULL when no match
-- Marketing still EXCLUDED (it's on the right side)


-- ─── RIGHT JOIN (RIGHT OUTER JOIN) — ALL right rows + matching left ────────
SELECT e.name, d.name AS dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.id;
-- Result: Bob-Engineering, NULL-Sales, Alice-HR, NULL-Marketing
-- ALL departments shown, employees are NULL when no match
-- Carol and Dave are EXCLUDED (no match in departments table)


-- ─── FULL OUTER JOIN — ALL rows from both tables ───────────────────────────
SELECT e.name, d.name AS dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
-- Result: ALL employees + ALL departments
-- Alice-HR, Bob-Engineering, Carol-NULL, Dave-NULL, NULL-Sales, NULL-Marketing


-- ─── CROSS JOIN — Cartesian product (every row × every row) ────────────────
SELECT e.name, d.name AS dept_name
FROM employees e
CROSS JOIN departments d;
-- Result: 4 employees × 4 departments = 16 rows
-- Use case: generating all combinations (e.g., all products × all sizes)


-- ─── SELF JOIN — table joined with itself ─────────────────────────────────
-- Find each employee and their manager (manager_id references employees.id)
SELECT
    e.name        AS employee_name,
    m.name        AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.id;


-- ─── Multiple JOINs ──────────────────────────────────────────────────────
SELECT
    e.first_name || ' ' || e.last_name  AS employee,
    d.name                               AS department,
    l.city                               AS office_location,
    p.title                              AS job_title
FROM employees e
INNER JOIN departments  d ON e.dept_id   = d.id
INNER JOIN locations    l ON d.location_id = l.id
LEFT  JOIN job_profiles p ON e.profile_id  = p.id
WHERE e.is_active = TRUE
ORDER BY d.name, e.last_name;


-- ─── JOIN with aggregation ────────────────────────────────────────────────
SELECT
    d.name                  AS department,
    COUNT(e.id)             AS employee_count,
    AVG(e.salary)           AS avg_salary,
    MAX(e.salary)           AS max_salary,
    MIN(e.salary)           AS min_salary
FROM departments d
LEFT JOIN employees e ON d.id = e.dept_id
GROUP BY d.id, d.name
ORDER BY avg_salary DESC;
```

### Join Types Visual Summary

```
INNER JOIN:   ══╦══      (intersection only)
               [A ∩ B]

LEFT JOIN:    ══╦══      (all of A + matching B)
              [A ∪ (A∩B)]

RIGHT JOIN:   ══╦══      (matching A + all of B)
              [(A∩B) ∪ B]

FULL OUTER:   ══╦══      (everything from both)
              [A ∪ B]

CROSS JOIN:   A × B      (every combination)
```

---

## 10. Aggregations & GROUP BY

```sql
-- ─── Aggregate Functions ──────────────────────────────────────────────────
SELECT
    COUNT(*)                AS total_rows,       -- counts ALL rows including NULLs
    COUNT(phone)            AS rows_with_phone,  -- counts only non-NULL values
    COUNT(DISTINCT dept_id) AS unique_depts,     -- counts distinct values
    SUM(salary)             AS total_payroll,
    AVG(salary)             AS average_salary,
    MAX(salary)             AS highest_salary,
    MIN(salary)             AS lowest_salary,
    STDDEV(salary)          AS salary_stddev,    -- standard deviation
    VARIANCE(salary)        AS salary_variance,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY salary) AS median_salary,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY salary) AS p95_salary,
    STRING_AGG(first_name, ', ' ORDER BY first_name) AS all_names  -- concat
FROM employees;


-- ─── GROUP BY — aggregate per group ───────────────────────────────────────
-- Count employees per department
SELECT
    dept_id,
    COUNT(*)        AS employee_count,
    AVG(salary)     AS avg_salary,
    SUM(salary)     AS total_salary
FROM employees
GROUP BY dept_id
ORDER BY employee_count DESC;


-- GROUP BY with JOIN
SELECT
    d.name              AS department,
    COUNT(e.id)         AS headcount,
    ROUND(AVG(e.salary), 0) AS avg_salary
FROM departments d
LEFT JOIN employees e ON d.id = e.dept_id
GROUP BY d.id, d.name
ORDER BY headcount DESC;


-- ─── WHERE vs HAVING ──────────────────────────────────────────────────────
-- WHERE filters ROWS (before grouping)
-- HAVING filters GROUPS (after aggregation)

SELECT
    dept_id,
    COUNT(*) AS employee_count,
    AVG(salary) AS avg_salary
FROM employees
WHERE is_active = TRUE              -- ← filter rows BEFORE grouping
GROUP BY dept_id
HAVING COUNT(*) > 3                 -- ← filter groups AFTER aggregation
   AND AVG(salary) > 60000
ORDER BY avg_salary DESC;


-- ─── ROLLUP — subtotals and grand total ───────────────────────────────────
SELECT
    dept_id,
    is_active,
    COUNT(*) AS count
FROM employees
GROUP BY ROLLUP (dept_id, is_active);
-- Produces: individual groups + department subtotals + grand total


-- ─── CUBE — all combinations of subtotals ─────────────────────────────────
SELECT dept_id, is_active, COUNT(*)
FROM employees
GROUP BY CUBE (dept_id, is_active);


-- ─── FILTER clause (PostgreSQL) — conditional aggregation ─────────────────
SELECT
    dept_id,
    COUNT(*) AS total,
    COUNT(*) FILTER (WHERE salary > 75000) AS high_earners,
    COUNT(*) FILTER (WHERE is_active = TRUE) AS active,
    AVG(salary) FILTER (WHERE is_active = TRUE) AS active_avg_salary
FROM employees
GROUP BY dept_id;
```

---

## 11. Subqueries & CTEs

### Subqueries

```sql
-- ─── Scalar subquery — returns one value ──────────────────────────────────
SELECT first_name, salary,
    (SELECT AVG(salary) FROM employees) AS company_avg
FROM employees;


-- ─── Subquery in WHERE ─────────────────────────────────────────────────────
-- Employees earning above the company average
SELECT first_name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Employees in the same department as Alice
SELECT first_name
FROM employees
WHERE dept_id = (
    SELECT dept_id FROM employees WHERE first_name = 'Alice' LIMIT 1
);

-- Employees NOT in any department with fewer than 3 people
SELECT first_name FROM employees
WHERE dept_id NOT IN (
    SELECT dept_id FROM employees
    GROUP BY dept_id
    HAVING COUNT(*) < 3
);


-- ─── EXISTS / NOT EXISTS ──────────────────────────────────────────────────
-- Departments that have at least one employee
SELECT name FROM departments d
WHERE EXISTS (
    SELECT 1 FROM employees e WHERE e.dept_id = d.id
);

-- Departments with NO employees
SELECT name FROM departments d
WHERE NOT EXISTS (
    SELECT 1 FROM employees e WHERE e.dept_id = d.id
);


-- ─── Subquery in FROM (derived table) ─────────────────────────────────────
SELECT dept_id, avg_salary
FROM (
    SELECT dept_id, AVG(salary) AS avg_salary
    FROM employees
    GROUP BY dept_id
) AS dept_averages
WHERE avg_salary > 70000;
```

### CTEs (Common Table Expressions)

```sql
-- ─── Basic CTE — with (named subquery) ────────────────────────────────────
WITH dept_stats AS (
    SELECT
        dept_id,
        COUNT(*) AS headcount,
        AVG(salary) AS avg_salary,
        MAX(salary) AS max_salary
    FROM employees
    GROUP BY dept_id
)
SELECT
    d.name AS department,
    ds.headcount,
    ROUND(ds.avg_salary, 0) AS avg_salary,
    ds.max_salary
FROM dept_stats ds
JOIN departments d ON ds.dept_id = d.id
ORDER BY ds.avg_salary DESC;


-- ─── Multiple CTEs ────────────────────────────────────────────────────────
WITH
active_employees AS (
    SELECT * FROM employees WHERE is_active = TRUE
),
dept_stats AS (
    SELECT dept_id, COUNT(*) AS count, AVG(salary) AS avg_sal
    FROM active_employees
    GROUP BY dept_id
),
top_depts AS (
    SELECT * FROM dept_stats WHERE count > 5
)
SELECT d.name, td.count, td.avg_sal
FROM top_depts td
JOIN departments d ON td.dept_id = d.id;


-- ─── Recursive CTE — for hierarchical data (org charts, trees) ────────────
-- Find all employees in a manager's team (all levels)
WITH RECURSIVE team AS (
    -- Base case: the manager themselves
    SELECT id, first_name, manager_id, 1 AS level
    FROM employees
    WHERE id = 5   -- manager id = 5

    UNION ALL

    -- Recursive case: employees reporting to anyone in 'team'
    SELECT e.id, e.first_name, e.manager_id, t.level + 1
    FROM employees e
    INNER JOIN team t ON e.manager_id = t.id
)
SELECT level, first_name FROM team ORDER BY level, first_name;


-- ─── CTE for data modification ────────────────────────────────────────────
-- CTE with DELETE (find and delete in one statement)
WITH old_inactive AS (
    SELECT id FROM employees
    WHERE is_active = FALSE AND hired_at < NOW() - INTERVAL '5 years'
)
DELETE FROM employees WHERE id IN (SELECT id FROM old_inactive);
```

---

## 12. Window Functions

Window functions perform calculations **across related rows** without collapsing them into groups — unlike GROUP BY which loses individual rows.

```sql
-- ─── Syntax ───────────────────────────────────────────────────────────────
-- function() OVER (
--     PARTITION BY column(s)    -- like GROUP BY but keeps all rows
--     ORDER BY column(s)        -- order within each partition
--     ROWS/RANGE frame          -- which rows to include in calculation
-- )

-- ─── Ranking functions ────────────────────────────────────────────────────
SELECT
    first_name,
    dept_id,
    salary,
    RANK()       OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rank_in_dept,
    DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dense_rank,
    ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS row_num,
    NTILE(4)     OVER (ORDER BY salary DESC)                      AS salary_quartile
FROM employees;
-- RANK:       1, 2, 2, 4 (skips 3 after tie)
-- DENSE_RANK: 1, 2, 2, 3 (no skipping)
-- ROW_NUMBER: 1, 2, 3, 4 (unique, no ties)
-- NTILE(4):   splits into 4 equal groups (quartiles)


-- ─── Aggregation windows ──────────────────────────────────────────────────
SELECT
    first_name,
    dept_id,
    salary,
    SUM(salary)    OVER (PARTITION BY dept_id)                    AS dept_total,
    AVG(salary)    OVER (PARTITION BY dept_id)                    AS dept_avg,
    MAX(salary)    OVER (PARTITION BY dept_id)                    AS dept_max,
    COUNT(*)       OVER (PARTITION BY dept_id)                    AS dept_headcount,
    SUM(salary)    OVER ()                                        AS company_total,
    salary / SUM(salary) OVER (PARTITION BY dept_id) * 100       AS pct_of_dept
FROM employees;


-- ─── Running total / cumulative ────────────────────────────────────────────
SELECT
    hired_at,
    salary,
    SUM(salary) OVER (ORDER BY hired_at
                      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
                     ) AS running_total_payroll
FROM employees
ORDER BY hired_at;


-- ─── LAG and LEAD — access previous/next rows ─────────────────────────────
SELECT
    first_name,
    salary,
    LAG(salary)  OVER (ORDER BY salary)          AS prev_salary,
    LEAD(salary) OVER (ORDER BY salary)          AS next_salary,
    LAG(salary, 2, 0) OVER (ORDER BY salary)    AS two_before,  -- 2 rows back, default 0
    salary - LAG(salary) OVER (ORDER BY salary) AS salary_gap
FROM employees;


-- ─── FIRST_VALUE, LAST_VALUE, NTH_VALUE ───────────────────────────────────
SELECT
    first_name,
    dept_id,
    salary,
    FIRST_VALUE(salary) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS dept_highest,
    LAST_VALUE(salary)  OVER (PARTITION BY dept_id ORDER BY salary DESC
                               ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
                              ) AS dept_lowest,
    NTH_VALUE(salary, 2) OVER (PARTITION BY dept_id ORDER BY salary DESC) AS second_highest
FROM employees;


-- ─── Find top earner per department ───────────────────────────────────────
SELECT * FROM (
    SELECT
        first_name,
        dept_id,
        salary,
        RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rnk
    FROM employees
) ranked
WHERE rnk = 1;
```

---

## 13. Indexes

```sql
-- ─── Basic Index ──────────────────────────────────────────────────────────
CREATE INDEX idx_employees_email       ON employees(email);
CREATE INDEX idx_employees_dept        ON employees(dept_id);
CREATE INDEX idx_employees_salary      ON employees(salary);
CREATE INDEX idx_employees_hired_at    ON employees(hired_at DESC);


-- ─── Unique Index ─────────────────────────────────────────────────────────
CREATE UNIQUE INDEX uq_employees_email ON employees(email);


-- ─── Composite Index (multi-column) ───────────────────────────────────────
-- Order matters: most selective column first, most used in queries first
CREATE INDEX idx_employees_dept_salary ON employees(dept_id, salary);
-- Supports: WHERE dept_id = 3
-- Supports: WHERE dept_id = 3 AND salary > 70000
-- Does NOT support: WHERE salary > 70000 (alone, without dept_id)


-- ─── Partial Index — index only part of the table ─────────────────────────
-- Only index active employees — smaller, faster
CREATE INDEX idx_active_employees ON employees(dept_id) WHERE is_active = TRUE;


-- ─── Expression Index (Functional Index) ──────────────────────────────────
-- Index the RESULT of a function — makes queries using that function fast
CREATE INDEX idx_email_lower ON employees(LOWER(email));
-- Now this query uses the index:
-- SELECT * FROM employees WHERE LOWER(email) = 'alice@company.com';


-- ─── GIN Index — for arrays, JSONB, full-text search ─────────────────────
CREATE INDEX idx_product_tags ON products USING GIN (tags);
CREATE INDEX idx_product_data ON products USING GIN (metadata);  -- for JSONB
CREATE INDEX idx_employee_search ON employees USING GIN (to_tsvector('english', first_name || ' ' || last_name));


-- ─── GiST Index — for geometric/range types ───────────────────────────────
CREATE INDEX idx_employee_salary_range ON employees USING GIST (salary);


-- ─── BRIN Index — for large tables with naturally ordered data ────────────
-- Very small, useful for timestamps, sequential IDs, time-series
CREATE INDEX idx_logs_created ON system_logs USING BRIN (created_at);


-- ─── Hash Index — for equality lookups only ───────────────────────────────
CREATE INDEX idx_employees_id_hash ON employees USING HASH (id);


-- ─── Concurrent Index (no table lock) ─────────────────────────────────────
-- Build index without locking the table — safe to run on production
CREATE INDEX CONCURRENTLY idx_employees_phone ON employees(phone);


-- ─── Index Management ─────────────────────────────────────────────────────
-- List all indexes on a table
SELECT indexname, indexdef FROM pg_indexes WHERE tablename = 'employees';

-- Drop an index
DROP INDEX idx_employees_email;
DROP INDEX IF EXISTS idx_employees_email;
DROP INDEX CONCURRENTLY idx_employees_email;   -- drop without locking

-- Check if a query uses an index
EXPLAIN SELECT * FROM employees WHERE email = 'alice@company.com';
EXPLAIN ANALYZE SELECT * FROM employees WHERE email = 'alice@company.com';


-- ─── When to use each index type ──────────────────────────────────────────
-- B-Tree (default): Most queries — equality, range, ORDER BY, LIKE 'prefix%'
-- Hash:             Equality only (=) — slightly faster than B-Tree for pure equality
-- GIN:              Arrays, JSONB, full-text search, @> operator
-- GiST:            Geometric types, range types, full-text search
-- BRIN:            Very large tables with naturally ordered data (timestamps, IDs)
-- Partial:         When you query a subset frequently (WHERE is_active = TRUE)
-- Expression:      When you frequently query LOWER(col) or other functions
```

---

## 14. Normalization

Normalization is the process of organizing a database to **reduce redundancy** and **improve data integrity**.

### 1NF — First Normal Form
**Rule:** Each column must contain **atomic (indivisible) values**. No repeating groups.

```sql
-- ❌ Bad (violates 1NF)
-- id | name  | phone_numbers
-- 1  | Alice | "9876543210, 9123456789"  ← multiple values in one cell!

-- ✅ Good (1NF)
-- employees: id | name
-- phones:    employee_id | phone_number
--            1           | 9876543210
--            1           | 9123456789

CREATE TABLE employee_phones (
    employee_id INTEGER REFERENCES employees(id),
    phone_number VARCHAR(20),
    PRIMARY KEY (employee_id, phone_number)
);
```

### 2NF — Second Normal Form
**Rule:** Must be in 1NF + every non-key column must depend on the **ENTIRE primary key** (no partial dependencies — applies to composite keys).

```sql
-- ❌ Bad (violates 2NF — composite key: order_id + product_id)
-- order_id | product_id | quantity | product_name | product_price
-- 1        | 101        | 2        | Laptop       | 49999
-- product_name depends only on product_id, not on (order_id + product_id) together!

-- ✅ Good (2NF)
-- order_items: order_id | product_id | quantity
-- products:    id       | name       | price

CREATE TABLE products    (id SERIAL PRIMARY KEY, name TEXT, price NUMERIC);
CREATE TABLE order_items (order_id INT, product_id INT REFERENCES products(id), quantity INT,
                          PRIMARY KEY (order_id, product_id));
```

### 3NF — Third Normal Form
**Rule:** Must be in 2NF + no **transitive dependencies** (non-key column depending on another non-key column).

```sql
-- ❌ Bad (violates 3NF)
-- id | name  | dept_id | dept_name | dept_location
-- dept_name depends on dept_id, not directly on id (employee's primary key)
-- That's: id → dept_id → dept_name (transitive!)

-- ✅ Good (3NF)
-- employees:   id | name | dept_id
-- departments: id | name | location

CREATE TABLE departments (id SERIAL PRIMARY KEY, name TEXT, location TEXT);
CREATE TABLE employees   (id SERIAL PRIMARY KEY, name TEXT, dept_id INT REFERENCES departments(id));
```

### BCNF — Boyce-Codd Normal Form
**Rule:** Must be in 3NF + for every functional dependency (X → Y), X must be a **superkey** (can uniquely identify rows). Stricter than 3NF.

### Denormalization — When to Break the Rules
```
When to deliberately denormalize:
✅ Read-heavy applications where JOIN performance is critical
✅ Reporting tables / data warehouses
✅ Caching frequently computed values (pre-computed totals)
✅ When the data changes rarely but is read millions of times
```

---

## 15. ACID Properties

**ACID** ensures database transactions are **reliable and consistent**.

| Property | Meaning | Example |
|----------|---------|---------|
| **Atomicity** | All operations in a transaction succeed, or NONE do | Bank transfer: debit + credit both happen or neither does |
| **Consistency** | Transaction takes DB from one valid state to another | Can't transfer more money than the account has |
| **Isolation** | Concurrent transactions don't interfere with each other | Two users booking same seat see independent states |
| **Durability** | Committed changes survive crashes | Power failure after COMMIT doesn't lose the data |

```sql
-- Atomicity example — both updates succeed or both are rolled back
BEGIN;
    UPDATE accounts SET balance = balance - 500 WHERE id = 1;   -- debit
    UPDATE accounts SET balance = balance + 500 WHERE id = 2;   -- credit
    -- If EITHER fails, the entire transaction is rolled back
COMMIT;  -- only now are changes permanent

-- Durability: PostgreSQL uses WAL (Write-Ahead Log)
-- All changes written to WAL first → if crash occurs → WAL used to recover
```

---

## 16. Transactions

```sql
-- ─── Basic Transaction ────────────────────────────────────────────────────
BEGIN;  -- or BEGIN TRANSACTION; or START TRANSACTION;

    UPDATE inventory SET quantity = quantity - 1 WHERE product_id = 101;
    INSERT INTO sales (product_id, quantity, sold_at) VALUES (101, 1, NOW());

COMMIT;     -- make changes permanent
-- OR:
ROLLBACK;   -- undo all changes since BEGIN


-- ─── SAVEPOINT — partial rollback ─────────────────────────────────────────
BEGIN;
    INSERT INTO orders (user_id, total) VALUES (1, 1000) RETURNING id;

    SAVEPOINT after_order;   -- create a checkpoint

    INSERT INTO payments (order_id, amount) VALUES (1001, 1000);
    -- If payment fails:
    ROLLBACK TO SAVEPOINT after_order;  -- undo only payment, keep order

    -- Try alternative payment:
    INSERT INTO payments (order_id, amount) VALUES (1001, 1000);

    RELEASE SAVEPOINT after_order;   -- release the savepoint (optional)

COMMIT;


-- ─── Transaction in PostgreSQL PL/pgSQL function ───────────────────────────
CREATE OR REPLACE FUNCTION transfer_money(
    sender_id    INTEGER,
    receiver_id  INTEGER,
    amount       NUMERIC
) RETURNS VOID AS $$
BEGIN
    -- Check sufficient balance
    IF (SELECT balance FROM accounts WHERE id = sender_id) < amount THEN
        RAISE EXCEPTION 'Insufficient balance in account %', sender_id;
    END IF;

    -- Deduct from sender
    UPDATE accounts SET balance = balance - amount WHERE id = sender_id;

    -- Add to receiver
    UPDATE accounts SET balance = balance + amount WHERE id = receiver_id;

    -- Insert transaction record
    INSERT INTO transactions (sender_id, receiver_id, amount, created_at)
    VALUES (sender_id, receiver_id, amount, NOW());

    -- All succeeded — function ends here and PostgreSQL auto-commits
EXCEPTION
    WHEN OTHERS THEN
        RAISE;  -- re-raise the exception, PostgreSQL auto-rolls back
END;
$$ LANGUAGE plpgsql;


-- ─── Autocommit ────────────────────────────────────────────────────────────
-- In PostgreSQL, each individual SQL statement is automatically wrapped in a
-- transaction if you don't explicitly BEGIN one.
-- psql default: autocommit = ON (each statement commits immediately)
```

---

## 17. Locking Mechanisms

```sql
-- ─── Row-level locks ──────────────────────────────────────────────────────
-- FOR UPDATE — locks selected rows, prevents other transactions from modifying them
BEGIN;
    SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
    -- Other transactions: SELECT is fine, UPDATE/DELETE blocks until this commits
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;

-- FOR SHARE — locks rows for reading, prevents modification but allows other reads
SELECT * FROM products WHERE id = 101 FOR SHARE;

-- FOR NO KEY UPDATE — like FOR UPDATE but allows FK references to proceed
SELECT * FROM orders WHERE id = 5 FOR NO KEY UPDATE;

-- SKIP LOCKED — skip rows already locked (useful for job queues)
SELECT * FROM job_queue WHERE status = 'pending' LIMIT 1 FOR UPDATE SKIP LOCKED;


-- ─── Table-level locks ────────────────────────────────────────────────────
LOCK TABLE employees IN SHARE MODE;          -- allow reads, block writes
LOCK TABLE employees IN EXCLUSIVE MODE;      -- block all access by others
LOCK TABLE employees IN ROW EXCLUSIVE MODE;  -- default for INSERT/UPDATE/DELETE


-- ─── Advisory locks — application-level locks ─────────────────────────────
SELECT pg_advisory_lock(12345);      -- acquire lock (blocks if taken)
SELECT pg_try_advisory_lock(12345);  -- try to acquire, returns true/false immediately
SELECT pg_advisory_unlock(12345);    -- release lock

-- Use case: ensure only one process runs a batch job at a time
-- Process 1: pg_try_advisory_lock(1001) → true  → runs the job
-- Process 2: pg_try_advisory_lock(1001) → false → skips (another is running)


-- ─── Deadlock ─────────────────────────────────────────────────────────────
-- Deadlock: Transaction A waits for B's lock, and B waits for A's lock → both stuck
-- PostgreSQL detects deadlocks automatically and kills one transaction

-- Prevent deadlocks: always acquire locks in the SAME ORDER across transactions
-- ❌ Bad:
-- Transaction 1: LOCK TABLE A, then LOCK TABLE B
-- Transaction 2: LOCK TABLE B, then LOCK TABLE A  ← deadlock possible!

-- ✅ Good:
-- Both transactions: LOCK TABLE A, then LOCK TABLE B (same order = no deadlock)


-- ─── Check for locks in PostgreSQL ────────────────────────────────────────
SELECT pid, usename, pg_blocking_pids(pid) AS blocked_by, query, state
FROM pg_stat_activity
WHERE cardinality(pg_blocking_pids(pid)) > 0;  -- show all blocked queries

-- View current locks
SELECT * FROM pg_locks;
```

---

## 18. Isolation Levels

Isolation levels control what **partial data** one transaction can see from another.

### The Problems They Solve

| Problem | Description |
|---------|-------------|
| **Dirty Read** | Reading data from a transaction that hasn't committed yet |
| **Non-Repeatable Read** | Same query returns different data within the same transaction (another committed between reads) |
| **Phantom Read** | New rows appear in a repeated query (another transaction inserted rows) |
| **Serialization Anomaly** | Overall result is inconsistent even though individual operations look fine |

### Isolation Levels

| Level | Dirty Read | Non-Repeatable Read | Phantom Read | Performance |
|-------|-----------|---------------------|-------------|-------------|
| **Read Uncommitted** | Possible ❌ | Possible ❌ | Possible ❌ | Fastest |
| **Read Committed** | ✅ Prevented | Possible ❌ | Possible ❌ | Fast |
| **Repeatable Read** | ✅ Prevented | ✅ Prevented | Possible ❌ | Medium |
| **Serializable** | ✅ Prevented | ✅ Prevented | ✅ Prevented | Slowest |

```sql
-- Set isolation level for a transaction
BEGIN;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;   -- PostgreSQL default
-- other options: READ UNCOMMITTED, REPEATABLE READ, SERIALIZABLE
-- ...
COMMIT;

-- PostgreSQL actually prevents phantom reads at REPEATABLE READ too (MVCC)
-- So PostgreSQL effectively has: Read Committed and Serializable as the useful levels

-- Set for the whole session
SET SESSION CHARACTERISTICS AS TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Check current isolation level
SHOW TRANSACTION ISOLATION LEVEL;
```

---

## 19. CAP Theorem

The CAP theorem states that a **distributed database** can guarantee at most **2 out of 3** properties simultaneously.

| Property | Meaning |
|----------|---------|
| **C**onsistency | All nodes see the same data at the same time |
| **A**vailability | Every request gets a response (might not be latest data) |
| **P**artition Tolerance | System keeps working even when network partitions occur |

```
In a DISTRIBUTED system:
Network partitions ALWAYS happen eventually → P is non-negotiable

So the real choice is: CP or AP

CP (Consistency + Partition Tolerance):
  → When partition occurs: stop serving requests until consistent
  → Examples: PostgreSQL (with sync replication), HBase, ZooKeeper
  → Use when: financial data, medical records, inventory

AP (Availability + Partition Tolerance):
  → When partition occurs: serve requests with possibly stale data
  → Examples: Cassandra, CouchDB, DynamoDB
  → Use when: social media feeds, analytics, shopping carts

CA (Consistency + Availability):
  → Only possible in a single-node system (no network partitions possible)
  → Examples: Traditional single-server PostgreSQL, MySQL
  → Not realistic for distributed systems
```

```
Trade-off examples:
┌─────────────────────────────────────────────────────────────┐
│ Banking app         → CP  (always consistent, might be slow) │
│ Social media feed   → AP  (might see old data, always fast)  │
│ E-commerce cart     → AP  (show stale cart, fix at checkout)  │
│ Seat reservations   → CP  (can't double-book!)                │
│ DNS resolution      → AP  (stale cache OK, always responds)   │
└─────────────────────────────────────────────────────────────┘
```

---

## 20. Triggers

```sql
-- ─── BEFORE trigger — runs BEFORE the operation ────────────────────────────
CREATE OR REPLACE FUNCTION set_updated_at()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();   -- modify the NEW row before it's saved
    RETURN NEW;               -- must RETURN NEW for BEFORE trigger
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_employees_updated_at
    BEFORE UPDATE ON employees
    FOR EACH ROW
    EXECUTE FUNCTION set_updated_at();


-- ─── AFTER trigger — runs AFTER the operation ─────────────────────────────
CREATE OR REPLACE FUNCTION log_salary_change()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.salary <> NEW.salary THEN
        INSERT INTO salary_audit_log (
            employee_id, old_salary, new_salary, changed_at, changed_by
        ) VALUES (
            NEW.id, OLD.salary, NEW.salary, NOW(), current_user
        );
    END IF;
    RETURN NULL;   -- RETURN value ignored for AFTER trigger
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_log_salary_change
    AFTER UPDATE OF salary ON employees
    FOR EACH ROW
    EXECUTE FUNCTION log_salary_change();


-- ─── INSTEAD OF trigger — for views ────────────────────────────────────────
CREATE OR REPLACE FUNCTION update_employee_view()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE employees
    SET first_name = NEW.first_name, salary = NEW.salary
    WHERE id = NEW.id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_update_employee_view
    INSTEAD OF UPDATE ON employee_summary_view
    FOR EACH ROW
    EXECUTE FUNCTION update_employee_view();


-- ─── Statement-level trigger — runs once per statement ─────────────────────
CREATE OR REPLACE FUNCTION notify_bulk_delete()
RETURNS TRIGGER AS $$
BEGIN
    -- RAISE NOTICE runs once even if 1000 rows deleted
    RAISE NOTICE 'Bulk delete performed on employees table at %', NOW();
    RETURN NULL;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_bulk_delete_notice
    AFTER DELETE ON employees
    FOR EACH STATEMENT   -- ← once per DELETE statement, not per row
    EXECUTE FUNCTION notify_bulk_delete();


-- ─── Trigger management ────────────────────────────────────────────────────
-- Disable a trigger
ALTER TABLE employees DISABLE TRIGGER trg_employees_updated_at;

-- Enable a trigger
ALTER TABLE employees ENABLE TRIGGER trg_employees_updated_at;

-- Disable all triggers on a table (use with care!)
ALTER TABLE employees DISABLE TRIGGER ALL;

-- Drop a trigger
DROP TRIGGER trg_employees_updated_at ON employees;

-- List triggers
SELECT trigger_name, event_manipulation, event_object_table, action_timing
FROM information_schema.triggers
WHERE event_object_schema = 'public';
```

---

## 21. Views

```sql
-- ─── Simple View ──────────────────────────────────────────────────────────
CREATE VIEW active_employees AS
    SELECT id, first_name, last_name, email, salary, dept_id
    FROM employees
    WHERE is_active = TRUE;

-- Use exactly like a table
SELECT * FROM active_employees WHERE dept_id = 3;
SELECT COUNT(*) FROM active_employees;


-- ─── View with JOIN ────────────────────────────────────────────────────────
CREATE VIEW employee_details AS
    SELECT
        e.id,
        e.first_name || ' ' || e.last_name  AS full_name,
        e.email,
        e.salary,
        d.name  AS department,
        m.first_name || ' ' || m.last_name  AS manager_name
    FROM employees e
    LEFT JOIN departments d ON e.dept_id = d.id
    LEFT JOIN employees m ON e.manager_id = m.id
    WHERE e.is_active = TRUE;


-- ─── Materialized View (PostgreSQL) — stored on disk, refreshed manually ────
-- Regular view: runs the query every time you SELECT from it
-- Materialized view: stores the result → fast reads → must refresh to update

CREATE MATERIALIZED VIEW dept_salary_stats AS
    SELECT
        d.name AS department,
        COUNT(e.id) AS headcount,
        ROUND(AVG(e.salary), 0) AS avg_salary,
        SUM(e.salary) AS total_payroll
    FROM employees e
    JOIN departments d ON e.dept_id = d.id
    WHERE e.is_active = TRUE
    GROUP BY d.id, d.name
WITH DATA;  -- populate immediately

-- Index a materialized view for fast queries
CREATE UNIQUE INDEX ON dept_salary_stats(department);

-- Refresh the materialized view (run when data changes)
REFRESH MATERIALIZED VIEW dept_salary_stats;
REFRESH MATERIALIZED VIEW CONCURRENTLY dept_salary_stats;  -- without locking reads


-- ─── Drop views ────────────────────────────────────────────────────────────
DROP VIEW employee_details;
DROP VIEW IF EXISTS employee_details;
DROP VIEW employee_details CASCADE;   -- drop views that depend on this one

DROP MATERIALIZED VIEW dept_salary_stats;
```

---

## 22. Stored Procedures & Functions

```sql
-- ─── Function — returns a value ────────────────────────────────────────────
CREATE OR REPLACE FUNCTION get_employee_count(dept_name TEXT)
RETURNS INTEGER AS $$
DECLARE
    emp_count INTEGER;
BEGIN
    SELECT COUNT(*) INTO emp_count
    FROM employees e
    JOIN departments d ON e.dept_id = d.id
    WHERE d.name = dept_name AND e.is_active = TRUE;

    RETURN emp_count;
END;
$$ LANGUAGE plpgsql;

-- Call a function
SELECT get_employee_count('Engineering');


-- ─── Function returning a table ────────────────────────────────────────────
CREATE OR REPLACE FUNCTION get_dept_employees(dept_name TEXT)
RETURNS TABLE (
    employee_id  INTEGER,
    full_name    TEXT,
    salary       NUMERIC
) AS $$
BEGIN
    RETURN QUERY
        SELECT e.id, e.first_name || ' ' || e.last_name, e.salary
        FROM employees e
        JOIN departments d ON e.dept_id = d.id
        WHERE d.name = dept_name AND e.is_active = TRUE
        ORDER BY e.salary DESC;
END;
$$ LANGUAGE plpgsql;

SELECT * FROM get_dept_employees('Engineering');


-- ─── Stored Procedure (PostgreSQL 11+) — no return value, supports COMMIT ──
CREATE OR REPLACE PROCEDURE process_payroll(dept_id_param INTEGER)
LANGUAGE plpgsql AS $$
DECLARE
    emp RECORD;
BEGIN
    FOR emp IN
        SELECT id, salary FROM employees WHERE dept_id = dept_id_param
    LOOP
        INSERT INTO payroll_log (employee_id, amount, paid_at)
        VALUES (emp.id, emp.salary, NOW());
    END LOOP;

    -- Procedures CAN commit/rollback inside them (unlike functions)
    COMMIT;
END;
$$;

-- Call a procedure
CALL process_payroll(3);


-- ─── Drop functions ────────────────────────────────────────────────────────
DROP FUNCTION get_employee_count(TEXT);
DROP FUNCTION IF EXISTS get_employee_count(TEXT);
DROP PROCEDURE process_payroll(INTEGER);
```

---

## 23. Constraints

```sql
-- ─── PRIMARY KEY ──────────────────────────────────────────────────────────
CREATE TABLE users (id SERIAL PRIMARY KEY);
-- or: CONSTRAINT pk_users PRIMARY KEY (id)

-- ─── UNIQUE ───────────────────────────────────────────────────────────────
CREATE TABLE users (email TEXT UNIQUE);
-- or: CONSTRAINT uq_email UNIQUE (email)
-- Composite unique:
ALTER TABLE order_items ADD CONSTRAINT uq_order_product UNIQUE (order_id, product_id);

-- ─── NOT NULL ─────────────────────────────────────────────────────────────
CREATE TABLE users (name TEXT NOT NULL, email TEXT NOT NULL);

-- ─── CHECK ────────────────────────────────────────────────────────────────
CREATE TABLE products (
    price    NUMERIC CHECK (price >= 0),
    stock    INTEGER CHECK (stock >= 0),
    category TEXT CHECK (category IN ('A', 'B', 'C')),
    end_date DATE,
    start_date DATE,
    CONSTRAINT chk_dates CHECK (end_date > start_date)
);

-- ─── FOREIGN KEY — with actions ───────────────────────────────────────────
CREATE TABLE orders (
    id      SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id)
        ON DELETE CASCADE        -- delete orders when user deleted
        ON UPDATE CASCADE,       -- update FK when user.id changes
    dept_id INTEGER REFERENCES departments(id)
        ON DELETE SET NULL       -- set NULL when dept deleted
        ON UPDATE NO ACTION      -- reject if dept id changes
);
-- ON DELETE options: CASCADE, SET NULL, SET DEFAULT, RESTRICT, NO ACTION
-- RESTRICT = error immediately; NO ACTION = error at end of transaction

-- ─── DEFERRABLE constraints ────────────────────────────────────────────────
-- Check constraint at end of transaction instead of immediately
ALTER TABLE order_items ADD CONSTRAINT fk_order
    FOREIGN KEY (order_id) REFERENCES orders(id)
    DEFERRABLE INITIALLY DEFERRED;   -- useful for circular references

-- ─── View all constraints ─────────────────────────────────────────────────
SELECT constraint_name, constraint_type, table_name
FROM information_schema.table_constraints
WHERE table_schema = 'public'
ORDER BY table_name, constraint_type;
```

---

## 24. PostgreSQL Specific Features

### ENUM Types

```sql
-- Create a custom type
CREATE TYPE order_status AS ENUM ('pending', 'processing', 'shipped', 'delivered', 'cancelled');

CREATE TABLE orders (
    id     SERIAL PRIMARY KEY,
    status order_status NOT NULL DEFAULT 'pending'
);

INSERT INTO orders (status) VALUES ('processing');  -- ✅
-- INSERT INTO orders (status) VALUES ('unknown');  ← ERROR: invalid input value

-- Add a new value to ENUM (must go to end unless using BEFORE/AFTER)
ALTER TYPE order_status ADD VALUE 'returned' AFTER 'delivered';

-- List ENUM values
SELECT unnest(enum_range(NULL::order_status));
```

### Arrays

```sql
-- Create table with array column
CREATE TABLE products (
    id   SERIAL PRIMARY KEY,
    name TEXT,
    tags TEXT[],                   -- array of text
    scores INTEGER[]               -- array of integers
);

INSERT INTO products (name, tags, scores)
VALUES ('Laptop', ARRAY['electronics', 'computer', 'portable'], ARRAY[95, 87, 92]);

-- Query arrays
SELECT * FROM products WHERE 'electronics' = ANY(tags);    -- tag exists in array
SELECT * FROM products WHERE tags @> ARRAY['electronics']; -- array contains all
SELECT * FROM products WHERE tags && ARRAY['laptop', 'phone'];  -- arrays overlap

-- Access array elements (1-indexed in PostgreSQL!)
SELECT tags[1] FROM products;              -- first element
SELECT tags[1:2] FROM products;            -- slice: first two elements
SELECT array_length(tags, 1) FROM products; -- number of elements
SELECT unnest(tags) FROM products;          -- expand array to rows
SELECT array_to_string(tags, ', ') FROM products;  -- join to string
```

### Range Types

```sql
-- Built-in range types
int4range    -- range of integers: '[1,10]'
int8range    -- range of bigints
numrange     -- range of numerics: '[1.5, 10.5)'
tsrange      -- range of timestamps (no tz)
tstzrange    -- range of timestamps (with tz)
daterange    -- range of dates

CREATE TABLE reservations (
    id         SERIAL PRIMARY KEY,
    room_id    INTEGER,
    stay_dates daterange NOT NULL,
    CONSTRAINT no_overlap EXCLUDE USING gist (room_id WITH =, stay_dates WITH &&)
    -- prevent overlapping reservations for same room!
);

INSERT INTO reservations (room_id, stay_dates)
VALUES (101, '[2024-01-15, 2024-01-20)');  -- inclusive start, exclusive end

-- Check if date falls within a range
SELECT * FROM reservations
WHERE stay_dates @> '2024-01-17'::date;   -- contains specific date

-- Check for overlapping ranges
SELECT * FROM reservations
WHERE stay_dates && '[2024-01-18, 2024-01-22)'::daterange;
```

### Generated Columns

```sql
CREATE TABLE employees (
    id          SERIAL PRIMARY KEY,
    first_name  TEXT NOT NULL,
    last_name   TEXT NOT NULL,
    full_name   TEXT GENERATED ALWAYS AS (first_name || ' ' || last_name) STORED
    -- full_name is auto-computed and stored — always up-to-date
);
```

### Table Inheritance

```sql
-- Parent table
CREATE TABLE vehicles (id SERIAL, brand TEXT, year INTEGER);

-- Child tables inherit parent's columns
CREATE TABLE cars   (num_doors INTEGER) INHERITS (vehicles);
CREATE TABLE trucks (payload_tons NUMERIC) INHERITS (vehicles);

INSERT INTO cars VALUES (DEFAULT, 'Toyota', 2024, 4);
SELECT * FROM vehicles;  -- shows ALL rows from cars + trucks + vehicles
SELECT * FROM ONLY vehicles;  -- only vehicles table rows
```

---

## 25. PostgreSQL JSON & JSONB

```sql
-- JSONB is binary JSON — indexed, faster, preferred over JSON

CREATE TABLE products (
    id       SERIAL PRIMARY KEY,
    name     TEXT,
    metadata JSONB
);

INSERT INTO products (name, metadata) VALUES
('Laptop', '{"brand": "Dell", "specs": {"ram": 16, "storage": 512}, "tags": ["electronics"]}'),
('Phone',  '{"brand": "Apple", "specs": {"ram": 8, "storage": 256}, "tags": ["electronics", "mobile"]}');


-- ─── Accessing JSONB values ────────────────────────────────────────────────
SELECT metadata -> 'brand' FROM products;              -- returns JSON: "Dell"
SELECT metadata ->> 'brand' FROM products;             -- returns TEXT: Dell (no quotes)
SELECT metadata -> 'specs' -> 'ram' FROM products;     -- nested: 16
SELECT metadata #>> '{specs,ram}' FROM products;       -- path access: "16"


-- ─── Filtering on JSONB ────────────────────────────────────────────────────
SELECT * FROM products WHERE metadata ->> 'brand' = 'Dell';
SELECT * FROM products WHERE (metadata -> 'specs' ->> 'ram')::INTEGER > 8;
SELECT * FROM products WHERE metadata @> '{"brand": "Apple"}';        -- contains
SELECT * FROM products WHERE metadata ? 'brand';                       -- key exists
SELECT * FROM products WHERE metadata ?| ARRAY['brand', 'color'];     -- any key exists
SELECT * FROM products WHERE metadata ?& ARRAY['brand', 'specs'];     -- all keys exist
SELECT * FROM products WHERE metadata -> 'tags' @> '["mobile"]';      -- array contains


-- ─── Updating JSONB ────────────────────────────────────────────────────────
-- Add/update a key
UPDATE products
SET metadata = metadata || '{"color": "black"}'
WHERE id = 1;

-- Remove a key
UPDATE products
SET metadata = metadata - 'color'
WHERE id = 1;

-- Set a nested value
UPDATE products
SET metadata = jsonb_set(metadata, '{specs,ram}', '32')
WHERE id = 1;

-- Remove a nested key
UPDATE products
SET metadata = metadata #- '{specs,storage}'
WHERE id = 1;


-- ─── JSONB functions ────────────────────────────────────────────────────────
SELECT jsonb_each(metadata) FROM products LIMIT 1;         -- expand to rows
SELECT jsonb_object_keys(metadata) FROM products;           -- list all keys
SELECT jsonb_array_elements(metadata -> 'tags') FROM products;  -- expand array
SELECT jsonb_pretty(metadata) FROM products LIMIT 1;       -- formatted output
SELECT jsonb_strip_nulls(metadata) FROM products;          -- remove null values
SELECT jsonb_typeof(metadata -> 'specs') FROM products;    -- "object"
SELECT jsonb_typeof(metadata -> 'tags') FROM products;     -- "array"


-- ─── GIN Index for JSONB ───────────────────────────────────────────────────
CREATE INDEX idx_products_metadata ON products USING GIN (metadata);
-- Now @>, ?, ?| operators will use this index
```

---

## 26. Performance & Query Optimization

### EXPLAIN & EXPLAIN ANALYZE

```sql
-- EXPLAIN: shows the query plan (no actual execution)
EXPLAIN SELECT * FROM employees WHERE dept_id = 3;

-- EXPLAIN ANALYZE: executes the query and shows actual timing
EXPLAIN ANALYZE SELECT * FROM employees WHERE dept_id = 3;

-- EXPLAIN with all details
EXPLAIN (ANALYZE, BUFFERS, VERBOSE, FORMAT JSON)
SELECT * FROM employees WHERE dept_id = 3;

-- Key terms in EXPLAIN output:
-- Seq Scan     → full table scan (no index used) — slow for large tables
-- Index Scan   → used an index — fast
-- Index Only Scan → all data from index itself (fastest)
-- Bitmap Scan  → multiple index lookups combined
-- Hash Join    → join using hash table
-- Nested Loop  → for each row in outer, scan inner
-- cost=0.00..8.29  → estimated startup cost .. total cost
-- rows=182         → estimated number of rows
-- width=128        → estimated bytes per row
-- actual time=0.050..1.234  → actual execution time (ms)
-- loops=1          → how many times this node was executed
```

### Common Optimization Techniques

```sql
-- ─── 1. Use indexes on WHERE, JOIN, ORDER BY columns ──────────────────────
CREATE INDEX idx_employees_dept_active ON employees(dept_id) WHERE is_active = TRUE;


-- ─── 2. Avoid SELECT * — fetch only needed columns ────────────────────────
-- ❌ Slow (fetches ALL columns including large TEXT/JSONB)
SELECT * FROM products WHERE category = 'Electronics';

-- ✅ Fast (only fetch what you need)
SELECT id, name, price FROM products WHERE category = 'Electronics';


-- ─── 3. Use EXISTS instead of COUNT for existence checks ──────────────────
-- ❌ Slow (counts all matching rows)
IF (SELECT COUNT(*) FROM orders WHERE user_id = 1) > 0 THEN ...

-- ✅ Fast (stops at first match)
IF EXISTS(SELECT 1 FROM orders WHERE user_id = 1) THEN ...


-- ─── 4. Batch inserts over individual inserts ─────────────────────────────
-- ❌ Slow (1000 separate transactions)
INSERT INTO logs VALUES (1, 'msg1');
INSERT INTO logs VALUES (2, 'msg2');
-- ... 998 more

-- ✅ Fast (one transaction)
INSERT INTO logs VALUES (1, 'msg1'), (2, 'msg2'), ... (1000, 'msg1000');


-- ─── 5. Use LIMIT for large result sets ───────────────────────────────────
SELECT * FROM events ORDER BY created_at DESC LIMIT 100;


-- ─── 6. Avoid functions on indexed columns in WHERE ───────────────────────
-- ❌ Can't use index on email column (function wraps it)
SELECT * FROM users WHERE LOWER(email) = 'alice@example.com';

-- ✅ Create expression index + query the same way
CREATE INDEX idx_users_email_lower ON users(LOWER(email));
SELECT * FROM users WHERE LOWER(email) = 'alice@example.com';  -- now uses index


-- ─── 7. Use connection pooling (PgBouncer) ────────────────────────────────
-- Each PostgreSQL connection is a separate OS process (~5-10MB)
-- PgBouncer maintains a pool of connections, reuses them for clients


-- ─── 8. VACUUM and ANALYZE ────────────────────────────────────────────────
-- PostgreSQL uses MVCC — dead rows accumulate after UPDATE/DELETE
VACUUM employees;               -- remove dead rows (non-blocking)
VACUUM FULL employees;          -- compact table (locks table!)
VACUUM ANALYZE employees;       -- vacuum + update statistics
ANALYZE employees;              -- update query planner statistics only
ANALYZE;                        -- analyze all tables

-- Autovacuum handles this automatically but you can run manually after bulk changes


-- ─── 9. Table partitioning ────────────────────────────────────────────────
-- Split large tables into smaller partitions for better performance

-- Range partitioning (e.g., partition logs by month)
CREATE TABLE system_logs (
    id         BIGSERIAL,
    message    TEXT,
    created_at TIMESTAMPTZ NOT NULL
) PARTITION BY RANGE (created_at);

CREATE TABLE system_logs_2024_01 PARTITION OF system_logs
    FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
CREATE TABLE system_logs_2024_02 PARTITION OF system_logs
    FOR VALUES FROM ('2024-02-01') TO ('2024-03-01');

-- Queries on created_at automatically go to the right partition
SELECT * FROM system_logs WHERE created_at >= '2024-01-01' AND created_at < '2024-02-01';
-- PostgreSQL only scans the January partition — much faster!


-- ─── 10. pg_stat_statements — find slow queries ───────────────────────────
-- Enable: add 'pg_stat_statements' to shared_preload_libraries in postgresql.conf
SELECT query, calls, mean_exec_time, total_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;  -- top 10 slowest queries
```

---

## 27. PostgreSQL Configuration & Admin

### Essential postgresql.conf Settings

```ini
# ─── Memory ──────────────────────────────────────────────────────────────
shared_buffers = 256MB          # PostgreSQL cache (25% of RAM is a good start)
work_mem = 64MB                 # memory per sort/hash operation per query
maintenance_work_mem = 512MB    # for VACUUM, CREATE INDEX operations
effective_cache_size = 1GB      # total memory available for caching (75% of RAM)

# ─── WAL (Write-Ahead Log) ────────────────────────────────────────────────
wal_level = replica             # enable replication
max_wal_size = 1GB
checkpoint_completion_target = 0.9

# ─── Connections ──────────────────────────────────────────────────────────
max_connections = 100           # max simultaneous connections
# Use PgBouncer for connection pooling in production

# ─── Query Planner ────────────────────────────────────────────────────────
random_page_cost = 1.1          # 1.1 for SSD, 4.0 for HDD (default)
effective_io_concurrency = 200  # 200 for SSD, 2 for HDD

# ─── Logging ──────────────────────────────────────────────────────────────
log_min_duration_statement = 1000  # log queries taking > 1 second
log_statement = 'ddl'              # log DDL statements
log_connections = on
log_disconnections = on
```

### Useful Admin Queries

```sql
-- ─── Database information ─────────────────────────────────────────────────
SELECT datname, pg_size_pretty(pg_database_size(datname)) AS size
FROM pg_database ORDER BY pg_database_size(datname) DESC;


-- ─── Table sizes ──────────────────────────────────────────────────────────
SELECT
    table_name,
    pg_size_pretty(pg_total_relation_size(quote_ident(table_name))) AS total_size,
    pg_size_pretty(pg_relation_size(quote_ident(table_name)))       AS table_size,
    pg_size_pretty(pg_indexes_size(quote_ident(table_name)))        AS indexes_size
FROM information_schema.tables
WHERE table_schema = 'public'
ORDER BY pg_total_relation_size(quote_ident(table_name)) DESC;


-- ─── Active connections ────────────────────────────────────────────────────
SELECT pid, usename, application_name, client_addr, state, query, query_start
FROM pg_stat_activity
WHERE state != 'idle'
ORDER BY query_start;


-- ─── Kill a query ─────────────────────────────────────────────────────────
SELECT pg_cancel_backend(pid);    -- send SIGINT (graceful cancel)
SELECT pg_terminate_backend(pid); -- send SIGTERM (hard kill)

-- Kill all idle connections
SELECT pg_terminate_backend(pid) FROM pg_stat_activity
WHERE state = 'idle' AND query_start < NOW() - INTERVAL '10 minutes';


-- ─── Index usage statistics ────────────────────────────────────────────────
-- Find unused indexes (waste of space + slow writes)
SELECT schemaname, tablename, indexname, idx_scan
FROM pg_stat_user_indexes
WHERE idx_scan = 0
ORDER BY schemaname, tablename;

-- Find most used indexes
SELECT schemaname, tablename, indexname, idx_scan, idx_tup_read, idx_tup_fetch
FROM pg_stat_user_indexes
ORDER BY idx_scan DESC;


-- ─── Cache hit ratio (should be > 95%) ────────────────────────────────────
SELECT
    sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) AS cache_hit_ratio
FROM pg_statio_user_tables;


-- ─── Table bloat — dead rows ───────────────────────────────────────────────
SELECT relname, n_live_tup, n_dead_tup,
    ROUND(n_dead_tup::NUMERIC / NULLIF(n_live_tup + n_dead_tup, 0) * 100, 1) AS dead_pct
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC;


-- ─── Replication status ────────────────────────────────────────────────────
-- On primary:
SELECT * FROM pg_stat_replication;

-- On replica:
SELECT * FROM pg_stat_wal_receiver;
```

---

## 28. Complete SQL Commands Reference

```sql
-- ─── DATABASE ─────────────────────────────────────────────────────────────
CREATE DATABASE myapp;
CREATE DATABASE myapp OWNER myuser ENCODING 'UTF8';
DROP DATABASE myapp;
ALTER DATABASE myapp RENAME TO myapp_v2;
ALTER DATABASE myapp OWNER TO newowner;

-- ─── USER & PERMISSIONS ───────────────────────────────────────────────────
CREATE USER myuser WITH PASSWORD 'secret';
CREATE ROLE myuser WITH LOGIN PASSWORD 'secret';
ALTER USER myuser WITH PASSWORD 'newsecret';
ALTER USER myuser SUPERUSER;            -- make superuser
ALTER USER myuser CREATEDB;             -- allow creating DBs
DROP USER myuser;

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE myapp TO myuser;
GRANT SELECT, INSERT, UPDATE ON employees TO myuser;
GRANT USAGE, SELECT ON SEQUENCE employees_id_seq TO myuser;
GRANT ALL ON ALL TABLES IN SCHEMA public TO myuser;
REVOKE SELECT ON employees FROM myuser;

-- ─── SCHEMA ───────────────────────────────────────────────────────────────
CREATE SCHEMA analytics;
CREATE SCHEMA IF NOT EXISTS analytics;
DROP SCHEMA analytics;
DROP SCHEMA analytics CASCADE;         -- drops all objects inside too
SET search_path TO analytics, public;  -- set schema search path

-- ─── TABLE ────────────────────────────────────────────────────────────────
CREATE TABLE t (id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE IF NOT EXISTS t (...);
CREATE TEMP TABLE temp_t (...);        -- temporary table (session-scoped)
CREATE UNLOGGED TABLE fast_t (...);    -- not crash-safe, but faster writes
ALTER TABLE t ADD COLUMN col TEXT;
ALTER TABLE t DROP COLUMN col;
ALTER TABLE t RENAME COLUMN old TO new;
ALTER TABLE t ALTER COLUMN col TYPE BIGINT;
ALTER TABLE t ALTER COLUMN col SET DEFAULT 0;
ALTER TABLE t ALTER COLUMN col SET NOT NULL;
DROP TABLE t;
DROP TABLE IF EXISTS t;
TRUNCATE TABLE t;
TRUNCATE TABLE t RESTART IDENTITY CASCADE;

-- ─── INDEX ────────────────────────────────────────────────────────────────
CREATE INDEX idx_name ON t(col);
CREATE UNIQUE INDEX idx_name ON t(col);
CREATE INDEX idx_name ON t(col1, col2);
CREATE INDEX CONCURRENTLY idx_name ON t(col);
CREATE INDEX idx_name ON t USING GIN(jsonb_col);
CREATE INDEX idx_name ON t USING BRIN(created_at);
CREATE INDEX idx_name ON t(col) WHERE is_active = TRUE;
DROP INDEX idx_name;
DROP INDEX CONCURRENTLY idx_name;
REINDEX TABLE t;
REINDEX DATABASE myapp;

-- ─── SEQUENCE ─────────────────────────────────────────────────────────────
CREATE SEQUENCE myseq START 1 INCREMENT 1;
SELECT nextval('myseq');        -- get next value
SELECT currval('myseq');        -- get current value
SELECT setval('myseq', 1000);  -- reset to 1000
ALTER SEQUENCE myseq RESTART WITH 1;
DROP SEQUENCE myseq;

-- ─── COPY (bulk import/export) ────────────────────────────────────────────
-- Import CSV into table
COPY employees(first_name, last_name, email, salary)
FROM '/tmp/employees.csv'
WITH (FORMAT CSV, HEADER TRUE, DELIMITER ',', NULL '');

-- Export table to CSV
COPY employees TO '/tmp/employees_export.csv'
WITH (FORMAT CSV, HEADER TRUE);

-- COPY from stdin (useful with psql or scripts)
\COPY employees FROM 'employees.csv' WITH (FORMAT CSV, HEADER TRUE);
```

---

## 29. Complete psql CLI Reference

```bash
# ─── Connect to PostgreSQL ─────────────────────────────────────────────────
psql -U postgres                          # connect as postgres user
psql -U myuser -d myapp                   # connect as myuser to myapp database
psql -h localhost -p 5432 -U myuser myapp # explicit host and port
psql "postgresql://myuser:secret@localhost:5432/myapp"  # connection string
PGPASSWORD=secret psql -U myuser myapp    # password via environment variable


# ─── psql Meta-Commands ────────────────────────────────────────────────────
\l                    # list all databases
\l+                   # list databases with sizes
\c myapp              # connect to database 'myapp'
\c myapp myuser       # connect as a different user

\dt                   # list all tables in current schema
\dt *.*               # list all tables in all schemas
\dt+ employees        # detailed info about 'employees' table

\d  employees         # describe table (columns, indexes, constraints)
\d+ employees         # describe table with extra detail (storage, comments)

\di                   # list all indexes
\di employees*        # list indexes for tables starting with 'employees'

\dv                   # list all views
\dm                   # list all materialized views
\df                   # list all functions
\ds                   # list all sequences
\du                   # list all users/roles
\dn                   # list all schemas

\e                    # open current query in text editor
\i /path/to/file.sql  # execute SQL from a file
\o /tmp/output.txt    # send query results to a file
\o                    # stop sending to file (back to screen)
\copy ...             # client-side copy (vs server-side COPY)

\timing on            # show execution time for queries
\timing off

\x                    # toggle expanded display (one line per column — great for wide tables)
\x auto               # auto-switch between normal and expanded

\pset pager off       # disable pager (shows all results without "more")
\pset null 'NULL'     # show NULL values as the text "NULL"
\pset border 2        # display style (0=no border, 1=inner only, 2=full)

\set HISTSIZE 1000    # keep 1000 queries in history
\set AUTOCOMMIT off   # disable autocommit (require explicit COMMIT)

\! ls                 # run a shell command
\! clear              # clear the terminal
\q                    # quit psql
\?                    # help on psql meta-commands
\h SELECT             # help on SQL command syntax


# ─── Run SQL from command line (non-interactive) ────────────────────────────
psql -U myuser -d myapp -c "SELECT COUNT(*) FROM employees;"
psql -U myuser -d myapp -f /path/to/script.sql
psql -U myuser -d myapp -c "\COPY employees FROM 'data.csv' WITH CSV HEADER"


# ─── pg_dump — backup ─────────────────────────────────────────────────────
pg_dump -U myuser myapp > backup.sql                    # plain SQL format
pg_dump -U myuser -Fc myapp > backup.dump               # custom format (compressed)
pg_dump -U myuser -Fc myapp -f backup.dump              # to file
pg_dump -U myuser -t employees myapp > employees.sql    # single table
pg_dump -U myuser --schema-only myapp > schema.sql      # schema only
pg_dump -U myuser --data-only myapp > data.sql          # data only
pg_dumpall -U postgres > all_databases.sql              # all databases


# ─── pg_restore — restore ─────────────────────────────────────────────────
pg_restore -U myuser -d myapp backup.dump               # restore from custom format
pg_restore -U myuser -d myapp --clean backup.dump       # drop before restore
pg_restore -U myuser -d myapp -t employees backup.dump  # restore single table
psql -U myuser myapp < backup.sql                       # restore SQL format


# ─── pg_stat_activity — monitor from CLI ──────────────────────────────────
psql -U postgres -c "SELECT pid, state, query FROM pg_stat_activity;"


# ─── createdb / dropdb shortcuts ──────────────────────────────────────────
createdb -U postgres myapp
dropdb   -U postgres myapp

# ─── createuser / dropuser shortcuts ─────────────────────────────────────
createuser -U postgres --interactive myuser
dropuser   -U postgres myuser


# ─── PostgreSQL service management ────────────────────────────────────────
# Ubuntu/Debian
sudo systemctl start   postgresql
sudo systemctl stop    postgresql
sudo systemctl restart postgresql
sudo systemctl status  postgresql
sudo systemctl enable  postgresql   # start on boot

# macOS (Homebrew)
brew services start  postgresql@16
brew services stop   postgresql@16
brew services restart postgresql@16

# Config file locations (Ubuntu)
# postgresql.conf: /etc/postgresql/16/main/postgresql.conf
# pg_hba.conf:     /etc/postgresql/16/main/pg_hba.conf
# data dir:        /var/lib/postgresql/16/main
```

---

## 30. Common Interview Questions

### Q1: What are ACID properties and why are they important?
**Answer:** ACID stands for Atomicity, Consistency, Isolation, and Durability. Atomicity means all operations in a transaction either complete successfully or none do — a bank transfer either debits AND credits, or neither happens. Consistency means a transaction always leaves the database in a valid state — you can't transfer more money than exists. Isolation means concurrent transactions don't interfere — two users booking the same seat run as if they were sequential. Durability means once a transaction commits, the changes survive crashes — PostgreSQL achieves this with Write-Ahead Logging (WAL). These properties are essential for financial systems, e-commerce, and any application where data correctness is critical.

---

### Q2: What is the difference between a WHERE clause and a HAVING clause?
**Answer:** Both filter rows, but at different stages. `WHERE` filters individual rows **before** any grouping or aggregation happens — it works on the raw table data and cannot reference aggregate functions like COUNT or AVG. `HAVING` filters **groups** after `GROUP BY` has been applied — it works on the result of aggregation and can use aggregate functions. Example: `WHERE salary > 50000` filters out low-paid employees before grouping. `HAVING COUNT(*) > 5` keeps only departments that have more than 5 employees after grouping. A query can use both — WHERE first reduces the rows, then GROUP BY groups them, then HAVING filters the groups.

---

### Q3: What is the difference between INNER JOIN, LEFT JOIN, and FULL OUTER JOIN?
**Answer:** INNER JOIN returns only rows where there is a match in both tables — non-matching rows from either side are excluded. LEFT JOIN returns all rows from the left table plus matching rows from the right — if there's no match on the right, the right-side columns are NULL. RIGHT JOIN is the mirror image. FULL OUTER JOIN returns all rows from both tables — unmatched rows from either side get NULLs for the missing side's columns. A practical example: LEFT JOIN employees to departments gives you all employees including those with no department assigned. INNER JOIN gives only employees who belong to an existing department.

---

### Q4: What are indexes, and when should you not use them?
**Answer:** An index is a separate data structure (usually a B-tree) that allows the database to find rows without scanning the entire table — similar to a book's index. They dramatically speed up SELECT queries. However, you should NOT index every column because: every index consumes disk space, every INSERT/UPDATE/DELETE must also update all indexes on that table (write overhead increases), and the query planner might choose the wrong index. Avoid indexing: columns with very few unique values (like boolean columns), small tables where a full scan is faster, columns never used in WHERE/JOIN/ORDER BY, and columns that are updated very frequently.

---

### Q5: What is normalization and when would you denormalize?
**Answer:** Normalization organizes a database to eliminate redundancy and ensure data integrity through a series of normal forms. 1NF eliminates repeating groups (atomic values). 2NF eliminates partial dependencies on composite keys. 3NF eliminates transitive dependencies (non-key depending on non-key). For example, storing dept_name in the employees table instead of just dept_id is a 3NF violation — if the department is renamed, you'd update thousands of employee rows. You deliberately denormalize when read performance is critical: data warehouses, reporting tables, and heavily-read data that changes rarely. The tradeoff is redundancy and update anomalies in exchange for faster reads and simpler queries.

---

### Q6: What is the difference between TRUNCATE and DELETE?
**Answer:** Both remove rows but very differently. `DELETE` removes rows one by one, logs each deletion in the transaction log, respects WHERE conditions to delete specific rows, can be rolled back, and fires row-level triggers. `TRUNCATE` removes ALL rows at once as a DDL operation, is dramatically faster for large tables, cannot have a WHERE clause, resets identity sequences with `RESTART IDENTITY`, and in PostgreSQL can be rolled back (unlike MySQL). Use TRUNCATE to quickly empty a large table; use DELETE when you need to remove specific rows or need the operation to be trigger-aware.

---

### Q7: What are window functions and how do they differ from GROUP BY?
**Answer:** Both perform calculations over sets of rows, but GROUP BY collapses rows into groups — losing individual row data — while window functions add a calculated column to each row without removing any rows. For example, `GROUP BY dept_id` with `AVG(salary)` gives one row per department. A window function `AVG(salary) OVER (PARTITION BY dept_id)` gives every single employee row PLUS their department's average salary as an additional column. Window functions are used for running totals, rankings within groups (top earner per department), comparing each row to its group average, and calculating metrics like lag/lead values.

---

### Q8: What is the CAP theorem?
**Answer:** The CAP theorem states that a distributed database can guarantee at most two of three properties: Consistency (all nodes see the same data simultaneously), Availability (every request gets a response even if nodes fail), and Partition Tolerance (system keeps working despite network failures). Since network partitions inevitably happen in distributed systems, the real choice is CP (consistent but might reject requests during partition) or AP (always responds but might return stale data). Banking systems choose CP — you never want stale balance data. Social media feeds choose AP — showing a slightly old feed is fine. Single-server PostgreSQL provides CA but isn't distributed, making it more accurate to say it provides full ACID rather than making CAP tradeoffs.

---

### Q9: What are the database isolation levels and what problems do they solve?
**Answer:** The four standard isolation levels address three concurrency problems. **Dirty Read**: reading uncommitted changes from another transaction. **Non-Repeatable Read**: the same SELECT returns different data within one transaction (another committed between reads). **Phantom Read**: a repeated query returns new rows (another inserted them). Read Uncommitted allows all three problems (fastest). Read Committed (PostgreSQL default) prevents dirty reads. Repeatable Read prevents dirty and non-repeatable reads. Serializable prevents all three, making transactions behave as if they ran sequentially (safest but slowest). PostgreSQL's MVCC architecture means its Repeatable Read level also prevents phantom reads in practice.

---

### Q10: What is the difference between a clustered and non-clustered index?
**Answer:** A **clustered index** determines the physical order of rows on disk — data pages are sorted by the index key. There can be only one clustered index per table since data can only be physically sorted one way. In SQL Server, the primary key is typically clustered. PostgreSQL doesn't have traditional clustered indexes but has `CLUSTER table USING index` which physically reorders the table once — it doesn't stay ordered after new inserts. A **non-clustered index** is a separate structure that points to the actual rows — it contains the index key and a row pointer (heap tuple ID). A table can have many non-clustered indexes. Non-clustered indexes on high-cardinality columns used in frequent WHERE clauses are the most common type of index optimization.

---

## 🔄 SQL & PostgreSQL — At a Glance

```
SQL Sub-languages:
DDL → CREATE, ALTER, DROP, TRUNCATE    (structure)
DML → INSERT, UPDATE, DELETE           (modify data)
DQL → SELECT                           (query data)
DCL → GRANT, REVOKE                    (permissions)
TCL → BEGIN, COMMIT, ROLLBACK          (transactions)

Query Execution Order:
FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT

Index Types:
B-Tree (default) → equality + range queries
Hash → equality only
GIN → arrays, JSONB, full-text search
GiST → geometric, range types
BRIN → large ordered tables (timestamps)
Partial → subset of table
Expression → function results

ACID: Atomicity + Consistency + Isolation + Durability
CAP:  Choose 2 of Consistency + Availability + Partition Tolerance
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| SQL | Standard language for relational databases — DDL, DML, DQL, DCL, TCL |
| PostgreSQL | Advanced open-source relational DB — ACID, JSON, arrays, extensions |
| INNER JOIN | Only rows matching in BOTH tables |
| LEFT JOIN | All left rows + matching right (NULL if no match) |
| FULL OUTER JOIN | All rows from both tables |
| WHERE | Filter rows BEFORE grouping |
| HAVING | Filter groups AFTER aggregation |
| GROUP BY | Aggregate rows into groups |
| Window function | Aggregate ACROSS rows without collapsing them |
| PARTITION BY | Divide rows into groups for window function |
| CTE | Named temporary result with `WITH` clause |
| Recursive CTE | CTE that references itself — for hierarchical data |
| Subquery | Query nested inside another query |
| B-Tree index | Default — supports equality, range, ORDER BY, LIKE 'prefix%' |
| GIN index | For arrays, JSONB, full-text search |
| BRIN index | For large ordered tables — very small footprint |
| Partial index | Index on a WHERE condition — smaller and faster |
| Expression index | Index on LOWER(col) or other function results |
| EXPLAIN ANALYZE | Show actual query execution plan and timing |
| Seq Scan | Full table scan — no index used |
| Index Scan | Used an index — faster |
| ACID | Atomicity, Consistency, Isolation, Durability |
| CAP | Consistency, Availability, Partition Tolerance — pick 2 |
| 1NF | Atomic values, no repeating groups |
| 2NF | 1NF + no partial dependencies |
| 3NF | 2NF + no transitive dependencies |
| Isolation levels | Read Uncommitted → Read Committed → Repeatable Read → Serializable |
| Dirty read | Reading uncommitted data from another transaction |
| Phantom read | New rows appear in repeated query within same transaction |
| Trigger | Auto-execute function on INSERT/UPDATE/DELETE |
| View | Named query — virtual table |
| Materialized view | Stored query result — must refresh manually |
| JSONB | Binary JSON — indexed, faster than JSON |
| SERIAL | Auto-increment integer — creates a sequence |
| NULLIF | Returns NULL if two values are equal |
| COALESCE | Returns first non-NULL value |
| FOR UPDATE | Row-level lock — blocks other transactions |
| SKIP LOCKED | Skip locked rows — perfect for job queues |
| VACUUM | Remove dead rows after UPDATE/DELETE (MVCC cleanup) |
| WAL | Write-Ahead Log — guarantees durability |
| pg_dump | Backup a PostgreSQL database |
| psql | PostgreSQL interactive command-line client |
| COPY | Bulk import/export — fastest way to load large data |
| RETURNING | Return rows affected by INSERT/UPDATE/DELETE |
| ON CONFLICT | UPSERT — insert or update if key conflict |

---

> 💡 **The Production Golden Rule:**
> Always combine — **EXPLAIN ANALYZE every slow query + Index WHERE and JOIN columns + Use connection pooling (PgBouncer) + Regular VACUUM ANALYZE + TIMESTAMPTZ over TIMESTAMP + NUMERIC over FLOAT for money + Transactions for multi-step writes + Partial indexes for filtered queries + pg_stat_statements to find slow queries**.
> These habits keep a PostgreSQL database fast, reliable, and maintainable at scale.

---

*Notes prepared based on: PostgreSQL Official Documentation, Use The Index Luke (markuswinand.com), High Performance PostgreSQL, W3Schools, GeeksforGeeks, and production best practices (2025–2026)*
