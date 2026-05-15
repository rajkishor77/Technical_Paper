# 🌿 Celery — Complete Study Notes
> **After reading this, no concept of Celery will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is Celery?](#1-what-is-celery)
2. [Why Celery? (vs Multithreading / Async)](#2-why-celery)
3. [Core Components](#3-core-components)
4. [Celery Architecture](#4-celery-architecture)
5. [Message Brokers](#5-message-brokers)
6. [Task Basics](#6-task-basics)
7. [Task States & Lifecycle](#7-task-states--lifecycle)
8. [Periodic Tasks (Beat Scheduler)](#8-periodic-tasks-beat-scheduler)
9. [Error Handling & Retries](#9-error-handling--retries)
10. [Task Routing & Queues](#10-task-routing--queues)
11. [Result Backend](#11-result-backend)
12. [Django + Celery Integration](#12-django--celery-integration)
13. [Best Practices](#13-best-practices)
14. [Celery vs RQ vs Huey](#14-celery-vs-rq-vs-huey)
15. [Limitations of Celery](#15-limitations-of-celery)
16. [Important CLI Commands](#16-important-cli-commands)
17. [Common Interview Questions](#17-common-interview-questions)

---

## 1. What is Celery?

Celery ek **open-source distributed task queue** hai jo Python mein background tasks run karne ke liye use hota hai.

### Simple Analogy 🍽️
> Think of Celery as a **Restaurant Order System**.
> - **Producer (Django view)** = Waiter jo order leta hai customer se
> - **Message Broker (RabbitMQ/Redis)** = Kitchen ka order board (orders yahan wait karte hain)
> - **Worker** = Chef jo orders execute karta hai
> - **Result Backend** = Delivery counter jahan completed orders rakhte hain
> - **Beat Scheduler** = Alarm clock jo automatically daily specials trigger karta hai

### Key Points
- Tasks ko **asynchronously** (background mein) run karta hai — user ko wait nahi karna padta
- **Distributed** hai — alag-alag machines pe workers chal sakte hain
- **Message Broker** ke through tasks pass hote hain (RabbitMQ ya Redis)
- Django, Flask, FastAPI — sab ke saath integrate hota hai
- Written in **Python**, production mein battle-tested

---

## 2. Why Celery?

### Problem without Celery ❌
```
User request aaya → Django view chali → Email bheja (10 sec lag gaye) → Response gaya
                                          ↑ User 10 seconds tak ruka raha!
```

### Solution with Celery ✅
```
User request aaya → Django view chali → Task queue mein daala → Turant response gaya
                                                 ↓
                                         Worker ne background mein email bheja
```

### Real-World Use Cases

| Use Case | Example |
|----------|---------|
| **Email bhejana** | User signup ke baad welcome email background mein jaaye |
| **PDF generate karna** | Report generate ho, user ko notify karo jab ready ho |
| **Image resize** | Upload hone ke baad thumbnail automatically bane |
| **Periodic tasks** | Roz raat 12 baje DB cleanup ho |
| **Third-party API** | Payment gateway call, SMS notification |
| **Data processing** | Large CSV import karna |
| **ML model training** | Model train karo background mein |

### Celery vs Multithreading vs Async

| Feature | Multithreading | Async (asyncio) | Celery |
|---------|---------------|-----------------|--------|
| Distributed execution | ❌ No | ❌ No | ✅ Yes |
| Retry on failure | Manual | Manual | ✅ Auto |
| Task scheduling | ❌ No | ❌ No | ✅ Yes (Beat) |
| Scale across machines | ❌ No | ❌ No | ✅ Yes |
| Result storage | ❌ No | ❌ No | ✅ Yes |
| Best for | CPU-bound | I/O-bound | Distributed bg tasks |

> **Simple rule:** Ek machine pe chhoti async operations ke liye `asyncio` theek hai. Distributed, scheduled, retry-wale heavy tasks ke liye → **Celery** best hai.

---

## 3. Core Components

### 3.1 Task Producer
- Wo code jo task ko queue mein daalta hai
- Example: Django view, management command, koi bhi Python function
- `task.delay()` ya `task.apply_async()` se task submit hota hai
- Producer **directly worker se baat nahi karta** — sirf broker ko message deta hai

### 3.2 Message Broker
- Tasks ko **temporarily store** karta hai jab tak worker process nahi kar leta
- Popular options:
  - **Redis** → Simple, fast, small-medium projects ke liye best
  - **RabbitMQ** → Robust, large production systems ke liye best
  - **SQS** → AWS managed service, cloud projects ke liye

### 3.3 Celery Worker
- Background mein chalta rahta hai aur broker se tasks pick karta hai
- Ek ya multiple workers chal sakte hain (horizontal scaling)
- Worker **concurrently** tasks execute kar sakta hai (threads / processes)
- Start command: `celery -A project worker -l info`

### 3.4 Beat Scheduler
- Celery ka **cron job** — periodic tasks run karta hai
- Defined schedule ke according automatically tasks trigger karta hai
- Start command: `celery -A project beat -l info`
- Example: "Roz 9 AM pe report email bhejo"

### 3.5 Result Backend
- **Optional** component — task ke result ya status store karta hai
- Common backends: Redis, Django DB (`django-db`), Memcached
- `result.get()` se task ka result fetch kar sakte ho
- Agar result ki zaroorat nahi → backend configure karne ki zaroorat nahi

### 3.6 Flower (Monitoring)
- Celery ka **web-based monitoring dashboard**
- Dekh sako active workers, task status, failures, queue length
- Install: `pip install flower`
- Start: `celery -A project flower`

---

## 4. Celery Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                          Your Application                            │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │   Task Producer (Django View / Script)                       │   │
│   │   add.delay(2, 3)  OR  add.apply_async(args=[2,3])           │   │
│   └──────────────────────────────┬──────────────────────────────┘   │
│                                  │                                   │
└──────────────────────────────────│───────────────────────────────────┘
                                   │ sends task message
                                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                    Message Broker (Redis / RabbitMQ)                 │
│                                                                      │
│   ┌──────────────┐   ┌──────────────┐   ┌─────────────────────┐    │
│   │ default queue│   │ high_priority│   │   email_queue       │    │
│   │  [task, task]│   │  [task]      │   │   [task, task, task]│    │
│   └──────────────┘   └──────────────┘   └─────────────────────┘    │
│                                                                      │
│   ┌──────────────────────────────────────┐                          │
│   │  Beat Scheduler (Periodic tasks)     │ ← triggers tasks on time │
│   └──────────────────────────────────────┘                          │
└─────────────────────────────────┬───────────────────────────────────┘
                                  │ workers pick tasks
                                  ▼
┌─────────────────────────────────────────────────────────────────────┐
│                         Celery Workers                               │
│                                                                      │
│   ┌──────────────┐   ┌──────────────┐   ┌──────────────┐           │
│   │   Worker 1   │   │   Worker 2   │   │   Worker 3   │           │
│   │  (Machine A) │   │  (Machine A) │   │  (Machine B) │           │
│   └──────┬───────┘   └──────┬───────┘   └──────┬───────┘           │
│          │ stores result     │                   │                   │
└──────────│───────────────────│───────────────────│───────────────────┘
           │                   │                   │
           ▼                   ▼                   ▼
┌─────────────────────────────────────────────────────────────────────┐
│                  Result Backend (Redis / Django DB)                  │
│   task_id: "abc123" → status: SUCCESS, result: 5                    │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 5. Message Brokers

### 5.1 Redis (Most Popular)
- In-memory store — **bahut fast** hai
- Simple setup, Django ke saath easily integrate hota hai
- Small to medium projects ke liye **perfect**
- Data loss ho sakta hai agar Redis crash ho (persistence configure karni padti hai)

```python
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
```

### 5.2 RabbitMQ (Production Grade)
- Dedicated message broker — **robust aur reliable**
- Advanced routing features (direct, topic, fanout exchange)
- Large, complex production systems ke liye best
- Setup thoda complex hai

```python
CELERY_BROKER_URL = 'amqp://guest:guest@localhost:5672//'
```

### 5.3 Amazon SQS
- AWS managed service — **fully managed, highly available**
- No server manage karna, auto-scaling
- But: AWS account chahiye, not open-source, thoda costly

### Broker Comparison

| Feature | Redis | RabbitMQ | SQS |
|---------|-------|----------|-----|
| Setup complexity | Easy ✅ | Medium | Easy ✅ |
| Speed | Very Fast ✅ | Fast | Medium |
| Durability | Configurable | High ✅ | High ✅ |
| Routing | Basic | Advanced ✅ | Basic |
| Cost | Free ✅ | Free ✅ | Pay-per-use |
| Best for | Small-Medium | Large Production | AWS Projects |

---

## 6. Task Basics

### 6.1 Task Define Karna

```python
# myapp/tasks.py
from celery import shared_task

@shared_task
def send_email(user_id):
    # email bhejne ka logic
    user = User.objects.get(id=user_id)
    send_mail(subject="Welcome!", ...)
    return f"Email sent to {user.email}"
```

**`@shared_task`** — yeh decorator task ko Celery ke saath register karta hai bina specific app instance ke

### 6.2 Task Call Karne ke Tarike

```python
# 1. Simple async call — turant queue mein jaata hai
result = send_email.delay(user_id=5)

# 2. apply_async — zyada control milta hai
result = send_email.apply_async(args=[5])                     # simple
result = send_email.apply_async(args=[5], countdown=60)       # 60 sec baad
result = send_email.apply_async(args=[5], eta=datetime(...))  # specific time pe
result = send_email.apply_async(args=[5], queue='emails')     # specific queue

# 3. Direct call (synchronous — worker bypass hota hai, testing ke liye)
result = send_email(5)
```

### 6.3 Result Access Karna

```python
result = send_email.delay(user_id=5)

print(result.id)          # Task ka unique ID
print(result.status)      # PENDING / SUCCESS / FAILURE
print(result.get())       # Block karke result wait karo (timeout dena recommended)
print(result.get(timeout=10))  # 10 seconds tak wait karo
```

---

## 7. Task States & Lifecycle

```
Task submit hua
      ↓
  PENDING  → Task queue mein wait kar raha hai
      ↓
  STARTED  → Worker ne task uthaya, execute ho raha hai
      ↓
   ┌──────────────────┐
   ↓                  ↓
SUCCESS            FAILURE
(result store)    (error store)
                       ↓
                   RETRY (agar configured ho)
                       ↓
              MAX_RETRIES ke baad → FAILURE
```

| State | Matlab |
|-------|--------|
| **PENDING** | Task queue mein wait kar raha hai ya ID unknown hai |
| **STARTED** | Worker ne task uthaya (track_started=True ho tabhi dikhta) |
| **SUCCESS** | Task successfully complete hua |
| **FAILURE** | Task fail hua, exception aaya |
| **RETRY** | Task dobara try ho raha hai |
| **REVOKED** | Task cancel kar diya gaya |

---

## 8. Periodic Tasks (Beat Scheduler)

### 8.1 Setup in settings.py

```python
from celery.schedules import crontab

CELERY_BEAT_SCHEDULE = {
    'send-daily-report': {
        'task': 'myapp.tasks.send_report',
        'schedule': crontab(hour=9, minute=0),   # Roz 9 AM pe
    },
    'cleanup-every-30min': {
        'task': 'myapp.tasks.cleanup_old_sessions',
        'schedule': 30.0,   # Har 30 seconds (float = seconds)
    },
    'weekly-newsletter': {
        'task': 'myapp.tasks.send_newsletter',
        'schedule': crontab(day_of_week='monday', hour=8, minute=0),  # Monday 8 AM
    },
}
```

### 8.2 Crontab Examples

```python
crontab()                           # Har minute
crontab(minute=0, hour='*/2')       # Har 2 ghante
crontab(day_of_week='mon-fri')      # Weekdays pe
crontab(day_of_month='1')           # Har mahine ki 1 tarikh
```

### 8.3 Beat Start karna
```bash
celery -A myproject beat -l info
```

> ⚠️ **Important:** Beat aur Worker alag-alag processes hain — dono ko alag terminal mein start karo!

---

## 9. Error Handling & Retries

### 9.1 Auto Retry

```python
@shared_task(bind=True, max_retries=3, default_retry_delay=60)
def send_email(self, user_id):
    try:
        # email logic
        pass
    except Exception as exc:
        raise self.retry(exc=exc, countdown=30)  # 30 sec baad retry
```

- **`bind=True`** — `self` milta hai task instance ka (retry ke liye zaruri)
- **`max_retries=3`** — maximum 3 baar retry karega
- **`default_retry_delay=60`** — default 60 sec wait karega retry se pehle
- **`countdown=30`** — is specific retry mein 30 sec wait karega

### 9.2 Exponential Backoff

```python
@shared_task(bind=True, max_retries=5)
def send_email(self, user_id):
    try:
        pass
    except Exception as exc:
        # 2^retry_count seconds wait karo (1, 2, 4, 8, 16 seconds)
        raise self.retry(exc=exc, countdown=2 ** self.request.retries)
```

### 9.3 On Failure Hook

```python
@shared_task(bind=True)
def send_email(self, user_id):
    pass

def on_task_failure(self, exc, task_id, args, kwargs, einfo):
    print(f"Task {task_id} failed: {exc}")
    # Alert bhejo ya log karo
```

---

## 10. Task Routing & Queues

### 10.1 Multiple Queues banana

```python
# settings.py
CELERY_TASK_ROUTES = {
    'myapp.tasks.send_email': {'queue': 'emails'},
    'myapp.tasks.generate_report': {'queue': 'reports'},
    'myapp.tasks.process_payment': {'queue': 'payments'},
}
```

### 10.2 Specific queue pe worker chalana

```bash
# Sirf email queue ka worker
celery -A myproject worker -Q emails -l info

# Multiple queues
celery -A myproject worker -Q emails,reports -l info

# High priority worker
celery -A myproject worker -Q payments --concurrency=4 -l info
```

### 10.3 Task ko specific queue pe send karna

```python
send_email.apply_async(args=[user_id], queue='emails')
```

> **Use Case:** Payment tasks ko alag high-priority queue mein daalo taaki email tasks slow payments ko slow na kare.

---

## 11. Result Backend

### 11.1 Redis Backend (Common)

```python
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
```

### 11.2 Django Database Backend

```python
# settings.py
CELERY_RESULT_BACKEND = 'django-db'
INSTALLED_APPS += ['django_celery_results']

# Phir migration run karo
python manage.py migrate
```

### 11.3 Result Use karna

```python
result = add.delay(4, 6)

# Non-blocking check
if result.ready():
    print(result.result)   # 10

# Blocking wait
value = result.get(timeout=5)

# Forget result (memory free karna)
result.forget()
```

### 11.4 Result Expiry Set karna

```python
# 24 ghante ke baad result automatically delete ho
CELERY_RESULT_EXPIRES = 86400  # seconds
```

---

## 12. Django + Celery Integration

### Step-by-Step Setup

**Step 1: Install karo**
```bash
pip install celery redis django-celery-results django-celery-beat
```

**Step 2: `celery.py` banao (project root mein)**
```python
# myproject/celery.py
import os
from celery import Celery

os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'myproject.settings')

app = Celery('myproject')
app.config_from_object('django.conf:settings', namespace='CELERY')
app.autodiscover_tasks()  # Har app ke tasks.py ko automatically load karta hai
```

**Step 3: `__init__.py` update karo**
```python
# myproject/__init__.py
from .celery import app as celery_app
__all__ = ['celery_app']
```

**Step 4: `settings.py` configure karo**
```python
CELERY_BROKER_URL = 'redis://localhost:6379/0'
CELERY_RESULT_BACKEND = 'redis://localhost:6379/0'
CELERY_TIMEZONE = 'Asia/Kolkata'
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
```

**Step 5: Task banao**
```python
# myapp/tasks.py
from celery import shared_task

@shared_task
def add(x, y):
    return x + y
```

**Step 6: Task call karo (view mein)**
```python
# myapp/views.py
from myapp.tasks import add

def my_view(request):
    result = add.delay(4, 6)
    return JsonResponse({'task_id': result.id})
```

**Step 7: Worker start karo**
```bash
celery -A myproject worker -l info
```

### File Structure

```
myproject/
│
├── myproject/
│   ├── __init__.py      ← celery app import yahan
│   ├── celery.py        ← Celery instance yahan define hota hai
│   ├── settings.py      ← Broker URL, result backend config
│   └── urls.py
│
├── myapp/
│   ├── tasks.py         ← Saare tasks yahan likhte hain
│   └── views.py         ← Yahan se tasks call hote hain
│
└── manage.py
```

---

## 13. Best Practices

### ✅ Do's

- **Reliable broker use karo** → RabbitMQ production ke liye, Redis development ke liye
- **`bind=True` aur retries** → Har important task mein error handling lagao
- **Task queues separate karo** → Email, payment, report — alag-alag queues
- **Idempotent tasks banao** → Agar task dobara chale, to duplicate side effects na ho
- **Timeout set karo** → `result.get(timeout=10)` — kabhi bhi infinite wait mat karo
- **Flower use karo** → Production mein monitoring zaruri hai
- **Small tasks banao** → Ek task ek kaam kare — 10 kaam ek task mein mat karo
- **Database objects pass mat karo** → Task mein `user_id` pass karo, `user` object nahi

### ❌ Don'ts

- **DB transactions ke andar task call mat karo** → Transaction commit hone se pehle task worker ke paas pahunch sakta hai
- **Large data task arguments mein mat daalo** → IDs pass karo, poora object nahi
- **Result backend always use mat karo** → Agar result chahiye nahi, configure karne ki zaroorat nahi
- **Tasks block mat karo** → `result.get()` kisi aur task ke andar mat lagao (deadlock)

---

## 14. Celery vs RQ vs Huey

| Feature | **Celery** | **RQ (Redis Queue)** | **Huey** |
|---------|------------|---------------------|----------|
| Broker support | Redis, RabbitMQ, SQS ✅ | Redis only | Redis, SQLite |
| Periodic tasks | ✅ Built-in (Beat) | ❌ (rq-scheduler chahiye) | ✅ Built-in |
| Task retry | ✅ Advanced | Basic | Basic |
| Monitoring | ✅ Flower | ✅ RQ Dashboard | ❌ Limited |
| Setup complexity | Medium | Easy ✅ | Easy ✅ |
| Production-ready | ✅ Battle-tested | Medium | Small projects |
| Django integration | ✅ Excellent | Good | Good |
| Community size | Large ✅ | Medium | Small |

> **Summary:**
> - **Celery** → Large, complex, production projects
> - **RQ** → Simple projects, sirf Redis chahiye
> - **Huey** → Lightweight, simple use cases

---

## 15. Limitations of Celery

| Limitation | Detail |
|------------|--------|
| **Setup complexity** | Broker, worker, beat, result backend — sab manage karna padta hai |
| **Latency** | Message broker ke through jaata hai → thoda delay hota hai |
| **Debugging mushkil** | Main app aur worker alag processes mein hote hain — trace karna tough |
| **Windows support limited** | Windows pe kuch features kaam nahi karte (prefork pool) |
| **Memory usage** | Workers background mein chalta rahta hai — resources consume hote hain |
| **Vertical scaling hard** | Horizontal scaling (zyada workers) easy hai, ek machine pe resources badhana tough |
| **Security** | Galat config se DoS aur message injection attacks possible |

---

## 16. Important CLI Commands

```bash
# Worker start karo (basic)
celery -A myproject worker -l info

# Worker start karo with concurrency
celery -A myproject worker -l info --concurrency=4

# Specific queue ka worker
celery -A myproject worker -Q emails,reports -l info

# Beat scheduler start karo (periodic tasks)
celery -A myproject beat -l info

# Worker + Beat ek saath (development ke liye)
celery -A myproject worker --beat -l info

# Flower monitoring dashboard (port 5555)
celery -A myproject flower

# Active tasks dekho
celery -A myproject inspect active

# Registered tasks dekho
celery -A myproject inspect registered

# Worker stats dekho
celery -A myproject inspect stats

# Queue mein pending tasks count karo (Redis)
redis-cli llen celery

# Sabhi tasks purge karo (danger!)
celery -A myproject purge

# Specific task revoke karo
celery -A myproject control revoke <task_id>

# Worker pool reload karo
celery -A myproject control pool_restart
```

---

## 17. Common Interview Questions

### Q1: Celery kya hai aur yeh kyu use karte hain?
**Answer:** Celery ek distributed task queue hai jo Python mein background tasks run karne ke liye use hota hai. Iska use isliye karte hain kyunki kuch tasks (email bhejna, PDF banana, image resize karna) bahut time lete hain. Inhe synchronously run karne se user ko wait karna padta hai aur web application slow feel hoti hai. Celery in tasks ko background mein ek separate worker process mein run karta hai, aur user ko turant response milta hai.

---

### Q2: Celery ke main components kya hain?
**Answer:**
- **Task Producer** — Task submit karta hai (Django view ya koi script)
- **Message Broker** — Tasks ko temporarily store karta hai (Redis / RabbitMQ)
- **Celery Worker** — Broker se tasks pick karke execute karta hai
- **Beat Scheduler** — Periodic/scheduled tasks automatically trigger karta hai
- **Result Backend** — Task ka result/status store karta hai (optional)

---

### Q3: `delay()` aur `apply_async()` mein kya fark hai?
**Answer:**
- `delay()` ek shortcut hai — sirf arguments pass karo, task turant queue mein chala jaata hai
- `apply_async()` zyada powerful hai — countdown (delay), eta (specific time), queue name, priority sab specify kar sakte ho
- Internally `delay(*args)` same hai `apply_async(args=args)` ke — sirf convenience ke liye hai

---

### Q4: Celery mein task retry kaise karte hain?
**Answer:** Task function mein `bind=True` set karo taaki `self` mile, phir `self.retry(exc=exception)` call karo. `max_retries` se maximum retry count set karo aur `countdown` se retry ke beech ka wait time set karo. Exponential backoff ke liye `countdown=2 ** self.request.retries` use kar sakte hain.

---

### Q5: Beat Scheduler kya hota hai?
**Answer:** Celery Beat ek separate process hai jo periodic tasks ko automatically schedule karta hai — bilkul Linux ke `cron` ki tarah. Settings mein `CELERY_BEAT_SCHEDULE` define karo jisme task name, schedule (crontab ya seconds) specify karo. Beat process khud tasks execute nahi karta — woh sirf broker ko message bhejta hai, actual execution worker karta hai. Dono processes alag-alag start karni padti hain.

---

### Q6: Idempotent task kya hoti hai aur yeh kyu important hai?
**Answer:** Idempotent task woh hai jo ek baar ya baar baar chale, result same rahe — koi duplicate side effects nahi hote. Celery mein tasks fail hone par retry hoti hain — agar task idempotent nahi hai, to retry pe duplicate emails, duplicate payments jaisi problems aa sakti hain. Isliye tasks is tarah design karo ki unhe safely multiple times run kiya ja sake.

---

### Q7: Celery mein multiple queues kyu aur kab use karte hain?
**Answer:** Multiple queues use karte hain jab different tasks ki alag-alag priorities ho. Jaise payment processing urgent hai aur marketing email non-urgent — agar dono ek hi queue mein hain, to bhare hue email queue ki wajah se payment tasks delay ho sakti hain. Alag queues mein payment tasks ke liye dedicated workers lagate hain jisse urgent kaam pehle hota hai.

---

### Q8: `@task` aur `@shared_task` mein kya fark hai?
**Answer:**
- `@task` specific Celery app instance ka use karta hai — tightly coupled hota hai
- `@shared_task` reusable hai — kisi bhi Celery app ke saath kaam karta hai, specific app import karne ki zaroorat nahi
- Django apps mein `@shared_task` recommend kiya jaata hai kyunki yeh app ko reusable aur independently testable banata hai

---

### Q9: Result Backend configure karna kab zaruri hai?
**Answer:** Sirf tab zaruri hai jab task ka result ya status jaanna ho. Agar fire-and-forget tasks hain (email bhejo, log likho) jinka result check nahi karna — result backend configure karne ki zaroorat nahi. Result backend ek extra service hai (Redis/DB) jo unnecessary overhead add karta hai agar use nahi karna. Default mein result tracking off rakhna better practice hai.

---

### Q10: Database transaction ke andar task call karna problematic kyu hai?
**Answer:** Agar Django DB transaction ke andar `task.delay()` call karo, to worker turant task uthaa sakta hai — lekin transaction abhi commit nahi hua hoga. Worker DB mein wo data nahi paayega jo transaction mein add kiya tha. Iske liye `transaction.on_commit(lambda: task.delay(...))` use karo — yeh ensure karta hai ki task sirf tab queue mein jaaye jab transaction successfully commit ho jaaye.

---

## 🔄 Celery — At a Glance

```
Producer → Broker → Worker → Result Backend
  ↑                    ↑
Django View        Beat Scheduler (for periodic tasks)
```

---

## 📝 Quick Revision Card

| Concept | Ek line mein |
|---------|-------------|
| Celery | Python distributed task queue — background tasks run karne ke liye |
| Task | `@shared_task` decorated Python function |
| Producer | Wo code jo `.delay()` ya `.apply_async()` call karta hai |
| Message Broker | Tasks ko temporarily hold karta hai (Redis / RabbitMQ) |
| Worker | Background process jo tasks execute karta hai |
| Beat Scheduler | Cron jaise periodic tasks automatically trigger karta hai |
| Result Backend | Task ka result/status store karta hai (optional) |
| `delay()` | Task turant queue mein bhejne ka shortcut |
| `apply_async()` | Task bhejne ka advanced method (countdown, eta, queue) |
| `bind=True` | Task ko `self` milta hai (retry ke liye zaruri) |
| `max_retries` | Maximum kitni baar retry kare |
| `countdown` | Retry se pehle kitne seconds wait kare |
| Idempotent task | Multiple baar chalane pe bhi duplicate effect nahi hota |
| Flower | Celery ka web monitoring dashboard |
| Prefetch | Worker ek baar mein kitne tasks uthaye |
| `on_commit` | Transaction commit hone ke baad task queue mein bhejne ke liye |

---

> 💡 **Production Golden Rule:**
> Always use — **Redis/RabbitMQ as broker + Retry with exponential backoff + Separate queues by priority + Idempotent tasks + Flower monitoring + `on_commit` for DB-related tasks**.
> Yeh 6 cheezein saath use karo → reliable, production-grade Celery system ban jaayega.

---

> 📚 *Notes based on: Celery Official Docs, Django + Celery integration guides, GeeksforGeeks, Real Python, production best practices (2025–2026)*
