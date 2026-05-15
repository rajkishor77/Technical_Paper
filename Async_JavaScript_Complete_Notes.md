# ⚡ Asynchronous JavaScript — Complete Study Notes
> **After reading this, every async JS concept will be crystal clear.**
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [How JavaScript Executes Code](#1-how-javascript-executes-code)
2. [Sync vs Async — The Core Difference](#2-sync-vs-async--the-core-difference)
3. [Web Browser APIs & Node.js APIs](#3-web-browser-apis--nodejs-apis)
4. [The Event Loop — Deep Dive](#4-the-event-loop--deep-dive)
5. [Microtasks vs Macrotasks](#5-microtasks-vs-macrotasks)
6. [Execution Order — The Full Picture](#6-execution-order--the-full-picture)
7. [Callbacks](#7-callbacks)
8. [Callback Hell & Inversion of Control](#8-callback-hell--inversion-of-control)
9. [Promises — Complete Guide](#9-promises--complete-guide)
10. [Promise Chaining](#10-promise-chaining)
11. [Promise Error Handling](#11-promise-error-handling)
12. [Promise Utility Functions](#12-promise-utility-functions)
13. [Promisification](#13-promisification)
14. [async / await — Complete Guide](#14-async--await--complete-guide)
15. [Error Handling with async/await](#15-error-handling-with-asyncawait)
16. [async/await vs Promises](#16-asyncawait-vs-promises)
17. [Real-World Async Patterns](#17-real-world-async-patterns)
18. [Common Async Mistakes & Fixes](#18-common-async-mistakes--fixes)
19. [Timers — setTimeout, setInterval, clearTimeout](#19-timers--settimeout-setinterval-cleartimeout)
20. [fetch API — HTTP Requests](#20-fetch-api--http-requests)
21. [AbortController — Cancel Requests](#21-abortcontroller--cancel-requests)
22. [Best Practices](#22-best-practices)
23. [Common Interview Questions](#23-common-interview-questions)

---

## 1. How JavaScript Executes Code

JavaScript is **single-threaded** — it has only **one Call Stack** and can only execute one thing at a time.

### The JavaScript Runtime — Key Components

```
┌──────────────────────────────────────────────────────────────┐
│                    JavaScript Runtime                         │
│                                                              │
│  ┌──────────────┐    ┌──────────────────────────────────┐   │
│  │  Call Stack  │    │       Web Browser APIs            │   │
│  │              │    │  (setTimeout, fetch, DOM events)  │   │
│  │ main()       │    │  These run OUTSIDE JS thread      │   │
│  │ sayHello()   │    └──────────────────────────────────┘   │
│  │ console.log()│                    ↓                       │
│  └──────────────┘    ┌──────────────────────────────────┐   │
│         ↑            │      Microtask Queue             │   │
│         │            │  (Promise .then, queueMicrotask)  │   │
│    Event Loop        └──────────────────────────────────┘   │
│    (scheduler)       ┌──────────────────────────────────┐   │
│         │            │      Callback/Macro Queue        │   │
│         └────────────│  (setTimeout, setInterval, I/O)  │   │
│                      └──────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────┘
```

### Call Stack — How Functions Are Tracked

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
function displayGreeting() {
  const message = greet("Alice");
  console.log(message);
}
displayGreeting();
```

```
Stack progression:
[empty]
→ [displayGreeting()]
→ [displayGreeting(), greet("Alice")]
→ [displayGreeting(), greet("Alice"), return "Hello, Alice!"]   ← greet pops
→ [displayGreeting(), console.log("Hello, Alice!")]             ← console.log pops
→ [displayGreeting()]                                           ← displayGreeting pops
→ [empty]
```

**Stack Overflow** — when the Call Stack grows too deep (infinite recursion):
```javascript
function infinite() { infinite(); }  // → RangeError: Maximum call stack size exceeded
```

---

## 2. Sync vs Async — The Core Difference

### Synchronous — Blocking
Every line waits for the previous line to finish before moving on.
```javascript
console.log("Step 1");
const result = heavyCalculation();   // if this takes 5 seconds, EVERYTHING stops
console.log("Step 2");               // waits 5 full seconds before printing
```

### Asynchronous — Non-Blocking
Long tasks are handed off to external environments. JS moves to the next line immediately.
```javascript
console.log("Step 1");
setTimeout(() => console.log("Timer done!"), 3000);   // handed off to browser API
console.log("Step 2");   // prints IMMEDIATELY — does not wait 3 seconds
// Output order: "Step 1" → "Step 2" → (3 sec later) "Timer done!"
```

### Comparison Table

| Feature | Synchronous | Asynchronous |
|---------|-------------|-------------|
| Execution | One line at a time, blocking | Starts task, continues to next line |
| Waiting | Program freezes | Program stays responsive |
| Analogy | Standing in a bank queue | Placing a food order + getting a buzzer |
| Examples | Math calculations, loops | setTimeout, fetch, file I/O |
| Danger | Blocks UI / freezes app | None — UI stays responsive |

---

## 3. Web Browser APIs & Node.js APIs

These are **external environments** that handle time-consuming tasks outside the JS thread.

### Browser APIs

| API | What It Does |
|-----|-------------|
| `setTimeout(fn, ms)` | Runs fn after ms milliseconds |
| `setInterval(fn, ms)` | Runs fn every ms milliseconds |
| `fetch(url)` | Makes HTTP network requests |
| `XMLHttpRequest` | Older way to make HTTP requests |
| `addEventListener` | Waits for user events (click, keypress) |
| `requestAnimationFrame` | Runs fn before next browser repaint (60fps) |
| `navigator.geolocation` | Gets GPS location |
| `localStorage / sessionStorage` | Browser storage APIs |

### Node.js APIs

| API | What It Does |
|-----|-------------|
| `fs.readFile()` | Read files from disk |
| `fs.writeFile()` | Write files to disk |
| `http.get()` | Make HTTP requests |
| `setTimeout / setInterval` | Same as browser |
| `setImmediate(fn)` | Run fn after I/O callbacks (Node-specific) |
| `process.nextTick(fn)` | Run fn before any I/O (highest priority in Node) |

### How They Work Together
```
JS Code               Browser API              Queue
─────────────────────────────────────────────────────
setTimeout(fn, 2000) ──→ Timer starts (2000ms) │
JS continues          │                         │
                       │ Timer done! ──────────→ Callback Queue
                       │                         │
Call Stack empty ←──────────────────────────────── Event Loop pulls fn
fn() runs             │                         │
```

---

## 4. The Event Loop — Deep Dive

The **Event Loop** is the scheduler that coordinates between the Call Stack and the queues. It runs an infinite loop checking one simple rule:

```
While (true) {
  if (Call Stack is empty) {
    if (Microtask Queue has tasks) {
      run ALL microtasks (until microtask queue is empty)
    } else if (Callback Queue has tasks) {
      run ONE macrotask from Callback Queue
    }
  }
}
```

### Step-by-Step Execution of a Simple Example

```javascript
console.log("A");                    // Line 1
setTimeout(() => console.log("B"), 0); // Line 2
console.log("C");                    // Line 3
```

```
Step 1: Call Stack runs: console.log("A") → prints "A"
Step 2: setTimeout seen → browser API starts 0ms timer → callback goes to Callback Queue
Step 3: Call Stack runs: console.log("C") → prints "C"
Step 4: Call Stack is empty
Step 5: Event Loop checks Microtask Queue → empty
Step 6: Event Loop checks Callback Queue → has "B" callback
Step 7: Moves B callback to Call Stack → prints "B"

Final Output: A → C → B
```

> 💡 Even `setTimeout(fn, 0)` does NOT run immediately. It always waits for the Call Stack to be completely empty first.

---

## 5. Microtasks vs Macrotasks

This is one of the most important concepts for interviews and understanding async output order.

### Microtasks (HIGH PRIORITY — "VIP Queue")
Run **immediately after current synchronous code** finishes, **before any macrotask**.
The entire microtask queue is drained before any macrotask runs.

| Source | Example |
|--------|---------|
| Promise `.then()` / `.catch()` / `.finally()` | `Promise.resolve().then(fn)` |
| `async/await` continuations | code after `await` |
| `queueMicrotask()` | `queueMicrotask(fn)` |
| `MutationObserver` callbacks | DOM change observation |

### Macrotasks (NORMAL PRIORITY — "Regular Queue")
Run **one at a time**, after ALL microtasks are cleared.

| Source | Example |
|--------|---------|
| `setTimeout` | `setTimeout(fn, 0)` |
| `setInterval` | `setInterval(fn, 1000)` |
| `setImmediate` | `setImmediate(fn)` (Node.js only) |
| DOM events | click, keypress handlers |
| I/O callbacks | file read, network callbacks |
| `requestAnimationFrame` | browser repaint callbacks |

### The Priority Rule (Critical!)
```
Synchronous Code → Microtasks (all of them) → ONE Macrotask → Microtasks → ONE Macrotask → ...
```

### Classic Interview Example
```javascript
console.log("1: Start");

setTimeout(() => console.log("2: setTimeout"), 0);

Promise.resolve()
  .then(() => console.log("3: Promise 1"))
  .then(() => console.log("4: Promise 2"));

console.log("5: End");
```

```
Output:
1: Start       ← synchronous
5: End         ← synchronous
3: Promise 1   ← microtask (higher priority than setTimeout!)
4: Promise 2   ← microtask (chained .then = new microtask)
2: setTimeout  ← macrotask (runs last, even though scheduled first!)
```

### Another Tricky Example
```javascript
console.log("1");
setTimeout(() => console.log("2: Timeout 1"), 0);
setTimeout(() => console.log("3: Timeout 2"), 0);
Promise.resolve().then(() => console.log("4: Promise 1"));
Promise.resolve().then(() => setTimeout(() => console.log("5: Inside Promise"), 0));
Promise.resolve().then(() => console.log("6: Promise 2"));
setTimeout(() => console.log("7: Timeout 3"), 0);
console.log("8");
```

```
Output: 1, 8, 4, 6, 2, 3, 7, 5
Reason:
- Sync: 1, 8
- Microtasks: 4, 6 (Promise.then)
    (Promise 2 schedules new setTimeout → goes to macrotask queue as "5")
- Macrotasks: 2, 3, 7, 5 (in order they were added)
```

### queueMicrotask()
```javascript
queueMicrotask(() => console.log("This is a microtask"));
// Runs before any setTimeout, but after current synchronous code
```

---

## 6. Execution Order — The Full Picture

```
┌─────────────────────────────────────────────────────────────┐
│  Priority: High ──────────────────────────────────► Low     │
│                                                             │
│  Synchronous Code                                           │
│       ↓                                                     │
│  process.nextTick  (Node.js ONLY — before microtask queue) │
│       ↓                                                     │
│  Microtask Queue  (Promise.then, async/await, queueMicrotask│
│       ↓  (drain ALL before next step)                       │
│  requestAnimationFrame  (browser ONLY — before repaint)    │
│       ↓                                                     │
│  Browser Re-render  (UI updates happen here)               │
│       ↓                                                     │
│  Macrotask Queue  (setTimeout, setInterval, I/O, DOM events)│
│       ↓  (run ONE, then go back to microtask check)         │
│  Repeat ↑                                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. Callbacks

A **callback** is a function passed as an argument to another function, to be called when an async operation completes.

```javascript
// Simple callback
function fetchData(url, onSuccess, onError) {
  // simulate async network request
  setTimeout(() => {
    if (url.startsWith("https")) {
      onSuccess({ data: "some data" });
    } else {
      onError(new Error("Insecure URL"));
    }
  }, 1000);
}

fetchData(
  "https://api.example.com/users",
  (data) => console.log("Got:", data),      // success callback
  (err) => console.error("Failed:", err)    // error callback
);

// Real-world: Node.js fs.readFile callback
const fs = require("fs");
fs.readFile("file.txt", "utf8", (err, data) => {
  if (err) {
    console.error("Error reading file:", err);
    return;
  }
  console.log("File contents:", data);
});

// Real-world: setTimeout
setTimeout(() => {
  console.log("This runs after 2 seconds");
}, 2000);

// Real-world: Event listener
button.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

---

## 8. Callback Hell & Inversion of Control

### Callback Hell (Pyramid of Doom)
When multiple async operations depend on each other, callbacks nest deeply:

```javascript
// Get user → get their orders → get order details → get product info → 😱
getUser(userId, (err, user) => {
  if (err) return handleError(err);

  getOrders(user.id, (err, orders) => {
    if (err) return handleError(err);

    getOrderDetails(orders[0].id, (err, details) => {
      if (err) return handleError(err);

      getProduct(details.productId, (err, product) => {
        if (err) return handleError(err);

        getReviews(product.id, (err, reviews) => {
          if (err) return handleError(err);
          // Finally use the data... after 5 levels of nesting 😵
          console.log(reviews);
        });
      });
    });
  });
});
```

**Problems with Callback Hell:**
- Code reads right-to-left instead of top-to-bottom
- Error handling must be repeated at every level
- Very hard to debug — stack traces are confusing
- Adding new steps means more nesting
- Hard to use `try/catch`

### Inversion of Control (IoC)
When using callbacks, you hand your function to someone else's code and **lose control** over:
- When it gets called
- How many times it gets called (once? twice? never?)
- What arguments it receives
- Whether errors are handled

```javascript
// You're trusting thirdPartyLibrary to call YOUR function correctly
thirdPartyLibrary.processPayment(data, myCallback);
// What if they call it twice? What if they never call it? You don't know!
```

**Promises solve both problems** by providing a standardized interface.

---

## 9. Promises — Complete Guide

### What is a Promise?
A **Promise** is a JavaScript object that represents the **eventual result** of an asynchronous operation. It's a guarantee: "I don't have the value yet, but I will either give you the value (resolve) or tell you it failed (reject)."

```javascript
// A promise is in one of 3 states:
//
// PENDING → the async work is still running
//     ↓           ↓
// FULFILLED    REJECTED
// (success)    (failure)
//
// Once settled (fulfilled or rejected), a promise CANNOT change state again.
```

### Creating a Promise

```javascript
const myPromise = new Promise((resolve, reject) => {
  // The executor function runs immediately (synchronously)

  // Simulate async work (e.g., network request)
  setTimeout(() => {
    const success = true;

    if (success) {
      resolve("Here is your data! ✅");   // fulfills the promise
    } else {
      reject(new Error("Something went wrong ❌"));  // rejects the promise
    }
  }, 2000);
});

// At this point, myPromise is PENDING...
```

### Consuming a Promise

```javascript
myPromise
  .then(value => {
    // Called when promise FULFILLS
    // value = whatever was passed to resolve()
    console.log("Success:", value);   // "Success: Here is your data! ✅"
    return value.toUpperCase();       // return value passes to next .then()
  })
  .then(upperValue => {
    console.log("Transformed:", upperValue);
  })
  .catch(error => {
    // Called when promise REJECTS
    // error = whatever was passed to reject()
    console.error("Error:", error.message);
  })
  .finally(() => {
    // ALWAYS runs — whether fulfilled or rejected
    // Perfect for cleanup: hide loading spinner, close connection
    console.log("Done! (success or failure)");
    hideLoadingSpinner();
  });
```

### Promise States — Visual

```
new Promise(executor runs immediately)
        ↓
    [PENDING]
    /        \
resolve()   reject()
   ↓              ↓
[FULFILLED]   [REJECTED]
   ↓              ↓
.then()       .catch()
   ↓              ↓
.finally()  .finally()
```

### Promise — Complete Reference

```javascript
// ─── Creating ──────────────────────────────────────────────────
new Promise((resolve, reject) => { ... })

// Already resolved (shortcut)
Promise.resolve("immediate value")
// Already rejected (shortcut)
Promise.reject(new Error("immediate error"))

// ─── Consuming ─────────────────────────────────────────────────
promise.then(onFulfilled)           // success handler
promise.then(onFulfilled, onRejected)  // both handlers in one .then
promise.catch(onRejected)           // error handler
promise.finally(onSettled)          // always runs after settled

// ─── States ────────────────────────────────────────────────────
// pending     → neither resolved nor rejected yet
// fulfilled   → resolve() was called; .then() will run
// rejected    → reject() was called; .catch() will run
// settled     → either fulfilled OR rejected (final state)

// ─── Important Rules ───────────────────────────────────────────
// 1. A promise can only be settled ONCE — state is permanent
// 2. .then() ALWAYS returns a new Promise (enables chaining)
// 3. Callbacks in .then/.catch are ALWAYS async (microtask queue)
// 4. Even Promise.resolve().then(fn) — fn runs after sync code
```

---

## 10. Promise Chaining

`.then()` always returns a **new Promise**. This allows you to chain multiple async steps cleanly.

### Basic Chain
```javascript
fetch("https://api.example.com/users/1")
  .then(response => response.json())        // Step 1: parse JSON → returns Promise
  .then(user => {
    console.log("Got user:", user.name);
    return fetch(`https://api.example.com/orders/${user.id}`); // Step 2: new request
  })
  .then(response => response.json())        // Step 3: parse orders JSON
  .then(orders => {
    console.log("Orders:", orders);
    return orders;
  })
  .catch(error => {
    // Catches ANY error from ANY step above
    console.error("Something failed:", error);
  })
  .finally(() => {
    hideLoadingSpinner();
  });
```

### How Values Flow Through the Chain
```javascript
Promise.resolve(1)
  .then(v => v + 1)         // receives 1, returns 2
  .then(v => v * 3)         // receives 2, returns 6
  .then(v => v - 2)         // receives 6, returns 4
  .then(v => console.log(v)) // receives 4, prints 4
// Output: 4
```

### Returning a Promise Inside .then (Flattening)
```javascript
// When you return a Promise inside .then(), the chain WAITS for it to settle
fetch("/api/step1")
  .then(r => r.json())
  .then(data => {
    // Returns a NEW promise — chain pauses here until this resolves
    return fetch("/api/step2?input=" + data.value);
  })
  .then(r => r.json())    // this only runs AFTER step2 fetch is done
  .then(finalData => console.log(finalData));
```

### Skipping Steps on Error
```javascript
Promise.resolve("start")
  .then(v => { throw new Error("step 2 failed!"); })   // throws error
  .then(v => console.log("step 3"))    // SKIPPED — error bypasses this
  .then(v => console.log("step 4"))    // SKIPPED — still propagating error
  .catch(err => {
    console.log("Caught:", err.message);   // "Caught: step 2 failed!"
    return "recovered";                    // chain resumes from here
  })
  .then(v => console.log("Step 5:", v))   // "Step 5: recovered"
```

---

## 11. Promise Error Handling

### Rules of Error Handling

```javascript
// Rule 1: .catch() at the END catches ALL errors in the chain
somePromise
  .then(step1)
  .then(step2)
  .then(step3)
  .catch(handleAnyError);    // ✅ one catch for the whole chain

// Rule 2: Error in .then() jumps to next .catch()
Promise.resolve()
  .then(() => { throw new Error("oops"); })
  .then(() => console.log("this skipped"))  // skipped
  .catch(err => console.log("caught:", err.message));  // "caught: oops"

// Rule 3: Returning inside .catch() RECOVERS the chain
Promise.reject("error")
  .catch(err => {
    console.log("Handled:", err);
    return "default value";    // recover with a fallback value
  })
  .then(val => console.log("Continuing:", val));  // "Continuing: default value"

// Rule 4: Throwing inside .catch() passes error to next .catch()
Promise.reject("original error")
  .catch(err => {
    throw new Error("new error from catch");  // re-throw
  })
  .catch(err => console.log("caught again:", err.message));

// Rule 5: Unhandled rejections are DANGEROUS
const p = Promise.reject("no handler");
// If nothing handles this → UnhandledPromiseRejection warning/crash!
// Always add .catch() or handle in async/await with try/catch

// Rule 6: .finally() DOES NOT receive a value — it's for cleanup only
promise
  .then(v => v * 2)
  .finally(() => hideSpinner())      // no argument — just runs cleanup
  .then(v => console.log(v));        // v is STILL the original v * 2
```

### Where to Place .catch()

```javascript
// ❌ BAD: .catch() in the MIDDLE only catches errors BEFORE it
promise
  .then(step1)
  .catch(earlyError)    // only catches step1 errors
  .then(step2)          // errors from step2 are NOT caught!
  .then(step3)

// ✅ GOOD: .catch() at the END catches everything
promise
  .then(step1)
  .then(step2)
  .then(step3)
  .catch(handleAll)     // catches errors from step1, step2, OR step3
```

---

## 12. Promise Utility Functions

### `Promise.all()` — Wait for ALL to succeed

```javascript
const p1 = fetch("/api/users").then(r => r.json());
const p2 = fetch("/api/products").then(r => r.json());
const p3 = fetch("/api/orders").then(r => r.json());

// All 3 requests run IN PARALLEL
Promise.all([p1, p2, p3])
  .then(([users, products, orders]) => {
    // Only runs when ALL THREE succeed
    console.log("Users:", users);
    console.log("Products:", products);
    console.log("Orders:", orders);
  })
  .catch(error => {
    // Rejects IMMEDIATELY if ANY ONE fails
    console.error("One of them failed:", error);
  });
```

**Rules:**
- ✅ Waits for ALL to resolve → returns array of results (in same order as input)
- ❌ If ANY one rejects → immediately rejects with that error (others are ignored)
- Best for **independent parallel tasks** that all must succeed

---

### `Promise.allSettled()` — Wait for ALL regardless of outcome

```javascript
const promises = [
  fetch("/api/users").then(r => r.json()),
  fetch("/api/broken-endpoint"),       // this one will fail
  fetch("/api/products").then(r => r.json())
];

Promise.allSettled(promises)
  .then(results => {
    results.forEach((result, index) => {
      if (result.status === "fulfilled") {
        console.log(`Promise ${index} succeeded:`, result.value);
      } else {
        console.log(`Promise ${index} failed:`, result.reason);
      }
    });
  });

// Each result is:
// { status: "fulfilled", value: ... }   ← for success
// { status: "rejected",  reason: ... }  ← for failure
```

**Rules:**
- ✅ Always waits for ALL promises to finish (never rejects)
- Returns array of result objects with `status` + `value` or `reason`
- Best for **when you need results from all, even failed ones** (e.g., batch operations, reporting)

---

### `Promise.race()` — First one to settle wins

```javascript
// Useful for timeouts!
function withTimeout(promise, ms) {
  const timeout = new Promise((_, reject) =>
    setTimeout(() => reject(new Error(`Timed out after ${ms}ms`)), ms)
  );
  return Promise.race([promise, timeout]);
}

withTimeout(fetch("/api/slow-endpoint"), 5000)
  .then(r => r.json())
  .then(data => console.log("Got data:", data))
  .catch(err => console.error(err.message));   // "Timed out after 5000ms"
```

**Rules:**
- ✅ Settles (fulfills OR rejects) as soon as the FIRST promise settles
- Other promises still run, but their results are ignored
- Best for **timeout patterns** or **racing multiple data sources**

---

### `Promise.any()` — First SUCCESS wins

```javascript
// Try 3 servers — use whichever responds first successfully
const servers = [
  fetch("https://server1.api.com/data"),
  fetch("https://server2.api.com/data"),
  fetch("https://server3.api.com/data")
];

Promise.any(servers)
  .then(response => response.json())
  .then(data => console.log("Fastest server won:", data))
  .catch(err => {
    // AggregateError — rejects only if ALL promises reject
    console.error("All servers failed:", err.errors);
  });
```

**Rules:**
- ✅ Resolves as soon as ANY promise fulfills (ignores rejections)
- ❌ Only rejects if ALL promises reject — throws `AggregateError`
- Best for **redundancy** — try multiple sources, use the fastest success

---

### Summary Comparison Table

| Method | Resolves When | Rejects When | Best For |
|--------|-------------|-------------|---------|
| `Promise.all` | ALL succeed | ANY fails (immediately) | All must succeed (parallel fetches) |
| `Promise.allSettled` | ALL finish (any state) | Never rejects | Need ALL results, even failures |
| `Promise.race` | ANY settles (first win or lose) | ANY rejects (if it's first) | Timeout implementation |
| `Promise.any` | ANY succeeds | ALL fail (AggregateError) | Redundancy, fastest server wins |

---

## 13. Promisification

**Promisification** = wrapping an old callback-based function in a Promise wrapper so it can be used with `.then()` and `async/await`.

### Manual Promisification

```javascript
// ─── Promisify setTimeout ──────────────────────────────────────
function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// Usage:
await delay(2000);   // wait 2 seconds
console.log("2 seconds passed!");

// Or chain:
delay(1000).then(() => console.log("Done waiting!"));

// ─── Promisify Node.js fs.readFile ────────────────────────────
function readFile(path, encoding = "utf8") {
  return new Promise((resolve, reject) => {
    fs.readFile(path, encoding, (err, data) => {
      if (err) reject(err);       // pass error to .catch()
      else resolve(data);         // pass data to .then()
    });
  });
}

// Usage:
readFile("config.json")
  .then(data => JSON.parse(data))
  .then(config => console.log(config))
  .catch(err => console.error("Could not read file:", err));

// ─── Promisify any (error, result) callback pattern ───────────
function promisify(callbackFn) {
  return function (...args) {
    return new Promise((resolve, reject) => {
      callbackFn(...args, (err, result) => {
        if (err) reject(err);
        else resolve(result);
      });
    });
  };
}

const readFileAsync = promisify(fs.readFile);
const writeFileAsync = promisify(fs.writeFile);

// Usage:
readFileAsync("input.txt", "utf8")
  .then(data => writeFileAsync("output.txt", data.toUpperCase()))
  .then(() => console.log("Done!"))
  .catch(console.error);
```

### Node.js Built-in Promisify
```javascript
const { promisify } = require("util");
const fs = require("fs");

const readFileAsync = promisify(fs.readFile);
const writeFileAsync = promisify(fs.writeFile);

// Or use fs.promises directly (Node.js 10+):
const { readFile, writeFile } = require("fs/promises");
const data = await readFile("file.txt", "utf8");
```

---

## 14. async / await — Complete Guide

`async/await` is **syntactic sugar over Promises** that makes async code look and read like synchronous code. Under the hood, it still uses Promises and the microtask queue.

### `async` Function

```javascript
// async keyword: this function ALWAYS returns a Promise
async function greet() {
  return "Hello!";               // implicitly wrapped in Promise.resolve("Hello!")
}

greet().then(msg => console.log(msg));   // "Hello!"
// OR:
const msg = await greet();              // "Hello!"

// Equivalent to:
function greet() {
  return Promise.resolve("Hello!");
}
```

### `await` Keyword

```javascript
// await can ONLY be used inside an async function (or top-level module)
async function fetchUser(id) {
  // await PAUSES this function and yields to the event loop
  // (other code can run while waiting — the thread is NOT blocked)
  const response = await fetch(`/api/users/${id}`);

  // Execution resumes HERE when the fetch promise resolves
  const user = await response.json();

  return user;   // wrapped in Promise.resolve(user)
}

// Call an async function:
fetchUser(1).then(user => console.log(user));
// OR:
const user = await fetchUser(1);   // in another async function or top-level module
```

### Sequential vs Parallel Execution

```javascript
// ─── Sequential (one after the other) ────────────────────────
// Each await waits for the previous to finish — total time = A + B + C
async function sequential() {
  const a = await fetchA();   // takes 1s
  const b = await fetchB();   // takes 1s — waits for A first
  const c = await fetchC();   // takes 1s — waits for B first
  return [a, b, c];           // total: ~3 seconds
}

// ─── Parallel (all at once — MUCH FASTER for independent tasks) ─
// All three start simultaneously — total time = max(A, B, C)
async function parallel() {
  const [a, b, c] = await Promise.all([
    fetchA(),   // starts immediately
    fetchB(),   // starts immediately
    fetchC()    // starts immediately
  ]);
  return [a, b, c];    // total: ~1 second!
}

// ─── Start all, await each result (another parallel pattern) ──
async function parallelV2() {
  const promiseA = fetchA();   // all start NOW
  const promiseB = fetchB();
  const promiseC = fetchC();

  const a = await promiseA;   // now we await results in order
  const b = await promiseB;
  const c = await promiseC;
  return [a, b, c];
}
```

### await in Loops

```javascript
// ❌ WRONG: forEach does NOT work with await (it ignores the async callback)
async function processAll(ids) {
  ids.forEach(async (id) => {
    const user = await fetchUser(id);  // these don't actually wait!
    console.log(user);
  });
  console.log("All done?");  // prints FIRST — before users are fetched!
}

// ✅ CORRECT Option 1: for...of loop (sequential)
async function processSequential(ids) {
  for (const id of ids) {
    const user = await fetchUser(id);  // waits for each one
    console.log(user);
  }
  console.log("All done!");  // prints AFTER all users are fetched
}

// ✅ CORRECT Option 2: Promise.all with map (parallel)
async function processParallel(ids) {
  const users = await Promise.all(ids.map(id => fetchUser(id)));
  users.forEach(user => console.log(user));
  console.log("All done!");
}
```

### Top-Level await (ES2022+)
```javascript
// In ES modules (.mjs files or type: "module" in package.json):
const data = await fetch("/api/data").then(r => r.json());
console.log(data);  // works at the TOP LEVEL — no async wrapper needed!
```

### How await Works Under the Hood
```javascript
async function example() {
  console.log("A");
  const result = await somePromise;   // function pauses HERE
  console.log("B", result);           // resumes as microtask when promise resolves
}

console.log("X");
example();
console.log("Y");

// Output: X → A → Y → B (result)
// Because:
// 1. "X" (sync)
// 2. example() called → "A" (sync inside async fn)
// 3. await hit → function PAUSES, control returns to caller
// 4. "Y" (sync, back in outer scope)
// 5. somePromise resolves → "B" resumes as microtask
```

---

## 15. Error Handling with async/await

```javascript
// ─── try/catch (clean, recommended) ──────────────────────────
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Fetch failed:", error.message);
    throw error;    // re-throw so caller can handle if needed
  } finally {
    hideLoadingSpinner();   // always runs
  }
}

// ─── Specific error types ─────────────────────────────────────
async function processData() {
  try {
    const data = await riskyOperation();
    return data;
  } catch (error) {
    if (error instanceof TypeError) {
      console.log("Type error:", error.message);
    } else if (error instanceof NetworkError) {
      console.log("Network error — retry?");
      return await processData();   // retry (careful with infinite loops!)
    } else {
      throw error;   // unknown error — re-throw
    }
  }
}

// ─── .catch() on async function call ─────────────────────────
const user = await fetchUser(1).catch(err => {
  console.log("Failed:", err);
  return null;    // fallback value
});

// ─── Wrapper utility — avoid try/catch everywhere ────────────
async function safe(promise) {
  try {
    const data = await promise;
    return [null, data];    // [error, data] pattern
  } catch (error) {
    return [error, null];
  }
}

// Usage:
const [err, user] = await safe(fetchUser(1));
if (err) {
  console.log("Error:", err.message);
} else {
  console.log("User:", user);
}
```

---

## 16. async/await vs Promises

| Feature | Promises | async/await |
|---------|---------|-------------|
| Syntax | `.then().catch()` chaining | Looks like sync code |
| Error handling | `.catch()` | `try/catch` |
| Readability | Gets complex with chains | Very clean and readable |
| Debugging | Stack traces can be confusing | Much better stack traces |
| Under the hood | Foundation | Built ON TOP of Promises |
| Parallel | `Promise.all()` | `await Promise.all()` |
| Conditional logic | Complex in chains | Easy — just use `if/else` |
| Loops | Tricky with `.then()` | Easy with `for...of` |

### Same code — two styles
```javascript
// ─── Promise style ────────────────────────────────────────────
function loadUserData(userId) {
  return fetch(`/api/users/${userId}`)
    .then(res => {
      if (!res.ok) throw new Error("User not found");
      return res.json();
    })
    .then(user => fetch(`/api/orders/${user.id}`))
    .then(res => res.json())
    .then(orders => ({ user, orders }))
    .catch(err => {
      console.error("Error:", err);
      throw err;
    });
}

// ─── async/await style ────────────────────────────────────────
async function loadUserData(userId) {
  try {
    const userRes = await fetch(`/api/users/${userId}`);
    if (!userRes.ok) throw new Error("User not found");
    const user = await userRes.json();

    const ordersRes = await fetch(`/api/orders/${user.id}`);
    const orders = await ordersRes.json();

    return { user, orders };
  } catch (err) {
    console.error("Error:", err);
    throw err;
  }
}
// Much easier to read and debug!
```

---

## 17. Real-World Async Patterns

### Pattern 1: Loading State Management
```javascript
async function loadDashboard() {
  showLoadingSpinner();
  setError(null);

  try {
    const [users, stats, notifications] = await Promise.all([
      fetchUsers(),
      fetchStats(),
      fetchNotifications()
    ]);

    renderDashboard({ users, stats, notifications });
  } catch (error) {
    setError("Failed to load dashboard. Please refresh.");
    console.error(error);
  } finally {
    hideLoadingSpinner();   // always hide spinner
  }
}
```

### Pattern 2: Retry with Exponential Backoff
```javascript
async function fetchWithRetry(url, maxRetries = 3) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error(`HTTP ${response.status}`);
      return await response.json();
    } catch (error) {
      if (attempt === maxRetries - 1) throw error;  // last attempt — give up

      const waitMs = Math.pow(2, attempt) * 1000;   // 1s, 2s, 4s...
      console.log(`Attempt ${attempt + 1} failed. Retrying in ${waitMs}ms...`);
      await delay(waitMs);
    }
  }
}
```

### Pattern 3: Race with Timeout
```javascript
async function fetchWithTimeout(url, timeoutMs = 5000) {
  const timeoutPromise = new Promise((_, reject) =>
    setTimeout(() => reject(new Error("Request timed out")), timeoutMs)
  );

  try {
    const data = await Promise.race([
      fetch(url).then(r => r.json()),
      timeoutPromise
    ]);
    return data;
  } catch (error) {
    if (error.message === "Request timed out") {
      console.warn("Slow network — falling back to cached data");
      return getCachedData(url);
    }
    throw error;
  }
}
```

### Pattern 4: Concurrent with Concurrency Limit
```javascript
// Process N items at once (not all at once, not one at a time)
async function processWithConcurrency(items, processFn, concurrency = 3) {
  const results = [];

  for (let i = 0; i < items.length; i += concurrency) {
    const chunk = items.slice(i, i + concurrency);
    const chunkResults = await Promise.all(chunk.map(processFn));
    results.push(...chunkResults);
  }

  return results;
}

// Usage: process 3 files at a time
const results = await processWithConcurrency(files, uploadFile, 3);
```

### Pattern 5: Cache-First with Async
```javascript
const cache = new Map();

async function getCachedOrFetch(key, fetchFn, ttlMs = 60000) {
  const cached = cache.get(key);

  if (cached && Date.now() - cached.timestamp < ttlMs) {
    return cached.data;   // return from cache
  }

  const data = await fetchFn();   // fetch fresh data
  cache.set(key, { data, timestamp: Date.now() });
  return data;
}

// Usage:
const user = await getCachedOrFetch(
  `user:${id}`,
  () => fetchUser(id),
  30000  // 30 second cache
);
```

### Pattern 6: Queue Processing
```javascript
class AsyncQueue {
  constructor(concurrency = 1) {
    this.concurrency = concurrency;
    this.running = 0;
    this.queue = [];
  }

  async add(fn) {
    if (this.running >= this.concurrency) {
      await new Promise(resolve => this.queue.push(resolve));
    }

    this.running++;
    try {
      return await fn();
    } finally {
      this.running--;
      if (this.queue.length > 0) {
        this.queue.shift()();  // unblock next waiting task
      }
    }
  }
}

const queue = new AsyncQueue(2);  // max 2 concurrent tasks
await queue.add(() => uploadFile("file1.txt"));
await queue.add(() => uploadFile("file2.txt"));
```

---

## 18. Common Async Mistakes & Fixes

### ❌ Mistake 1: Forgetting await
```javascript
// ❌ WRONG — returns Promise, not the actual value
async function getUser() {
  const response = fetch("/api/user");    // forgot await!
  const data = response.json();           // response is a Promise, not a Response object
  return data;
}

// ✅ CORRECT
async function getUser() {
  const response = await fetch("/api/user");
  const data = await response.json();
  return data;
}
```

### ❌ Mistake 2: await in forEach
```javascript
// ❌ WRONG — forEach ignores async/await
async function bad() {
  items.forEach(async (item) => {
    await processItem(item);  // these don't actually sequence!
  });
  console.log("done?");  // prints BEFORE items are processed
}

// ✅ CORRECT — use for...of for sequential
async function good() {
  for (const item of items) {
    await processItem(item);
  }
  console.log("Actually done!");
}

// ✅ OR — use Promise.all for parallel
async function goodParallel() {
  await Promise.all(items.map(item => processItem(item)));
  console.log("All done in parallel!");
}
```

### ❌ Mistake 3: Sequential when Parallel is possible
```javascript
// ❌ SLOW — 3 seconds total (waits 1s + 1s + 1s)
async function slow() {
  const a = await fetchA();   // 1s
  const b = await fetchB();   // 1s (waits for a unnecessarily)
  const c = await fetchC();   // 1s (waits for b unnecessarily)
}

// ✅ FAST — ~1 second total (all 3 run at same time)
async function fast() {
  const [a, b, c] = await Promise.all([fetchA(), fetchB(), fetchC()]);
}
```

### ❌ Mistake 4: Returning value inside .then() from async function
```javascript
// ❌ WRONG — reading data BEFORE async operation completes
function getData() {
  let result;
  fetch("/api/data")
    .then(r => r.json())
    .then(data => { result = data; });   // async — runs later!
  return result;   // undefined! function returns before fetch completes
}

// ✅ CORRECT — return the promise itself
function getData() {
  return fetch("/api/data").then(r => r.json());
}
// OR use async/await:
async function getData() {
  const r = await fetch("/api/data");
  return r.json();
}
```

### ❌ Mistake 5: Missing error handling
```javascript
// ❌ WRONG — unhandled rejection = crash/warning
async function risky() {
  const data = await fetch("/api/might-fail").then(r => r.json());
  return data;
}
// If fetch fails → UnhandledPromiseRejection!

// ✅ CORRECT — always handle errors
async function safe() {
  try {
    const data = await fetch("/api/might-fail").then(r => r.json());
    return data;
  } catch (err) {
    console.error("Failed:", err);
    return null;    // or throw err to let caller handle
  }
}
```

### ❌ Mistake 6: Creating a Promise inside an async function unnecessarily
```javascript
// ❌ ANTIPATTERN — "explicit Promise construction antipattern"
async function unnecessary() {
  return new Promise(async (resolve, reject) => {
    try {
      const data = await fetch("/api");
      resolve(data);
    } catch (err) {
      reject(err);
    }
  });
}

// ✅ CORRECT — async function already returns a Promise!
async function correct() {
  const data = await fetch("/api");
  return data;
}
```

---

## 19. Timers — setTimeout, setInterval, clearTimeout

### setTimeout
```javascript
// Run fn once after delay milliseconds
const timerId = setTimeout(fn, delay, arg1, arg2);

// Basic usage
setTimeout(() => {
  console.log("Runs once after 2 seconds");
}, 2000);

// Pass arguments
setTimeout((name, age) => {
  console.log(`Hello ${name}, you are ${age}`);
}, 1000, "Alice", 30);

// Cancel it before it fires
const id = setTimeout(() => console.log("never runs"), 5000);
clearTimeout(id);   // timer cancelled

// Async delay helper
const delay = (ms) => new Promise(resolve => setTimeout(resolve, ms));
await delay(1000);   // pause for 1 second in async function
```

### setInterval
```javascript
// Run fn repeatedly every delay milliseconds
const intervalId = setInterval(fn, delay);

// Example: update clock every second
const clockId = setInterval(() => {
  document.getElementById("clock").textContent = new Date().toLocaleTimeString();
}, 1000);

// Stop the interval
clearInterval(clockId);

// ─── setInterval with async function ─────────────────────────
// ❌ WRONG — interval fires regardless of async completion
setInterval(async () => {
  await syncData();  // if this takes >1s, calls pile up!
}, 1000);

// ✅ CORRECT — use setTimeout recursively
async function scheduleSync() {
  while (true) {
    await syncData();
    await delay(1000);   // wait AFTER completion
  }
}
```

### setImmediate (Node.js only)
```javascript
// Runs after current I/O callbacks, before setTimeout
setImmediate(() => console.log("setImmediate"));
setTimeout(() => console.log("setTimeout 0"), 0);
// In Node.js inside I/O callback: setImmediate runs first
```

### process.nextTick (Node.js only — HIGHEST priority)
```javascript
// Runs before ANY I/O, before microtask queue
process.nextTick(() => console.log("nextTick — runs first!"));
Promise.resolve().then(() => console.log("Promise microtask"));
// Output: nextTick → Promise microtask
```

---

## 20. fetch API — HTTP Requests

```javascript
// ─── Basic GET ────────────────────────────────────────────────
const response = await fetch("https://api.example.com/users");
if (!response.ok) {
  throw new Error(`HTTP error! Status: ${response.status}`);
}
const users = await response.json();

// ─── Parsing response body ────────────────────────────────────
response.json()     // parse as JSON → returns Promise<Object>
response.text()     // parse as plain text → returns Promise<string>
response.blob()     // parse as binary Blob (images, files)
response.arrayBuffer()  // raw binary data
response.formData()     // FormData object

// ─── Response properties ──────────────────────────────────────
response.ok          // true if status 200–299
response.status      // HTTP status code (200, 404, 500, etc.)
response.statusText  // "OK", "Not Found", etc.
response.headers     // response headers

// ─── POST request ─────────────────────────────────────────────
const response = await fetch("/api/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Authorization": `Bearer ${token}`
  },
  body: JSON.stringify({ name: "Alice", age: 30 })
});
const created = await response.json();

// ─── PUT request ──────────────────────────────────────────────
await fetch(`/api/users/${id}`, {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Alice Updated" })
});

// ─── DELETE request ───────────────────────────────────────────
await fetch(`/api/users/${id}`, { method: "DELETE" });

// ─── PATCH request ────────────────────────────────────────────
await fetch(`/api/users/${id}`, {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ age: 31 })
});

// ─── Sending form data ────────────────────────────────────────
const form = new FormData();
form.append("file", fileInput.files[0]);
form.append("name", "myfile.pdf");
await fetch("/api/upload", { method: "POST", body: form });
// Don't set Content-Type when using FormData — browser sets it automatically

// ─── Query parameters ─────────────────────────────────────────
const params = new URLSearchParams({ page: 1, limit: 10, search: "alice" });
const response = await fetch(`/api/users?${params}`);

// ─── Full wrapper utility ─────────────────────────────────────
async function api(endpoint, options = {}) {
  const baseURL = "https://api.example.com";
  const defaults = {
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${getToken()}`
    }
  };

  const config = {
    ...defaults,
    ...options,
    headers: { ...defaults.headers, ...options.headers }
  };

  const response = await fetch(`${baseURL}${endpoint}`, config);

  if (!response.ok) {
    const error = await response.json().catch(() => ({}));
    throw new Error(error.message || `HTTP ${response.status}`);
  }

  return response.json();
}

// Usage:
const user = await api("/users/1");
const created = await api("/users", {
  method: "POST",
  body: JSON.stringify({ name: "Alice" })
});
```

---

## 21. AbortController — Cancel Requests

```javascript
// Create a controller for cancellation
const controller = new AbortController();
const signal = controller.signal;

// Start a fetch with the signal attached
const fetchPromise = fetch("/api/slow-endpoint", { signal });

// Cancel after 5 seconds
const timeoutId = setTimeout(() => {
  controller.abort();   // sends abort signal
  console.log("Request aborted!");
}, 5000);

try {
  const response = await fetchPromise;
  clearTimeout(timeoutId);   // cancel timeout if fetch succeeds
  const data = await response.json();
  console.log("Data:", data);
} catch (error) {
  if (error.name === "AbortError") {
    console.log("Fetch was cancelled");
  } else {
    throw error;
  }
}

// ─── React pattern — cancel on component unmount ──────────────
useEffect(() => {
  const controller = new AbortController();

  async function loadData() {
    try {
      const data = await fetch("/api/data", {
        signal: controller.signal
      }).then(r => r.json());
      setData(data);
    } catch (error) {
      if (error.name !== "AbortError") {
        setError(error.message);
      }
    }
  }

  loadData();

  return () => controller.abort();  // cleanup: cancel on unmount
}, []);
```

---

## 22. Best Practices

### ✅ DOs

**Promises:**
- Always add `.catch()` at the end of every promise chain
- Use `Promise.all()` for independent parallel tasks — don't await sequentially
- Use `Promise.allSettled()` when you need ALL results even if some fail
- Return a value or throw in `.catch()` to control chain recovery

**async/await:**
- Use `try/catch` for error handling in async functions
- Always `await` async calls — never ignore them
- Use `for...of` for sequential async loops
- Start all independent promises before awaiting them for parallelism

**General:**
- Set timeouts on all network requests — never trust external services
- Handle all promise rejections — no unhandled rejections in production
- Log errors with context — `console.error("Failed fetching user:", err)`
- Use `finally` for cleanup (spinners, connections, locks)

### ❌ DON'Ts

- Never `await` inside `forEach` — use `for...of` or `Promise.all().map()`
- Never create a Promise unnecessarily inside an async function
- Never leave promises unhandled — always `.catch()` or `try/catch`
- Never do sequential awaits for independent operations (performance killer)
- Never ignore the return value of an async function without awaiting it
- Never block the main thread with heavy synchronous computation — use Web Workers

---

## 23. Common Interview Questions

### Q1: What is the Event Loop and how does it work?
**Answer:** The Event Loop is JavaScript's scheduler that manages async execution despite JS being single-threaded. It continuously monitors the Call Stack. When the stack is empty, it first drains the entire Microtask Queue (Promise callbacks, queueMicrotask), then picks one task from the Macrotask Queue (setTimeout, setInterval). This repeats indefinitely, giving JavaScript non-blocking async capabilities.

### Q2: What is the difference between Microtasks and Macrotasks?
**Answer:** Microtasks (Promise `.then`, `queueMicrotask`, `async/await` continuations) have higher priority — the entire microtask queue is drained after every piece of sync code, before any macrotask runs. Macrotasks (setTimeout, setInterval, DOM events, I/O) run one at a time, after all microtasks. This is why `Promise.resolve().then(fn)` runs before `setTimeout(fn, 0)`, even if setTimeout was scheduled first.

### Q3: What is Callback Hell and how do Promises solve it?
**Answer:** Callback Hell (Pyramid of Doom) is deeply nested callbacks for sequential async operations, making code unreadable, hard to debug, and error handling repetitive at every level. It also suffers from "Inversion of Control" — you hand your function to third-party code and lose control over when/how it's called. Promises solve this by providing a flat chainable `.then()` interface, a single `.catch()` for error handling, and a standardized contract that always calls your handler predictably once.

### Q4: What are the states of a Promise?
**Answer:** Three states: (1) **Pending** — initial state, async work still running; (2) **Fulfilled** — `resolve()` was called, success value available, `.then()` runs; (3) **Rejected** — `reject()` was called, error available, `.catch()` runs. Once a promise is **settled** (fulfilled or rejected), its state is permanent — it cannot change again.

### Q5: What is the difference between `Promise.all`, `Promise.allSettled`, `Promise.race`, and `Promise.any`?
**Answer:** `Promise.all` waits for ALL to succeed — immediately rejects if any fails. `Promise.allSettled` waits for ALL to finish regardless of outcome — never rejects, returns status objects. `Promise.race` settles as soon as the FIRST promise settles (win or lose) — useful for timeouts. `Promise.any` resolves as soon as the FIRST succeeds — only rejects if ALL fail (AggregateError), useful for redundancy.

### Q6: What does `async/await` do under the hood?
**Answer:** `async/await` is syntactic sugar over Promises. An `async` function always returns a Promise (even if you return a plain value — it's wrapped in `Promise.resolve()`). The `await` keyword pauses the async function's execution (without blocking the Call Stack) and registers the rest of the function as a microtask. When the awaited Promise resolves, the microtask runs and the function resumes. The thread is free to handle other code while waiting.

### Q7: Why should you NOT use `await` inside `forEach`?
**Answer:** `Array.forEach` does not await the return value of its callback. It calls the async callback and immediately moves to the next iteration without waiting for the promise to resolve. The result: all iterations kick off simultaneously (or in unpredictable order), and the code after `forEach` runs before any async work completes. Use `for...of` for sequential processing, or `Promise.all(array.map(async fn))` for parallel processing.

### Q8: What is the difference between sequential and parallel async execution?
**Answer:** Sequential: each `await` waits for the previous to finish before starting the next — total time = sum of all times. Use when operations depend on each other's results. Parallel: start all promises simultaneously, then await them together with `Promise.all()` — total time = longest single operation. Use when operations are independent. Parallel is dramatically faster for multiple independent API calls.

### Q9: What happens if a Promise is rejected with no `.catch()` handler?
**Answer:** It becomes an **unhandled rejection**. In browsers, this fires the `unhandledrejection` event and usually logs a severe warning. In Node.js, newer versions crash the process with exit code 1 (since Node 15+). This is a serious bug — it means your application silently fails with no error recovery. Always handle rejections with `.catch()` or `try/catch`.

### Q10: What is promisification and when would you use it?
**Answer:** Promisification wraps a legacy callback-based function (usually following the `(error, result)` pattern) inside a new Promise, giving it a `.then/.catch` interface compatible with async/await. You use it when working with older APIs (Node.js `fs`, `crypto`, etc.) that predate Promises. Node.js provides `util.promisify()` for this automatically. Modern Node.js also ships `fs/promises`, `dns/promises`, etc. — pre-promisified versions of standard modules.

---

## 🔄 Async JavaScript — Complete Execution Flow

```
JS Code starts executing (synchronous)
             ↓
       Call Stack runs
       (one at a time)
             ↓
   Async task encountered
   (setTimeout, fetch, etc.)
             ↓
  Handed to Browser/Node API
  (runs OUTSIDE JS thread)
             ↓
  JS continues next line
  (non-blocking!)
             ↓
  API completes task
             ↓
  Callback added to queue:
  ┌──────────────────────┐
  │ Microtask Queue?     │ ← Promise .then, async/await
  │ Macrotask Queue?     │ ← setTimeout, setInterval
  └──────────────────────┘
             ↓
  Call Stack empties
             ↓
  Event Loop runs ALL microtasks first
             ↓
  Event Loop picks ONE macrotask
             ↓
  Repeat forever ♾️
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Single-threaded | JS has one Call Stack — executes one thing at a time |
| Call Stack | Where synchronous code runs — LIFO (Last In, First Out) |
| Web APIs | Browser/Node environment handles async tasks outside JS thread |
| Event Loop | Scheduler: moves callbacks from queues to Call Stack when it's empty |
| Microtask Queue | High-priority queue: Promise.then, queueMicrotask, async/await |
| Macrotask Queue | Normal queue: setTimeout, setInterval, DOM events, I/O |
| Execution Order | Sync → All Microtasks → ONE Macrotask → All Microtasks → repeat |
| Callback | Function passed as argument; called when async work completes |
| Callback Hell | Deeply nested callbacks; unreadable "Pyramid of Doom" |
| Inversion of Control | Losing control over when/how your callback is called |
| Promise | Object representing future value; pending → fulfilled/rejected |
| pending | Promise initial state; async work still running |
| fulfilled | resolve() called; .then() runs |
| rejected | reject() called; .catch() runs |
| settled | Promise is done (fulfilled OR rejected — permanent) |
| `.then()` | Success handler; returns NEW Promise (enables chaining) |
| `.catch()` | Error handler; catches rejections from chain |
| `.finally()` | Always runs after settled; cleanup tasks |
| `Promise.all` | Wait for ALL to succeed; fail-fast on any rejection |
| `Promise.allSettled` | Wait for ALL to finish; never rejects; get all statuses |
| `Promise.race` | First to settle (win or lose) wins |
| `Promise.any` | First to SUCCEED wins; AggregateError if all fail |
| `async` function | Always returns a Promise; enables await inside |
| `await` | Pauses async fn; resumes as microtask when Promise resolves |
| `try/catch` | Error handling for async/await |
| Promisification | Wrapping callback function in a Promise |
| Sequential await | `a = await f1(); b = await f2()` — one after another |
| Parallel await | `[a,b] = await Promise.all([f1(), f2()])` — simultaneous |
| `setTimeout` | Run fn once after delay ms (macrotask) |
| `setInterval` | Run fn repeatedly every delay ms (macrotask) |
| `clearTimeout` | Cancel a setTimeout |
| `clearInterval` | Cancel a setInterval |
| `queueMicrotask` | Manually schedule a microtask |
| `fetch` | Modern browser API for HTTP requests; returns Promise |
| `AbortController` | Cancel in-flight fetch requests |
| Unhandled rejection | Promise reject with no .catch() → crash/warning in production |

---

> 💡 **The Golden Rule of Async JavaScript:**
> Microtasks ALWAYS run before Macrotasks. Promises ALWAYS beat setTimeout — even `setTimeout(fn, 0)`.
> Use `Promise.all()` for parallel independent tasks. Always handle errors. Never `await` in `forEach`.

---
*Notes prepared with research from: MDN Web Docs, javascript.info, Medium (Feb 2026), DEV Community (Feb 2026), Peerlist (2025), TutorialReference.com (2026), javascript.plainenglish.io (Dec 2025)*
