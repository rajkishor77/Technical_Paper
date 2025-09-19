# Technical Paper: Core Concepts in CSS, Python, and Git

---

## Abstract
This paper explains common concepts from CSS, Python, and Git in very simple words. Topics include types of CSS, string manipulation, CSV handling in Python, list operations, Git remotes, Flexbox, and differences between CSS properties. Each section gives a short explanation with examples for easy understanding.

---

## 1. Types of CSS
There are **three main types of CSS** used to style web pages:

i. **Inline CSS** – Written inside the HTML element.  
   ```html
   <p style="color:red;">Hello World</p>
   ```
ii. **Internal CSS** – Written in a `<style>` block inside the HTML file.

```html
<style>
  p { color: blue; }
</style>
```
iii. **External CSS** – Written in a .css file and linked to HTML.

```html
<link rel="stylesheet" href="style.css">
```
## 2. What is String Manipulation?
String manipulation means changing or working with text.
Examples: removing spaces, changing case, replacing words.

```python
text = " Hello World "
print(text.strip())          # removes spaces
print(text.upper())          # HELLO WORLD
print(text.replace("World", "Python"))  # Hello Python
```

# 3. Reading CSV Files in Python
A CSV file stores data in rows and columns separated by commas.
In Python, we can use pandas:

```python
import pandas as pd

data = pd.read_csv("data.csv")
print(data.head())   # shows first 5 rows
```

## 4. remove(), pop(), and delete() in Python
These are used to remove items from a list:

**remove(value)**: Removes by value.

**pop(index)**: Removes by position (default is last).

**del:** Deletes by index or entire list.

```python
nums = [10, 20, 30, 40]
nums.remove(20)   # removes value 20
nums.pop(1)       # removes element at index 1
del nums[0]       # deletes element at index 0
```

## 5. Class Method vs Static Method in Python
**Class Method (@classmethod)**

Works with the class itself and can access class variables.

```python
class Demo:
    count = 0
    @classmethod
    def show(cls):
        return cls.count
```

**Static Method (@staticmethod)**

Works like a normal function inside a class. No class/instance required.

```python
class Demo:
    @staticmethod
    def add(x, y):
        return x + y
```
## 6. Remote in Git
A remote is a link to a repository stored online (like GitHub).
We use it to push our changes to share and pull updates from others.

```bash
git remote add origin https://github.com/user/repo.git
git push -u origin main
``` 

## 7. Display Flex in CSS
Flexbox (display: flex;) is a CSS layout tool.
It arranges items in a row or column and makes alignment easier.

```css
.container {
  display: flex;
  justify-content: space-between;
}
```
## 8. Difference: display: none vs visibility: hidden
| Property              | Effect                                                          |
| --------------------- | --------------------------------------------------------------- |
| `display: none;`      | Element is hidden and takes **no space** in the page layout.    |
| `visibility: hidden;` | Element is hidden but still takes **space** in the page layout. |


## 9. Difference Between Inline-block and Block
| Type             | Behavior                                                         |
| ---------------- | ---------------------------------------------------------------- |
| **Block**        | Starts on a new line, takes full width. Example: `<div>`, `<p>`. |
| **Inline-block** | Stays inline but allows width/height. Good for menus/buttons.    |

```html
<div style="display:block; background:yellow;">Block Element</div>
<span style="display:inline-block; width:120px; background:pink;">Inline-Block</span>
```

## References
MDN Web Docs on CSS

Python Official Documentation

Git Documentation