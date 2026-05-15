# 🍃 MongoDB — Complete Study Notes
> **After reading this, no concept of MongoDB will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is MongoDB?](#1-what-is-mongodb)
2. [NoSQL vs SQL — The Core Difference](#2-nosql-vs-sql--the-core-difference)
3. [Core Concepts](#3-core-concepts)
4. [MongoDB Architecture](#4-mongodb-architecture)
5. [BSON & Data Types](#5-bson--data-types)
6. [CRUD Operations](#6-crud-operations)
7. [Query Operators](#7-query-operators)
8. [Update Operators](#8-update-operators)
9. [Aggregation Pipeline](#9-aggregation-pipeline)
10. [Indexes](#10-indexes)
11. [Data Modeling & Relationships](#11-data-modeling--relationships)
12. [Transactions](#12-transactions)
13. [Replication](#13-replication)
14. [Sharding](#14-sharding)
15. [MongoDB Security](#15-mongodb-security)
16. [MongoDB vs MySQL vs Redis](#16-mongodb-vs-mysql-vs-redis)
17. [Complete MongoDB Commands Reference](#17-complete-mongodb-commands-reference)
18. [MongoDB with Python (PyMongo)](#18-mongodb-with-python-pymongo)
19. [MongoDB with Node.js (Mongoose)](#19-mongodb-with-nodejs-mongoose)
20. [Common Use Cases](#20-common-use-cases)
21. [Best Practices](#21-best-practices)
22. [Common Interview Questions](#22-common-interview-questions)

---

## 1. What is MongoDB?

MongoDB is an **open-source, document-oriented NoSQL database** that stores data in flexible, JSON-like documents instead of rigid rows and columns.

### Simple Analogy 🗂️
> Think of MongoDB as a **Filing Cabinet Full of Folders**.
> - **Database** = The entire filing cabinet
> - **Collection** = A labeled drawer (e.g., "Users", "Orders", "Products")
> - **Document** = A single file/folder inside the drawer — each folder can have different content
> - **Field** = A piece of information written on the file (name, age, address)
> - **`_id`** = The unique serial number printed on every file — no two files have the same number
> - **Index** = A sticky tab that lets you jump directly to any file without scanning the whole drawer
> - **Aggregation Pipeline** = A processing conveyor belt — put raw files in, get analyzed reports out

### Key Points
- Written in **C++**, released in **2009** by **10gen** (now MongoDB Inc.)
- Stores data as **BSON** (Binary JSON) — fast and compact
- **Schema-less (flexible schema)** — each document in a collection can have different fields
- **Horizontally scalable** via sharding — spread data across many servers
- **Highly available** via replica sets — automatic failover
- Used by **eBay, Uber, Adobe, LinkedIn, Facebook, Bosch** and thousands more
- Available as **MongoDB Community** (free) and **MongoDB Atlas** (managed cloud)

---

## 2. NoSQL vs SQL — The Core Difference

### SQL (Relational) Mindset
```
Users Table:
┌────┬───────────┬─────┬────────────────────┐
│ id │ name      │ age │ email              │
├────┼───────────┼─────┼────────────────────┤
│  1 │ Alice     │  28 │ alice@example.com  │
│  2 │ Bob       │  35 │ bob@example.com    │
└────┴───────────┴─────┴────────────────────┘
Every row must have the exact same columns. Schema is FIXED.
Adding a new field? ALTER TABLE — risky on large tables.
```

### MongoDB (Document) Mindset
```javascript
// Users Collection — each document can look different
{ "_id": 1, "name": "Alice", "age": 28, "email": "alice@example.com" }
{ "_id": 2, "name": "Bob",   "age": 35, "email": "bob@example.com", "phone": "9876543210" }
{ "_id": 3, "name": "Carol", "skills": ["Python", "MongoDB"], "address": { "city": "Mumbai", "zip": "400001" } }

Schema is FLEXIBLE. No ALTER TABLE. Just add new fields to new documents.
```

### When to Choose MongoDB vs SQL

| Choose **MongoDB** when | Choose **SQL** when |
|------------------------|---------------------|
| Data structure varies per record | Strict, uniform schema needed |
| Rapid development, changing requirements | Data has many relationships (JOINs needed) |
| Large volumes of unstructured/semi-structured data | ACID transactions are critical everywhere |
| Need horizontal scaling (sharding) | Complex reporting and analytics (BI tools) |
| Real-time apps, catalogs, content management | Financial systems, ERP, banking |
| JSON-heavy APIs — data maps naturally to documents | Data integrity constraints are paramount |

---

## 3. Core Concepts

### 3.1 Database
- A container that holds a group of **collections**
- Similar to a database in SQL
- One MongoDB server can host **multiple databases**
- Default system databases: `admin`, `local`, `config`

### 3.2 Collection
- A group of **MongoDB documents** — similar to a SQL table
- Does NOT enforce a schema — documents inside can have different structures
- Created automatically when you first insert a document into it
- Name is case-sensitive: `users` ≠ `Users`

### 3.3 Document
- The basic unit of data in MongoDB — equivalent to a SQL row
- Stored as **BSON** (Binary JSON) internally
- Maximum size: **16 MB per document**
- Can contain nested documents (sub-documents) and arrays
- Every document has a unique **`_id`** field (auto-generated if not provided)

```javascript
// A rich document example — notice nested objects and arrays
{
  "_id": ObjectId("64a1f2b3c4d5e6f7a8b9c0d1"),
  "name": "Alice Johnson",
  "age": 28,
  "email": "alice@example.com",
  "isActive": true,
  "joinedAt": ISODate("2024-01-15T10:30:00Z"),
  "address": {
    "street": "123 MG Road",
    "city": "Bangalore",
    "state": "Karnataka",
    "zip": "560001"
  },
  "skills": ["Python", "MongoDB", "React"],
  "scores": [95, 87, 92],
  "metadata": null
}
```

### 3.4 ObjectId
- MongoDB's default `_id` type — a **12-byte unique identifier**
- Structure:
  ```
  64a1f2b3  c4d5  e6f7  a8b9c0d1
  ────────  ────  ────  ────────
  4 bytes   3 bytes 2b   3 bytes
  timestamp machineID  processID  counter
  ```
- Sortable by creation time — newer ObjectIds are always "greater"
- `ObjectId.getTimestamp()` tells you exactly when a document was created

### 3.5 Field
- A key-value pair inside a document
- Key = string, Value = any BSON type (string, number, array, object, boolean, date, null...)
- Field names cannot start with `$` or contain `.` (these are reserved)

---

## 4. MongoDB Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                         Your Application                                 │
│                   (Python / Node.js / Java / Go)                        │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │  MongoDB Driver (sends wire protocol)
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                         mongod Process                                   │
│                    (The MongoDB Server Daemon)                           │
│                                                                          │
│  ┌──────────────────────────────────────────────────────────────────┐   │
│  │                      Storage Engine                               │   │
│  │         WiredTiger (default) — document-level locking            │   │
│  │         In-Memory Engine — for pure in-memory workloads          │   │
│  └──────────────────────────────────────────────────────────────────┘   │
│                                                                          │
│  ┌────────────────┐  ┌─────────────────┐  ┌──────────────────────────┐ │
│  │   Query Engine │  │  Index Manager  │  │  Replication (oplog)     │ │
│  │  (finds docs,  │  │  (B-Tree, text, │  │  (primary → secondaries) │ │
│  │   runs plans)  │  │   geo, hashed)  │  │                          │ │
│  └────────────────┘  └─────────────────┘  └──────────────────────────┘ │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
               ┌───────────────┼───────────────┐
               ▼               ▼               ▼
        ┌────────────┐  ┌────────────┐  ┌────────────┐
        │  Database  │  │  Database  │  │  Database  │
        │  "shopdb"  │  │  "authdb"  │  │  "logdb"   │
        │            │  │            │  │            │
        │ Collections│  │ Collections│  │ Collections│
        │  - products│  │  - users   │  │  - events  │
        │  - orders  │  │  - sessions│  │  - errors  │
        └────────────┘  └────────────┘  └────────────┘
```

### mongod vs mongos vs mongoc

| Process | Role |
|---------|------|
| **mongod** | The main database server daemon — stores and serves data |
| **mongos** | The query router for sharded clusters — routes requests to the right shard |
| **mongo / mongosh** | The interactive shell — where you type commands |
| **mongodump / mongorestore** | Backup and restore tools |
| **mongoexport / mongoimport** | Export/import data as JSON or CSV |

---

## 5. BSON & Data Types

### What is BSON?
- **B**inary **JSON** — MongoDB's wire and storage format
- Extends JSON with extra types: `Date`, `ObjectId`, `Int32`, `Int64`, `Decimal128`, `Binary`
- More compact and faster to parse than plain JSON

### BSON Data Types

| Type | Example | Notes |
|------|---------|-------|
| `String` | `"Alice"` | UTF-8 encoded |
| `Integer (32-bit)` | `NumberInt(42)` | 32-bit signed |
| `Integer (64-bit)` | `NumberLong(9999999999)` | 64-bit signed |
| `Double` | `3.14` | 64-bit floating point |
| `Decimal128` | `NumberDecimal("9.99")` | High-precision — use for money |
| `Boolean` | `true` / `false` | |
| `Date` | `ISODate("2024-01-15")` | Milliseconds since Unix epoch |
| `ObjectId` | `ObjectId("64a1...")` | 12-byte unique ID |
| `Array` | `["a", "b", "c"]` | Ordered list of values |
| `Object` | `{ "city": "Mumbai" }` | Embedded sub-document |
| `Null` | `null` | Absence of value |
| `Binary` | `BinData(0, "...")` | Raw binary data, file storage |
| `Regex` | `/^alice/i` | Regular expression |
| `Timestamp` | `Timestamp(1, 1)` | Internal replication use |

> ⚠️ **Money tip:** Never use `Double` for monetary values — floating point errors! Use `Decimal128` or store as integer cents.

---

## 6. CRUD Operations

### 6.1 Create — Insert Documents

```javascript
// Insert ONE document
db.users.insertOne({
  name: "Alice",
  age: 28,
  email: "alice@example.com",
  city: "Mumbai"
})
// Returns: { acknowledged: true, insertedId: ObjectId("...") }

// Insert MANY documents at once
db.users.insertMany([
  { name: "Bob",   age: 35, email: "bob@example.com",   city: "Delhi" },
  { name: "Carol", age: 22, email: "carol@example.com", city: "Pune"  },
  { name: "Dave",  age: 31, email: "dave@example.com",  city: "Mumbai"}
])
// Returns: { acknowledged: true, insertedIds: { 0: ObjectId, 1: ObjectId, 2: ObjectId } }

// Insert with a custom _id (you choose the ID)
db.products.insertOne({ _id: "iphone-15-pro", name: "iPhone 15 Pro", price: 134900 })
```

### 6.2 Read — Find Documents

```javascript
// Find ALL documents in a collection
db.users.find()

// Find with pretty formatting (mongosh auto-formats now)
db.users.find().pretty()

// Find ONE document (returns first match)
db.users.findOne({ city: "Mumbai" })

// Find with a filter — all users in Mumbai
db.users.find({ city: "Mumbai" })

// Find with multiple conditions (implicit AND)
db.users.find({ city: "Mumbai", age: 28 })

// Projection — include only specific fields (1 = include, 0 = exclude)
db.users.find({ city: "Mumbai" }, { name: 1, email: 1, _id: 0 })

// Find and count results
db.users.countDocuments({ city: "Mumbai" })
db.users.estimatedDocumentCount()  // fast approximate count

// Sort results (1 = ascending, -1 = descending)
db.users.find().sort({ age: -1 })        // oldest first
db.users.find().sort({ name: 1 })        // alphabetical

// Limit results
db.users.find().limit(5)                  // first 5 only

// Skip documents (for pagination)
db.users.find().skip(10).limit(5)         // page 3, 5 per page

// Chaining: filter + sort + skip + limit
db.users.find({ city: "Mumbai" }).sort({ age: 1 }).skip(0).limit(10)
```

### 6.3 Update — Modify Documents

```javascript
// Update ONE document (first match)
db.users.updateOne(
  { name: "Alice" },           // filter — find this document
  { $set: { age: 29 } }        // update — change age to 29
)

// Update MANY documents
db.users.updateMany(
  { city: "Mumbai" },                      // filter — all Mumbai users
  { $set: { country: "India" } }           // add/update a field
)

// Replace an entire document (only _id stays, everything else replaced)
db.users.replaceOne(
  { name: "Alice" },
  { name: "Alice", age: 30, email: "alice.new@example.com", city: "Bangalore" }
)

// Upsert — update if exists, insert if not
db.users.updateOne(
  { email: "newuser@example.com" },
  { $set: { name: "New User", age: 25 } },
  { upsert: true }
)

// findOneAndUpdate — find, update, and return the document
db.users.findOneAndUpdate(
  { name: "Alice" },
  { $set: { age: 30 } },
  { returnDocument: "after" }   // return the updated version
)
```

### 6.4 Delete — Remove Documents

```javascript
// Delete ONE document (first match)
db.users.deleteOne({ name: "Alice" })

// Delete MANY documents
db.users.deleteMany({ city: "Mumbai" })

// Delete ALL documents in a collection (keeps the collection itself)
db.users.deleteMany({})

// findOneAndDelete — find, delete, and return the deleted document
db.users.findOneAndDelete({ name: "Bob" })

// Drop the entire collection (removes collection + all documents + indexes)
db.users.drop()

// Drop the entire database
db.dropDatabase()
```

---

## 7. Query Operators

### 7.1 Comparison Operators

```javascript
// $eq — equal to (same as just writing the value)
db.users.find({ age: { $eq: 28 } })
db.users.find({ age: 28 })              // shorthand — same result

// $ne — not equal
db.users.find({ city: { $ne: "Mumbai" } })

// $gt — greater than
db.users.find({ age: { $gt: 25 } })

// $gte — greater than or equal
db.users.find({ age: { $gte: 25 } })

// $lt — less than
db.users.find({ age: { $lt: 30 } })

// $lte — less than or equal
db.users.find({ age: { $lte: 30 } })

// $in — value is in the given array
db.users.find({ city: { $in: ["Mumbai", "Delhi", "Bangalore"] } })

// $nin — value is NOT in the given array
db.users.find({ city: { $nin: ["Chennai", "Kolkata"] } })
```

### 7.2 Logical Operators

```javascript
// $and — all conditions must be true
db.users.find({
  $and: [
    { age: { $gte: 25 } },
    { city: "Mumbai" }
  ]
})
// Shorthand (implicit AND — same result):
db.users.find({ age: { $gte: 25 }, city: "Mumbai" })

// $or — at least one condition must be true
db.users.find({
  $or: [
    { city: "Mumbai" },
    { city: "Delhi" }
  ]
})

// $nor — none of the conditions can be true
db.users.find({
  $nor: [
    { city: "Mumbai" },
    { age: { $lt: 18 } }
  ]
})

// $not — negate a condition
db.users.find({ age: { $not: { $gt: 30 } } })  // age <= 30
```

### 7.3 Element Operators

```javascript
// $exists — field exists (or does not exist)
db.users.find({ phone: { $exists: true } })     // has a phone field
db.users.find({ phone: { $exists: false } })    // does not have a phone field

// $type — field is of a specific BSON type
db.users.find({ age: { $type: "int" } })
db.users.find({ age: { $type: "string" } })
```

### 7.4 Array Operators

```javascript
// $all — array contains ALL specified values
db.users.find({ skills: { $all: ["Python", "MongoDB"] } })

// $elemMatch — at least one array element matches ALL conditions
db.orders.find({
  items: { $elemMatch: { price: { $gt: 100 }, qty: { $gte: 2 } } }
})

// $size — array has exactly N elements
db.users.find({ skills: { $size: 3 } })
```

### 7.5 Evaluation Operators

```javascript
// $regex — match against a regular expression
db.users.find({ email: { $regex: "@gmail.com$" } })
db.users.find({ name: { $regex: "^ali", $options: "i" } })  // case-insensitive

// $where — JavaScript expression (slow — avoid in production)
db.users.find({ $where: "this.age > 25 && this.city === 'Mumbai'" })

// $expr — use aggregation expressions in a query
db.orders.find({
  $expr: { $gt: ["$totalPrice", "$budget"] }   // compare two fields
})
```

---

## 8. Update Operators

### 8.1 Field Update Operators

```javascript
// $set — set field value (add if doesn't exist)
db.users.updateOne({ name: "Alice" }, { $set: { age: 30, city: "Pune" } })

// $unset — remove a field entirely
db.users.updateOne({ name: "Alice" }, { $unset: { phone: "" } })

// $inc — increment (or decrement with negative value) a number
db.products.updateOne({ _id: "prod1" }, { $inc: { stock: -1 } })   // decrease stock
db.users.updateOne({ name: "Alice" }, { $inc: { loginCount: 1 } }) // increase counter

// $mul — multiply a field's value
db.products.updateOne({ _id: "prod1" }, { $mul: { price: 1.1 } })  // 10% price increase

// $rename — rename a field
db.users.updateMany({}, { $rename: { "fname": "firstName" } })

// $min — update field only if new value is LESS than current value
db.scores.updateOne({ user: "Alice" }, { $min: { lowestScore: 45 } })

// $max — update field only if new value is GREATER than current value
db.scores.updateOne({ user: "Alice" }, { $max: { highScore: 98 } })

// $currentDate — set field to current date
db.users.updateOne(
  { name: "Alice" },
  { $currentDate: { lastModified: true, lastLogin: { $type: "timestamp" } } }
)
```

### 8.2 Array Update Operators

```javascript
// $push — add an element to the end of an array
db.users.updateOne({ name: "Alice" }, { $push: { skills: "Docker" } })

// $push with $each — add multiple elements
db.users.updateOne(
  { name: "Alice" },
  { $push: { skills: { $each: ["Kubernetes", "Redis"] } } }
)

// $push with $each + $sort + $slice — add, sort, and keep only top N
db.users.updateOne(
  { name: "Alice" },
  { $push: { scores: { $each: [88, 95], $sort: -1, $slice: 5 } } }
)

// $pop — remove the first (-1) or last (1) element of an array
db.users.updateOne({ name: "Alice" }, { $pop: { skills: 1 } })   // remove last
db.users.updateOne({ name: "Alice" }, { $pop: { skills: -1 } })  // remove first

// $pull — remove all elements that match a condition
db.users.updateOne({ name: "Alice" }, { $pull: { skills: "Docker" } })
db.users.updateMany({}, { $pull: { scores: { $lt: 50 } } })  // remove low scores

// $pullAll — remove all matching values from array
db.users.updateOne({ name: "Alice" }, { $pullAll: { skills: ["Docker", "Redis"] } })

// $addToSet — add element only if it does NOT already exist (no duplicates)
db.users.updateOne({ name: "Alice" }, { $addToSet: { skills: "Python" } })

// $ (positional) — update first array element that matches filter
db.users.updateOne(
  { name: "Alice", skills: "Python" },
  { $set: { "skills.$": "Python 3.11" } }
)
```

---

## 9. Aggregation Pipeline

The aggregation pipeline processes documents through a series of **stages**. Output of one stage is input to the next.

```
Collection → [$match] → [$group] → [$sort] → [$project] → [$limit] → Result
```

### Core Pipeline Stages

```javascript
// Full example: Monthly revenue report
db.orders.aggregate([

  // Stage 1: $match — filter documents (like WHERE in SQL)
  { $match: { status: "completed", createdAt: { $gte: ISODate("2024-01-01") } } },

  // Stage 2: $group — group and aggregate (like GROUP BY in SQL)
  {
    $group: {
      _id: { $month: "$createdAt" },   // group by month
      totalRevenue: { $sum: "$amount" },
      orderCount: { $count: {} },
      avgOrder: { $avg: "$amount" },
      maxOrder: { $max: "$amount" },
      minOrder: { $min: "$amount" }
    }
  },

  // Stage 3: $sort — sort results (like ORDER BY)
  { $sort: { "_id": 1 } },

  // Stage 4: $project — reshape output (like SELECT in SQL)
  {
    $project: {
      month: "$_id",
      totalRevenue: { $round: ["$totalRevenue", 2] },
      orderCount: 1,
      avgOrder: { $round: ["$avgOrder", 2] },
      _id: 0
    }
  },

  // Stage 5: $limit — keep only N results
  { $limit: 12 }
])
```

### All Important Pipeline Stages

```javascript
// $match — filter documents
{ $match: { status: "active" } }

// $group — group and calculate aggregates
{ $group: { _id: "$city", count: { $sum: 1 }, total: { $sum: "$price" } } }

// $project — include/exclude/reshape fields
{ $project: { name: 1, email: 1, fullName: { $concat: ["$firstName", " ", "$lastName"] } } }

// $sort — sort documents
{ $sort: { price: -1 } }

// $limit — keep first N
{ $limit: 10 }

// $skip — skip first N (pagination)
{ $skip: 20 }

// $unwind — deconstruct array into individual documents (one doc per array element)
{ $unwind: "$skills" }
// { skills: ["Python","Go"] } → { skills: "Python" } + { skills: "Go" }

// $lookup — LEFT JOIN with another collection
{
  $lookup: {
    from: "orders",          // the other collection
    localField: "_id",       // field in current collection
    foreignField: "userId",  // field in orders collection
    as: "userOrders"         // output array field name
  }
}

// $addFields — add new computed fields (keeps existing fields)
{ $addFields: { fullName: { $concat: ["$firstName", " ", "$lastName"] } } }

// $replaceRoot — replace the document with a sub-document
{ $replaceRoot: { newRoot: "$address" } }

// $count — count documents and output single result
{ $count: "totalUsers" }

// $out — write aggregation results to a new collection
{ $out: "monthly_summary" }

// $bucket — group into ranges (e.g., age groups)
{
  $bucket: {
    groupBy: "$age",
    boundaries: [0, 18, 30, 50, 100],
    default: "Other",
    output: { count: { $sum: 1 } }
  }
}

// $facet — run multiple pipelines in parallel on the same dataset
{
  $facet: {
    byCity: [ { $group: { _id: "$city", count: { $sum: 1 } } } ],
    byAge:  [ { $bucket: { groupBy: "$age", boundaries: [18, 30, 50] } } ]
  }
}
```

---

## 10. Indexes

### Why Indexes Matter
```
Without an index:
db.users.find({ email: "alice@example.com" })
→ MongoDB scans EVERY document (COLLSCAN) — O(n) — slow for large collections

With an index on "email":
→ MongoDB jumps directly to the document (IXSCAN) — O(log n) — always fast
```

### Index Types and Creation

```javascript
// ─── Single Field Index ───────────────────────────────────────────────────
// Create an ascending index on email (most queries use this field)
db.users.createIndex({ email: 1 })

// Create a descending index on createdAt (for "latest first" queries)
db.users.createIndex({ createdAt: -1 })


// ─── Unique Index ──────────────────────────────────────────────────────────
// Enforce uniqueness — no two users can have the same email
db.users.createIndex({ email: 1 }, { unique: true })


// ─── Compound Index ────────────────────────────────────────────────────────
// Index on multiple fields — supports queries using both or just the first field
db.users.createIndex({ city: 1, age: -1 })
// Supports: find({ city, age }),  find({ city })
// Does NOT support: find({ age })  (leftmost prefix rule)


// ─── Text Index ────────────────────────────────────────────────────────────
// Full-text search on string fields
db.articles.createIndex({ title: "text", body: "text" })
db.articles.find({ $text: { $search: "mongodb tutorial" } })
db.articles.find({ $text: { $search: "\"exact phrase\"" } })
db.articles.find({ $text: { $search: "mongodb -sql" } })  // mongodb but NOT sql


// ─── Wildcard Index ────────────────────────────────────────────────────────
// Index all fields in a document (useful for dynamic schemas)
db.logs.createIndex({ "$**": 1 })


// ─── TTL Index (Time-To-Live) ──────────────────────────────────────────────
// Automatically delete documents after a time period
// Perfect for sessions, cache, OTP, logs
db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })  // 1 hour
db.otpCodes.createIndex({ expiresAt: 1 }, { expireAfterSeconds: 0 })     // delete at expiresAt time


// ─── Geospatial Index ──────────────────────────────────────────────────────
// Index for location-based queries
db.stores.createIndex({ location: "2dsphere" })
// Find stores within 5km of a point
db.stores.find({
  location: {
    $near: {
      $geometry: { type: "Point", coordinates: [77.5946, 12.9716] },
      $maxDistance: 5000  // 5km in meters
    }
  }
})


// ─── Hashed Index ─────────────────────────────────────────────────────────
// For sharding — distributes data evenly across shards
db.users.createIndex({ userId: "hashed" })


// ─── Partial Index ─────────────────────────────────────────────────────────
// Index only documents that match a filter — smaller, faster
db.orders.createIndex(
  { createdAt: 1 },
  { partialFilterExpression: { status: "pending" } }
)


// ─── Sparse Index ──────────────────────────────────────────────────────────
// Index only documents that HAVE the indexed field
db.users.createIndex({ phone: 1 }, { sparse: true })
```

### Index Management

```javascript
// List all indexes on a collection
db.users.getIndexes()

// Check if a query uses an index (IXSCAN) or does a full scan (COLLSCAN)
db.users.find({ email: "alice@example.com" }).explain("executionStats")

// Drop a specific index
db.users.dropIndex({ email: 1 })
db.users.dropIndex("email_1")     // by index name

// Drop ALL indexes except _id
db.users.dropIndexes()

// Rebuild all indexes
db.users.reIndex()
```

---

## 11. Data Modeling & Relationships

### Two Approaches

#### Approach 1: Embedding (Denormalization)
```javascript
// User document with embedded address and orders
{
  "_id": ObjectId("..."),
  "name": "Alice",
  "address": {
    "street": "123 MG Road",
    "city": "Bangalore"
  },
  "recentOrders": [
    { "orderId": "ORD001", "amount": 2500, "date": ISODate("2024-01-15") },
    { "orderId": "ORD002", "amount": 1200, "date": ISODate("2024-01-20") }
  ]
}
```
**✅ Use when:** Data is always queried together, one-to-few relationship, data doesn't change independently

#### Approach 2: Referencing (Normalization)
```javascript
// Users collection
{ "_id": ObjectId("user1"), "name": "Alice", "email": "alice@example.com" }

// Orders collection (references user)
{ "_id": ObjectId("order1"), "userId": ObjectId("user1"), "amount": 2500 }
{ "_id": ObjectId("order2"), "userId": ObjectId("user1"), "amount": 1200 }
```
**✅ Use when:** Data is queried independently, one-to-many or many-to-many, data updates frequently, document would exceed 16MB

### Decision Guide

| Scenario | Use |
|----------|-----|
| User → Address (1:1) | **Embed** always |
| Blog Post → Comments (1:few) | **Embed** comments in post |
| User → Orders (1:many, >100 orders possible) | **Reference** — orders in separate collection |
| Student → Courses (many:many) | **Reference** both ways or use junction collection |
| Product → Reviews (1:many, read together) | **Embed** recent N reviews, reference the rest |

---

## 12. Transactions

MongoDB supports **multi-document ACID transactions** (since v4.0).

```javascript
// Start a session
const session = db.getMongo().startSession()

session.startTransaction({
  readConcern: { level: "snapshot" },
  writeConcern: { w: "majority" }
})

try {
  const accounts = session.getDatabase("bank").accounts

  // Debit from sender
  accounts.updateOne(
    { _id: "alice" },
    { $inc: { balance: -500 } },
    { session }
  )

  // Credit to receiver
  accounts.updateOne(
    { _id: "bob" },
    { $inc: { balance: 500 } },
    { session }
  )

  // Both succeed → commit
  session.commitTransaction()
  print("Transaction committed!")

} catch (error) {
  // Any failure → rollback both operations
  session.abortTransaction()
  print("Transaction aborted: " + error)

} finally {
  session.endSession()
}
```

### ACID in MongoDB

| Property | How MongoDB Achieves It |
|----------|------------------------|
| **Atomicity** | Within a transaction, all operations succeed or all are rolled back |
| **Consistency** | Schema validation + write concern ensures data integrity |
| **Isolation** | Snapshot isolation — in-progress transactions don't see each other's changes |
| **Durability** | Write concern `w: "majority"` + journaling ensures data survives crashes |

> ⚠️ Use transactions only when truly needed — they add overhead. For single-document operations, MongoDB is already atomic without a transaction.

---

## 13. Replication

### What is a Replica Set?
A Replica Set is a group of MongoDB servers that hold the **same data**. If the primary (main) server fails, one of the secondaries automatically becomes the new primary.

```
┌─────────────────────────────────────────────────────────┐
│                    Replica Set                           │
│                                                          │
│  ┌─────────────┐       oplog       ┌──────────────────┐ │
│  │   PRIMARY   │ ─────────────────►│  SECONDARY 1     │ │
│  │ (reads +    │                   │  (can serve reads)│ │
│  │  writes)    │ ─────────────────►│  SECONDARY 2     │ │
│  └──────┬──────┘       oplog       └──────────────────┘ │
│         │                                                │
│         │  If PRIMARY goes down:                        │
│         │  → Secondaries hold an election               │
│         │  → One becomes new PRIMARY automatically      │
│         │  → Zero downtime for your application         │
└─────────────────────────────────────────────────────────┘
```

### Replica Set Setup (Quick)
```bash
# Start 3 mongod instances on different ports
mongod --port 27017 --dbpath /data/rs0 --replSet "rs0"
mongod --port 27018 --dbpath /data/rs1 --replSet "rs0"
mongod --port 27019 --dbpath /data/rs2 --replSet "rs0"

# Initiate the replica set (run inside mongosh on primary)
rs.initiate({
  _id: "rs0",
  members: [
    { _id: 0, host: "localhost:27017" },
    { _id: 1, host: "localhost:27018" },
    { _id: 2, host: "localhost:27019" }
  ]
})

rs.status()    // check replica set status
rs.isMaster()  // check which is primary
```

---

## 14. Sharding

Sharding splits data across multiple servers (**shards**) so MongoDB can handle datasets larger than one machine's capacity.

```
┌──────────────────────────────────────────────────────────────────────┐
│                        Application                                    │
└─────────────────────────────┬────────────────────────────────────────┘
                              │
                              ▼
┌──────────────────────────────────────────────────────────────────────┐
│                    mongos (Query Router)                              │
│            Routes each query to the correct shard                    │
└───────────────┬──────────────────┬─────────────────┬─────────────────┘
                │                  │                 │
                ▼                  ▼                 ▼
        ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
        │   Shard 1    │  │   Shard 2    │  │   Shard 3    │
        │ userId: 1-   │  │ userId: 334- │  │ userId: 667- │
        │        333   │  │        666   │  │        999   │
        │(Replica Set) │  │(Replica Set) │  │(Replica Set) │
        └──────────────┘  └──────────────┘  └──────────────┘
```

### Shard Key — The Most Important Decision
- The field by which data is distributed across shards
- **Ranged sharding** → consecutive values go to the same shard (good for range queries)
- **Hashed sharding** → hash of value distributes evenly (good for write-heavy workloads)
- Choosing a bad shard key leads to **hot spots** — one shard getting all the traffic

---

## 15. MongoDB Security

### Key Security Measures

```javascript
// 1. Enable Authentication — create an admin user
use admin
db.createUser({
  user: "adminUser",
  pwd: "StrongPassword123!",
  roles: [{ role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase"]
})

// 2. Create a restricted user for your app
use myapp
db.createUser({
  user: "appUser",
  pwd: "AppPassword456!",
  roles: [{ role: "readWrite", db: "myapp" }]
})

// 3. List all users
db.getUsers()

// 4. Drop a user
db.dropUser("appUser")

// 5. Update a user's password
db.changeUserPassword("appUser", "NewPassword789!")
```

### Built-in Roles

| Role | Access Level |
|------|-------------|
| `read` | Read only on one database |
| `readWrite` | Read and write on one database |
| `dbAdmin` | Database admin tasks (indexes, stats) |
| `userAdmin` | Manage users on one database |
| `clusterAdmin` | Manage the entire cluster |
| `readAnyDatabase` | Read on all databases |
| `readWriteAnyDatabase` | Read/write on all databases |
| `dbAdminAnyDatabase` | Admin on all databases |
| `userAdminAnyDatabase` | Manage users on all databases |
| `root` | Superuser — all permissions |

### Security Best Practices
- Always enable `--auth` in `mongod.conf`
- Use **TLS/SSL** for connections in production
- **Never expose port 27017** directly to the internet — use a firewall
- Create **least-privilege users** — app users should not have `root`
- Enable **IP whitelisting** in MongoDB Atlas
- Encrypt data at rest with **WiredTiger encryption**
- Regularly **audit** database access logs

---

## 16. MongoDB vs MySQL vs Redis

| Feature | **MongoDB** | **MySQL** | **Redis** |
|---------|-------------|-----------|-----------|
| **Type** | Document NoSQL | Relational SQL | In-memory key-value |
| **Schema** | Flexible (schemaless) | Fixed (strict schema) | No schema |
| **Storage** | Disk (with memory cache) | Disk | RAM (optional disk) |
| **Speed** | Fast | Moderate | Extremely fast |
| **Query language** | MongoDB Query Language | SQL | Redis commands |
| **Joins** | `$lookup` (limited) | ✅ Native JOINs | ❌ No |
| **Transactions** | ✅ ACID (v4.0+) | ✅ Full ACID | Partial (MULTI/EXEC) |
| **Scaling** | ✅ Horizontal (sharding) | Vertical mostly | ✅ Cluster mode |
| **Data format** | BSON documents | Rows and columns | Key-value, lists, sets |
| **Best for** | Catalogs, CMS, apps | Financial, ERP, reporting | Caching, sessions, queues |
| **Indexing** | Rich (text, geo, TTL) | B-Tree | Limited |
| **Persistence** | ✅ Always | ✅ Always | Optional (RDB/AOF) |
| **Use case** | Unstructured/semi-structured data | Relational, complex queries | Speed layer over other DBs |

---

## 17. Complete MongoDB Commands Reference

### Database Commands

```javascript
// Show all databases
show dbs
show databases

// Switch to (or create) a database
use myapp

// Show current database name
db
db.getName()

// Show database stats
db.stats()

// Drop the current database (DANGEROUS — deletes everything)
db.dropDatabase()

// Run a command on the database
db.runCommand({ serverStatus: 1 })
db.runCommand({ currentOp: 1 })    // show running operations
db.runCommand({ killOp: 1, op: <opid> })  // kill a specific operation
```

### Collection Commands

```javascript
// Show all collections in current database
show collections
db.getCollectionNames()

// Create a collection explicitly (optional — auto-created on first insert)
db.createCollection("users")

// Create with options (capped collection — fixed size, auto-deletes old docs)
db.createCollection("logs", { capped: true, size: 10485760, max: 1000 })
// size = max bytes, max = max documents

// Drop a collection
db.users.drop()

// Rename a collection
db.users.renameCollection("customers")

// Collection stats
db.users.stats()
db.users.count()                    // deprecated but still works
db.users.countDocuments({})         // recommended
db.users.estimatedDocumentCount()   // faster, approximate
```

### Index Commands

```javascript
// List all indexes
db.users.getIndexes()
db.users.listIndexes().toArray()

// Create indexes
db.users.createIndex({ email: 1 }, { unique: true })
db.users.createIndex({ city: 1, age: -1 })
db.users.createIndex({ name: "text" })
db.users.createIndex({ createdAt: 1 }, { expireAfterSeconds: 86400 })

// Drop indexes
db.users.dropIndex("email_1")
db.users.dropIndex({ email: 1 })
db.users.dropIndexes()     // drop all except _id

// Analyze query performance
db.users.find({ email: "alice@example.com" }).explain()
db.users.find({ email: "alice@example.com" }).explain("executionStats")
db.users.find({ email: "alice@example.com" }).explain("allPlansExecution")
```

### Aggregation Commands

```javascript
// Run aggregation pipeline
db.orders.aggregate([ { $match: {} }, { $group: { _id: "$status", count: { $sum: 1 } } } ])

// Aggregation with options
db.orders.aggregate(
  [ { $group: { _id: "$city", total: { $sum: "$amount" } } } ],
  { allowDiskUse: true, comment: "revenue-by-city" }
)

// MapReduce (legacy — prefer aggregation pipeline)
db.orders.mapReduce(
  function() { emit(this.city, this.amount) },       // map
  function(key, values) { return Array.sum(values) }, // reduce
  { out: "revenue_by_city" }
)
```

### Cursor Methods

```javascript
// Cursor methods chain on find()
db.users.find()
  .sort({ age: -1 })       // sort descending by age
  .limit(10)               // return max 10
  .skip(20)                // skip first 20 (page 3)
  .projection({ name: 1 }) // only return name field (mongosh uses different syntax)

// Count results (without fetching)
db.users.find({ city: "Mumbai" }).count()       // deprecated
db.users.countDocuments({ city: "Mumbai" })     // recommended

// Convert cursor to array
db.users.find().toArray()

// forEach — iterate over each document
db.users.find().forEach(doc => print(doc.name))

// Check if more results exist
const cursor = db.users.find()
cursor.hasNext()   // true/false
cursor.next()      // get next document
```

### Backup & Restore Commands (Run in Terminal)

```bash
# Dump (backup) entire MongoDB instance
mongodump --out /backup/

# Dump a specific database
mongodump --db myapp --out /backup/

# Dump a specific collection
mongodump --db myapp --collection users --out /backup/

# Restore from backup
mongorestore /backup/

# Restore specific database
mongorestore --db myapp /backup/myapp/

# Export collection as JSON
mongoexport --db myapp --collection users --out users.json

# Export as CSV
mongoexport --db myapp --collection users --type csv --fields name,email,city --out users.csv

# Import from JSON
mongoimport --db myapp --collection users --file users.json

# Import from CSV
mongoimport --db myapp --collection users --type csv --headerline --file users.csv
```

### Replica Set Commands

```javascript
rs.status()           // full replica set status
rs.isMaster()         // which node is primary
rs.conf()             // current replica set configuration
rs.initiate()         // initialize a new replica set
rs.add("host:port")   // add a new member
rs.remove("host:port") // remove a member
rs.stepDown()         // force the primary to step down (triggers election)
rs.printReplicationInfo()         // replication lag info
rs.printSecondaryReplicationInfo() // secondary lag info
```

### User Management Commands

```javascript
// Create user
db.createUser({ user: "dev", pwd: "pass", roles: ["readWrite"] })

// List users
db.getUsers()
show users

// Update user roles
db.updateUser("dev", { roles: [{ role: "dbAdmin", db: "myapp" }] })

// Change password
db.changeUserPassword("dev", "newPassword")

// Delete user
db.dropUser("dev")

// Authenticate in shell
db.auth("dev", "pass")
```

---

## 18. MongoDB with Python (PyMongo)

```python
from pymongo import MongoClient, ASCENDING, DESCENDING
from bson.objectid import ObjectId
from datetime import datetime

# ─── Connect ──────────────────────────────────────────────────────────────
client = MongoClient("mongodb://localhost:27017/")
db = client["myapp"]
users = db["users"]

# ─── Insert ───────────────────────────────────────────────────────────────
result = users.insert_one({
    "name": "Alice", "age": 28, "email": "alice@example.com", "city": "Mumbai"
})
print(result.inserted_id)

users.insert_many([
    {"name": "Bob", "age": 35, "city": "Delhi"},
    {"name": "Carol", "age": 22, "city": "Pune"}
])

# ─── Find ─────────────────────────────────────────────────────────────────
# Find all
for user in users.find():
    print(user)

# Find with filter
alice = users.find_one({"name": "Alice"})
print(alice["email"])

# Find with projection
for user in users.find({"city": "Mumbai"}, {"name": 1, "email": 1, "_id": 0}):
    print(user)

# Find with operators
for user in users.find({"age": {"$gte": 25, "$lte": 35}}):
    print(user)

# Sort, skip, limit (pagination)
page_2 = list(users.find().sort("name", ASCENDING).skip(10).limit(10))

# Count
total = users.count_documents({"city": "Mumbai"})

# ─── Update ───────────────────────────────────────────────────────────────
users.update_one({"name": "Alice"}, {"$set": {"age": 30}})
users.update_many({"city": "Mumbai"}, {"$set": {"country": "India"}})

# Upsert
users.update_one(
    {"email": "new@example.com"},
    {"$set": {"name": "New User", "age": 25}},
    upsert=True
)

# ─── Delete ───────────────────────────────────────────────────────────────
users.delete_one({"name": "Alice"})
users.delete_many({"city": "Mumbai"})

# ─── Aggregation ──────────────────────────────────────────────────────────
pipeline = [
    {"$match": {"city": "Mumbai"}},
    {"$group": {"_id": "$city", "count": {"$sum": 1}, "avgAge": {"$avg": "$age"}}},
    {"$sort": {"count": -1}}
]
results = list(users.aggregate(pipeline))

# ─── Indexes ──────────────────────────────────────────────────────────────
users.create_index([("email", ASCENDING)], unique=True)
users.create_index([("city", ASCENDING), ("age", DESCENDING)])
users.create_index([("name", "text")])
users.create_index("createdAt", expireAfterSeconds=3600)

# ─── Close connection ─────────────────────────────────────────────────────
client.close()
```

---

## 19. MongoDB with Node.js (Mongoose)

```javascript
const mongoose = require('mongoose')

// ─── Connect ───────────────────────────────────────────────────────────────
await mongoose.connect('mongodb://localhost:27017/myapp')

// ─── Define Schema ─────────────────────────────────────────────────────────
const userSchema = new mongoose.Schema({
  name:      { type: String, required: true, trim: true },
  email:     { type: String, required: true, unique: true, lowercase: true },
  age:       { type: Number, min: 0, max: 120 },
  city:      { type: String, default: "Unknown" },
  skills:    [String],
  isActive:  { type: Boolean, default: true },
  createdAt: { type: Date, default: Date.now }
})

// Add an index
userSchema.index({ city: 1, age: -1 })

const User = mongoose.model('User', userSchema)

// ─── Insert ────────────────────────────────────────────────────────────────
const alice = await User.create({ name: 'Alice', email: 'alice@example.com', age: 28, city: 'Mumbai' })

await User.insertMany([
  { name: 'Bob',   email: 'bob@example.com',   age: 35, city: 'Delhi' },
  { name: 'Carol', email: 'carol@example.com', age: 22, city: 'Pune'  }
])

// ─── Find ──────────────────────────────────────────────────────────────────
const allUsers   = await User.find()
const mumbaiUsers = await User.find({ city: 'Mumbai' })
const user       = await User.findOne({ email: 'alice@example.com' })
const byId       = await User.findById('64a1f2b3c4d5e6f7a8b9c0d1')

// Pagination
const page = 1, perPage = 10
const users = await User.find({ city: 'Mumbai' })
  .sort({ name: 1 })
  .skip((page - 1) * perPage)
  .limit(perPage)
  .select('name email -_id')    // projection: include name+email, exclude _id

// ─── Update ────────────────────────────────────────────────────────────────
await User.updateOne({ name: 'Alice' }, { $set: { age: 30 } })
await User.updateMany({ city: 'Mumbai' }, { $set: { country: 'India' } })
const updated = await User.findByIdAndUpdate(id, { $inc: { age: 1 } }, { new: true })

// ─── Delete ────────────────────────────────────────────────────────────────
await User.deleteOne({ name: 'Alice' })
await User.deleteMany({ isActive: false })
await User.findByIdAndDelete(id)

// ─── Aggregation ───────────────────────────────────────────────────────────
const stats = await User.aggregate([
  { $group: { _id: '$city', count: { $sum: 1 }, avgAge: { $avg: '$age' } } },
  { $sort: { count: -1 } }
])
```

---

## 20. Common Use Cases

| Use Case | How MongoDB Helps | Example |
|----------|------------------|---------|
| **Product Catalogs** | Each product can have different attributes (no fixed schema) | Electronics vs Clothing — totally different fields |
| **Content Management** | Articles, blogs, pages all have different structures | CMS like WordPress-style apps |
| **User Profiles** | Rich, nested user data — preferences, history, social links | Social networks, apps |
| **Real-time Analytics** | Aggregation pipeline processes events as they come in | Click tracking, user behavior |
| **IoT Data** | Millions of sensor readings with varying data shapes | Smart devices, telemetry |
| **Mobile Apps** | JSON documents map directly to mobile app objects | Offline-first sync |
| **Game Data** | Player states, leaderboards, inventory with custom attributes | Multiplayer games |
| **Logging** | Flexible schema absorbs new log fields without migration | Application logs, audit trails |
| **E-commerce** | Orders, products, users, reviews — all in one place | Uber, eBay, Amazon |
| **Geospatial Apps** | Built-in geospatial indexes and operators | Uber (driver location), delivery apps |

### Companies Using MongoDB

| Company | Use Case |
|---------|----------|
| **Uber** | Real-time trip and location data, driver profiles |
| **eBay** | Product catalog with millions of different item types |
| **Adobe** | Creative Cloud — managing user assets and metadata |
| **LinkedIn** | Member profiles, activity data |
| **Bosch** | IoT device data from millions of connected sensors |
| **SEGA** | Game data, player profiles, leaderboards |
| **Forbes** | Content management for articles and media |

---

## 21. Best Practices

### ✅ Do's

- **Choose the right data model** → Embed data that is always queried together; reference data that is queried independently
- **Always index query fields** → Run `explain("executionStats")` to confirm IXSCAN not COLLSCAN
- **Use projection** → Only fetch fields you actually need — reduces network and memory overhead
- **Use `countDocuments()` not `count()`** → `count()` is deprecated
- **Set `writeConcern: "majority"`** → Ensures data is actually replicated before confirming a write
- **Use `Decimal128` for money** → Never `Double` — floating point errors cause financial bugs
- **Design for your query patterns** → MongoDB schema should reflect how data is accessed, not just stored
- **Keep documents focused** → Avoid documents growing unboundedly (e.g., appending to an ever-growing array)
- **Use TTL indexes for temporary data** → Sessions, OTPs, logs — let MongoDB clean them up automatically
- **Enable authentication** → Never run `mongod` without `--auth` in production

### ❌ Don'ts

- **Don't use `$where` in production** → It runs JavaScript — slow and no index support
- **Don't index everything** → Each index adds write overhead — index only queried fields
- **Don't store large files in documents** → Use **GridFS** for files > 16MB
- **Don't ignore the 16MB document limit** → If a document can grow unboundedly, use references
- **Don't skip backups** → Use `mongodump` or MongoDB Atlas automated backups regularly
- **Don't use `.toArray()` on huge cursors** → Stream with `forEach` to avoid loading everything into memory
- **Don't expose port 27017 publicly** → Always firewall MongoDB — it should never be directly internet-accessible
- **Don't hardcode connection strings with passwords** → Use environment variables or secret managers

---

## 22. Common Interview Questions

### Q1: What is MongoDB and how is it different from SQL databases?
**Answer:** MongoDB is an open-source document-oriented NoSQL database that stores data as flexible JSON-like documents (BSON) instead of fixed rows and columns. The key differences are: MongoDB has a dynamic schema (each document can have different fields), it scales horizontally via sharding, it stores related data together in nested documents instead of across multiple joined tables, and it uses MongoDB Query Language instead of SQL. It is ideal when data structure varies, changes frequently, or when you need to scale out across many servers.

---

### Q2: What is a Document and a Collection in MongoDB?
**Answer:** A Document is the basic unit of data in MongoDB — equivalent to a row in SQL. It is a JSON-like object containing field-value pairs, and it can contain nested documents and arrays. Each document has a unique `_id` field. A Collection is a group of documents, equivalent to a table in SQL. Unlike SQL tables, a collection does not enforce a fixed schema — documents in the same collection can have completely different fields.

---

### Q3: What is BSON and why does MongoDB use it?
**Answer:** BSON stands for Binary JSON. MongoDB uses BSON instead of plain JSON for storage and network transmission because it is more compact (smaller storage footprint), faster to encode and decode, and supports additional data types that JSON does not have, such as `Date`, `ObjectId`, `Int32`, `Int64`, `Decimal128`, and `Binary`. When you write a query in the MongoDB shell, you write JSON-like syntax — MongoDB internally converts it to BSON.

---

### Q4: What is an Index in MongoDB and why is it important?
**Answer:** An Index is a special data structure that stores a small portion of the collection's data in an easily traversable form. Without an index, MongoDB must scan every document in a collection to find matching results (COLLSCAN — collection scan). With an index on the queried field, MongoDB jumps directly to the matching documents (IXSCAN — index scan), which is dramatically faster for large collections. You can use `.explain("executionStats")` to verify whether a query is using an index. Create indexes on fields that appear frequently in `find()` filters, `sort()`, and aggregation `$match` stages.

---

### Q5: What is the Aggregation Pipeline in MongoDB?
**Answer:** The Aggregation Pipeline is a framework for transforming and analyzing data in MongoDB. It works like a conveyor belt — documents flow through a series of stages, where each stage transforms the data and passes it to the next. Common stages include `$match` (filter, like SQL WHERE), `$group` (group and calculate aggregates, like SQL GROUP BY), `$project` (reshape output, like SQL SELECT), `$sort`, `$limit`, `$skip`, `$unwind` (flatten arrays), and `$lookup` (join with another collection). It is more powerful and efficient than MapReduce and is the recommended way to do analytics in MongoDB.

---

### Q6: What is the difference between Embedding and Referencing in MongoDB?
**Answer:** Embedding means storing related data as a nested document or array inside a parent document — all related data in one place. It is fast for reads because no join is needed, but makes updates complex if the nested data changes frequently. Referencing means storing a reference (usually an `_id`) to a document in another collection — similar to a foreign key in SQL. It is better when the related data is large, changes independently, or is queried separately. The rule of thumb: embed when you always query the data together; reference when you query it independently or it can grow unboundedly.

---

### Q7: What is a Replica Set and why is it needed?
**Answer:** A Replica Set is a group of MongoDB servers that all hold the same data. One node is the Primary (handles all reads and writes) and the others are Secondaries (continuously copy data from the primary via the oplog). If the primary goes down, the secondaries automatically hold an election and promote one of themselves to the new primary — your application reconnects automatically with no data loss. Replica sets provide high availability, automatic failover, and also allow read operations to be distributed across secondaries to reduce load on the primary.

---

### Q8: What is Sharding in MongoDB?
**Answer:** Sharding is MongoDB's approach to horizontal scaling. When a single server cannot hold all the data or handle all the traffic, sharding splits the data across multiple servers called shards. A `mongos` router sits between the application and the shards — it knows which shard holds which data and routes queries accordingly. Data is distributed based on a shard key — a field chosen during sharding setup. Choosing the right shard key is critical: a bad key causes hot spots (one shard gets all the traffic), while a good key distributes reads and writes evenly.

---

### Q9: What is a TTL Index and when do you use it?
**Answer:** A TTL (Time-To-Live) Index automatically deletes documents after a specified number of seconds. You create it with `db.collection.createIndex({ fieldName: 1 }, { expireAfterSeconds: N })`. MongoDB runs a background job every 60 seconds that checks for and deletes expired documents. Use it for data that should only exist temporarily — user sessions (expire after inactivity), OTP codes (expire after 5 minutes), cached data, log entries, or any ephemeral data. It eliminates the need to write cron jobs or background cleanup scripts.

---

### Q10: When should you NOT use MongoDB?
**Answer:** MongoDB is not the right choice when: you have highly relational data requiring many complex JOINs (SQL databases handle this more naturally and efficiently); you need full ACID transactions across many collections at high volume (SQL databases were designed for this from the ground up); you are building financial or banking systems where strict data integrity constraints and complex reporting are non-negotiable; or your team already has deep SQL expertise and the data is naturally tabular. MongoDB excels at flexible, document-centric data — when your data is naturally hierarchical, varies per record, or changes shape over time.

---

## 🔄 MongoDB — At a Glance

```
Documents → Collections → Database → mongod process
    ↑              ↑           ↑
  BSON format   Flexible    Multiple DBs
  _id required  Schema      on one server

Queries → Query Engine → Indexes (IXSCAN) → Fast results
                       → No Index (COLLSCAN) → Slow on large collections

Replication: Primary → oplog → Secondaries (auto-failover)
Sharding: mongos router → Shard 1 / Shard 2 / Shard 3
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| MongoDB | Open-source document NoSQL database — stores flexible JSON-like documents |
| Document | Basic data unit — JSON-like object with field-value pairs, max 16MB |
| Collection | Group of documents — like a SQL table but schema-free |
| Database | Container for collections — one server holds many databases |
| `_id` | Unique identifier for every document — auto-generated ObjectId if not set |
| BSON | Binary JSON — MongoDB's internal storage format, more types than JSON |
| ObjectId | 12-byte unique ID: timestamp + machine + process + counter |
| Schema-less | No fixed structure — each document can have different fields |
| `insertOne` | Insert a single document |
| `insertMany` | Insert multiple documents at once |
| `find` | Query documents — returns a cursor |
| `findOne` | Return first matching document |
| `updateOne` | Update first matching document |
| `updateMany` | Update all matching documents |
| `deleteOne` | Delete first matching document |
| `deleteMany` | Delete all matching documents |
| `$set` | Set a field value (add if not exists) |
| `$inc` | Increment a numeric field |
| `$push` | Add an element to an array |
| `$pull` | Remove matching elements from an array |
| `$addToSet` | Add to array only if value doesn't already exist |
| `$lookup` | JOIN with another collection in aggregation |
| `$unwind` | Deconstruct array — one document per array element |
| Index | Data structure for fast lookups — avoids full collection scan |
| TTL Index | Auto-deletes documents after N seconds — for sessions, OTPs |
| Replica Set | Multiple servers with same data — automatic failover |
| Sharding | Split data across servers — horizontal scaling |
| `mongodump` | Backup entire database to disk |
| `explain()` | Show query execution plan — check if index is being used |
| Aggregation Pipeline | Multi-stage data processing — filter, group, project, sort |
| Embedding | Store related data nested in same document |
| Referencing | Store `_id` reference to another collection (like a foreign key) |
| WiredTiger | Default MongoDB storage engine — document-level locking |

---

> 💡 **The Production Golden Rule:**
> Always combine — **Replica Set for HA + Indexes on query fields + Correct data model (embed vs reference) + Authentication enabled + Regular mongodump backups + explain() to verify query plans + TTL indexes for temporary data**.
> These 7 things together build a reliable, fast, production-grade MongoDB deployment.

---

*Notes prepared based on: MongoDB Official Docs, MongoDB University, GeeksforGeeks, and production best practices (2025–2026)*
