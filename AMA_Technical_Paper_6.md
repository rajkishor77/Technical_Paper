# AMA Technical Paper 6

### **1. What is an IP address?**

* An **IP address (Internet Protocol address)** is a unique identifier assigned to every device connected to a network, allowing it to communicate with other devices.
* There are two main types: **IPv4** (e.g., `192.168.1.1`) and **IPv6** (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`). IPv4 uses 32-bit numbers, while IPv6 uses 128-bit numbers to allow more devices.
* IP addresses can be **public** (accessible over the internet) or **private** (used within local networks).
* They work like postal addresses, helping route data packets to the correct device.
* Example: When you type `google.com`, DNS converts the domain into an IP like `142.250.190.14` so your browser can connect.

---

### **2. What is the purpose of the render() function in Django?**

* In Django, `render()` is a **shortcut function** that combines a template with context data and returns an **HTTP response** to the client.
* Its basic syntax:

```python
render(request, template_name, context)
```

* **request**: the incoming HTTP request object.
* **template_name**: the HTML file to render (e.g., `'home.html'`).
* **context**: a Python dictionary containing data to display dynamically (e.g., `{'name': 'Raj'}`).
* Example usage:

```python
def home(request):
    return render(request, 'home.html', {'username': 'Raj'})
```

* This makes displaying dynamic content on web pages simple and integrates data from models seamlessly.

---

### **3. What are models in Django?**

* **Models** are Python classes that define the structure and behavior of your database tables.
* Each class represents a **table**, and each attribute represents a **column/field**.
* Defined in `models.py`, models handle data abstraction, letting you work with Python objects instead of raw SQL.
* Django automatically generates the SQL required to create tables via **migrations**.
* Example:

```python
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    email = models.EmailField()
```

* Using this model, Django can create a `Student` table with `name`, `age`, and `email` columns.

---

### **4. What are views in Django?**

* **Views** are Python functions or classes that **process requests** and **return responses** to the user.
* They act as the bridge between models (data) and templates (UI).
* Views handle business logic like fetching data from the database, processing it, and sending it to templates.
* Example of a simple function-based view:

```python
from django.shortcuts import render
from .models import Student

def student_list(request):
    students = Student.objects.all()
    return render(request, 'students.html', {'students': students})
```

* Django also supports **class-based views** (CBVs) for more complex and reusable logic.

---

### **5. What are templates in Django?**

* Templates are **HTML files with placeholders and template tags** that allow dynamic content rendering.
* Stored in the `templates/` folder, they use **template syntax** to include variables and control structures:

```html
<h1>Welcome, {{ username }}</h1>
{% for student in students %}
  <p>{{ student.name }}</p>
{% endfor %}
```

* Templates keep presentation separate from business logic, following Django’s **MTV (Model-Template-View)** architecture.
* They can include loops, conditions, and filters for formatting data without modifying Python code.

---

### **6. What are static files in Django?**

* **Static files** are non-dynamic assets like CSS, JavaScript, and images that enhance the appearance and interactivity of a web app.
* Stored typically in a `static/` directory inside an app or project.
* Django settings:

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / "static"]
```

* In templates, they are accessed using `{% load static %}` and `{% static 'css/style.css' %}`.
* Static files are different from **media files**, which are uploaded by users.

---

### **7. How do you create a superuser in Django?**

* A **superuser** is an admin account with full access to Django’s admin interface.
* To create one, use the terminal command:

```bash
python manage.py createsuperuser
```

* You will be prompted to enter a **username**, **email**, and **password**.
* After creation, you can log in at `http://localhost:8000/admin` to manage models, users, and other data.
* Superusers are essential for managing the site during development or production.

---


