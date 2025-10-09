
# Technical Paper: AMA 5

---

## 1. Purpose of the fetch() API in JavaScript

### Definition:
The `fetch()` API in JavaScript is used to make **HTTP requests** to servers. It allows web applications to get or send data without reloading the page.

### ⚙️ How It Works:
- It returns a **Promise**, which resolves to the response of the request.
- It helps in loading data dynamically from APIs.

###  Example:
```javascript
fetch('https://api.example.com/data')
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.log('Error:', error));
````

###  Key Points:

* Used for API calls and data fetching.
* Works asynchronously using promises.
* Easier alternative to older `XMLHttpRequest`.

---

## 2. Difference Between Static and Dynamic Websites

###  Definition:

Websites can be classified as **static** or **dynamic** based on how their content is delivered to users.

###  Static Websites:

* Show the **same content** to every visitor.
* Built using only **HTML, CSS, and basic JavaScript**.
* Example: Portfolio sites, company landing pages.

###  Dynamic Websites:

* Content **changes** based on user interaction or data.
* Use **server-side languages** like PHP, Node.js, or Python.
* Example: Facebook, Gmail, E-commerce sites.

###  Key Difference Table:

| Feature | Static Website | Dynamic Website     |
| ------- | -------------- | ------------------- |
| Content | Fixed          | Changes dynamically |
| Backend | Not required   | Required            |
| Speed   | Faster         | Slightly slower     |
| Example | Portfolio      | Social media site   |

---

## 3. What Are Cookies and How Are They Used?

###  Definition:

Cookies are small text files stored in the browser by websites to **remember user information** and settings.

### Purpose:

* Track user sessions.
* Store login details or preferences.
* Manage shopping carts and personalization.

###  Example:

```javascript
document.cookie = "username=Raj; expires=Fri, 31 Dec 2025 12:00:00 UTC; path=/";
```

###  Key Points:

* Stored on the client side.
* Helps maintain continuity between visits.
* Can be deleted or blocked for privacy.

---

## 4. What Are Callback Functions?

###  Definition:

A **callback function** is a function passed as an argument to another function to be executed later.

###  Purpose:

Used for **asynchronous operations** like fetching data, event handling, or time delays.

###  Example:

```javascript
function greet(name, callback) {
  console.log('Hello, ' + name);
  callback();
}

function bye() {
  console.log('Goodbye!');
}

greet('Raj', bye);
```

###  Key Points:

* Helps control the order of execution.
* Commonly used in event listeners and async tasks.
* Foundation for Promises and async/await.

---

## 5. Difference Between innerHTML and textContent

###  Definition:

Both are JavaScript properties used to get or set the content inside an HTML element.

###  innerHTML:

* Returns or sets HTML content including tags.
* Can be used to insert HTML elements dynamically.

###  textContent:

* Returns or sets only **plain text**, without tags.

###  Example:

```javascript
document.getElementById("demo").innerHTML = "<b>Hello</b>"; // Bold text
document.getElementById("demo").textContent = "<b>Hello</b>"; // Shows tags as text
```

###  Key Points:

* `innerHTML` allows HTML rendering.
* `textContent` is safer (no HTML injection).

---

## 6. Purpose of the “this” Keyword

###  Definition:

The `this` keyword refers to the **object that owns the current code** being executed.

###  Behavior:

* In a method, `this` refers to the **object**.
* In global scope, it refers to the **window** (browser).

###  Example:

```javascript
const person = {
  name: "Raj",
  greet: function() {
    console.log("Hello " + this.name);
  }
};

person.greet(); // Hello Raj
```

###  Key Points:

* Value of `this` depends on where and how it’s called.
* Arrow functions don’t have their own `this`.

---

## 7. Difference Between Margin and Padding

###  Definition:

Both **margin** and **padding** are CSS properties that create space, but in different areas.

###  Margin:

* Adds space **outside** the element’s border.
* Affects distance between elements.

###  Padding:

* Adds space **inside** the element’s border.
* Affects space around content.

###  Example:

```css
div {
  margin: 20px;  /* Outer space */
  padding: 10px; /* Inner space */
}
```

###  Key Points:

| Property | Affects        | Position    |
| -------- | -------------- | ----------- |
| Margin   | Outside border | Outer space |
| Padding  | Inside border  | Inner space |

---

## 8. Purpose of z-index in CSS

###  Definition:

The `z-index` property controls the **stacking order** of overlapping elements.

###  How It Works:

* Higher `z-index` values appear **on top**.
* Works only on positioned elements (`relative`, `absolute`, `fixed`, etc.).

###  Example:

```css
.box1 { position: absolute; z-index: 1; }
.box2 { position: absolute; z-index: 2; } /* On top of box1 */
```

###  Key Points:

* Controls visual layering of elements.
* Useful in modals, popups, and overlapping layouts.

---

#  References

* [MDN Web Docs – JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
* [W3Schools – HTML & CSS](https://www.w3schools.com)
* [CSS Tricks – z-index Guide](https://css-tricks.com/)

