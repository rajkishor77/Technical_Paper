# Technical Paper: Core Database Concepts for Data Integrity and Performance

## Abstract  

This paper explains the most important database concepts used to maintain data reliability, performance, and consistency. Topics covered include **ACID properties, CAP theorem, joins, aggregations, normalization, indexes, transactions, locking mechanisms, isolation levels, and triggers**. These are foundational concepts for anyone working with relational databases (RDBMS) or distributed databases.  

---

## 1. ACID Properties  

ACID ensures that database transactions are **reliable and consistent**.  

- **Atomicity**: All operations in a transaction either complete or none do.  
- **Consistency**: A transaction always brings the database from one valid state to another.  
- **Isolation**: Transactions do not interfere with each other.  
- **Durability**: Once committed, changes are permanent even after a crash.  

**Example (SQL Transaction):**  

```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 500 WHERE id = 1;
UPDATE accounts SET balance = balance + 500 WHERE id = 2;
COMMIT;
```
## 2. CAP Theorem
The CAP theorem states that a distributed system can only provide two out of three:

**Consistency (C)** – All users see the same data.

**Availability (A)** – System always responds, even if some nodes fail.

**Partition Tolerance (P)** – Works despite network issues.

**Trade-offs:**

CP systems focus on consistency.

AP systems focus on availability.

CA systems are nearly impossible in distributed environments.

## 3. Joins
Joins are used to combine rows from multiple tables.

**Types of Joins**

**INNER JOIN:** Returns only matching rows.

**LEFT JOIN:** Returns all rows from the left table, even if no match exists.

**RIGHT JOIN:** Returns all rows from the right table.

**FULL OUTER JOIN:** Returns all rows, with NULL where no match exists.

**CROSS JOIN:** Returns the Cartesian product of two tables.

**SELF JOIN:** Joins a table with itself.

**Example (INNER JOIN):**

```sql
SELECT e.name, d.department_name
FROM employees e
INNER JOIN departments d
ON e.dept_id = d.id;
```
**Example (LEFT JOIN):**

```sql
SELECT e.name, d.department_name
FROM employees e
LEFT JOIN departments d
ON e.dept_id = d.id;
```
**Example (FULL OUTER JOIN):**

```sql
SELECT e.name, d.department_name
FROM employees e
FULL OUTER JOIN departments d
ON e.dept_id = d.id;
```

## 4. Aggregations and Filters
Aggregations summarize data, while filters refine results.

**WHERE** – Filters rows before grouping.

**GROUP BY** – Groups rows for aggregation.

**HAVING** – Filters after aggregation.

**Example:**

```sql
SELECT dept_id, COUNT(*) AS total_employees
FROM employees
WHERE salary > 50000
GROUP BY dept_id
HAVING COUNT(*) > 5;
```

## 5. Normalization
Normalization reduces redundancy and improves data integrity.

**1NF:** No repeating groups; each cell is atomic.

**2NF:** Every non-key attribute depends on the whole key.

**3NF:** No transitive dependency (non-key → non-key).

**BCNF:** Stronger form of 3NF.

## 6. Indexes
Indexes improve data retrieval speed but may slow down writes.

**B-tree Index:** Best for range queries.

**Hash Index:** Best for equality lookups.

**Bitmap Index:** Useful for low-cardinality data.

**Example:**

```sql
CREATE INDEX idx_employee_name
ON employees(name);
```

## 7. Transactions
A transaction is a group of operations executed together.

**Example:**

```sql
BEGIN;
UPDATE stock SET quantity = quantity - 1 WHERE product_id = 101;
INSERT INTO sales(product_id, qty) VALUES (101, 1);
COMMIT;
```

If one step fails, the transaction can be rolled back.

## 8. Locking Mechanism
   
Locks prevent data corruption in concurrent transactions.

**Shared Lock:** Multiple reads allowed, no writes.

**Exclusive Lock:** Only one transaction can read/write.

**Deadlocks:** Occur when two transactions wait on each other.

## 9. Database Isolation Levels
Isolation levels prevent problems like dirty reads and phantom reads.

**Read Uncommitted** – Can see uncommitted changes (least safe).

**Read Committed** – Reads only committed data.

**Repeatable Read** – Ensures same row returns same values.

**Serializable** – Highest level, transactions behave sequentially.

## 10. Triggers
Triggers automatically run on specific events (INSERT, UPDATE, DELETE).

**Example:**

```sql
CREATE TRIGGER update_timestamp
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  UPDATE employees SET last_modified = NOW() WHERE id = NEW.id;
END;
```

## Conclusion
- Understanding these concepts helps in building efficient, consistent, and reliable database systems.
- ACID ensures reliability.
- CAP theorem guides distributed databases.
- Joins, normalization, indexes, and triggers improve performance and automation.



## References
---
https://www.geeksforgeeks.org/normal-forms-in-dbms/  
https://www.w3schools.com/sql/sql_join.asp  
https://www.w3schools.com/sql/sql_aggregate_functions.asp  
https://www.w3schools.com/sql/sql_having.asp  
https://www.postgresql.org/docs/current/indexes.html  
https://www.geeksforgeeks.org/indexing-in-databases-set-1/  
https://docs.oracle.com/cd/B28359_01/server.111/b28310/transact.htm  
