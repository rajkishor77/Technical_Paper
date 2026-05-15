# 🐍 Django — Complete Study Notes
> **After reading this, no concept of Django will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is Django?](#1-what-is-django)
2. [Django Project Structure](#2-django-project-structure)
3. [Settings File — Deep Dive](#3-settings-file--deep-dive)
4. [URLs & Routing](#4-urls--routing)
5. [Views — FBV & CBV](#5-views--fbv--cbv)
6. [Templates](#6-templates)
7. [Models & Fields](#7-models--fields)
8. [ORM — Complete Query Reference](#8-orm--complete-query-reference)
9. [Aggregations & Annotations](#9-aggregations--annotations)
10. [Migrations](#10-migrations)
11. [Django Admin](#11-django-admin)
12. [Forms & Validation](#12-forms--validation)
13. [Authentication & Authorization](#13-authentication--authorization)
14. [Middleware](#14-middleware)
15. [Static & Media Files](#15-static--media-files)
16. [Django REST Framework (DRF)](#16-django-rest-framework-drf)
17. [Signals](#17-signals)
18. [Caching](#18-caching)
19. [Transactions](#19-transactions)
20. [Security](#20-security)
21. [WSGI & ASGI](#21-wsgi--asgi)
22. [Complete manage.py Commands Reference](#22-complete-managepy-commands-reference)
23. [Django with PostgreSQL](#23-django-with-postgresql)
24. [Best Practices](#24-best-practices)
25. [Common Interview Questions](#25-common-interview-questions)

---

## 1. What is Django?

Django is a **high-level, open-source Python web framework** that encourages rapid development and clean, pragmatic design. It follows the **MVT (Model-View-Template)** architectural pattern.

### Simple Analogy 🏗️
> Think of Django as a **Pre-built Office Building**.
> - **Model** = The basement (foundation) — stores and manages all data
> - **View** = The employees — they process requests and decide what response to send
> - **Template** = The interior design — the HTML pages users actually see
> - **URL Dispatcher** = The receptionist — routes incoming visitors to the right department
> - **ORM** = The filing clerk — lets you talk to the database without knowing SQL
> - **Middleware** = Security guards at every door — check requests before they reach employees
> - **settings.py** = The building blueprint — controls everything about how the building runs
> - **manage.py** = The master control panel — run commands to manage the whole building

### Key Points
- Created by **Adrian Holovaty and Simon Willison** in 2003 at the Lawrence Journal-World newspaper
- Open-sourced in **2005** — now maintained by the Django Software Foundation
- Written in **Python** — follows Python's "batteries included" philosophy
- Motto: **"The web framework for perfectionists with deadlines"**
- Current stable version: **Django 5.x**
- Used by **Instagram, Pinterest, Disqus, Mozilla, NASA, The Washington Post**
- Follows **DRY (Don't Repeat Yourself)** principle throughout

### Django MVT vs MVC

| Layer | MVC | Django MVT | Role |
|-------|-----|-----------|------|
| Data layer | Model | **Model** | Database tables, business logic |
| Logic layer | Controller | **View** | Processes request, calls model, returns response |
| Presentation layer | View | **Template** | HTML rendered and sent to the browser |

> In Django, the **URL dispatcher** plays the role of the Controller — it decides which View handles a request.

---

## 2. Django Project Structure

### Creating a Project and App

```bash
# Install Django
pip install django

# Create a new project
django-admin startproject myproject

# Navigate into project
cd myproject

# Create an app inside the project
python manage.py startapp myapp
```

### Full Project Structure Explained

```
myproject/                      ← Root folder (your project directory)
│
├── manage.py                   ← CLI tool for running commands (runserver, migrate, etc.)
│
├── myproject/                  ← The main project package
│   ├── __init__.py             ← Makes this a Python package
│   ├── settings.py             ← All project configuration (database, apps, security...)
│   ├── urls.py                 ← Root URL configuration — maps URLs to views
│   ├── wsgi.py                 ← Entry point for WSGI servers (Gunicorn, Apache)
│   └── asgi.py                 ← Entry point for ASGI servers (Daphne, Uvicorn) — async
│
└── myapp/                      ← Your custom application
    ├── __init__.py
    ├── admin.py                ← Register models to appear in Django Admin
    ├── apps.py                 ← App configuration (name, signals, etc.)
    ├── models.py               ← Database table definitions (the "M" in MVT)
    ├── views.py                ← Request handling logic (the "V" in MVT)
    ├── urls.py                 ← App-level URL patterns (you create this file)
    ├── forms.py                ← Django Form classes (you create this file)
    ├── serializers.py          ← DRF serializers for API (you create this file)
    ├── tests.py                ← Unit and integration tests
    ├── migrations/             ← Auto-generated database migration files
    │   └── 0001_initial.py
    └── templates/              ← HTML template files (you create this folder)
        └── myapp/
            └── index.html
```

---

## 3. Settings File — Deep Dive

The `settings.py` file is the **central configuration file** for your entire Django project. Every major behavior is controlled from here.

### Core Settings Explained

```python
# ─── Security ─────────────────────────────────────────────────────────────
SECRET_KEY = 'django-insecure-your-secret-key-here'
# Used for: CSRF tokens, session signing, password hashing, signed cookies
# NEVER commit this to Git in production — use environment variables

DEBUG = True
# True → show detailed error pages (development only)
# False → show generic error pages (ALWAYS False in production)

ALLOWED_HOSTS = ['localhost', '127.0.0.1', 'yourdomain.com']
# Which hostnames can serve this app
# In production: ['yourdomain.com', 'www.yourdomain.com']


# ─── Installed Apps ───────────────────────────────────────────────────────
INSTALLED_APPS = [
    # Django built-in apps
    'django.contrib.admin',         # /admin/ — the admin dashboard
    'django.contrib.auth',          # User authentication system
    'django.contrib.contenttypes',  # Content type framework (used by permissions)
    'django.contrib.sessions',      # Session management
    'django.contrib.messages',      # One-time flash messages
    'django.contrib.staticfiles',   # Serves CSS, JS, images in development

    # Third-party apps
    'rest_framework',               # Django REST Framework for APIs
    'corsheaders',                  # Handle Cross-Origin requests
    'django_extensions',            # Extra management commands

    # Your custom apps
    'myapp',
]


# ─── Database ─────────────────────────────────────────────────────────────
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',   # SQLite (development)
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}

# PostgreSQL (production):
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydb',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}


# ─── Templates ────────────────────────────────────────────────────────────
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],   # Global templates directory
        'APP_DIRS': True,                   # Also look inside each app's templates/ folder
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]


# ─── Static & Media Files ─────────────────────────────────────────────────
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']   # Dev — where your static files live
STATIC_ROOT = BASE_DIR / 'staticfiles'     # Production — collectstatic puts files here

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'            # Where uploaded files are stored


# ─── Authentication ────────────────────────────────────────────────────────
LOGIN_URL = '/login/'
LOGIN_REDIRECT_URL = '/dashboard/'
LOGOUT_REDIRECT_URL = '/'
AUTH_PASSWORD_VALIDATORS = [...]           # Rules for password strength


# ─── Email ────────────────────────────────────────────────────────────────
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_PORT = 587
EMAIL_USE_TLS = True
EMAIL_HOST_USER = 'yourapp@gmail.com'
EMAIL_HOST_PASSWORD = 'your-app-password'


# ─── Internationalization ─────────────────────────────────────────────────
LANGUAGE_CODE = 'en-us'
TIME_ZONE = 'Asia/Kolkata'   # Use your timezone
USE_I18N = True              # Enable translations
USE_TZ = True                # Store datetimes as UTC in the database
```

### Multiple Settings Files (Dev / Prod)

```
myproject/
├── settings/
│   ├── __init__.py
│   ├── base.py          ← Common settings shared across all environments
│   ├── development.py   ← DEBUG=True, SQLite, email console backend
│   └── production.py    ← DEBUG=False, PostgreSQL, real email, S3 storage
```

```bash
# Run with specific settings
python manage.py runserver --settings=myproject.settings.development
python manage.py migrate --settings=myproject.settings.production

# Or set as environment variable
export DJANGO_SETTINGS_MODULE=myproject.settings.production
```

---

## 4. URLs & Routing

### How URL Routing Works

```
Browser: GET /products/42/
        ↓
myproject/urls.py  (root URL conf)
        ↓
myapp/urls.py      (app URL conf — included via include())
        ↓
views.product_detail(request, pk=42)
        ↓
HttpResponse → Browser
```

### Root URLs (`myproject/urls.py`)

```python
from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),

    # Include app-level URLs — clean separation
    path('',         include('myapp.urls')),
    path('api/',     include('myapp.api_urls')),
    path('accounts/', include('django.contrib.auth.urls')),  # built-in auth views
]

# Serve media files in development
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### App-level URLs (`myapp/urls.py`)

```python
from django.urls import path, re_path
from . import views

app_name = 'myapp'  # Namespace — allows {% url 'myapp:product_list' %}

urlpatterns = [
    # Simple path
    path('',                    views.index,           name='index'),

    # Path with integer parameter
    path('products/',           views.product_list,    name='product_list'),
    path('products/<int:pk>/',  views.product_detail,  name='product_detail'),

    # Path with string parameter
    path('category/<str:slug>/', views.category,       name='category'),

    # Path with slug parameter
    path('posts/<slug:slug>/',  views.post_detail,     name='post_detail'),

    # Path with UUID parameter
    path('order/<uuid:order_id>/', views.order_detail, name='order_detail'),

    # Regular expression (advanced patterns)
    re_path(r'^archive/(?P<year>[0-9]{4})/$', views.archive, name='archive'),
]
```

### URL Path Converters

| Converter | Matches | Example |
|-----------|---------|---------|
| `<int:pk>` | Zero or positive integer | `products/42/` |
| `<str:name>` | Any non-empty string (no `/`) | `users/alice/` |
| `<slug:slug>` | Letters, numbers, hyphens, underscores | `posts/my-first-post/` |
| `<uuid:id>` | UUID format | `orders/550e8400-e29b-41d4-a716/` |
| `<path:file>` | Any string including `/` | `files/docs/report.pdf` |

---

## 5. Views — FBV & CBV

### 5.1 Function-Based Views (FBV)

```python
from django.shortcuts import render, get_object_or_404, redirect
from django.http import HttpResponse, JsonResponse
from django.contrib.auth.decorators import login_required
from .models import Product

# Simple view
def index(request):
    return HttpResponse("Hello, World!")

# Render a template with context
def product_list(request):
    products = Product.objects.all().order_by('-created_at')
    context = {'products': products, 'title': 'All Products'}
    return render(request, 'myapp/product_list.html', context)

# Get single object or return 404
def product_detail(request, pk):
    product = get_object_or_404(Product, pk=pk)
    return render(request, 'myapp/product_detail.html', {'product': product})

# Handle GET and POST in same view
def create_product(request):
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)
        if form.is_valid():
            form.save()
            return redirect('myapp:product_list')
    else:
        form = ProductForm()
    return render(request, 'myapp/create_product.html', {'form': form})

# JSON response (for simple APIs)
def api_products(request):
    products = list(Product.objects.values('id', 'name', 'price'))
    return JsonResponse({'products': products})

# Restrict to logged-in users only
@login_required
def dashboard(request):
    return render(request, 'myapp/dashboard.html')
```

### 5.2 Class-Based Views (CBV)

```python
from django.views import View
from django.views.generic import (
    ListView, DetailView, CreateView, UpdateView, DeleteView, TemplateView
)
from django.contrib.auth.mixins import LoginRequiredMixin
from django.urls import reverse_lazy
from .models import Product
from .forms import ProductForm

# ─── Generic List View ────────────────────────────────────────────────────
class ProductListView(ListView):
    model = Product
    template_name = 'myapp/product_list.html'
    context_object_name = 'products'   # variable name in template
    ordering = ['-created_at']
    paginate_by = 10                   # automatic pagination!

    def get_queryset(self):            # customize the queryset
        return Product.objects.filter(is_active=True)

    def get_context_data(self, **kwargs):   # add extra context
        context = super().get_context_data(**kwargs)
        context['title'] = 'Products'
        return context


# ─── Generic Detail View ──────────────────────────────────────────────────
class ProductDetailView(DetailView):
    model = Product
    template_name = 'myapp/product_detail.html'
    context_object_name = 'product'


# ─── Generic Create View ──────────────────────────────────────────────────
class ProductCreateView(LoginRequiredMixin, CreateView):
    model = Product
    form_class = ProductForm
    template_name = 'myapp/product_form.html'
    success_url = reverse_lazy('myapp:product_list')

    def form_valid(self, form):        # called when form is valid
        form.instance.created_by = self.request.user
        return super().form_valid(form)


# ─── Generic Update View ──────────────────────────────────────────────────
class ProductUpdateView(LoginRequiredMixin, UpdateView):
    model = Product
    form_class = ProductForm
    template_name = 'myapp/product_form.html'
    success_url = reverse_lazy('myapp:product_list')


# ─── Generic Delete View ──────────────────────────────────────────────────
class ProductDeleteView(LoginRequiredMixin, DeleteView):
    model = Product
    template_name = 'myapp/product_confirm_delete.html'
    success_url = reverse_lazy('myapp:product_list')


# ─── Custom Class-Based View ──────────────────────────────────────────────
class CustomView(View):
    def get(self, request, *args, **kwargs):
        return render(request, 'myapp/custom.html')

    def post(self, request, *args, **kwargs):
        # handle POST
        return redirect('myapp:index')
```

### FBV vs CBV — When to Use What

| Use | **FBV** | **CBV** |
|-----|---------|---------|
| Simple, custom logic | ✅ Better | Overkill |
| Standard CRUD operations | More boilerplate | ✅ Much less code |
| Reusing logic across views | Mixins get messy | ✅ Clean inheritance |
| Readability for beginners | ✅ Easier to read | Needs Django knowledge |
| Complex conditional logic | ✅ Clearer | Can get hard to follow |

---

## 6. Templates

### Template Syntax

```html
<!-- myapp/templates/myapp/product_list.html -->

<!-- 1. VARIABLES — {{ variable_name }} -->
<h1>{{ title }}</h1>
<p>Welcome, {{ user.username }}!</p>
<p>Product: {{ product.name }}</p>

<!-- 2. FILTERS — {{ variable|filter }} -->
<p>{{ name|upper }}</p>                    <!-- UPPERCASE -->
<p>{{ name|lower }}</p>                    <!-- lowercase -->
<p>{{ name|title }}</p>                    <!-- Title Case -->
<p>{{ description|truncatewords:20 }}</p>  <!-- first 20 words -->
<p>{{ price|floatformat:2 }}</p>           <!-- 2 decimal places: 9.99 -->
<p>{{ date|date:"d M Y" }}</p>             <!-- 15 Jan 2024 -->
<p>{{ date|timesince }}</p>                <!-- "3 days ago" -->
<p>{{ text|linebreaks }}</p>               <!-- \n → <br> -->
<p>{{ html_content|safe }}</p>             <!-- render HTML (careful!) -->
<p>{{ items|length }}</p>                  <!-- list length -->
<p>{{ value|default:"N/A" }}</p>           <!-- fallback if empty -->

<!-- 3. TAGS — {% tag %} -->

<!-- FOR loop -->
{% for product in products %}
    <div class="product">
        <h2>{{ product.name }}</h2>
        <p>Price: ₹{{ product.price }}</p>
        <!-- Loop variables -->
        <small>Item {{ forloop.counter }} of {{ forloop.revcounter }}</small>
    </div>
{% empty %}
    <p>No products found.</p>     <!-- shown if list is empty -->
{% endfor %}

<!-- IF / ELIF / ELSE -->
{% if user.is_authenticated %}
    <a href="/logout/">Logout</a>
{% elif user.is_staff %}
    <a href="/admin/">Admin</a>
{% else %}
    <a href="/login/">Login</a>
{% endif %}

<!-- URL tag — generates URL by name -->
<a href="{% url 'myapp:product_detail' pk=product.pk %}">View Product</a>
<a href="{% url 'myapp:product_list' %}">All Products</a>

<!-- CSRF token — required in all POST forms -->
<form method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Submit</button>
</form>

<!-- Static files -->
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<img src="{% static 'images/logo.png' %}" alt="Logo">

<!-- Block tag — for template inheritance -->
{% block content %}
    Default content here
{% endblock content %}

<!-- Include another template -->
{% include 'myapp/navbar.html' %}
{% include 'myapp/card.html' with product=product %}

<!-- With tag — create a variable -->
{% with total=order.items.count %}
    <p>You have {{ total }} item{{ total|pluralize }}.</p>
{% endwith %}

<!-- Comment -->
{# This is a comment — not rendered in HTML #}
```

### Template Inheritance

```html
<!-- base.html — the parent template -->
<!DOCTYPE html>
<html>
<head>
    <title>{% block title %}My Site{% endblock %}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/style.css' %}">
</head>
<body>
    {% include 'navbar.html' %}

    <main>
        {% block content %}{% endblock %}
    </main>

    {% include 'footer.html' %}
    {% block extra_js %}{% endblock %}
</body>
</html>
```

```html
<!-- product_list.html — the child template -->
{% extends 'base.html' %}

{% block title %}Products — My Shop{% endblock %}

{% block content %}
<h1>All Products</h1>
{% for product in products %}
    <p>{{ product.name }} — ₹{{ product.price }}</p>
{% endfor %}
{% endblock %}

{% block extra_js %}
<script src="{% static 'js/products.js' %}"></script>
{% endblock %}
```

---

## 7. Models & Fields

### Defining Models

```python
from django.db import models
from django.contrib.auth.models import User
from django.core.validators import MinValueValidator, MaxValueValidator, RegexValidator


class Category(models.Model):
    name = models.CharField(max_length=100)
    slug = models.SlugField(unique=True)

    class Meta:
        verbose_name_plural = "Categories"
        ordering = ['name']

    def __str__(self):
        return self.name


class Product(models.Model):

    # ─── Text Fields ──────────────────────────────────────────────────────
    name        = models.CharField(max_length=200)          # Short text, required DB max length
    slug        = models.SlugField(unique=True)             # URL-friendly text
    description = models.TextField(blank=True)              # Long text, no length limit
    sku         = models.CharField(max_length=50, unique=True)

    # ─── Numeric Fields ───────────────────────────────────────────────────
    price       = models.DecimalField(max_digits=10, decimal_places=2,
                    validators=[MinValueValidator(0)])      # Use DecimalField for money!
    stock       = models.IntegerField(default=0)
    weight      = models.FloatField(null=True, blank=True)
    rating      = models.IntegerField(
                    validators=[MinValueValidator(1), MaxValueValidator(5)],
                    null=True, blank=True)

    # ─── Boolean Fields ───────────────────────────────────────────────────
    is_active   = models.BooleanField(default=True)
    is_featured = models.BooleanField(default=False)

    # ─── Date/Time Fields ─────────────────────────────────────────────────
    created_at  = models.DateTimeField(auto_now_add=True)  # Set ONCE on creation
    updated_at  = models.DateTimeField(auto_now=True)      # Updated on every save()
    published_at = models.DateField(null=True, blank=True)

    # ─── File/Image Fields ────────────────────────────────────────────────
    image       = models.ImageField(upload_to='products/', blank=True, null=True)
    # upload_to='products/' → file saved at MEDIA_ROOT/products/filename.jpg
    document    = models.FileField(upload_to='docs/', blank=True)

    # ─── Other Fields ─────────────────────────────────────────────────────
    email       = models.EmailField(blank=True)
    website     = models.URLField(blank=True)
    phone       = models.CharField(
                    max_length=15,
                    validators=[RegexValidator(r'^\+?1?\d{9,15}$')],
                    blank=True)

    # ─── Relationship Fields ──────────────────────────────────────────────
    category    = models.ForeignKey(
                    Category,
                    on_delete=models.SET_NULL,
                    null=True, blank=True,
                    related_name='products')  # category.products.all()

    created_by  = models.ForeignKey(
                    User,
                    on_delete=models.CASCADE,
                    related_name='products')

    tags        = models.ManyToManyField('Tag', blank=True)

    class Meta:
        ordering = ['-created_at']          # Default sort: newest first
        verbose_name = "Product"
        verbose_name_plural = "Products"
        indexes = [
            models.Index(fields=['name']),
            models.Index(fields=['category', 'is_active']),
        ]
        constraints = [
            models.UniqueConstraint(fields=['name', 'category'], name='unique_product_per_category'),
            models.CheckConstraint(check=models.Q(price__gte=0), name='price_non_negative'),
        ]

    def __str__(self):
        return self.name

    def get_absolute_url(self):
        from django.urls import reverse
        return reverse('myapp:product_detail', kwargs={'pk': self.pk})

    @property
    def is_in_stock(self):
        return self.stock > 0

    def apply_discount(self, percent):
        discount = self.price * (percent / 100)
        return self.price - discount
```

### All `on_delete` Options

| Option | What Happens When Parent Is Deleted |
|--------|-------------------------------------|
| `CASCADE` | Child records are also deleted |
| `PROTECT` | Deletion is blocked — raises `ProtectedError` |
| `RESTRICT` | Like PROTECT but raises `RestrictedError` (Django 3.1+) |
| `SET_NULL` | Child FK field is set to `NULL` (need `null=True`) |
| `SET_DEFAULT` | Child FK field is set to its default value |
| `SET(value)` | Child FK field is set to a specific value or function result |
| `DO_NOTHING` | Nothing — you must handle database integrity manually |

### Common Field Options

| Option | Meaning | Example |
|--------|---------|---------|
| `null=True` | DB column can be NULL | `age = IntegerField(null=True)` |
| `blank=True` | Form validation allows empty | `bio = TextField(blank=True)` |
| `default=value` | Default value if not set | `is_active = BooleanField(default=True)` |
| `unique=True` | No two records can have same value | `email = EmailField(unique=True)` |
| `db_index=True` | Create a DB index on this column | `slug = SlugField(db_index=True)` |
| `choices=LIST` | Only allow specific values | `status = CharField(choices=STATUS)` |
| `editable=False` | Not shown in admin/forms | `created_at = DateTimeField(editable=False)` |
| `verbose_name` | Human-readable label | `CharField(verbose_name="Full Name")` |

---

## 8. ORM — Complete Query Reference

### Basic QuerySet Operations

```python
from myapp.models import Product, Category

# ─── CREATE ───────────────────────────────────────────────────────────────
# Method 1: create() — saves immediately
product = Product.objects.create(name="Laptop", price=49999, stock=10)

# Method 2: save() — useful when you need to do something before saving
product = Product(name="Laptop", price=49999)
product.stock = 10
product.save()

# get_or_create — get if exists, create if not
product, created = Product.objects.get_or_create(
    sku="LAP001",
    defaults={'name': 'Laptop', 'price': 49999}
)
# created = True (new) or False (already existed)

# update_or_create — update if exists, create if not
product, created = Product.objects.update_or_create(
    sku="LAP001",
    defaults={'price': 45999, 'stock': 15}
)

# Bulk insert — much faster than individual creates
Product.objects.bulk_create([
    Product(name="Mouse", price=999),
    Product(name="Keyboard", price=1499),
    Product(name="Monitor", price=15999),
])


# ─── READ ─────────────────────────────────────────────────────────────────
# Get ALL records
products = Product.objects.all()

# Get ONE record by primary key
product = Product.objects.get(pk=1)      # raises DoesNotExist if not found
product = Product.objects.get(id=1)      # same thing

# get() with any field — raises DoesNotExist OR MultipleObjectsReturned
product = Product.objects.get(sku="LAP001")

# Safe get — use get_object_or_404 in views
from django.shortcuts import get_object_or_404
product = get_object_or_404(Product, pk=1)  # returns 404 response if not found

# First and last
product = Product.objects.first()        # first in default ordering
product = Product.objects.last()

# Exists check (efficient — no data fetched)
Product.objects.filter(name="Laptop").exists()   # True or False

# Count
Product.objects.count()
Product.objects.filter(is_active=True).count()


# ─── FILTER / EXCLUDE ─────────────────────────────────────────────────────
# Exact match
Product.objects.filter(name="Laptop")
Product.objects.filter(name__exact="Laptop")      # same thing

# Case-insensitive exact
Product.objects.filter(name__iexact="laptop")

# Contains (LIKE %value%)
Product.objects.filter(name__contains="top")
Product.objects.filter(name__icontains="top")     # case-insensitive

# Starts with / ends with
Product.objects.filter(name__startswith="La")
Product.objects.filter(name__endswith="op")

# Comparison
Product.objects.filter(price__gt=1000)             # price > 1000
Product.objects.filter(price__gte=1000)            # price >= 1000
Product.objects.filter(price__lt=5000)             # price < 5000
Product.objects.filter(price__lte=5000)            # price <= 5000

# Range
Product.objects.filter(price__range=(1000, 5000))  # 1000 <= price <= 5000

# IN list
Product.objects.filter(id__in=[1, 2, 3, 4])
Product.objects.filter(category__name__in=["Electronics", "Books"])

# IS NULL / IS NOT NULL
Product.objects.filter(image__isnull=True)
Product.objects.filter(image__isnull=False)

# Date lookups
Product.objects.filter(created_at__year=2024)
Product.objects.filter(created_at__month=1)
Product.objects.filter(created_at__date=date(2024, 1, 15))
Product.objects.filter(created_at__gte=datetime(2024, 1, 1))

# Exclude — opposite of filter
Product.objects.exclude(is_active=False)
Product.objects.exclude(category__name="Archived")

# Chaining filters (AND)
Product.objects.filter(is_active=True).filter(price__lt=5000)
Product.objects.filter(is_active=True, price__lt=5000)   # same thing

# OR queries — use Q objects
from django.db.models import Q

Product.objects.filter(Q(name__icontains="laptop") | Q(name__icontains="computer"))
Product.objects.filter(Q(price__lt=1000) | Q(is_featured=True))
Product.objects.filter(~Q(category__name="Archived"))   # NOT — tilde is negation

# Complex Q combinations
Product.objects.filter(
    (Q(price__lt=5000) | Q(is_featured=True)) & Q(is_active=True)
)


# ─── ORDERING ─────────────────────────────────────────────────────────────
Product.objects.order_by('name')          # ascending
Product.objects.order_by('-price')        # descending (prefix with -)
Product.objects.order_by('category', '-price')  # multiple fields
Product.objects.order_by('?')            # random order (slow on large tables)


# ─── SLICING (LIMIT / OFFSET) ─────────────────────────────────────────────
Product.objects.all()[:10]               # LIMIT 10
Product.objects.all()[10:20]             # LIMIT 10 OFFSET 10 (page 2)
Product.objects.all()[:1].get()          # first item (avoid negative indexing)


# ─── PROJECTION (SELECT SPECIFIC FIELDS) ──────────────────────────────────
# values() — returns list of dicts
Product.objects.values('id', 'name', 'price')
# [{'id': 1, 'name': 'Laptop', 'price': Decimal('49999')}, ...]

# values_list() — returns list of tuples
Product.objects.values_list('id', 'name')
# [(1, 'Laptop'), (2, 'Mouse'), ...]

# values_list with flat=True — flat list of single field
Product.objects.values_list('name', flat=True)
# ['Laptop', 'Mouse', 'Keyboard', ...]

# only() — fetch only specified fields (defers the rest)
Product.objects.only('name', 'price')

# defer() — fetch all fields EXCEPT the listed ones
Product.objects.defer('description', 'image')


# ─── RELATED OBJECT QUERIES ───────────────────────────────────────────────
# ForeignKey — follow relationship with double underscore
Product.objects.filter(category__name="Electronics")
Product.objects.filter(category__slug="electronics")

# Reverse ForeignKey — from Category to its Products
category = Category.objects.get(pk=1)
category.products.all()                       # related_name='products'
category.products.filter(is_active=True)

# ManyToMany
product = Product.objects.get(pk=1)
product.tags.all()
product.tags.add(tag)
product.tags.remove(tag)
product.tags.set([tag1, tag2])
product.tags.clear()

# select_related — fetches FK and OneToOne in a single SQL JOIN (reduces queries!)
products = Product.objects.select_related('category', 'created_by').all()

# prefetch_related — fetches ManyToMany and reverse FK in separate queries (efficient)
products = Product.objects.prefetch_related('tags').all()

# Combined
products = Product.objects.select_related('category').prefetch_related('tags')


# ─── UPDATE ───────────────────────────────────────────────────────────────
# Update one object
product = Product.objects.get(pk=1)
product.price = 45999
product.save()

# Update only specific fields (more efficient)
product.save(update_fields=['price', 'updated_at'])

# Bulk update — efficient, single SQL UPDATE
Product.objects.filter(category__name="Electronics").update(is_active=True)
Product.objects.filter(stock=0).update(is_active=False)

# F() — reference another field in an update (avoids race conditions)
from django.db.models import F
Product.objects.filter(pk=1).update(stock=F('stock') - 1)   # decrement stock
Product.objects.all().update(price=F('price') * 1.1)         # 10% price increase


# ─── DELETE ───────────────────────────────────────────────────────────────
# Delete one
product = Product.objects.get(pk=1)
product.delete()

# Bulk delete
Product.objects.filter(is_active=False).delete()
Product.objects.filter(stock=0, is_featured=False).delete()


# ─── DEBUG — See the actual SQL ───────────────────────────────────────────
qs = Product.objects.filter(price__lt=5000).order_by('-created_at')
print(qs.query)
# SELECT "myapp_product"."id", ... FROM "myapp_product"
# WHERE "myapp_product"."price" < 5000 ORDER BY "myapp_product"."created_at" DESC
```

---

## 9. Aggregations & Annotations

```python
from django.db.models import Avg, Count, Sum, Max, Min, StdDev, Variance
from myapp.models import Product, Order

# ─── AGGREGATE — one result for the whole queryset ─────────────────────────
# Single aggregation
result = Product.objects.aggregate(Avg('price'))
# {'price__avg': 12499.5}

# Multiple aggregations at once
result = Product.objects.aggregate(
    avg_price=Avg('price'),
    total_stock=Sum('stock'),
    max_price=Max('price'),
    min_price=Min('price'),
    product_count=Count('id')
)
# {'avg_price': 12499.5, 'total_stock': 450, 'max_price': 89999, ...}

# Aggregate on a filtered queryset
result = Product.objects.filter(is_active=True).aggregate(
    avg_price=Avg('price'),
    count=Count('id')
)


# ─── ANNOTATE — adds a computed field to EACH object in the queryset ───────
from django.db.models import Count, Sum, F, ExpressionWrapper, DecimalField

# Count related objects — number of products per category
categories = Category.objects.annotate(product_count=Count('products'))
for cat in categories:
    print(cat.name, cat.product_count)

# Sum across related model
from myapp.models import OrderItem
orders = Order.objects.annotate(
    total_amount=Sum('items__price')
)

# Computed field using F expressions
products = Product.objects.annotate(
    discounted_price=ExpressionWrapper(
        F('price') * 0.9,
        output_field=DecimalField(max_digits=10, decimal_places=2)
    )
)
for p in products:
    print(p.name, p.discounted_price)

# Count with filter (conditional aggregation)
from django.db.models import Count, Q
categories = Category.objects.annotate(
    active_products=Count('products', filter=Q(products__is_active=True)),
    total_products=Count('products')
)

# Group by + having (filter on aggregated value)
from django.db.models import Count
popular_categories = Category.objects.annotate(
    product_count=Count('products')
).filter(product_count__gte=5).order_by('-product_count')
```

---

## 10. Migrations

```bash
# ─── Create migration files (after changing models.py) ────────────────────
python manage.py makemigrations
python manage.py makemigrations myapp           # specific app only
python manage.py makemigrations --name add_slug_to_product  # custom name

# ─── Apply migrations to the database ─────────────────────────────────────
python manage.py migrate
python manage.py migrate myapp                  # specific app only
python manage.py migrate myapp 0003             # migrate to specific version

# ─── See migration status ─────────────────────────────────────────────────
python manage.py showmigrations
python manage.py showmigrations myapp

# ─── See the SQL a migration will run ─────────────────────────────────────
python manage.py sqlmigrate myapp 0001

# ─── Undo (rollback) migrations ───────────────────────────────────────────
python manage.py migrate myapp 0002             # roll back to 0002 (undo 0003+)
python manage.py migrate myapp zero             # undo ALL migrations for this app

# ─── Detect issues without creating files ─────────────────────────────────
python manage.py makemigrations --check         # exit code 1 if changes needed
python manage.py migrate --plan                 # show what will be run

# ─── Squash migrations (combine many into one) ─────────────────────────────
python manage.py squashmigrations myapp 0001 0010
```

### Data Migrations (Populate or Transform Data)

```python
# migrations/0005_populate_slugs.py
from django.db import migrations
from django.utils.text import slugify

def populate_slugs(apps, schema_editor):
    Product = apps.get_model('myapp', 'Product')
    for product in Product.objects.all():
        if not product.slug:
            product.slug = slugify(product.name)
            product.save()

def reverse_slugs(apps, schema_editor):
    pass  # nothing to reverse

class Migration(migrations.Migration):
    dependencies = [('myapp', '0004_product_slug')]
    operations = [
        migrations.RunPython(populate_slugs, reverse_slugs),
    ]
```

---

## 11. Django Admin

```python
# myapp/admin.py

from django.contrib import admin
from .models import Product, Category

# ─── Simple registration ───────────────────────────────────────────────────
admin.site.register(Category)


# ─── Full-featured registration ───────────────────────────────────────────
@admin.register(Product)
class ProductAdmin(admin.ModelAdmin):

    # Columns shown in the list view
    list_display = ('name', 'price', 'stock', 'is_active', 'category', 'created_at')

    # Fields that link to the detail page
    list_display_links = ('name',)

    # Fields editable directly in the list view (no need to open detail)
    list_editable = ('price', 'is_active', 'stock')

    # Sidebar filters
    list_filter = ('is_active', 'is_featured', 'category', 'created_at')

    # Search box (searches these fields)
    search_fields = ('name', 'sku', 'description')

    # Pre-populate slug from name
    prepopulated_fields = {'slug': ('name',)}

    # Date hierarchy navigation at the top
    date_hierarchy = 'created_at'

    # Default sort
    ordering = ('-created_at',)

    # How many items per page
    list_per_page = 25

    # Fields shown in the detail/edit form
    fields = ('name', 'slug', 'description', 'price', 'stock', 'category', 'image', 'is_active')

    # OR use fieldsets for grouped sections
    fieldsets = (
        ('Basic Info', {
            'fields': ('name', 'slug', 'description')
        }),
        ('Pricing & Inventory', {
            'fields': ('price', 'stock')
        }),
        ('Settings', {
            'fields': ('is_active', 'is_featured', 'category'),
            'classes': ('collapse',)  # collapsible section
        }),
    )

    # Read-only fields
    readonly_fields = ('created_at', 'updated_at')

    # Custom action
    def make_active(self, request, queryset):
        queryset.update(is_active=True)
        self.message_user(request, f"{queryset.count()} products activated.")
    make_active.short_description = "Mark selected products as active"
    actions = ['make_active']

    # Customize queryset for list view (add select_related for performance)
    def get_queryset(self, request):
        return super().get_queryset(request).select_related('category')


# Customize admin site branding
admin.site.site_header = "My Shop Admin"
admin.site.site_title = "My Shop"
admin.site.index_title = "Dashboard"
```

---

## 12. Forms & Validation

```python
# myapp/forms.py

from django import forms
from django.core.validators import MinValueValidator
from .models import Product

# ─── Regular Form (not tied to a model) ───────────────────────────────────
class ContactForm(forms.Form):
    name    = forms.CharField(max_length=100, label="Your Name")
    email   = forms.EmailField()
    subject = forms.CharField(max_length=200)
    message = forms.CharField(widget=forms.Textarea(attrs={'rows': 5}))
    rating  = forms.IntegerField(
                validators=[MinValueValidator(1)],
                widget=forms.NumberInput(attrs={'min': 1, 'max': 5}))

    # Custom field validation
    def clean_email(self):
        email = self.cleaned_data['email']
        if 'spam.com' in email:
            raise forms.ValidationError("We don't accept spam domains.")
        return email

    # Cross-field validation
    def clean(self):
        cleaned_data = super().clean()
        name = cleaned_data.get('name')
        message = cleaned_data.get('message')
        if name and message and name.lower() in message.lower():
            raise forms.ValidationError("Please don't include your name in the message.")
        return cleaned_data


# ─── ModelForm (tied to a model — much less code) ──────────────────────────
class ProductForm(forms.ModelForm):
    class Meta:
        model = Product
        fields = ['name', 'description', 'price', 'stock', 'category', 'image']
        # OR exclude specific fields:
        # exclude = ['created_by', 'created_at']

        widgets = {
            'description': forms.Textarea(attrs={'rows': 4, 'class': 'form-control'}),
            'price': forms.NumberInput(attrs={'step': '0.01', 'class': 'form-control'}),
        }
        labels = {
            'name': 'Product Name',
            'sku': 'SKU Code',
        }
        help_texts = {
            'price': 'Enter price in Indian Rupees.',
        }
        error_messages = {
            'name': {'required': 'Product name is required.'},
        }

    def clean_price(self):
        price = self.cleaned_data['price']
        if price <= 0:
            raise forms.ValidationError("Price must be greater than zero.")
        return price
```

### Using Forms in Views

```python
def create_product(request):
    if request.method == 'POST':
        form = ProductForm(request.POST, request.FILES)  # FILES for image uploads
        if form.is_valid():
            product = form.save(commit=False)       # don't save to DB yet
            product.created_by = request.user       # add extra data
            product.save()                          # NOW save
            form.save_m2m()                         # save ManyToMany fields
            messages.success(request, "Product created!")
            return redirect('myapp:product_list')
        else:
            messages.error(request, "Please fix the errors below.")
    else:
        form = ProductForm()

    return render(request, 'myapp/product_form.html', {'form': form})
```

---

## 13. Authentication & Authorization

```python
# ─── Login / Logout / Register Views ──────────────────────────────────────
from django.contrib.auth import authenticate, login, logout
from django.contrib.auth.models import User

def login_view(request):
    if request.method == 'POST':
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(request, username=username, password=password)
        if user is not None:
            login(request, user)
            next_url = request.GET.get('next', '/dashboard/')
            return redirect(next_url)
        else:
            messages.error(request, "Invalid username or password.")
    return render(request, 'registration/login.html')

def logout_view(request):
    logout(request)
    return redirect('/')

def register_view(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            login(request, user)
            return redirect('/dashboard/')
    else:
        form = UserCreationForm()
    return render(request, 'registration/register.html', {'form': form})


# ─── Protecting Views ──────────────────────────────────────────────────────
from django.contrib.auth.decorators import login_required, permission_required
from django.contrib.auth.mixins import LoginRequiredMixin, PermissionRequiredMixin

# FBV — decorator
@login_required
def dashboard(request):
    return render(request, 'dashboard.html')

@login_required(login_url='/custom-login/')
def profile(request):
    pass

@permission_required('myapp.can_publish', raise_exception=True)
def publish_product(request):
    pass

# CBV — mixin
class DashboardView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'next'

    def get(self, request):
        return render(request, 'dashboard.html')


# ─── Working with the User object ─────────────────────────────────────────
user = request.user

user.is_authenticated    # True if logged in
user.is_staff            # True if staff user
user.is_superuser        # True if superuser
user.username
user.email
user.first_name
user.last_name
user.get_full_name()

# Check permissions
user.has_perm('myapp.add_product')
user.has_perm('myapp.change_product')
user.has_perm('myapp.delete_product')
user.has_perms(['myapp.add_product', 'myapp.change_product'])

# Set password
user.set_password('new_password')
user.save()

# Check password
user.check_password('raw_password')  # True/False
```

---

## 14. Middleware

```python
# ─── Custom Middleware ─────────────────────────────────────────────────────
# myapp/middleware.py

import time
import logging
logger = logging.getLogger(__name__)

class RequestTimingMiddleware:
    """Logs how long each request takes."""

    def __init__(self, get_response):
        self.get_response = get_response   # get_response = next middleware or view

    def __call__(self, request):
        # CODE HERE runs BEFORE the view
        start_time = time.time()

        response = self.get_response(request)  # calls the next layer

        # CODE HERE runs AFTER the view
        duration = time.time() - start_time
        logger.info(f"{request.method} {request.path} — {duration:.3f}s")

        return response

    def process_exception(self, request, exception):
        # Called when a view raises an exception
        logger.error(f"Exception on {request.path}: {exception}")
        return None  # Return None to let Django's default handler run

    def process_view(self, request, view_func, view_args, view_kwargs):
        # Called just before Django calls the view
        return None  # Return None to continue; return HttpResponse to short-circuit
```

```python
# Register it in settings.py
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'myapp.middleware.RequestTimingMiddleware',   # your custom middleware
]
```

### Middleware Execution Order

```
Request comes in:
SecurityMiddleware       ← runs first (top of list)
  SessionMiddleware
    CommonMiddleware
      CsrfViewMiddleware
        AuthenticationMiddleware
          MessageMiddleware
            XFrameOptionsMiddleware
              VIEW RUNS HERE
            XFrameOptionsMiddleware  ← then unwinds in reverse
          MessageMiddleware
        AuthenticationMiddleware
      CsrfViewMiddleware
    CommonMiddleware
  SessionMiddleware
SecurityMiddleware       ← exits last
Response goes out
```

---

## 15. Static & Media Files

```python
# settings.py
STATIC_URL = '/static/'
STATICFILES_DIRS = [BASE_DIR / 'static']      # your static files (CSS, JS, images)
STATIC_ROOT = BASE_DIR / 'staticfiles'         # collectstatic destination

MEDIA_URL = '/media/'
MEDIA_ROOT = BASE_DIR / 'media'                # uploaded user files
```

```bash
# Collect all static files into STATIC_ROOT (run before deploying)
python manage.py collectstatic
```

```python
# Serve media files in development (urls.py)
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [...] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

```html
<!-- In templates -->
{% load static %}
<link rel="stylesheet" href="{% static 'css/style.css' %}">
<script src="{% static 'js/app.js' %}"></script>
<img src="{% static 'images/logo.png' %}" alt="Logo">

<!-- Accessing an uploaded user file -->
<img src="{{ product.image.url }}" alt="{{ product.name }}">
```

---

## 16. Django REST Framework (DRF)

```bash
pip install djangorestframework
```

```python
# settings.py
INSTALLED_APPS = [..., 'rest_framework']

REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticatedOrReadOnly',
    ],
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 20,
    'DEFAULT_THROTTLE_CLASSES': [
        'rest_framework.throttling.AnonRateThrottle',
        'rest_framework.throttling.UserRateThrottle',
    ],
    'DEFAULT_THROTTLE_RATES': {
        'anon': '100/day',
        'user': '1000/day',
    }
}
```

```python
# myapp/serializers.py
from rest_framework import serializers
from .models import Product, Category

class CategorySerializer(serializers.ModelSerializer):
    class Meta:
        model = Category
        fields = ['id', 'name', 'slug']

class ProductSerializer(serializers.ModelSerializer):
    category = CategorySerializer(read_only=True)
    category_id = serializers.PrimaryKeyRelatedField(
        queryset=Category.objects.all(), source='category', write_only=True
    )
    is_in_stock = serializers.SerializerMethodField()

    class Meta:
        model = Product
        fields = ['id', 'name', 'slug', 'price', 'stock', 'category',
                  'category_id', 'is_active', 'is_in_stock', 'created_at']
        read_only_fields = ['id', 'created_at']

    def get_is_in_stock(self, obj):
        return obj.stock > 0

    def validate_price(self, value):
        if value <= 0:
            raise serializers.ValidationError("Price must be greater than zero.")
        return value
```

```python
# myapp/views.py (API views)
from rest_framework import viewsets, permissions, filters
from rest_framework.decorators import action
from rest_framework.response import Response
from django_filters.rest_framework import DjangoFilterBackend
from .models import Product
from .serializers import ProductSerializer

class ProductViewSet(viewsets.ModelViewSet):
    queryset = Product.objects.select_related('category').filter(is_active=True)
    serializer_class = ProductSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]

    # Filtering, searching, ordering
    filter_backends = [DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter]
    filterset_fields = ['category', 'is_featured']
    search_fields = ['name', 'description']
    ordering_fields = ['price', 'created_at']
    ordering = ['-created_at']

    # Custom action
    @action(detail=False, methods=['get'], url_path='featured')
    def featured(self, request):
        products = self.get_queryset().filter(is_featured=True)
        serializer = self.get_serializer(products, many=True)
        return Response(serializer.data)

    @action(detail=True, methods=['post'], url_path='toggle-active')
    def toggle_active(self, request, pk=None):
        product = self.get_object()
        product.is_active = not product.is_active
        product.save()
        return Response({'is_active': product.is_active})
```

```python
# myapp/api_urls.py
from rest_framework.routers import DefaultRouter
from . import views

router = DefaultRouter()
router.register('products', views.ProductViewSet)

urlpatterns = router.urls
# Auto-generates:
# GET/POST  /api/products/
# GET/PUT/PATCH/DELETE /api/products/{pk}/
# GET /api/products/featured/
# POST /api/products/{pk}/toggle-active/
```

---

## 17. Signals

```python
# myapp/signals.py
from django.db.models.signals import post_save, post_delete, pre_save
from django.contrib.auth.signals import user_logged_in
from django.dispatch import receiver
from django.core.mail import send_mail
from .models import Product, Order

# Run code AFTER a model is saved
@receiver(post_save, sender=Order)
def order_created_notification(sender, instance, created, **kwargs):
    if created:                              # only on creation, not updates
        send_mail(
            subject=f"Order #{instance.id} Confirmed",
            message=f"Your order has been placed. Total: ₹{instance.total}",
            from_email='orders@myshop.com',
            recipient_list=[instance.user.email],
        )

# Run code BEFORE a model is saved
@receiver(pre_save, sender=Product)
def auto_generate_slug(sender, instance, **kwargs):
    if not instance.slug:
        from django.utils.text import slugify
        instance.slug = slugify(instance.name)

# Run code AFTER a model is deleted
@receiver(post_delete, sender=Product)
def delete_product_image(sender, instance, **kwargs):
    if instance.image:
        instance.image.delete(save=False)   # delete file from disk

# User login signal
@receiver(user_logged_in)
def on_user_login(sender, request, user, **kwargs):
    from django.utils import timezone
    user.last_login_ip = request.META.get('REMOTE_ADDR')
    user.save(update_fields=['last_login_ip'])
```

```python
# myapp/apps.py — register signals
class MyappConfig(AppConfig):
    name = 'myapp'

    def ready(self):
        import myapp.signals  # this connects all the receivers
```

---

## 18. Caching

```python
# settings.py

# Redis cache (recommended for production)
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
    }
}

# Memcached alternative
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.memcached.PyMemcacheCache',
        'LOCATION': '127.0.0.1:11211',
    }
}
```

```python
from django.core.cache import cache
from django.views.decorators.cache import cache_page
from django.views.decorators.vary import vary_on_headers

# ─── Low-level cache API ──────────────────────────────────────────────────
cache.set('my_key', 'my_value', timeout=300)    # store for 5 minutes
value = cache.get('my_key')                      # None if not found or expired
value = cache.get('my_key', default='fallback')

cache.set_many({'key1': 'val1', 'key2': 'val2'}, timeout=600)
cache.get_many(['key1', 'key2'])

cache.delete('my_key')
cache.delete_many(['key1', 'key2'])
cache.clear()                                    # clear entire cache (careful!)

# Increment/Decrement (atomic — great for counters)
cache.set('page_views', 0)
cache.incr('page_views')
cache.decr('page_views')

# ─── Cache-aside pattern (most common) ────────────────────────────────────
def get_products():
    cache_key = 'all_active_products'
    products = cache.get(cache_key)
    if products is None:
        products = list(Product.objects.filter(is_active=True).values())
        cache.set(cache_key, products, timeout=3600)   # cache for 1 hour
    return products

# ─── View-level caching ────────────────────────────────────────────────────
@cache_page(60 * 15)         # cache this entire view for 15 minutes
def product_list(request):
    ...

# Vary on headers (e.g., different cache per user)
@cache_page(60 * 15)
@vary_on_headers('Accept-Language', 'Cookie')
def product_list(request):
    ...
```

---

## 19. Transactions

```python
from django.db import transaction

# ─── @atomic decorator ─────────────────────────────────────────────────────
@transaction.atomic
def transfer_money(sender_id, receiver_id, amount):
    sender   = Account.objects.select_for_update().get(id=sender_id)
    receiver = Account.objects.select_for_update().get(id=receiver_id)

    if sender.balance < amount:
        raise ValueError("Insufficient balance")

    sender.balance   -= amount
    receiver.balance += amount
    sender.save()
    receiver.save()
    # If any exception is raised → BOTH saves are automatically rolled back

# ─── Context manager ───────────────────────────────────────────────────────
def create_order(user, cart_items):
    with transaction.atomic():
        order = Order.objects.create(user=user, status='pending')
        for item in cart_items:
            OrderItem.objects.create(order=order, product=item.product, qty=item.qty)
            item.product.stock -= item.qty
            item.product.save()
        # All above in one transaction — all succeed or all rollback

# ─── Savepoints — nested transactions ─────────────────────────────────────
def complex_operation():
    with transaction.atomic():                      # outer transaction
        do_operation_1()

        with transaction.atomic():                  # creates a SAVEPOINT
            try:
                do_risky_operation()
            except Exception:
                pass   # rolls back only the inner savepoint, outer continues

        do_operation_2()

# ─── select_for_update — lock rows ────────────────────────────────────────
# Prevents other transactions from reading/updating the same rows (row-level lock)
with transaction.atomic():
    product = Product.objects.select_for_update().get(pk=1)
    product.stock -= 1
    product.save()

# ─── on_commit — run after transaction commits ─────────────────────────────
def after_commit():
    send_confirmation_email(order_id)

with transaction.atomic():
    order = Order.objects.create(...)
    transaction.on_commit(lambda: send_confirmation_email(order.id))
    # Email is only sent if the transaction commits successfully
```

---

## 20. Security

### Built-in Django Security Features

| Threat | Django Protection | How |
|--------|------------------|-----|
| **CSRF** | `CsrfViewMiddleware` | `{% csrf_token %}` in every POST form |
| **XSS** | Auto-escaping templates | `{{ var }}` escapes `<`, `>`, `&` automatically |
| **SQL Injection** | ORM parameterized queries | Never builds raw SQL from user input |
| **Clickjacking** | `XFrameOptionsMiddleware` | `X-Frame-Options: DENY` header |
| **Password** | PBKDF2 hashing | Passwords never stored in plain text |
| **HTTPS** | `SecurityMiddleware` | Forces HSTS, redirects HTTP → HTTPS |

### Production Security Settings

```python
# settings/production.py

DEBUG = False                          # NEVER True in production
SECRET_KEY = os.environ['SECRET_KEY']  # from environment, not hardcoded

ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']

# HTTPS enforcement
SECURE_SSL_REDIRECT = True             # redirect all HTTP → HTTPS
SECURE_HSTS_SECONDS = 31536000        # 1 year HSTS
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True

# Cookie security
SESSION_COOKIE_SECURE = True           # session cookie only over HTTPS
CSRF_COOKIE_SECURE = True              # CSRF cookie only over HTTPS
SESSION_COOKIE_HTTPONLY = True         # JS cannot access session cookie
CSRF_COOKIE_HTTPONLY = True

# Clickjacking
X_FRAME_OPTIONS = 'DENY'

# Content type sniffing
SECURE_CONTENT_TYPE_NOSNIFF = True

# Browser XSS filter
SECURE_BROWSER_XSS_FILTER = True
```

---

## 21. WSGI & ASGI

### WSGI (Synchronous — Traditional)

```python
# myproject/wsgi.py
import os
from django.core.wsgi import get_wsgi_application
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')
application = get_wsgi_application()
```

```bash
# Run with Gunicorn (production WSGI server)
pip install gunicorn
gunicorn myproject.wsgi:application --workers 4 --bind 0.0.0.0:8000
```

### ASGI (Asynchronous — for WebSockets, async views)

```python
# myproject/asgi.py
import os
from django.core.asgi import get_asgi_application
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')
application = get_asgi_application()
```

```bash
# Run with Uvicorn (production ASGI server)
pip install uvicorn
uvicorn myproject.asgi:application --workers 4 --host 0.0.0.0 --port 8000
```

| | **WSGI** | **ASGI** |
|-|---------|---------|
| Request type | Synchronous only | Sync + Async + WebSockets |
| Server | Gunicorn, uWSGI | Uvicorn, Daphne |
| Use case | Traditional web apps | Real-time apps, chat, notifications |
| Django support | Since beginning | Django 3.0+ |

---

## 22. Complete manage.py Commands Reference

```bash
# ─── Project & App ────────────────────────────────────────────────────────
django-admin startproject myproject           # create a new project
python manage.py startapp myapp              # create a new app inside the project


# ─── Development Server ───────────────────────────────────────────────────
python manage.py runserver                   # start dev server at localhost:8000
python manage.py runserver 0.0.0.0:8000      # accessible from other devices
python manage.py runserver 8080              # custom port
python manage.py runserver --noreload        # don't auto-reload on file changes
python manage.py runserver --nostatic        # don't serve static files


# ─── Database & Migrations ────────────────────────────────────────────────
python manage.py makemigrations              # detect model changes → create migration files
python manage.py makemigrations myapp        # specific app only
python manage.py makemigrations --empty myapp  # empty migration (for data migrations)
python manage.py makemigrations --merge       # resolve migration conflicts
python manage.py makemigrations --check       # exit 1 if changes detected (for CI)

python manage.py migrate                     # apply all pending migrations
python manage.py migrate myapp               # migrate specific app
python manage.py migrate myapp 0003          # migrate to specific version
python manage.py migrate myapp zero          # rollback ALL migrations for an app

python manage.py showmigrations              # list all migrations and status
python manage.py showmigrations myapp        # specific app

python manage.py sqlmigrate myapp 0001       # show SQL for a migration
python manage.py squashmigrations myapp 0001 0010  # squash migration range

python manage.py dbshell                     # open raw SQL shell for your DB
python manage.py inspectdb                   # generate models from existing DB tables
python manage.py inspectdb > models.py       # save to file


# ─── Django Shell ─────────────────────────────────────────────────────────
python manage.py shell                       # interactive Python shell with Django loaded
python manage.py shell -c "from myapp.models import Product; print(Product.objects.count())"
python manage.py shell_plus                  # enhanced shell (django-extensions) — auto-imports models


# ─── Static Files ─────────────────────────────────────────────────────────
python manage.py collectstatic               # copy all static files to STATIC_ROOT
python manage.py collectstatic --noinput     # no confirmation prompt (for CI/CD)
python manage.py findstatic style.css        # find where a static file comes from


# ─── User Management ──────────────────────────────────────────────────────
python manage.py createsuperuser             # create an admin superuser (interactive)
python manage.py changepassword username     # change a user's password
python manage.py create_test_users           # (custom) if you've written this command


# ─── Fixtures (Import / Export Data) ──────────────────────────────────────
python manage.py dumpdata                    # export ALL data as JSON
python manage.py dumpdata myapp              # export specific app
python manage.py dumpdata myapp.Product      # export specific model
python manage.py dumpdata myapp --indent 2 --output products.json

python manage.py loaddata products.json      # import data from fixture file
python manage.py loaddata myapp/fixtures/initial_data.json


# ─── Testing ──────────────────────────────────────────────────────────────
python manage.py test                        # run ALL tests
python manage.py test myapp                  # test specific app
python manage.py test myapp.tests.ProductTests  # specific test class
python manage.py test --verbosity=2          # verbose output
python manage.py test --keepdb               # keep test DB between runs (faster)
python manage.py test --parallel             # run tests in parallel
python manage.py test --failfast             # stop on first failure


# ─── Validation & Checks ──────────────────────────────────────────────────
python manage.py check                       # run system checks (settings, models, etc.)
python manage.py check --deploy              # extra checks for production deployment
python manage.py validate                    # deprecated — use check


# ─── Clearing & Maintenance ───────────────────────────────────────────────
python manage.py clearsessions               # delete expired sessions from database
python manage.py flush                       # DELETE all data from all tables (DANGEROUS)
python manage.py reset_db                    # (django-extensions) drops and recreates DB


# ─── Internationalization ─────────────────────────────────────────────────
python manage.py makemessages -l hi          # create translation file for Hindi
python manage.py compilemessages             # compile .po files to .mo


# ─── Graph (django-extensions) ────────────────────────────────────────────
python manage.py graph_models myapp -o model_graph.png   # visualize model relationships
```

---

## 23. Django with PostgreSQL

```bash
pip install psycopg2-binary
```

```python
# settings.py
DATABASES = {
    'default': {
        'ENGINE':   'django.db.backends.postgresql',
        'NAME':     os.environ.get('DB_NAME', 'mydb'),
        'USER':     os.environ.get('DB_USER', 'postgres'),
        'PASSWORD': os.environ.get('DB_PASSWORD', ''),
        'HOST':     os.environ.get('DB_HOST', 'localhost'),
        'PORT':     os.environ.get('DB_PORT', '5432'),
        'CONN_MAX_AGE': 60,   # keep connections open for 60 seconds (connection pooling)
        'OPTIONS': {
            'sslmode': 'require',   # enforce SSL in production
        }
    }
}
```

---

## 24. Best Practices

### ✅ Do's

- **Use environment variables** for `SECRET_KEY`, DB passwords, API keys — never hardcode them
- **Use `get_object_or_404()`** in views instead of bare `.get()` — proper 404 responses
- **Use `select_related` / `prefetch_related`** to avoid N+1 query problems
- **Use `only()` / `defer()`** when you only need a few fields — reduces data transfer
- **Write tests** — `python manage.py test` catches regressions early
- **Use `@transaction.atomic`** for any operation that must succeed or fail together
- **Add `__str__`** to every model — makes admin and shell readable
- **Use `Meta.ordering`** on models so querysets are always consistently ordered
- **Use `F()` objects** for updating fields based on current value — avoids race conditions
- **Run `python manage.py check --deploy`** before every production deployment

### ❌ Don'ts

- **Don't use `DEBUG=True` in production** — exposes full stack traces to users
- **Don't use `*` in `ALLOWED_HOSTS` in production** — security risk
- **Don't use `FloatField` for money** — use `DecimalField` to avoid floating-point errors
- **Don't call `.all()` then loop and call `.filter()` inside** — causes N+1 query problem
- **Don't put business logic in views** — put it in models or service layer
- **Don't commit `SECRET_KEY` to Git** — rotate immediately if exposed
- **Don't skip migrations in production** — always run `migrate` after deploying
- **Don't use raw SQL unless necessary** — ORM is safer and database-agnostic
- **Don't store passwords in plain text** — always use Django's `set_password()`
- **Don't ignore Django's system check warnings** — `python manage.py check`

---

## 25. Common Interview Questions

### Q1: What is Django and what is its architectural pattern?
**Answer:** Django is a high-level Python web framework that follows the MVT (Model-View-Template) pattern. The Model defines the data structure and interacts with the database, the View contains the business logic — it processes the request, interacts with the model, and returns a response, and the Template is the HTML presentation layer. The URL dispatcher routes incoming requests to the correct view. Django's philosophy is "batteries included" — it ships with an ORM, admin interface, authentication, form handling, and security features out of the box.

---

### Q2: What is the Django ORM and why is it useful?
**Answer:** The ORM (Object Relational Mapper) lets you interact with the database using Python code instead of writing raw SQL. You define your data as Python classes (models), and Django automatically creates the corresponding SQL tables. When you query with `Product.objects.filter(price__lt=5000)`, Django translates that into `SELECT * FROM product WHERE price < 5000`. The ORM is database-agnostic — the same Python code works with SQLite, PostgreSQL, or MySQL. It also provides protection against SQL injection because all queries are parameterized automatically.

---

### Q3: What is the difference between `null=True` and `blank=True`?
**Answer:** They operate at different levels. `null=True` is a **database-level** setting — it allows the column to store a NULL value in the database. `blank=True` is a **form/validation-level** setting — it allows the field to be empty when submitting a form. For string-based fields like `CharField` and `TextField`, Django convention is to use `blank=True` without `null=True` — Django stores empty strings instead of NULL. For non-string fields like `IntegerField`, `DateField`, and `ForeignKey`, use both `null=True, blank=True` when the field should be optional.

---

### Q4: What is `select_related` vs `prefetch_related`?
**Answer:** Both solve the N+1 query problem but in different ways. `select_related` is for ForeignKey and OneToOne relationships — it performs a single SQL JOIN to fetch the related object in the same query. `prefetch_related` is for ManyToMany and reverse ForeignKey relationships — it runs a separate SQL query and Python-level joining. The rule: use `select_related` for single-valued relationships (FK, O2O), and `prefetch_related` for multi-valued relationships (M2M, reverse FK). Without them, accessing a related object inside a loop triggers a new database query for every iteration.

---

### Q5: What is a Django migration and why is it needed?
**Answer:** A migration is an auto-generated Python file that records a change to your models — adding a field, removing a model, creating a new table. It keeps the database schema in sync with your `models.py`. When you change a model, you run `makemigrations` to generate the migration file, then `migrate` to apply it to the database. Migrations provide a version-controlled history of database changes that the whole team can apply consistently. `showmigrations` shows which migrations have been applied and `sqlmigrate` shows the actual SQL that will run.

---

### Q6: What is middleware in Django?
**Answer:** Middleware is a framework of hooks that sits between the HTTP request and the view. Each middleware component can process a request before it reaches the view and process the response before it is sent to the client. Middleware runs in order — the first middleware in the list processes the request first and the response last (like a stack). Django's built-in middleware handles session management (`SessionMiddleware`), CSRF protection (`CsrfViewMiddleware`), authentication (`AuthenticationMiddleware`), security headers (`SecurityMiddleware`), and clickjacking prevention (`XFrameOptionsMiddleware`). You can write custom middleware by defining a class with `__init__` and `__call__` methods.

---

### Q7: What is `@transaction.atomic` and when do you use it?
**Answer:** `@transaction.atomic` wraps database operations in a single transaction — if any operation inside raises an exception, all changes are automatically rolled back. Without it, each `.save()` call is its own transaction — if the second save fails, the first save is already committed and your data is inconsistent. Use it whenever you have multiple related database writes that must all succeed or all fail together — bank transfers, order creation (create order + update stock + create order items), or any operation where partial completion would leave the data in a broken state.

---

### Q8: What is the difference between `aggregate()` and `annotate()`?
**Answer:** Both perform calculations, but at different levels. `aggregate()` returns a single summary value for the entire queryset — like `Product.objects.aggregate(Avg('price'))` returns one average across all products. `annotate()` adds a calculated field to each individual object in the queryset — like `Category.objects.annotate(product_count=Count('products'))` adds a `product_count` attribute to every category object. Think of aggregate as "give me one number for the whole set" and annotate as "give me an extra column for each row."

---

### Q9: What is CSRF and how does Django protect against it?
**Answer:** CSRF (Cross-Site Request Forgery) is an attack where a malicious website tricks a logged-in user into unknowingly submitting a form to your site — for example, a hidden form that transfers money. Django's `CsrfViewMiddleware` generates a unique token for each user session. Every HTML form must include `{% csrf_token %}` which inserts a hidden field with this token. When a POST request arrives, Django checks that the submitted token matches the session token. Since the attacker's site cannot read the CSRF token from your domain (same-origin policy), the attack fails.

---

### Q10: What is the difference between FBV and CBV in Django?
**Answer:** Function-Based Views (FBV) are simple Python functions that take a request and return a response. They are easy to read and understand, and work well for custom or complex logic. Class-Based Views (CBV) are Python classes that provide structure and code reuse through inheritance. Django provides generic CBVs like `ListView`, `DetailView`, `CreateView`, `UpdateView`, and `DeleteView` that implement common CRUD patterns with minimal code. Use FBVs for unique logic where clarity matters; use CBVs for standard CRUD operations where you want to avoid writing boilerplate.

---

## 🔄 Django — At a Glance

```
Request → URL Dispatcher → Middleware (in) → View → Model → Database
                                                  ↓
Response ← URL Dispatcher ← Middleware (out) ← View ← Template
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Django | High-level Python web framework — batteries included |
| MVT | Model (data) + View (logic) + Template (HTML) |
| `settings.py` | Central config — database, apps, middleware, security |
| `SECRET_KEY` | Cryptographic key — CSRF, sessions, signed cookies |
| `urls.py` | Maps URL patterns to view functions |
| `models.py` | Python classes that define DB tables |
| `views.py` | Request processing logic — returns HttpResponse |
| `templates/` | HTML files with Django template language |
| ORM | Write Python, get SQL — database-agnostic queries |
| `makemigrations` | Detect model changes → create migration file |
| `migrate` | Apply migration files → update actual database |
| `select_related` | SQL JOIN for FK/O2O — prevents N+1 queries |
| `prefetch_related` | Separate query for M2M/reverse FK — prevents N+1 |
| `aggregate()` | One summary value for entire queryset (AVG, SUM...) |
| `annotate()` | Add computed field to each object in queryset |
| `Q()` | Complex OR/AND queries |
| `F()` | Reference another field in a query/update |
| `@transaction.atomic` | All DB ops succeed or all rollback |
| `select_for_update()` | Lock rows during a transaction |
| Middleware | Hooks that run on every request/response |
| CSRF | `{% csrf_token %}` in every POST form |
| `@login_required` | Restrict view to authenticated users |
| `get_object_or_404` | Get object or return 404 — use in all views |
| Signals | Post-save, pre-save hooks — decouple side effects |
| DRF | Django REST Framework — build JSON APIs |
| WSGI | Sync interface — Gunicorn, uWSGI |
| ASGI | Async interface — Uvicorn, Daphne |
| `collectstatic` | Copy all static files to STATIC_ROOT for deployment |
| `check --deploy` | Run production deployment checks |

---

> 💡 **The Production Golden Rule:**
> Always combine — **`DEBUG=False` + `SECRET_KEY` from env + `ALLOWED_HOSTS` set + HTTPS enforced + `select_related/prefetch_related` on list views + `@transaction.atomic` on multi-step writes + `collectstatic` run + `check --deploy` passing**.
> These 8 things together make a secure, efficient, production-grade Django application.

---

*Notes prepared based on: Django Official Documentation, Two Scoops of Django, Real Python, GeeksforGeeks, and production best practices (2025–2026)*
