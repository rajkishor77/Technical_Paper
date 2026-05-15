# 🔴 Redis — Complete Study Notes
> **After reading this, every Redis concept will be crystal clear.**
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [What is Redis?](#1-what-is-redis)
2. [History of Redis](#2-history-of-redis)
3. [How Redis Works](#3-how-redis-works)
4. [Why Redis is So Fast](#4-why-redis-is-so-fast)
5. [Data Types — Complete Reference](#5-data-types--complete-reference)
6. [Key Expiry & TTL](#6-key-expiry--ttl)
7. [Persistence — RDB & AOF](#7-persistence--rdb--aof)
8. [Caching Patterns](#8-caching-patterns)
9. [Pub/Sub Messaging](#9-pubsub-messaging)
10. [Redis Streams](#10-redis-streams)
11. [Transactions & Pipelining](#11-transactions--pipelining)
12. [Replication — Primary & Replica](#12-replication--primary--replica)
13. [High Availability — Redis Sentinel](#13-high-availability--redis-sentinel)
14. [Horizontal Scaling — Redis Cluster](#14-horizontal-scaling--redis-cluster)
15. [Memory Management & Eviction](#15-memory-management--eviction)
16. [Redis Security](#16-redis-security)
17. [Redis with Python](#17-redis-with-python)
18. [Redis with Node.js](#18-redis-with-nodejs)
19. [Redis vs Alternatives](#19-redis-vs-alternatives)
20. [Real-World Use Cases](#20-real-world-use-cases)
21. [All Redis CLI Commands — Explained](#21-all-redis-cli-commands--explained)
22. [Best Practices](#22-best-practices)
23. [Common Interview Questions](#23-common-interview-questions)

---

## 1. What is Redis?

**Redis** (Remote Dictionary Server) is an open-source, **in-memory data structure store** that can be used as a:
- **Cache** — store frequently accessed data in RAM for sub-millisecond retrieval
- **Database** — a primary or secondary data store with optional persistence
- **Message Broker** — pub/sub and streams for real-time messaging
- **Session Store** — fast user session management
- **Queue** — task and job queues using Lists

### Simple Analogy 📚
> Think of Redis like a **whiteboard** next to your main filing cabinet (database).
> - Fetching a file from the filing cabinet (disk DB) takes time — you have to walk there, search, pull it out.
> - Writing the answer on the whiteboard (Redis/RAM) means next time anyone asks — you just glance at it. Instant.
> - But the whiteboard has limited space, so you only write what gets asked most often.

### Key Facts
- Stores data in **RAM** → sub-millisecond response times
- Supports **rich data structures** beyond simple key-value (strings, lists, sets, sorted sets, hashes, streams, etc.)
- Can handle **100,000+ operations per second** on a single instance
- **Open-source** (BSD license — returned to open-source in 2025 after a brief license change)
- Works as cache + database + message broker in one tool

---

## 2. History of Redis

| Year | Event |
|------|-------|
| 2009 | Created by Salvatore Sanfilippo (antirez) to solve real-time web log scaling |
| 2010 | VMware sponsors Redis development |
| 2015 | Redis Labs formed; Redis 3.0 adds Cluster support |
| 2018 | Redis Streams introduced (Redis 5.0) |
| 2020 | Redis 6.0 — TLS, ACL (Access Control Lists) |
| 2022 | Redis 7.0 — Redis Functions, sharded Pub/Sub |
| 2024 | Moved to dual-license (RSALv2 + SSPLv1) — controversy |
| 2025 | Redis returns to open-source BSD license |
| 2026 | Redis 8.0 — major performance improvements, enhanced observability |

---

## 3. How Redis Works

### Request Flow (Cache Layer Pattern)

```
Client Request
      ↓
  API Gateway / Application
      ↓
  Check Redis Cache
      ↓
  ┌──────────────┐          ┌──────────────────────┐
  │  CACHE HIT   │          │     CACHE MISS        │
  │  Data found  │          │  Not in Redis         │
  │  in Redis    │          │       ↓               │
  │      ↓       │          │  Query Main Database  │
  │  Return to   │          │       ↓               │
  │  Client      │          │  Store result in Redis│
  │  instantly   │          │  (with TTL)           │
  └──────────────┘          │       ↓               │
                            │  Return to Client     │
                            └──────────────────────┘
```

### How Redis Stores Data
- Everything is stored as a **key → value** pair
- **Keys** are always strings (max 512 MB, keep them short and descriptive)
- **Values** can be one of 10+ data types (string, list, set, hash, sorted set, stream, etc.)

```
"user:1001:name"    →  "Alice"             (String)
"user:1001:cart"    →  ["item1", "item2"]  (List)
"leaderboard"       →  {Alice: 9500, Bob: 8200} (Sorted Set)
"product:42"        →  {name: "Phone", price: "999"} (Hash)
```

### Key Naming Convention (Best Practice)
```
object-type:id:field
user:1001:profile
session:abc123
product:42:details
order:2026:stats
```
Use **colons as separators** — Redis groups keys with the same prefix in tools like RedisInsight.

---

## 4. Why Redis is So Fast

| Factor | How It Helps |
|--------|-------------|
| **In-Memory Storage** | All data in RAM; no disk I/O; memory access is ~100,000x faster than disk |
| **Single-Threaded Event Loop** | No locking, no context switching, no thread overhead |
| **Non-Blocking I/O** | Handles thousands of connections without blocking |
| **Optimized Data Structures** | Internally uses skip lists, zip lists, hash tables — chosen per data size |
| **RESP Protocol** | Simple text-based protocol; minimal parsing overhead |
| **Pipelining** | Multiple commands sent in one round trip |

> **Result:** Redis typically responds in **under 1 millisecond** — often in **microseconds**.  
> A typical disk-based DB query: 50–200ms. Redis: 0.1–1ms. That's **100–2000x faster**.

---

## 5. Data Types — Complete Reference

### 5.1 String — The Basic Type
The most fundamental type. Can store text, numbers, or binary data (images, serialized JSON).

```bash
SET name "Alice"              # store a string
GET name                      # → "Alice"
SET counter 0                 # store a number as string
INCR counter                  # → 1 (atomic increment)
INCRBY counter 10             # → 11 (increment by 10)
DECR counter                  # → 10
DECRBY counter 3              # → 7
INCRBYFLOAT price 1.5         # increment float value
APPEND name " Smith"          # → "Alice Smith" (append to existing)
STRLEN name                   # → 11 (length of string)
GETRANGE name 0 4             # → "Alice" (substring)
SETRANGE name 6 "Jones"       # replace part of string
MSET k1 v1 k2 v2 k3 v3       # set multiple keys at once
MGET k1 k2 k3                 # get multiple keys at once
SETNX key value               # set ONLY if key does not exist (NX = Not eXists)
SETEX key 60 value            # set with 60-second expiry
SET key value EX 60 NX        # set with expiry + only if not exists (atomic)
GETSET key newvalue           # get old value and set new value atomically
GETDEL key                    # get value and delete the key atomically
```

**Use cases:** Session tokens, feature flags, counters, rate limiters, simple string caching.

---

### 5.2 List — Ordered Sequence (Double-Ended Queue)
An ordered list of strings. Can be used as a stack (LIFO) or queue (FIFO). Elements can repeat.

```bash
LPUSH tasks "task1"           # push to LEFT (head)
LPUSH tasks "task2" "task3"   # push multiple (task3 at head now)
RPUSH tasks "task4"           # push to RIGHT (tail)
LRANGE tasks 0 -1             # get ALL elements: [task3, task2, task1, task4]
LRANGE tasks 0 2              # get first 3 elements
LPOP tasks                    # remove and return from LEFT head (task3)
RPOP tasks                    # remove and return from RIGHT tail (task4)
LLEN tasks                    # length of the list
LINDEX tasks 0                # get element at index 0
LSET tasks 0 "updated_task"   # set element at index
LINSERT tasks BEFORE "task1" "new_task"  # insert before an element
LREM tasks 0 "task1"          # remove all occurrences of "task1"
LTRIM tasks 0 9               # keep only elements from index 0 to 9
BLPOP tasks 5                 # blocking pop — wait up to 5 seconds
BRPOP tasks 5                 # blocking pop from right — wait up to 5 seconds
RPOPLPUSH src dst             # atomically move last element of src to head of dst
LMOVE src dst LEFT RIGHT      # move element from one list to another (Redis 6.2+)
```

**Use cases:**
- **Message queues** — RPUSH to add, BLPOP to consume
- **Activity feeds** — keep latest N events with LPUSH + LTRIM
- **Undo/redo stacks** — LPUSH + LPOP
- **Task queues** — Celery uses Redis Lists as its broker

---

### 5.3 Set — Unordered, Unique Collection
A collection of unique strings. No duplicates, no guaranteed order.

```bash
SADD fruits "apple" "banana" "mango"   # add members
SADD fruits "apple"                    # ignored — already exists
SMEMBERS fruits                        # get ALL members (unordered)
SCARD fruits                           # count of members → 3
SISMEMBER fruits "apple"               # check if member → 1 (yes)
SMISMEMBER fruits "apple" "grape"      # check multiple → [1, 0]
SREM fruits "banana"                   # remove a member
SPOP fruits                            # remove and return a random member
SRANDMEMBER fruits 2                   # get 2 random members (no removal)
SUNION set1 set2                       # union of two sets
SINTER set1 set2                       # intersection (common members)
SDIFF set1 set2                        # members in set1 but NOT in set2
SUNIONSTORE dest set1 set2             # union result stored in dest
SINTERSTORE dest set1 set2             # intersection stored in dest
SMOVE set1 set2 "member"              # move member from set1 to set2
```

**Use cases:**
- **Tags** — articles tagged with "python", "redis", "backend"
- **Unique visitors** — track which user IDs visited a page (no duplicates)
- **Friend lists** — SINTER to find mutual friends
- **Random selection** — lottery winners, random recommendations

---

### 5.4 Sorted Set (ZSet) — Set with Scores
Like a Set, but every member has a **floating-point score**. Members are always sorted by score (ascending). Unique members, but scores can repeat.

```bash
ZADD leaderboard 9500 "Alice"          # add member with score
ZADD leaderboard 8200 "Bob"
ZADD leaderboard 9500 "Charlie"        # same score as Alice
ZADD leaderboard NX 7000 "Dave"        # add only if not exists
ZADD leaderboard XX 8500 "Bob"         # update only if exists
ZRANGE leaderboard 0 -1                # all members (low → high score)
ZRANGE leaderboard 0 -1 WITHSCORES    # with scores
ZREVRANGE leaderboard 0 2 WITHSCORES  # top 3 (high → low)
ZRANK leaderboard "Alice"             # rank (0-indexed, lowest first) → 2
ZREVRANK leaderboard "Alice"          # rank from highest → 0 (she's top)
ZSCORE leaderboard "Alice"            # get score → 9500
ZINCRBY leaderboard 500 "Bob"         # increment Bob's score by 500
ZCARD leaderboard                     # count of members
ZCOUNT leaderboard 8000 10000         # count with scores between 8000 and 10000
ZRANGEBYSCORE leaderboard 8000 +inf   # members with score >= 8000
ZRANGEBYSCORE leaderboard -inf 8000   # members with score <= 8000
ZRANGEBYSCORE leaderboard 8000 +inf WITHSCORES LIMIT 0 5  # with pagination
ZREM leaderboard "Dave"               # remove a member
ZPOPMAX leaderboard                   # remove and return highest scoring member
ZPOPMIN leaderboard                   # remove and return lowest scoring member
ZREMRANGEBYSCORE leaderboard -inf 7000  # remove all with score <= 7000
ZREMRANGEBYRANK leaderboard 0 2        # remove members by rank range
ZUNIONSTORE dest 2 zset1 zset2         # union of sorted sets
ZINTERSTORE dest 2 zset1 zset2         # intersection of sorted sets
```

**Use cases:**
- **Leaderboards** — games, top scores (ZREVRANGE to get top N)
- **Priority queues** — score = priority or timestamp
- **Rate limiting** — score = timestamp, ZRANGEBYSCORE to count recent events
- **Trending content** — score = view/like count

---

### 5.5 Hash — Dictionary of Fields
Like a Python dict or JSON object stored under one key.

```bash
HSET user:1001 name "Alice" age 30 city "Mumbai"   # set multiple fields
HGET user:1001 name                                 # → "Alice"
HMGET user:1001 name age                            # → ["Alice", "30"]
HGETALL user:1001                                   # → {name, Alice, age, 30, city, Mumbai}
HKEYS user:1001                                     # → [name, age, city]
HVALS user:1001                                     # → [Alice, 30, Mumbai]
HLEN user:1001                                      # → 3 (number of fields)
HEXISTS user:1001 email                             # → 0 (field doesn't exist)
HDEL user:1001 city                                 # delete a field
HINCRBY user:1001 age 1                             # age becomes 31
HINCRBYFLOAT user:1001 balance 10.50               # float increment
HSETNX user:1001 role "admin"                       # set ONLY if field doesn't exist
```

**Use cases:**
- **User profiles** — store all user fields under `user:1001`
- **Product details** — name, price, stock, category under one key
- **Configuration objects** — app settings as structured data
- **Shopping carts** — productId → quantity mapping

---

### 5.6 Bitmap — Compact Boolean Flags
Stores binary (0/1) flags at bit-level positions. Extremely space-efficient.

```bash
SETBIT active_users 1001 1     # user 1001 was active today
SETBIT active_users 1002 1
SETBIT active_users 1003 0     # user 1003 was NOT active
GETBIT active_users 1001       # → 1
BITCOUNT active_users          # count of 1-bits (active users today)
BITOP AND dest key1 key2       # AND of two bitmaps
BITOP OR dest key1 key2        # OR of two bitmaps
BITPOS active_users 1          # position of first 1-bit (first active user)
```

**Use cases:**
- **Daily active users** — one bit per user per day
- **Feature flags** — is feature X enabled for user Y?
- **Attendance tracking** — 1 bit per day per employee

---

### 5.7 HyperLogLog — Approximate Unique Count
Estimates the **cardinality (unique count)** of a very large set with very little memory (~12 KB regardless of set size). Error rate is ~0.81%.

```bash
PFADD visitors "user1" "user2" "user3"   # add elements
PFADD visitors "user1"                    # duplicate ignored
PFCOUNT visitors                          # approximate unique count → ~3
PFADD page2_visitors "user2" "user4"
PFMERGE all_visitors visitors page2_visitors  # merge multiple HyperLogLogs
PFCOUNT all_visitors                      # → ~4
```

**Use cases:**
- **Unique page views** — count distinct visitors without storing all IDs
- **Unique searches** — how many unique search queries today?
- Any "count distinct" at massive scale with ~0.81% margin of error

---

### 5.8 Geospatial — Location-Based Data
Store and query geographic coordinates (latitude, longitude).

```bash
GEOADD locations 72.8777 19.0760 "Mumbai"   # longitude, latitude, name
GEOADD locations 77.5946 12.9716 "Bangalore"
GEOADD locations 88.3639 22.5726 "Kolkata"

GEOPOS locations "Mumbai"                   # get coordinates of Mumbai
GEODIST locations "Mumbai" "Bangalore" km   # distance in km → ~843 km
GEOSEARCH locations FROMLONLAT 72.8777 19.0760 BYRADIUS 1000 km ASC  # nearest within 1000km
GEOSEARCH locations FROMMEMBER "Mumbai" BYRADIUS 1000 km ASC COUNT 5  # top 5 nearest
GEOHASH locations "Mumbai"                  # geohash string for external use
```

**Use cases:**
- **Nearby drivers** — Uber/Ola finding the nearest driver
- **Delivery zones** — is this address within delivery radius?
- **Store locator** — nearest branch within X km

---

### Data Type Summary Table

| Data Type | Structure | Best For |
|-----------|-----------|---------|
| String | Simple value | Caching, counters, tokens |
| List | Ordered sequence | Queues, activity feeds, stacks |
| Set | Unique collection | Tags, unique visitors, unions |
| Sorted Set | Scored unique members | Leaderboards, priority queues |
| Hash | Field-value map | User profiles, objects, carts |
| Bitmap | Bit array | Boolean flags at scale |
| HyperLogLog | Probabilistic counter | Approximate unique counts |
| Geospatial | Lat/lng indexed | Location-based queries |
| Stream | Append-only log | Event sourcing, message queues |

---

## 6. Key Expiry & TTL

Redis can automatically **delete keys after a set time** (Time-To-Live). This is essential for caches.

```bash
SET session:abc123 "user_data" EX 3600   # set with 1-hour expiry
SET otp:9820012345 "4821" EX 300          # OTP expires in 5 minutes

EXPIRE key 60                             # set expiry on existing key (60 seconds)
PEXPIRE key 5000                          # set expiry in milliseconds (5000ms = 5s)
EXPIREAT key 1735689600                   # set expiry as Unix timestamp
PEXPIREAT key 1735689600000              # Unix timestamp in milliseconds

TTL key                                   # remaining time in seconds (-1 = no expiry, -2 = gone)
PTTL key                                  # remaining time in milliseconds

PERSIST key                               # remove expiry (make key permanent)

# Check what happens when TTL expires:
SET temp "value" EX 1
# wait 2 seconds
EXISTS temp                              # → 0 (key is gone)
GET temp                                 # → nil
```

### TTL Return Values

| TTL Return | Meaning |
|-----------|---------|
| Positive number | Seconds remaining before expiry |
| `-1` | Key exists but has no expiry (permanent) |
| `-2` | Key does not exist (expired or never set) |

---

## 7. Persistence — RDB & AOF

Redis is in-memory — data is lost on restart **unless** persistence is configured.

### 7.1 RDB — Redis Database Snapshots
Creates a **point-in-time binary snapshot** of all data at specified intervals.

```bash
# redis.conf settings
save 900 1      # save snapshot if 1+ key changed in last 900 seconds (15 min)
save 300 10     # save if 10+ keys changed in last 300 seconds (5 min)
save 60 10000   # save if 10000+ keys changed in last 60 seconds
dbfilename dump.rdb
dir /var/lib/redis
```

```bash
# Manual snapshot commands
SAVE                  # synchronous — blocks Redis until done (avoid in production)
BGSAVE                # background save — forks a child process, non-blocking ✅
LASTSAVE              # Unix timestamp of last successful save
```

**Pros:**
- Very compact binary format — small file size, fast to load on restart
- Great for backups and disaster recovery
- Minimal performance impact (child process does the work)

**Cons:**
- You can lose data between snapshots (e.g., last 5 minutes of writes)
- Not ideal if you need strong durability

---

### 7.2 AOF — Append Only File
**Logs every write command** to a file. On restart, Redis replays the log to reconstruct the dataset.

```bash
# redis.conf settings
appendonly yes
appendfilename "appendonly.aof"

# How often to fsync (write) to disk:
appendfsync always      # after every command → safest, slowest
appendfsync everysec    # every second → good balance (default, recommended ✅)
appendfsync no          # let OS decide → fastest, least safe
```

**Pros:**
- Much better durability — with `everysec`, you lose at most 1 second of data
- Human-readable file (it's just Redis commands)
- Can replay from beginning to reconstruct any state

**Cons:**
- AOF file grows large over time (use BGREWRITEAOF to compact it)
- Slightly slower than RDB on restart (replaying commands)

```bash
BGREWRITEAOF    # compact the AOF file in the background (removes redundant commands)
```

---

### 7.3 RDB vs AOF — Comparison

| Feature | RDB | AOF |
|---------|-----|-----|
| Durability | Lower (loses data between snapshots) | Higher (loses max 1 second with everysec) |
| File Size | Compact (binary) | Larger (grows over time) |
| Recovery Speed | Fast (load binary file) | Slower (replay all commands) |
| Backup | ✅ Excellent for backups | ⚠️ Less compact |
| Performance Impact | Low (background fork) | Slightly higher (fsync overhead) |
| Human Readable | ❌ Binary | ✅ Yes (Redis commands) |

### Production Recommendation
```
Use BOTH RDB + AOF:
- AOF = for durability (lose at most 1 second)
- RDB = for fast backup and fast restart

# redis.conf
save 900 1
appendonly yes
appendfsync everysec
```

---

## 8. Caching Patterns

### 8.1 Cache-Aside (Lazy Loading) ✅ Most Common
```python
def get_user(user_id):
    # 1. Check cache first
    cached = redis.get(f"user:{user_id}")
    if cached:
        return json.loads(cached)           # cache HIT

    # 2. Cache miss → query database
    user = db.query("SELECT * FROM users WHERE id = %s", user_id)

    # 3. Store in cache for 1 hour
    redis.setex(f"user:{user_id}", 3600, json.dumps(user))

    return user
```
- App manages the cache explicitly
- Only loads data into cache when it's actually requested
- Cache can become stale if DB is updated without invalidating

---

### 8.2 Read-Through
- Cache sits in front of the DB; on a miss, **the cache** queries the DB and populates itself
- App only ever talks to cache — DB interaction is hidden
- Used in managed caching systems (AWS ElastiCache)

---

### 8.3 Write-Through
```python
def update_user(user_id, data):
    # Write to DB first
    db.execute("UPDATE users SET ... WHERE id = %s", user_id)
    # Immediately update cache too
    redis.setex(f"user:{user_id}", 3600, json.dumps(data))
```
- Cache is always in sync with DB
- Slightly slower writes (two writes: DB + cache)
- Prevents stale data

---

### 8.4 Write-Behind (Write-Back)
- App writes to cache ONLY
- Cache asynchronously writes to DB later in batches
- Fastest writes — but risk of data loss if cache crashes before syncing

---

### 8.5 Cache Stampede (Thundering Herd) — Problem & Solution
**Problem:** When a popular cached key expires, thousands of requests simultaneously try to rebuild it — overwhelming the database.

```python
# SOLUTION 1: Mutex Lock
def get_with_lock(key, fetch_fn, ttl=3600):
    data = redis.get(key)
    if data:
        return json.loads(data)

    # Only one process rebuilds the cache
    lock_key = f"lock:{key}"
    if redis.set(lock_key, "1", nx=True, ex=10):   # acquire lock
        data = fetch_fn()
        redis.setex(key, ttl, json.dumps(data))
        redis.delete(lock_key)
        return data
    else:
        time.sleep(0.1)                             # wait for lock holder
        return get_with_lock(key, fetch_fn, ttl)   # retry

# SOLUTION 2: Probabilistic Early Expiration
# Refresh the cache slightly before it expires based on probability
```

---

## 9. Pub/Sub Messaging

Redis Pub/Sub implements a **publish-subscribe messaging pattern** for real-time communication.

```
Publisher → Channel → All Subscribers receive the message
```

### Basic Pub/Sub

**Subscriber (in terminal 1):**
```bash
SUBSCRIBE chat:room1          # subscribe to a channel
# Output:
# 1) "subscribe"
# 2) "chat:room1"
# 3) (integer) 1

# Wait for messages...
# 1) "message"
# 2) "chat:room1"
# 3) "Hello from Alice!"
```

**Publisher (in terminal 2):**
```bash
PUBLISH chat:room1 "Hello from Alice!"    # → 1 (number of subscribers who received it)
```

### Pattern Subscribe
```bash
PSUBSCRIBE news.*             # subscribe to all channels matching pattern
PSUBSCRIBE user:*:notifications
# Receives messages from: news.sports, news.tech, news.finance, etc.
```

### Pub/Sub Commands
```bash
SUBSCRIBE channel1 channel2   # subscribe to one or more channels
UNSUBSCRIBE channel1          # unsubscribe from a channel
PSUBSCRIBE news.*             # pattern subscribe
PUNSUBSCRIBE news.*           # pattern unsubscribe
PUBLISH channel message       # publish a message to a channel
PUBSUB CHANNELS               # list active channels with subscribers
PUBSUB NUMSUB channel1        # count subscribers per channel
PUBSUB NUMPAT                 # count of pattern subscriptions
```

### Important Limitations of Pub/Sub
- Messages are **fire-and-forget** — if no subscriber is online, the message is LOST
- No message persistence, no delivery guarantees
- Not suitable for durable messaging (use **Redis Streams** for that)

**Use cases:** Live chat, real-time notifications, live dashboards, event broadcasting.

---

## 10. Redis Streams

Streams are Redis's most powerful feature for **durable, persistent message queues** — similar to Kafka but built into Redis.

```bash
# Add a message to a stream
XADD orders * product "Phone" quantity 2 price 999
# * = auto-generated ID (timestamp-sequence: 1714567890-0)

XADD orders 1714567890-1 product "Laptop" quantity 1 price 75000  # custom ID

# Read messages from a stream
XREAD COUNT 10 STREAMS orders 0      # read up to 10 messages from beginning
XREAD COUNT 10 STREAMS orders $      # read only NEW messages ($ = latest)

# Read with blocking (wait up to 5 seconds for new messages)
XREAD BLOCK 5000 COUNT 10 STREAMS orders $

# Get stream length
XLEN orders

# Range of messages
XRANGE orders - +               # all messages (- = earliest, + = latest)
XRANGE orders 1714567890-0 +    # from specific ID onward
XREVRANGE orders + -            # reverse order

# Consumer Groups — for competing consumers
XGROUP CREATE orders my-group $ MKSTREAM   # create group ($ = only new messages)
XREADGROUP GROUP my-group worker1 COUNT 10 STREAMS orders >   # read as worker1
# > means "messages not yet delivered to this group"

# Acknowledge processed message
XACK orders my-group 1714567890-0

# See pending (unacknowledged) messages
XPENDING orders my-group - + 10

# Claim a stuck message (if worker1 died)
XCLAIM orders my-group worker2 60000 1714567890-0  # after 60s idle

# Trim stream to last N messages
XTRIM orders MAXLEN 10000
```

### Streams vs Pub/Sub

| Feature | Pub/Sub | Streams |
|---------|---------|---------|
| Persistence | ❌ Fire and forget | ✅ Messages persisted |
| Replay | ❌ | ✅ Read from any point |
| Consumer Groups | ❌ | ✅ Competing consumers |
| Acknowledgment | ❌ | ✅ |
| At-least-once | ❌ | ✅ |
| Use case | Transient broadcasts | Durable event queues |

---

## 11. Transactions & Pipelining

### Transactions (MULTI/EXEC)
Redis transactions allow you to execute a group of commands **atomically** — all or nothing.

```bash
MULTI                         # start transaction (queue commands)
SET balance 1000
INCR transfer_count
DECRBY balance 200
EXEC                          # execute all queued commands atomically
```

```bash
DISCARD                       # cancel the transaction (clear queue)
```

**Key behavior:**
- Commands are **queued**, not executed immediately after MULTI
- On EXEC, all queued commands run atomically — no other client's command can interleave
- If Redis crashes during EXEC, AOF logs handle partial write recovery
- **Errors before EXEC** (syntax errors) → all commands discarded
- **Errors during EXEC** (wrong type) → other commands still run (unlike SQL transactions)

### WATCH — Optimistic Locking
```bash
WATCH balance                 # watch for changes
MULTI
DECRBY balance 200
EXEC                          # returns nil if "balance" changed since WATCH → retry!
```
If `balance` is modified by another client between WATCH and EXEC, the transaction is **aborted** and returns nil. Your code should retry.

### Pipelining — Batch Commands
Pipelining sends multiple commands to Redis in a single network round-trip — dramatically reducing latency for bulk operations.

```python
# Without pipelining: N round trips
for i in range(1000):
    redis.set(f"key:{i}", i)    # 1000 network round trips 😢

# With pipelining: 1 round trip for all 1000 commands ✅
pipe = redis.pipeline()
for i in range(1000):
    pipe.set(f"key:{i}", i)
results = pipe.execute()         # one network round trip
```

### Transactions vs Pipelining

| Feature | MULTI/EXEC | Pipeline |
|---------|-----------|---------|
| Atomicity | ✅ Atomic | ❌ Not atomic |
| Round trips | N (queued then one EXEC) | 1 (batched) |
| Speed | Fast | Fastest |
| Use case | Critical operations needing atomicity | Bulk reads/writes |

---

## 12. Replication — Primary & Replica

Redis supports **master-replica** (primary-replica) replication for:
- **High availability** — if primary fails, a replica can be promoted
- **Read scaling** — direct read traffic to replicas

```
Primary (writes) → Replica 1 (reads)
               → Replica 2 (reads)
               → Replica 3 (reads)
```

```bash
# redis.conf on replica
replicaof 192.168.1.10 6379     # point to primary IP and port
# OR set dynamically:
REPLICAOF 192.168.1.10 6379

# Check replication status
INFO replication

# Promote replica to primary (disconnect from master)
REPLICAOF NO ONE
```

**Important:** Replicas are **read-only by default**. All writes must go to primary.  
Replication is **asynchronous** — slight lag is normal.

---

## 13. High Availability — Redis Sentinel

**Redis Sentinel** provides automatic failover — if the primary goes down, Sentinel elects a new primary from the replicas automatically.

```
                    Sentinel 1
                    Sentinel 2    ← monitors primary and replicas
                    Sentinel 3

        Primary ──────────────► Replica 1
                                Replica 2

        If primary fails:
        Sentinels vote → promote Replica 1 as new Primary → clients redirected
```

```bash
# sentinel.conf
sentinel monitor mymaster 192.168.1.10 6379 2   # 2 = quorum (votes needed to failover)
sentinel down-after-milliseconds mymaster 5000   # mark primary down after 5s of no response
sentinel failover-timeout mymaster 60000          # failover must complete in 60s
sentinel parallel-syncs mymaster 1               # 1 replica syncs at a time during failover
```

```bash
# Connect to Sentinel
redis-cli -p 26379

# Sentinel commands
SENTINEL masters                          # list all monitored masters
SENTINEL slaves mymaster                  # list replicas of mymaster
SENTINEL sentinels mymaster               # list other sentinels
SENTINEL get-master-addr-by-name mymaster # get current primary address
SENTINEL failover mymaster               # manually trigger failover
SENTINEL reset mymaster                  # reset sentinel state
```

### Sentinel Quorum
- Minimum **3 Sentinels** in production
- Quorum = (N/2)+1 → for 3 Sentinels, quorum = 2 (2 must agree primary is down)

---

## 14. Horizontal Scaling — Redis Cluster

**Redis Cluster** automatically **shards data** across multiple primary nodes. It provides both high availability and the ability to store more data than fits on one server.

```
Redis Cluster (6 nodes: 3 primary + 3 replica)

Primary 1 (slots 0–5460)       → Replica A
Primary 2 (slots 5461–10922)   → Replica B
Primary 3 (slots 10923–16383)  → Replica C
```

### Hash Slots
Redis Cluster divides the key space into **16,384 hash slots**.
```
hash_slot = CRC16(key) % 16384
```
Each primary node owns a range of hash slots. Data is automatically routed to the correct node.

```bash
# Create a cluster (6 nodes minimum: 3 primary + 3 replica)
redis-cli --cluster create \
  127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 \
  127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006 \
  --cluster-replicas 1

# Check cluster status
redis-cli -p 7001 cluster info
redis-cli -p 7001 cluster nodes

# Add a new primary node
redis-cli --cluster add-node 127.0.0.1:7007 127.0.0.1:7001

# Rebalance hash slots across nodes
redis-cli --cluster rebalance 127.0.0.1:7001

# Remove a node
redis-cli --cluster del-node 127.0.0.1:7001 <node-id>

# Reshard slots from one node to another
redis-cli --cluster reshard 127.0.0.1:7001
```

### Sentinel vs Cluster

| Feature | Sentinel | Cluster |
|---------|---------|---------|
| Purpose | HA for single primary | HA + Horizontal Scaling |
| Data Sharding | ❌ | ✅ (16,384 slots) |
| Multi-Primary | ❌ (one primary) | ✅ (multiple primaries) |
| Memory Limit | One server's RAM | Sum of all nodes' RAM |
| Complexity | Medium | High |
| Best For | Data fits on one server | Dataset exceeds one server |

---

## 15. Memory Management & Eviction

### Setting Memory Limit
```bash
# redis.conf
maxmemory 2gb           # limit Redis to 2 GB
maxmemory-policy allkeys-lru   # what to do when memory is full
```

### Eviction Policies

| Policy | Behavior | Use Case |
|--------|----------|---------|
| `noeviction` | Return error when memory full (default) | Mission-critical data |
| `allkeys-lru` | Evict least recently used key (any key) | General caching ✅ |
| `allkeys-lfu` | Evict least frequently used key | Frequency-based caching |
| `allkeys-random` | Evict a random key | Random cache |
| `volatile-lru` | Evict LRU key only from keys with TTL | Mixed persistent + cache |
| `volatile-lfu` | Evict LFU key only from keys with TTL | Mixed |
| `volatile-random` | Random eviction among keys with TTL | Mixed |
| `volatile-ttl` | Evict key with shortest remaining TTL first | Mixed |

**Production recommendation:** Use `allkeys-lru` for pure cache use cases.

### Memory Monitoring Commands
```bash
INFO memory                      # full memory report
MEMORY USAGE key                 # memory used by a specific key (in bytes)
MEMORY DOCTOR                    # advice about memory issues
MEMORY STATS                     # detailed memory stats
DEBUG JMAP                       # memory breakdown by data type
```

---

## 16. Redis Security

### 1. Require Password (AUTH)
```bash
# redis.conf
requirepass your_strong_password_here

# Connect with password
redis-cli -a your_strong_password_here
AUTH your_strong_password_here   # authenticate after connecting
```

### 2. ACL — Access Control Lists (Redis 6.0+)
Fine-grained user-level access control.
```bash
# Create a read-only user
ACL SETUSER readonly on >readpass ~* &* +GET +MGET +HGET +LRANGE

# Create a user with access to specific key patterns only
ACL SETUSER appuser on >apppass ~user:* +@all

# List all users
ACL LIST

# View current user
ACL WHOAMI

# Delete a user
ACL DELUSER readonly
```

### 3. Bind to Localhost (Never Expose to Internet)
```bash
# redis.conf
bind 127.0.0.1      # only listen on localhost
# OR bind to a private IP: bind 10.0.0.5
```

### 4. Disable Dangerous Commands
```bash
# redis.conf
rename-command FLUSHALL ""    # disable FLUSHALL
rename-command FLUSHDB ""     # disable FLUSHDB
rename-command CONFIG "ADMIN_CONFIG_9a3b"  # rename CONFIG to secret name
rename-command DEBUG ""       # disable DEBUG
```

### 5. Enable TLS (Redis 6.0+)
```bash
# redis.conf
tls-port 6380
tls-cert-file /path/to/server.crt
tls-key-file /path/to/server.key
tls-ca-cert-file /path/to/ca.crt
```

---

## 17. Redis with Python

```bash
pip install redis
```

```python
import redis
import json

# Basic connection
r = redis.Redis(host='localhost', port=6379, db=0, decode_responses=True)

# Connection with password
r = redis.Redis(host='localhost', port=6379, password='secret', decode_responses=True)

# Connection pool (recommended for production)
pool = redis.ConnectionPool(host='localhost', port=6379, max_connections=20)
r = redis.Redis(connection_pool=pool)

# Test connection
r.ping()   # → True

# ─── String ─────────────────────────────────────────────────────
r.set('name', 'Alice')
print(r.get('name'))           # → 'Alice'
r.setex('otp', 300, '4821')   # set with 5-minute TTL
r.incr('visits')

# ─── Hash ───────────────────────────────────────────────────────
r.hset('user:1001', mapping={'name': 'Alice', 'age': 30, 'city': 'Mumbai'})
print(r.hgetall('user:1001'))  # → {'name': 'Alice', 'age': '30', 'city': 'Mumbai'}
r.hincrby('user:1001', 'age', 1)

# ─── List ───────────────────────────────────────────────────────
r.rpush('tasks', 'task1', 'task2', 'task3')
print(r.lrange('tasks', 0, -1))   # → ['task1', 'task2', 'task3']
task = r.blpop('tasks', timeout=5)   # blocking pop

# ─── Set ────────────────────────────────────────────────────────
r.sadd('tags', 'python', 'redis', 'backend')
print(r.smembers('tags'))

# ─── Sorted Set ─────────────────────────────────────────────────
r.zadd('leaderboard', {'Alice': 9500, 'Bob': 8200, 'Charlie': 9500})
print(r.zrevrange('leaderboard', 0, 2, withscores=True))  # top 3

# ─── Cache-Aside Pattern ────────────────────────────────────────
def get_user(user_id):
    key = f'user:{user_id}'
    cached = r.get(key)
    if cached:
        return json.loads(cached)    # cache hit
    user = db.get_user(user_id)      # cache miss — query DB
    r.setex(key, 3600, json.dumps(user))
    return user

# ─── Pipeline (Batch Commands) ──────────────────────────────────
pipe = r.pipeline()
for i in range(1000):
    pipe.set(f'key:{i}', i)
pipe.execute()   # one round trip for all 1000 sets

# ─── Transaction ────────────────────────────────────────────────
with r.pipeline() as pipe:
    while True:
        try:
            pipe.watch('balance')
            balance = int(r.get('balance'))
            pipe.multi()
            pipe.decrby('balance', 200)
            pipe.execute()
            break
        except redis.WatchError:
            continue   # balance changed by another client — retry

# ─── Pub/Sub ────────────────────────────────────────────────────
# Publisher
r.publish('notifications', json.dumps({'type': 'message', 'text': 'Hello!'}))

# Subscriber
pubsub = r.pubsub()
pubsub.subscribe('notifications')
for message in pubsub.listen():
    if message['type'] == 'message':
        data = json.loads(message['data'])
        print(data)
```

---

## 18. Redis with Node.js

```bash
npm install ioredis
```

```javascript
const Redis = require('ioredis');

// Basic connection
const redis = new Redis({ host: 'localhost', port: 6379 });

// Connection with password
const redis = new Redis({ host: 'localhost', port: 6379, password: 'secret' });

// Test connection
await redis.ping();   // → 'PONG'

// ─── String ─────────────────────────────────────────────────────
await redis.set('name', 'Alice');
const name = await redis.get('name');          // → 'Alice'
await redis.setex('session:abc', 3600, 'data'); // with TTL
await redis.incr('visitors');

// ─── Hash ───────────────────────────────────────────────────────
await redis.hset('user:1001', 'name', 'Alice', 'age', '30');
const user = await redis.hgetall('user:1001'); // → {name: 'Alice', age: '30'}

// ─── List ───────────────────────────────────────────────────────
await redis.rpush('tasks', 'task1', 'task2');
const tasks = await redis.lrange('tasks', 0, -1);

// ─── Sorted Set ─────────────────────────────────────────────────
await redis.zadd('scores', 9500, 'Alice', 8200, 'Bob');
const top = await redis.zrevrange('scores', 0, 2, 'WITHSCORES');

// ─── Cache-Aside Pattern ────────────────────────────────────────
async function getUser(userId) {
  const key = `user:${userId}`;
  const cached = await redis.get(key);
  if (cached) return JSON.parse(cached);        // cache hit
  const user = await db.query('SELECT * FROM users WHERE id = ?', [userId]);
  await redis.setex(key, 3600, JSON.stringify(user));
  return user;
}

// ─── Pipeline ───────────────────────────────────────────────────
const pipeline = redis.pipeline();
for (let i = 0; i < 1000; i++) {
  pipeline.set(`key:${i}`, i);
}
await pipeline.exec();   // one round trip

// ─── Pub/Sub ────────────────────────────────────────────────────
// Publisher
await redis.publish('channel', JSON.stringify({ event: 'order_placed', id: 42 }));

// Subscriber (separate connection — must be dedicated)
const subscriber = new Redis();
await subscriber.subscribe('channel');
subscriber.on('message', (channel, message) => {
  const data = JSON.parse(message);
  console.log('Received:', data);
});
```

---

## 19. Redis vs Alternatives

### Redis vs Memcached

| Feature | Redis | Memcached |
|---------|-------|-----------|
| Data Types | 10+ (string, list, set, sorted set, hash, stream...) | String only |
| Persistence | ✅ RDB + AOF | ❌ Memory only |
| Pub/Sub | ✅ | ❌ |
| Clustering | ✅ Native Cluster | ✅ Basic |
| Replication | ✅ | ❌ |
| Transactions | ✅ MULTI/EXEC | ❌ |
| Lua Scripting | ✅ | ❌ |
| Memory Efficiency | Good | Slightly better for pure string cache |
| Use Case | All-purpose in-memory store | Simple, pure string cache |

**Verdict:** Choose Redis unless you specifically need Memcached's slightly better memory efficiency for pure string caching. Redis is the industry standard.

### Redis vs Kafka

| Feature | Redis | Kafka |
|---------|-------|-------|
| Type | In-memory store with Streams | Distributed event streaming |
| Throughput | 100K+ ops/sec | 1M+ msgs/sec |
| Latency | Sub-millisecond | ~5ms |
| Message Retention | Configurable (Streams) | Configurable (days to forever) |
| Replay | ✅ Streams | ✅ Native |
| Consumer Groups | ✅ Streams | ✅ Native |
| Best For | Caching + lightweight messaging | High-volume event streaming |

### Redis vs MongoDB

| Feature | Redis | MongoDB |
|---------|-------|---------|
| Storage | In-memory (RAM) | Disk-based |
| Speed | Sub-millisecond | 10–100ms |
| Data Model | Key-value with rich types | Document (BSON/JSON) |
| Query Power | Limited (key-based) | Rich queries, aggregation |
| Persistence | Optional | Always |
| Use Case | Cache, sessions, real-time | Primary persistent database |

---

## 20. Real-World Use Cases

### By Company

| Company | How They Use Redis |
|---------|-------------------|
| **Amazon / Flipkart** | Cache product details, prices, sessions → fast page loads during sale events |
| **Netflix** | Cache frequently accessed content metadata, manage real-time user sessions |
| **Facebook / Instagram** | Real-time notifications, feeds, user activity counters |
| **Uber** | Real-time driver location tracking, ride matching, surge pricing |
| **Twitter** | Timeline caching, trending topics (sorted sets), real-time counts |
| **GitHub** | Rate limiting API requests, session storage |
| **Slack** | Presence status, message queuing, real-time collaboration |

### By Pattern

| Pattern | Redis Feature | Example |
|---------|--------------|---------|
| **Caching** | String + TTL | Cache DB query results |
| **Session Store** | String/Hash + TTL | User login sessions |
| **Leaderboard** | Sorted Set | Game scores, top users |
| **Rate Limiting** | INCR + EXPIRE | API request limits |
| **Job Queue** | List (LPUSH/BRPOP) | Celery task queue |
| **Pub/Sub** | Pub/Sub | Chat, notifications |
| **Unique Visitors** | HyperLogLog | Page analytics |
| **Location** | Geospatial | Nearby drivers, stores |
| **OTP** | String + EXPIRE | One-time passwords (5 min) |
| **Distributed Lock** | SET NX EX | Prevent race conditions |

---

## 21. All Redis CLI Commands — Explained

### CONNECTING & SERVER

```bash
# Connect to Redis
redis-cli                           # connect to localhost:6379
redis-cli -h 192.168.1.10 -p 6379  # connect to specific host/port
redis-cli -a password               # connect with password
redis-cli -n 2                      # connect to database number 2

# Authentication after connecting
AUTH password

# Test connection
PING                                # → PONG
PING "hello"                        # → "hello" (echo)

# Server info
INFO                                # all server statistics
INFO server                         # server-level info
INFO memory                         # memory statistics
INFO replication                    # replication status
INFO clients                        # connected clients
INFO stats                          # command statistics
INFO keyspace                       # database info (number of keys, TTLs)

DBSIZE                              # total number of keys in current database
CONFIG GET maxmemory                # get a config value
CONFIG SET maxmemory 2gb            # set a config value at runtime
CONFIG REWRITE                      # persist runtime config changes to redis.conf

# Select database (Redis has 16 databases by default: 0-15)
SELECT 0                            # switch to database 0 (default)
SELECT 1                            # switch to database 1

# Move key to a different database
MOVE keyname 1                      # move key to database 1
```

---

### KEY MANAGEMENT

```bash
# Check if a key exists
EXISTS mykey                        # → 1 (yes) or 0 (no)
EXISTS k1 k2 k3                     # check multiple keys → count of existing

# Check the type of a value
TYPE mykey                          # → string / list / set / zset / hash / stream

# Find keys matching a pattern (AVOID on production — blocks!)
KEYS *                              # all keys
KEYS user:*                         # all keys starting with "user:"
KEYS user:*:name                    # pattern with wildcards

# Safe alternative to KEYS (non-blocking, cursor-based)
SCAN 0                              # returns cursor + batch of keys
SCAN 0 MATCH user:* COUNT 100       # scan with pattern + batch size
# Use returned cursor for next call; stop when cursor returns 0

# Delete keys
DEL key1 key2 key3                  # delete one or more keys → count deleted
UNLINK key1 key2                    # async delete (non-blocking, better for production)

# Rename a key
RENAME oldkey newkey                # rename (overwrites newkey if exists)
RENAMENX oldkey newkey              # rename ONLY if newkey doesn't exist

# Set expiry
EXPIRE key 60                       # expire in 60 seconds
PEXPIRE key 5000                    # expire in 5000 milliseconds
EXPIREAT key 1735689600             # expire at Unix timestamp
PERSIST key                         # remove expiry (make permanent)

# Check remaining TTL
TTL key                             # seconds remaining (-1 = no expiry, -2 = not found)
PTTL key                            # milliseconds remaining

# Copy a key
COPY source destination             # copy value (Redis 6.2+)

# Dump and restore (for migration)
DUMP key                            # serialize key to binary
RESTORE newkey 0 <binary>           # restore from binary
```

---

### STRING COMMANDS

```bash
SET key value                       # basic set
GET key                             # basic get
MSET k1 v1 k2 v2 k3 v3             # multi set
MGET k1 k2 k3                       # multi get
SETNX key value                     # set only if not exists
SETEX key seconds value             # set with expiry
SET key value EX 60 NX              # set with expiry + only if not exists (atomic)
GETSET key newvalue                 # get old value, set new (atomic)
GETDEL key                          # get and delete
APPEND key " more text"            # append to string
STRLEN key                          # length of string value
INCR key                            # increment integer by 1
INCRBY key 10                       # increment by 10
DECR key                            # decrement by 1
DECRBY key 5                        # decrement by 5
INCRBYFLOAT key 1.5                 # increment float
GETRANGE key 0 4                    # substring (inclusive)
SETRANGE key 6 "world"             # overwrite part of string
```

---

### HASH COMMANDS

```bash
HSET key field value                # set one field
HSET key f1 v1 f2 v2               # set multiple fields
HGET key field                      # get one field
HMGET key f1 f2                     # get multiple fields
HGETALL key                         # get all field-value pairs
HKEYS key                           # all field names
HVALS key                           # all values
HLEN key                            # number of fields
HEXISTS key field                   # does field exist?
HDEL key field1 field2              # delete fields
HINCRBY key field 5                 # increment integer field
HINCRBYFLOAT key field 1.5         # increment float field
HSETNX key field value             # set field only if not exists
HRANDFIELD key 2                    # get 2 random field names
HRANDFIELD key 2 WITHVALUES        # get 2 random fields with values
```

---

### LIST COMMANDS

```bash
LPUSH key val1 val2                 # push to left (head)
RPUSH key val1 val2                 # push to right (tail)
LPOP key                            # pop from left
RPOP key                            # pop from right
LPOP key 3                          # pop 3 from left (Redis 6.2+)
BLPOP key 5                         # blocking pop left (wait 5s for item)
BRPOP key 5                         # blocking pop right
LRANGE key 0 -1                     # get all elements
LLEN key                            # list length
LINDEX key 0                        # element at index
LSET key 0 "newvalue"              # set element at index
LINSERT key BEFORE pivot value      # insert before element
LINSERT key AFTER pivot value       # insert after element
LREM key 0 value                    # remove all occurrences of value
LTRIM key 0 99                      # keep only elements 0–99
LMOVE src dst LEFT RIGHT           # atomically move element between lists
```

---

### SET COMMANDS

```bash
SADD key m1 m2 m3                  # add members
SREM key m1 m2                     # remove members
SMEMBERS key                        # get all members
SCARD key                           # count of members
SISMEMBER key member                # check if member exists → 0/1
SMISMEMBER key m1 m2               # check multiple → [1, 0]
SPOP key                            # remove and return random member
SRANDMEMBER key 3                   # get 3 random members (no removal)
SUNION s1 s2                        # union (all members)
SINTER s1 s2                        # intersection (common)
SDIFF s1 s2                         # difference (in s1 not in s2)
SUNIONSTORE dest s1 s2              # union stored in dest
SINTERSTORE dest s1 s2              # intersection stored in dest
SMOVE s1 s2 member                  # move member from s1 to s2
```

---

### SORTED SET COMMANDS

```bash
ZADD key score member               # add member with score
ZADD key NX score member            # add only if not exists
ZADD key XX score member            # update only if exists
ZADD key GT score member            # update only if new score > current (Redis 6.2+)
ZADD key LT score member            # update only if new score < current
ZSCORE key member                   # get score of member
ZRANK key member                    # rank (0-indexed, low → high score)
ZREVRANK key member                 # rank (0-indexed, high → low score)
ZINCRBY key 100 member              # increment score by 100
ZCARD key                           # count of members
ZCOUNT key 8000 10000              # count with score in range
ZRANGE key 0 -1                     # all members (low → high)
ZRANGE key 0 -1 WITHSCORES        # with scores
ZRANGE key 0 2 REV WITHSCORES     # top 3 (high → low) in Redis 6.2+
ZREVRANGE key 0 2 WITHSCORES      # top 3 (older syntax)
ZRANGEBYSCORE key 8000 +inf        # members with score >= 8000
ZRANGEBYSCORE key -inf +inf WITHSCORES LIMIT 0 10  # paginated
ZRANGEBYLEX key "[a" "[z"          # range by lexicographic order (same score)
ZREM key m1 m2                     # remove members
ZPOPMAX key                         # remove and return highest score member
ZPOPMIN key                         # remove and return lowest score member
BZPOPMAX key 5                      # blocking zpopmax
ZREMRANGEBYSCORE key -inf 7000     # remove all with score <= 7000
ZREMRANGEBYRANK key 0 2            # remove by rank
ZUNIONSTORE dest 2 z1 z2           # union of sorted sets
ZINTERSTORE dest 2 z1 z2           # intersection
ZDIFFSTORE dest 2 z1 z2            # difference
ZRANDMEMBER key 3 WITHSCORES      # get random members
```

---

### SERVER MAINTENANCE

```bash
# Flush data (USE WITH CAUTION!)
FLUSHDB                             # delete all keys in current database
FLUSHALL                            # delete ALL keys in ALL databases
FLUSHDB ASYNC                       # flush asynchronously (non-blocking)

# Persistence
SAVE                                # synchronous snapshot (blocks)
BGSAVE                              # background snapshot (non-blocking ✅)
BGREWRITEAOF                        # rewrite AOF file in background
LASTSAVE                            # Unix timestamp of last successful BGSAVE

# Debugging
DEBUG SLEEP 5                       # make server sleep 5 seconds (for testing)
DEBUG RELOAD                        # reload RDB from disk
OBJECT ENCODING key                 # internal encoding of a key's value
OBJECT IDLETIME key                 # seconds since key was last accessed
OBJECT FREQ key                     # access frequency (when LFU policy is used)
OBJECT HELP                         # help for OBJECT subcommands

# Monitor all commands in real time (dev only — performance impact!)
MONITOR

# Slow log
SLOWLOG GET 10                      # last 10 slow commands
SLOWLOG LEN                         # count of slow log entries
SLOWLOG RESET                       # clear slow log

# Connected clients
CLIENT LIST                         # list all connected clients
CLIENT GETNAME                      # name of current connection
CLIENT SETNAME "myapp"             # name the current connection
CLIENT ID                           # get current connection ID
CLIENT KILL ID 5                    # disconnect client with ID 5

# Shutdown
SHUTDOWN SAVE                       # flush to disk and exit
SHUTDOWN NOSAVE                     # exit without flushing (data lost)
DEBUG OBJECT key                    # detailed debug info for a key
```

---

## 22. Best Practices

### ✅ DOs

**Keys:**
- Use **descriptive, structured key names** — `user:1001:profile` not `u1001`
- Keep keys **short but meaningful** — long keys waste memory
- Always set a **TTL on cache keys** — prevents stale data and memory bloat
- Use **namespaces with colons** — `object:id:field` for organization

**Memory:**
- Always set **`maxmemory`** in production — prevents Redis from consuming all RAM
- Set `maxmemory-policy allkeys-lru` for pure cache workloads
- Monitor with `INFO memory` and set up alerts before hitting the limit
- Use `MEMORY USAGE key` to find unexpectedly large keys

**Performance:**
- Use **pipelining** for bulk operations — one round trip instead of N
- Use **connection pooling** in your application — never create a new connection per request
- Use `SCAN` instead of `KEYS *` in production — KEYS blocks the entire server
- Use `UNLINK` instead of `DEL` for large keys — async deletion doesn't block

**Persistence:**
- Use **both RDB + AOF** in production
- Set `appendfsync everysec` — best balance of performance and durability
- **Back up RDB files** regularly to external storage (S3, etc.)
- Test your restore procedure — a backup you've never tested is not a backup

**High Availability:**
- Use **Sentinel** (minimum 3) for HA with a single primary
- Use **Cluster** when data outgrows one server's memory
- Always have at least **one replica** per primary

### ❌ DON'Ts

- Never use `KEYS *` in production — it blocks Redis for seconds on large datasets (use SCAN)
- Never use `FLUSHALL` or `FLUSHDB` without thinking twice in production
- Never expose Redis port (6379) to the internet — always bind to private IP
- Never store sensitive data without encryption at rest + ACL protection
- Never skip TTL on cache keys — unbounded caches fill up RAM
- Never use `MONITOR` in production under load — it copies every command and causes severe slowdown
- Never use large keys (avoid storing big blobs) — hash them or store references
- Never rely on Redis as the only data store without persistence configured

---

## 23. Common Interview Questions

### Q1: What is Redis and what makes it different from a traditional database?
**Answer:** Redis is an open-source, in-memory data structure store. Unlike traditional disk-based databases (MySQL, PostgreSQL), Redis stores all data in RAM, giving sub-millisecond response times. It supports rich data structures beyond simple key-value: lists, sets, sorted sets, hashes, streams, and more. It can be used as a cache, primary database (with persistence), message broker, or session store.

### Q2: What data types does Redis support?
**Answer:** Redis supports: String (text, numbers, binary), List (ordered sequence, use as queue/stack), Set (unique unordered collection), Sorted Set (unique members with float scores, ordered by score), Hash (field-value map like a dict), Bitmap (bit-level operations), HyperLogLog (approximate unique count), Geospatial (lat/lng queries), and Stream (persistent append-only log for messaging).

### Q3: What is the difference between RDB and AOF persistence?
**Answer:** RDB creates point-in-time binary snapshots at configured intervals — compact files, fast to load, but data loss between snapshots is possible. AOF logs every write command — better durability (max 1 second data loss with everysec), but larger files that grow over time. In production, use both: AOF for durability and RDB for fast backups and restarts.

### Q4: How does Redis handle expiration of keys?
**Answer:** You set a TTL using `EXPIRE key seconds`, `SETEX key seconds value`, or `SET key value EX seconds`. Redis uses two mechanisms: lazy expiration (checks TTL when key is accessed) and active expiration (periodic background scan of random keys to remove expired ones). `TTL key` returns remaining seconds; -1 means no expiry; -2 means key is gone.

### Q5: What is the difference between Redis Sentinel and Redis Cluster?
**Answer:** Sentinel provides high availability for a single primary setup — it monitors the primary and replicas, automatically fails over to a replica if the primary goes down. The entire dataset still lives on one primary. Cluster provides both high availability AND horizontal scaling by sharding data across multiple primary nodes using 16,384 hash slots — you can store more data than fits on one server and distribute the write load.

### Q6: What are the Redis eviction policies and when do you use them?
**Answer:** When `maxmemory` is hit, Redis evicts keys based on the configured policy. Key policies: `allkeys-lru` — evict least recently used key from all keys (best for general cache); `volatile-lru` — LRU only among keys with TTL (when you have mixed persistent + cache data); `noeviction` — return error (for critical non-cache data). For pure caching, always use `allkeys-lru`.

### Q7: What is the difference between Pub/Sub and Redis Streams?
**Answer:** Pub/Sub is fire-and-forget — if no subscriber is connected when a message is published, the message is permanently lost. Streams are a persistent, durable append-only log where messages survive even without active consumers. Streams also support consumer groups (competing consumers, acknowledgments, and replay from any point) — similar to Kafka. Use Pub/Sub for transient broadcasts; use Streams when delivery guarantees matter.

### Q8: What is pipelining in Redis and why is it important?
**Answer:** Pipelining allows sending multiple Redis commands in a single network round trip instead of one round trip per command. Without pipelining, each command waits for the server's reply before sending the next — network latency multiplies. With pipelining, you buffer all commands and send them at once. This can improve throughput by 10–100x for bulk operations. Note: pipelining is NOT atomic (use MULTI/EXEC for atomicity).

### Q9: How does the Cache-Aside (Lazy Loading) pattern work?
**Answer:** The application first checks Redis for the data. If found (cache hit), it returns immediately — no database query needed. If not found (cache miss), the app queries the main database, stores the result in Redis with a TTL for future requests, then returns the data to the caller. The cache is only populated on demand — data not frequently accessed never enters the cache. This is the most common Redis caching pattern.

### Q10: Why is Redis single-threaded and does that limit its performance?
**Answer:** Redis processes commands on a single main thread, which eliminates locking overhead, context switching, and race conditions between threads. This simplifies the implementation and avoids synchronization bugs. Despite being single-threaded, Redis handles 100,000+ operations per second because all operations are in-memory (no disk I/O) and the event loop processes commands with non-blocking I/O. Since Redis 6.0, I/O reading and writing are threaded while command processing remains single-threaded.

---

## 🔄 Redis — Complete Flow at a Glance

```
Client Request
      ↓
Application Layer
      ↓
Redis (in-memory cache, sub-ms) ←──── Cache Hit → Return instantly
      ↓ Cache Miss
Main Database (disk, 50-200ms)
      ↓
Store result in Redis with TTL
      ↓
Return to client
      ↓
Next request for same data → Cache Hit → Return in microseconds ✅
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Redis | Open-source in-memory data store; cache + database + message broker |
| In-Memory | All data in RAM → sub-millisecond response |
| String | Basic key-value; supports INCR, APPEND, TTL |
| List | Ordered sequence; use as queue (RPUSH/BLPOP) or stack (LPUSH/LPOP) |
| Set | Unique unordered collection; supports UNION, INTER, DIFF |
| Sorted Set | Unique members with float scores; ordered by score; perfect for leaderboards |
| Hash | Field-value map under one key; like a Python dict |
| HyperLogLog | Approximate unique count; ~12KB regardless of set size; 0.81% error |
| Geospatial | Stores coordinates; queries by radius or distance |
| Stream | Persistent append-only log; consumer groups; durable messaging |
| TTL | Time-To-Live; key auto-deleted after set duration |
| EXPIRE | Set expiry on existing key (seconds) |
| SETEX | Set key + value + expiry in one command |
| RDB | Snapshot persistence; compact, fast restore, possible data loss |
| AOF | Command log persistence; better durability (max 1s loss with everysec) |
| Eviction | What Redis does when maxmemory is full (allkeys-lru = best for cache) |
| Pub/Sub | Fire-and-forget messaging; no persistence; fast real-time broadcasts |
| MULTI/EXEC | Redis transaction; commands queued then executed atomically |
| Pipeline | Batch commands in one round trip; not atomic; max throughput |
| WATCH | Optimistic locking; abort transaction if watched key changes |
| Replication | Primary → Replicas; async; read scaling + HA foundation |
| Sentinel | HA for single primary; monitors + auto-failover; needs 3 sentinels |
| Cluster | Horizontal scaling; 16,384 hash slots; multiple primaries + replicas |
| Cache-Aside | App checks Redis first; on miss queries DB, stores in Redis with TTL |
| Write-Through | Write to DB + cache simultaneously; consistent but slower writes |
| SCAN | Safe alternative to KEYS *; cursor-based, non-blocking |
| MONITOR | Real-time command logging; dev only, severe production impact |
| ACL | Access Control Lists (Redis 6+); per-user permissions |
| maxmemory | Cap Redis RAM usage; always set in production |
| CONNECTION POOL | Reuse connections; never create new connection per request |

---

> 💡 **Production Golden Rule for Redis:**
> Always combine — **TTL on all cache keys + maxmemory with allkeys-lru + Both RDB+AOF persistence + Connection Pooling + Sentinel (or Cluster) for HA + ACL for security + SCAN instead of KEYS + UNLINK instead of DEL**.
> These form the foundation of a reliable, secure, high-performance Redis deployment.

---
*Notes prepared with research from: Redis Official Docs, DevToolbox (Feb 2026), Calmops (Mar 2026), DragonflyDB Guide (2025), LabEx Interview Guide (2025), Generalist Programmer Tutorial (2025), CodePress Academy (Apr 2026)*
