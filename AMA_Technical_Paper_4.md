# AMA Technical Paper 4
---

### **1. What are pure and impure functions?**

* A **pure function** always gives the **same output** for the same input. It doesn’t depend on external data or change any variable outside its scope. Example: `function add(a, b) { return a + b; }`
* An **impure function** may depend on or modify something **outside itself**, like updating a global variable or printing to the console.
* Pure functions are **predictable and easy to test**, while impure ones can cause unexpected results.
* In real projects, pure functions help in writing **clean, reusable, and bug-free code**.

---

### **2. What is the difference between `map()` and `forEach()`?**

* The **`map()`** method creates a **new array** by applying a function to each element, returning modified results. Example: `arr.map(x => x * 2)` doubles all values.
* The **`forEach()`** method simply **loops through** each element to perform an action but does not return a new array.
* Use `map()` when you want to **transform data**, and `forEach()` when you only want to **perform side effects** like logging or updating.
* Overall, `map()` is useful for **functional programming**, while `forEach()` is for simple iteration tasks.

---

### **3. What is the use of the spread operator (`...`)?**

* The **spread operator (`...`)** is used to **expand** elements of arrays or objects into individual items.
* It helps in **copying**, **merging**, or **passing** array elements and object properties easily. Example: `let arr2 = [...arr1];` copies an array.
* It can also be used to **combine multiple arrays or objects**: `let combined = [...arr1, ...arr2];`.
* In short, it makes the code **shorter, cleaner, and easier to read**, especially when dealing with large data structures.

---

### **4. What is the purpose of `setTimeout()` and `setInterval()`?**

* `setTimeout()` runs a function **once** after a specific delay (in milliseconds). Example: `setTimeout(() => alert("Hello"), 2000);` runs after 2 seconds.
* `setInterval()` runs a function **repeatedly** at fixed time intervals. Example: `setInterval(() => console.log("Hi"), 1000);` runs every second.
* Both functions are part of **asynchronous JavaScript**, meaning they don’t block other code from running.
* These are widely used in **timers, animations, games, and real-time updates** on web pages.

---

### **5. What is the difference between `JSON.parse()` and `JSON.stringify()`?**

* `JSON.parse()` converts **JSON text** into a **JavaScript object**, so you can access its data easily. Example: `JSON.parse('{"name":"Raj"}')` → `{name: "Raj"}`.
* `JSON.stringify()` converts a **JavaScript object** into a **JSON string**, which can be sent to a server. Example: `JSON.stringify({name: "Raj"})` → `'{"name":"Raj"}'`.
* They are essential for **data transfer between frontend and backend**, since JSON is a common web format.
* In simple words, `parse()` reads JSON data, while `stringify()` prepares it to send.

---

### **6. What is the use of the `try...catch` block?**

* The `try...catch` block is used to **handle runtime errors** in JavaScript without stopping the program.
* Code inside `try` is tested — if it throws an error, the control moves to `catch`, which safely handles it.
* Example:

  ```js
  try { console.log(x); }  
  catch (err) { console.log("Error found:", err); }
  ```
* It helps in creating **stable and user-friendly** applications that don’t crash suddenly.
* You can also use `finally` to run code that executes **no matter what happens**, such as closing files or ending connections.

---

### **7. What is the difference between `justify-content` and `align-items` in flexbox?**

* `justify-content` controls how items are **placed horizontally** along the main axis. For example, `justify-content: center;` puts items in the center of the row.
* `align-items` controls how items are **aligned vertically** along the cross-axis. For example, `align-items: center;` centers them top to bottom.
* They help in designing **flexible and responsive layouts** easily.
* Together, they make elements look **balanced and properly aligned**, improving the visual structure of web pages.
