# Django Technical Paper

## 1. **Settings File**

The `settings.py` file in a Django project is the **main configuration file**. It defines how your Django application behaves and connects all parts of your project.

###  Key Functions of `settings.py`:

* Manages **database connection** details.
* Lists **installed apps** in the project.
* Defines **middleware** (request/response handling layers).
* Configures **templates**, **static files**, and **media files**.
* Handles **security settings** (like `DEBUG`, `SECRET_KEY`, and `ALLOWED_HOSTS`).
* Controls **internationalization**, **language**, and **time zone**.

Each Django project comes with a default `settings.py` when created using:

```bash
django-admin startproject projectname
```

You can also create **different settings files** for:

* Development
* Testing
* Production (secure environment)

---

## **What is SECRET_KEY?**

The `SECRET_KEY` is a **unique random string** that Django uses for cryptographic operations. It’s automatically generated when a new project is created.

###  Purpose of SECRET_KEY:

* Used to **hash passwords** and other sensitive data.
* Generates **CSRF tokens** to prevent CSRF attacks.
* Secures **session data** and cookies.
* Protects the integrity of **signed data**.

### Important Note:

* Keep your `SECRET_KEY` **private and hidden** — never upload it to GitHub or share it publicly.
* In production, store it in an **environment variable** for security.

**Example:**

```python
SECRET_KEY = 'django-insecure-8r9@#@21dj!_random_long_key_here_!@#as9d'
```

---

## **Default Django Apps Inside settings.py**

The default Django apps are listed under `INSTALLED_APPS` in `settings.py`.

**Default apps include:**

```python
INSTALLED_APPS = [
  'django.contrib.admin',         # Admin interface
  'django.contrib.auth',          # Authentication and user management
  'django.contrib.contenttypes',  # Handles model permissions
  'django.contrib.sessions',      # Session framework
  'django.contrib.messages',      # Messaging system
  'django.contrib.staticfiles',   # Manages static files (CSS, JS, images)
]
```

### You Can Add More Apps:

You can add both **custom apps** and **third-party apps** like:

* `rest_framework` → for building APIs.
* `corsheaders` → for handling cross-origin requests.
* `django_extensions`, `crispy_forms`, etc.

These are added manually by the developer to extend project functionality.

---

## **What is Middleware?**

**Middleware** is a framework of hooks into Django’s request/response processing.
It’s a **layer between the user’s request and the view** that allows you to process data globally before or after the view runs.

###  Example Middleware List:

```python
MIDDLEWARE = [
  'django.middleware.security.SecurityMiddleware',
  'django.contrib.sessions.middleware.SessionMiddleware',
  'django.middleware.common.CommonMiddleware',
  'django.middleware.csrf.CsrfViewMiddleware',
  'django.contrib.auth.middleware.AuthenticationMiddleware',
  'django.contrib.messages.middleware.MessageMiddleware',
  'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
```

###  Common Middleware Types:

| Middleware                   | Purpose                                           |
| ---------------------------- | ------------------------------------------------- |
| **SecurityMiddleware**       | Adds security headers (HTTPS, HSTS).              |
| **SessionMiddleware**        | Manages user sessions.                            |
| **CommonMiddleware**         | Adds useful HTTP features like URL normalization. |
| **CsrfViewMiddleware**       | Protects against Cross-Site Request Forgery.      |
| **AuthenticationMiddleware** | Associates users with requests.                   |
| **MessageMiddleware**        | Manages temporary flash messages.                 |
| **XFrameOptionsMiddleware**  | Prevents clickjacking by controlling iframes.     |

---

## **Django Security**

Security is a major feature of Django. It includes built-in protection against common web vulnerabilities.

---

###  **1. CSRF (Cross-Site Request Forgery)**

**Definition:**
An attack that tricks a user into submitting a request unknowingly.

**Example:**
If you are logged in, a hacker can make you unknowingly send a request to delete data.

**Django Protection:**
Django automatically includes CSRF protection via:

```html
{% csrf_token %}
```

This ensures that only trusted forms from your website are accepted.

---

###  **2. XSS (Cross-Site Scripting)**

**Definition:**
Attackers inject malicious scripts (JavaScript) into web pages viewed by others.

**Example:**
Entering `<script>alert('hacked')</script>` in a form.

**Django Protection:**
Django automatically **escapes special characters** in templates, making sure scripts are not executed.
You can also manually mark safe HTML using `|safe` carefully.

---

###  **3. Clickjacking**

**Definition:**
Tricking users into clicking hidden elements or buttons inside an invisible iframe.

**Example:**
An attacker embeds your site inside their malicious site and hides dangerous buttons.

**Django Protection:**
`XFrameOptionsMiddleware` adds headers to prevent your site from being rendered inside iframes:

```python
X_FRAME_OPTIONS = 'DENY'
```

---

### **4. Other Security Middleware**

| Middleware                   | Function                                              |
| ---------------------------- | ----------------------------------------------------- |
| **SecurityMiddleware**       | Forces HTTPS, adds HSTS, and strengthens headers.     |
| **AuthenticationMiddleware** | Ensures only logged-in users access restricted pages. |
| **SessionMiddleware**        | Protects against session fixation and hijacking.      |

---

## **What is WSGI?**

**WSGI (Web Server Gateway Interface)** is a standard interface between **Python web applications** and **web servers**.

###  Role of WSGI:

* Acts as a **bridge** between your Django app and the web server (like Apache, Nginx, or Gunicorn).
* Converts HTTP requests from the server into Python objects for Django to handle.
* Converts Django’s responses back into HTTP responses for the client.

**Example File:** `wsgi.py`

```python
import os
from django.core.wsgi import get_wsgi_application

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')
application = get_wsgi_application()
```

Here’s a **detailed and easy-to-understand Markdown (.md)** explanation for your next Django review points — covering **Models**, **on_delete**, **Fields & Validators**, and **Modules vs Classes**.
It’s written in a clean note format for quick reading and revision.

---

# 2. Django Models files

## **1. What is the Models File in Django?**

The **`models.py`** file in Django defines the **structure of your database tables** using Python code.
Each **Model** in Django represents a **table** in the database, and each **attribute** in the model represents a **field (column)**.

###  Key Features:

* Models are defined as **Python classes** that inherit from `django.db.models.Model`.
* Django automatically creates SQL tables for these models.
* You can use Django’s **ORM (Object Relational Mapper)** to query and manipulate data without writing SQL.

###  Example:

```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    roll_no = models.IntegerField(unique=True)
    email = models.EmailField()
    age = models.IntegerField()
```

This creates a table named `Student` with columns: `id`, `name`, `roll_no`, `email`, and `age`.

### Behind the Scenes:

* Django automatically adds an `id` field as the **primary key**.
* Each model can be linked to others using **relationships** (OneToOne, ForeignKey, ManyToMany).

---

## **2. What is on_delete = models.CASCADE?**

The `on_delete` parameter defines **what happens to related records** when the referenced object is deleted.

For example, if a **Student** belongs to a **Department**, and the Department is deleted — what should happen to the Student record?

### Example:

```python
class Department(models.Model):
    name = models.CharField(max_length=100)

class Student(models.Model):
    name = models.CharField(max_length=100)
    department = models.ForeignKey(Department, on_delete=models.CASCADE)
```

###  Explanation:

If the **Department** is deleted → all related **Students** are also deleted.
This is called **cascade deletion**.

---

### Other `on_delete` Options:

| Option             | Description                                                                           |
| ------------------ | ------------------------------------------------------------------------------------- |
| **CASCADE**        | Deletes all related objects automatically.                                            |
| **PROTECT**        | Prevents deletion if related objects exist.                                           |
| **SET_NULL**       | Sets the foreign key to `NULL` instead of deleting the record (requires `null=True`). |
| **SET_DEFAULT**    | Sets a default value instead of deleting.                                             |
| **DO_NOTHING**     | Does nothing — you must handle it manually.                                           |
| **RESTRICT (new)** | Similar to PROTECT but raises a specific error when deletion is blocked.              |

### Example:

```python
department = models.ForeignKey(Department, on_delete=models.SET_NULL, null=True)
```

If the department is deleted → the department field in `Student` becomes `NULL`.

---

## **3. Fields and Validators in Django Models**

Django provides many **Field types** that represent database columns with automatic validation.

---

### Common Field Types:

| Field Type                    | Description                           | Example                                                          |
| ----------------------------- | ------------------------------------- | ---------------------------------------------------------------- |
| **CharField**                 | For short text strings.               | `name = models.CharField(max_length=100)`                        |
| **TextField**                 | For long text (no max length).        | `bio = models.TextField()`                                       |
| **IntegerField**              | For storing integers.                 | `age = models.IntegerField()`                                    |
| **FloatField**                | For decimal numbers.                  | `price = models.FloatField()`                                    |
| **BooleanField**              | True or False values.                 | `is_active = models.BooleanField(default=True)`                  |
| **DateField / DateTimeField** | For dates or timestamps.              | `created_at = models.DateTimeField(auto_now_add=True)`           |
| **EmailField**                | Automatically validates email format. | `email = models.EmailField()`                                    |
| **URLField**                  | For storing valid URLs.               | `website = models.URLField()`                                    |
| **ForeignKey**                | Many-to-one relationship.             | `user = models.ForeignKey(User, on_delete=models.CASCADE)`       |
| **OneToOneField**             | One-to-one relationship.              | `profile = models.OneToOneField(User, on_delete=models.CASCADE)` |
| **ManyToManyField**           | Many-to-many relationship.            | `tags = models.ManyToManyField(Tag)`                             |

---

### Common Validators:

Validators are used to ensure **data correctness** before saving.

| Validator                 | Description                                 |
| ------------------------- | ------------------------------------------- |
| **MaxValueValidator(n)**  | Ensures value ≤ n                           |
| **MinValueValidator(n)**  | Ensures value ≥ n                           |
| **MaxLengthValidator(n)** | Limits the string length                    |
| **MinLengthValidator(n)** | Ensures minimum string length               |
| **EmailValidator()**      | Validates proper email format               |
| **RegexValidator()**      | Checks custom patterns (like phone numbers) |

###  Example:

```python
from django.core.validators import MaxValueValidator, MinValueValidator

class Product(models.Model):
    name = models.CharField(max_length=50)
    price = models.FloatField(validators=[MinValueValidator(0.0)])
    rating = models.IntegerField(validators=[MinValueValidator(1), MaxValueValidator(5)])
```


## **4. Difference Between Python Module and Python Class**

### **Python Module**

* A **Python file (.py)** that contains code (functions, classes, or variables).
* Helps organize code logically.
* You can **import** a module into another Python file.

**Example:**

```python
# student.py
def greeting():
    print("Hello Student!")
```

Now, you can import and use it:

```python
import student
student.greeting()
```

So, a **module = a single Python file**.

---

### **Python Class**

* A **blueprint** for creating objects.
* Defines **attributes (variables)** and **methods (functions)**.
* Supports **object-oriented programming (OOP)**.

**Example:**

```python
class Student:
    def __init__(self, name, roll_no):
        self.name = name
        self.roll_no = roll_no

    def show(self):
        print(f"Name: {self.name}, Roll No: {self.roll_no}")
```

So, a **class = a template used inside a module** to define an object’s behavior.

---

### Summary Table:

| Concept    | Python Module                       | Python Class                      |
| ---------- | ----------------------------------- | --------------------------------- |
| Definition | A `.py` file containing Python code | A blueprint for creating objects  |
| Contains   | Functions, variables, classes       | Attributes & methods              |
| Example    | `models.py`, `views.py`             | `class Student(models.Model)`     |
| Usage      | Used to organize code               | Used to model data/objects        |
| Relation   | A class can exist inside a module   | A module can contain many classes |

---


# 3. Django ORM & Database Concepts 

## **1. Django ORM (Object Relational Mapper)**

### What is ORM?

* ORM stands for **Object Relational Mapper**.
* It allows you to **interact with the database using Python code** instead of SQL.
* Django ORM automatically **translates Python queries into SQL queries** behind the scenes.

### Why use ORM?

* Easier to write and read than raw SQL.
* Database independent — works with PostgreSQL, MySQL, SQLite, etc.
* Provides built-in **data validation and security**.

### Example:

```python
from myapp.models import Student

# Create a new record
Student.objects.create(name="Raj", roll_no=101, marks=90)

# Retrieve all records
students = Student.objects.all()

# Filter records
Student.objects.filter(marks__gte=50)

# Update records
Student.objects.filter(roll_no=101).update(marks=95)

# Delete records
Student.objects.filter(name="Raj").delete()
```

Django ORM converts these commands into SQL automatically.

---

## **2. Using ORM Queries in Django Shell**

###  Steps to use the Django Shell:

1. Open terminal in your project directory.
2. Run:

   ```bash
   python manage.py shell
   ```
3. Import your model:

   ```python
   from myapp.models import Student
   ```
4. You can now perform ORM queries directly:

   ```python
   Student.objects.all()
   Student.objects.filter(marks__gte=80)
   Student.objects.get(id=1)
   ```

###  Tip:

You can also use:

```bash
python manage.py dbshell
```

to directly access the database using SQL (not ORM).

---

##  **3. Turning ORM Queries into SQL**

To see the **actual SQL query** generated by Django ORM, use:

```python
queryset = Student.objects.filter(marks__gte=50)
print(queryset.query)
```

This will output the SQL equivalent, for example:

```sql
SELECT * FROM "myapp_student" WHERE "myapp_student"."marks" >= 50;
```

This helps in debugging and understanding what’s happening behind the scenes.

---

## **4. What are Aggregations?**

### Definition:

Aggregation means **performing calculations** like sum, average, count, min, or max on a group of data.

Django provides functions for this under `django.db.models`.

###  Common Aggregation Functions:

* `Sum`
* `Avg`
* `Count`
* `Min`
* `Max`

###  Example:

```python
from django.db.models import Avg, Count, Sum, Max, Min
from myapp.models import Student

Student.objects.aggregate(Avg('marks'))
# Output: {'marks__avg': 78.5}

Student.objects.aggregate(Sum('marks'))
# Output: {'marks__sum': 450}
```

You can also combine multiple aggregations:

```python
Student.objects.aggregate(Avg('marks'), Max('marks'), Min('marks'))
```

### Use Case:

Useful for generating **reports, dashboards, and summaries**.

---

## **5. What are Annotations?**

### Definition:

Annotation allows you to **add calculated fields** to each object in a queryset.

Unlike Aggregation (which gives one result), Annotation **adds an extra field** to every row.

### Example:

```python
from django.db.models import F, Sum
from myapp.models import Student, Subject

# Example: Total marks per student
students = Student.objects.annotate(total_marks=Sum('subject__marks'))

for s in students:
    print(s.name, s.total_marks)
```

Here, each student object now has an extra attribute `total_marks` that is calculated dynamically.

### 🔹 Use Case:

Used for **ranking, scoring, or calculating totals** in real time without writing SQL.

---

## **6. What is a Migration File? Why is it Needed?**

### Definition:

Migration files are **auto-generated Python files** that record changes made to your models (like adding a new field, deleting a model, etc.).

### 🔹 Why Needed?

They **keep the database schema in sync** with your `models.py` file.

When you modify a model, Django tracks the change and allows you to apply it to the database using migrations.

###  Example Workflow:

1. You add a new field in your model:

   ```python
   class Student(models.Model):
       name = models.CharField(max_length=100)
       marks = models.IntegerField()
       age = models.IntegerField(default=18)
   ```
2. Run the following commands:

   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```
3. Django creates a migration file like:

   ```
   0002_student_age.py
   ```

   and updates the database schema accordingly.

### Summary:

| Command          | Purpose                                                |
| ---------------- | ------------------------------------------------------ |
| `makemigrations` | Detects changes in models and prepares migration files |
| `migrate`        | Applies the migrations to update the actual database   |
| `showmigrations` | Lists all migrations and their status                  |
| `sqlmigrate`     | Shows the SQL code for a specific migration            |

---

## **7. What are SQL Transactions? (Non-ORM Concept)**

### Definition:

A **transaction** is a group of one or more SQL operations that are executed as a **single unit**.

If all operations succeed → the transaction is committed.
If any operation fails → all changes are **rolled back** (undone).

###  Example (in SQL):

```sql
BEGIN;
INSERT INTO accounts VALUES ('Raj', 1000);
UPDATE accounts SET balance = balance - 500 WHERE name='Raj';
COMMIT;
```

If any query fails, you can rollback:

```sql
ROLLBACK;
```

### Purpose:

* Ensures **data integrity**.
* Prevents **partial updates** (if something fails midway).
* Follows **ACID principles**:

  * **A**tomicity: All or nothing.
  * **C**onsistency: Keeps data valid.
  * **I**solation: Transactions don’t affect each other.
  * **D**urability: Once committed, changes are saved.

---

## **8. What are Atomic Transactions in Django?**

### Definition:

In Django, **atomic transactions** ensure that all database operations inside a block are treated as a single transaction.

If any statement fails, **all changes are rolled back automatically**.

### Example:

```python
from django.db import transaction

@transaction.atomic
def transfer_money(sender, receiver, amount):
    sender.balance -= amount
    receiver.balance += amount
    sender.save()
    receiver.save()
```

If an error occurs in the middle, Django **automatically rolls back** all changes — no partial updates happen.

###  Manual Use:

You can also use it like this:

```python
with transaction.atomic():
    # database operations
    ...
```

### Benefits:

* Prevents inconsistent data.
* Ensures **safe financial or critical operations**.
* Makes debugging and error handling easier.

---

## Summary Table

| Concept                | Description                                   | Example                                   |
| ---------------------- | --------------------------------------------- | ----------------------------------------- |
| **ORM**                | Interacts with the database using Python code | `Student.objects.filter(age=20)`          |
| **Aggregation**        | Performs calculations like SUM, AVG, COUNT    | `Student.objects.aggregate(Sum('marks'))` |
| **Annotation**         | Adds computed fields to each record           | `.annotate(total=Sum('subject__marks'))`  |
| **Migration**          | Updates DB schema when models change          | `makemigrations`, `migrate`               |
| **SQL Transaction**    | Group of SQL statements executed together     | `BEGIN...COMMIT...ROLLBACK`               |
| **Atomic Transaction** | Django’s way of handling safe transactions    | `@transaction.atomic`                     |

---



#  **References**

###  **General Django Documentation**

1. Django Software Foundation. *Official Django Documentation.*
   URL: [https://docs.djangoproject.com/en/stable/](https://docs.djangoproject.com/en/stable/)
   Description: The primary and most authoritative source for Django concepts including ORM, Middleware, Models, and Security settings.

---

### **Settings File & WSGI**

2. Django Documentation. *Settings — Django configuration system.*
   URL: [https://docs.djangoproject.com/en/stable/topics/settings/](https://docs.djangoproject.com/en/stable/topics/settings/)
   Description: Provides complete information about Django’s `settings.py`, including `SECRET_KEY`, installed apps, and configuration best practices.

3. Python Software Foundation. *Web Server Gateway Interface (WSGI) — PEP 3333.*
   URL: [https://peps.python.org/pep-3333/](https://peps.python.org/pep-3333/)
   Description: Explains how WSGI serves as an interface between web servers and Python web applications, including Django.

4. DigitalOcean Community Tutorials. *How To Set Up Django with Gunicorn and Nginx (WSGI).*
   URL: [https://www.digitalocean.com/community/tutorials](https://www.digitalocean.com/community/tutorials)
   Description: Practical setup and explanation of how WSGI works in deployment with Django.

---

### **Models & Fields**

5. Django Documentation. *Models — Defining your data models.*
   URL: [https://docs.djangoproject.com/en/stable/topics/db/models/](https://docs.djangoproject.com/en/stable/topics/db/models/)
   Description: Details about Django models, fields, and relationships including `on_delete` behaviors.

6. Django Documentation. *Model field reference.*
   URL: [https://docs.djangoproject.com/en/stable/ref/models/fields/](https://docs.djangoproject.com/en/stable/ref/models/fields/)
   Description: Lists all built-in field types (e.g., CharField, IntegerField) and their parameters.

7. Real Python. *Django Models: Mastering Database Interactions.*
   URL: [https://realpython.com/django-models-database/](https://realpython.com/django-models-database/)
   Description: A simplified explanation of how Django ORM maps models to SQL tables.

---

### **ORM, Aggregations & Migrations**

8. Django Documentation. *QuerySets and ORM.*
   URL: [https://docs.djangoproject.com/en/stable/topics/db/queries/](https://docs.djangoproject.com/en/stable/topics/db/queries/)
   Description: Covers QuerySet methods, filters, and query optimization.

9. Django Documentation. *Aggregation and Annotation.*
   URL: [https://docs.djangoproject.com/en/stable/topics/db/aggregation/](https://docs.djangoproject.com/en/stable/topics/db/aggregation/)
   Description: Official explanation of `annotate()` and `aggregate()` methods in ORM.

10. Django Documentation. *Migrations — Managing database schema changes.*
    URL: [https://docs.djangoproject.com/en/stable/topics/migrations/](https://docs.djangoproject.com/en/stable/topics/migrations/)
    Description: Describes how Django tracks and applies model changes to the database.

11. GeeksforGeeks. *Django ORM Queries – A Complete Guide.*
    URL: [https://www.geeksforgeeks.org/django-orm-queries/](https://www.geeksforgeeks.org/django-orm-queries/)
    Description: A simplified overview with code examples for ORM operations.

12. Stack Overflow Discussions — *Django ORM Internals and SQL Conversion.*
    URL: [https://stackoverflow.com/questions/tagged/django-orm](https://stackoverflow.com/questions/tagged/django-orm)
    Description: Helpful community insights on using `.query` and SQL debugging.

---

### **Transactions**

13. Django Documentation. *Transactions — Managing database integrity.*
    URL: [https://docs.djangoproject.com/en/stable/topics/db/transactions/](https://docs.djangoproject.com/en/stable/topics/db/transactions/)
    Description: Explains `@transaction.atomic` and how Django ensures data consistency.

14. W3Schools SQL Tutorial. *SQL Transactions.*
    URL: [https://www.w3schools.com/sql/sql_transactions.asp](https://www.w3schools.com/sql/sql_transactions.asp)
    Description: Easy-to-understand explanation of SQL transactions, commits, and rollbacks.

15. PostgreSQL Manual. *Transactions.*
    URL: [https://www.postgresql.org/docs/current/tutorial-transactions.html](https://www.postgresql.org/docs/current/tutorial-transactions.html)
    Description: Official explanation of transaction handling in relational databases.

---

### **Security (CSRF, XSS, Clickjacking, Middleware)**

16. Django Documentation. *Security Overview.*
    URL: [https://docs.djangoproject.com/en/stable/topics/security/](https://docs.djangoproject.com/en/stable/topics/security/)
    Description: Covers Django’s built-in protections like CSRF, XSS, SQL injection, and clickjacking prevention.

17. Mozilla Developer Network (MDN). *Cross-Site Scripting (XSS).*
    URL: [https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)
    Description: In-depth explanation of XSS attacks and mitigation strategies.

18. OWASP Foundation. *Cross-Site Request Forgery (CSRF).*
    URL: [https://owasp.org/www-community/attacks/csrf](https://owasp.org/www-community/attacks/csrf)
    Description: Details on CSRF vulnerabilities and defense mechanisms.

19. OWASP Foundation. *Clickjacking Defense Cheat Sheet.*
    URL: [https://owasp.org/www-project-cheat-sheets/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html](https://owasp.org/www-project-cheat-sheets/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html)
    Description: Explains how `X-Frame-Options` headers protect Django apps from clickjacking.

20. Real Python. *Understanding Django Middleware and Security Layers.*
    URL: [https://realpython.com/django-middleware/](https://realpython.com/django-middleware/)
    Description: Provides examples and simplified explanations of middleware functions.

---

### **Python Concepts**

21. Python Documentation. *Modules vs Classes.*
    URL: [https://docs.python.org/3/tutorial/modules.html](https://docs.python.org/3/tutorial/modules.html)
    Description: Explains how Python modules and classes differ conceptually and in use.

22. GeeksforGeeks. *Difference Between Python Module and Class.*
    URL: [https://www.geeksforgeeks.org/difference-between-python-module-and-class/](https://www.geeksforgeeks.org/difference-between-python-module-and-class/)
    Description: Simple explanation comparing module-level and class-level code.

---

### **Tutorials and Community Guides**

23. FreeCodeCamp. *Complete Django for Beginners Tutorial.*
    URL: [https://www.freecodecamp.org/news/django-beginners-guide/](https://www.freecodecamp.org/news/django-beginners-guide/)
    Description: Comprehensive beginner’s guide to Django with examples and exercises.

24. TutorialsPoint. *Django Framework Overview.*
    URL: [https://www.tutorialspoint.com/django/](https://www.tutorialspoint.com/django/)
    Description: Simplified theoretical overview of all Django components.

25. Medium Articles — *Django ORM, Security, and Middleware Explained Simply.*
    URL: [https://medium.com/tag/django](https://medium.com/tag/django)
    Description: Collection of easy-to-understand Django articles for practical learners.
---

### **Citation Format (Optional for Report)**

If you are preparing a **thesis or report**, you can cite as:

> Django Software Foundation. *Django Documentation (Version 5.0)*. Retrieved from [https://docs.djangoproject.com/en/stable/](https://docs.djangoproject.com/en/stable/)

---
