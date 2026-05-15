# 🐇 RabbitMQ — Complete Study Notes
> **Isko padhne ke baad RabbitMQ ka koi bhi concept anjaana nahi rahega.**  
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [RabbitMQ Kya Hai?](#1-rabbitmq-kya-hai)
2. [RabbitMQ Ka Itihas](#2-rabbitmq-ka-itihas)
3. [Core Components](#3-core-components)
4. [Exchange Types](#4-exchange-types-detail-mein)
5. [Queue Types](#5-queue-types)
6. [Messaging Patterns](#6-messaging-patterns)
7. [Message Lifecycle](#7-message-lifecycle)
8. [Dead Letter Exchange (DLX)](#8-dead-letter-exchange-dlx)
9. [Acknowledgment System](#9-acknowledgment-system)
10. [Clustering & High Availability](#10-clustering--high-availability)
11. [Performance Tuning](#11-performance-tuning--best-practices)
12. [RabbitMQ vs Kafka](#12-rabbitmq-vs-kafka)
13. [Protocols Supported](#13-protocols-supported)
14. [Real-Life Use Cases](#14-real-life-use-cases)
15. [Quick Start Guide](#15-quick-start-guide)
16. [Important CLI Commands](#16-important-cli-commands)
17. [Common Interview Questions](#17-common-interview-questions)

---

## 1. RabbitMQ Kya Hai?

RabbitMQ ek **open-source Message Broker** hai jo applications ke beech mein messages ka aadaan-pradaan karta hai — bina ek doosre ko jaane.

### Simple Analogy 🏣
> RabbitMQ ek **Post Office** ki tarah hai.
> - **Producer** = letter likhne wala (sender)
> - **Exchange** = post office (sorting center)
> - **Routing Key** = pin code / address
> - **Queue** = mailbox (letter wait karta hai)
> - **Consumer** = letter padhne wala (receiver)

### Key Points:
- **AMQP** (Advanced Message Queuing Protocol) pe based hai
- **Erlang** programming language mein likha gaya hai
- Erlang ki wajah se — massive concurrency, fault tolerance, aur real-time performance milta hai
- Messages **asynchronously** deliver hote hain → services ek doosre ka wait nahi karti

---

## 2. RabbitMQ Ka Itihas

| Year | Event |
|------|-------|
| 2007 | RabbitMQ release hua — Erlang mein likha, AMQP implement kiya |
| 2010 | VMware ne acquire kiya |
| 2013 | Pivotal Software ke under aaya |
| 2019 | VMware ne phir acquire kiya |
| 2023 | RabbitMQ 3.12 — Performance improvements |
| 2024 | RabbitMQ 4.0 — Khepri metadata storage, Quorum Queue default bana |
| 2026 | RabbitMQ 4.1 — Quorum queue memory ~35% reduce, Native Streams mature |

---

## 3. Core Components

### 3.1 Producer (Sender)
- Messages generate karta hai aur Exchange ko bhejta hai
- Message types: JSON, XML, plain text, binary
- Producer **directly queue ko message nahi bhejta** — Exchange ko bhejta hai
- Publisher Confirms set kar sakta hai → broker ne message receive kiya confirmation

### 3.2 Exchange (Router / Switchboard)
- Producer ka message pehle Exchange pe aata hai
- Exchange decide karta hai ki message kaunsi Queue(s) mein jayega
- **Routing Key** aur **Exchange Type** ke basis pe routing hoti hai
- Exchange ke 4 main types hain (detail mein Section 4 mein)

### 3.3 Queue (Buffer / Mailbox)
- Messages temporarily store hoti hai queue mein
- Consumer ke aane tak messages wait karti hain
- Queue properties:
  - **Durable** → server restart pe bhi queue bachti hai
  - **Transient** → server restart pe delete ho jaati hai
  - **Exclusive** → sirf ek connection use kar sakta hai, connection band hone pe delete
  - **Auto-delete** → last consumer disconnect hone pe delete
  - **TTL (Time-To-Live)** → message ek time ke baad delete ho jata hai
  - **Max Length** → queue bhar jaane pe naye messages drop ya dead-letter ho jaate hain

### 3.4 Consumer (Receiver)
- Queue se messages read aur process karta hai
- Do tarike se consume kar sakta hai:
  - **Push** (subscribe) → broker automatic deliver karta hai ✅ (recommended)
  - **Pull** → consumer manually maangta hai
- Message process hone ke baad **ACK** (acknowledgment) bhejta hai

### 3.5 Binding
- Exchange aur Queue ke beech connection
- Binding mein **Routing Key** hoti hai jo batati hai kis message ko kahan bhejna hai

### 3.6 Routing Key
- Producer message ke saath ek "label" deta hai
- Exchange is label ko padhke decide karta hai routing

### 3.7 Virtual Host (vHost)
- RabbitMQ ke andar ek **namespace** / isolated environment
- Alag-alag applications alag vHost use kar sakti hain
- Users ke alag-alag permissions ho sakte hain per vHost
- Default vHost: `/`
- Example: `/development`, `/staging`, `/production`

### 3.8 Channel
- Ek TCP connection ke andar **lightweight virtual connection**
- Ek TCP connection pe **multiple channels** khole ja sakte hain (TCP expensive hai, channel cheap hai)
- **Rule:** Ek channel = ek thread (per thread/coroutine ek channel use karo)
- Publishing ya consuming har kaam channel pe hota hai

---

## 4. Exchange Types (Detail Mein)

### 4.1 Direct Exchange
```
Producer → [routing_key: "order"] → Direct Exchange
                                          ↓ (exact match)
                                     Queue: "order_queue"
```
- **Exact match** hota hai routing key aur binding key mein
- Use case: Specific service ko message bhejna (e.g., order service, payment service)

### 4.2 Fanout Exchange
```
Producer → Fanout Exchange → Queue 1 (Email Service)
                           → Queue 2 (SMS Service)
                           → Queue 3 (Push Notification)
```
- **Routing key ignore** hota hai
- **Saari bound queues** mein message jaata hai (broadcast)
- Use case: Notification system, news feed, live sports scores

### 4.3 Topic Exchange
```
Producer → [routing_key: "order.payment.success"]
         → Topic Exchange
              → "order.*"         → Queue 1
              → "*.payment.*"     → Queue 2
              → "order.#"         → Queue 3 (# = zero ya zyada words)
```
- Pattern matching on routing key (dot-separated words)
- `*` → exactly **ek word** match karta hai
- `#` → **zero ya zyada words** match karta hai
- Use case: Logging systems, IoT sensors (temperature.zone1.alert)

### 4.4 Headers Exchange
```
Producer → [headers: {type: "report", format: "pdf"}]
         → Headers Exchange
              → Queue 1 (x-match: all → type=report AND format=pdf)
              → Queue 2 (x-match: any → type=report OR format=pdf)
```
- Routing key ki jagah **message headers** se routing
- `x-match: all` → saare headers match hone chahiye (AND condition)
- `x-match: any` → koi bhi ek header match ho (OR condition)
- Flexible hai lekin **slowest** exchange type

### Exchange Comparison Table

| Exchange | Routing Basis | Speed | Use Case |
|----------|--------------|-------|----------|
| Direct | Exact key match | ⚡⚡⚡ Fastest | Task distribution |
| Fanout | Broadcast (no key) | ⚡⚡⚡ Fast | Notifications, broadcast |
| Topic | Pattern matching | ⚡⚡ Medium | Logging, IoT |
| Headers | Message headers | ⚡ Slowest | Complex routing rules |

---

## 5. Queue Types

### 5.1 Classic Queue
- Traditional queue, backward compatible
- Transient workloads ke liye theek hai
- **Production mein avoid karo** — RabbitMQ 4.0 mein mirroring remove ho gayi

### 5.2 Quorum Queue ✅ (Production Recommended)
- **Raft consensus algorithm** pe based → majority nodes agree karte hain
- Data multiple nodes pe replicate hota hai (leader + followers)
- Automatic leader election agar leader fail ho
- Memory ~35% kam (RabbitMQ 4.1 mein)
- **Split-brain problem solve ho gayi**
- Use case: Banking, orders, payments — jahan koi message lose nahi hona chahiye

### 5.3 Streams (Newest)
- Kafka-jaise **append-only log** structure
- Consumers offset se padh sakte hain → **message replay** possible
- Multiple consumers ek hi log se padh sakte hain → efficient fan-out
- Use case: Event sourcing, audit logs, replay-needed scenarios

### Queue Types Comparison

| Feature | Classic | Quorum | Stream |
|---------|---------|--------|--------|
| Durability | Optional | Always | Always |
| Replication | None (4.0+) | Raft-based | Yes |
| Message Replay | ❌ | ❌ | ✅ |
| Production Safe | ⚠️ | ✅ | ✅ |
| Priority Support | ✅ | ✅ | ❌ |

---

## 6. Messaging Patterns

### 6.1 Point-to-Point (Work Queue)
```
Producer → Queue → Consumer 1
                 → Consumer 2 (load balanced)
```
- Ek message sirf **ek consumer** process karta hai
- Multiple consumers add karo → automatic load balancing
- Use case: Image processing, email sending, PDF generation

### 6.2 Publish / Subscribe
```
Producer → Fanout Exchange → Queue 1 → Consumer A
                           → Queue 2 → Consumer B
                           → Queue 3 → Consumer C
```
- Ek message → multiple consumers (sabhi receive karte hain)
- Use case: Live score updates, news broadcast, notifications

### 6.3 Request / Reply (RPC Pattern)
```
Client → Request Queue → Server
Client ← Reply Queue   ← Server
```
- Two-way communication
- Client ek "reply_to" queue deta hai message ke saath
- Server process karke reply queue pe response deta hai
- Use case: Synchronous-like calls in async system, data fetch

### 6.4 Routing Pattern
```
Producer → Direct Exchange → [key=error]  → Error Queue → Error Handler
                           → [key=info]   → Info Queue  → Logger
                           → [key=warning]→ Warn Queue  → Monitor
```

### 6.5 Topic / Pattern Matching
```
Producer → Topic Exchange → "logs.error.*" → Critical Alert Queue
                          → "logs.#"       → All Logs Queue
```

---

## 7. Message Lifecycle

```
1. Producer message banata hai
        ↓
2. Producer message Exchange ko bhejta hai (with routing key)
        ↓
3. Exchange routing rule check karta hai
        ↓
4. Message Queue mein store hoti hai
        ↓
5. Consumer message uthata hai (push ya pull)
        ↓
6. Consumer process karta hai
        ↓
7a. SUCCESS → ACK bhejta hai → RabbitMQ message delete karta hai ✅
7b. FAILURE → NACK/reject → RabbitMQ message requeue karta hai 🔁
7c. TTL expire ya queue full → Dead Letter Exchange (DLX) → DLQ ☠️
```

### Message Durability ke liye 3 cheezein zaroori:
1. **Queue durable=true** → queue survive kare restart
2. **Message delivery_mode=2 (persistent)** → disk pe likha jaye
3. **Publisher Confirms** → broker ne persist kiya confirm kare

---

## 8. Dead Letter Exchange (DLX)

### DLX Kya Hai?
Jab message **deliver nahi ho sakta**, toh use ek special exchange pe bheja jaata hai — **Dead Letter Exchange**. Wahan se message **Dead Letter Queue (DLQ)** mein jaata hai.

### Message "Dead" Kab Hota Hai?
| Reason | Description |
|--------|-------------|
| **NACK/Reject** | Consumer ne `requeue=false` ke saath reject kiya |
| **TTL Expire** | Message ka time-to-live khatam ho gaya |
| **Queue Full** | Queue apni max length pe pahunch gayi |
| **Delivery Limit** | Quorum queue mein zyada baar redeliver ho gaya |

### DLX Flow:
```
Main Queue → [message fail/expire] → Dead Letter Exchange → Dead Letter Queue
                                                                    ↓
                                                         (inspect / retry / alert)
```

### DLX Setup (Python example):
```python
# Dead Letter Exchange declare karo
channel.exchange_declare(exchange='dlx', exchange_type='direct', durable=True)

# DLQ declare karo
channel.queue_declare(queue='dead_letters', durable=True)
channel.queue_bind(queue='dead_letters', exchange='dlx', routing_key='failed')

# Main queue mein DLX attach karo
channel.queue_declare(
    queue='main_queue',
    durable=True,
    arguments={
        'x-dead-letter-exchange': 'dlx',
        'x-dead-letter-routing-key': 'failed',
        'x-message-ttl': 60000  # 60 seconds TTL
    }
)
```

### Retry Pattern with DLX:
```
Main Queue → [fail] → Retry Queue (TTL: 10s) → [expire] → Main Queue
Main Queue → [fail again] → Retry Queue (TTL: 60s) → [expire] → Main Queue
Main Queue → [fail 3rd time] → DLQ (manual inspection)
```
Yeh **Exponential Backoff Retry** pattern hai — production mein bahut use hota hai.

### DLX Kab Use Karo?
- Jab messages fail ho sakte hain lekin lose nahi hone chahiye
- Jab TTL set ki ho aur expired messages track karni ho
- Jab queue overflow se bachna ho
- Debugging ke liye production issues trace karne ke liye

---

## 9. Acknowledgment System

### Consumer Acknowledgments (ACK)
```python
# Manual ACK (recommended)
def callback(ch, method, properties, body):
    try:
        process(body)        # kaam karo
        ch.basic_ack(delivery_tag=method.delivery_tag)   # success → delete
    except Exception:
        ch.basic_nack(delivery_tag=method.delivery_tag, requeue=True)  # fail → requeue
```

| Type | Description |
|------|-------------|
| **ACK (Positive)** | Message successfully process hua → queue se delete |
| **NACK (Negative)** | Processing fail hua → requeue ya dead-letter |
| **Auto ACK** | Message deliver hote hi delete ❌ (production mein use mat karo!) |

### Publisher Confirms
- Producer ko confirmation milti hai ki broker ne message receive aur persist kiya
- Crash se pehle message save hua ya nahi — confirm hota hai

### Prefetch Count (QoS)
- Consumer ek saath kitne messages uthaye bina ACK ke → **prefetch count**
- RabbitMQ tab tak naye messages nahi deta jab tak old messages ACK na hon
- **Rule:** 
  - Chhota processing time → higher prefetch (100-300)
  - Bada processing time → lower prefetch (1-10)
  - Default: unlimited (bad for production!)

```python
channel.basic_qos(prefetch_count=10)  # ek saath max 10 messages
```

---

## 10. Clustering & High Availability

### Single Node vs Cluster

| Feature | Single Node | Cluster |
|---------|-------------|---------|
| Fault Tolerance | ❌ | ✅ |
| Scalability | Limited | High |
| Use Case | Dev/Test | Production |

### RabbitMQ Clustering
- Multiple RabbitMQ servers → ek cluster
- **Users, exchanges, bindings, vhosts** → saare nodes pe share hote hain
- **Queues** → by default sirf ek node pe hoti hain (unless Quorum Queue)
- Nodes ek doosre se **Erlang distribution protocol** se baat karte hain

### Quorum Queues (Cluster mein)
```
Node 1 [Leader]  ←→  Node 2 [Follower]  ←→  Node 3 [Follower]
     ↑
 Sab writes yahan hote hain, phir followers pe propagate
```
- **Raft consensus** → majority (N/2 + 1) nodes agree karein
- 3 nodes cluster → 1 node fail ho sakta hai
- 5 nodes cluster → 2 nodes fail ho sakte hain
- Leader fail → automatic new leader elect

### AWS Amazon MQ RabbitMQ
- **Single-instance**: 1 broker, 1 AZ, Network Load Balancer ke behind
- **Cluster**: 3 nodes, 3 different AZs, automatic mirroring, automatic rebalancing

---

## 11. Performance Tuning & Best Practices

### ✅ DO's (Kya Karna Chahiye)

**Queue Management:**
- Queue **chhoti rakho** → large queues RAM khate hain → RabbitMQ disk flush karta hai → slow
- **Durable queues + persistent messages** use karo production mein
- Queue ka **TTL set karo** → expired messages auto-delete
- **Max length limit** set karo → overflow control

**Connection & Channel:**
- **Ek TCP connection** per application instance → connections expensive hain
- **Multiple channels** ek connection pe → channels cheap hain
- **Ek channel per thread** use karo

**Consumer:**
- **Manual ACK** hamesha use karo → `auto_ack=False`
- **Prefetch count** set karo → consumer overwhelm mat karo
- **Dead Letter Queue** hamesha configure karo

**Exchange:**
- **Direct exchange fastest** hai → jab bhi ho sake direct use karo
- Queue ko directly publish mat karo (sendToQueue avoid karo) → exchange use karo

**Monitoring:**
- **Management Plugin** enable karo → web UI pe sab dekho
- Queue length, consumer count, message rate monitor karo

### ❌ DON'Ts (Kya Nahi Karna Chahiye)

- `auto_ack=True` production mein kabhi nahi → message crash pe lost ho jaata hai
- Har publish pe **naya connection** mat banao → TCP expensive hai
- **Bahut badi queue** mat banao → RAM issue aur slowdown
- **Endless retry loop** DLX se mat banao → poison messages cycle karte hain
- **Classic queue mirroring** mat use karo (RabbitMQ 4.0 mein remove ho gayi)

---

## 12. RabbitMQ vs Kafka

### Architecture Difference

| Feature | RabbitMQ | Apache Kafka |
|---------|----------|--------------|
| Type | Message Broker | Event Streaming Platform |
| Model | Push-based | Pull-based |
| Message Storage | Queue drain hone pe delete | Log-based, retention period tak |
| Message Replay | Streams se ✅ | Native ✅ |
| Throughput | 10K-100K msg/sec | 1M+ msg/sec |
| Latency | Sub-millisecond | ~5ms (p99 at scale) |
| Routing | Complex (4 exchange types) | Simple (topics/partitions) |
| Protocol | AMQP, MQTT, STOMP | Native TCP binary |
| Best For | Complex routing, task queues, microservices | High-volume streaming, analytics |

### Kab RabbitMQ Use Karo?
- Complex routing logic chahiye
- Individual message delivery guarantee chahiye
- Low-latency task queues (sub-ms response needed)
- IoT, microservices, background jobs
- Credit card transactions, order processing

### Kab Kafka Use Karo?
- Bahut high throughput chahiye (millions of msgs/sec)
- Message replay / event sourcing needed
- Real-time analytics, log aggregation
- Long retention needed

### Hybrid Approach (Best of Both)
```
Microservices ←→ RabbitMQ ←→ Business Logic
Analytics/Streaming ←→ Kafka ←→ Data Pipelines
```
> Many organizations dono use karte hain — RabbitMQ commands/jobs ke liye, Kafka events/analytics ke liye.

---

## 13. Protocols Supported

| Protocol | Full Name | Use Case |
|----------|-----------|----------|
| **AMQP 0-9-1** | Advanced Message Queuing Protocol | Default, most feature-rich |
| **AMQP 1.0** | AMQP modern version | Enterprise interoperability |
| **MQTT** | Message Queuing Telemetry Transport | IoT, lightweight pub/sub |
| **STOMP** | Simple Text Oriented Messaging Protocol | Easy to use, text-based |
| **HTTP/WebSocket** | Web protocols | Browser-based apps |
| **RabbitMQ Streams** | Native streaming protocol | High-throughput, replay |

---

## 14. Real-Life Use Cases

| Company | Use Case |
|---------|----------|
| **eBay** | Background tasks aur service decoupling |
| **Instagram** | Upload ke baad async image/video processing |
| **Mozilla** | Real-time log aggregation aur routing |
| **BBC** | Real-time content updates across platforms |
| **SoundCloud** | Microservices ke beech communication |
| **Slack** | Real-time notifications aur message delivery |

### Industry-wise Use Cases:

**E-commerce:**
- Order placement → Payment → Inventory → Shipping (async pipeline)
- Email/SMS notifications queue

**Banking/Finance:**
- Transaction processing
- Fraud detection alerts
- Credit card authorization routing

**Healthcare:**
- Patient record updates
- Lab result notifications
- Medical device data (IoT)

**Media & Entertainment:**
- Video encoding pipeline
- Live event notifications
- Content delivery triggers

**Gaming:**
- Multiplayer game events
- Leaderboard updates
- In-game notifications

---

## 15. Quick Start Guide

### Step 1: Install RabbitMQ

**Docker (Sabse easy):**
```bash
docker run -d --name rabbitmq \
  -p 5672:5672 \
  -p 15672:15672 \
  rabbitmq:3-management
```
- Port 5672 → AMQP connection
- Port 15672 → Web Management UI
- Management UI: http://localhost:15672 (user: guest, pass: guest)

**Ubuntu/Linux:**
```bash
sudo apt-get install rabbitmq-server
sudo systemctl start rabbitmq-server
sudo rabbitmq-plugins enable rabbitmq_management
```

### Step 2: Python Example (Producer)
```python
import pika

# Connection banao
connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

# Queue declare karo (durable = restart survive karega)
channel.queue_declare(queue='hello', durable=True)

# Message bhejo
channel.basic_publish(
    exchange='',          # default exchange
    routing_key='hello',  # queue ka naam = routing key
    body='Hello World!',
    properties=pika.BasicProperties(
        delivery_mode=2,  # persistent message
    )
)
print("Message bheja!")
connection.close()
```

### Step 3: Python Example (Consumer)
```python
import pika

connection = pika.BlockingConnection(pika.ConnectionParameters('localhost'))
channel = connection.channel()

channel.queue_declare(queue='hello', durable=True)
channel.basic_qos(prefetch_count=1)  # ek saath 1 message

def callback(ch, method, properties, body):
    print(f"Mila: {body.decode()}")
    ch.basic_ack(delivery_tag=method.delivery_tag)  # manual ACK

channel.basic_consume(queue='hello', on_message_callback=callback)
print("Waiting for messages...")
channel.start_consuming()
```

---

## 16. Important CLI Commands

```bash
# RabbitMQ server status check
rabbitmqctl status

# Saari queues dekho
rabbitmqctl list_queues

# Queue ki detail dekho
rabbitmqctl list_queues name messages consumers

# Saare exchanges dekho
rabbitmqctl list_exchanges

# Saari bindings dekho
rabbitmqctl list_bindings

# User create karo
rabbitmqctl add_user myuser mypassword

# User ko admin banao
rabbitmqctl set_user_tags myuser administrator

# vHost create karo
rabbitmqctl add_vhost myvhost

# Permissions set karo
rabbitmqctl set_permissions -p myvhost myuser ".*" ".*" ".*"

# Management plugin enable karo
rabbitmq-plugins enable rabbitmq_management

# Queue purge karo (messages delete)
rabbitmqctl purge_queue queuename

# Node ka cluster status
rabbitmqctl cluster_status
```

---

## 17. Common Interview Questions

### Q1: RabbitMQ aur Kafka mein kya difference hai?
**Answer:** RabbitMQ ek message broker hai complex routing ke liye (push-based, low-latency), jabki Kafka ek event streaming platform hai high-throughput ke liye (pull-based, log-based storage). RabbitMQ messages consume hone ke baad delete hote hain (Streams ke alawa), Kafka mein retention period tak rehte hain.

### Q2: Dead Letter Queue kya hai aur kab use karte hain?
**Answer:** DLQ ek special queue hai jahan failed/expired/rejected messages jaate hain. Tab use karte hain jab: consumer NACK kare with requeue=false, TTL expire ho, ya queue full ho jaye. Production mein hamesha DLX configure karna chahiye taaki koi message permanently lose na ho.

### Q3: Exchange aur Queue mein kya fark hai?
**Answer:** Exchange message receive karta hai aur routing decide karta hai — yeh ek router hai. Queue message store karti hai consumer ke liye — yeh ek buffer/mailbox hai. Producer exchange ko message bhejta hai, exchange queue ko.

### Q4: Quorum Queue classic queue se better kyun hai?
**Answer:** Quorum Queue Raft consensus algorithm use karta hai, jo multiple nodes pe data replicate karta hai. Agar ek node fail ho, automatically naya leader elect hota hai. Classic queue mirroring unreliable thi aur RabbitMQ 4.0 mein remove ho gayi.

### Q5: Prefetch count kya hota hai?
**Answer:** Prefetch count define karta hai ki consumer ek saath kitne unacknowledged messages receive kar sakta hai. Jab count reach ho, RabbitMQ naye messages tab tak nahi bhejta jab tak koi ACK na aaye. Yeh consumer ko overwhelm hone se bachata hai.

### Q6: Ek TCP connection pe multiple channels kyun use karte hain?
**Answer:** TCP connections expensive hain (handshake, resource allocation). Channels lightweight virtual connections hain jo ek TCP connection share karte hain. Isliye ek app instance ke liye ek TCP connection aur uske andar multiple channels (per thread) best practice hai.

### Q7: Message durability ke liye kya karna padta hai?
**Answer:** Teen cheezein zaroori hain: (1) Queue durable=True declare karo, (2) Message delivery_mode=2 (persistent) set karo, (3) Publisher Confirms enable karo. Teeno saath ho toh message broker crash pe bhi safe rahega.

### Q8: Topic Exchange mein `*` aur `#` ka kya matlab hai?
**Answer:** `*` exactly ek word match karta hai routing key mein (dot-separated). `#` zero ya zyada words match karta hai. Example: `order.*` → `order.placed`, `order.cancelled` ko match karega. `order.#` → `order`, `order.placed`, `order.placed.2024` sab match karega.

---

## 🔄 RabbitMQ Architecture — Ek Nazar Mein

```
┌─────────────────────────────────────────────────────────────┐
│                        RabbitMQ Broker                       │
│                                                              │
│  ┌──────────┐    ┌───────────────────────────────────────┐  │
│  │          │    │              Exchanges                 │  │
│  │ Producer │───►│  Direct | Fanout | Topic | Headers     │  │
│  │          │    │                                        │  │
│  └──────────┘    └────────────────┬──────────────────────┘  │
│                                   │ (Routing Key + Bindings) │
│                                   ▼                          │
│                  ┌────────────────────────────────────────┐  │
│                  │               Queues                   │  │
│                  │  [Queue 1]  [Queue 2]  [Queue 3]       │  │
│                  │  (Durable)  (Quorum)   (Stream)        │  │
│                  └──────────────┬─────────────────────────┘  │
│                                 │                            │
└─────────────────────────────────│────────────────────────────┘
                                  ▼
                   ┌──────────────────────────┐
                   │        Consumers         │
                   │  [Worker 1] [Worker 2]   │
                   │      ↑ ACK/NACK ↑        │
                   └──────────────────────────┘
```

---

## 📝 Quick Revision Card

| Concept | Ek Line Mein |
|---------|-------------|
| RabbitMQ | Open-source message broker, AMQP, Erlang |
| Producer | Message bhejta hai |
| Exchange | Message route karta hai (4 types) |
| Queue | Message store karta hai |
| Consumer | Message receive aur process karta hai |
| Binding | Exchange-Queue connection |
| Routing Key | Routing ka "label" |
| vHost | Isolated namespace |
| Channel | Lightweight virtual TCP connection |
| ACK | Processing success confirm |
| NACK | Processing fail → requeue |
| DLX | Failed messages ka special exchange |
| DLQ | Dead messages ki queue |
| TTL | Message ka expiry time |
| Prefetch | Consumer ek saath kitne messages le |
| Quorum Queue | Raft-based replicated queue (production) |
| Publisher Confirms | Broker ne message receive kiya → confirm |

---

> 💡 **Pro Tip:** Production mein hamesha — Quorum Queues + Manual ACK + DLX + Prefetch Count + Durable Queues + Publisher Confirms use karo. Yahi 6 cheezein ek reliable RabbitMQ system banati hain.

---
*Notes prepared with research from: CloudAMQP, RabbitMQ Official Docs, Medium (2025-2026), Airbyte, DevToolbox*
