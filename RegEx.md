# 📘 Technical Paper of Python Regular Expression or RegEx

## Python RegEx
A Regular Expression or RegEx is a special sequence of characters that uses a search pattern to find a string or set of strings.

It can detect the presence or absence of a text by matching it with a particular pattern and also can split a pattern into one or more sub-patterns.

## Regex Module in Python
Python has a built-in module named "re" that is used for regular expressions in Python. We can import this module by using import statement.

Importing re module in Python using following command:
```python
import re
```
## How to Use RegEx in Python?
You can use RegEx in Python after importing **re** module.

**Example:**

This Python code uses regular expressions to search for the word "portal" in the given string and then prints the start and end indices of the matched word within the string.


```python
import re

s = 'GeeksforGeeks: A computer science portal for geeks'
match = re.search(r'portal', s)
​
print('Start Index:', match.start())
print('End Index:', match.end())
```
```
Output:
Start Index: 34
End Index: 40
```


---
# 🔑 Useful Regular Expression Functions in Python

The `re` module in Python provides powerful tools for working with **regular expressions**.  
Below are the most **useful functions** along with explanations and examples.

---

### 1. `re.match(pattern, string)`
- **Purpose:** Match only at the **beginning** of the string.  
- **Returns:** Match object if found, else `None`.

**Example:**
```python
import re
text = "Hello World"
m = re.match(r"Hello", text)
print(m.group())  # Output: Hello
```
### 2. `re.search(pattern, string)`
**Purpose:** Search the entire string for the first occurrence.
**Returns:** Match object if found, else None.

**Example:**
```python
s = re.search(r"World", "Say Hello World")
print(s.group())  # Output: World
```
### 3. `re.findall(pattern, string)`
**Purpose:** Returns a list of all matches in the string.

**Returns:** List of strings.

**Example:**
```python
f = re.findall(r"\d+", "My numbers are 42 and 365")
print(f)  # Output: ['42', '365']
```
### 4. `re.finditer(pattern, string)`
**Purpose:** Returns an iterator of match objects.

**Returns:** Iterator of Match objects.

**Example:**
```python
for match in re.finditer(r"\d+", "My numbers are 42 and 365"):
    print(match.group(), "at", match.span())
# Output:
# 42 at (14, 16)
# 365 at (21, 24)
```
### 5. `re.split(pattern, string)`
**Purpose:** Split a string by regex pattern.

**Returns:** List of strings.

**Example:**
```python
print(re.split(r"[;,\s]+", "apple;banana orange,grape"))
# Output: ['apple', 'banana', 'orange', 'grape']
```
### 6. `re.sub(pattern, repl, string)`
**Purpose:** Replace all matches with another string.

**Returns:** New string.

**Example:**
```python
print(re.sub(r"cat", "dog", "cat bat cat"))
# Output: dog bat dog
```
### 7. `re.subn(pattern, repl, string)`
**Purpose:** Works like sub() but also returns the number of replacements.

**Returns:** Tuple (new string, number of replacements).

**Example:**
```python
print(re.subn(r"cat", "dog", "cat bat cat"))
# Output: ('dog bat dog', 2)
```
### 8. `re.compile(pattern)`
**Purpose:** Compiles a regex pattern into a regex object (faster for reuse).

**Returns:** Compiled regex object.

**Example:**
```python
pattern = re.compile(r"\d+")
print(pattern.findall("123 abc 456"))  # Output: ['123', '456']
```
### 9. `re.fullmatch(pattern, string)`
**Purpose:** Match the entire string to the pattern.

**Returns:** Match object if fully matched, else None.

**Example:**
```python
print(re.fullmatch(r"\d+", "12345"))   # Match object
print(re.fullmatch(r"\d+", "123a"))    # None
```
### 10.  `re.escape(string)`
**Purpose:** Escapes all special regex characters.

**Returns:** Safe string.

**Example:**
```python
print(re.escape("Hello. How are you?"))
# Output: Hello\. How\ are\ you\?
```
---

# 📊 Summary Table
| Function         | Purpose                                 | Example Output                    |
| ---------------- | --------------------------------------- | --------------------------------- |
| `re.match()`     | Match only at the start of the string   | `"Hello"`                         |
| `re.search()`    | Find first match in the entire string   | `"World"`                         |
| `re.findall()`   | Get all matches as a list               | `['bat', 'mat', 'rat']`           |
| `re.finditer()`  | Get matches as iterator with positions  | `(4,8), (15,19)`                  |
| `re.sub()`       | Replace all matches                     | `"banana orange banana"`          |
| `re.subn()`      | Replace + return count of substitutions | `('banana banana orange', 2)`     |
| `re.split()`     | Split string by regex pattern           | `['one', 'two', 'three', 'four']` |
| `re.compile()`   | Compile regex pattern for reuse         | `['123', '456']`                  |
| `re.escape()`    | Escape special regex characters         | `"Hello\. How\ are\ you\?"`       |
| `re.fullmatch()` | Match entire string                     | `"12345"` ✅, `"123a"` ❌           |


# 🔍 Python Regex: Match Object & Pattern Object

When you use `re.match()`, `re.search()`, or `re.finditer()`, they return a **Match object** if a match is found.  
When you use `re.compile()`, you get a **Pattern object** that stores regex information.

---
---
## 🎯 Match Object Methods

### 1. `.group([n])`
**Definition:** Returns the matched string.  
- `group(0)` → entire match  
- `group(1), group(2)...` → specific capturing groups  
```python
import re
text = "My number is 12345"
m = re.search(r"(\d+)", text)
print(m.group(0))   # Output: 12345 (entire match)
print(m.group(1))   # Output: 12345 (first group)
```
### 2. `.groups()`
**Definition:** Returns all captured groups as a tuple.
```python
m = re.search(r"(\w+) (\w+)", "Hello World")
print(m.groups())   # Output: ('Hello', 'World')
```
### 3. .groupdict()
**Definition:** Returns groups as a dictionary if groups are named.
```python
m = re.search(r"(?P<first>\w+) (?P<last>\w+)", "John Doe")
print(m.groupdict())   # Output: {'first': 'John', 'last': 'Doe'}
```
### 4. `.start()`
**Definition:** Returns the starting index of the match.
```python
m = re.search(r"World", "Hello World")
print(m.start())   # Output: 6
```
### 5. `.end()`
**Definition:** Returns the ending index of the match (1 past the last character).
```python
m = re.search(r"World", "Hello World")
print(m.end())   # Output: 11
```
### 6. `.span()`
**Definition:** Returns a tuple (start, end).
```python
m = re.search(r"World", "Hello World")
print(m.span())   # Output: (6, 11)
```
### 7. `.string`
Definition: Returns the original string that was searched.
```python
m = re.search(r"World", "Hello World")
print(m.string)   # Output: Hello World
```
### 8. `.re`
**Definition:** Returns the regex pattern object used.
```python
pattern = re.compile(r"World")
m = pattern.search("Hello World")
print(m.re)       # Output: re.compile('World')
```
---

---
# ⚙️ Pattern Object Attributes
When you use re.compile(), you get a Pattern object.
This object has attributes describing the compiled regex.

### 1. .pattern
**Definition:** The original regex string used to compile.
```python
pattern = re.compile(r"\d+")
print(pattern.pattern)   # Output: \d+
```
### 2. .flags
**Definition:** Shows regex flags (like IGNORECASE, MULTILINE).
```python
pattern = re.compile(r"hello", re.IGNORECASE)
print(pattern.flags)     # Output: 2 (represents IGNORECASE flag)
```
### 3. .groups
**Definition:** Number of capturing groups in the regex.
```python
pattern = re.compile(r"(\d+)-(\d+)")
print(pattern.groups)    # Output: 2
```
### 4. .groupindex
**Definition:** Dictionary mapping named groups to their indices.
```python
pattern = re.compile(r"(?P<area>\d+)-(?P<number>\d+)")
print(pattern.groupindex)   # Output: {'area': 1, 'number': 2}
```


---

## 🎯 Match Object Methods

When you use `re.match()`, `re.search()`, or `re.finditer()`, they return a **Match Object**.  
These objects give more details about the match.

| Method | Description | Example |
|--------|-------------|---------|
| `.group([n])` | Returns the entire match (`0`) or specific capturing group. | ```python m = re.search(r"(Hello) (World)", "Hello World") print(m.group())    # Hello World print(m.group(1))   # Hello print(m.group(2))   # World ``` |
| `.groups()` | Returns all groups as a tuple. | ```python m = re.search(r"(Hello) (World)", "Hello World") print(m.groups())  # ('Hello', 'World') ``` |
| `.groupdict()` | Returns named groups as a dictionary. | ```python m = re.search(r"(?P<first>Hello) (?P<second>World)", "Hello World") print(m.groupdict())  # {'first': 'Hello', 'second': 'World'} ``` |
| `.start()` | Starting index of the match. | ```python m = re.search(r"World", "Hello World") print(m.start())  # 6 ``` |
| `.end()` | Ending index of the match. | ```python m = re.search(r"World", "Hello World") print(m.end())  # 11 ``` |
| `.span()` | Returns `(start, end)` tuple. | ```python m = re.search(r"World", "Hello World") print(m.span())  # (6, 11) ``` |
| `.string` | The original string. | ```python m = re.search(r"World", "Hello World") print(m.string)  # Hello World ``` |
| `.re` | The regex pattern used. | ```python pat = re.compile(r"World") m = pat.search("Hello World") print(m.re)  # re.compile('World') ``` |

---

## ⚙️ Pattern Object Attributes

When you **compile a regex** using `re.compile()`, it returns a **Pattern Object** with attributes.

| Attribute | Description | Example |
|-----------|-------------|---------|
| `.pattern` | The original regex pattern string. | ```python pat = re.compile(r"\d+") print(pat.pattern)  # \d+ ``` |
| `.flags` | Regex flags (e.g., IGNORECASE, MULTILINE). | ```python pat = re.compile(r"\d+", re.IGNORECASE) print(pat.flags)  # 2 ``` |
| `.groups` | Number of capturing groups. | ```python pat = re.compile(r"(a)(b)(c)") print(pat.groups)  # 3 ``` |
| `.groupindex` | Dictionary mapping **named groups** to indices. | ```python pat = re.compile(r"(?P<first>\w+) (?P<second>\w+)") print(pat.groupindex)  # {'first': 1, 'second': 2} ``` |

---



## 📖 References

- [Python `re` module documentation](https://docs.python.org/3/library/re.html)  
- [Real Python – Regular Expressions](https://realpython.com/regex-python/)  
- [W3Schools – Python RegEx](https://www.w3schools.com/python/python_regex.asp)  
- [GeeksforGeeks – Python Regex](https://www.geeksforgeeks.org/python-regex-re-module/)  
