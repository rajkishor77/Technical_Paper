# 🐍 Python Interview Questions & Answers
### JSW Data Engineer Interview Preparation — Friday

---
## 📋 Table of Contents
> Click on any question to jump directly to its answer

- [🐍 Python Interview Questions \& Answers](#-python-interview-questions--answers)
    - [JSW Data Engineer Interview Preparation — Friday](#jsw-data-engineer-interview-preparation--friday)
  - [📋 Table of Contents](#-table-of-contents)
  - [🔵 SECTION 1: PYTHON BASICS (Q1–Q15)](#-section-1-python-basics-q1q15)
    - [🔷 Q1. What is Python and why is it used in Data Engineering?](#-q1-what-is-python-and-why-is-it-used-in-data-engineering)
    - [🔷 Q2. What are Python's key data types?](#-q2-what-are-pythons-key-data-types)
    - [🔷 Q3. What is the difference between List, Tuple, Set and Dictionary?](#-q3-what-is-the-difference-between-list-tuple-set-and-dictionary)
    - [🔷 Q4. What is the difference between `==` and `is` in Python?](#-q4-what-is-the-difference-between--and-is-in-python)
    - [🔷 Q5. What are \*args and \*\*kwargs?](#-q5-what-are-args-and-kwargs)
    - [🔷 Q6. What is a Lambda function?](#-q6-what-is-a-lambda-function)
    - [🔷 Q7. What is List Comprehension?](#-q7-what-is-list-comprehension)
    - [🔷 Q8. What is the difference between `append()` and `extend()` in a list?](#-q8-what-is-the-difference-between-append-and-extend-in-a-list)
    - [🔷 Q9. What is a Python Dictionary and how to iterate over it?](#-q9-what-is-a-python-dictionary-and-how-to-iterate-over-it)
    - [🔷 Q10. What is the difference between `deepcopy` and `shallowcopy`?](#-q10-what-is-the-difference-between-deepcopy-and-shallowcopy)
    - [🔷 Q11. What are Python Decorators?](#-q11-what-are-python-decorators)
    - [🔷 Q12. What are Python Generators?](#-q12-what-are-python-generators)
    - [🔷 Q13. What is Exception Handling in Python?](#-q13-what-is-exception-handling-in-python)
    - [🔷 Q14. What is the difference between `read()`, `readline()` and `readlines()`?](#-q14-what-is-the-difference-between-read-readline-and-readlines)
    - [🔷 Q15. What is `with` statement in Python?](#-q15-what-is-with-statement-in-python)
  - [🟡 SECTION 1B: PYTHON THEORY (Q15B–Q15F)](#-section-1b-python-theory-q15bq15f)
    - [🔷 Q15B. What is Python's GIL (Global Interpreter Lock)?](#-q15b-what-is-pythons-gil-global-interpreter-lock)
    - [🔷 Q15C. What is Python Memory Management?](#-q15c-what-is-python-memory-management)
    - [🔷 Q15D. What are Python's Mutable vs Immutable types?](#-q15d-what-are-pythons-mutable-vs-immutable-types)
    - [🔷 Q15E. What is Type Hinting in Python?](#-q15e-what-is-type-hinting-in-python)
    - [🔷 Q15F. What is the difference between `is`, `in` and `==` operators?](#-q15f-what-is-the-difference-between-is-in-and--operators)
  - [🟡 SECTION 2: OOP CONCEPTS (Q16–Q22)](#-section-2-oop-concepts-q16q22)
    - [🔷 Q16. What are the 4 pillars of OOP?](#-q16-what-are-the-4-pillars-of-oop)
    - [🔷 Q17. What is the difference between a Class and an Object?](#-q17-what-is-the-difference-between-a-class-and-an-object)
    - [🔷 Q18. What is `__init__` in Python?](#-q18-what-is-__init__-in-python)
    - [🔷 Q19. What is Inheritance in Python?](#-q19-what-is-inheritance-in-python)
    - [🔷 Q20. What is the difference between `@classmethod` and `@staticmethod`?](#-q20-what-is-the-difference-between-classmethod-and-staticmethod)
    - [🔷 Q21. What is Abstraction in Python?](#-q21-what-is-abstraction-in-python)
    - [🔷 Q22. What is Encapsulation in Python?](#-q22-what-is-encapsulation-in-python)
  - [🟠 SECTION 3: DATA ENGINEERING WITH PYTHON (Q23–Q38)](#-section-3-data-engineering-with-python-q23q38)
    - [🔷 Q23. What is Pandas and why is it used?](#-q23-what-is-pandas-and-why-is-it-used)
    - [🔷 Q24. How do you read a CSV file using Pandas?](#-q24-how-do-you-read-a-csv-file-using-pandas)
    - [🔷 Q25. How do you handle missing values in Pandas?](#-q25-how-do-you-handle-missing-values-in-pandas)
    - [🔷 Q26. How do you filter data in a Pandas DataFrame?](#-q26-how-do-you-filter-data-in-a-pandas-dataframe)
    - [🔷 Q27. What is `groupby()` in Pandas?](#-q27-what-is-groupby-in-pandas)
    - [🔷 Q28. What is `merge()` vs `concat()` in Pandas?](#-q28-what-is-merge-vs-concat-in-pandas)
    - [🔷 Q29. How do you connect Python to PostgreSQL?](#-q29-how-do-you-connect-python-to-postgresql)
    - [🔷 Q30. How do you read data from PostgreSQL into Pandas?](#-q30-how-do-you-read-data-from-postgresql-into-pandas)
    - [🔷 Q31. What is SQLAlchemy and why use it?](#-q31-what-is-sqlalchemy-and-why-use-it)
    - [🔷 Q32. What is ETL in Data Engineering?](#-q32-what-is-etl-in-data-engineering)
    - [🔷 Q33. How do you read and write JSON files in Python?](#-q33-how-do-you-read-and-write-json-files-in-python)
    - [🔷 Q34. How do you work with dates in Python?](#-q34-how-do-you-work-with-dates-in-python)
    - [🔷 Q35. What is the difference between `map()`, `filter()` and `reduce()`?](#-q35-what-is-the-difference-between-map-filter-and-reduce)
    - [🔷 Q36. How do you remove duplicates from a list?](#-q36-how-do-you-remove-duplicates-from-a-list)
    - [🔷 Q37. How do you sort a list of dictionaries in Python?](#-q37-how-do-you-sort-a-list-of-dictionaries-in-python)
    - [🔷 Q38. What is Multithreading vs Multiprocessing in Python?](#-q38-what-is-multithreading-vs-multiprocessing-in-python)
    - [🔷 Q38B. What is the difference between Compiled and Interpreted languages?](#-q38b-what-is-the-difference-between-compiled-and-interpreted-languages)
    - [🔷 Q38C. What is Lazy Evaluation in Python?](#-q38c-what-is-lazy-evaluation-in-python)
    - [🔷 Q38D. What is a Context Manager and how to create one?](#-q38d-what-is-a-context-manager-and-how-to-create-one)
  - [🔴 SECTION 4: DATA PIPELINES \& ADVANCED (Q39–Q50)](#-section-4-data-pipelines--advanced-q39q50)
    - [🔷 Q39. What is Apache Kafka and how does Python connect to it?](#-q39-what-is-apache-kafka-and-how-does-python-connect-to-it)
    - [🔷 Q40. What is RabbitMQ and how is it different from Kafka?](#-q40-what-is-rabbitmq-and-how-is-it-different-from-kafka)
    - [🔷 Q41. What is Celery in Python?](#-q41-what-is-celery-in-python)
    - [🔷 Q42. What is Redis and how is it used in Data Engineering?](#-q42-what-is-redis-and-how-is-it-used-in-data-engineering)
    - [🔷 Q43. What is MongoDB and how to use it with Python?](#-q43-what-is-mongodb-and-how-to-use-it-with-python)
    - [🔷 Q44. What is Docker and why is it used in Data Engineering?](#-q44-what-is-docker-and-why-is-it-used-in-data-engineering)
    - [🔷 Q45. What is a virtual environment in Python and why use it?](#-q45-what-is-a-virtual-environment-in-python-and-why-use-it)
    - [🔷 Q46. How do you schedule a Python script to run automatically?](#-q46-how-do-you-schedule-a-python-script-to-run-automatically)
    - [🔷 Q47. How do you make an API call in Python?](#-q47-how-do-you-make-an-api-call-in-python)
    - [🔷 Q48. How do you write a Python script to insert bulk data into PostgreSQL efficiently?](#-q48-how-do-you-write-a-python-script-to-insert-bulk-data-into-postgresql-efficiently)
    - [🔷 Q49. What is Data Versioning and how do you handle it?](#-q49-what-is-data-versioning-and-how-do-you-handle-it)
    - [🔷 Q50. What is the difference between `iloc` and `loc` in Pandas?](#-q50-what-is-the-difference-between-iloc-and-loc-in-pandas)
  - [🔵 SECTION 5: JSW SPECIFIC + EXTRA IMPORTANT (Q51–Q65)](#-section-5-jsw-specific--extra-important-q51q65)
    - [🔷 Q51. Tell me about yourself and your projects. (Most asked at JSW)](#-q51-tell-me-about-yourself-and-your-projects-most-asked-at-jsw)
    - [🔷 Q52. What is the difference between ETL and ELT?](#-q52-what-is-the-difference-between-etl-and-elt)
    - [🔷 Q53. What is Data Pipeline? How did you build one?](#-q53-what-is-data-pipeline-how-did-you-build-one)
    - [🔷 Q54. What is Data Quality and how do you ensure it?](#-q54-what-is-data-quality-and-how-do-you-ensure-it)
    - [🔷 Q55. What is the difference between OLTP and OLAP?](#-q55-what-is-the-difference-between-oltp-and-olap)
    - [🔷 Q56. What is Idempotency in Data Engineering?](#-q56-what-is-idempotency-in-data-engineering)
    - [🔷 Q57. How do you handle large files in Python without loading into memory?](#-q57-how-do-you-handle-large-files-in-python-without-loading-into-memory)
    - [🔷 Q58. What is Data Normalization? Explain 1NF, 2NF, 3NF.](#-q58-what-is-data-normalization-explain-1nf-2nf-3nf)
    - [🔷 Q59. What is an Index in a database and why is it important?](#-q59-what-is-an-index-in-a-database-and-why-is-it-important)
    - [🔷 Q60. What is the difference between `DELETE`, `TRUNCATE` and `DROP` in SQL?](#-q60-what-is-the-difference-between-delete-truncate-and-drop-in-sql)
    - [🔷 Q61. What are Window Functions in SQL? (Very Important)](#-q61-what-are-window-functions-in-sql-very-important)
    - [🔷 Q62. What is a CTE (Common Table Expression) in SQL?](#-q62-what-is-a-cte-common-table-expression-in-sql)
    - [🔷 Q63. How do you debug a failing data pipeline?](#-q63-how-do-you-debug-a-failing-data-pipeline)
    - [🔷 Q64. What is the difference between Synchronous and Asynchronous processing?](#-q64-what-is-the-difference-between-synchronous-and-asynchronous-processing)
    - [🔷 Q65. Behavioral Questions — JSW will definitely ask these](#-q65-behavioral-questions--jsw-will-definitely-ask-these)
  - [💡 FINAL QUICK REVISION](#-final-quick-revision)


---

## 🔵 SECTION 1: PYTHON BASICS (Q1–Q15)

---

<a id="q1"></a>
### 🔷 Q1. What is Python and why is it used in Data Engineering?

> **What is Python:**
> - High-level, interpreted, general-purpose programming language
> - Created by Guido van Rossum in 1991
> - Easy to read and write — syntax is close to plain English
> - Dynamically typed — no need to declare variable types
>
> **Why Data Engineers use Python:**
> - Write ETL pipelines — extract, transform and load data
> - Huge library support — Pandas, NumPy, SQLAlchemy, PySpark
> - Connect to any database — PostgreSQL, MongoDB, Redis, MySQL
> - Call REST APIs and scrape web data easily
> - Automate and schedule data jobs using Celery, Airflow
> - Process large files — CSV, JSON, Parquet, Excel
> - Integrate with cloud platforms — AWS S3, GCP, Azure
> - Work with messaging tools — Kafka, RabbitMQ
> - Strong community support and documentation

---

<a id="q2"></a>
### 🔷 Q2. What are Python's key data types?

> - **int** — whole numbers → `10, -5`
> - **float** — decimal numbers → `3.14`
> - **str** — text → `"hello"`
> - **bool** — True or False
> - **list** — ordered, changeable → `[1, 2, 3]`
> - **tuple** — ordered, unchangeable → `(1, 2, 3)`
> - **dict** — key-value pairs → `{"name": "Raj"}`
> - **set** — unique unordered values → `{1, 2, 3}`

---

<a id="q3"></a>
### 🔷 Q3. What is the difference between List, Tuple, Set and Dictionary?

| | List | Tuple | Set | Dictionary |
|---|---|---|---|---|
| Ordered | ✅ Yes | ✅ Yes | ❌ No | ✅ Yes (Python 3.7+) |
| Changeable | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes |
| Duplicates | ✅ Yes | ✅ Yes | ❌ No | Keys: ❌ No |
| Syntax | `[ ]` | `( )` | `{ }` | `{ key: value }` |
| Use case | General data storage | Fixed data, faster | Unique values | Key-value mapping |

---

<a id="q4"></a>
### 🔷 Q4. What is the difference between `==` and `is` in Python?

> - `==` checks if **values** are equal → `5 == 5` → True
> - `is` checks if both point to the **same object in memory** → used for None check
>
> **Best Practice:** Always use `if x is None` not `if x == None`

---

<a id="q5"></a>
### 🔷 Q5. What are *args and **kwargs?

> - `*args` — allows passing **multiple positional arguments** to a function
> - `**kwargs` — allows passing **multiple keyword arguments** as a dictionary

```python
def show(*args, **kwargs):
    print(args)    # (1, 2, 3)
    print(kwargs)  # {'name': 'Raj', 'age': 23}

show(1, 2, 3, name="Raj", age=23)
```

---

<a id="q6"></a>
### 🔷 Q6. What is a Lambda function?

> A small anonymous function written in one line.

```python
# Normal function
def add(a, b):
    return a + b

# Lambda function
add = lambda a, b: a + b
print(add(3, 4))  # 7
```

> Commonly used with `map()`, `filter()`, `sorted()`

---

<a id="q7"></a>
### 🔷 Q7. What is List Comprehension?

> A short and clean way to create a list.

```python
# Normal way
squares = []
for i in range(5):
    squares.append(i * i)

# List Comprehension
squares = [i * i for i in range(5)]
# Output: [0, 1, 4, 9, 16]
```

---

<a id="q8"></a>
### 🔷 Q8. What is the difference between `append()` and `extend()` in a list?

```python
a = [1, 2, 3]

a.append([4, 5])   # Adds as single element → [1, 2, 3, [4, 5]]
a.extend([4, 5])   # Adds each element → [1, 2, 3, 4, 5]
```

---

<a id="q9"></a>
### 🔷 Q9. What is a Python Dictionary and how to iterate over it?

```python
data = {"name": "Raj", "age": 23, "city": "Bengaluru"}

# Iterate keys
for key in data:
    print(key)

# Iterate keys and values
for key, value in data.items():
    print(key, value)
```

---

<a id="q10"></a>
### 🔷 Q10. What is the difference between `deepcopy` and `shallowcopy`?

> - **Shallow copy** — copies the object but NOT the nested objects inside. Changes in nested objects affect both.
> - **Deep copy** — copies everything including nested objects. Completely independent copy.

```python
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)
deep = copy.deepcopy(original)

original[0][0] = 99
print(shallow)  # [[99, 2], [3, 4]] — affected!
print(deep)     # [[1, 2], [3, 4]]  — not affected
```

---

<a id="q11"></a>
### 🔷 Q11. What are Python Decorators?

> **What is a Decorator:**
> - A function that wraps another function to add extra behavior
> - Does not change the original function's code
> - Uses `@decorator_name` syntax above the function
> - Returns a new function with added functionality
>
> **Real-world uses in Data Engineering:**
> - Logging — log when a pipeline function starts and ends
> - Timing — measure how long a function takes to run
> - Retry logic — retry a function if it fails (API calls, DB connections)
> - Authentication — check if user is authorized before running function
> - Caching — store results of expensive functions

```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Done")
        return result
    return wrapper

@log_decorator
def add(a, b):
    return a + b

add(3, 4)
# Calling add
# Done
```

---

<a id="q12"></a>
### 🔷 Q12. What are Python Generators?

> Generators are functions that yield values one at a time instead of returning all at once. They save memory for large data.

```python
def number_generator(n):
    for i in range(n):
        yield i

gen = number_generator(5)
print(next(gen))  # 0
print(next(gen))  # 1
```

> **Data Engineering use:** Reading large CSV files line by line without loading entire file into memory.

---

<a id="q13"></a>
### 🔷 Q13. What is Exception Handling in Python?

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
finally:
    print("This always runs")
```

> **Data Engineering use:** Handling database connection failures, file not found errors, API timeouts.

---

<a id="q14"></a>
### 🔷 Q14. What is the difference between `read()`, `readline()` and `readlines()`?

```python
# read()      → reads entire file as one string
# readline()  → reads one line at a time
# readlines() → reads all lines and returns a list

with open("data.txt", "r") as f:
    content = f.read()        # entire file
    line = f.readline()       # one line
    lines = f.readlines()     # list of all lines
```

---

<a id="q15"></a>
### 🔷 Q15. What is `with` statement in Python?

> `with` statement automatically handles opening and closing of resources like files or database connections — even if an error occurs.

```python
# Without with — need to manually close
f = open("data.txt")
data = f.read()
f.close()

# With with — auto closes
with open("data.txt") as f:
    data = f.read()
```

---

## 🟡 SECTION 1B: PYTHON THEORY (Q15B–Q15F)

---

<a id="q15b"></a>
### 🔷 Q15B. What is Python's GIL (Global Interpreter Lock)?

> GIL is a lock in Python that allows only one thread to execute Python code at a time — even on multi-core CPUs.
>
> - This means **multithreading does NOT give true parallelism** in Python for CPU-heavy tasks
> - For **I/O tasks** (file reading, API calls, DB queries) — multithreading still works well because GIL is released during I/O wait
> - For **CPU-heavy tasks** — use `multiprocessing` instead, which creates separate processes each with their own GIL

---

<a id="q15c"></a>
### 🔷 Q15C. What is Python Memory Management?

> Python manages memory automatically using:
> - **Reference Counting** — every object tracks how many variables point to it. When count reaches 0, memory is freed.
> - **Garbage Collector** — handles circular references (when two objects reference each other but nothing else points to them)
> - **Heap Memory** — all Python objects are stored in a private heap
>
> You can manually free memory using `del variable` or `gc.collect()`

---

<a id="q15d"></a>
### 🔷 Q15D. What are Python's Mutable vs Immutable types?

| Mutable (can be changed) | Immutable (cannot be changed) |
|---|---|
| list, dict, set | int, float, str, tuple |
| `a = [1,2]; a.append(3)` ✅ | `s = "hello"; s[0] = "H"` ❌ Error |

> **Why it matters in Data Engineering:** Passing mutable objects to functions can cause unexpected changes. Always be careful when modifying DataFrames inside functions.

---

<a id="q15e"></a>
### 🔷 Q15E. What is Type Hinting in Python?

> Type hints tell you (and the IDE) what data type a function expects and returns. Makes code more readable and easier to debug.

```python
def calculate_profit(sales: float, cost: float) -> float:
    return sales - cost

def process_data(df: pd.DataFrame, region: str) -> pd.DataFrame:
    return df[df['region'] == region]
```

> Type hints are not enforced at runtime — they are just hints for developers.

---

<a id="q15f"></a>
### 🔷 Q15F. What is the difference between `is`, `in` and `==` operators?

```python
# == checks value equality
5 == 5         # True
[1,2] == [1,2] # True

# is checks same memory object
a = [1, 2]
b = a
b is a         # True — same object
c = [1, 2]
c is a         # False — different objects, same value

# in checks membership
3 in [1, 2, 3]         # True
'name' in {'name': 'Raj'}  # True — checks keys in dict
```

---

## 🟡 SECTION 2: OOP CONCEPTS (Q16–Q22)

---

<a id="q16"></a>
### 🔷 Q16. What are the 4 pillars of OOP?

> 1. **Encapsulation** — hiding data inside a class
> 2. **Inheritance** — child class gets properties of parent class
> 3. **Polymorphism** — same function behaves differently in different classes
> 4. **Abstraction** — hiding complex implementation, showing only what is needed

---

<a id="q17"></a>
### 🔷 Q17. What is the difference between a Class and an Object?

> - **Class** — blueprint or template
> - **Object** — actual instance created from the class

```python
class Car:              # Class
    def __init__(self, brand):
        self.brand = brand

my_car = Car("Toyota")  # Object
print(my_car.brand)     # Toyota
```

---

<a id="q18"></a>
### 🔷 Q18. What is `__init__` in Python?

> `__init__` is the constructor method that runs automatically when an object is created. It initializes the object's attributes.

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

emp = Employee("Raj", 50000)
print(emp.name)    # Raj
```

---

<a id="q19"></a>
### 🔷 Q19. What is Inheritance in Python?

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):       # Dog inherits from Animal
    def speak(self):
        print("Woof!")

d = Dog()
d.speak()   # Woof!
```

---

<a id="q20"></a>
### 🔷 Q20. What is the difference between `@classmethod` and `@staticmethod`?

| | @classmethod | @staticmethod |
|---|---|---|
| Access | Can access class variables via `cls` | Cannot access class or instance |
| Use | Alternative constructors | Utility functions related to class |
| First argument | `cls` | Nothing special |

---

<a id="q21"></a>
### 🔷 Q21. What is Abstraction in Python?

> Hiding implementation details and showing only the necessary interface using `ABC` (Abstract Base Class).

```python
from abc import ABC, abstractmethod

class Pipeline(ABC):
    @abstractmethod
    def extract(self):
        pass

    @abstractmethod
    def load(self):
        pass
```

---

<a id="q22"></a>
### 🔷 Q22. What is Encapsulation in Python?

> Restricting direct access to data using private (`__`) or protected (`_`) variables.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance    # private variable

    def get_balance(self):          # access via method only
        return self.__balance

acc = BankAccount(10000)
print(acc.get_balance())   # 10000
# print(acc.__balance)     # Error! Cannot access directly
```

---

## 🟠 SECTION 3: DATA ENGINEERING WITH PYTHON (Q23–Q38)

---

<a id="q23"></a>
### 🔷 Q23. What is Pandas and why is it used?

> Pandas is a Python library for data manipulation and analysis. It provides two main structures:
> - **Series** — one-dimensional labeled array (like a column)
> - **DataFrame** — two-dimensional table (like a spreadsheet or SQL table)

---

<a id="q24"></a>
### 🔷 Q24. How do you read a CSV file using Pandas?

```python
import pandas as pd

df = pd.read_csv("data.csv")
print(df.head())        # first 5 rows
print(df.shape)         # (rows, columns)
print(df.columns)       # column names
print(df.dtypes)        # data types
print(df.info())        # full summary
```

---

<a id="q25"></a>
### 🔷 Q25. How do you handle missing values in Pandas?

```python
# Check missing values
df.isnull().sum()

# Drop rows with null values
df.dropna(inplace=True)

# Fill null values with a value
df['sales'].fillna(0, inplace=True)

# Fill with mean
df['sales'].fillna(df['sales'].mean(), inplace=True)
```

---

<a id="q26"></a>
### 🔷 Q26. How do you filter data in a Pandas DataFrame?

```python
# Filter rows where Sales > 1000
df_filtered = df[df['Sales'] > 1000]

# Multiple conditions
df_filtered = df[(df['Sales'] > 1000) & (df['Region'] == 'East')]

# Filter using query
df_filtered = df.query("Sales > 1000 and Region == 'East'")
```

---

<a id="q27"></a>
### 🔷 Q27. What is `groupby()` in Pandas?

> Groups data by a column and applies aggregation functions — similar to SQL GROUP BY.

```python
# Total sales by region
df.groupby('Region')['Sales'].sum()

# Multiple aggregations
df.groupby('Region').agg({
    'Sales': 'sum',
    'Profit': 'mean',
    'Orders': 'count'
})
```

---

<a id="q28"></a>
### 🔷 Q28. What is `merge()` vs `concat()` in Pandas?

```python
# merge() — like SQL JOIN, combines on a common column
result = pd.merge(df1, df2, on='customer_id', how='inner')

# concat() — stacks DataFrames on top of each other (like SQL UNION)
result = pd.concat([df1, df2], ignore_index=True)
```

---

<a id="q29"></a>
### 🔷 Q29. How do you connect Python to PostgreSQL?

```python
import psycopg2

conn = psycopg2.connect(
    host="localhost",
    database="mydb",
    user="postgres",
    password="password"
)

cursor = conn.cursor()
cursor.execute("SELECT * FROM sales LIMIT 10")
rows = cursor.fetchall()

for row in rows:
    print(row)

conn.close()
```

---

<a id="q30"></a>
### 🔷 Q30. How do you read data from PostgreSQL into Pandas?

```python
import pandas as pd
import psycopg2

conn = psycopg2.connect(host="localhost", database="mydb",
                         user="postgres", password="password")

df = pd.read_sql("SELECT * FROM sales WHERE year = 2024", conn)
print(df.head())
conn.close()
```

---

<a id="q31"></a>
### 🔷 Q31. What is SQLAlchemy and why use it?

> SQLAlchemy is a Python library that provides a clean way to interact with databases using Python code (ORM) instead of raw SQL. It supports PostgreSQL, MySQL, SQLite and more.

```python
from sqlalchemy import create_engine
import pandas as pd

engine = create_engine('postgresql://user:password@localhost/mydb')

# Write DataFrame to database
df.to_sql('sales_table', engine, if_exists='replace', index=False)

# Read from database
df = pd.read_sql('SELECT * FROM sales_table', engine)
```

---

<a id="q32"></a>
### 🔷 Q32. What is ETL in Data Engineering?

> **ETL = Extract, Transform, Load**
>
> **Extract:**
> - Pull data from source systems
> - Sources: databases, REST APIs, CSV files, cloud storage (S3), Kafka streams
> - Handle different formats: JSON, CSV, XML, Parquet
> - Handle authentication, rate limits and pagination for APIs
>
> **Transform:**
> - Clean data — remove nulls, fix data types, remove duplicates
> - Filter — keep only relevant rows
> - Aggregate — group by, sum, average
> - Enrich — add calculated columns, join with reference data
> - Standardize — consistent date formats, casing, encoding
> - Validate — check business rules, data quality checks
>
> **Load:**
> - Write data to destination — data warehouse, database, data lake
> - Strategies: Full load (replace all), Incremental load (only new/changed records)
> - Handle errors — log failures, retry failed records
> - Notify on success/failure

```python
# Simple ETL example in Python
import pandas as pd

# EXTRACT
df = pd.read_csv("raw_sales.csv")

# TRANSFORM
df.dropna(inplace=True)
df['profit_margin'] = df['Profit'] / df['Sales']
df['date'] = pd.to_datetime(df['date'])

# LOAD
df.to_csv("clean_sales.csv", index=False)
```

---

<a id="q33"></a>
### 🔷 Q33. How do you read and write JSON files in Python?

```python
import json

# Read JSON file
with open("data.json", "r") as f:
    data = json.load(f)

# Write JSON file
with open("output.json", "w") as f:
    json.dump(data, f, indent=4)

# Convert Python dict to JSON string
json_str = json.dumps({"name": "Raj", "age": 23})

# Convert JSON string to Python dict
data = json.loads(json_str)
```

---

<a id="q34"></a>
### 🔷 Q34. How do you work with dates in Python?

```python
from datetime import datetime, timedelta

# Current date and time
now = datetime.now()
print(now)  # 2025-05-20 10:30:00

# Format date
print(now.strftime("%Y-%m-%d"))  # 2025-05-20

# Add/subtract days
yesterday = now - timedelta(days=1)
next_week = now + timedelta(days=7)

# Convert string to datetime
dt = datetime.strptime("2025-01-15", "%Y-%m-%d")
```

---

<a id="q35"></a>
### 🔷 Q35. What is the difference between `map()`, `filter()` and `reduce()`?

```python
numbers = [1, 2, 3, 4, 5]

# map() — applies function to each element
squares = list(map(lambda x: x**2, numbers))
# [1, 4, 9, 16, 25]

# filter() — keeps elements where condition is True
evens = list(filter(lambda x: x % 2 == 0, numbers))
# [2, 4]

# reduce() — combines all elements into single value
from functools import reduce
total = reduce(lambda x, y: x + y, numbers)
# 15
```

---

<a id="q36"></a>
### 🔷 Q36. How do you remove duplicates from a list?

```python
data = [1, 2, 2, 3, 3, 4]

# Using set
unique = list(set(data))

# Using Pandas
df.drop_duplicates(inplace=True)

# Check duplicates in DataFrame
df.duplicated().sum()
```

---

<a id="q37"></a>
### 🔷 Q37. How do you sort a list of dictionaries in Python?

```python
employees = [
    {"name": "Raj", "salary": 50000},
    {"name": "Amit", "salary": 70000},
    {"name": "Priya", "salary": 60000}
]

# Sort by salary ascending
sorted_emp = sorted(employees, key=lambda x: x['salary'])

# Sort by salary descending
sorted_emp = sorted(employees, key=lambda x: x['salary'], reverse=True)
```

---

<a id="q38"></a>
### 🔷 Q38. What is Multithreading vs Multiprocessing in Python?

| | Multithreading | Multiprocessing |
|---|---|---|
| What | Multiple threads in same process | Multiple separate processes |
| Use case | I/O tasks (file read, API calls, DB queries) | CPU-heavy tasks (data processing) |
| Memory | Shared memory | Separate memory |
| Library | `threading` | `multiprocessing` |

> **Data Engineering use:** Use multithreading to fetch data from multiple APIs simultaneously.

---

<a id="q38b"></a>
### 🔷 Q38B. What is the difference between Compiled and Interpreted languages?

> - **Compiled** (C, C++) — code is converted to machine code before running. Faster execution.
> - **Interpreted** (Python) — code is executed line by line at runtime. Slower but easier to write and debug.
>
> Python is interpreted — that is why it is slower than C++ for heavy computation, but much easier for data pipelines and scripting.

---

<a id="q38c"></a>
### 🔷 Q38C. What is Lazy Evaluation in Python?

> Lazy evaluation means the value is computed only when it is actually needed — not immediately.
>
> - **Generators** use lazy evaluation — values are yielded one at a time, not all at once
> - **Range object** is lazy — `range(1000000)` does not create a list of 1 million numbers in memory
>
> Very useful in Data Engineering when dealing with large datasets.

```python
# Eager (loads everything at once) — bad for large data
data = [row for row in open("big_file.csv")]  # loads all into memory

# Lazy (one at a time) — memory efficient
def read_rows(file):
    with open(file) as f:
        for row in f:
            yield row
```

---

<a id="q38d"></a>
### 🔷 Q38D. What is a Context Manager and how to create one?

> A Context Manager handles setup and cleanup of resources automatically — using `with` statement.

```python
# Built-in context manager
with open("file.txt") as f:
    data = f.read()
# File is automatically closed here

# Custom context manager using class
class DatabaseConnection:
    def __enter__(self):
        self.conn = connect_to_db()
        return self.conn

    def __exit__(self, exc_type, exc_val, exc_tb):
        self.conn.close()

with DatabaseConnection() as conn:
    conn.execute("SELECT * FROM sales")
```

> Very common in Data Engineering — wrapping database connections, file operations and transactions.

---

## 🔴 SECTION 4: DATA PIPELINES & ADVANCED (Q39–Q50)

---

<a id="q39"></a>
### 🔷 Q39. What is Apache Kafka and how does Python connect to it?

> Kafka is a distributed messaging system used for real-time data streaming.
> - **Producer** — sends messages to a topic
> - **Consumer** — reads messages from a topic

```python
from kafka import KafkaProducer, KafkaConsumer
import json

# Producer — send data
producer = KafkaProducer(
    bootstrap_servers='localhost:9092',
    value_serializer=lambda v: json.dumps(v).encode('utf-8')
)
producer.send('sales-topic', {"product": "TV", "price": 50000})

# Consumer — receive data
consumer = KafkaConsumer(
    'sales-topic',
    bootstrap_servers='localhost:9092',
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))
)
for message in consumer:
    print(message.value)
```

---

<a id="q40"></a>
### 🔷 Q40. What is RabbitMQ and how is it different from Kafka?

| | RabbitMQ | Kafka |
|---|---|---|
| Type | Message Queue | Event Streaming Platform |
| Data retention | Deleted after consumed | Retained for configured time |
| Use case | Task queues, job processing | Real-time analytics, log streaming |
| Speed | Moderate | Very high throughput |
| Order | Not guaranteed | Guaranteed within partition |

---

<a id="q41"></a>
### 🔷 Q41. What is Celery in Python?

> Celery is a distributed task queue library. It allows you to run tasks asynchronously in the background.
>
> Example use case: After a user uploads a file, process it in the background without blocking the main application.

```python
from celery import Celery

app = Celery('tasks', broker='redis://localhost:6379/0')

@app.task
def process_data(file_path):
    # do heavy processing
    return "Done"

# Call task asynchronously
process_data.delay("data.csv")
```

---

<a id="q42"></a>
### 🔷 Q42. What is Redis and how is it used in Data Engineering?

> Redis is an in-memory key-value database used for:
> - **Caching** — store frequently accessed data to avoid repeated DB queries
> - **Session storage** — store temporary user session data
> - **Message broker** — used with Celery for task queues

```python
import redis

r = redis.Redis(host='localhost', port=6379, db=0)

# Set a value
r.set('user:1', 'Rajkishor')

# Get a value
name = r.get('user:1')
print(name)  # b'Rajkishor'

# Set with expiry (cache for 60 seconds)
r.setex('user:1', 60, 'Rajkishor')
```

---

<a id="q43"></a>
### 🔷 Q43. What is MongoDB and how to use it with Python?

> MongoDB is a NoSQL database that stores data as JSON-like documents instead of tables.

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client["mydb"]
collection = db["sales"]

# Insert document
collection.insert_one({"product": "TV", "price": 50000, "region": "East"})

# Find documents
results = collection.find({"region": "East"})
for doc in results:
    print(doc)

# Find one
doc = collection.find_one({"product": "TV"})
```

---

<a id="q44"></a>
### 🔷 Q44. What is Docker and why is it used in Data Engineering?

> Docker is a tool that packages your application and all its dependencies into a **container** that runs the same everywhere — on your laptop, server or cloud.
>
> Data Engineering use:
> - Run PostgreSQL, Kafka, Redis locally without installing them
> - Package your Python pipeline into a container for deployment

```bash
# Run PostgreSQL in Docker
docker run -d -e POSTGRES_PASSWORD=pass -p 5432:5432 postgres

# Run your Python app in Docker
docker build -t my-pipeline .
docker run my-pipeline
```

---

<a id="q45"></a>
### 🔷 Q45. What is a virtual environment in Python and why use it?

> A virtual environment creates an isolated Python environment for each project so that different projects can have different library versions without conflict.

```bash
# Create virtual environment
python -m venv myenv

# Activate (Linux/Mac)
source myenv/bin/activate

# Activate (Windows)
myenv\Scripts\activate

# Install libraries
pip install pandas psycopg2

# Save dependencies
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt
```

---

<a id="q46"></a>
### 🔷 Q46. How do you schedule a Python script to run automatically?

> - **Linux/Mac** — use `cron` jobs
> - **Windows** — use Task Scheduler
> - **Python library** — use `schedule` or `APScheduler`

```python
import schedule
import time

def run_pipeline():
    print("Running data pipeline...")

# Run every day at 8 AM
schedule.every().day.at("08:00").do(run_pipeline)

while True:
    schedule.run_pending()
    time.sleep(1)
```

---

<a id="q47"></a>
### 🔷 Q47. How do you make an API call in Python?

```python
import requests

# GET request
response = requests.get("https://api.example.com/sales")

if response.status_code == 200:
    data = response.json()
    print(data)
else:
    print(f"Error: {response.status_code}")

# POST request
payload = {"user": "Raj", "amount": 5000}
response = requests.post("https://api.example.com/create", json=payload)
```

---

<a id="q48"></a>
### 🔷 Q48. How do you write a Python script to insert bulk data into PostgreSQL efficiently?

```python
import psycopg2
import pandas as pd

df = pd.read_csv("sales.csv")
conn = psycopg2.connect(host="localhost", database="mydb",
                         user="postgres", password="pass")
cursor = conn.cursor()

# Bulk insert using executemany
records = df.values.tolist()
cursor.executemany(
    "INSERT INTO sales (product, region, sales) VALUES (%s, %s, %s)",
    records
)

conn.commit()
cursor.close()
conn.close()
print("Data inserted successfully!")
```

---

<a id="q49"></a>
### 🔷 Q49. What is Data Versioning and how do you handle it?

> Data Versioning means tracking changes to datasets over time so you can:
> - Reproduce past results
> - Roll back to previous data state
> - Audit who changed what and when
>
> Common approaches:
> - Add `created_at` and `updated_at` timestamps to tables
> - Maintain a separate history/audit table
> - Use tools like DVC (Data Version Control) for large datasets
> - Soft delete: add `is_deleted` flag instead of actually deleting rows

```python
# Example: Adding version tracking in PostgreSQL table
CREATE TABLE sales (
    id SERIAL PRIMARY KEY,
    product VARCHAR,
    price FLOAT,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW(),
    is_deleted BOOLEAN DEFAULT FALSE
);
```

---

<a id="q50"></a>
### 🔷 Q50. What is the difference between `iloc` and `loc` in Pandas?

```python
df = pd.DataFrame({
    'name': ['Raj', 'Amit', 'Priya'],
    'sales': [100, 200, 150]
}, index=[10, 20, 30])

# loc — label-based (use actual index labels)
print(df.loc[10])           # row with index label 10
print(df.loc[10, 'sales'])  # specific value

# iloc — integer position-based (0, 1, 2...)
print(df.iloc[0])           # first row
print(df.iloc[0, 1])        # first row, second column
```

---

## 🔵 SECTION 5: JSW SPECIFIC + EXTRA IMPORTANT (Q51–Q65)

---

<a id="q51"></a>
### 🔷 Q51. Tell me about yourself and your projects. (Most asked at JSW)

> JSW me pehla question yahi hota hai — CV se directly poochha jata hai.
> - Apna introduction do (already prepared hai)
> - DataWeave ka DSA platform explain karo
> - Elevest me data pipeline explain karo
> - Skills confidently bolo: Python, SQL, PostgreSQL, Kafka, Docker

---

<a id="q52"></a>
### 🔷 Q52. What is the difference between ETL and ELT?

| | ETL | ELT |
|---|---|---|
| Full form | Extract Transform Load | Extract Load Transform |
| Transform | Before loading into warehouse | After loading into warehouse |
| Use case | Traditional data warehouses | Modern cloud warehouses (BigQuery, Snowflake) |
| Speed | Slower for large data | Faster with cloud power |
| Example | Python pipeline → clean → PostgreSQL | Raw data → S3 → transform inside Redshift |

---

<a id="q53"></a>
### 🔷 Q53. What is Data Pipeline? How did you build one?

> A Data Pipeline is a series of automated steps that moves data from source to destination — collecting, processing and storing it.
>
> Your answer (from your experience):
> *"At Elevest Technologies, I built a pipeline that scraped real-time market data, pre-processed it using Pandas, ran backtesting logic using Backtrader, and stored results for analysis. At DataWeave, I worked on scheduling and publishing data pipelines that tracked product KPIs across eCommerce channels."*

---

<a id="q54"></a>
### 🔷 Q54. What is Data Quality and how do you ensure it?

> Data Quality means data is accurate, complete, consistent and timely.
>
> Ways to ensure it:
> - Check for null/missing values → `df.isnull().sum()`
> - Remove duplicates → `df.drop_duplicates()`
> - Validate data types → `df.dtypes`
> - Check value ranges (no negative prices)
> - Write SQL validation queries
> - Add logs and alerts when data quality checks fail

---

<a id="q55"></a>
### 🔷 Q55. What is the difference between OLTP and OLAP?

| | OLTP | OLAP |
|---|---|---|
| Full form | Online Transaction Processing | Online Analytical Processing |
| Purpose | Day-to-day transactions | Data analysis and reporting |
| Operations | INSERT, UPDATE, DELETE | SELECT, aggregations |
| Data volume | Small transactions | Large historical data |
| Example | Bank transactions, order placing | Sales reports, dashboards |
| Database | PostgreSQL, MySQL | Redshift, BigQuery, Snowflake |

---

<a id="q56"></a>
### 🔷 Q56. What is Idempotency in Data Engineering?

> Idempotency means running the same pipeline multiple times gives the same result — no duplicate data, no errors.
>
> Example: If your pipeline runs twice due to a failure, the data should not be inserted twice.
>
> How to achieve it:
> - Use `INSERT ... ON CONFLICT DO NOTHING` in PostgreSQL
> - Use `UPSERT` (update if exists, insert if not)
> - Add unique constraints on tables
> - Use `if_exists='replace'` in Pandas `to_sql()`

---

<a id="q57"></a>
### 🔷 Q57. How do you handle large files in Python without loading into memory?

```python
import pandas as pd

# Read CSV in chunks — does not load entire file at once
for chunk in pd.read_csv("large_file.csv", chunksize=10000):
    # process each chunk
    process(chunk)

# Using generator for line by line reading
def read_large_file(file_path):
    with open(file_path, 'r') as f:
        for line in f:
            yield line.strip()
```

> This is very important for Data Engineering — always mention this in interviews.

---

<a id="q58"></a>
### 🔷 Q58. What is Data Normalization? Explain 1NF, 2NF, 3NF.

> Normalization organizes a database to reduce redundancy and improve data integrity.

> - **1NF** — Each column has atomic (single) values, no repeating groups
> - **2NF** — 1NF + every non-key column depends on the FULL primary key
> - **3NF** — 2NF + no transitive dependency (non-key column should not depend on another non-key column)

---

<a id="q59"></a>
### 🔷 Q59. What is an Index in a database and why is it important?

> An Index is a data structure that speeds up data retrieval in a database — like an index in a book.
>
> Without index → full table scan (slow)
> With index → direct lookup (fast)

```sql
-- Create index on frequently queried column
CREATE INDEX idx_sales_region ON sales(region);

-- Now this query is much faster
SELECT * FROM sales WHERE region = 'East';
```

> Use indexes on columns used in WHERE, JOIN and ORDER BY clauses.

---

<a id="q60"></a>
### 🔷 Q60. What is the difference between `DELETE`, `TRUNCATE` and `DROP` in SQL?

| | DELETE | TRUNCATE | DROP |
|---|---|---|---|
| What | Removes specific rows | Removes all rows | Removes entire table |
| WHERE clause | ✅ Yes | ❌ No | ❌ No |
| Rollback | ✅ Possible | ❌ Not possible | ❌ Not possible |
| Speed | Slow (logs each row) | Fast | Fast |

---

<a id="q61"></a>
### 🔷 Q61. What are Window Functions in SQL? (Very Important)

> Window functions perform calculations across a set of rows related to the current row — without collapsing rows like GROUP BY.

```sql
-- RANK — rank sales by region
SELECT name, region, sales,
       RANK() OVER (PARTITION BY region ORDER BY sales DESC) as rank
FROM sales_table;

-- Running total
SELECT date, sales,
       SUM(sales) OVER (ORDER BY date) as running_total
FROM sales_table;

-- Previous row value
SELECT date, sales,
       LAG(sales, 1) OVER (ORDER BY date) as prev_sales
FROM sales_table;
```

---

<a id="q62"></a>
### 🔷 Q62. What is a CTE (Common Table Expression) in SQL?

> A CTE is a temporary named result set that you can reference within a query — makes complex queries readable.

```sql
WITH top_customers AS (
    SELECT customer_id, SUM(sales) as total_sales
    FROM orders
    GROUP BY customer_id
    HAVING SUM(sales) > 10000
)
SELECT c.name, t.total_sales
FROM customers c
JOIN top_customers t ON c.id = t.customer_id;
```

---

<a id="q63"></a>
### 🔷 Q63. How do you debug a failing data pipeline?

> Step by step approach:
> 1. **Check logs** — what error message is shown
> 2. **Check data source** — is input data available and correct format
> 3. **Check individual steps** — run each step separately
> 4. **Check database** — connection issues, permissions
> 5. **Check data quality** — null values, wrong data types
> 6. **Add print/logging statements** to find exact failure point
>
> From your experience: *"At DataWeave, I investigated data versioning and scheduling issues by checking pipeline logs, validating PostgreSQL queries, and testing each stage independently."*

---

<a id="q64"></a>
### 🔷 Q64. What is the difference between Synchronous and Asynchronous processing?

| | Synchronous | Asynchronous |
|---|---|---|
| How | One task completes before next starts | Tasks run independently, don't wait |
| Speed | Slower for multiple tasks | Faster, parallel execution |
| Example | Reading a file line by line | Celery background tasks, Kafka consumers |
| Use in DE | Simple sequential pipelines | High-volume real-time data processing |

---

<a id="q65"></a>
### 🔷 Q65. Behavioral Questions — JSW will definitely ask these

> **"Why do you want to join JSW?"**
> *"JSW operates at a massive scale — managing data across steel, energy and infrastructure sectors. I want to work on data pipelines that have real business impact at this scale. My experience at DataWeave with large-scale eCommerce analytics aligns well with JSW's data-driven operations."*

> **"What was the most challenging problem you solved?"**
> *Prepare one story from DataWeave or Elevest — a real data issue you investigated and fixed.*

> **"Where do you see yourself in 3 years?"**
> *"I want to grow into a Senior Data Engineer role, designing scalable data architectures and working on real-time streaming pipelines."*

> **"What are your strengths?"**
> *"Problem solving with data, writing optimized SQL queries, and building reliable Python pipelines."*

---

## 💡 FINAL QUICK REVISION

**Python Basics:**
> ✅ List = ordered + changeable | Tuple = ordered + fixed | Set = unique | Dict = key-value
> ✅ `*args` = multiple positional args | `**kwargs` = multiple keyword args
> ✅ Generator uses `yield` — saves memory for large data
> ✅ Always use `with open()` for file handling — auto closes file
> ✅ Use `try/except/finally` for error handling in pipelines

**Pandas:**
> ✅ `merge()` = JOIN on column | `concat()` = UNION stack rows
> ✅ `groupby()` in Pandas = GROUP BY in SQL
> ✅ `loc` = label-based | `iloc` = position-based
> ✅ `fillna()` = fill nulls | `dropna()` = drop nulls | `isnull()` = check nulls

**Data Engineering:**
> ✅ ETL = Extract → Transform → Load | ELT = Extract → Load → Transform
> ✅ OLTP = transactions (INSERT/UPDATE) | OLAP = analytics (SELECT/aggregation)
> ✅ Idempotency = running pipeline multiple times gives same result, no duplicates
> ✅ Index = speeds up database queries (use on WHERE and JOIN columns)

**Tools:**
> ✅ Kafka = real-time streaming | RabbitMQ = task queue | Celery = async tasks
> ✅ Redis = in-memory cache | MongoDB = NoSQL document store
> ✅ Docker = containerize your app | Virtual env = isolate Python dependencies

**SQL:**
> ✅ Window functions: RANK(), DENSE_RANK(), ROW_NUMBER(), LAG(), SUM() OVER()
> ✅ CTE = WITH clause — temporary named result set for clean queries
> ✅ DELETE = specific rows | TRUNCATE = all rows | DROP = entire table
> ✅ 1NF → 2NF → 3NF = normalization levels to reduce redundancy

**JSW Interview Tips:**
> ✅ Pehla question = "Tell me about yourself" — introduction ready rakho
> ✅ DataWeave experience confidently explain karo — pipeline, PostgreSQL, versioning
> ✅ Elevest experience — data pipeline, scraping, backtesting explain karo
> ✅ "Why JSW?" ka answer ready rakho

---

*All the best Rajkishor! Python + Tableau + SQL = JSW Data Engineer ready! 🎯*
