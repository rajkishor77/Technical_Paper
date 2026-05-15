# 🏗️ SOLID Principles — Complete Study Notes
> **After reading this, no concept of SOLID will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is SOLID?](#1-what-is-solid)
2. [Why Do We Need SOLID?](#2-why-do-we-need-solid)
3. [S — Single Responsibility Principle (SRP)](#3-s--single-responsibility-principle-srp)
4. [O — Open/Closed Principle (OCP)](#4-o--openclosed-principle-ocp)
5. [L — Liskov Substitution Principle (LSP)](#5-l--liskov-substitution-principle-lsp)
6. [I — Interface Segregation Principle (ISP)](#6-i--interface-segregation-principle-isp)
7. [D — Dependency Inversion Principle (DIP)](#7-d--dependency-inversion-principle-dip)
8. [SOLID in JavaScript / TypeScript](#8-solid-in-javascript--typescript)
9. [SOLID in Django / Python](#9-solid-in-django--python)
10. [SOLID Violations — How to Spot Them](#10-solid-violations--how-to-spot-them)
11. [SOLID + Design Patterns Connection](#11-solid--design-patterns-connection)
12. [Code Smells That Violate SOLID](#12-code-smells-that-violate-solid)
13. [SOLID vs Other Principles (DRY, KISS, YAGNI)](#13-solid-vs-other-principles-dry-kiss-yagni)
14. [Real-World Project Examples](#14-real-world-project-examples)
15. [Common Interview Questions](#15-common-interview-questions)

---

## 1. What is SOLID?

SOLID is a set of **five object-oriented design principles** introduced by **Robert C. Martin (Uncle Bob)** that guide developers toward writing code that is clean, flexible, and easy to maintain.

### Simple Analogy 🏠
> Think of SOLID as the **5 rules of building a well-organised house**:
> - **S** = Each room has ONE purpose — kitchen only cooks, bedroom only sleeps
> - **O** = You can add new rooms (garage, gym) WITHOUT demolishing existing walls
> - **L** = Any door that fits a standard frame can be used — no surprises
> - **I** = Each light switch only controls WHAT IT NEEDS TO — not the whole house
> - **D** = The house's wiring connects to a STANDARD socket — not one specific brand

### What Each Letter Stands For

| Letter | Principle | One-Line Definition |
|--------|-----------|---------------------|
| **S** | Single Responsibility | A class should have only ONE reason to change |
| **O** | Open/Closed | Open for extension, closed for modification |
| **L** | Liskov Substitution | Subclasses must be usable in place of their parent |
| **I** | Interface Segregation | Don't force classes to implement what they don't need |
| **D** | Dependency Inversion | Depend on abstractions, not concrete implementations |

### Key Points
- Coined by **Robert C. Martin** in his 2000 paper *Design Principles and Design Patterns*
- The acronym **SOLID** was later organized by **Michael Feathers**
- These are **guidelines**, not strict laws — apply them with judgment
- SOLID applies to **Object-Oriented Programming (OOP)** primarily — Python, Java, C#, TypeScript
- Following SOLID leads to code that is: **testable, maintainable, scalable, and readable**

---

## 2. Why Do We Need SOLID?

### The Problem Without SOLID

```
Month 1: Code works fine — small, simple, easy to understand
Month 3: More features added — code is getting complex
Month 6: A simple change breaks 5 other things
Month 12: Nobody wants to touch this code — "just don't break anything"
Month 18: Rewriting from scratch because the code is unmaintainable
```

### What Happens When You Violate SOLID

| Violation | Consequence |
|-----------|-------------|
| One class doing everything | Changing one feature breaks unrelated features |
| Modifying existing code to add features | Previously tested and working code breaks |
| Subclass behaving differently than parent | Runtime surprises and unpredictable bugs |
| One large interface for everything | Classes implement methods they don't use |
| High-level code tightly coupled to details | Swapping a database requires changing 50 files |

### What SOLID Gives You

```
Without SOLID:                          With SOLID:
─────────────────────────────────────   ─────────────────────────────────────
One change breaks many things     →     Isolated changes — one thing at a time
Hard to add new features          →     Add features without touching old code
Tests are fragile and complex     →     Small, focused, easy tests
Can't swap components (DB, Email) →     Swap any component in one place
New developer can't understand    →     Each class has one clear purpose
```

---

## 3. S — Single Responsibility Principle (SRP)

### Definition
> **A class should have only ONE reason to change.**
> In other words: one class = one job.

### Real-Life Analogy
> In a **restaurant**, the chef cooks, the waiter serves, and the cashier handles billing.
> If the chef also had to take orders AND handle billing AND clean tables — one problem in billing would affect cooking, and changing the billing process would require retraining the chef.

### The Problem — Before SRP

```python
# ❌ BAD — One class doing THREE different jobs
class User:
    def __init__(self, name, email):
        self.name = name
        self.email = email

    # Job 1: User data management
    def get_user_info(self):
        return {"name": self.name, "email": self.email}

    # Job 2: Database operations — NOT the User's responsibility!
    def save_to_database(self):
        print(f"INSERT INTO users VALUES ('{self.name}', '{self.email}')")

    # Job 3: Email sending — NOT the User's responsibility!
    def send_welcome_email(self):
        print(f"Sending welcome email to {self.email}")

    # Job 4: Validation — arguably okay, but still could be separate
    def validate_email(self):
        return "@" in self.email

# PROBLEM: If the database changes (MySQL → PostgreSQL),
#          you have to change the User class.
# PROBLEM: If the email service changes (SMTP → SendGrid),
#          you have to change the User class.
# PROBLEM: User has FOUR reasons to change — violates SRP!
```

### The Solution — After SRP

```python
# ✅ GOOD — Each class has exactly ONE job

class User:
    """Responsibility: Represent user data only"""
    def __init__(self, name, email):
        self.name = name
        self.email = email

    def get_info(self):
        return {"name": self.name, "email": self.email}


class UserValidator:
    """Responsibility: Validate user data only"""
    @staticmethod
    def validate_email(email: str) -> bool:
        return "@" in email and "." in email

    @staticmethod
    def validate_name(name: str) -> bool:
        return len(name.strip()) >= 2


class UserRepository:
    """Responsibility: Database operations only"""
    def save(self, user: User):
        print(f"INSERT INTO users VALUES ('{user.name}', '{user.email}')")

    def find_by_email(self, email: str) -> User:
        print(f"SELECT * FROM users WHERE email = '{email}'")


class EmailService:
    """Responsibility: Send emails only"""
    def send_welcome_email(self, user: User):
        print(f"Sending welcome email to {user.email}")

    def send_password_reset(self, user: User, token: str):
        print(f"Sending password reset to {user.email} with token {token}")


# Using them together — each class has its own single reason to change
user = User("Alice", "alice@example.com")

if UserValidator.validate_email(user.email):
    UserRepository().save(user)
    EmailService().send_welcome_email(user)
```

### SRP in a Django App

```python
# ❌ BAD Django view doing too many things
def register_view(request):
    # Parsing request
    name = request.POST.get("name")
    email = request.POST.get("email")
    password = request.POST.get("password")

    # Validation (should be in a Form/Serializer)
    if not email or "@" not in email:
        return JsonResponse({"error": "Invalid email"}, status=400)

    # Business logic (should be in a service layer)
    if User.objects.filter(email=email).exists():
        return JsonResponse({"error": "Email already exists"}, status=400)

    # DB operation (should be in a repository)
    user = User.objects.create_user(name=name, email=email, password=password)

    # Email sending (should be in an email service)
    send_mail("Welcome!", f"Hi {name}", "noreply@app.com", [email])

    return JsonResponse({"message": "Registered successfully"})


# ✅ GOOD — Responsibilities separated
# forms.py — handles validation
class RegisterForm(forms.Form):
    name     = forms.CharField(min_length=2)
    email    = forms.EmailField()
    password = forms.CharField(min_length=8)

# services.py — handles business logic
class UserService:
    def register(self, name, email, password):
        if User.objects.filter(email=email).exists():
            raise ValueError("Email already registered")
        user = User.objects.create_user(username=name, email=email, password=password)
        EmailService().send_welcome(user)
        return user

# views.py — handles ONLY request/response
def register_view(request):
    form = RegisterForm(request.POST)
    if not form.is_valid():
        return JsonResponse({"errors": form.errors}, status=400)
    try:
        user = UserService().register(**form.cleaned_data)
        return JsonResponse({"message": "Registered", "id": user.id})
    except ValueError as e:
        return JsonResponse({"error": str(e)}, status=400)
```

### How to Identify SRP Violations
- A class has **more than one section** that changes for different reasons
- The class name contains **"and"** — e.g., `UserAndEmailManager`
- The class has **many unrelated methods** — saving, validating, formatting, sending
- When you describe what the class does, you use **"and"** more than once
- Writing a unit test requires **mocking many different external dependencies**

### SRP Benefits
- Each class is **small and focused** — easy to understand at a glance
- Changes to the email service don't touch the User class
- Each class can be **tested independently** and easily
- Classes become **reusable** in other contexts

---

## 4. O — Open/Closed Principle (OCP)

### Definition
> **Software entities should be open for extension but closed for modification.**
> Add new features by writing NEW code, not by changing existing working code.

### Real-Life Analogy
> A **smartphone** lets you install new apps anytime — you extend its functionality
> without modifying the phone's operating system. The OS is "closed" for modification
> but "open" for extension through the app model.

### The Problem — Before OCP

```python
# ❌ BAD — Every new payment method requires modifying existing code
class PaymentProcessor:
    def process_payment(self, amount: float, payment_type: str):
        if payment_type == "credit_card":
            print(f"Processing ₹{amount} via Credit Card")
        elif payment_type == "paypal":
            print(f"Processing ₹{amount} via PayPal")
        elif payment_type == "upi":
            print(f"Processing ₹{amount} via UPI")
        # PROBLEM: Adding "crypto" requires modifying this tested, working method!
        # Every new payment method = new risk of breaking existing ones
        elif payment_type == "crypto":
            print(f"Processing ₹{amount} via Crypto")
```

### The Solution — After OCP

```python
# ✅ GOOD — Add new payment methods WITHOUT touching existing code

from abc import ABC, abstractmethod

class PaymentMethod(ABC):
    """Abstract base — the "closed" part that never changes"""
    @abstractmethod
    def process(self, amount: float) -> bool:
        pass

    @abstractmethod
    def get_name(self) -> str:
        pass


class CreditCardPayment(PaymentMethod):
    """Extension — open for adding new methods like this"""
    def process(self, amount: float) -> bool:
        print(f"Charging ₹{amount} to Credit Card")
        return True

    def get_name(self) -> str:
        return "Credit Card"


class PayPalPayment(PaymentMethod):
    def process(self, amount: float) -> bool:
        print(f"Processing ₹{amount} via PayPal")
        return True

    def get_name(self) -> str:
        return "PayPal"


class UPIPayment(PaymentMethod):
    def process(self, amount: float) -> bool:
        print(f"Processing ₹{amount} via UPI")
        return True

    def get_name(self) -> str:
        return "UPI"


# Adding Crypto in future — ZERO changes to existing code!
class CryptoPayment(PaymentMethod):
    def process(self, amount: float) -> bool:
        print(f"Processing ₹{amount} via Crypto Wallet")
        return True

    def get_name(self) -> str:
        return "Crypto"


# PaymentProcessor never needs to change
class PaymentProcessor:
    def __init__(self, payment_method: PaymentMethod):
        self.method = payment_method

    def checkout(self, amount: float):
        success = self.method.process(amount)
        if success:
            print(f"Payment of ₹{amount} via {self.method.get_name()} successful!")
        return success


# Usage — easily swap payment methods
PaymentProcessor(CreditCardPayment()).checkout(999)
PaymentProcessor(UPIPayment()).checkout(1499)
PaymentProcessor(CryptoPayment()).checkout(5000)  # new method — no existing code changed!
```

### OCP with Configuration (Data-Driven Extension)

```python
# Another OCP approach — use a registry instead of if/elif chains

DISCOUNT_STRATEGIES = {}  # registry

def discount_strategy(name):
    """Decorator to register a new strategy"""
    def wrapper(fn):
        DISCOUNT_STRATEGIES[name] = fn
        return fn
    return wrapper

@discount_strategy("student")
def student_discount(price):
    return price * 0.85   # 15% off

@discount_strategy("senior")
def senior_discount(price):
    return price * 0.80   # 20% off

@discount_strategy("premium")
def premium_discount(price):
    return price * 0.70   # 30% off

# Add new strategy without changing any existing code:
@discount_strategy("employee")
def employee_discount(price):
    return price * 0.50   # 50% off

def apply_discount(price, discount_type):
    strategy = DISCOUNT_STRATEGIES.get(discount_type)
    if not strategy:
        raise ValueError(f"Unknown discount type: {discount_type}")
    return strategy(price)

apply_discount(1000, "premium")   # 700
apply_discount(1000, "employee")  # 500
```

### How to Identify OCP Violations
- You see a **long if/elif/else** chain checking "type" or "kind"
- Every time a **new variant is added**, the same function gets a new `elif`
- You need to **modify tested, deployed code** to add a feature
- The same function is modified **over and over** by different developers

### OCP Benefits
- Existing code stays **untouched and stable** — tested code keeps working
- New features are **isolated** in new classes — failures are contained
- Code becomes naturally **pluggable** — swap components without side effects

---

## 5. L — Liskov Substitution Principle (LSP)

### Definition
> **Objects of a subclass should be replaceable with objects of the parent class
> without breaking the application.**
> If class B extends class A, you should be able to use B wherever A is expected — with no surprises.

### Real-Life Analogy
> If every vehicle has a **drive()** method, you should be able to call `drive()` on a Car,
> a Truck, or a Bus and get sensible behaviour. If calling `drive()` on a "NoBrakesCar"
> crashes the system — that subclass violates LSP.

### The Problem — Before LSP

```python
# ❌ BAD — The classic Bird/Ostrich example
class Bird:
    def fly(self):
        print("I am flying!")

class Eagle(Bird):
    def fly(self):
        print("Eagle soaring high!")

class Ostrich(Bird):
    def fly(self):
        raise Exception("Ostriches can't fly!")  # 💥 Breaks the contract!

# This function works for Eagle but crashes for Ostrich!
def make_bird_fly(bird: Bird):
    bird.fly()   # 💥 RuntimeError when bird is an Ostrich

make_bird_fly(Eagle())    # ✅ Works
make_bird_fly(Ostrich())  # ❌ Crashes!
```

### The Solution — After LSP

```python
# ✅ GOOD — Redesign the hierarchy to reflect reality

class Bird:
    """Base bird — all birds share this"""
    def __init__(self, name: str):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")

    def breathe(self):
        print(f"{self.name} is breathing")


class FlyingBird(Bird):
    """Only for birds that actually fly"""
    def fly(self):
        print(f"{self.name} is flying!")


class RunningBird(Bird):
    """For birds that run instead"""
    def run(self):
        print(f"{self.name} is running fast!")


class Eagle(FlyingBird):
    def fly(self):
        print(f"{self.name} is soaring at 300 feet!")


class Parrot(FlyingBird):
    def fly(self):
        print(f"{self.name} is flying and talking!")


class Ostrich(RunningBird):
    def run(self):
        print(f"{self.name} is running at 70 km/h!")


class Penguin(RunningBird):
    def swim(self):   # penguins swim, not run — further specialization is fine
        print(f"{self.name} is swimming!")


# Now these functions can safely use LSP
def let_bird_fly(bird: FlyingBird):
    bird.fly()   # ✅ Guaranteed to work — all FlyingBirds can fly

def let_bird_run(bird: RunningBird):
    bird.run()   # ✅ Guaranteed to work — all RunningBirds can run

let_bird_fly(Eagle("Eagle"))    # ✅ Eagle is soaring...
let_bird_fly(Parrot("Parrot"))  # ✅ Parrot is flying...
let_bird_run(Ostrich("Ostrich"))  # ✅ Ostrich is running...
```

### Another Example — Rectangle/Square (Classic LSP Trap)

```python
# ❌ BAD — Square is NOT a proper subtype of Rectangle in code
class Rectangle:
    def __init__(self, width, height):
        self.width  = width
        self.height = height

    def set_width(self, w):
        self.width = w

    def set_height(self, h):
        self.height = h

    def area(self):
        return self.width * self.height


class Square(Rectangle):
    def set_width(self, w):
        self.width  = w     # Square must keep both sides equal
        self.height = w     # This OVERRIDES Rectangle's behaviour!

    def set_height(self, h):
        self.width  = h     # Again, both sides change!
        self.height = h


def test_rectangle(rect: Rectangle):
    rect.set_width(5)
    rect.set_height(10)
    assert rect.area() == 50, f"Expected 50, got {rect.area()}"
    print("Test passed!")

test_rectangle(Rectangle(0, 0))  # ✅ Passes — area = 50
test_rectangle(Square(0, 0))     # ❌ FAILS — area = 100 (Square changed width when height was set)


# ✅ GOOD — Don't inherit Square from Rectangle; use a common abstraction
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self) -> float:
        pass

class Rectangle(Shape):
    def __init__(self, width: float, height: float):
        self.width  = width
        self.height = height

    def area(self) -> float:
        return self.width * self.height

class Square(Shape):
    def __init__(self, side: float):
        self.side = side

    def area(self) -> float:
        return self.side ** 2

# Both are Shapes — and area() works correctly for each
shapes = [Rectangle(5, 10), Square(5)]
for shape in shapes:
    print(shape.area())   # 50, 25 ✅
```

### LSP Rules — What Subclasses Must Obey

| Rule | Meaning |
|------|---------|
| **Preconditions** | Subclass must accept AT LEAST what the parent accepts (not more strict) |
| **Postconditions** | Subclass must return AT LEAST what the parent guarantees (not weaker) |
| **Exceptions** | Subclass must NOT throw NEW exceptions the parent doesn't throw |
| **Behaviour** | Subclass must not change the meaning of the parent's methods |

### How to Identify LSP Violations
- A subclass method **raises NotImplementedError or an Exception**
- You write `if isinstance(obj, SubClass):` to handle a special case
- A subclass **ignores or weakens** a method from the parent
- Unit tests pass for the parent but **fail for the subclass**
- The subclass **overrides a method to do nothing** (`pass`)

---

## 6. I — Interface Segregation Principle (ISP)

### Definition
> **No client should be forced to depend on methods it does not use.**
> Make interfaces small and focused — many specific interfaces are better than one big general interface.

### Real-Life Analogy
> A **basic TV remote** has power, volume, and channel buttons.
> An **advanced remote** also has Netflix, voice control, and smart home buttons.
> If everyone was given only one remote with ALL possible buttons, most buttons would be useless
> for most people — that's an ISP violation.

### The Problem — Before ISP

```python
# ❌ BAD — One fat interface forces all classes to implement everything
from abc import ABC, abstractmethod

class Worker(ABC):
    @abstractmethod
    def work(self):
        pass

    @abstractmethod
    def eat(self):    # 🤔 Why would a Robot eat?
        pass

    @abstractmethod
    def sleep(self):  # 🤔 Why would a Robot sleep?
        pass

    @abstractmethod
    def get_paid(self):  # 🤔 Not all workers get paid the same way
        pass


class HumanWorker(Worker):
    def work(self):    print("Human working")
    def eat(self):     print("Human eating lunch")
    def sleep(self):   print("Human sleeping")
    def get_paid(self): print("Human receives salary")


class RobotWorker(Worker):
    def work(self):    print("Robot working 24/7")
    def eat(self):     raise NotImplementedError("Robots don't eat!")   # 💥
    def sleep(self):   raise NotImplementedError("Robots don't sleep!") # 💥
    def get_paid(self): raise NotImplementedError("Robots aren't paid!") # 💥

# PROBLEM: RobotWorker is forced to implement eat(), sleep(), get_paid()
# even though these make no sense for a robot.
```

### The Solution — After ISP

```python
# ✅ GOOD — Split into small, focused interfaces

from abc import ABC, abstractmethod

class Workable(ABC):
    """Anyone who can do work"""
    @abstractmethod
    def work(self) -> None:
        pass


class Eatable(ABC):
    """Anyone who needs to eat"""
    @abstractmethod
    def eat(self) -> None:
        pass


class Sleepable(ABC):
    """Anyone who needs to sleep"""
    @abstractmethod
    def sleep(self) -> None:
        pass


class Payable(ABC):
    """Anyone who receives payment"""
    @abstractmethod
    def get_paid(self, amount: float) -> None:
        pass


# Human worker needs ALL four — implement all four interfaces
class HumanWorker(Workable, Eatable, Sleepable, Payable):
    def __init__(self, name: str):
        self.name = name

    def work(self):
        print(f"{self.name} is working")

    def eat(self):
        print(f"{self.name} is eating")

    def sleep(self):
        print(f"{self.name} is sleeping")

    def get_paid(self, amount: float):
        print(f"{self.name} received ₹{amount}")


# Robot only needs Workable — no forced useless methods
class RobotWorker(Workable):
    def __init__(self, model: str):
        self.model = model

    def work(self):
        print(f"Robot {self.model} is working 24/7")


# Freelancer works and gets paid but doesn't have a fixed sleep schedule tracked by the system
class FreelancerWorker(Workable, Payable):
    def __init__(self, name: str):
        self.name = name

    def work(self):
        print(f"Freelancer {self.name} is working")

    def get_paid(self, amount: float):
        print(f"Freelancer {self.name} received ₹{amount} via bank transfer")


# Everything works cleanly — no useless NotImplementedError anywhere!
human     = HumanWorker("Alice")
robot     = RobotWorker("R2D2")
freelancer = FreelancerWorker("Bob")

human.work()         # ✅
robot.work()         # ✅
freelancer.work()    # ✅
human.eat()          # ✅
human.get_paid(50000) # ✅
freelancer.get_paid(25000)  # ✅
```

### ISP in API Design (Practical Example)

```python
# ❌ BAD — One large repository interface for everything
class UserRepository(ABC):
    @abstractmethod
    def find_by_id(self, id): pass
    @abstractmethod
    def save(self, user): pass
    @abstractmethod
    def delete(self, id): pass
    @abstractmethod
    def find_all(self): pass
    @abstractmethod
    def find_by_email(self, email): pass
    @abstractmethod
    def update_password(self, id, pw): pass
    @abstractmethod
    def get_login_history(self, id): pass
    @abstractmethod
    def export_to_csv(self): pass      # 🤔 unrelated to data access!
    @abstractmethod
    def send_report(self, email): pass  # 🤔 definitely doesn't belong here!


# ✅ GOOD — Segregated interfaces
class UserReader(ABC):
    @abstractmethod
    def find_by_id(self, id): pass
    @abstractmethod
    def find_by_email(self, email): pass
    @abstractmethod
    def find_all(self): pass

class UserWriter(ABC):
    @abstractmethod
    def save(self, user): pass
    @abstractmethod
    def update_password(self, id, pw): pass

class UserDeleter(ABC):
    @abstractmethod
    def delete(self, id): pass

class UserAudit(ABC):
    @abstractmethod
    def get_login_history(self, id): pass

# A read-only service only needs UserReader — no forced dependency on write/delete
class UserReadOnlyService:
    def __init__(self, repo: UserReader):
        self.repo = repo
```

### How to Identify ISP Violations
- A class has **methods that raise `NotImplementedError` or `pass`**
- You see `# Not applicable for this class` comments
- Classes implement an interface but **only use half the methods**
- One interface has **methods from unrelated concerns** grouped together
- Adding a new method to an interface forces **changes in many classes** that don't need it

---

## 7. D — Dependency Inversion Principle (DIP)

### Definition
> **High-level modules should not depend on low-level modules. Both should depend on abstractions.**
> Abstractions should not depend on details. Details should depend on abstractions.

### Real-Life Analogy
> A **power socket** (abstraction) lets you plug in any device — laptop, phone, fan.
> The socket doesn't know or care what device you plug in.
> If the socket was designed specifically for one brand's laptop, you couldn't use anything else.
> DIP says: design to the socket (abstraction), not to the specific device (concrete class).

### The Problem — Before DIP

```python
# ❌ BAD — High-level module hardcoded to low-level implementation
class MySQLDatabase:
    def get_connection(self):
        print("Connected to MySQL database")

    def query(self, sql: str):
        print(f"MySQL Query: {sql}")
        return []


class UserService:
    def __init__(self):
        # PROBLEM: UserService is tightly coupled to MySQLDatabase
        # If we want to switch to PostgreSQL or MongoDB, we MUST change UserService
        # If we want to test UserService, we MUST have MySQL running!
        self.db = MySQLDatabase()

    def get_user(self, user_id: int):
        results = self.db.query(f"SELECT * FROM users WHERE id = {user_id}")
        return results

# Testing UserService requires a real MySQL database — fragile and slow!
```

### The Solution — After DIP

```python
# ✅ GOOD — Both high-level and low-level depend on an abstraction

from abc import ABC, abstractmethod
from typing import List, Dict, Any

# ─── The Abstraction (the "socket") ──────────────────────────────────────
class Database(ABC):
    """High-level module and low-level modules both depend on THIS"""

    @abstractmethod
    def query(self, sql: str) -> List[Dict]:
        pass

    @abstractmethod
    def execute(self, sql: str) -> bool:
        pass


# ─── Low-level implementations (the "devices") ───────────────────────────
class MySQLDatabase(Database):
    def query(self, sql: str) -> List[Dict]:
        print(f"[MySQL] Executing: {sql}")
        return [{"id": 1, "name": "Alice"}]  # simulated

    def execute(self, sql: str) -> bool:
        print(f"[MySQL] Executing: {sql}")
        return True


class PostgreSQLDatabase(Database):
    def query(self, sql: str) -> List[Dict]:
        print(f"[PostgreSQL] Executing: {sql}")
        return [{"id": 1, "name": "Alice"}]

    def execute(self, sql: str) -> bool:
        print(f"[PostgreSQL] Executing: {sql}")
        return True


class MockDatabase(Database):
    """For testing — no real database needed"""
    def __init__(self):
        self.data = [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]

    def query(self, sql: str) -> List[Dict]:
        return self.data

    def execute(self, sql: str) -> bool:
        return True


# ─── High-level module (depends only on abstraction) ─────────────────────
class UserService:
    def __init__(self, db: Database):   # accepts ANY Database
        self.db = db                    # depends on abstraction, not implementation

    def get_user(self, user_id: int) -> Dict:
        results = self.db.query(f"SELECT * FROM users WHERE id = {user_id}")
        return results[0] if results else {}

    def create_user(self, name: str, email: str) -> bool:
        return self.db.execute(f"INSERT INTO users (name, email) VALUES ('{name}', '{email}')")


# Usage — swap database with NO changes to UserService!
mysql_service    = UserService(MySQLDatabase())
postgres_service = UserService(PostgreSQLDatabase())
test_service     = UserService(MockDatabase())     # ✅ Test without real DB!

mysql_service.get_user(1)
test_service.get_user(1)    # fast, isolated test!
```

### DIP with Email Service (Real-World Example)

```python
# ✅ GOOD — NotificationService depends on abstraction, not concrete email provider

from abc import ABC, abstractmethod

class EmailProvider(ABC):
    @abstractmethod
    def send(self, to: str, subject: str, body: str) -> bool:
        pass


class SMTPEmailProvider(EmailProvider):
    def send(self, to, subject, body) -> bool:
        print(f"[SMTP] Sending '{subject}' to {to}")
        return True


class SendGridProvider(EmailProvider):
    def send(self, to, subject, body) -> bool:
        print(f"[SendGrid API] Sending '{subject}' to {to}")
        return True


class MailgunProvider(EmailProvider):
    def send(self, to, subject, body) -> bool:
        print(f"[Mailgun API] Sending '{subject}' to {to}")
        return True


class MockEmailProvider(EmailProvider):
    """For testing — no real emails sent"""
    def __init__(self):
        self.sent_emails = []

    def send(self, to, subject, body) -> bool:
        self.sent_emails.append({"to": to, "subject": subject})
        return True


class NotificationService:
    def __init__(self, email_provider: EmailProvider):
        self.email = email_provider    # depends on abstraction

    def notify_user(self, user_email: str, message: str) -> bool:
        return self.email.send(user_email, "Notification", message)

    def send_welcome(self, user_email: str, name: str) -> bool:
        return self.email.send(user_email, "Welcome!", f"Hello {name}, welcome!")


# Easily switch providers
NotificationService(SMTPEmailProvider()).send_welcome("alice@example.com", "Alice")
NotificationService(SendGridProvider()).send_welcome("alice@example.com", "Alice")

# Test without sending real emails
mock    = MockEmailProvider()
service = NotificationService(mock)
service.send_welcome("test@test.com", "Test User")
print(mock.sent_emails)   # [{'to': 'test@test.com', 'subject': 'Welcome!'}]
```

### How to Identify DIP Violations
- A class **creates its own dependencies** inside `__init__`: `self.db = MySQLDatabase()`
- High-level classes **import concrete classes** directly
- Switching one component (database, email) requires **changing many files**
- Unit tests require **setting up real databases, real email servers, real file systems**
- The phrase **"tightly coupled"** applies to any two classes in the system

### Dependency Injection (DI) — How to Apply DIP

```python
# Three ways to inject dependencies (all are forms of DIP)

# 1. Constructor Injection (preferred — explicit, testable)
class OrderService:
    def __init__(self, db: Database, emailer: EmailProvider, logger: Logger):
        self.db      = db
        self.emailer = emailer
        self.logger  = logger


# 2. Method Injection (for optional/per-call dependencies)
class ReportGenerator:
    def generate(self, data, formatter: ReportFormatter):
        return formatter.format(data)


# 3. Property Injection (for optional defaults)
class DataProcessor:
    def __init__(self):
        self.logger = ConsoleLogger()     # default

    def set_logger(self, logger: Logger):
        self.logger = logger              # can be replaced
```

---

## 8. SOLID in JavaScript / TypeScript

```typescript
// TypeScript interfaces make SOLID natural to apply

// ─── SRP in TypeScript ────────────────────────────────────────────────────
class UserEntity {
  constructor(public name: string, public email: string) {}
}

class UserRepository {
  save(user: UserEntity): void { console.log(`Saving ${user.name}`) }
  findById(id: number): UserEntity | null { return null }
}

class EmailService {
  sendWelcome(user: UserEntity): void { console.log(`Welcome email to ${user.email}`) }
}

class UserRegistrationService {
  constructor(
    private repo: UserRepository,
    private email: EmailService
  ) {}

  register(name: string, emailAddr: string): UserEntity {
    const user = new UserEntity(name, emailAddr)
    this.repo.save(user)
    this.email.sendWelcome(user)
    return user
  }
}


// ─── OCP in TypeScript ────────────────────────────────────────────────────
interface DiscountStrategy {
  apply(price: number): number
  label: string
}

class StudentDiscount implements DiscountStrategy {
  label = "Student"
  apply(price: number): number { return price * 0.85 }
}

class PremiumDiscount implements DiscountStrategy {
  label = "Premium"
  apply(price: number): number { return price * 0.70 }
}

// Add new discount — no existing code changes
class FlashSaleDiscount implements DiscountStrategy {
  label = "Flash Sale"
  apply(price: number): number { return price * 0.50 }
}

class PriceCalculator {
  calculate(price: number, strategy: DiscountStrategy): number {
    const final = strategy.apply(price)
    console.log(`${strategy.label}: ₹${price} → ₹${final}`)
    return final
  }
}


// ─── DIP in TypeScript ────────────────────────────────────────────────────
interface IDatabase {
  query(sql: string): Promise<any[]>
  execute(sql: string): Promise<boolean>
}

interface IEmailProvider {
  send(to: string, subject: string, body: string): Promise<boolean>
}

class UserService {
  constructor(
    private db: IDatabase,           // depends on interface
    private email: IEmailProvider    // depends on interface
  ) {}

  async createUser(name: string, emailAddr: string) {
    await this.db.execute(`INSERT INTO users VALUES ('${name}', '${emailAddr}')`)
    await this.email.send(emailAddr, "Welcome!", `Hello ${name}!`)
  }
}


// ─── JavaScript (without TypeScript) — using duck typing ─────────────────
// In plain JavaScript, "interfaces" are just conventions/comments
// Use JSDoc to document expected shapes

/**
 * @typedef {Object} PaymentMethod
 * @property {function(number): Promise<boolean>} process
 * @property {function(): string} getName
 */

class StripePayment {
  async process(amount) { console.log(`Stripe: ₹${amount}`); return true }
  getName() { return "Stripe" }
}

class RazorpayPayment {
  async process(amount) { console.log(`Razorpay: ₹${amount}`); return true }
  getName() { return "Razorpay" }
}

// Works with any object that has .process() and .getName() — duck typing
async function checkout(cart, paymentMethod) {
  const total = cart.reduce((sum, item) => sum + item.price, 0)
  const success = await paymentMethod.process(total)
  console.log(`Paid ₹${total} via ${paymentMethod.getName()}: ${success}`)
}
```

---

## 9. SOLID in Django / Python

```python
# ─── SRP: Service Layer Pattern ──────────────────────────────────────────
# views.py — ONLY handles HTTP
class OrderCreateView(APIView):
    def post(self, request):
        serializer = OrderSerializer(data=request.data)
        if serializer.is_valid():
            try:
                order = OrderService().create_order(
                    user=request.user,
                    items=serializer.validated_data["items"]
                )
                return Response({"order_id": order.id}, status=201)
            except InsufficientStockError as e:
                return Response({"error": str(e)}, status=400)
        return Response(serializer.errors, status=400)


# services.py — ONLY handles business logic
class OrderService:
    def create_order(self, user, items):
        with transaction.atomic():
            order = Order.objects.create(user=user, status="pending")
            for item_data in items:
                product = Product.objects.select_for_update().get(id=item_data["product_id"])
                if product.stock < item_data["quantity"]:
                    raise InsufficientStockError(f"{product.name} out of stock")
                product.stock -= item_data["quantity"]
                product.save()
                OrderItem.objects.create(order=order, product=product, quantity=item_data["quantity"])
            NotificationService().send_order_confirmation(user, order)
            return order


# ─── OCP: Strategy Pattern for Reports ───────────────────────────────────
from abc import ABC, abstractmethod

class ReportFormatter(ABC):
    @abstractmethod
    def format(self, data: list) -> str:
        pass

class JSONFormatter(ReportFormatter):
    def format(self, data):
        import json
        return json.dumps(data, indent=2)

class CSVFormatter(ReportFormatter):
    def format(self, data):
        import csv, io
        output = io.StringIO()
        if data:
            writer = csv.DictWriter(output, fieldnames=data[0].keys())
            writer.writeheader()
            writer.writerows(data)
        return output.getvalue()

class HTMLFormatter(ReportFormatter):
    def format(self, data):
        rows = "".join(f"<tr>{''.join(f'<td>{v}</td>' for v in row.values())}</tr>" for row in data)
        return f"<table>{rows}</table>"

class ReportService:
    def __init__(self, formatter: ReportFormatter):
        self.formatter = formatter

    def generate(self, queryset):
        data = list(queryset.values())
        return self.formatter.format(data)


# ─── DIP: Repository Pattern ──────────────────────────────────────────────
from abc import ABC, abstractmethod

class UserRepositoryInterface(ABC):
    @abstractmethod
    def get_by_id(self, id: int): pass

    @abstractmethod
    def get_by_email(self, email: str): pass

    @abstractmethod
    def save(self, user) -> None: pass


class DjangoUserRepository(UserRepositoryInterface):
    def get_by_id(self, id):
        return User.objects.get(id=id)

    def get_by_email(self, email):
        return User.objects.filter(email=email).first()

    def save(self, user):
        user.save()


class MockUserRepository(UserRepositoryInterface):
    """For testing — no database"""
    def __init__(self):
        self.users = {}

    def get_by_id(self, id):
        return self.users.get(id)

    def get_by_email(self, email):
        return next((u for u in self.users.values() if u.email == email), None)

    def save(self, user):
        self.users[user.id] = user


class ProfileService:
    def __init__(self, user_repo: UserRepositoryInterface):
        self.user_repo = user_repo    # depends on abstraction

    def get_profile(self, user_id: int):
        user = self.user_repo.get_by_id(user_id)
        return {"name": user.name, "email": user.email} if user else None
```

---

## 10. SOLID Violations — How to Spot Them

```
SRP Violations — Warning Signs:
─────────────────────────────────────────────────────────────────────
✗ Class/module name contains "and": UserAndEmailManager
✗ A class has 10+ methods that do unrelated things
✗ Changing the email logic requires touching the User model
✗ A single unit test needs to mock database AND email AND file system

OCP Violations — Warning Signs:
─────────────────────────────────────────────────────────────────────
✗ Long if/elif chains checking "type", "kind", "mode" variables
✗ The same function is opened and edited every time a new variant is added
✗ Comments like "# Add new payment types here" inside existing methods
✗ A bug in new code can break old, tested functionality

LSP Violations — Warning Signs:
─────────────────────────────────────────────────────────────────────
✗ Subclass method raises NotImplementedError or does nothing (pass)
✗ Code checks isinstance(obj, SubClass) to skip certain behaviour
✗ Parent class tests pass but subclass tests fail
✗ Method override weakens a behaviour (returns None when parent returns data)

ISP Violations — Warning Signs:
─────────────────────────────────────────────────────────────────────
✗ Interface has methods that make no sense for some implementors
✗ Many implementations have "raise NotImplementedError" stubs
✗ Adding a method to an interface forces changes in 20 unrelated classes
✗ Class implements an interface but leaves half the methods empty

DIP Violations — Warning Signs:
─────────────────────────────────────────────────────────────────────
✗ High-level class imports and instantiates low-level class directly
✗ Unit tests require real database / real email server / real files
✗ Switching a database requires changing 15 different files
✗ __init__ contains: self.db = MySQLDatabase() (concrete, not injected)
```

---

## 11. SOLID + Design Patterns Connection

SOLID principles and Design Patterns reinforce each other. Here's how they connect:

| SOLID Principle | Related Design Patterns |
|-----------------|------------------------|
| **SRP** | Facade (hide complex subsystems), Command (separate action from invoker) |
| **OCP** | Strategy (swap algorithms), Decorator (add behaviour without changing), Observer |
| **LSP** | Template Method (override safely), Factory Method (subclasses decide implementation) |
| **ISP** | Adapter (wrap to match interface), Proxy (control access to an interface) |
| **DIP** | Dependency Injection, Factory, Abstract Factory, Service Locator |

```python
# OCP + Strategy Pattern together
class SortStrategy(ABC):
    @abstractmethod
    def sort(self, data: list) -> list:
        pass

class BubbleSort(SortStrategy):
    def sort(self, data): return sorted(data)   # simplified

class QuickSort(SortStrategy):
    def sort(self, data): return sorted(data)   # simplified

class MergeSort(SortStrategy):
    def sort(self, data): return sorted(data)   # simplified

class Sorter:
    def __init__(self, strategy: SortStrategy):
        self.strategy = strategy                # DIP — depends on abstraction

    def sort(self, data):
        return self.strategy.sort(data)

# Open for extension (add RadixSort) — closed for modification (Sorter never changes)
```

---

## 12. Code Smells That Violate SOLID

```
GOD CLASS (violates SRP)
────────────────────────────────────────────────────────────────────────
One class that knows and does everything.
Signs: 500+ line class, dozens of unrelated methods, imported everywhere.
Fix: Break into multiple focused classes.


SHOTGUN SURGERY (violates SRP)
────────────────────────────────────────────────────────────────────────
One change requires editing many different files.
Signs: "I need to update this in 5 places" conversations.
Fix: Consolidate related responsibilities into one place.


SWITCH/IF-ELIF CHAINS (violates OCP)
────────────────────────────────────────────────────────────────────────
Long if/elif chains that keep growing.
Signs: Every new feature adds another elif to the same block.
Fix: Use polymorphism (Strategy, Command patterns).


REFUSED BEQUEST (violates LSP)
────────────────────────────────────────────────────────────────────────
Subclass inherits but refuses to use parent's methods.
Signs: raise NotImplementedError, or override with pass.
Fix: Redesign the inheritance hierarchy. Prefer composition.


FAT INTERFACE (violates ISP)
────────────────────────────────────────────────────────────────────────
One large interface with methods for every possible consumer.
Signs: Implementing classes have many stub/empty methods.
Fix: Split into role-based interfaces.


CONCRETE CLASS DEPENDENCY (violates DIP)
────────────────────────────────────────────────────────────────────────
High-level code directly instantiates low-level concrete classes.
Signs: import MySQLDatabase, self.db = MySQLDatabase() in business logic.
Fix: Inject dependencies through constructors or factory methods.
```

---

## 13. SOLID vs Other Principles (DRY, KISS, YAGNI)

| Principle | Full Form | What It Says | Relation to SOLID |
|-----------|-----------|--------------|-------------------|
| **DRY** | Don't Repeat Yourself | Every piece of knowledge should have one authoritative location | SRP helps avoid duplication by giving each thing one home |
| **KISS** | Keep It Simple, Stupid | Prefer simple solutions over complex ones | SOLID adds structure but shouldn't create unnecessary complexity |
| **YAGNI** | You Aren't Gonna Need It | Don't build features until you actually need them | Don't over-apply OCP by creating abstractions for features that may never exist |
| **SoC** | Separation of Concerns | Different concerns should be in different sections of code | SRP is the class-level application of SoC |

```
When to apply SOLID vs when NOT to:

✅ Apply SOLID when:
   - The system will grow and change over time
   - Multiple developers are working on the codebase
   - You are writing library or framework code
   - The code needs thorough unit testing
   - Business rules are complex and changing

⚠️ Be careful with SOLID when:
   - Writing a quick prototype or throwaway script
   - The application is small and will never grow
   - Over-engineering adds complexity without real benefit
   - YAGNI applies — you're designing for hypothetical future needs

SOLID + KISS: Add abstraction only when you have a real, current need — not speculatively.
SOLID + DRY: Let DRY guide WHERE things live; let SOLID guide HOW they connect.
```

---

## 14. Real-World Project Examples

### E-Commerce Application Applying All 5 Principles

```python
from abc import ABC, abstractmethod
from typing import List
from dataclasses import dataclass

# ─── Domain Models (SRP: each class = one thing) ──────────────────────────
@dataclass
class Product:
    id: int
    name: str
    price: float
    stock: int

@dataclass
class CartItem:
    product: Product
    quantity: int

@dataclass
class Order:
    id: int
    items: List[CartItem]
    total: float
    status: str


# ─── Abstractions — both high and low level depend on these (DIP) ──────────
class PaymentGateway(ABC):          # ISP: focused interface
    @abstractmethod
    def charge(self, amount: float, token: str) -> bool: pass

class InventoryRepository(ABC):     # ISP: focused interface
    @abstractmethod
    def reduce_stock(self, product_id: int, qty: int) -> bool: pass

class NotificationSender(ABC):      # ISP: focused interface
    @abstractmethod
    def send(self, user_email: str, message: str) -> None: pass

class DiscountStrategy(ABC):        # OCP: extend without modifying
    @abstractmethod
    def apply(self, total: float) -> float: pass


# ─── Concrete Implementations ─────────────────────────────────────────────
class StripeGateway(PaymentGateway):
    def charge(self, amount, token):
        print(f"[Stripe] Charged ₹{amount}")
        return True

class RazorpayGateway(PaymentGateway):
    def charge(self, amount, token):
        print(f"[Razorpay] Charged ₹{amount}")
        return True

class DatabaseInventory(InventoryRepository):
    def reduce_stock(self, product_id, qty):
        print(f"[DB] Reduced stock of product {product_id} by {qty}")
        return True

class EmailNotification(NotificationSender):
    def send(self, user_email, message):
        print(f"[Email] → {user_email}: {message}")

class NoDiscount(DiscountStrategy):
    def apply(self, total): return total

class PremiumDiscount(DiscountStrategy):    # OCP: add new discount — zero change to Order
    def apply(self, total): return total * 0.80

class VIPDiscount(DiscountStrategy):        # OCP: add another — zero change to Order
    def apply(self, total): return total * 0.60


# ─── Service Layer (SRP: one job — orchestrate order creation) ─────────────
class OrderService:
    def __init__(
        self,
        payment:   PaymentGateway,        # DIP: depends on abstraction
        inventory: InventoryRepository,   # DIP: depends on abstraction
        notifier:  NotificationSender,    # DIP: depends on abstraction
        discount:  DiscountStrategy       # DIP + OCP: swap discount strategy
    ):
        self.payment   = payment
        self.inventory = inventory
        self.notifier  = notifier
        self.discount  = discount

    def place_order(self, user_email: str, items: List[CartItem], payment_token: str) -> Order:
        # Calculate total
        raw_total = sum(item.product.price * item.quantity for item in items)
        final_total = self.discount.apply(raw_total)

        # Charge payment
        if not self.payment.charge(final_total, payment_token):
            raise ValueError("Payment failed")

        # Update inventory
        for item in items:
            if not self.inventory.reduce_stock(item.product.id, item.quantity):
                raise ValueError(f"Stock update failed for {item.product.name}")

        # Create order object
        order = Order(id=1, items=items, total=final_total, status="confirmed")

        # Notify user
        self.notifier.send(user_email, f"Order #{order.id} confirmed — ₹{final_total}")

        return order


# ─── Usage — easily swap ANY component without changing OrderService ───────
premium_service = OrderService(
    payment   = StripeGateway(),       # swap to RazorpayGateway() anytime
    inventory = DatabaseInventory(),
    notifier  = EmailNotification(),
    discount  = PremiumDiscount()      # swap to VIPDiscount() for different users
)

cart = [CartItem(Product(1, "Laptop", 49999, 10), 1)]
premium_service.place_order("alice@example.com", cart, "tok_visa_123")
```

---

## 15. Common Interview Questions

### Q1: What are the SOLID principles and why are they important?
**Answer:** SOLID is a set of 5 object-oriented design principles — Single Responsibility (one class, one job), Open/Closed (extend without modifying), Liskov Substitution (subclasses replace parents safely), Interface Segregation (small focused interfaces), and Dependency Inversion (depend on abstractions not concrete classes). They are important because they make code easier to understand (each class has a clear purpose), safer to change (isolated changes don't cascade), easier to test (small focused classes with injected dependencies), and easier to extend (add new features without breaking old ones). Without SOLID, codebases become tangled webs where every change breaks something unrelated.

---

### Q2: What is the Single Responsibility Principle? Give a real-world example.
**Answer:** SRP says a class should have only one reason to change — meaning it should do only one job. A class that handles user data, saves to a database, sends emails, and validates inputs has four reasons to change: if the database changes, if the email service changes, if validation rules change, or if user data structure changes. The fix is to split it: User class for data, UserRepository for database, EmailService for emails, UserValidator for validation. A real-world analogy is a restaurant — the chef cooks, the waiter serves, the cashier bills. If the chef also handled billing, a change to billing software would disrupt the kitchen.

---

### Q3: What is the Open/Closed Principle? How do you implement it?
**Answer:** OCP says software should be open for extension but closed for modification — you add new features by writing new code, not by changing existing tested code. The classic sign of a violation is an if/elif chain that keeps growing every time a new variant is added. The fix is to use polymorphism — define an abstract base class or interface with the expected behaviour, then create new concrete classes for each variant. Adding a new payment method means creating a new class, not editing the existing payment processor. This is typically implemented using the Strategy, Decorator, or Observer design patterns.

---

### Q4: What is the Liskov Substitution Principle? What is the classic example of its violation?
**Answer:** LSP says that objects of a subclass should be usable wherever the parent class is expected, without breaking the application. The classic violation is the Bird/Ostrich problem — if Bird has a `fly()` method and Ostrich extends Bird but overrides `fly()` to throw an exception, any code that calls `fly()` on a Bird will crash when given an Ostrich. The fix is to redesign the hierarchy: separate FlyingBird and NonFlyingBird, so Ostrich only inherits from NonFlyingBird. The Rectangle/Square problem is another classic: a Square inheriting from Rectangle violates LSP because setting width and height independently — which is valid for Rectangle — produces wrong results for Square.

---

### Q5: What is the Dependency Inversion Principle? How does it differ from Dependency Injection?
**Answer:** DIP is a design principle — it says high-level modules (business logic) should not depend on low-level modules (database, email, file system). Both should depend on abstractions (interfaces or abstract classes). This makes high-level logic independent of implementation details. Dependency Injection (DI) is the most common pattern for implementing DIP — you inject the dependency (the concrete class) from outside, usually through the constructor, instead of creating it inside the class. DIP says WHAT to do (depend on abstractions); DI says HOW to do it (pass the dependency in). For example, `UserService(db: DatabaseInterface)` — UserService depends on the interface (DIP), and the concrete database is passed in from outside (DI).

---

### Q6: What is the difference between Interface Segregation and Single Responsibility?
**Answer:** They operate at different levels. SRP is about classes — a class should have only one reason to change and do only one job. ISP is about interfaces — an interface should only contain methods that all its implementors actually need. A class can follow SRP (it has one clear job) but still violate ISP (it's forced to implement methods from a fat interface that it doesn't use). For example, a SimplePrinter class has one job (printing) — that's SRP. But if it must implement `scan()` and `fax()` from a large Worker interface even though it can't do those things, that's an ISP violation. The fix is to split the interface into Printable, Scannable, and Faxable.

---

### Q7: How do you apply SOLID principles in a real Django project?
**Answer:** In Django, SRP is applied by separating concerns across layers — views handle only HTTP request/response, serializers handle validation and data shaping, a services layer holds business logic, and models handle data representation. OCP is applied in the services layer using the Strategy pattern for things like discount calculation, report formatting, or notification delivery — new strategies are added as new classes without changing existing ones. DIP is applied by injecting service and repository dependencies rather than importing concrete classes directly inside business logic. ISP is applied by keeping Django REST Framework serializers and viewsets focused — a serializer for reading doesn't also handle writes if they have different shapes.

---

### Q8: Can you violate SOLID in the right situations?
**Answer:** Yes — SOLID principles are guidelines, not laws. Over-applying them can lead to over-engineering: unnecessary abstractions, complex class hierarchies, and dozens of files for trivial logic. YAGNI (You Aren't Gonna Need It) and KISS (Keep It Simple) complement SOLID. For a small script or a simple one-off utility, rigidly applying SRP and DIP adds complexity without value. For prototypes and MVPs, moving fast with simpler code is often the right choice — you can refactor toward SOLID once the design stabilizes. The key is judgment: apply SOLID proportionally to the scale, team size, lifespan, and complexity of the project.

---

## 🔄 SOLID — At a Glance

```
S — Single Responsibility
     One class → One job → One reason to change
     Fix: Split large classes into small focused ones

O — Open/Closed
     Add new features → New class → Never modify working code
     Fix: Use abstract base classes + polymorphism instead of if/elif chains

L — Liskov Substitution
     Subclass → Must work everywhere parent works → No surprises
     Fix: Redesign hierarchy — don't inherit if you can't honour the contract

I — Interface Segregation
     Interface → Only what implementors actually need → No forced stubs
     Fix: Split fat interfaces into small role-based interfaces

D — Dependency Inversion
     High-level → Depends on abstraction → Not on concrete classes
     Fix: Inject dependencies (constructor injection) + use abstract classes
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| SOLID | 5 OOP design principles for clean, maintainable, testable code |
| **S** — SRP | One class, one job, one reason to change |
| **O** — OCP | Extend with new classes — never edit working code |
| **L** — LSP | Subclass must work wherever parent is expected — no surprises |
| **I** — ISP | Small focused interfaces — don't force useless method implementations |
| **D** — DIP | Depend on abstractions — inject concrete classes from outside |
| God Class | SRP violation — one class doing everything |
| if/elif chain | OCP violation — should be polymorphism instead |
| NotImplementedError in subclass | LSP violation — wrong hierarchy design |
| Stub methods in interface impl | ISP violation — interface is too fat |
| `self.db = MySQLDatabase()` in service | DIP violation — concrete class hardcoded |
| Abstraction | Abstract class or interface that hides implementation details |
| Dependency Injection | Passing dependencies in from outside (constructor, method, property) |
| Strategy Pattern | OCP tool — swap algorithm/behaviour without changing context class |
| Polymorphism | OCP + LSP tool — different classes respond to same interface correctly |
| Uncle Bob | Robert C. Martin — creator of SOLID principles |
| DRY | Don't Repeat Yourself — complements SRP |
| KISS | Keep It Simple — don't over-engineer with SOLID |
| YAGNI | You Aren't Gonna Need It — don't add abstractions speculatively |
| Refused Bequest | LSP smell — subclass ignores/overrides parent methods |
| Shotgun Surgery | SRP smell — one change requires editing many files |
| Fat Interface | ISP smell — too many methods for any single implementor |
| Constructor Injection | Preferred DIP pattern — pass dependency in `__init__` |

---

> 💡 **The Production Golden Rule:**
> Always apply — **SRP first (clear separation of concerns) + DIP to decouple layers + OCP when you see repeating if/elif + LSP when designing inheritance + ISP when interfaces grow large**.
> Start with SRP and DIP — they give you 80% of SOLID's benefits and make everything else easier.

---

*Notes prepared based on: Robert C. Martin — Design Principles and Design Patterns (2000), Clean Architecture (2017), GeeksforGeeks, FreeCodeCamp, DigitalOcean, and production best practices (2025–2026)*
