# 🟨 JavaScript — Complete Study Notes
> **After reading this, no concept of JavaScript will feel unfamiliar.**
> Last Updated: May 2026 | Interview-ready & Production-backed

---

## 📌 Table of Contents
1. [What is JavaScript?](#1-what-is-javascript)
2. [Data Types](#2-data-types)
3. [Variable Declarations — var, let, const](#3-variable-declarations--var-let-const)
4. [Truthy and Falsy Values](#4-truthy-and-falsy-values)
5. [Operators](#5-operators)
6. [Functions — All Ways to Declare](#6-functions--all-ways-to-declare)
7. [Scope & Hoisting](#7-scope--hoisting)
8. [Closures](#8-closures)
9. [Arrow Functions vs Regular Functions](#9-arrow-functions-vs-regular-functions)
10. [this Keyword](#10-this-keyword)
11. [Pass by Value vs Pass by Reference](#11-pass-by-value-vs-pass-by-reference)
12. [Arrays — All Methods](#12-arrays--all-methods)
13. [Objects — All Methods](#13-objects--all-methods)
14. [Strings — All Methods](#14-strings--all-methods)
15. [Loops — All Types](#15-loops--all-types)
16. [Destructuring](#16-destructuring)
17. [Spread & Rest Operators](#17-spread--rest-operators)
18. [Template Literals](#18-template-literals)
19. [Default Parameters](#19-default-parameters)
20. [Error Handling](#20-error-handling)
21. [Promises & Async/Await](#21-promises--asyncawait)
22. [Higher-Order Functions](#22-higher-order-functions)
23. [Modules — CommonJS & ES Modules](#23-modules--commonjs--es-modules)
24. [Classes & Prototypes](#24-classes--prototypes)
25. [Nullish Coalescing & Optional Chaining](#25-nullish-coalescing--optional-chaining)
26. [Event Loop & Asynchronous JavaScript](#26-event-loop--asynchronous-javascript)
27. [console Methods](#27-console-methods)
28. [Best Practices & Style Guide](#28-best-practices--style-guide)
29. [Common Interview Questions](#29-common-interview-questions)
30. [Map & Set](#30-map--set)
31. [WeakMap & WeakRef](#31-weakmap--weakref)
32. [Iterators & Generators](#32-iterators--generators)
33. [Symbol — Deep Dive](#33-symbol--deep-dive)
34. [Proxy & Reflect](#34-proxy--reflect)
35. [Regular Expressions (RegEx)](#35-regular-expressions-regex)
36. [JSON — Parse, Stringify & Edge Cases](#36-json--parse-stringify--edge-cases)
37. [Date & Time](#37-date--time)
38. [Math Object — All Methods](#38-math-object--all-methods)
39. [Number Methods — All](#39-number-methods--all)
40. [DOM Manipulation (Browser)](#40-dom-manipulation-browser)
41. [Events (Browser)](#41-events-browser)
42. [Fetch API & HTTP](#42-fetch-api--http)
43. [LocalStorage, SessionStorage & Cookies](#43-localstorage-sessionstorage--cookies)
44. [Design Patterns in JavaScript](#44-design-patterns-in-javascript)
45. [Performance Tips](#45-performance-tips)
46. [ES2020–ES2024 New Features](#46-es2020es2024-new-features)
47. [Garbage Collection & Memory Management](#47-garbage-collection--memory-management)
48. [Strict Mode](#48-strict-mode)
49. [Short-Circuit Evaluation & Logical Assignment](#49-short-circuit-evaluation--logical-assignment)
50. [Callbacks vs Promises vs Async/Await — Full Comparison](#50-callbacks-vs-promises-vs-asyncawait--full-comparison)
51. [Debounce & Throttle](#51-debounce--throttle)
52. [Deep vs Shallow Copy](#52-deep-vs-shallow-copy)
53. [Functional Programming Concepts](#53-functional-programming-concepts)
54. [Type Conversion Cheat Sheet](#54-type-conversion-cheat-sheet)
55. [Common JavaScript Mistakes & How to Avoid Them](#55-common-javascript-mistakes--how-to-avoid-them)
56. [Node.js Specific Concepts](#56-nodejs-specific-concepts)
57. [Updated Interview Questions — Advanced](#57-updated-interview-questions--advanced)

---

## 1. What is JavaScript?

JavaScript is a **high-level, single-threaded, dynamically-typed, interpreted programming language** primarily used to make web pages interactive. It runs in the browser and on the server (via Node.js).

### Simple Analogy 🏠
> Think of building a website as constructing a house:
> - **HTML** = The structure — walls, floors, rooms (the skeleton)
> - **CSS** = The interior design — paint, furniture, decoration (how it looks)
> - **JavaScript** = The electricity and plumbing — makes things actually work (lights turn on, taps flow, doors open automatically)

### Key Points
- Created by **Brendan Eich** in just **10 days** in 1995 at Netscape
- Originally named **Mocha**, then **LiveScript**, then **JavaScript**
- Standardized as **ECMAScript (ES)** — current version: **ES2024**
- Runs in the **browser** (client-side) AND on the **server** (Node.js)
- **Dynamically typed** — variable types are determined at runtime, not declared
- **Single-threaded** — executes one thing at a time, but uses async patterns to handle concurrency
- The only language that natively runs in every web browser in the world

### JavaScript Engine
- **V8** — Chrome, Node.js, Edge
- **SpiderMonkey** — Firefox
- **JavaScriptCore** — Safari

---

## 2. Data Types

JavaScript has **8 data types** — 7 primitives and 1 non-primitive (object).

### 2.1 Primitive Types (Immutable — stored by value)

```javascript
// 1. string — any text, in single, double, or backtick quotes
let name = "Alice"
let greeting = 'Hello'
let message = `Hi ${name}`         // template literal

// 2. number — integers AND decimals (no separate int/float)
let age = 25
let price = 9.99
let negative = -10
let big = 1_000_000                // underscores for readability (ES2021)
console.log(Number.MAX_SAFE_INTEGER)  // 9007199254740991

// Special number values:
console.log(0 / 0)                 // NaN  (Not a Number)
console.log(1 / 0)                 // Infinity
console.log(typeof NaN)            // "number" (confusing but true!)
console.log(isNaN(NaN))            // true
console.log(isFinite(Infinity))    // false

// 3. bigint — for numbers larger than Number.MAX_SAFE_INTEGER
let huge = 9007199254740991n       // add 'n' at the end
let result = huge + 1n             // must operate with other bigints

// 4. boolean — only two values
let isLoggedIn = true
let isAdmin = false

// 5. undefined — declared but not assigned
let x
console.log(x)                     // undefined
console.log(typeof x)              // "undefined"

// 6. null — intentionally set to "no value"
let selectedUser = null
console.log(typeof null)           // "object" — famous JS bug, it's actually null

// 7. symbol — unique, immutable identifier
const id1 = Symbol("id")
const id2 = Symbol("id")
console.log(id1 === id2)           // false — every Symbol is unique
```

### 2.2 Non-Primitive Type (Object — stored by reference)

```javascript
// Plain Object — key-value pairs
const person = { name: "Alice", age: 28, isActive: true }

// Array — ordered list (special object)
const colors = ["red", "green", "blue"]
console.log(typeof colors)         // "object"
console.log(Array.isArray(colors)) // true — correct way to check for array

// Function — also an object in JavaScript!
function greet() { return "hello" }
console.log(typeof greet)          // "function"
```

### Type Checking

```javascript
typeof "hello"           // "string"
typeof 42                // "number"
typeof true              // "boolean"
typeof undefined         // "undefined"
typeof null              // "object" ← known bug in JS
typeof {}                // "object"
typeof []                // "object"
typeof function(){}      // "function"
typeof Symbol()          // "symbol"
typeof 42n               // "bigint"

// Better checks:
Array.isArray([])                        // true — for arrays
value === null                           // true — for null specifically
value instanceof Date                   // true — for class instances
Object.prototype.toString.call(value)   // "[object Array]", "[object Date]" etc.
```

### Type Coercion (Automatic Type Conversion)

```javascript
// Implicit coercion (JavaScript does it automatically)
"5" + 3           // "53"  — number converted to string (+ prefers string)
"5" - 3           // 2     — string converted to number (- forces number)
"5" * "2"         // 10    — both converted to number
true + 1          // 2     — true = 1
false + 1         // 1     — false = 0
null + 1          // 1     — null = 0
undefined + 1     // NaN   — undefined can't convert

// Explicit coercion (you do it on purpose)
Number("42")      // 42
Number("")        // 0
Number("abc")     // NaN
Number(true)      // 1
Number(false)     // 0
Number(null)      // 0
Number(undefined) // NaN

String(42)        // "42"
String(true)      // "true"
String(null)      // "null"

Boolean(0)        // false
Boolean("")       // false
Boolean(null)     // false
Boolean(undefined)// false
Boolean("hello")  // true
Boolean([])       // true  ← empty array is TRUTHY!
Boolean({})       // true  ← empty object is TRUTHY!
```

---

## 3. Variable Declarations — var, let, const

### Side-by-Side Comparison

| Feature | `var` | `let` | `const` |
|---------|-------|-------|---------|
| **Scope** | Function or Global | Block `{}` | Block `{}` |
| **Hoisting** | Yes (initialized as `undefined`) | Yes (but in TDZ — unusable) | Yes (but in TDZ — unusable) |
| **Re-declare** | ✅ Yes | ❌ No | ❌ No |
| **Re-assign** | ✅ Yes | ✅ Yes | ❌ No |
| **Use today** | ❌ Never | ✅ When value changes | ✅ Default choice |

```javascript
// ─── const — use by default ─────────────────────────────────────────────
const PI = 3.14159
const API_KEY = "abc123"
// PI = 3  ← TypeError: Assignment to constant variable

// IMPORTANT: const with objects/arrays — the variable can't be re-assigned
// but the CONTENTS of the object/array CAN change
const user = { name: "Alice", age: 28 }
user.age = 29           // ✅ this works — mutating the object
// user = {}            // ❌ this fails — re-assigning the variable

const colors = ["red", "green"]
colors.push("blue")     // ✅ works — mutating the array
// colors = []          // ❌ fails — re-assigning the variable


// ─── let — when value will change ───────────────────────────────────────
let score = 0
score = 10              // ✅ works
score++                 // ✅ works

let total               // declared without value — starts as undefined
total = 100


// ─── var — what NOT to do (and why it fails) ─────────────────────────────
var x = 1
if (true) {
  var x = 2             // same variable! overwrites x everywhere
}
console.log(x)          // 2 ← unexpected!

// With let:
let y = 1
if (true) {
  let y = 2             // different y — only lives inside this block
}
console.log(y)          // 1 ← correct!

// var hoisting surprise:
console.log(name)       // undefined (no error!) — var hoisted but not initialized
var name = "Alice"

// var in loops leaks out:
for (var i = 0; i < 3; i++) { /* ... */ }
console.log(i)          // 3 ← i leaked outside the loop!

for (let j = 0; j < 3; j++) { /* ... */ }
// console.log(j)       // ReferenceError — j is block-scoped to the loop
```

---

## 4. Truthy and Falsy Values

Every value in JavaScript is either truthy or falsy when used in a boolean context (like an `if` statement).

### The Only 6 Falsy Values — Memorize These!

```javascript
if (false)      { } // falsy
if (0)          { } // falsy
if (-0)         { } // falsy
if (0n)         { } // falsy (bigint zero)
if ("")         { } // falsy (empty string)
if ('')         { } // falsy (empty string)
if (``)         { } // falsy (empty template literal)
if (null)       { } // falsy
if (undefined)  { } // falsy
if (NaN)        { } // falsy

// EVERYTHING ELSE is TRUTHY — including these surprises:
if ("0")        { } // TRUTHY — non-empty string
if (" ")        { } // TRUTHY — string with a space
if ([])         { } // TRUTHY — empty array!
if ({})         { } // TRUTHY — empty object!
if (-1)         { } // TRUTHY — any non-zero number
if (function(){}) { } // TRUTHY — any function
```

```javascript
// Practical use — common patterns
const name = userInput || "Guest"       // use "Guest" if userInput is falsy
const count = value ?? 0                // use 0 only if value is null/undefined (not 0 or "")

// Checking if a variable has a real value
if (username) { }                       // true if not null/undefined/""
if (username !== undefined && username !== null) { }  // more explicit

// Why !value is risky:
function process(amount) {
  if (!amount) throw new Error("Amount required")  // WRONG — throws for amount = 0!
  if (amount === undefined) throw new Error("Amount required")  // CORRECT
}
```

---

## 5. Operators

```javascript
// ─── Arithmetic ────────────────────────────────────────────────────────
5 + 3     // 8
5 - 3     // 2
5 * 3     // 15
10 / 3    // 3.333...
10 % 3    // 1  (remainder)
2 ** 3    // 8  (exponentiation — 2 to the power of 3)
10 / 3 | 0  // 3  (quick integer division trick)

// ─── Assignment ────────────────────────────────────────────────────────
let x = 10
x += 5     // x = x + 5  → 15
x -= 3     // x = x - 3  → 12
x *= 2     // x = x * 2  → 24
x /= 4     // x = x / 4  → 6
x **= 2    // x = x ** 2 → 36
x++        // x = x + 1  (post-increment — uses x then increments)
++x        // x = x + 1  (pre-increment — increments then uses x)
x--        // x = x - 1  (post-decrement)
--x        // x = x - 1  (pre-decrement)


// ─── Comparison ────────────────────────────────────────────────────────
5 == "5"    // true  ← loose equality (type coercion) — AVOID
5 === "5"   // false ← strict equality (no coercion) — ALWAYS USE THIS
5 != "5"    // false ← loose inequality
5 !== "5"   // true  ← strict inequality — ALWAYS USE THIS
5 > 3       // true
5 < 3       // false
5 >= 5      // true
5 <= 4      // false


// ─── Logical ───────────────────────────────────────────────────────────
true && true     // true  (AND — both must be true)
true && false    // false
false || true    // true  (OR — at least one must be true)
false || false   // false
!true            // false (NOT — flips boolean)
!false           // true

// Short-circuit evaluation:
const name = null
const display = name || "Anonymous"   // "Anonymous" — name is falsy, use right side
const user = { name: "Alice" }
const label = user && user.name       // "Alice" — user is truthy, evaluate right side

// Nullish coalescing (??) — only triggers on null/undefined (not 0 or "")
const port = userPort ?? 3000         // 3000 only if userPort is null or undefined
const val = 0 ?? 42                   // 0  ← 0 is NOT null/undefined, keep it
const val2 = 0 || 42                  // 42 ← 0 is falsy, use right side


// ─── Ternary (Conditional) Operator ────────────────────────────────────
const age = 20
const status = age >= 18 ? "Adult" : "Minor"   // "Adult"

// Nested ternary — readable limit: max 2 levels
const grade = score >= 90 ? "A" : score >= 70 ? "B" : "C"


// ─── typeof and instanceof ─────────────────────────────────────────────
typeof "hello"         // "string"
typeof 42              // "number"
typeof null            // "object"  ← the famous bug
[] instanceof Array    // true
{} instanceof Object   // true
new Date() instanceof Date  // true
```

---

## 6. Functions — All Ways to Declare

```javascript
// ─── 1. Function Declaration ─────────────────────────────────────────────
// Hoisted completely — can call BEFORE the function is defined
function add(a, b) {
  return a + b
}
add(2, 3)  // 5

// If no return statement, function returns undefined
function greet(name) {
  console.log(`Hello, ${name}`)
}
const result = greet("Alice")  // logs "Hello, Alice", result = undefined


// ─── 2. Function Expression ──────────────────────────────────────────────
// NOT hoisted — must be defined before calling
const multiply = function(a, b) {
  return a * b
}
multiply(3, 4)  // 12

// Named function expression (useful for recursion + stack traces)
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1)
}


// ─── 3. Arrow Function ───────────────────────────────────────────────────
// Concise syntax — NO own 'this', 'arguments', or 'prototype'
const square = (n) => n * n             // single param: parentheses optional
const square2 = n => n * n             // same thing
const double = n => n * 2

const addTwo = (a, b) => a + b         // multiple params: parentheses required
const nothing = () => undefined        // no params: empty parentheses required

// Multi-line arrow function — needs curly braces AND explicit return
const divide = (a, b) => {
  if (b === 0) throw new Error("Cannot divide by zero")
  return a / b
}

// Returning an object literal — wrap in parentheses to avoid confusion with block
const makeUser = (name, age) => ({ name, age })
makeUser("Alice", 28)  // { name: "Alice", age: 28 }


// ─── 4. Immediately Invoked Function Expression (IIFE) ───────────────────
// Runs immediately — used to create a private scope
(function() {
  const privateVar = "I am private"
  console.log(privateVar)
})()

// Arrow function IIFE
(() => {
  console.log("Arrow IIFE")
})()


// ─── 5. Generator Function ───────────────────────────────────────────────
function* idGenerator() {
  let id = 1
  while (true) {
    yield id++         // pauses here, returns id, resumes on .next()
  }
}
const gen = idGenerator()
gen.next().value   // 1
gen.next().value   // 2
gen.next().value   // 3


// ─── Function Parameters ─────────────────────────────────────────────────
// Default parameters
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`
}
greet()                  // "Hello, Guest!"
greet("Alice")           // "Hello, Alice!"
greet("Bob", "Hi")       // "Hi, Bob!"

// Rest parameters — collects remaining arguments into an array
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0)
}
sum(1, 2, 3, 4, 5)       // 15

// Combining normal + rest params
function log(level, ...messages) {
  console.log(`[${level}]`, ...messages)
}
log("INFO", "Server started", "Port 3000")
```

---

## 7. Scope & Hoisting

### Scope

```javascript
// ─── Global Scope ────────────────────────────────────────────────────────
const globalVar = "I am global"         // accessible everywhere

// ─── Function Scope ──────────────────────────────────────────────────────
function myFunc() {
  const local = "I am local"           // only inside myFunc
  console.log(globalVar)               // can access global
}
// console.log(local)                  // ReferenceError — can't access outside

// ─── Block Scope (let and const only) ────────────────────────────────────
{
  let blockVar = "block"
  const blockConst = "also block"
  console.log(blockVar)               // works
}
// console.log(blockVar)              // ReferenceError

// ─── Lexical (Static) Scope ──────────────────────────────────────────────
// Functions can access variables from the scope they were DEFINED in (not where called)
const outerVar = "outer"

function outer() {
  const outerFunc = "outerFunc"
  function inner() {
    console.log(outerVar)    // ✅ — accesses outer scope
    console.log(outerFunc)   // ✅ — accesses enclosing function's scope
  }
  inner()
}
```

### Hoisting

```javascript
// ─── var hoisting — moved to top, initialized as undefined ──────────────
console.log(x)   // undefined (no error!)
var x = 5
console.log(x)   // 5
// JavaScript sees this as:
// var x = undefined  ← hoisted
// console.log(x)     → undefined
// x = 5
// console.log(x)     → 5

// ─── let/const hoisting — hoisted but NOT initialized (TDZ) ─────────────
// console.log(y)   // ReferenceError: Cannot access 'y' before initialization
let y = 10
// The period between the start of the block and the declaration is called
// the Temporal Dead Zone (TDZ) — accessing the variable here throws

// ─── Function Declaration — fully hoisted ────────────────────────────────
sayHello()                // "Hello!" — works before declaration!
function sayHello() { console.log("Hello!") }

// ─── Function Expression — NOT hoisted (only the variable is) ────────────
// greet()                // TypeError: greet is not a function
const greet = function() { console.log("Hi") }
```

---

## 8. Closures

A closure is a function that **remembers and accesses variables from its outer scope** even after the outer function has finished executing.

```javascript
// Basic closure
function createCounter() {
  let count = 0                       // this variable is "closed over"
  return function() {
    count++
    return count
  }
}
const counter = createCounter()
counter()   // 1
counter()   // 2
counter()   // 3
// count is private — can't be accessed from outside!


// Closure with parameters — factory function
function createMultiplier(multiplier) {
  return (number) => number * multiplier
}
const double = createMultiplier(2)
const triple = createMultiplier(3)
double(5)   // 10
triple(5)   // 15


// Closure for data privacy (module pattern)
function createBankAccount(initialBalance) {
  let balance = initialBalance            // private — not accessible outside

  return {
    deposit: (amount) => { balance += amount },
    withdraw: (amount) => {
      if (amount > balance) throw new Error("Insufficient funds")
      balance -= amount
    },
    getBalance: () => balance
  }
}
const account = createBankAccount(1000)
account.deposit(500)
account.withdraw(200)
account.getBalance()    // 1300
// account.balance      // undefined — balance is private!


// Classic closure bug with var in loops (and the fix)
// ❌ Problem with var:
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100)  // prints 3, 3, 3 — not 0, 1, 2!
}

// ✅ Fix 1: Use let (block-scoped, new binding per iteration)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100)  // prints 0, 1, 2 ✅
}

// ✅ Fix 2: Use IIFE to capture the current value of i
for (var i = 0; i < 3; i++) {
  ((j) => setTimeout(() => console.log(j), 100))(i)  // prints 0, 1, 2 ✅
}
```

---

## 9. Arrow Functions vs Regular Functions

```javascript
// ─── The key difference: `this` binding ──────────────────────────────────

// Regular function — 'this' depends on HOW the function is called
const timer = {
  seconds: 0,
  start: function() {
    setInterval(function() {
      this.seconds++              // ❌ 'this' is undefined (or window in browser)
      console.log(this.seconds)  // NaN
    }, 1000)
  }
}

// Arrow function — 'this' is INHERITED from the surrounding scope (lexical this)
const timer2 = {
  seconds: 0,
  start: function() {
    setInterval(() => {
      this.seconds++             // ✅ 'this' is the timer2 object
      console.log(this.seconds)  // 1, 2, 3...
    }, 1000)
  }
}

// ─── Full Comparison ──────────────────────────────────────────────────────
// Feature          | Regular Function        | Arrow Function
// this             | Dynamic (call-site)     | Lexical (definition-site)
// arguments object | ✅ Has own 'arguments'  | ❌ No 'arguments' object
// new keyword      | ✅ Can be a constructor | ❌ Cannot use new
// prototype        | ✅ Has .prototype       | ❌ No .prototype
// yield            | ✅ Can be a generator   | ❌ Cannot be a generator
// Object method    | ✅ Preferred            | ❌ Avoid (wrong 'this')
// Callback         | ✅ Works                | ✅ Preferred (shorter)

// ─── Where to use each ────────────────────────────────────────────────────
// Use regular function for:
const obj = {
  name: "Alice",
  greet: function() {            // ✅ object methods — needs own 'this'
    return `Hi, I'm ${this.name}`
  }
}

// Use arrow function for:
const numbers = [1, 2, 3]
const doubled = numbers.map(n => n * 2)   // ✅ callbacks and HOFs
```

---

## 10. this Keyword

```javascript
// 1. Global context — 'this' is the global object (window in browser, global in Node)
console.log(this)                   // window (browser) / {} (Node module)

// 2. Regular function — 'this' depends on caller
function show() { console.log(this) }
show()                              // undefined (strict mode) / window (sloppy)

// 3. Object method — 'this' is the object before the dot
const user = {
  name: "Alice",
  greet() { return `Hello, I'm ${this.name}` }
}
user.greet()                        // "Hello, I'm Alice"

// 4. Explicit binding — call, apply, bind
function introduce(city, country) {
  return `${this.name} from ${city}, ${country}`
}
const person = { name: "Alice" }

introduce.call(person, "Mumbai", "India")         // immediate call, args listed
introduce.apply(person, ["Mumbai", "India"])      // immediate call, args as array
const boundFn = introduce.bind(person, "Mumbai")  // creates new function, call later
boundFn("India")

// 5. Constructor function — 'this' is the newly created object
function Person(name, age) {
  this.name = name
  this.age = age
}
const alice = new Person("Alice", 28)

// 6. Class — 'this' is the instance
class Car {
  constructor(brand) { this.brand = brand }
  info() { return `This is a ${this.brand}` }
}
const car = new Car("Toyota")
car.info()   // "This is a Toyota"
```

---

## 11. Pass by Value vs Pass by Reference

```javascript
// ─── Primitives — passed by VALUE (copy of the value) ────────────────────
let a = 10
let b = a          // b gets a COPY of 10
b = 20             // only b changes
console.log(a)     // 10 ← a is unchanged

function changeNumber(num) {
  num = 999        // only the local copy changes
}
let score = 5
changeNumber(score)
console.log(score) // 5 ← unchanged


// ─── Objects/Arrays — passed by REFERENCE (copy of the memory address) ───
const arr1 = [1, 2, 3]
const arr2 = arr1                   // arr2 holds the SAME reference
arr2.push(4)
console.log(arr1)                   // [1, 2, 3, 4] ← arr1 was also changed!

function mutateArray(arr) {
  arr.push(99)                      // mutates the ORIGINAL array
}
const items = [1, 2]
mutateArray(items)
console.log(items)                  // [1, 2, 99] ← changed!

// But re-assigning the parameter doesn't affect the original
function tryToReplace(arr) {
  arr = [10, 20]                    // only the local reference changes
}
const nums = [1, 2, 3]
tryToReplace(nums)
console.log(nums)                   // [1, 2, 3] ← unchanged


// ─── Avoiding mutation — always copy first ────────────────────────────────
// Shallow copy of an array
const original = [1, 2, 3]
const copy1 = [...original]         // spread operator
const copy2 = original.slice()      // slice with no args
const copy3 = Array.from(original)  // Array.from

// Shallow copy of an object
const user = { name: "Alice", age: 28 }
const copy = { ...user }            // spread operator
const copy2b = Object.assign({}, user)

// Deep copy (for nested objects/arrays)
const deep = JSON.parse(JSON.stringify(obj))    // simple but loses functions/dates
const deep2 = structuredClone(obj)             // modern, handles most types (ES2022)
```

---

## 12. Arrays — All Methods

```javascript
const arr = [1, 2, 3, 4, 5]

// ─── MUTATING methods (change the original array) ─────────────────────────
arr.push(6)               // add to END → [1,2,3,4,5,6] — returns new length
arr.pop()                 // remove from END → [1,2,3,4,5] — returns removed item
arr.unshift(0)            // add to START → [0,1,2,3,4,5] — returns new length
arr.shift()               // remove from START → [1,2,3,4,5] — returns removed item
arr.splice(1, 2)          // remove 2 elements starting at index 1 → [1,4,5]
arr.splice(1, 0, 10, 11)  // insert 10,11 at index 1, delete 0 → [1,10,11,2,3,4,5]
arr.splice(1, 1, 99)      // replace 1 element at index 1 with 99
arr.sort()                // sort in place (ALPHABETICAL by default — sorts [10,9,2] as [10,2,9]!)
arr.sort((a, b) => a - b) // sort numerically ASCENDING
arr.sort((a, b) => b - a) // sort numerically DESCENDING
arr.reverse()             // reverse in place
arr.fill(0)               // fill entire array with 0
arr.fill(0, 2, 4)         // fill from index 2 to 4 with 0
arr.copyWithin(0, 2)      // copy elements from index 2 to index 0


// ─── NON-MUTATING methods (return new array, original untouched) ──────────
arr.map(x => x * 2)               // transform every element → new array same length
arr.filter(x => x > 2)            // keep elements matching condition → new array (shorter)
arr.slice(1, 3)                   // extract portion → new array (index 1 inclusive, 3 exclusive)
arr.concat([6, 7])                // combine arrays → new array
arr.flat()                        // flatten one level deep: [[1,2],[3]] → [1,2,3]
arr.flat(2)                       // flatten 2 levels deep
arr.flatMap(x => [x, x * 2])     // map + flat in one step
arr.toSorted((a, b) => a - b)    // non-mutating sort (ES2023)
arr.toReversed()                  // non-mutating reverse (ES2023)
arr.toSpliced(1, 1, 99)          // non-mutating splice (ES2023)
arr.with(2, 99)                   // replace element at index 2 with 99 — new array (ES2023)


// ─── Search & Test methods ────────────────────────────────────────────────
arr.find(x => x > 3)              // FIRST element matching condition (4) or undefined
arr.findIndex(x => x > 3)        // INDEX of first match (3) or -1
arr.findLast(x => x < 4)         // LAST element matching condition (3) — ES2023
arr.findLastIndex(x => x < 4)    // index of last match — ES2023
arr.indexOf(3)                    // index of first occurrence of 3 (2) or -1
arr.lastIndexOf(3)                // index of last occurrence of 3 (2) or -1
arr.includes(3)                   // true/false — does array contain 3?
arr.some(x => x > 4)             // true if AT LEAST ONE element matches condition
arr.every(x => x > 0)            // true if ALL elements match condition


// ─── Aggregation methods ──────────────────────────────────────────────────
arr.reduce((acc, curr) => acc + curr, 0)   // accumulate to single value (sum = 15)
arr.reduce((acc, curr) => acc + curr)      // no initial value (uses first element as acc)
arr.reduceRight((acc, curr) => acc + curr, 0)  // reduce from RIGHT to LEFT
arr.join(" - ")                            // "1 - 2 - 3 - 4 - 5"
arr.join("")                               // "12345"


// ─── Iteration methods ────────────────────────────────────────────────────
arr.forEach(x => console.log(x))           // run function for each (no return value)
arr.entries()                             // iterator of [index, value] pairs
arr.keys()                                // iterator of indices
arr.values()                              // iterator of values


// ─── Creation methods ─────────────────────────────────────────────────────
Array.from("hello")                       // ["h","e","l","l","o"] — from iterable
Array.from({length: 5}, (_, i) => i)     // [0,1,2,3,4]
Array.from({length: 3}, () => "x")       // ["x","x","x"]
Array.of(1, 2, 3)                         // [1, 2, 3]
Array.isArray([])                         // true


// ─── Chaining ─────────────────────────────────────────────────────────────
const result = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .filter(n => n % 2 === 0)     // [2, 4, 6, 8, 10]
  .map(n => n ** 2)             // [4, 16, 36, 64, 100]
  .reduce((sum, n) => sum + n, 0) // 220
```

---

## 13. Objects — All Methods

```javascript
const user = { name: "Alice", age: 28, city: "Mumbai" }

// ─── Object static methods ────────────────────────────────────────────────
Object.keys(user)          // ["name", "age", "city"]
Object.values(user)        // ["Alice", 28, "Mumbai"]
Object.entries(user)       // [["name","Alice"], ["age",28], ["city","Mumbai"]]

// Iterate over entries
Object.entries(user).forEach(([key, value]) => {
  console.log(`${key}: ${value}`)
})

Object.assign({}, user)             // shallow copy
Object.assign(target, source)       // copy properties from source into target (mutates target)
Object.assign({}, obj1, obj2)       // merge obj1 and obj2 into new object

Object.freeze(user)                 // prevent any modifications — deep read-only
Object.isFrozen(user)               // true/false

Object.seal(user)                   // can update existing properties but can't add/delete
Object.isSealed(user)               // true/false

Object.fromEntries([["a", 1], ["b", 2]])  // { a: 1, b: 2 } — inverse of entries()
Object.fromEntries(new Map([["a", 1]]))   // works with Map too

Object.hasOwn(user, "name")         // true — modern replacement for hasOwnProperty
user.hasOwnProperty("name")         // true — older way

Object.getOwnPropertyNames(user)    // all own properties (including non-enumerable)
Object.defineProperty(obj, "id", { value: 1, writable: false, enumerable: false })
Object.getPrototypeOf(user)         // get prototype of an object

// ─── Spread operator — most common way to work with objects ──────────────
const updated = { ...user, age: 29 }         // update a property (immutable)
const extended = { ...user, role: "admin" }  // add a new property
const { city, ...rest } = user              // remove a property via destructuring

// ─── Property shorthand ───────────────────────────────────────────────────
const name = "Alice"
const age = 28
const person = { name, age }       // { name: "Alice", age: 28 }
// Same as: { name: name, age: age }

// ─── Computed property names ──────────────────────────────────────────────
const key = "dynamicKey"
const obj = { [key]: "value", [`${key}_2`]: "value2" }
// { dynamicKey: "value", dynamicKey_2: "value2" }

// ─── Optional chaining ────────────────────────────────────────────────────
const user2 = { address: { city: "Mumbai" } }
user2?.address?.city            // "Mumbai" — safe navigation
user2?.phone?.number            // undefined — no error if phone doesn't exist
user2?.greet?.()                // call method only if it exists
arr?.[0]                        // safe array access
```

---

## 14. Strings — All Methods

```javascript
const str = "  Hello, JavaScript World!  "

// ─── Whitespace ───────────────────────────────────────────────────────────
str.trim()                          // "Hello, JavaScript World!"
str.trimStart()                     // "Hello, JavaScript World!  "
str.trimEnd()                       // "  Hello, JavaScript World!"

// ─── Case ─────────────────────────────────────────────────────────────────
"hello".toUpperCase()               // "HELLO"
"HELLO".toLowerCase()               // "hello"

// ─── Search ───────────────────────────────────────────────────────────────
"hello world".includes("world")     // true
"hello world".startsWith("hello")   // true
"hello world".endsWith("world")     // true
"hello world".indexOf("o")          // 4 — first occurrence (-1 if not found)
"hello world".lastIndexOf("o")      // 7 — last occurrence
"hello world".search(/world/)       // 6 — regex search, returns index

// ─── Extract ──────────────────────────────────────────────────────────────
"hello world".slice(0, 5)           // "hello" — start inclusive, end exclusive
"hello world".slice(-5)             // "world" — negative = from end
"hello world".slice(6)              // "world" — to end
"hello world".substring(0, 5)       // "hello" — like slice but no negative indices
"hello world".charAt(0)             // "h"
"hello world"[0]                    // "h" — same as charAt

// ─── Split & Join ─────────────────────────────────────────────────────────
"a,b,c".split(",")                  // ["a", "b", "c"]
"hello".split("")                   // ["h","e","l","l","o"]
"one two three".split(" ")          // ["one", "two", "three"]
"hello".split("", 3)                // ["h", "e", "l"] — limit to 3 items

// ─── Replace ──────────────────────────────────────────────────────────────
"hello world".replace("world", "JS")          // "hello JS" — replaces first only
"aabbcc".replace(/b/g, "X")                   // "aaXXcc" — regex global flag
"hello world".replaceAll("l", "L")            // "heLLo worLd" — replaces ALL
"hello".replace(/(\w+)/, (match) => match.toUpperCase())  // "HELLO" — with callback

// ─── Repeat & Pad ─────────────────────────────────────────────────────────
"ha".repeat(3)                      // "hahaha"
"5".padStart(3, "0")                // "005" — pad to total length 3
"5".padEnd(3, "0")                  // "500"
"42".padStart(6)                    // "    42" — pads with spaces by default

// ─── Template Literal ─────────────────────────────────────────────────────
const name = "Alice"
const age = 28
`Hello, ${name}! You are ${age} years old.`
`Math: ${2 + 2}`                    // "Math: 4" — expressions work!
`Multiline
string
here`                               // actual newlines preserved

// Tagged template literal (advanced)
function highlight(strings, ...values) {
  return strings.reduce((acc, str, i) => acc + str + (values[i] ? `<b>${values[i]}</b>` : ""), "")
}
highlight`Hello ${name}, you are ${age}!`  // "Hello <b>Alice</b>, you are <b>28</b>!"

// ─── Conversion ───────────────────────────────────────────────────────────
"42".parseInt                       // use Number() instead
Number("42")                        // 42
parseInt("42px")                    // 42 — parses until non-number
parseFloat("3.14abc")               // 3.14
(255).toString(16)                  // "ff" — number to hex
(255).toString(2)                   // "11111111" — number to binary
```

---

## 15. Loops — All Types

```javascript
const numbers = [10, 20, 30, 40, 50]
const person = { name: "Alice", age: 28, city: "Mumbai" }

// ─── for loop — classic, full control ─────────────────────────────────────
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i])
}

// Backwards
for (let i = numbers.length - 1; i >= 0; i--) {
  console.log(numbers[i])
}


// ─── for...of — iterate VALUES of any iterable ────────────────────────────
// Best for: arrays, strings, Maps, Sets, generators
for (const num of numbers) {
  console.log(num)           // 10, 20, 30, 40, 50
}

// With index using entries()
for (const [index, value] of numbers.entries()) {
  console.log(index, value)  // 0 10, 1 20, 2 30...
}

// Works on strings too
for (const char of "hello") {
  console.log(char)          // "h", "e", "l", "l", "o"
}


// ─── for...in — iterate KEYS of an object ─────────────────────────────────
// Best for: plain objects (NOT for arrays — use for...of instead)
for (const key in person) {
  console.log(key, person[key])   // "name Alice", "age 28", "city Mumbai"
}
// ⚠️ for...in also iterates prototype properties — use hasOwnProperty or for...of entries


// ─── forEach — array method with callback ─────────────────────────────────
// Cannot use break or continue — just runs through
numbers.forEach((num, index) => {
  console.log(index, num)
})

// ─── while loop ───────────────────────────────────────────────────────────
let count = 0
while (count < 5) {
  console.log(count)
  count++
}

// ─── do...while — runs at least once ──────────────────────────────────────
let attempts = 0
do {
  attempts++
  console.log(`Attempt ${attempts}`)
} while (attempts < 3)


// ─── Loop control statements ──────────────────────────────────────────────
// break — exit the loop entirely
for (const num of numbers) {
  if (num === 30) break        // stops when num is 30
  console.log(num)             // 10, 20
}

// continue — skip to next iteration
for (const num of numbers) {
  if (num === 30) continue     // skip 30
  console.log(num)             // 10, 20, 40, 50
}

// Labeled break/continue — break out of nested loops
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer   // breaks the OUTER loop
  }
}
```

---

## 16. Destructuring

```javascript
// ─── Object Destructuring ─────────────────────────────────────────────────
const user = { name: "Alice", age: 28, city: "Mumbai", role: "admin" }

const { name, age } = user                     // basic destructuring
const { name: userName, age: userAge } = user  // rename variables
const { name: n, role = "user" } = user        // with default value (used if undefined)
const { city, ...rest } = user                 // rest — rest = { name, age, role }

// Nested object destructuring
const profile = { name: "Alice", address: { city: "Mumbai", zip: "400001" } }
const { address: { city, zip } } = profile     // city = "Mumbai", zip = "400001"

// In function parameters
function displayUser({ name, age, role = "user" }) {
  console.log(`${name} (${age}) — ${role}`)
}
displayUser(user)


// ─── Array Destructuring ──────────────────────────────────────────────────
const colors = ["red", "green", "blue", "yellow"]

const [first, second] = colors                 // first="red", second="green"
const [, , third] = colors                     // skip first two → third="blue"
const [a, b, ...remaining] = colors            // remaining=["blue","yellow"]
const [x = "default"] = []                     // default value if undefined → x="default"

// Swap variables without temp variable
let p = 1, q = 2
;[p, q] = [q, p]                               // p=2, q=1

// From function return value
function getMinMax(arr) {
  return [Math.min(...arr), Math.max(...arr)]
}
const [min, max] = getMinMax([3, 1, 4, 1, 5, 9])  // min=1, max=9

// ─── Mixed Destructuring ──────────────────────────────────────────────────
const data = {
  status: "success",
  results: [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
  ]
}
const { status, results: [{ name: firstName }, { name: secondName }] } = data
// firstName = "Alice", secondName = "Bob"
```

---

## 17. Spread & Rest Operators

Both use `...` but in opposite directions.

```javascript
// ─── SPREAD operator — EXPANDS items ──────────────────────────────────────
// Arrays
const arr1 = [1, 2, 3]
const arr2 = [4, 5, 6]
const combined = [...arr1, ...arr2]               // [1,2,3,4,5,6]
const withExtra = [...arr1, 99, ...arr2]          // [1,2,3,99,4,5,6]
const copy = [...arr1]                             // shallow copy

// Objects
const obj1 = { a: 1, b: 2 }
const obj2 = { b: 3, c: 4 }
const merged = { ...obj1, ...obj2 }               // { a:1, b:3, c:4 } — obj2 overwrites
const updated = { ...user, age: 29 }             // update one field immutably

// In function calls
function sum(a, b, c) { return a + b + c }
const nums = [1, 2, 3]
sum(...nums)                                      // 6 — spreads array as arguments

Math.max(...[3, 1, 4, 1, 5, 9])                  // 9
Math.min(...nums)                                 // 1

// String to array
const chars = [..."hello"]                        // ["h","e","l","l","o"]
const unique = [...new Set([1,1,2,2,3])]         // [1,2,3] — remove duplicates!


// ─── REST parameter — COLLECTS items ──────────────────────────────────────
// In function parameters (must be LAST parameter)
function sum(...numbers) {                        // all arguments collected into array
  return numbers.reduce((acc, n) => acc + n, 0)
}
sum(1, 2, 3, 4, 5)  // 15

function logInfo(level, ...messages) {            // first param is level, rest are messages
  messages.forEach(msg => console.log(`[${level}] ${msg}`))
}
logInfo("INFO", "Server started", "Port: 3000", "DB connected")

// In destructuring (must be LAST)
const [head, ...tail] = [1, 2, 3, 4, 5]         // head=1, tail=[2,3,4,5]
const { id, ...details } = { id: 1, name: "Alice", age: 28 }  // details={name,age}
```

---

## 18. Template Literals

```javascript
// ─── Basic interpolation ──────────────────────────────────────────────────
const name = "Alice"
const age = 28
console.log(`My name is ${name} and I am ${age} years old.`)

// Expressions inside ${}
console.log(`2 + 2 = ${2 + 2}`)                // "2 + 2 = 4"
console.log(`${name.toUpperCase()} wins!`)      // "ALICE wins!"
console.log(`Is adult: ${age >= 18}`)           // "Is adult: true"
console.log(`${age >= 18 ? "Adult" : "Minor"}`) // "Adult"

// ─── Multi-line strings ───────────────────────────────────────────────────
const html = `
  <div class="card">
    <h2>${name}</h2>
    <p>Age: ${age}</p>
  </div>
`

// ─── Nested template literals ─────────────────────────────────────────────
const items = ["apple", "banana", "cherry"]
const list = `
  <ul>
    ${items.map(item => `<li>${item}</li>`).join("\n    ")}
  </ul>
`

// ─── Tagged template literals ─────────────────────────────────────────────
function currency(strings, ...values) {
  return strings.reduce((result, str, i) => {
    const value = values[i - 1]
    return result + (typeof value === "number" ? `$${value.toFixed(2)}` : value) + str
  })
}
const price = 9.99
currency`The price is ${price} today`   // "The price is $9.99 today"
```

---

## 19. Default Parameters

```javascript
// Basic default parameter
function greet(name = "Guest") {
  return `Hello, ${name}!`
}
greet()            // "Hello, Guest!"
greet("Alice")     // "Hello, Alice!"
greet(undefined)   // "Hello, Guest!" — undefined triggers the default
greet(null)        // "Hello, null!"  — null does NOT trigger the default!

// Default values can be expressions or function calls
function createId(prefix = "USER", suffix = Date.now()) {
  return `${prefix}_${suffix}`
}

// Default can reference earlier parameters
function createRect(width, height = width) {
  return { width, height }
}
createRect(5)      // { width: 5, height: 5 } — square!
createRect(5, 10)  // { width: 5, height: 10 }

// With destructuring
function displayUser({ name = "Unknown", role = "user", active = true } = {}) {
  console.log(`${name} | ${role} | ${active}`)
}
displayUser({ name: "Alice" })   // "Alice | user | true"
displayUser()                    // "Unknown | user | true" — {} default prevents error
```

---

## 20. Error Handling

```javascript
// ─── try...catch...finally ────────────────────────────────────────────────
try {
  const data = JSON.parse("invalid json")        // throws SyntaxError
  console.log(data)
} catch (error) {
  console.error(error.name)                      // "SyntaxError"
  console.error(error.message)                   // "Unexpected token i in..."
  console.error(error.stack)                     // full stack trace
} finally {
  console.log("This ALWAYS runs — cleanup here")  // runs whether error or not
}


// ─── Throwing errors ──────────────────────────────────────────────────────
// Always use new Error() — gives you the stack trace
function divide(a, b) {
  if (typeof a !== "number" || typeof b !== "number") {
    throw new TypeError("Both arguments must be numbers")
  }
  if (b === 0) {
    throw new RangeError("Cannot divide by zero")
  }
  return a / b
}

// Don't do this — loses stack trace:
// throw "Something went wrong"          ❌

// Do this:
// throw new Error("Something went wrong")  ✅


// ─── Built-in Error types ─────────────────────────────────────────────────
new Error("generic error")
new TypeError("wrong type")              // wrong data type
new RangeError("value out of range")     // number outside allowed range
new ReferenceError("variable not found") // accessing undeclared variable
new SyntaxError("invalid syntax")        // code that can't be parsed
new URIError("invalid URI")              // malformed URI in encode/decode functions


// ─── Custom Error classes ─────────────────────────────────────────────────
class ValidationError extends Error {
  constructor(message, field) {
    super(message)
    this.name = "ValidationError"
    this.field = field
  }
}

class NotFoundError extends Error {
  constructor(resource, id) {
    super(`${resource} with id ${id} not found`)
    this.name = "NotFoundError"
    this.statusCode = 404
  }
}

// Using custom errors
try {
  const user = getUserById(999)
  if (!user) throw new NotFoundError("User", 999)
} catch (error) {
  if (error instanceof NotFoundError) {
    console.log(error.statusCode, error.message)   // 404 "User with id 999 not found"
  } else {
    throw error                                     // re-throw unknown errors!
  }
}


// ─── Error handling with async/await ─────────────────────────────────────
async function fetchData(url) {
  try {
    const response = await fetch(url)
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`)
    }
    const data = await response.json()
    return data
  } catch (error) {
    console.error("Fetch failed:", error.message)
    throw error    // re-throw so caller can also handle it
  }
}


// ─── Reading a stack trace ────────────────────────────────────────────────
// TypeError: Cannot read properties of undefined (reading 'name')
//   at getUserName (app.js:15:20)        ← where the error happened in YOUR code
//   at processUser (app.js:28:10)        ← which function called getUserName
//   at main (app.js:45:3)               ← which function called processUser
//   at Object.<anonymous> (app.js:50:1) ← top-level code
//
// Start reading from the TOP (most specific) and work down.
// Find the FIRST line that mentions YOUR file — that's where the bug is.
```

---

## 21. Promises & Async/Await

```javascript
// ─── The Problem: Callback Hell ───────────────────────────────────────────
// Reading nested async code is hard — this is "callback hell":
getUser(id, function(user) {
  getOrders(user.id, function(orders) {
    getOrderDetails(orders[0].id, function(details) {
      // deeply nested — hard to read and error-handle
    })
  })
})


// ─── Promise — an object representing future completion or failure ─────────
// A Promise is in one of three states: pending, fulfilled (resolved), rejected

// Creating a promise
const myPromise = new Promise((resolve, reject) => {
  const success = true
  if (success) {
    resolve("Data loaded successfully!")  // fulfilled
  } else {
    reject(new Error("Something went wrong"))  // rejected
  }
})

// Consuming a promise
myPromise
  .then(data => console.log(data))          // runs on resolve
  .catch(error => console.error(error))     // runs on reject
  .finally(() => console.log("Done!"))      // always runs


// ─── Chaining promises ────────────────────────────────────────────────────
fetch("https://api.example.com/user/1")
  .then(response => response.json())       // returns another promise
  .then(user => fetch(`/api/orders/${user.id}`))  // chain another async call
  .then(response => response.json())
  .then(orders => console.log(orders))
  .catch(error => console.error(error))   // single catch handles all errors


// ─── Promise utility methods ──────────────────────────────────────────────
// Promise.all — run multiple promises in PARALLEL, wait for ALL to resolve
const [user, orders, settings] = await Promise.all([
  fetchUser(1),
  fetchOrders(1),
  fetchSettings(1)
])
// If ANY promise rejects → immediately rejects

// Promise.allSettled — wait for ALL, get results even if some fail
const results = await Promise.allSettled([
  fetchUser(1),
  fetchUser(999),   // this one might fail
])
results.forEach(result => {
  if (result.status === "fulfilled") console.log(result.value)
  if (result.status === "rejected")  console.log(result.reason)
})

// Promise.race — resolves/rejects as soon as the FIRST promise settles
const fastest = await Promise.race([fetchFromServer1(), fetchFromServer2()])

// Promise.any — resolves with the FIRST fulfilled promise (ignores rejections)
const first = await Promise.any([fetchFromServer1(), fetchFromServer2()])


// ─── async/await — syntactic sugar over Promises ──────────────────────────
// async function always returns a Promise
// await pauses execution until the Promise resolves

async function loadUserData(userId) {
  try {
    const response = await fetch(`/api/users/${userId}`)

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}`)
    }

    const user = await response.json()
    const orders = await fetch(`/api/orders/${user.id}`)
    const orderData = await orders.json()

    return { user, orders: orderData }

  } catch (error) {
    console.error("Failed to load user data:", error)
    throw error
  }
}

// Parallel with async/await (don't await one at a time if not dependent!)
async function loadAll() {
  // ❌ Slow — sequential (each waits for the previous)
  const user = await fetchUser()
  const orders = await fetchOrders()

  // ✅ Fast — parallel (all start at the same time)
  const [user2, orders2] = await Promise.all([fetchUser(), fetchOrders()])
}
```

---

## 22. Higher-Order Functions

```javascript
// A Higher-Order Function (HOF) either:
// 1. Takes a function as an argument (callback)
// 2. Returns a function

// ─── Functions that take other functions ──────────────────────────────────
function runTwice(fn) {
  fn()
  fn()
}
runTwice(() => console.log("Hello!"))  // "Hello!" printed twice

// Built-in HOFs: map, filter, reduce, forEach, find, some, every
const numbers = [1, 2, 3, 4, 5]
numbers.map(n => n * 2)       // pass a function AS an argument
numbers.filter(n => n > 2)
numbers.reduce((acc, n) => acc + n, 0)


// ─── Functions that return functions ──────────────────────────────────────
function createGreeting(greeting) {
  return function(name) {            // returns a new function
    return `${greeting}, ${name}!`
  }
}
const sayHello = createGreeting("Hello")
const sayHi    = createGreeting("Hi")
sayHello("Alice")    // "Hello, Alice!"
sayHi("Bob")         // "Hi, Bob!"


// ─── Function Composition ─────────────────────────────────────────────────
const double = x => x * 2
const addTen = x => x + 10
const square = x => x * x

// Manual composition
const result = square(addTen(double(5)))  // double(5)=10 → addTen(10)=20 → square(20)=400

// Compose utility function
const compose = (...fns) => x => fns.reduceRight((v, fn) => fn(v), x)
const transform = compose(square, addTen, double)
transform(5)   // 400 — right to left: double first, then addTen, then square

const pipe = (...fns) => x => fns.reduce((v, fn) => fn(v), x)
const process = pipe(double, addTen, square)
process(5)     // 400 — left to right: double first, then addTen, then square


// ─── Currying ─────────────────────────────────────────────────────────────
// Transform a function that takes multiple arguments into a series of single-argument functions
const add = a => b => a + b             // curried function
const add5 = add(5)                     // partial application
add5(3)    // 8
add5(10)   // 15
add(2)(3)  // 5

// Practical currying example
const log = level => message => console.log(`[${level}] ${message}`)
const info  = log("INFO")
const error = log("ERROR")
info("Server started")    // "[INFO] Server started"
error("DB connection failed")  // "[ERROR] DB connection failed"


// ─── Memoization ──────────────────────────────────────────────────────────
// Cache function results to avoid re-computation
function memoize(fn) {
  const cache = new Map()
  return function(...args) {
    const key = JSON.stringify(args)
    if (cache.has(key)) {
      return cache.get(key)       // return cached result
    }
    const result = fn.apply(this, args)
    cache.set(key, result)
    return result
  }
}

const expensiveCalc = memoize(n => {
  console.log("Computing...")
  return n * n * n
})
expensiveCalc(10)   // "Computing..." then 1000
expensiveCalc(10)   // 1000 (from cache — no "Computing..." log)
```

---

## 23. Modules — CommonJS & ES Modules

```javascript
// ─── CommonJS (Node.js default) ──────────────────────────────────────────

// math.js — exporting
const add = (a, b) => a + b
const subtract = (a, b) => a - b
const PI = 3.14159

module.exports = { add, subtract, PI }      // export object
module.exports = add                         // export single value

// app.js — importing
const math = require("./math")
math.add(2, 3)

const { add, PI } = require("./math")       // destructure on import
const add2 = require("./math").add


// ─── ES Modules (modern — browser + Node.js with "type":"module") ──────────

// math.js — named exports
export const add = (a, b) => a + b
export const subtract = (a, b) => a - b
export const PI = 3.14159

// OR: export at the bottom
const multiply = (a, b) => a * b
export { multiply, PI }

// Default export — one per file
export default function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0)
}

// app.js — importing named exports
import { add, subtract, PI } from "./math.js"

// Import and rename
import { add as addNumbers } from "./math.js"

// Import everything as a namespace
import * as math from "./math.js"
math.add(2, 3)
math.PI

// Import default export
import calculateTotal from "./math.js"

// Import both default and named
import calculateTotal, { add, PI } from "./math.js"

// Dynamic import (lazy loading)
const module = await import("./math.js")
module.add(2, 3)

// ─── CommonJS vs ES Modules ───────────────────────────────────────────────
// CommonJS: require() — synchronous, runs at runtime — Node.js default
// ES Modules: import/export — statically analyzed, tree-shakeable — modern standard
// Use ES Modules for new projects whenever possible
```

---

## 24. Classes & Prototypes

```javascript
// ─── Class Syntax (ES6+) ─────────────────────────────────────────────────
class Animal {
  // Static property — belongs to the class, not instances
  static count = 0

  // Constructor — runs when new Animal() is called
  constructor(name, sound) {
    this.name = name       // instance property
    this.sound = sound
    Animal.count++
  }

  // Instance method
  speak() {
    return `${this.name} says ${this.sound}!`
  }

  // Getter — access like a property
  get info() {
    return `${this.name} (${this.sound})`
  }

  // Setter — set like a property
  set nickname(value) {
    this._nickname = value.trim()
  }

  // Static method — called on class, not instance
  static create(name, sound) {
    return new Animal(name, sound)
  }

  // Private field (ES2022) — truly private, not accessible outside
  #age = 0
  getAge() { return this.#age }
  setAge(value) { this.#age = value }
}

const cat = new Animal("Cat", "Meow")
cat.speak()              // "Cat says Meow!"
cat.info                 // "Cat (Meow)"
cat.nickname = "  Kitty  "
Animal.count             // 1
Animal.create("Dog", "Woof")


// ─── Inheritance ──────────────────────────────────────────────────────────
class Dog extends Animal {
  constructor(name, breed) {
    super(name, "Woof")        // must call super() before using this
    this.breed = breed
  }

  // Override parent method
  speak() {
    return `${super.speak()} (${this.breed})`  // call parent method with super
  }

  fetch() {
    return `${this.name} fetches the ball!`
  }
}

const dog = new Dog("Rex", "Labrador")
dog.speak()          // "Rex says Woof! (Labrador)"
dog.fetch()          // "Rex fetches the ball!"
dog instanceof Dog   // true
dog instanceof Animal // true


// ─── Prototype Chain ──────────────────────────────────────────────────────
// Every object has a hidden [[Prototype]] link to another object
// Property lookup: JS checks the object first, then its prototype, then prototype's prototype...
// This chain continues until null is reached

const arr = [1, 2, 3]
// arr → Array.prototype (has .push, .map, .filter...) → Object.prototype → null

// Prototype-based approach (before ES6 classes — still used in older code)
function Vehicle(make, model) {
  this.make = make
  this.model = model
}
Vehicle.prototype.describe = function() {
  return `${this.make} ${this.model}`
}
const car = new Vehicle("Toyota", "Camry")
car.describe()   // "Toyota Camry"
```

---

## 25. Nullish Coalescing & Optional Chaining

```javascript
// ─── Nullish Coalescing (??) ──────────────────────────────────────────────
// Returns right side ONLY when left side is null or undefined
// Unlike ||, it does NOT trigger for 0, false, or ""

const userAge = 0
const display1 = userAge || 18        // 18 — WRONG! 0 is falsy
const display2 = userAge ?? 18        // 0  — CORRECT! 0 is not null/undefined

const config = null
const port = config ?? 3000           // 3000 — config is null

const name = ""
const label1 = name || "Anonymous"    // "Anonymous" — empty string is falsy
const label2 = name ?? "Anonymous"    // "" — empty string is NOT null/undefined

// Nullish assignment (??=)
let value = null
value ??= "default"                   // value = "default" (was null)
let value2 = 0
value2 ??= "default"                  // value2 = 0 (NOT null/undefined, no change)


// ─── Optional Chaining (?.) ───────────────────────────────────────────────
// Access nested properties safely — returns undefined instead of throwing an error
const user = { name: "Alice", address: { city: "Mumbai" } }
const userWithoutAddress = { name: "Bob" }

user?.address?.city                   // "Mumbai"
userWithoutAddress?.address?.city     // undefined — no error!
// Without optional chaining:
// userWithoutAddress.address.city    // TypeError: Cannot read property 'city' of undefined

// Calling methods safely
user?.greet?.()                       // undefined (greet doesn't exist — no error)
user?.greet()                         // TypeError (greet doesn't exist — this will throw!)

// Array access
const data = null
data?.[0]                             // undefined — no error
data?.[0]?.name                       // undefined — no error

// Chaining with nullish coalescing
const city = user?.profile?.address?.city ?? "Unknown"  // "Unknown" if any step is null/undefined
```

---

## 26. Event Loop & Asynchronous JavaScript

```javascript
// JavaScript is single-threaded — one thing at a time
// The Event Loop allows async operations without blocking

// ─── How It Works ─────────────────────────────────────────────────────────
// Call Stack   → where code executes (synchronous, LIFO)
// Web APIs     → browser/Node APIs that handle async work (setTimeout, fetch, etc.)
// Callback Queue (Macro-task) → setTimeout, setInterval, I/O callbacks
// Microtask Queue  → Promise callbacks (.then/.catch), queueMicrotask
//
// Order of execution:
// 1. Run all synchronous code (empty the call stack)
// 2. Run ALL microtasks (Promises) — entire queue
// 3. Run ONE macro-task (setTimeout callback)
// 4. Repeat from step 2

console.log("1 — synchronous")

setTimeout(() => console.log("4 — setTimeout (macro-task)"), 0)  // even with 0ms delay!

Promise.resolve().then(() => console.log("3 — Promise (micro-task)"))

console.log("2 — synchronous")

// Output ORDER: 1, 2, 3, 4
// Microtasks (Promises) ALWAYS run before macro-tasks (setTimeout)!


// ─── setTimeout and setInterval ───────────────────────────────────────────
const timerId = setTimeout(() => {
  console.log("Runs once after 2 seconds")
}, 2000)

clearTimeout(timerId)              // cancel before it fires

const intervalId = setInterval(() => {
  console.log("Runs every 1 second")
}, 1000)

clearInterval(intervalId)          // stop the interval

// setTimeout with 0ms — defers to next iteration of event loop
setTimeout(() => console.log("After current synchronous code finishes"), 0)

// ─── queueMicrotask ────────────────────────────────────────────────────────
queueMicrotask(() => console.log("Runs as a microtask"))  // before setTimeout
```

---

## 27. console Methods

```javascript
// ─── Output methods ────────────────────────────────────────────────────────
console.log("Hello, world!")              // general output
console.log("Multiple", "values", 42)     // log multiple values
console.log({ name: "Alice", age: 28 })  // logs object with expandable properties
console.log([1, 2, 3])                   // logs array

console.error("Something went wrong!")   // red output + stack trace
console.warn("This is deprecated")       // yellow output
console.info("Server started on port 3000")  // similar to log (often styled differently)

console.dir(document.body)              // full interactive property listing of an object
console.dir(someFunction)               // shows function object properties

// ─── Structured output ─────────────────────────────────────────────────────
const users = [
  { name: "Alice", age: 28, city: "Mumbai" },
  { name: "Bob",   age: 35, city: "Delhi"  },
  { name: "Carol", age: 22, city: "Pune"   }
]
console.table(users)                    // renders as a neat, sortable table

// ─── Grouping ──────────────────────────────────────────────────────────────
console.group("User Details")           // start a collapsible group
console.log("Name: Alice")
console.log("Age: 28")
console.groupEnd()                      // end the group

console.groupCollapsed("Config")        // start collapsed by default
console.log("Port: 3000")
console.groupEnd()

// ─── Timing ────────────────────────────────────────────────────────────────
console.time("fetchData")               // start a timer with label
await fetchData()
console.timeEnd("fetchData")            // "fetchData: 342.15ms"
console.timeLog("fetchData")            // log elapsed time without stopping

// ─── Counting ──────────────────────────────────────────────────────────────
console.count("click")                  // "click: 1"
console.count("click")                  // "click: 2"
console.count("click")                  // "click: 3"
console.countReset("click")            // reset to 0

// ─── Stack trace ───────────────────────────────────────────────────────────
console.trace("Where was this called?") // prints current call stack

// ─── Assert ────────────────────────────────────────────────────────────────
console.assert(2 + 2 === 4, "Math is broken!")  // only logs if FIRST argument is false
console.assert(2 + 2 === 5, "2+2 is not 5!")    // logs: "Assertion failed: 2+2 is not 5!"

// ─── Clear ─────────────────────────────────────────────────────────────────
console.clear()                         // clear the console output

// ─── Formatting ────────────────────────────────────────────────────────────
console.log("%s has %d items", "Cart", 3)        // "Cart has 3 items"
console.log("%c Styled text", "color: red; font-size: 20px")  // browser — styled output
console.log("%o", someObject)            // print object (like console.dir)
```

---

## 28. Best Practices & Style Guide

### Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Variables & Functions | `camelCase` | `getUserData`, `totalPrice` |
| Classes & Constructors | `PascalCase` | `UserAccount`, `ProductCard` |
| Constants (module-level) | `UPPER_SNAKE_CASE` | `MAX_RETRIES`, `API_BASE_URL` |
| Private class fields | `#name` | `#password`, `#balance` |
| Boolean variables | `is/has/can` prefix | `isLoggedIn`, `hasPermission` |
| Event handlers | `on/handle` prefix | `onClick`, `handleSubmit` |

```javascript
// ✅ Good naming
const isUserLoggedIn = true
const MAX_LOGIN_ATTEMPTS = 5
const getUserById = (id) => { }
class UserProfile { }

// ❌ Bad naming
const x = true
const val = 5
const data = (a) => { }
class userprofile { }


// ─── Code Quality Rules ───────────────────────────────────────────────────

// 1. Use const by default, let when needed, never var
const API_URL = "https://api.example.com"
let retryCount = 0

// 2. Always use === (strict equality), never ==
if (value === undefined) { }    // ✅
if (value == undefined) { }     // ❌

// 3. Use explicit undefined checks over falsy checks when 0 or "" are valid
if (count !== undefined) { }    // ✅
if (count) { }                  // ❌ fails when count = 0

// 4. Always throw Error objects, never strings
throw new Error("Something failed")    // ✅
throw "Something failed"               // ❌

// 5. Prefer immutable methods over mutating
const sorted = [...arr].sort()        // ✅ original untouched
arr.sort()                            // ❌ mutates original

// 6. One function = one job (Single Responsibility)
// ✅ Good
const fetchUser = (id) => fetch(`/api/users/${id}`).then(r => r.json())
const displayUser = (user) => document.getElementById("name").textContent = user.name

// ❌ Bad — mixing concerns
const fetchAndDisplayUser = (id) => {
  fetch(`/api/users/${id}`)
    .then(r => r.json())
    .then(user => document.getElementById("name").textContent = user.name)
}

// 7. Early return to reduce nesting
// ✅ Good
function processOrder(order) {
  if (!order) return null
  if (!order.items.length) return { error: "Empty order" }
  if (order.total < 0) return { error: "Invalid total" }
  return processValidOrder(order)
}

// ❌ Bad — pyramid of doom
function processOrder(order) {
  if (order) {
    if (order.items.length) {
      if (order.total >= 0) {
        return processValidOrder(order)
      }
    }
  }
}

// 8. Avoid global variables — use modules or closures
// ❌ Don't pollute global scope
var config = { api: "..." }

// ✅ Use modules
// config.js → export const config = { api: "..." }

// 9. Handle errors explicitly — don't swallow them
try {
  riskyOperation()
} catch (error) {
  // ✅ Log and/or rethrow
  console.error("Operation failed:", error.message)
  throw error
}
// ❌ Never:
try { riskyOperation() } catch (e) { /* empty catch */ }

// 10. Use optional chaining and nullish coalescing to be safe
const city = user?.address?.city ?? "Unknown"   // ✅
const city2 = user && user.address && user.address.city || "Unknown"  // ❌ old, verbose
```

---

## 29. Common Interview Questions

### Q1: What is the difference between `==` and `===`?
**Answer:** `==` is loose equality — it performs type coercion before comparing. For example, `0 == false` is `true` and `"5" == 5` is `true` because JavaScript converts both sides to the same type first. `===` is strict equality — it checks both value AND type without any conversion. `0 === false` is `false` and `"5" === 5` is `false`. Always use `===` in production code to avoid subtle bugs caused by unexpected type coercion.

---

### Q2: What is a closure and why is it useful?
**Answer:** A closure is created when a function "remembers" and can access variables from its outer (enclosing) function's scope even after that outer function has returned. It works because JavaScript functions carry a reference to their surrounding lexical environment, not just the code. Closures are useful for: data privacy (creating private variables like a counter that can only be incremented through a specific function), factory functions (creating multiple functions with different pre-configured state), and maintaining state in callbacks and event handlers.

---

### Q3: What is the difference between `null` and `undefined`?
**Answer:** `undefined` means a variable has been declared but no value has been assigned to it yet — JavaScript sets it automatically. `null` is an intentional assignment meaning "no value" or "empty" — a developer explicitly sets it to represent the absence of an object. Their `typeof` also differs: `typeof undefined` returns `"undefined"` while `typeof null` returns `"object"` (which is a historical bug in JavaScript). To check for either, use `value == null` (the only safe use of `==`) or check each explicitly with `===`.

---

### Q4: What is the difference between `let`, `const`, and `var`?
**Answer:** `var` is function-scoped (or global if outside a function), can be re-declared and re-assigned, and is hoisted with an initial value of `undefined` — which causes confusing bugs and should never be used in modern code. `let` is block-scoped, cannot be re-declared in the same scope, can be re-assigned, and is hoisted but placed in a Temporal Dead Zone (accessing it before declaration throws). `const` is block-scoped, cannot be re-declared or re-assigned (but the contents of an object/array it holds can be mutated), and also goes through TDZ. Use `const` by default, `let` when the value needs to change, and never use `var`.

---

### Q5: What are Promises and how does async/await relate to them?
**Answer:** A Promise is an object representing the eventual completion or failure of an asynchronous operation. It can be in one of three states: pending (ongoing), fulfilled (completed successfully), or rejected (failed). You chain `.then()` to handle success, `.catch()` for errors, and `.finally()` for cleanup that always runs. `async/await` is syntactic sugar built on top of Promises that makes asynchronous code look and behave more like synchronous code. An `async` function always returns a Promise, and `await` inside it pauses execution until the awaited Promise settles. Both achieve the same thing — `async/await` is generally more readable.

---

### Q6: What is the event loop in JavaScript?
**Answer:** JavaScript is single-threaded — it has one call stack and executes one thing at a time. The event loop is what allows JavaScript to perform non-blocking operations despite this. When an async operation (like `setTimeout` or `fetch`) is called, it's handed off to the browser's Web APIs. When it completes, its callback goes into a queue. The event loop constantly checks: if the call stack is empty, it picks up the next task from the queue and pushes it onto the stack. The microtask queue (Promises) is always emptied completely before the macro-task queue (setTimeout callbacks) runs.

---

### Q7: What is the difference between `map`, `filter`, and `reduce`?
**Answer:** All three are non-mutating array methods that take a callback. `map` transforms every element and always returns a new array of the same length — use it when you want to convert each item. `filter` tests each element against a condition and returns a new array containing only elements that pass — the result is shorter or equal length. `reduce` accumulates all elements into a single value (a number, string, object, or even another array) — use it for aggregation like sum, grouping, or flattening. They can be chained together because `map` and `filter` both return new arrays.

---

### Q8: What is hoisting in JavaScript?
**Answer:** Hoisting is JavaScript's behavior of processing declarations before code executes — conceptually moving them to the top of their scope. Function declarations are fully hoisted — you can call a function before the line where it's defined. Variables declared with `var` are hoisted and initialized to `undefined` — which is why accessing a `var` before its declaration returns `undefined` instead of throwing an error. Variables declared with `let` and `const` are also hoisted but NOT initialized — they are in a "Temporal Dead Zone" and accessing them before their declaration throws a `ReferenceError`.

---

### Q9: What is the difference between arrow functions and regular functions?
**Answer:** The most important difference is `this` binding. Regular functions get their own `this` dynamically — it depends on how the function is called (the object before the dot, or `undefined` in strict mode). Arrow functions do not have their own `this` — they inherit it from the surrounding lexical scope where they were defined. This makes arrow functions ideal for callbacks and methods inside other methods where you want `this` to refer to the outer object. Additionally, arrow functions have no `arguments` object, cannot be used as constructors with `new`, and have no `prototype` property.

---

### Q10: What is the difference between the spread operator (`...`) and the rest parameter (`...`)?
**Answer:** Both use the same `...` syntax but do opposite things. The spread operator expands an iterable (array, string, or object) into individual elements — you use it when calling a function (`Math.max(...arr)`), combining arrays (`[...arr1, ...arr2]`), copying objects (`{...obj, newKey: value}`), or passing multiple arguments. The rest parameter collects multiple individual values into a single array — you use it in a function's parameter list (`function sum(...nums)`) to handle variable numbers of arguments, or in destructuring (`const [head, ...tail] = arr`) to collect the remaining items. Spread = one thing expands into many; Rest = many things collected into one.

---

## 🔄 JavaScript — At a Glance

```
Source Code → JS Engine (Parser → AST → Bytecode → Execution)
                 ↓
           Call Stack (sync code runs here)
                 ↓
      Web APIs (setTimeout, fetch, DOM events)
                 ↓
    Microtask Queue (Promises) → runs BEFORE macro-tasks
    Macro-task Queue (setTimeout) → runs AFTER microtasks
                 ↓
           Event Loop (keeps checking: stack empty? → pick from queue)
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| JavaScript | Single-threaded, dynamic, interpreted — runs in browser + Node.js |
| Primitive types | string, number, bigint, boolean, undefined, null, symbol |
| `typeof null` | `"object"` — famous JS bug |
| `const` | Block-scoped, can't re-assign — default choice |
| `let` | Block-scoped, can re-assign — when value changes |
| `var` | Function-scoped, hoisted — NEVER use |
| Falsy values | `false, 0, "", null, undefined, NaN` — only 6! |
| Truthy | Everything else including `[]`, `{}`, `"0"` |
| `===` | Strict equality — value AND type, no coercion — always use |
| `==` | Loose equality — coerces types — avoid |
| Hoisting | Declarations moved to top; `var` → `undefined`; `let/const` → TDZ |
| TDZ | Temporal Dead Zone — `let/const` declared but not yet initialized |
| Closure | Inner function remembers outer function's variables after outer returns |
| `this` in arrow fn | Inherited from surrounding scope (lexical) |
| `this` in regular fn | Set by how it's called (dynamic) |
| Pass by value | Primitives — copy passed, original unchanged |
| Pass by reference | Objects/arrays — reference shared, mutation affects original |
| `map` | Transform every element → new array same length |
| `filter` | Keep elements matching condition → new array (shorter) |
| `reduce` | Accumulate all elements → single value |
| `find` | First element matching condition |
| `some` | True if at least one matches |
| `every` | True if all match |
| Spread `...` | Expand iterable into individual elements |
| Rest `...` | Collect multiple arguments into an array |
| Destructuring | Unpack values from objects/arrays into variables |
| Template literal | Backtick strings with `${}` interpolation |
| Optional chaining `?.` | Safe property access — undefined instead of error |
| Nullish coalescing `??` | Fallback only for null/undefined (not 0 or "") |
| Promise | Object representing future async result |
| `async/await` | Syntactic sugar over Promises — looks synchronous |
| `Promise.all` | Run multiple promises in parallel — wait for all |
| Event loop | Mechanism that processes async callbacks when stack is empty |
| Microtasks | Promise callbacks — run before setTimeout callbacks |
| Macro-tasks | setTimeout, setInterval — run after microtasks |
| Closure | Function + its surrounding lexical environment |
| Currying | `f(a)(b)` — function returns a function that takes next arg |
| HOF | Function that takes or returns a function |
| CommonJS | `require()` / `module.exports` — Node.js default |
| ES Modules | `import` / `export` — modern standard |
| Prototype chain | Property lookup chain: obj → proto → Object.prototype → null |

---

> 💡 **The Production Golden Rule:**
> Always combine — **`const` by default + `===` always + `throw new Error()` never strings + Optional chaining + Nullish coalescing + async/await with try/catch + Immutable array methods + Named functions for debugging**.
> These 8 habits make your JavaScript code predictable, debuggable, and professional.

---

## 30. Map & Set

### Map — Key-Value pairs with ANY type as key

```javascript
// ─── Map vs Object ─────────────────────────────────────────────────────────
// Object: keys must be strings or symbols
// Map:    keys can be ANYTHING — objects, functions, numbers, etc.

const map = new Map()

// ─── Adding entries ────────────────────────────────────────────────────────
map.set("name", "Alice")           // string key
map.set(42, "the answer")          // number key
map.set(true, "boolean key")       // boolean key
const objKey = { id: 1 }
map.set(objKey, "object as key")   // object as key!

// Initialize with entries
const prices = new Map([
  ["apple", 50],
  ["banana", 30],
  ["mango", 80]
])

// ─── Reading entries ───────────────────────────────────────────────────────
map.get("name")                    // "Alice"
map.get(42)                        // "the answer"
map.get(objKey)                    // "object as key"
map.has("name")                    // true
map.has("email")                   // false
map.size                           // 4 (property, not method)

// ─── Deleting entries ──────────────────────────────────────────────────────
map.delete("name")                 // true (returns whether key existed)
map.clear()                        // removes all entries

// ─── Iterating ─────────────────────────────────────────────────────────────
for (const [key, value] of prices) {
  console.log(`${key}: ₹${value}`)
}
prices.forEach((value, key) => console.log(key, value))

prices.keys()                      // iterator of keys
prices.values()                    // iterator of values
prices.entries()                   // iterator of [key, value] pairs

// Convert to array
const arr = [...prices]            // [["apple",50], ["banana",30], ...]
const keys = [...prices.keys()]    // ["apple", "banana", "mango"]
const vals = [...prices.values()]  // [50, 30, 80]

// ─── Map vs Object — When to use what ─────────────────────────────────────
// Use Map when:
// - Keys are not strings (objects, numbers, functions as keys)
// - Order of insertion matters (Map preserves insertion order)
// - Frequent add/delete operations
// - Need to know the size easily (.size)
// Use Object when:
// - Simple string keys
// - JSON serialization needed (Map doesn't serialize to JSON)
// - Prototypal access needed


// ─── Set — collection of UNIQUE values ────────────────────────────────────
const set = new Set()

set.add(1)
set.add(2)
set.add(3)
set.add(2)      // duplicate — silently ignored
set.add("2")    // different from number 2 — added (strict equality check)

set.size         // 4
set.has(2)       // true
set.has(5)       // false
set.delete(2)    // removes 2
set.clear()      // removes all

// Initialize with an array
const nums = new Set([1, 2, 3, 2, 1, 4])  // {1, 2, 3, 4} — duplicates removed!

// ─── Most common use: REMOVE DUPLICATES from an array ─────────────────────
const withDups = [1, 2, 3, 2, 1, 4, 3, 5]
const unique   = [...new Set(withDups)]    // [1, 2, 3, 4, 5] ✅

const tags = ["js", "python", "js", "go", "python"]
const uniqueTags = [...new Set(tags)]      // ["js", "python", "go"]

// ─── Set iteration ─────────────────────────────────────────────────────────
for (const val of nums) console.log(val)
nums.forEach(val => console.log(val))
[...nums].map(val => val * 2)             // convert to array first, then map

// ─── Set operations (union, intersection, difference) ─────────────────────
const a = new Set([1, 2, 3, 4])
const b = new Set([3, 4, 5, 6])

const union        = new Set([...a, ...b])           // {1,2,3,4,5,6}
const intersection = new Set([...a].filter(x => b.has(x)))  // {3,4}
const difference   = new Set([...a].filter(x => !b.has(x))) // {1,2}
```

---

## 31. WeakMap & WeakRef

```javascript
// ─── WeakMap — Map with weak references to keys ───────────────────────────
// Keys MUST be objects (not primitives)
// Keys are weakly held — if the object key has no other references,
// it gets garbage collected automatically (no memory leaks!)
// NOT iterable — no size, no forEach, no iteration

const weakMap = new WeakMap()

let user = { name: "Alice" }
weakMap.set(user, { sessionToken: "abc123", loginTime: Date.now() })

weakMap.get(user)              // { sessionToken: "abc123", ... }
weakMap.has(user)              // true
weakMap.delete(user)           // true

user = null                    // user object is garbage collected
                               // weakMap entry is ALSO automatically cleaned up!

// ─── Perfect use case: storing private data for class instances ────────────
const privateData = new WeakMap()

class BankAccount {
  constructor(owner, balance) {
    privateData.set(this, { balance, transactionHistory: [] })
    this.owner = owner
  }

  deposit(amount) {
    const data = privateData.get(this)
    data.balance += amount
    data.transactionHistory.push({ type: "deposit", amount })
  }

  getBalance() {
    return privateData.get(this).balance
  }
}

const account = new BankAccount("Alice", 1000)
account.deposit(500)
account.getBalance()           // 1500
// account.balance            // undefined — truly private!


// ─── WeakSet — Set with weak references ───────────────────────────────────
// Only stores objects, not primitives
// Objects are weakly held — garbage collected when no other references
// NOT iterable

const weakSet = new WeakSet()
let obj = { id: 1 }
weakSet.add(obj)
weakSet.has(obj)               // true
weakSet.delete(obj)

// Use case: track which DOM elements have been processed
const processed = new WeakSet()
function processElement(el) {
  if (processed.has(el)) return    // already done
  // ...do work...
  processed.add(el)
}


// ─── WeakRef — hold weak reference to an object ───────────────────────────
// The referenced object CAN be garbage collected
// Use .deref() to get the object — returns undefined if already GC'd

let cache = new WeakRef({ data: [1, 2, 3] })
const value = cache.deref()
if (value) {
  console.log(value.data)       // [1, 2, 3] if still alive
} else {
  console.log("Object was garbage collected — refetch needed")
}
```

---

## 32. Iterators & Generators

```javascript
// ─── Iterator Protocol ────────────────────────────────────────────────────
// An object is an "iterator" if it has a .next() method
// that returns { value: any, done: boolean }

// Custom iterator
function createRange(start, end) {
  let current = start
  return {
    next() {
      if (current <= end) {
        return { value: current++, done: false }
      }
      return { value: undefined, done: true }
    }
  }
}

const range = createRange(1, 5)
range.next()    // { value: 1, done: false }
range.next()    // { value: 2, done: false }
range.next()    // { value: 3, done: false }
// ...
range.next()    // { value: undefined, done: true }


// ─── Iterable Protocol ────────────────────────────────────────────────────
// An object is "iterable" if it has [Symbol.iterator]() that returns an iterator
// Arrays, Strings, Maps, Sets are all built-in iterables

// Custom iterable object
const myRange = {
  from: 1,
  to: 5,
  [Symbol.iterator]() {          // makes this object iterable
    let current = this.from
    const last = this.to
    return {
      next() {
        if (current <= last) return { value: current++, done: false }
        return { value: undefined, done: true }
      }
    }
  }
}

for (const num of myRange) console.log(num)   // 1 2 3 4 5
const arr = [...myRange]                       // [1, 2, 3, 4, 5]
const [a, b, c] = myRange                     // destructuring works too!


// ─── Generator Functions ──────────────────────────────────────────────────
// function* — can pause (yield) and resume execution
// Returns a Generator object which is both iterator AND iterable

function* simpleGenerator() {
  console.log("Start")
  yield 1               // pause, hand back 1
  console.log("After 1")
  yield 2               // pause, hand back 2
  console.log("After 2")
  yield 3               // pause, hand back 3
  console.log("End")    // runs after last .next() call
}

const gen = simpleGenerator()
gen.next()    // logs "Start", returns { value: 1, done: false }
gen.next()    // logs "After 1", returns { value: 2, done: false }
gen.next()    // logs "After 2", returns { value: 3, done: false }
gen.next()    // logs "End", returns { value: undefined, done: true }


// ─── Infinite generator ────────────────────────────────────────────────────
function* idGen(start = 1) {
  while (true) {
    yield start++
  }
}
const ids = idGen()
ids.next().value   // 1
ids.next().value   // 2
ids.next().value   // 3 — keeps going forever, but only computes when asked


// ─── Practical generator: range ───────────────────────────────────────────
function* range(start, end, step = 1) {
  for (let i = start; i <= end; i += step) {
    yield i
  }
}
[...range(0, 10, 2)]  // [0, 2, 4, 6, 8, 10]
for (const n of range(1, 5)) console.log(n)  // 1 2 3 4 5


// ─── Generator delegation with yield* ─────────────────────────────────────
function* inner() { yield "a"; yield "b" }
function* outer() {
  yield 1
  yield* inner()    // delegate to inner generator
  yield 2
}
[...outer()]   // [1, "a", "b", 2]


// ─── Async generators ─────────────────────────────────────────────────────
async function* fetchPages(baseUrl, pages) {
  for (let i = 1; i <= pages; i++) {
    const response = await fetch(`${baseUrl}?page=${i}`)
    const data = await response.json()
    yield data
  }
}

// Consume with for await...of
for await (const page of fetchPages("/api/products", 5)) {
  console.log(page)   // processes one page at a time — memory efficient!
}
```

---

## 33. Symbol — Deep Dive

```javascript
// ─── Symbol basics ────────────────────────────────────────────────────────
const sym1 = Symbol("description")
const sym2 = Symbol("description")
sym1 === sym2    // false — every Symbol is ALWAYS unique

// ─── Use case 1: Unique object keys (no collision) ─────────────────────────
const ID    = Symbol("id")
const NAME  = Symbol("name")

const user = {
  [ID]: 123,
  [NAME]: "Alice",
  name: "Alice (public)"    // different from Symbol("name")
}

user[ID]          // 123
user[NAME]        // "Alice"
user.name         // "Alice (public)"

// Symbols are hidden from most iterations:
Object.keys(user)          // ["name"] — Symbol keys NOT included
Object.values(user)        // ["Alice (public)"]
JSON.stringify(user)       // '{"name":"Alice (public)"}' — Symbols dropped!
for (const key in user) { }  // only "name", no Symbols

// But accessible with:
Object.getOwnPropertySymbols(user)   // [Symbol(id), Symbol(name)]
Reflect.ownKeys(user)               // ["name", Symbol(id), Symbol(name)]


// ─── Use case 2: Well-known Symbols (customize built-in behavior) ──────────
// Symbol.iterator — make object iterable with for...of
const range = {
  from: 1, to: 5,
  [Symbol.iterator]() {
    let n = this.from
    return { next: () => n <= this.to ? { value: n++, done: false } : { done: true } }
  }
}
[...range]   // [1, 2, 3, 4, 5]

// Symbol.toPrimitive — control type coercion
const money = {
  amount: 100,
  currency: "USD",
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return this.amount
    if (hint === "string") return `${this.amount} ${this.currency}`
    return this.amount
  }
}
+money           // 100 (number hint)
`${money}`       // "100 USD" (string hint)
money + 50       // 150 (default hint)

// Symbol.hasInstance — customize instanceof
class EvenNumber {
  static [Symbol.hasInstance](num) {
    return Number.isInteger(num) && num % 2 === 0
  }
}
2  instanceof EvenNumber   // true
3  instanceof EvenNumber   // false
4  instanceof EvenNumber   // true


// ─── Global Symbol registry ────────────────────────────────────────────────
const s1 = Symbol.for("shared")    // creates or retrieves from global registry
const s2 = Symbol.for("shared")
s1 === s2                          // true! — same instance from registry
Symbol.keyFor(s1)                  // "shared" — get key back from registry
```

---

## 34. Proxy & Reflect

```javascript
// ─── Proxy — intercept and customize object operations ────────────────────
// new Proxy(target, handler)
// handler defines "traps" — functions that intercept operations

const user = { name: "Alice", age: 28, _password: "secret" }

const safeUser = new Proxy(user, {
  // Intercept property reads
  get(target, prop) {
    if (prop.startsWith("_")) {
      throw new Error(`Access denied: "${prop}" is private`)
    }
    return Reflect.get(target, prop)  // default behavior
  },

  // Intercept property writes
  set(target, prop, value) {
    if (prop === "age" && typeof value !== "number") {
      throw new TypeError("age must be a number")
    }
    if (prop === "age" && value < 0) {
      throw new RangeError("age cannot be negative")
    }
    return Reflect.set(target, prop, value)
  },

  // Intercept 'in' operator
  has(target, prop) {
    if (prop.startsWith("_")) return false   // hide private props
    return Reflect.has(target, prop)
  },

  // Intercept property deletion
  deleteProperty(target, prop) {
    if (prop.startsWith("_")) throw new Error("Cannot delete private properties")
    return Reflect.deleteProperty(target, prop)
  }
})

safeUser.name             // "Alice" ✅
// safeUser._password     // Error: Access denied ❌
safeUser.age = 29         // ✅
// safeUser.age = "old"   // TypeError ❌
"name" in safeUser        // true
"_password" in safeUser   // false (hidden by proxy)


// ─── Proxy for validation (practical) ────────────────────────────────────
function createValidator(target, validators) {
  return new Proxy(target, {
    set(obj, prop, value) {
      if (validators[prop]) {
        const error = validators[prop](value)
        if (error) throw new Error(`${prop}: ${error}`)
      }
      return Reflect.set(obj, prop, value)
    }
  })
}

const userWithValidation = createValidator({}, {
  name: val => typeof val !== "string" ? "must be a string" : null,
  age:  val => (typeof val !== "number" || val < 0) ? "must be a non-negative number" : null,
  email: val => !val.includes("@") ? "must be a valid email" : null
})

userWithValidation.name = "Alice"       // ✅
userWithValidation.age  = 28            // ✅
// userWithValidation.age = -5          // Error: age: must be a non-negative number


// ─── Reflect — mirrors object operations ──────────────────────────────────
// Reflect methods are the "default" behavior versions of Proxy traps
// Always use Reflect inside Proxy traps instead of direct target manipulation

Reflect.get(obj, "name")            // same as obj.name
Reflect.set(obj, "age", 30)         // same as obj.age = 30 — returns true/false
Reflect.has(obj, "name")            // same as "name" in obj
Reflect.deleteProperty(obj, "name") // same as delete obj.name
Reflect.ownKeys(obj)                // all own keys (strings + symbols)
Reflect.apply(fn, context, args)    // same as fn.apply(context, args)
Reflect.construct(Cls, args)        // same as new Cls(...args)
```

---

## 35. Regular Expressions (RegEx)

```javascript
// ─── Creating RegEx ───────────────────────────────────────────────────────
const re1 = /hello/          // literal syntax (preferred)
const re2 = /hello/gi        // with flags: g=global, i=case-insensitive
const re3 = new RegExp("hello", "gi")  // constructor (use when pattern is dynamic)

// ─── Flags ────────────────────────────────────────────────────────────────
// g — global: find ALL matches, not just the first
// i — case-insensitive
// m — multiline: ^ and $ match start/end of each LINE
// s — dotAll: . matches newlines too
// u — unicode: enable full unicode support
// d — indices: include match indices in result

// ─── Testing ──────────────────────────────────────────────────────────────
/hello/.test("say hello world")   // true — does pattern exist?
/hello/.test("hi there")          // false

// ─── Matching ─────────────────────────────────────────────────────────────
"hello world".match(/\w+/g)       // ["hello", "world"] — all words (global flag)
"hello world".match(/(\w+)\s(\w+)/)  // ["hello world", "hello", "world"] — with groups

// matchAll — returns iterator of ALL matches with groups
const str = "cat bat sat"
const matches = [...str.matchAll(/[a-z]at/g)]
matches[0][0]   // "cat"
matches[1][0]   // "bat"

// ─── Replacing ────────────────────────────────────────────────────────────
"hello WORLD".replace(/world/i, "JavaScript")   // "hello JavaScript"
"aabbcc".replace(/b/g, "X")                      // "aaXXcc"
"2024-01-15".replace(/(\d{4})-(\d{2})-(\d{2})/, "$3/$2/$1")  // "15/01/2024"
"hello world".replace(/(\w+)/g, match => match.toUpperCase())  // "HELLO WORLD"
"hello world".replaceAll("o", "0")               // "hell0 w0rld"

// ─── Searching ────────────────────────────────────────────────────────────
"hello world".search(/world/)     // 6 — index of first match (-1 if not found)

// ─── Splitting ────────────────────────────────────────────────────────────
"one1two2three3".split(/\d/)      // ["one", "two", "three", ""]
"a, b ,  c".split(/\s*,\s*/)      // ["a", "b", "c"]

// ─── Character classes ────────────────────────────────────────────────────
// .   — any character except newline
// \d  — digit [0-9]
// \D  — non-digit
// \w  — word character [a-zA-Z0-9_]
// \W  — non-word character
// \s  — whitespace (space, tab, newline)
// \S  — non-whitespace
// \b  — word boundary
// \B  — non-word boundary
// [abc]   — character set (a, b, or c)
// [^abc]  — negated set (anything except a, b, c)
// [a-z]   — range (lowercase letters)
// [A-Z]   — range (uppercase letters)
// [0-9]   — range (digits)

// ─── Quantifiers ──────────────────────────────────────────────────────────
// *     — 0 or more (greedy)
// +     — 1 or more (greedy)
// ?     — 0 or 1 (optional)
// {n}   — exactly n times
// {n,}  — n or more times
// {n,m} — between n and m times
// *?    — 0 or more (lazy — as few as possible)
// +?    — 1 or more (lazy)

// ─── Anchors ──────────────────────────────────────────────────────────────
// ^  — start of string (or start of line with m flag)
// $  — end of string (or end of line with m flag)
// \b — word boundary
/^hello/  .test("hello world")   // true — starts with hello
/world$/  .test("hello world")   // true — ends with world
/^\d{5}$/ .test("12345")         // true — exactly 5 digits

// ─── Groups ───────────────────────────────────────────────────────────────
// (abc)        — capturing group
// (?<name>abc) — named capturing group
// (?:abc)      — non-capturing group
// (?=abc)      — lookahead: what follows must be abc
// (?!abc)      — negative lookahead
// (?<=abc)     — lookbehind: what precedes must be abc
// (?<!abc)     — negative lookbehind

const dateStr = "2024-01-15"
const match = dateStr.match(/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/)
match.groups.year    // "2024"
match.groups.month   // "01"
match.groups.day     // "15"

// ─── Practical examples ───────────────────────────────────────────────────
const emailRegex    = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/
const phoneRegex    = /^\+?[1-9]\d{9,14}$/
const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
const urlRegex      = /^https?:\/\/(www\.)?[-a-zA-Z0-9@:%._+~#=]{2,256}\.[a-z]{2,6}\b/
const ipv4Regex     = /^(\d{1,3}\.){3}\d{1,3}$/
const hexColorRegex = /^#([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$/

emailRegex.test("alice@example.com")    // true
emailRegex.test("not-an-email")         // false
```

---

## 36. JSON — Parse, Stringify & Edge Cases

```javascript
// ─── JSON.stringify — convert JS value to JSON string ─────────────────────
const user = { name: "Alice", age: 28, active: true, scores: [95, 87, 92] }

JSON.stringify(user)
// '{"name":"Alice","age":28,"active":true,"scores":[95,87,92]}'

// With indentation (pretty print)
JSON.stringify(user, null, 2)
// {
//   "name": "Alice",
//   "age": 28,
//   ...
// }

// With replacer array — only include these keys
JSON.stringify(user, ["name", "age"])
// '{"name":"Alice","age":28}'

// With replacer function — control each key-value
JSON.stringify(user, (key, value) => {
  if (key === "age") return undefined    // exclude age
  if (typeof value === "number") return value * 2   // double all numbers
  return value
})

// ─── What stringify IGNORES or changes ────────────────────────────────────
JSON.stringify({ fn: function(){}, sym: Symbol(), undef: undefined })
// '{}'  — functions, Symbols, and undefined are omitted!

JSON.stringify([undefined, function(){}, Symbol()])
// '[null,null,null]' — in arrays, they become null

JSON.stringify({ date: new Date() })
// '{"date":"2024-01-15T10:30:00.000Z"}' — Date becomes ISO string

JSON.stringify({ map: new Map() })
// '{"map":{}}' — Map/Set serialize as empty objects!

// toJSON method — customize serialization
const specialObj = {
  name: "Alice",
  password: "secret",
  toJSON() {
    return { name: this.name }  // only serialize name
  }
}
JSON.stringify(specialObj)    // '{"name":"Alice"}' — password excluded


// ─── JSON.parse — convert JSON string to JS value ─────────────────────────
const jsonStr = '{"name":"Alice","age":28,"scores":[95,87,92]}'
const parsed = JSON.parse(jsonStr)
parsed.name      // "Alice"
parsed.scores    // [95, 87, 92]

// With reviver function — transform values during parsing
const withDates = '{"name":"Alice","createdAt":"2024-01-15T10:30:00.000Z"}'
const result = JSON.parse(withDates, (key, value) => {
  if (key === "createdAt") return new Date(value)   // convert string back to Date
  return value
})
result.createdAt instanceof Date   // true ✅


// ─── Safe JSON parse with error handling ──────────────────────────────────
function safeJsonParse(str, fallback = null) {
  try {
    return JSON.parse(str)
  } catch {
    return fallback
  }
}

safeJsonParse('{"name":"Alice"}')   // { name: "Alice" }
safeJsonParse("invalid json")        // null (fallback)
safeJsonParse("invalid", {})         // {} (custom fallback)


// ─── Deep clone with JSON (simple but limited) ────────────────────────────
const original = { a: 1, b: { c: 2 }, arr: [1, 2, 3] }
const clone = JSON.parse(JSON.stringify(original))
// ⚠️ Loses: functions, Symbols, undefined, Date becomes string, Map/Set empty
// ✅ Use structuredClone() instead (ES2022) — handles more types
const betterClone = structuredClone(original)
```

---

## 37. Date & Time

```javascript
// ─── Creating dates ────────────────────────────────────────────────────────
const now     = new Date()                        // current date and time
const specific = new Date("2024-01-15")           // from ISO string
const fromParts = new Date(2024, 0, 15, 10, 30)  // year, month(0-indexed!), day, hours, mins
const fromMs  = new Date(1705312200000)           // from milliseconds (Unix timestamp)

// ─── Getting values ────────────────────────────────────────────────────────
const d = new Date("2024-01-15T10:30:45.500Z")

d.getFullYear()          // 2024
d.getMonth()             // 0 — January is 0! (0-11)
d.getDate()              // 15 — day of month (1-31)
d.getDay()               // 1 — day of week (0=Sunday, 1=Monday, ..., 6=Saturday)
d.getHours()             // 10 (local time)
d.getMinutes()           // 30
d.getSeconds()           // 45
d.getMilliseconds()      // 500
d.getTime()              // milliseconds since Unix epoch (Jan 1, 1970 UTC)
d.getTimezoneOffset()    // minutes offset from UTC

// UTC versions
d.getUTCFullYear()
d.getUTCMonth()
d.getUTCDate()
d.getUTCHours()

// ─── Setting values ────────────────────────────────────────────────────────
const date = new Date()
date.setFullYear(2025)
date.setMonth(11)         // December (0-indexed)
date.setDate(25)
date.setHours(0, 0, 0, 0) // midnight

// ─── Formatting ────────────────────────────────────────────────────────────
const dt = new Date("2024-01-15T10:30:00")

dt.toString()            // "Mon Jan 15 2024 10:30:00 GMT+0530 (IST)"
dt.toISOString()         // "2024-01-15T05:00:00.000Z" — UTC ISO 8601
dt.toLocaleDateString()  // "1/15/2024" (en-US)
dt.toLocaleTimeString()  // "10:30:00 AM"
dt.toLocaleString()      // "1/15/2024, 10:30:00 AM"

// Intl.DateTimeFormat — full control over locale formatting
const formatter = new Intl.DateTimeFormat("en-IN", {
  year: "numeric", month: "long", day: "numeric",
  hour: "2-digit", minute: "2-digit", hour12: true
})
formatter.format(dt)     // "15 January 2024 at 10:30 AM"

// ─── Date comparison ───────────────────────────────────────────────────────
const d1 = new Date("2024-01-01")
const d2 = new Date("2024-06-01")

d1 < d2          // true  — comparison works directly
d1.getTime() < d2.getTime()   // true (explicit, clearer)
d1.getTime() === d2.getTime() // true only if exact same millisecond

// ─── Date arithmetic ───────────────────────────────────────────────────────
const start = new Date("2024-01-01")
const end   = new Date("2024-12-31")
const diffMs    = end - start                    // milliseconds
const diffDays  = Math.floor(diffMs / (1000 * 60 * 60 * 24))  // 365 days

// Add 7 days to a date
const nextWeek = new Date(now.getTime() + 7 * 24 * 60 * 60 * 1000)

// Add N days cleanly
function addDays(date, days) {
  const result = new Date(date)
  result.setDate(result.getDate() + days)
  return result
}

// ─── Performance timing ────────────────────────────────────────────────────
Date.now()                      // current timestamp in ms (faster than new Date().getTime())
performance.now()               // high-resolution timestamp in ms (microsecond precision)

const t0 = performance.now()
expensiveOperation()
const t1 = performance.now()
console.log(`Took ${t1 - t0} milliseconds`)
```

---

## 38. Math Object — All Methods

```javascript
// ─── Constants ────────────────────────────────────────────────────────────
Math.PI          // 3.141592653589793
Math.E           // 2.718281828459045 (Euler's number)
Math.LN2         // 0.6931... (natural log of 2)
Math.SQRT2       // 1.4142... (square root of 2)
Math.LOG2E       // 1.4426...
Math.LOG10E      // 0.4342...

// ─── Rounding ─────────────────────────────────────────────────────────────
Math.round(4.5)       // 5  — round to nearest integer (0.5 rounds UP)
Math.round(4.4)       // 4
Math.floor(4.9)       // 4  — always rounds DOWN (towards -∞)
Math.floor(-4.1)      // -5 — floor goes more negative
Math.ceil(4.1)        // 5  — always rounds UP (towards +∞)
Math.ceil(-4.9)       // -4 — ceil goes less negative
Math.trunc(4.9)       // 4  — truncate decimal (towards 0)
Math.trunc(-4.9)      // -4 — towards 0 (not -∞ like floor)

// Fix to N decimal places
(3.14159).toFixed(2)   // "3.14" — returns STRING!
Math.round(3.14159 * 100) / 100  // 3.14 — returns number

// ─── Absolute value & sign ────────────────────────────────────────────────
Math.abs(-5)          // 5
Math.abs(5)           // 5
Math.sign(-5)         // -1
Math.sign(0)          // 0
Math.sign(5)          // 1

// ─── Min & Max ────────────────────────────────────────────────────────────
Math.min(3, 1, 4, 1, 5, 9)       // 1
Math.max(3, 1, 4, 1, 5, 9)       // 9
Math.min(...[3, 1, 4, 1, 5, 9])  // 1 — spread array
Math.max(...[3, 1, 4, 1, 5, 9])  // 9

// ─── Power & Roots ────────────────────────────────────────────────────────
Math.pow(2, 10)       // 1024 (2^10)
2 ** 10               // 1024 — same thing (modern syntax)
Math.sqrt(16)         // 4
Math.cbrt(27)         // 3 (cube root)
Math.hypot(3, 4)      // 5 — √(3² + 4²)
Math.hypot(3, 4, 5)   // √(3² + 4² + 5²)

// ─── Logarithms ───────────────────────────────────────────────────────────
Math.log(Math.E)      // 1 (natural log — ln)
Math.log2(8)          // 3 (log base 2)
Math.log10(1000)      // 3 (log base 10)
Math.log1p(x)         // ln(1+x) — more accurate for small x

// ─── Trigonometry ─────────────────────────────────────────────────────────
Math.sin(Math.PI / 2)  // 1
Math.cos(0)            // 1
Math.tan(Math.PI / 4)  // 1
Math.asin(1)           // π/2
Math.acos(1)           // 0
Math.atan(1)           // π/4
Math.atan2(y, x)       // angle in radians from origin to point (y, x)

// ─── Random ───────────────────────────────────────────────────────────────
Math.random()                             // 0.0 to 0.999... (never exactly 1)
Math.floor(Math.random() * 6) + 1        // random int 1–6 (dice roll)
Math.floor(Math.random() * (max - min + 1)) + min  // random int min–max

function randomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min
}
randomInt(1, 100)   // random integer from 1 to 100 inclusive

function shuffle(arr) {
  return [...arr].sort(() => Math.random() - 0.5)  // simple (not perfectly uniform)
}

// Fisher-Yates shuffle (uniform distribution)
function shuffleFY(arr) {
  const a = [...arr]
  for (let i = a.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [a[i], a[j]] = [a[j], a[i]]
  }
  return a
}

// ─── Other ────────────────────────────────────────────────────────────────
Math.clz32(1)         // 31 — count leading zeros in 32-bit representation
Math.fround(1.5)      // nearest 32-bit float
Math.imul(2, 4)       // 8 — 32-bit integer multiplication
```

---

## 39. Number Methods — All

```javascript
// ─── Number static methods ────────────────────────────────────────────────
Number.isInteger(42)          // true
Number.isInteger(42.5)        // false
Number.isInteger("42")        // false — NO coercion!

Number.isFinite(Infinity)     // false
Number.isFinite(42)           // true
Number.isFinite("42")         // false — NO coercion (unlike global isFinite)

Number.isNaN(NaN)             // true
Number.isNaN(42)              // false
Number.isNaN("hello")         // false — NO coercion (unlike global isNaN!)
isNaN("hello")                // true — global isNaN DOES coerce!

Number.parseInt("42px")       // 42 — parses until non-numeric character
Number.parseFloat("3.14abc")  // 3.14

Number.MAX_SAFE_INTEGER       // 9007199254740991 (2^53 - 1)
Number.MIN_SAFE_INTEGER       // -9007199254740991
Number.MAX_VALUE              // ~1.79e+308 (largest representable number)
Number.MIN_VALUE              // ~5e-324 (smallest positive number)
Number.POSITIVE_INFINITY      // Infinity
Number.NEGATIVE_INFINITY      // -Infinity
Number.NaN                    // NaN
Number.EPSILON                // 2.22e-16 (smallest difference between floats)

Number.isSafeInteger(9007199254740991)  // true
Number.isSafeInteger(9007199254740992)  // false — above MAX_SAFE_INTEGER

// ─── Number instance methods ──────────────────────────────────────────────
(3.14159).toFixed(2)          // "3.14" — returns STRING, rounds
(1234.5).toLocaleString()     // "1,234.5" (en-US)
(1234.5).toLocaleString("en-IN")  // "1,234.5"
(255).toString(16)            // "ff" — to hex
(255).toString(2)             // "11111111" — to binary
(255).toString(8)             // "377" — to octal
(3.14159).toPrecision(4)      // "3.142" — 4 significant digits

// ─── Floating point gotcha ────────────────────────────────────────────────
0.1 + 0.2                     // 0.30000000000000004 (NOT 0.3!)
0.1 + 0.2 === 0.3             // false ← classic JS floating point trap

// Fix: use toFixed or epsilon comparison
(0.1 + 0.2).toFixed(10)       // "0.3000000000" — round for display
Math.abs(0.1 + 0.2 - 0.3) < Number.EPSILON  // true — correct comparison

// For money — use integers (store as cents)
const price = 999    // ₹9.99 stored as paise
const tax   = 18     // ₹0.18 stored as paise
const total = (price + tax) / 100   // ₹10.17 — display only

// ─── parseInt & parseFloat ────────────────────────────────────────────────
parseInt("10")           // 10
parseInt("10.9")         // 10 — truncates decimal
parseInt("10px")         // 10 — stops at non-numeric
parseInt("px10")         // NaN — starts with non-numeric
parseInt("0xff")         // 255 — parses hex
parseInt("10", 2)        // 2 — parse as binary
parseInt("ff", 16)       // 255 — parse as hex
parseFloat("3.14kg")     // 3.14
parseFloat("$3.14")      // NaN

// ─── Intl.NumberFormat — locale-aware formatting ──────────────────────────
new Intl.NumberFormat("en-IN", { style: "currency", currency: "INR" }).format(12345.67)
// "₹12,345.67"

new Intl.NumberFormat("en-US", { style: "percent" }).format(0.857)
// "85.7%"

new Intl.NumberFormat("de-DE").format(1234567.89)
// "1.234.567,89" — German format
```

---

## 40. DOM Manipulation (Browser)

```javascript
// ─── Selecting elements ────────────────────────────────────────────────────
document.getElementById("myId")              // single element by ID (fastest)
document.querySelector(".myClass")           // first match using CSS selector
document.querySelectorAll("p.intro")         // NodeList of ALL matches
document.getElementsByClassName("myClass")  // HTMLCollection (live)
document.getElementsByTagName("div")        // HTMLCollection (live)
document.getElementsByName("username")      // by name attribute

// ─── Navigating the DOM ────────────────────────────────────────────────────
const el = document.querySelector("#main")
el.parentElement              // parent element
el.parentNode                 // parent node (could be document)
el.children                   // HTMLCollection of child ELEMENTS
el.childNodes                 // NodeList of ALL child nodes (includes text nodes)
el.firstElementChild          // first child element
el.lastElementChild           // last child element
el.nextElementSibling         // next sibling element
el.previousElementSibling     // previous sibling element

// ─── Creating & adding elements ────────────────────────────────────────────
const div = document.createElement("div")
div.textContent = "Hello"
div.className = "card"
div.id = "myCard"

document.body.appendChild(div)              // add as last child
document.body.prepend(div)                  // add as first child
el.insertAdjacentElement("beforebegin", div) // before el
el.insertAdjacentElement("afterend", div)    // after el
el.insertAdjacentHTML("beforeend", "<p>text</p>")  // insert raw HTML

// insertAdjacentHTML positions:
// "beforebegin" — before the element itself
// "afterbegin"  — inside element, before first child
// "beforeend"   — inside element, after last child
// "afterend"    — after the element itself

// ─── Removing elements ─────────────────────────────────────────────────────
el.remove()                    // remove element from DOM (modern)
el.parentElement.removeChild(el)  // old way

// ─── Reading & modifying content ───────────────────────────────────────────
el.textContent = "New text"    // safe — escapes HTML
el.innerHTML   = "<b>Bold</b>" // renders HTML — ⚠️ XSS risk if using user input!
el.innerText   = "Visible text" // respects CSS visibility (slower than textContent)
el.outerHTML                   // element + its HTML tag

// ─── Attributes ────────────────────────────────────────────────────────────
el.getAttribute("src")
el.setAttribute("src", "image.png")
el.removeAttribute("disabled")
el.hasAttribute("required")
el.dataset.userId              // access data-user-id attribute
el.dataset.userId = "123"      // set data-user-id="123"

// ─── Classes ───────────────────────────────────────────────────────────────
el.classList.add("active", "highlight")
el.classList.remove("inactive")
el.classList.toggle("visible")           // add if not present, remove if present
el.classList.toggle("open", isOpen)      // force add/remove based on boolean
el.classList.replace("old", "new")
el.classList.contains("active")          // true/false
el.className = "card active"             // set all classes at once (replaces existing)

// ─── Styles ────────────────────────────────────────────────────────────────
el.style.color = "red"
el.style.backgroundColor = "blue"        // camelCase in JS for CSS properties
el.style.cssText = "color: red; font-size: 16px;"  // set multiple at once
window.getComputedStyle(el).color         // get actual computed style (includes CSS)

// ─── Dimensions & Position ─────────────────────────────────────────────────
el.getBoundingClientRect()    // { top, left, right, bottom, width, height }
el.offsetWidth / el.offsetHeight  // element + padding + border
el.clientWidth / el.clientHeight  // element + padding (no border/scrollbar)
el.scrollWidth / el.scrollHeight  // total scrollable size
el.scrollTop / el.scrollLeft      // current scroll position
window.innerWidth / window.innerHeight   // viewport dimensions

// ─── Fragment — batch DOM manipulation (performance) ────────────────────────
const fragment = document.createDocumentFragment()
for (let i = 0; i < 1000; i++) {
  const li = document.createElement("li")
  li.textContent = `Item ${i}`
  fragment.appendChild(li)      // add to fragment (NOT the live DOM)
}
document.querySelector("ul").appendChild(fragment)  // ONE DOM update!
```

---

## 41. Events (Browser)

```javascript
// ─── Adding event listeners ────────────────────────────────────────────────
const btn = document.querySelector("#myBtn")

// addEventListener is preferred over onclick = fn
btn.addEventListener("click", function(event) {
  console.log("clicked!", event)
})

// Arrow function — loses 'this' but has event object
btn.addEventListener("click", (event) => {
  console.log(event.target)    // element that was clicked
})

// Named function — can be removed later
function handleClick(e) { console.log(e) }
btn.addEventListener("click", handleClick)
btn.removeEventListener("click", handleClick)  // must be same function reference

// ─── Event object ──────────────────────────────────────────────────────────
btn.addEventListener("click", (event) => {
  event.target           // element that triggered the event
  event.currentTarget    // element the listener is attached to
  event.type             // "click"
  event.preventDefault() // prevent default behavior (form submit, link follow)
  event.stopPropagation() // stop event from bubbling up to parent elements
  event.stopImmediatePropagation()  // stop bubbling + prevent other listeners on same element
  event.clientX / event.clientY    // mouse position relative to viewport
  event.pageX / event.pageY        // mouse position relative to document
  event.key / event.code / event.keyCode  // keyboard info
  event.ctrlKey / event.shiftKey / event.altKey / event.metaKey  // modifier keys
})

// ─── Event propagation ─────────────────────────────────────────────────────
// Events bubble UP from target → parent → ... → document (default)
// Events can also CAPTURE (capture phase): document → ... → target

// Add listener in CAPTURE phase (before bubbling)
parent.addEventListener("click", handler, { capture: true })
// or: parent.addEventListener("click", handler, true)  // older syntax

// ─── Event delegation — attach ONE listener to parent for multiple children ──
// Instead of adding listener to each button, add one to the container
document.querySelector(".btn-container").addEventListener("click", (event) => {
  if (event.target.matches(".delete-btn")) {
    const id = event.target.dataset.id
    deleteItem(id)
  }
  if (event.target.matches(".edit-btn")) {
    editItem(event.target.dataset.id)
  }
})
// Benefits: works for dynamically added elements, better performance

// ─── Common event types ────────────────────────────────────────────────────
// Mouse events:
// click, dblclick, mousedown, mouseup, mouseover, mouseout, mousemove, contextmenu

// Keyboard events:
// keydown, keyup, keypress (deprecated)
document.addEventListener("keydown", (e) => {
  if (e.key === "Enter") { }
  if (e.key === "Escape") { }
  if (e.ctrlKey && e.key === "s") { e.preventDefault(); saveFile() }
})

// Form events:
// submit, change, input, focus, blur, reset
document.querySelector("form").addEventListener("submit", (e) => {
  e.preventDefault()            // prevent page reload
  const data = new FormData(e.target)
  console.log(Object.fromEntries(data))
})

// Window events:
// load, DOMContentLoaded, resize, scroll, beforeunload
window.addEventListener("DOMContentLoaded", () => {
  // DOM is ready — safe to query elements
})
window.addEventListener("load", () => {
  // Everything (images, stylesheets) is loaded
})
window.addEventListener("resize", debounce(() => {
  console.log(window.innerWidth)
}, 200))

// Custom events
const myEvent = new CustomEvent("itemAdded", {
  detail: { id: 1, name: "Apple" },
  bubbles: true
})
document.dispatchEvent(myEvent)
document.addEventListener("itemAdded", (e) => console.log(e.detail))

// ─── Debounce & Throttle (performance patterns) ────────────────────────────
// Debounce — wait until N ms after LAST call (typing stopped, resize ended)
function debounce(fn, delay) {
  let timer
  return function(...args) {
    clearTimeout(timer)
    timer = setTimeout(() => fn.apply(this, args), delay)
  }
}
const searchInput = document.querySelector("#search")
searchInput.addEventListener("input", debounce((e) => {
  fetchResults(e.target.value)   // only fires 300ms after user stops typing
}, 300))

// Throttle — fire at most once per N ms (scroll, mousemove)
function throttle(fn, limit) {
  let lastCall = 0
  return function(...args) {
    const now = Date.now()
    if (now - lastCall >= limit) {
      lastCall = now
      return fn.apply(this, args)
    }
  }
}
window.addEventListener("scroll", throttle(() => {
  updateScrollIndicator()        // fires at most once every 100ms
}, 100))
```

---

## 42. Fetch API & HTTP

```javascript
// ─── Basic fetch (GET) ────────────────────────────────────────────────────
const response = await fetch("https://api.example.com/users")

// Check for HTTP errors (fetch only rejects on network errors, not 404/500!)
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`)
}

const data = await response.json()     // parse JSON body
// OR:
const text = await response.text()    // plain text
const blob = await response.blob()    // binary data (images, files)
const form = await response.formData() // FormData

// ─── Response properties ──────────────────────────────────────────────────
response.status          // 200, 404, 500, etc.
response.statusText      // "OK", "Not Found", etc.
response.ok              // true if status 200-299
response.headers.get("Content-Type")
response.url
response.redirected

// ─── POST request ─────────────────────────────────────────────────────────
const response2 = await fetch("https://api.example.com/users", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "Authorization": "Bearer eyJhbG..."
  },
  body: JSON.stringify({ name: "Alice", email: "alice@example.com" })
})
const created = await response2.json()

// ─── Other HTTP methods ────────────────────────────────────────────────────
// PUT (replace entire resource)
await fetch(`/api/users/${id}`, {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify(updatedUser)
})

// PATCH (update specific fields)
await fetch(`/api/users/${id}`, {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ age: 29 })
})

// DELETE
await fetch(`/api/users/${id}`, { method: "DELETE" })

// ─── Uploading files ──────────────────────────────────────────────────────
const formData = new FormData()
formData.append("name", "Alice")
formData.append("avatar", fileInput.files[0])

await fetch("/api/upload", {
  method: "POST",
  body: formData
  // DO NOT set Content-Type — browser sets it automatically with boundary
})

// ─── Abort a fetch (cancel in-flight request) ─────────────────────────────
const controller = new AbortController()
const signal = controller.signal

setTimeout(() => controller.abort(), 5000)  // cancel after 5 seconds

try {
  const response = await fetch("/api/slow-endpoint", { signal })
  const data = await response.json()
} catch (error) {
  if (error.name === "AbortError") {
    console.log("Request was cancelled")
  } else {
    throw error
  }
}

// ─── Complete fetch utility function ──────────────────────────────────────
async function apiRequest(url, { method = "GET", body, headers = {} } = {}) {
  const config = {
    method,
    headers: {
      "Content-Type": "application/json",
      "Authorization": `Bearer ${localStorage.getItem("token")}`,
      ...headers
    }
  }
  if (body) config.body = JSON.stringify(body)

  const response = await fetch(url, config)

  if (response.status === 401) {
    // Redirect to login
    window.location.href = "/login"
    return
  }
  if (!response.ok) {
    const error = await response.json().catch(() => ({}))
    throw new Error(error.message || `HTTP ${response.status}`)
  }
  return response.status === 204 ? null : response.json()
}

// Usage:
const user     = await apiRequest("/api/users/1")
const newUser  = await apiRequest("/api/users", { method: "POST", body: { name: "Alice" } })
const updated  = await apiRequest("/api/users/1", { method: "PATCH", body: { age: 29 } })
await           apiRequest("/api/users/1", { method: "DELETE" })
```

---

## 43. LocalStorage, SessionStorage & Cookies

```javascript
// ─── localStorage — persists FOREVER (until cleared) ─────────────────────
// Max ~5MB | Same origin only | Synchronous | Strings only

localStorage.setItem("username", "Alice")
localStorage.getItem("username")          // "Alice"
localStorage.removeItem("username")
localStorage.clear()                      // remove ALL localStorage data
localStorage.length                       // number of items
localStorage.key(0)                       // key at index 0

// Store objects — must serialize to JSON
localStorage.setItem("user", JSON.stringify({ name: "Alice", age: 28 }))
const user = JSON.parse(localStorage.getItem("user"))

// Safe get with default
function getStorage(key, defaultVal = null) {
  try {
    const item = localStorage.getItem(key)
    return item ? JSON.parse(item) : defaultVal
  } catch {
    return defaultVal
  }
}

// Storage event — fires in OTHER tabs when localStorage changes
window.addEventListener("storage", (e) => {
  console.log(e.key)       // changed key
  console.log(e.newValue)  // new value
  console.log(e.oldValue)  // old value
  console.log(e.url)       // page that made the change
})


// ─── sessionStorage — cleared when tab/window closes ─────────────────────
// Same API as localStorage, but data lives only for the browser session
sessionStorage.setItem("tempData", JSON.stringify({ step: 1 }))
sessionStorage.getItem("tempData")
sessionStorage.removeItem("tempData")
sessionStorage.clear()


// ─── Cookies ─────────────────────────────────────────────────────────────
// Can be read by server (unlike localStorage) | Sent in HTTP headers
// Can have expiry, domain, path, Secure, HttpOnly, SameSite flags

// Set a cookie
document.cookie = "username=Alice"
document.cookie = "username=Alice; expires=Thu, 31 Dec 2024 23:59:59 GMT"
document.cookie = "username=Alice; max-age=86400"  // 1 day in seconds
document.cookie = "token=abc123; Secure; SameSite=Strict; path=/"

// Read cookies (comes as one big string!)
document.cookie   // "username=Alice; theme=dark; lang=en"

// Parse cookies
function getCookie(name) {
  const cookies = Object.fromEntries(
    document.cookie.split("; ").map(c => c.split("="))
  )
  return cookies[name]
}
getCookie("username")   // "Alice"

// Delete a cookie (set expiry in the past)
document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT"

// ─── When to use what ─────────────────────────────────────────────────────
// localStorage  → user preferences, theme, language (persists long-term)
// sessionStorage → form wizard state, temp UI state (clears on tab close)
// Cookies       → auth tokens (HttpOnly for security), anything server needs to read
```

---

## 44. Design Patterns in JavaScript

```javascript
// ─── 1. Singleton — ensure only ONE instance exists ───────────────────────
class DatabaseConnection {
  static #instance = null

  constructor(config) {
    if (DatabaseConnection.#instance) {
      return DatabaseConnection.#instance   // return existing instance
    }
    this.config = config
    this.isConnected = false
    DatabaseConnection.#instance = this
  }

  connect() { this.isConnected = true }
  static getInstance() { return DatabaseConnection.#instance }
}

const db1 = new DatabaseConnection({ host: "localhost" })
const db2 = new DatabaseConnection({ host: "other" })
db1 === db2    // true — same instance!


// ─── 2. Observer / Event Emitter ──────────────────────────────────────────
class EventEmitter {
  constructor() { this.events = {} }

  on(event, listener) {
    if (!this.events[event]) this.events[event] = []
    this.events[event].push(listener)
    return this  // enable chaining
  }

  off(event, listener) {
    this.events[event] = this.events[event]?.filter(l => l !== listener)
    return this
  }

  emit(event, ...args) {
    this.events[event]?.forEach(listener => listener(...args))
    return this
  }

  once(event, listener) {
    const wrapper = (...args) => { listener(...args); this.off(event, wrapper) }
    return this.on(event, wrapper)
  }
}

const emitter = new EventEmitter()
emitter.on("login", user => console.log(`${user.name} logged in`))
emitter.once("error", err => console.error(err))  // fires only once
emitter.emit("login", { name: "Alice" })


// ─── 3. Factory — create objects without using new directly ───────────────
function createUser(type, name) {
  const base = { name, createdAt: new Date() }

  if (type === "admin") {
    return { ...base, role: "admin", permissions: ["read", "write", "delete"] }
  }
  if (type === "editor") {
    return { ...base, role: "editor", permissions: ["read", "write"] }
  }
  return { ...base, role: "viewer", permissions: ["read"] }
}

const admin  = createUser("admin", "Alice")
const viewer = createUser("viewer", "Bob")


// ─── 4. Module Pattern — create private scope ─────────────────────────────
const counter = (() => {
  let count = 0                  // private

  return {
    increment: () => ++count,
    decrement: () => --count,
    reset:     () => { count = 0 },
    getCount:  () => count
  }
})()

counter.increment()   // 1
counter.increment()   // 2
counter.getCount()    // 2
// counter.count      // undefined — private!


// ─── 5. Strategy — swap algorithms at runtime ─────────────────────────────
const sortStrategies = {
  bubble: (arr) => { /* bubble sort */ return [...arr].sort() },
  quick:  (arr) => { /* quick sort  */ return [...arr].sort((a, b) => a - b) },
  merge:  (arr) => { /* merge sort  */ return [...arr].sort((a, b) => a - b) }
}

function sortData(data, strategy = "quick") {
  return sortStrategies[strategy](data)
}

sortData([3, 1, 4], "bubble")
sortData([3, 1, 4], "merge")


// ─── 6. Decorator — add behavior without modifying original ───────────────
// Function decorator
function withLogging(fn) {
  return function(...args) {
    console.log(`Calling ${fn.name} with`, args)
    const result = fn.apply(this, args)
    console.log(`${fn.name} returned`, result)
    return result
  }
}

function add(a, b) { return a + b }
const loggedAdd = withLogging(add)
loggedAdd(2, 3)   // logs call and result, returns 5


// ─── 7. Command — encapsulate actions as objects (undo/redo) ───────────────
class TextEditor {
  constructor() { this.text = ""; this.history = [] }

  execute(command) {
    command.execute(this)
    this.history.push(command)
  }

  undo() {
    const command = this.history.pop()
    command?.undo(this)
  }
}

const insertCommand = (text) => ({
  execute: (editor) => { editor.text += text },
  undo:    (editor) => { editor.text = editor.text.slice(0, -text.length) }
})

const editor = new TextEditor()
editor.execute(insertCommand("Hello"))
editor.execute(insertCommand(" World"))
editor.text    // "Hello World"
editor.undo()
editor.text    // "Hello"
```

---

## 45. Performance Tips

```javascript
// ─── 1. Avoid N+1 — batch operations ──────────────────────────────────────
// ❌ Bad — N+1 DOM updates
items.forEach(item => {
  document.getElementById("list").innerHTML += `<li>${item}</li>`
})

// ✅ Good — build string, one update
document.getElementById("list").innerHTML = items.map(i => `<li>${i}</li>`).join("")


// ─── 2. Use DocumentFragment for many DOM insertions ──────────────────────
const fragment = document.createDocumentFragment()
items.forEach(item => {
  const li = document.createElement("li")
  li.textContent = item
  fragment.appendChild(li)
})
list.appendChild(fragment)   // single reflow


// ─── 3. Debounce expensive operations ─────────────────────────────────────
// Already covered in section 41 — search inputs, resize handlers


// ─── 4. Lazy evaluation — don't compute until needed ──────────────────────
const expensiveValue = (() => {
  let cached
  return () => {
    if (cached !== undefined) return cached
    cached = /* expensive computation */ runComputation()
    return cached
  }
})()


// ─── 5. Use Web Workers for heavy computation ─────────────────────────────
// Runs in separate thread — doesn't block the UI
const worker = new Worker("heavy-task.js")
worker.postMessage({ data: largeArray })
worker.onmessage = (e) => console.log("Result:", e.data)


// ─── 6. Avoid memory leaks ────────────────────────────────────────────────
// ❌ Bad — event listener added but never removed
function setupComponent() {
  window.addEventListener("resize", handleResize)
}

// ✅ Good — clean up on component unmount
function setupComponent() {
  window.addEventListener("resize", handleResize)
  return () => window.removeEventListener("resize", handleResize)  // cleanup fn
}

// ❌ Bad — circular reference
const obj = {}
obj.self = obj   // obj references itself — prevents GC


// ─── 7. Use const where possible ──────────────────────────────────────────
// JS engine can optimize const declarations better than let/var


// ─── 8. Avoid unnecessary object creation in hot paths ────────────────────
// ❌ Bad — creates new object every call
function getConfig() { return { timeout: 5000, retries: 3 } }
setInterval(() => { const c = getConfig(); /* ... */ }, 100)

// ✅ Good — create once
const CONFIG = { timeout: 5000, retries: 3 }
setInterval(() => { /* use CONFIG */ }, 100)


// ─── 9. short-circuit evaluation ──────────────────────────────────────────
// ✅ Avoid running expensive function if first check fails
const result = isValid(input) && expensiveTransform(input)


// ─── 10. Proper async patterns ────────────────────────────────────────────
// ❌ Sequential — slower (second waits for first)
const user   = await fetchUser()
const orders = await fetchOrders()

// ✅ Parallel — faster (both run simultaneously)
const [user2, orders2] = await Promise.all([fetchUser(), fetchOrders()])
```

---

## 46. ES2020–ES2024 New Features

```javascript
// ─── ES2020 ────────────────────────────────────────────────────────────────
// Optional chaining ?.
user?.address?.city

// Nullish coalescing ??
const port = config ?? 3000

// BigInt
const big = 9007199254740991n

// Promise.allSettled — wait for all, don't fail on rejection
await Promise.allSettled([p1, p2, p3])

// String.matchAll — all regex matches with groups
const matches = [...str.matchAll(/\d+/g)]

// Dynamic import
const module = await import("./myModule.js")

// globalThis — works in browser, Node, Web Workers
globalThis.setTimeout


// ─── ES2021 ────────────────────────────────────────────────────────────────
// String.replaceAll
"aabbcc".replaceAll("b", "X")   // "aaXXcc"

// Logical assignment operators
x &&= newVal   // x = x && newVal  (assign only if x is truthy)
x ||= newVal   // x = x || newVal  (assign only if x is falsy)
x ??= newVal   // x = x ?? newVal  (assign only if x is null/undefined)

// Numeric separators for readability
const million = 1_000_000
const pi = 3.141_592_653

// Promise.any — first fulfilled
const first = await Promise.any([p1, p2, p3])

// WeakRef & FinalizationRegistry


// ─── ES2022 ────────────────────────────────────────────────────────────────
// Class static blocks
class Config {
  static db
  static {                          // runs once when class is evaluated
    Config.db = process.env.DB_URL
  }
}

// Private class fields (#)
class BankAccount {
  #balance = 0                      // truly private field
  get balance() { return this.#balance }
}

// Array.at() — negative indexing!
[1, 2, 3].at(-1)                    // 3 — last element
[1, 2, 3].at(-2)                    // 2 — second to last
"hello".at(-1)                      // "o"

// Object.hasOwn() — replaces hasOwnProperty
Object.hasOwn(obj, "name")          // true/false

// Error.cause — chain errors
const err = new Error("DB failed", { cause: originalError })
err.cause   // the original error

// await at top level in ES Modules (top-level await)
const data = await fetch("/api").then(r => r.json())  // works at module top level


// ─── ES2023 ────────────────────────────────────────────────────────────────
// Array.findLast / findLastIndex
[1, 2, 3, 4].findLast(n => n % 2 === 0)        // 4
[1, 2, 3, 4].findLastIndex(n => n % 2 === 0)   // 3

// Non-mutating array methods (copy versions of sort/reverse/splice)
[3, 1, 2].toSorted()                // [1, 2, 3] — new array, original unchanged
[1, 2, 3].toReversed()              // [3, 2, 1] — new array
[1, 2, 3].toSpliced(1, 1, 9)       // [1, 9, 3] — new array
[1, 2, 3].with(1, 99)              // [1, 99, 3] — replace at index, new array

// Array.from with Set for deduplication (official pattern)
Array.from(new Set([1, 1, 2, 2, 3]))  // [1, 2, 3]

// Hashbang grammar (#!)
#!/usr/bin/env node  // first line of Node.js scripts


// ─── ES2024 ────────────────────────────────────────────────────────────────
// Object.groupBy — group array elements by a key
const inventory = [
  { name: "Apple", type: "fruit" },
  { name: "Banana", type: "fruit" },
  { name: "Carrot", type: "vegetable" }
]
const grouped = Object.groupBy(inventory, item => item.type)
// { fruit: [{...}, {...}], vegetable: [{...}] }

// Map.groupBy
const groupedMap = Map.groupBy(inventory, item => item.type)

// Promise.withResolvers — create promise with resolve/reject accessible outside
const { promise, resolve, reject } = Promise.withResolvers()
setTimeout(() => resolve("done!"), 1000)
await promise   // "done!" after 1 second

// Temporal API (Stage 3 — coming soon, replacing Date object)
// const now = Temporal.Now.plainDateTimeISO()
// Much better date/time API — timezone aware, immutable

// Array.isArray improvements and Uint8Array.from/to methods
Uint8Array.fromBase64("SGVsbG8=")  // Uint8Array for binary data handling
```

---

---

## 47. Garbage Collection & Memory Management

```javascript
// ─── How JavaScript manages memory ────────────────────────────────────────
// 1. Allocate — memory is reserved when you create variables/objects
// 2. Use     — read and write operations
// 3. Release — garbage collector automatically frees memory no longer reachable

// JavaScript uses the Mark-and-Sweep algorithm:
// GC starts from "roots" (global vars, call stack)
// Marks everything reachable
// Sweeps (frees) everything NOT marked

// ─── Memory Leaks — common causes ─────────────────────────────────────────

// 1. Global variables that never get cleaned up
// ❌ Bad
function processData() {
  result = [1, 2, 3, 4, 5]  // no var/let/const → becomes a global variable!
}

// ✅ Good
function processData() {
  const result = [1, 2, 3, 4, 5]  // local — freed when function exits
}


// 2. Forgotten event listeners
// ❌ Bad — listener keeps reference to element even after it's removed from DOM
const button = document.getElementById("btn")
button.addEventListener("click", handleClick)
// ... later remove button from DOM but listener still in memory


// ✅ Good — always remove listeners when no longer needed
button.removeEventListener("click", handleClick)
// OR use AbortController (modern approach)
const controller = new AbortController()
button.addEventListener("click", handleClick, { signal: controller.signal })
controller.abort()  // removes all listeners attached with this signal


// 3. Closures holding references to large objects
// ❌ Bad — hugeData stays in memory as long as getData is alive
function createClosure() {
  const hugeData = new Array(1_000_000).fill("data")
  return function getData() {
    return hugeData[0]  // holds reference to ALL of hugeData
  }
}

// ✅ Good — only keep what you need
function createClosure() {
  const hugeData = new Array(1_000_000).fill("data")
  const firstItem = hugeData[0]   // extract only what you need
  return function getData() {
    return firstItem              // hugeData can now be GC'd
  }
}


// 4. Uncleared timers
// ❌ Bad — setInterval runs forever, keeps callback + its closure in memory
const intervalId = setInterval(() => {
  processItems(items)
}, 1000)
// If you navigate away without clearing this, it runs forever

// ✅ Good — always clear intervals when done
clearInterval(intervalId)


// 5. Detached DOM nodes
// ❌ Bad — DOM node removed from page but still referenced in JS
let detachedDiv = document.createElement("div")
document.body.appendChild(detachedDiv)
document.body.removeChild(detachedDiv)
// detachedDiv variable still holds reference → memory leak!

// ✅ Good
detachedDiv = null  // release the reference


// ─── WeakRef & FinalizationRegistry (ES2021) ──────────────────────────────
// WeakRef — hold a reference to an object WITHOUT preventing GC
const cache = new Map()
function cacheExpensiveObject(key, obj) {
  cache.set(key, new WeakRef(obj))
}
function getCached(key) {
  const ref = cache.get(key)
  return ref?.deref()  // returns object if still alive, undefined if GC'd
}

// FinalizationRegistry — run a callback AFTER an object is GC'd
const registry = new FinalizationRegistry((key) => {
  cache.delete(key)  // clean up cache entry after object is GC'd
  console.log(`${key} was garbage collected`)
})
registry.register(obj, "myKey")


// ─── Memory profiling (in browser DevTools) ───────────────────────────────
// Chrome DevTools → Memory tab
// → Heap snapshot: see all objects in memory and their sizes
// → Allocation timeline: see when memory was allocated over time
// → Allocation sampling: low-overhead profiling for production


// ─── Tips to avoid memory leaks ───────────────────────────────────────────
// 1. Always clear setInterval/setTimeout when no longer needed
// 2. Remove event listeners when elements are removed from the DOM
// 3. Set variables to null when you are done with large objects
// 4. Use WeakMap/WeakSet for caching objects tied to DOM elements
// 5. Avoid storing too much state in closures
// 6. Use browser DevTools Memory tab to detect leaks in production
```

---

## 48. Strict Mode

```javascript
// ─── What is Strict Mode? ─────────────────────────────────────────────────
// Strict mode makes JavaScript more strict — it catches common mistakes
// and prevents the use of error-prone features

// Enable for an entire file (must be FIRST line)
"use strict"

// Enable for a single function only
function strictFn() {
  "use strict"
  // strict rules apply inside here only
}

// ES Modules and Classes are ALWAYS in strict mode automatically
// You don't need to write "use strict" in .mjs files or inside class bodies


// ─── What Strict Mode changes ─────────────────────────────────────────────

// 1. Undeclared variables throw ReferenceError (no accidental globals)
// ❌ Without strict:
x = 10          // creates a global variable silently!

// ✅ With strict:
"use strict"
x = 10          // ReferenceError: x is not defined


// 2. Writing to read-only properties throws TypeError
"use strict"
const obj = Object.freeze({ name: "Alice" })
obj.name = "Bob"  // TypeError: Cannot assign to read only property


// 3. Deleting non-deletable properties throws TypeError
delete Object.prototype  // TypeError in strict mode


// 4. Duplicate parameter names throw SyntaxError
// ❌ function bad(a, a) { }  // silently ignored without strict
"use strict"
// function bad(a, a) { }    // SyntaxError: Duplicate parameter name


// 5. `this` is undefined in regular functions (not window/global)
"use strict"
function regularFn() {
  console.log(this)   // undefined (not window!)
}
regularFn()


// 6. `with` statement is forbidden (was a bad feature)
// with (obj) { ... }  // SyntaxError in strict mode


// 7. Octal literals are forbidden
// let x = 010  // SyntaxError in strict — use 0o10 instead
let x = 0o10   // ✅ explicit octal syntax


// 8. Reserved words cannot be used as variable names
// let implements = 1  // SyntaxError — reserved for future use
// let interface = 2   // SyntaxError


// ─── Should you always use strict mode? ───────────────────────────────────
// Yes — always! If you use ES Modules (import/export) or Classes, you already are.
// For older scripts, add "use strict" at the top of every file.
// It catches real bugs and makes code safer and faster (engines optimize better).
```

---

## 49. Short-Circuit Evaluation & Logical Assignment

```javascript
// ─── Short-Circuit Evaluation ─────────────────────────────────────────────
// JavaScript evaluates left to right and STOPS as soon as the result is determined

// AND (&&) — stops at first FALSY value, returns that value
// If all are truthy, returns the LAST value
false && "hello"      // false  (stopped at false — never evaluated "hello")
0 && "hello"          // 0      (stopped at 0)
"hello" && "world"    // "world" (both truthy — returns last)
null && doSomething() // null    (doSomething() is NEVER called!)

// OR (||) — stops at first TRUTHY value, returns that value
// If all are falsy, returns the LAST value
false || "hello"      // "hello" (false is falsy, tries next)
null || undefined     // undefined (both falsy, returns last)
"hello" || "world"    // "hello" (stopped at first truthy)
"" || "default"       // "default"


// ─── Practical short-circuit patterns ─────────────────────────────────────

// Default value with ||
const name = userInput || "Guest"

// Guard clause — only call if exists
user && user.save()
// same as: if (user) { user.save() }

// Conditional execution
isLoggedIn && renderDashboard()

// Chaining optional operations
const city = user && user.address && user.address.city  // old style
const city2 = user?.address?.city                       // modern (use this)


// ─── Logical Assignment Operators (ES2021) ────────────────────────────────

// ||= (Logical OR assignment)
// Assign ONLY if current value is FALSY
let a = null
a ||= "default"           // a = "default" (was falsy)
let b = "existing"
b ||= "default"           // b = "existing" (was truthy, no change)
// Equivalent to: a = a || "default"


// &&= (Logical AND assignment)
// Assign ONLY if current value is TRUTHY
let user = { name: "Alice" }
user &&= { ...user, lastSeen: Date.now() }  // user = updated object (was truthy)
let guest = null
guest &&= { lastSeen: Date.now() }          // guest = null (was falsy, no change)
// Equivalent to: user = user && { ...user, lastSeen: Date.now() }


// ??= (Nullish assignment)
// Assign ONLY if current value is null or undefined
let config = null
config ??= { port: 3000 }     // config = { port: 3000 } (was null)
let port = 0
port ??= 3000                  // port = 0 (0 is NOT null/undefined, no change!)
// Equivalent to: config = config ?? { port: 3000 }


// ─── Comparison: ||= vs ??= ───────────────────────────────────────────────
let count1 = 0
count1 ||= 10    // count1 = 10 — 0 is falsy!

let count2 = 0
count2 ??= 10    // count2 = 0 — 0 is NOT null/undefined, no change

// Key rule: use ??= when 0, false, or "" are valid values you want to keep
```

---

## 50. Callbacks vs Promises vs Async/Await — Full Comparison

```javascript
// ─── Same task written all 3 ways ─────────────────────────────────────────
// Task: fetch user → fetch their orders → calculate total


// 1. CALLBACKS (old way — avoid) ───────────────────────────────────────────
// Problem: Deeply nested = "callback hell" = hard to read and error-handle
function getUser(id, callback) { /* ... */ }
function getOrders(userId, callback) { /* ... */ }

getUser(1, function(error, user) {
  if (error) {
    console.error(error)
    return
  }
  getOrders(user.id, function(error, orders) {
    if (error) {
      console.error(error)
      return
    }
    calculateTotal(orders, function(error, total) {
      if (error) {
        console.error(error)
        return
      }
      console.log("Total:", total)
    })
  })
})
// Every level needs its own error handling — easily mishandled
// Can't use try/catch
// Can't use return to exit
// Logic flows diagonally (pyramid of doom)


// 2. PROMISES (better) ─────────────────────────────────────────────────────
// Flat chaining — one .catch() handles all errors
getUser(1)
  .then(user => getOrders(user.id))      // returns new Promise
  .then(orders => calculateTotal(orders)) // chain continues
  .then(total => console.log("Total:", total))
  .catch(error => console.error(error))   // ONE catch for all errors
  .finally(() => console.log("Done"))

// But — complex branching logic can still get messy
// "What if step 2 fails but step 3 should still run?"
// Requires nesting inside .then() to share variables between steps


// 3. ASYNC/AWAIT (best — use this) ────────────────────────────────────────
// Looks synchronous, is actually asynchronous
async function processUserOrder(userId) {
  try {
    const user = await getUser(userId)         // wait for user
    const orders = await getOrders(user.id)    // wait for orders
    const total = await calculateTotal(orders) // wait for total
    console.log("Total:", total)
    return total
  } catch (error) {
    console.error("Failed:", error.message)    // ONE catch for all
    throw error
  } finally {
    console.log("Done")
  }
}

// Easy to use conditional logic between steps
async function processOrder(userId) {
  const user = await getUser(userId)
  if (user.isPremium) {
    const discount = await getPremiumDiscount(user.id)  // only for premium
    return applyDiscount(discount)
  }
  return standardPrice()
}


// ─── Full Comparison Table ────────────────────────────────────────────────
// Feature            | Callbacks      | Promises          | Async/Await
// Readability        | Poor (nested)  | Good (flat chain) | Best (linear)
// Error handling     | Manual + error | .catch()          | try/catch
// Debugging          | Hard           | Medium            | Easy (stack trace)
// Conditional logic  | Complex        | Messy nesting     | Easy (if/else)
// Parallel execution | Hard           | Promise.all()     | await Promise.all()
// Return values      | Via callback   | .then() chain     | return keyword
// Modern?            | Old, avoid     | Yes, standard     | Best practice


// ─── Parallel vs Sequential ───────────────────────────────────────────────

// ❌ Sequential (slow) — each waits for previous to finish
async function loadPageSlow() {
  const user    = await fetchUser()    // 300ms
  const posts   = await fetchPosts()  // 200ms   — starts AFTER user finishes
  const friends = await fetchFriends() // 150ms  — starts AFTER posts finishes
  // Total: 650ms
}

// ✅ Parallel (fast) — all start at the same time
async function loadPageFast() {
  const [user, posts, friends] = await Promise.all([
    fetchUser(),    // 300ms
    fetchPosts(),   // 200ms  — starts immediately, runs in parallel
    fetchFriends()  // 150ms  — starts immediately, runs in parallel
  ])
  // Total: 300ms (only as long as the slowest one)
}

// ✅ Parallel with error tolerance — continue even if some fail
async function loadPageTolerant() {
  const results = await Promise.allSettled([fetchUser(), fetchPosts(), fetchFriends()])
  const [userResult, postsResult, friendsResult] = results
  const user  = userResult.status  === "fulfilled" ? userResult.value  : null
  const posts = postsResult.status === "fulfilled" ? postsResult.value : []
}
```

---

## 51. Debounce & Throttle

```javascript
// ─── The Problem ──────────────────────────────────────────────────────────
// Some events fire VERY rapidly: keyup, resize, scroll, mousemove
// Running expensive operations on every event causes performance problems

// Example: Search API called on every keypress
input.addEventListener("keyup", () => {
  searchAPI(input.value)  // ❌ called on EVERY keystroke — too many requests!
})


// ─── DEBOUNCE — wait until user STOPS, then fire ONCE ─────────────────────
// Use when: search inputs, form validation, window resize handler
// "I'll fire only after you've stopped for X milliseconds"

function debounce(fn, delay) {
  let timeoutId

  return function(...args) {
    clearTimeout(timeoutId)         // cancel previous timer

    timeoutId = setTimeout(() => {
      fn.apply(this, args)          // fire only after delay with NO new calls
    }, delay)
  }
}

// Usage
const debouncedSearch = debounce((query) => {
  console.log("Searching for:", query)
  searchAPI(query)
}, 300)

input.addEventListener("keyup", (e) => {
  debouncedSearch(e.target.value)   // ✅ only fires 300ms after user STOPS typing
})

// User types "javascript":
// j         → timer set (300ms)
// ja        → timer RESET (300ms)
// jav       → timer RESET (300ms)
// ...
// javascript → timer RESET (300ms)
// [300ms pause] → searchAPI("javascript") fires ONCE ✅


// ─── THROTTLE — fire at most once per X milliseconds ──────────────────────
// Use when: scroll events, mousemove, game loops, rate-limiting API calls
// "I'll fire immediately, but then wait X ms before I'm ready to fire again"

function throttle(fn, limit) {
  let lastCall = 0

  return function(...args) {
    const now = Date.now()
    if (now - lastCall >= limit) {
      lastCall = now
      return fn.apply(this, args)   // fire if enough time has passed
    }
    // otherwise ignore the call
  }
}

// Usage
const throttledScroll = throttle(() => {
  console.log("Scroll position:", window.scrollY)
  updateProgressBar()
}, 200)

window.addEventListener("scroll", throttledScroll)
// ✅ updateProgressBar() fires at most once every 200ms no matter how fast user scrolls


// ─── Debounce vs Throttle ─────────────────────────────────────────────────
// Debounce:  fires ONCE after activity STOPS  → best for: search, resize, save-on-type
// Throttle:  fires at REGULAR INTERVALS while active → best for: scroll, drag, mouse


// ─── Leading vs Trailing ──────────────────────────────────────────────────
// Trailing debounce (default above): fires AFTER the wait period
// Leading debounce: fires IMMEDIATELY on first call, ignores subsequent calls during wait

function debounceLeading(fn, delay) {
  let timeoutId
  let hasRun = false

  return function(...args) {
    if (!hasRun) {
      fn.apply(this, args)    // fire immediately on first call
      hasRun = true
    }
    clearTimeout(timeoutId)
    timeoutId = setTimeout(() => {
      hasRun = false          // allow next call after delay
    }, delay)
  }
}

// In practice: use lodash's _.debounce / _.throttle which support both options
// import { debounce, throttle } from "lodash"
```

---

## 52. Deep vs Shallow Copy

```javascript
// ─── Shallow Copy — copies only ONE level deep ────────────────────────────
// Nested objects/arrays are still SHARED (same reference)

const original = {
  name: "Alice",
  age: 28,
  address: { city: "Mumbai", zip: "400001" },  // nested object
  hobbies: ["reading", "coding"]               // nested array
}

// Shallow copy methods:
const copy1 = { ...original }                // spread
const copy2 = Object.assign({}, original)   // Object.assign

copy1.name = "Bob"                  // ✅ doesn't affect original (primitive)
copy1.address.city = "Delhi"        // ❌ DOES affect original! (shared reference)
copy1.hobbies.push("gaming")        // ❌ DOES affect original! (shared reference)

console.log(original.address.city)  // "Delhi" ← changed!
console.log(original.hobbies)       // ["reading", "coding", "gaming"] ← changed!


// ─── Deep Copy — copies ALL levels completely ─────────────────────────────
// Nested objects/arrays are independent copies

// Method 1: JSON.parse(JSON.stringify()) — simple but has limitations
const deepCopy1 = JSON.parse(JSON.stringify(original))
deepCopy1.address.city = "Pune"
console.log(original.address.city)  // "Mumbai" ← unchanged ✅

// JSON method LIMITATIONS — does NOT handle:
// ❌ Functions (they are stripped out)
// ❌ undefined values (stripped out)
// ❌ Date objects (converted to string)
// ❌ Map, Set, WeakMap (converted to {})
// ❌ Circular references (throws error)
// ❌ Symbol keys (stripped out)

const withDate = { name: "Alice", created: new Date() }
const jsonCopy = JSON.parse(JSON.stringify(withDate))
console.log(typeof jsonCopy.created)  // "string" — not a Date! ❌


// Method 2: structuredClone() — modern, handles most cases (ES2022)
const deepCopy2 = structuredClone(original)
deepCopy2.address.city = "Bangalore"
console.log(original.address.city)   // "Mumbai" ← unchanged ✅

// structuredClone HANDLES:
// ✅ Nested objects and arrays
// ✅ Date objects (stays as Date)
// ✅ Map and Set
// ✅ ArrayBuffer, TypedArray
// ✅ Circular references!

// structuredClone DOES NOT handle:
// ❌ Functions (throws DataCloneError)
// ❌ DOM nodes
// ❌ Class instances lose their methods (only data is copied)

const withFn = { name: "Alice", greet: () => "hello" }
// structuredClone(withFn)  // ❌ DataCloneError: greet() cannot be cloned


// Method 3: Recursive manual deep clone (handles everything you define)
function deepClone(obj, visited = new WeakMap()) {
  if (obj === null || typeof obj !== "object") return obj  // primitive
  if (visited.has(obj)) return visited.get(obj)  // circular reference

  if (obj instanceof Date) return new Date(obj)
  if (obj instanceof RegExp) return new RegExp(obj)
  if (obj instanceof Map) {
    const copy = new Map()
    visited.set(obj, copy)
    obj.forEach((v, k) => copy.set(deepClone(k, visited), deepClone(v, visited)))
    return copy
  }
  if (obj instanceof Set) {
    const copy = new Set()
    visited.set(obj, copy)
    obj.forEach(v => copy.add(deepClone(v, visited)))
    return copy
  }

  const copy = Array.isArray(obj) ? [] : Object.create(Object.getPrototypeOf(obj))
  visited.set(obj, copy)
  for (const key of Object.keys(obj)) {
    copy[key] = deepClone(obj[key], visited)
  }
  return copy
}


// ─── Quick Decision Guide ─────────────────────────────────────────────────
// Primitives only                → any copy method (all are deep by default)
// Shallow — top-level change     → { ...obj } or Object.assign
// Deep — nested data, no fns     → structuredClone() (modern) or JSON hack (legacy)
// Deep — with functions/classes  → custom deepClone() or lodash _.cloneDeep()


// ─── Array copying ────────────────────────────────────────────────────────
const arr = [[1, 2], [3, 4]]

const shallowArr = [...arr]              // shallow — inner arrays still shared
const deepArr = structuredClone(arr)     // deep — inner arrays are independent

shallowArr[0].push(99)
console.log(arr[0])       // [1, 2, 99] ← affected! shallow copy

deepArr[0].push(99)
console.log(arr[0])       // [1, 2] ← not affected! deep copy
```

---

## 53. Functional Programming Concepts

```javascript
// Functional Programming (FP) is a style of writing code where:
// - Functions are the primary building blocks
// - Data is immutable — you never modify, you transform
// - No side effects in pure functions
// - Composition over inheritance


// ─── 1. Pure Functions ────────────────────────────────────────────────────
// A pure function:
// - Given same inputs, ALWAYS returns same output
// - Has NO side effects (doesn't modify anything outside itself)

// ✅ Pure
function add(a, b) { return a + b }
function double(arr) { return arr.map(n => n * 2) }  // returns new array

// ❌ Impure — depends on external state
let total = 0
function addToTotal(n) { total += n }  // modifies external variable

// ❌ Impure — side effect (I/O)
function getUser(id) {
  const user = db.query(id)  // database call — different results each time
  return user
}

// Why care? Pure functions are:
// → Predictable and easy to test (no setup/teardown needed)
// → Easy to reason about (no hidden dependencies)
// → Cacheable/memoizable (same input → same output, always)


// ─── 2. Immutability ──────────────────────────────────────────────────────
// Never mutate — always create a new version with the change

// ❌ Mutating
const user = { name: "Alice", age: 28 }
user.age = 29                          // modifies original

// ✅ Immutable update
const updatedUser = { ...user, age: 29 }  // new object, original untouched

// ❌ Mutating array
const items = ["a", "b", "c"]
items.push("d")                        // modifies original

// ✅ Immutable array operations
const newItems = [...items, "d"]       // new array
const removed  = items.filter(i => i !== "b")  // new array without "b"
const updated  = items.map((i, idx) => idx === 1 ? "B" : i)  // new array with "B"


// ─── 3. Function Composition ──────────────────────────────────────────────
// Build complex operations from simple, single-purpose functions

const trim   = str => str.trim()
const lower  = str => str.toLowerCase()
const split  = str => str.split(" ")
const unique = arr => [...new Set(arr)]
const sort   = arr => [...arr].sort()

// Manual composition
const processText = str => sort(unique(split(lower(trim(str)))))

// pipe — left to right (most readable)
const pipe = (...fns) => x => fns.reduce((v, fn) => fn(v), x)
const processText2 = pipe(trim, lower, split, unique, sort)

processText2("  Hello World hello  ")  // ["hello", "world"]


// ─── 4. Point-Free Style ──────────────────────────────────────────────────
// Write functions without explicitly mentioning the data they operate on

const numbers = [1, 2, 3, 4, 5]

// Not point-free
numbers.filter(n => n % 2 === 0)

// Point-free
const isEven = n => n % 2 === 0
numbers.filter(isEven)          // cleaner, reusable, testable


// ─── 5. Avoiding Side Effects ─────────────────────────────────────────────
// Side effects: modifying external state, I/O, DOM changes, API calls
// Strategy: push side effects to the edges of your program — keep core logic pure

// ❌ Side effect in the middle of logic
function calculateTax(price) {
  const tax = price * 0.18
  document.getElementById("tax").textContent = tax  // side effect!
  return price + tax
}

// ✅ Separate pure logic from side effects
function calculateTax(price) {
  return price * 1.18    // pure — just math
}

function displayTax(price) {
  const total = calculateTax(price)
  document.getElementById("tax").textContent = total * 0.18  // side effect isolated here
}


// ─── 6. Partial Application ───────────────────────────────────────────────
// Pre-fill some arguments of a function, creating a more specific function

function multiply(a, b) { return a * b }

// Using bind for partial application
const double = multiply.bind(null, 2)    // a = 2, b is free
const triple = multiply.bind(null, 3)    // a = 3, b is free
double(5)   // 10
triple(5)   // 15

// Using closures
const discount = (percent) => (price) => price * (1 - percent / 100)
const tenPercent = discount(10)
const twentyPercent = discount(20)
tenPercent(100)    // 90
twentyPercent(100) // 80
```

---

## 54. Type Conversion Cheat Sheet

```javascript
// ─── TO NUMBER ────────────────────────────────────────────────────────────
Number("42")          // 42
Number("3.14")        // 3.14
Number("")            // 0
Number(" ")           // 0
Number("42abc")       // NaN
Number(true)          // 1
Number(false)         // 0
Number(null)          // 0
Number(undefined)     // NaN
Number([])            // 0
Number([3])           // 3
Number([1,2])         // NaN
Number({})            // NaN

parseInt("42px")      // 42   — stops at first non-numeric
parseInt("0xFF", 16)  // 255  — parse hex
parseInt("111", 2)    // 7    — parse binary
parseFloat("3.14abc") // 3.14

+"42"                 // 42   — unary plus (fast conversion)
+true                 // 1
+false                // 0
+null                 // 0
+undefined            // NaN
+""                   // 0


// ─── TO STRING ────────────────────────────────────────────────────────────
String(42)            // "42"
String(3.14)          // "3.14"
String(true)          // "true"
String(false)         // "false"
String(null)          // "null"
String(undefined)     // "undefined"
String([1,2,3])       // "1,2,3"
String({})            // "[object Object]"

(42).toString()       // "42"
(255).toString(16)    // "ff"   — to hex
(255).toString(2)     // "11111111" — to binary
(7).toString(8)       // "7"    — to octal

42 + ""               // "42"  — implicit via concatenation
`${42}`               // "42"  — template literal


// ─── TO BOOLEAN ───────────────────────────────────────────────────────────
Boolean(0)            // false
Boolean("")           // false
Boolean(null)         // false
Boolean(undefined)    // false
Boolean(NaN)          // false
Boolean(false)        // false
Boolean(1)            // true
Boolean("hello")      // true
Boolean([])           // true  ← surprising!
Boolean({})           // true  ← surprising!
Boolean(-1)           // true
Boolean("0")          // true  ← non-empty string!

!!0                   // false — double NOT for quick boolean conversion
!!"hello"             // true
!!null                // false


// ─── EQUALITY TYPE COERCION (why == is dangerous) ─────────────────────────
0 == false            // true  (false → 0)
1 == true             // true  (true → 1)
"" == false           // true  (both → 0)
"0" == false          // true  ("0" → 0, false → 0)
null == undefined     // true  (special case)
null == 0             // false (null only equals null or undefined)
NaN == NaN            // false (NaN is never equal to anything, even itself!)
[] == false           // true  ([] → "" → 0, false → 0)
[] == ![]             // true  (one of JS's most confusing quirks)

// Just use === and avoid all of this confusion
```

---

## 55. Common JavaScript Mistakes & How to Avoid Them

```javascript
// ─── 1. Using == instead of === ────────────────────────────────────────────
// ❌
if (userAge == "18") { }    // coerces string to number — confusing
// ✅
if (userAge === 18) { }     // explicit, no surprises


// ─── 2. Not handling async errors ──────────────────────────────────────────
// ❌ Unhandled promise rejection
async function loadData() {
  const data = await fetchData()  // if this throws, error silently swallowed!
}

// ✅ Always use try/catch with async/await
async function loadData() {
  try {
    const data = await fetchData()
    return data
  } catch (error) {
    console.error("Failed to load:", error)
    throw error  // re-throw so caller knows
  }
}


// ─── 3. Mutating function arguments ────────────────────────────────────────
// ❌ Mutating an object passed to a function
function addRole(user) {
  user.role = "admin"   // mutates the original object!
  return user
}

// ✅ Return a new object
function addRole(user) {
  return { ...user, role: "admin" }
}


// ─── 4. Forgetting that sort() is alphabetical by default ──────────────────
// ❌
[10, 9, 2, 1, 100].sort()   // [1, 10, 100, 2, 9] ← wrong! sorted as strings

// ✅
[10, 9, 2, 1, 100].sort((a, b) => a - b)  // [1, 2, 9, 10, 100] ← correct


// ─── 5. NaN comparisons ────────────────────────────────────────────────────
// ❌ NaN is never equal to anything — even itself!
const result = NaN
if (result === NaN) { }      // always false!
if (result == NaN)  { }      // always false!

// ✅
if (isNaN(result)) { }       // true
if (Number.isNaN(result)) { } // more strict — only true for actual NaN (preferred)
Number.isNaN("hello")         // false (string is not NaN)
isNaN("hello")                // true  (coerces first — misleading!)


// ─── 6. Accidentally creating global variables ─────────────────────────────
// ❌
function calculate() {
  result = 42    // forgot let/const — creates global variable!
}

// ✅ Use "use strict" at top of file — this will throw a ReferenceError
"use strict"
function calculate() {
  result = 42    // ReferenceError: result is not defined
}


// ─── 7. Callback in loop with var ──────────────────────────────────────────
// ❌ Classic var + setTimeout bug
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100)  // prints: 3, 3, 3
}

// ✅ Use let (creates new binding per iteration)
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100)  // prints: 0, 1, 2
}


// ─── 8. Truthy/falsy traps ─────────────────────────────────────────────────
// ❌ Using !value to check "no value" when 0 is valid
function setQuantity(qty) {
  if (!qty) throw new Error("Qty required")  // throws when qty = 0!
}

// ✅ Be explicit
function setQuantity(qty) {
  if (qty === undefined || qty === null) throw new Error("Qty required")
  // OR: if (qty == null) — the only safe use of ==
}


// ─── 9. Sequential awaits when parallel is possible ───────────────────────
// ❌ Slow — 3 sequential API calls: 300 + 200 + 150 = 650ms
async function loadPage() {
  const a = await fetchA()   // waits 300ms
  const b = await fetchB()   // waits 200ms AFTER a
  const c = await fetchC()   // waits 150ms AFTER b
}

// ✅ Fast — parallel: only as slow as the slowest = 300ms
async function loadPage() {
  const [a, b, c] = await Promise.all([fetchA(), fetchB(), fetchC()])
}


// ─── 10. Not cleaning up event listeners and timers ────────────────────────
// ❌
function setupComponent() {
  window.addEventListener("resize", handleResize)
  const timer = setInterval(update, 1000)
  // component is removed but listeners and timer keep running!
}

// ✅ Return a cleanup function
function setupComponent() {
  const handleResize = () => updateLayout()
  window.addEventListener("resize", handleResize)
  const timer = setInterval(update, 1000)

  return function cleanup() {
    window.removeEventListener("resize", handleResize)
    clearInterval(timer)
  }
}
const cleanup = setupComponent()
// later: cleanup()  // call when done


// ─── 11. Floating point arithmetic ────────────────────────────────────────
// ❌
0.1 + 0.2          // 0.30000000000000004 — not 0.3!
0.1 + 0.2 === 0.3  // false!

// ✅ Round to required decimal places
(0.1 + 0.2).toFixed(1) === "0.3"  // true
Math.round((0.1 + 0.2) * 10) / 10 // 0.3
// For money: store in integer cents (100 = $1.00) or use a decimal library


// ─── 12. Destructuring with undefined ─────────────────────────────────────
// ❌ Throws if the object is null/undefined
const { name } = getUser()   // TypeError if getUser() returns null!

// ✅ Guard first
const user = getUser()
const { name } = user ?? {}  // safe: use empty object as fallback
// OR:
const { name } = getUser() || {}
```

---

## 56. Node.js Specific Concepts

```javascript
// ─── What is Node.js? ──────────────────────────────────────────────────────
// Node.js is a JavaScript runtime built on Chrome's V8 engine
// Lets you run JavaScript OUTSIDE the browser — on servers, CLI tools, etc.
// Non-blocking I/O — handles many concurrent connections efficiently
// Uses CommonJS (require) OR ES Modules (import)


// ─── Global objects (available without import) ────────────────────────────
console.log(__filename)        // full path of current file
console.log(__dirname)         // directory of current file
console.log(process.env)       // environment variables
console.log(process.argv)      // command-line arguments
console.log(process.cwd())     // current working directory
process.exit(0)                // exit with success code
process.exit(1)                // exit with error code
process.on("uncaughtException", (err) => { console.error(err) })


// ─── process.env — environment variables ─────────────────────────────────
// In terminal:
// PORT=3000 node app.js
const port = process.env.PORT || 3000
const dbUrl = process.env.DATABASE_URL
const isProduction = process.env.NODE_ENV === "production"

// Load from .env file using dotenv:
// npm install dotenv
require("dotenv").config()          // CommonJS
// import "dotenv/config"            // ES Modules
console.log(process.env.SECRET_KEY) // reads from .env file


// ─── Built-in modules (no install needed) ────────────────────────────────
const fs   = require("fs")         // File System
const path = require("path")       // Path manipulation
const os   = require("os")         // Operating system info
const http = require("http")       // HTTP server (low-level)
const https = require("https")     // HTTPS
const url  = require("url")        // URL parsing
const crypto = require("crypto")   // Cryptography
const events = require("events")   // EventEmitter
const stream = require("stream")   // Streams
const child_process = require("child_process")  // Run shell commands


// ─── fs (File System) ─────────────────────────────────────────────────────
const fs = require("fs").promises  // use promises version

// Read a file
const content = await fs.readFile("./data.txt", "utf-8")

// Write a file (creates or overwrites)
await fs.writeFile("./output.txt", "Hello, World!", "utf-8")

// Append to a file
await fs.appendFile("./log.txt", `[${new Date().toISOString()}] Event logged\n`)

// Check if file exists
try {
  await fs.access("./config.json")
  console.log("File exists")
} catch {
  console.log("File does not exist")
}

// Read directory
const files = await fs.readdir("./src")

// Create directory
await fs.mkdir("./uploads", { recursive: true })

// Delete file
await fs.unlink("./temp.txt")

// Rename / move
await fs.rename("./old.txt", "./new.txt")

// Read JSON file (common pattern)
const config = JSON.parse(await fs.readFile("./config.json", "utf-8"))

// Write JSON file
await fs.writeFile("./config.json", JSON.stringify(config, null, 2))


// ─── path module ─────────────────────────────────────────────────────────
const path = require("path")

path.join(__dirname, "uploads", "file.png")     // "/app/uploads/file.png"
path.resolve("./src", "app.js")                 // absolute path
path.basename("/app/src/app.js")                // "app.js"
path.dirname("/app/src/app.js")                 // "/app/src"
path.extname("file.png")                        // ".png"
path.parse("/app/src/app.js")
// { root: '/', dir: '/app/src', base: 'app.js', ext: '.js', name: 'app' }


// ─── EventEmitter ─────────────────────────────────────────────────────────
const EventEmitter = require("events")

class OrderService extends EventEmitter {
  placeOrder(order) {
    // process order...
    this.emit("orderPlaced", order)   // emit an event with data
    this.emit("log", `Order ${order.id} placed`)
  }
}

const orderService = new OrderService()

// Listen to events
orderService.on("orderPlaced", (order) => {
  sendConfirmationEmail(order)
})

orderService.on("log", (message) => {
  console.log(`[LOG] ${message}`)
})

// One-time listener
orderService.once("orderPlaced", (order) => {
  console.log("First order ever placed!")
})

orderService.placeOrder({ id: 1, product: "Laptop" })


// ─── process.argv — CLI arguments ─────────────────────────────────────────
// node script.js hello world --name=Alice
console.log(process.argv)
// ["/path/to/node", "/path/to/script.js", "hello", "world", "--name=Alice"]

const args = process.argv.slice(2)    // ["hello", "world", "--name=Alice"]
const name = args.find(a => a.startsWith("--name="))?.split("=")[1]  // "Alice"


// ─── npm scripts ─────────────────────────────────────────────────────────
// package.json "scripts" field — run with npm run <name>
// "scripts": {
//   "start":   "node app.js",              → npm start
//   "dev":     "nodemon app.js",           → npm run dev
//   "test":    "jest",                     → npm test
//   "build":   "tsc",                      → npm run build
//   "lint":    "eslint .",                 → npm run lint
//   "format":  "prettier --write ."       → npm run format
// }
```

---

## 57. Updated Interview Questions — Advanced

### Q1: What is a memory leak in JavaScript and how do you prevent it?
**Answer:** A memory leak happens when your program allocates memory that is never released, causing the application to gradually consume more and more RAM until it slows down or crashes. Common causes are: global variables that are never cleared, event listeners that are added but never removed when their element is deleted, setInterval running forever without clearInterval, closures that hold large objects in scope longer than needed, and DOM nodes removed from the page but still referenced in JavaScript variables. To prevent them: always remove event listeners when no longer needed, always clearInterval when done, set large object references to null when done, use WeakMap/WeakRef for object caches, and regularly profile with Chrome DevTools Memory tab.

---

### Q2: What is debounce and throttle, and when do you use each?
**Answer:** Both are techniques to limit how often a function runs in response to rapid events. Debounce waits until the user has stopped triggering the event for a specified time, then fires once — perfect for search inputs (fire the API call only after the user stops typing for 300ms), window resize handlers, and auto-save on form input. Throttle fires the function at most once per time window regardless of how many times the event fires — perfect for scroll events (update a progress bar at most every 200ms), mousemove, and game loop updates. The key difference: debounce fires once after activity stops; throttle fires at regular intervals while activity continues.

---

### Q3: What is the difference between shallow copy and deep copy?
**Answer:** A shallow copy creates a new object at the top level but nested objects and arrays are still the same references — if you modify a nested object in the copy, the original is also modified. A deep copy creates completely independent copies at all levels — modifying the copy never affects the original. Methods for shallow copy: spread operator `{...obj}`, `Object.assign({}, obj)`. Methods for deep copy: `structuredClone(obj)` (modern, handles Date, Map, Set, circular refs), `JSON.parse(JSON.stringify(obj))` (simple but loses functions, Dates, undefined), or a custom recursive function. Use shallow copy when you only need to change top-level properties; use deep copy when you need to modify nested structures without side effects.

---

### Q4: What are pure functions and why are they important?
**Answer:** A pure function always returns the same output for the same input and has no side effects — it doesn't modify anything outside itself (no global state, no DOM changes, no API calls). They are important because they are completely predictable and easy to reason about, trivial to unit-test (no mocking of external dependencies needed), safe to run in parallel (no shared state), and easy to memoize (cache the result since input always produces same output). The core principle of functional programming is to push side effects (API calls, DOM manipulation, logging) to the edges of your application and keep the business logic in pure functions.

---

### Q5: What is the difference between `||=`, `&&=`, and `??=`?
**Answer:** All three are logical assignment operators introduced in ES2021. `||=` assigns only if the current value is falsy — `a ||= "default"` is equivalent to `a = a || "default"`. `&&=` assigns only if the current value is truthy — `user &&= { ...user, updated: true }` only updates user if user already exists. `??=` assigns only if the current value is specifically null or undefined (not 0, false, or empty string) — `count ??= 0` keeps `count = 0` unchanged while `count ||= 0` would wrongly replace a valid `0` value with the right-hand side. The `??=` operator is the safest for optional initialization because it doesn't accidentally overwrite valid falsy values.

---

### Q6: What is strict mode and what does it do?
**Answer:** Strict mode is an opt-in mode that makes JavaScript more strict and secure. You enable it by placing `"use strict"` at the top of a file or function. Note that ES Modules and class bodies are always in strict mode automatically. What it changes: undeclared variables throw a ReferenceError instead of silently creating globals, writing to read-only properties throws a TypeError, duplicate parameter names in functions are a SyntaxError, `this` inside regular function calls is `undefined` instead of the global object, the `with` statement is forbidden. Always use strict mode — it catches real bugs and allows JavaScript engines to make better optimizations.

---

### Q7: How does Node.js handle concurrency if it's single-threaded?
**Answer:** Node.js is single-threaded for JavaScript execution, but it uses a multi-threaded **libuv library** under the hood for I/O operations. When you do a file read, network request, or database query, Node.js hands it off to libuv which uses the operating system's asynchronous I/O and a thread pool for operations that can't be done asynchronously by the OS. Node.js's own thread stays free to process other requests. When the I/O completes, the callback is placed in the event queue and the event loop picks it up when the JavaScript thread is free. This makes Node.js very efficient for I/O-bound workloads (many concurrent API calls, database queries) but not ideal for CPU-bound work (heavy computation) which blocks the single JS thread.

---

### Q8: What is the Temporal Dead Zone (TDZ)?
**Answer:** The TDZ is the period between the start of a block scope and the point where a `let` or `const` variable is actually declared. During this period, the variable exists (it's been hoisted) but has NOT been initialized — accessing it throws a `ReferenceError`. For example, if you write `console.log(x)` before `let x = 5` in the same block, you get a `ReferenceError: Cannot access 'x' before initialization`. This is different from `var` which is hoisted AND initialized to `undefined`, so accessing `var x` before its declaration returns `undefined` rather than throwing. The TDZ exists to prevent the class of bugs that `var` hoisting enables — it forces you to declare variables before using them.

---

## 🔄 JavaScript — Complete Summary Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    JavaScript Runtime (Browser / Node.js)                │
│                                                                          │
│  ┌──────────────────┐     ┌─────────────────────────────────────────┐   │
│  │   Call Stack     │     │           Web APIs / Node APIs           │   │
│  │  (sync code)     │     │  setTimeout, fetch, fs, DOM events...   │   │
│  │                  │     │  (handled outside JS thread)             │   │
│  │  main()          │     └─────────────────────┬───────────────────┘   │
│  │  ├── fetchUser() │                           │ callback ready         │
│  │  └── processData │                           ▼                       │
│  └──────────────────┘     ┌─────────────────────────────────────────┐   │
│                           │  Microtask Queue (Promises .then)        │   │
│          ↑                │  Runs BEFORE macro-tasks — entire queue  │   │
│   Event Loop              └─────────────────────────────────────────┘   │
│   (when stack empty:      ┌─────────────────────────────────────────┐   │
│    1. Run all microtasks  │  Macro-task Queue (setTimeout, I/O)      │   │
│    2. Run one macro-task  │  Runs AFTER microtasks — one at a time   │   │
│    3. Repeat)             └─────────────────────────────────────────┘   │
│                                                                          │
│  ┌────────────────────────────────────────────────────────────────────┐ │
│  │                  Memory (Heap)                                      │ │
│  │  Objects, Arrays, Functions stored here                             │ │
│  │  GC marks reachable objects → sweeps unreachable ones              │ │
│  └────────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────┘
```

---

*Notes prepared based on: MDN Web Docs, You Don't Know JS (Kyle Simpson), JavaScript.info, TC39 proposals, and production best practices (2025–2026)*
