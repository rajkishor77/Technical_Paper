
-----

# Asynchronous JavaScript Fundamentals

## 1\. How JS executes the code?

JavaScript is **single-threaded**, meaning it has only **one Call Stack** (like one chef 🧑‍🍳) to execute code. It runs tasks **synchronously**, one after the other. If a task takes a long time (e.g., fetching data from a server), the entire program would stop or "block" until that task is done. To prevent this blocking, JS uses external environments (like **Web Browser APIs**) to handle long tasks, coordinated by the **Event Loop**.

## 2\. What is diff between Sync & Async?

| Feature | **Synchronous (Sync)** | **Asynchronous (Async)** |
| :--- | :--- | :--- |
| **Pace** | **Blocking**. Tasks execute sequentially (one after the other). | **Non-blocking**. Tasks start, but execution continues immediately to the next line. The result comes back later. |
| **Analogy** | Waiting in line at a bank teller. | Placing a food order and receiving a text when it's ready. |
| **JS Example**| Math calculations, array loops. | Timers (`setTimeout`), Network Requests (`fetch`). |

## 3\. What are the ways to make the code Async?

There are three main patterns for handling asynchronous operations:

1.  **Callbacks**: Functions passed as arguments to be executed when an operation finishes. (The oldest method).
2.  **Promises**: Objects representing a future value or error. (The modern standard).
3.  **`async/await`**: A syntax layer on top of Promises that makes async code look sequential and easy to read.

## 4\. What are Web Browser APIs?

These are not part of the core JavaScript language but are **external environments** provided by the browser (or Node.js). They are responsible for executing time-consuming tasks so the single JavaScript thread isn't blocked.

  * The JS engine sends tasks like timers or network calls to these APIs.
  * Once the task is complete, the API places a function (a callback) back into a queue for the Event Loop to pick up.
  * *Examples*: `setTimeout()`, `fetch()`, and interacting with the Document Object Model (DOM).

## 5\. What is event loop?

The **Event Loop** is the core scheduler that manages how asynchronous tasks are handled.

1.  It constantly checks if the **Call Stack** (where sync code runs) is **empty**.
2.  If the Call Stack is empty, it takes the first completed task from the **Callback Queue** (or Microtask Queue, for Promises) and pushes it onto the Call Stack for execution.
    The Event Loop ensures that long-running tasks don't stop the main program from responding.

-----

# Learning Promises

## 6\. What is a callback hell?

**Callback Hell** (or the "Pyramid of Doom") is a situation where multiple asynchronous operations are nested deeply using callbacks.

  * The code becomes difficult to read, debug, and maintain because of the extreme horizontal indentation.
  * It looks like a messy, ever-widening pyramid of functions.

## 7\. What is inversion of control in callbacks?

**Inversion of Control (IoC)** means you lose control over *when* or *how* your callback function is executed. You hand your function off to another library or API and simply trust that it will call your function correctly. Promises fix this by giving you a rigid, standardized, and controllable interface (`.then()`, `.catch()`).

-----

## Promises

### 8\. What is a Promise?

A **Promise** is a special object that acts as a placeholder for a result that is not yet available, representing the eventual completion (or failure) of an asynchronous operation. It's a guarantee that you will get a value or an error sometime in the future.

### 9\. How to create a new promise?

You create a Promise using the `Promise` constructor, which takes an **executor function** that immediately runs. This executor function gives you two arguments:

  * **`resolve`**: A function you call when the async operation **succeeds**.
  * **`reject`**: A function you call when the async operation **fails**.

<!-- end list -->

```javascript
const examplePromise = new Promise((resolve, reject) => {
  // Simulate fetching data after 1 second
  setTimeout(() => {
    const success = true; 
    if (success) {
      resolve("Data received!"); // Call resolve on success
    } else {
      reject("Error: Connection timed out."); // Call reject on failure
    }
  }, 1000);
});
```

### 10\. What are different states of a Promise - pending, fulfilled, rejected

A Promise is always in one of these three states:

1.  **Pending**: The initial state. The async operation is still running.
2.  **Fulfilled (or Resolved)**: The operation completed successfully and has a resulting value.
3.  **Rejected**: The operation failed and has a reason (error object).

A Promise is **settled** once it is either fulfilled or rejected. Its state cannot change after it has been settled.

### 11\. How to consume an existing promise?

You consume a Promise using the `.then()` method for success and the `.catch()` method for failure.

```javascript
examplePromise
  .then(value => {
    console.log("Success:", value); // Runs on Fulfillment
  })
  .catch(reason => {
    console.error("Failure:", reason); // Runs on Rejection
  });
```

-----

# Promise Chaining

## 12\. How to chain promises using `.then`

Promise chaining is key to readable async code. You link multiple `.then()` methods together.

  * When a `.then()` handler returns a simple value (e.g., a number or string), that value is passed as input to the next `.then()`.
  * When a `.then()` handler returns a **new Promise**, the chain pauses and waits for that new Promise to settle before continuing to the next `.then()`.

## 13\. How to handle errors in a promise chain using `.catch`

The `.catch(onRejected)` method is dedicated to handling rejections. It functions like a centralized error handler for **any rejection** that occurs at any point in the chain **before it**.

## 14\. `finally` block in a promise chain

The `.finally(onSettled)` method executes a function when the Promise is **settled** (meaning it was either fulfilled *or* rejected). It is ideal for **cleanup tasks** (e.g., hiding a loading screen) because it runs regardless of the outcome.

## 15\. What happens when an Error gets thrown inside `.then` when there is a `.catch`

If an error is thrown inside a `.then()` handler, the chain **immediately stops** executing the subsequent `.then()` blocks and **jumps directly** to the next available `.catch()` block.

## 16\. What happens when an Error gets thrown inside `.then` when there is no `.catch`

If an error is thrown and there is **no `.catch()`** handler in the chain, the error becomes an **unhandled rejection**. This is a serious issue that usually results in a runtime error or a severe warning, potentially crashing the application or leaving it in a broken state.

## 17\. Why must `.catch` be placed towards the end of the promise chain?

Placing the `.catch()` handler at the end of the chain creates a **single, global safety net** for the entire asynchronous workflow. This ensures that *any* error from *any* step—the initial promise or any subsequent transformation—is caught and handled consistently.

## 18\. How to consume multiple promises by chaining?

You consume multiple promises sequentially by starting the next promise within the current `.then()`. This is mainly done when the second promise **depends** on the result of the first.

## 19\. How to consume multiple promises by `Promise.all`?

`Promise.all(iterable)` is used when you need to wait for **multiple independent tasks** to all succeed.

  * **Success**: It fulfills only when **all** promises in the input array fulfill. It returns an array of their results.
  * **Failure**: It **rejects immediately** with the error of the first promise that fails.

## 20\. How to do error handling when using promises?

Use the **`.catch()`** method. Place it at the end of a chain or immediately after collective methods like `Promise.all()` to catch all potential rejections.

## 21\. Why is error handling the most important part of using a promise?

Error handling is critical for **application stability** and **user experience**. It ensures that:

1.  Your application doesn't stop due to **unhandled rejections**.
2.  Users receive meaningful feedback (e.g., "Login Failed") instead of the application silently freezing.
3.  The program can execute **fallback logic** to recover gracefully.

## 22\. How to promisify an asynchronous callbacks based function - eg. `setTimeout`, `fs.readFile`

To **promisify** a function is to wrap a legacy function that uses callbacks (often following the `(error, data)` pattern) inside a new Promise. This gives the old function the modern, chainable `.then/.catch` interface.

*Example (Promisifying `setTimeout`):*

```javascript
function delay(ms) {
  // We resolve the promise when setTimeout's timer is done
  return new Promise(resolve => {
    setTimeout(resolve, ms); 
  });
}
// Now it's promise-based and chainable:
delay(1000).then(() => console.log("Timer finished."));
```

-----

# Promise Based Utility Functions

## 23\. How to use the following promise based functions

  * **`Promise.resolve(value)`**: Returns a Promise that is **already fulfilled** with the given `value`.

      * *Use*: To quickly turn a synchronous value into a Promise to start a chain.

  * **`Promise.reject(reason)`**: Returns a Promise that is **already rejected** with the given `reason` (Error object).

      * *Use*: To quickly return a Promise guaranteed to fail.

  * **`Promise.all(iterable)`**: **Waits for ALL to succeed.** Rejects if *any* promise rejects.

  * **`Promise.allSettled(iterable)`**: **Waits for ALL to finish.** Resolves only when *all* promises (success or failure) are settled. Returns an array detailing the outcome of each one. Use this when you need results from all, even the failures.

  * **`Promise.any(iterable)`**: **Waits for the FIRST one to succeed.** Resolves as soon as *any* promise fulfills. Rejects only if *all* of the promises reject. Use this for fetching from the fastest source.

  * **`Promise.race(iterable)`**: **Waits for the VERY FIRST to settle.** Settles (fulfills or rejects) as soon as *any* single promise finishes. Use this for implementing a timeout or seeing which resource responds first.