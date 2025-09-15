# Core Questions on Git, SQL, and Python Concepts

---

## 1. What is Staging Area  
In Git, the **staging area** is like a “middle place” between your working directory and the repository.  
- When you edit files, they stay in the working directory.  
- To include them in the next commit, you add them to the staging area using `git add`.  
- This allows you to prepare and review what changes will be saved.  
- It helps keep commits clean and organized, as you can commit selected changes instead of all edits.  

---

## 2. What is SQL  
SQL stands for **Structured Query Language**. It is the standard language used to interact with relational databases.  
- Helps in creating tables, inserting, updating, deleting, and reading data.  
- Works with structured data in rows and columns (like Excel).  
- Common databases: MySQL, PostgreSQL, Oracle, SQL Server.  
- SQL is divided into categories like DDL, DML, DQL, etc.  

**Example:**  
```sql
SELECT name, salary FROM employees WHERE salary > 50000;
```
## 3. What is Abstraction
Abstraction means hiding extra details and showing only the important parts.

In Python, abstraction is done using abstract classes and methods (from abc module).

**Example:** While driving a car, you just use the steering and brakes without knowing the internal working of the engine.

In programming, it makes code simpler and easier to use.

## 4. Difference Between Grid and Flex
**Flexbox**
Works in one direction at a time (row OR column).

Best for aligning items like menus, toolbars, or small sections.

**Example:** Arranging buttons in a row.

**Grid**
Works in two directions (rows AND columns).

Best for building full-page layouts (header, sidebar, content, footer).

**Example:** Dividing a webpage into multiple blocks.

**Quick Comparison Table**

| Feature    | Flexbox (Flex)        | CSS Grid                |
| ---------- | --------------------- | ----------------------- |
| Direction  | One axis (row/column) | Two axes (row & column) |
| Use Case   | Align items in a line | Full page layout        |
| Simplicity | Easy, simple layouts  | More powerful, complex  |

## 5. Explain DML in SQL
DML (Data Manipulation Language) is used to handle data inside database tables.

**INSERT** → Add new records

**UPDATE** → Modify existing records

**DELETE** → Remove records

**Example:**

```sql
INSERT INTO employees (id, name, salary) VALUES (1, 'John', 50000);
UPDATE employees SET salary = 60000 WHERE id = 1;
DELETE FROM employees WHERE id = 1;
```
## 6. How to Merge Two Branches in Git
Merging combines changes from one branch into another.

Switch to the branch you want to update (for example, main):

```git
git checkout main
```
**Run merge command:**
```git
git merge feature_branch
```
This brings all changes from feature_branch into main.

If both branches changed the same lines, Git will ask to resolve conflicts.

## 7. How to Create a New Branch in Git
A branch is like a copy of your project for working on new features safely.

Command to create and switch to a new branch:
```git
git checkout -b branch_name
```
**Example:**
```git
git checkout -b feature_login
```
Now you can work on this new branch without affecting the main branch.

