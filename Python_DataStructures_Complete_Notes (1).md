# 🐍 Python Data Structures — Complete Study Notes
> **After reading this, every Python data structure concept will be crystal clear.**
> Last Updated: May 2026 | Research-backed & Production-ready

---

## 📌 Table of Contents
1. [Overview — All Data Structures at a Glance](#1-overview--all-data-structures-at-a-glance)
2. [List — Complete Guide](#2-list--complete-guide)
3. [Tuple — Complete Guide](#3-tuple--complete-guide)
4. [Set — Complete Guide](#4-set--complete-guide)
5. [Dictionary — Complete Guide](#5-dictionary--complete-guide)
6. [String — Complete Guide](#6-string--complete-guide)
7. [Stack — Implementation & Usage](#7-stack--implementation--usage)
8. [Queue — Implementation & Usage](#8-queue--implementation--usage)
9. [Linked List — Implementation](#9-linked-list--implementation)
10. [Comprehensions — List, Dict, Set](#10-comprehensions--list-dict-set)
11. [Sorting — sorted() & .sort()](#11-sorting--sorted--sort)
12. [Copying — Shallow vs Deep](#12-copying--shallow-vs-deep)
13. [Itertools & Collections Module](#13-itertools--collections-module)
14. [Common Patterns & Tricks](#14-common-patterns--tricks)
15. [Time Complexity Cheat Sheet](#15-time-complexity-cheat-sheet)
16. [Common Interview Questions](#16-common-interview-questions)

---

## 1. Overview — All Data Structures at a Glance

```
Python Built-in Data Structures:

  List     [1, 2, 3]         → Ordered, Mutable, Allows Duplicates, Indexed
  Tuple    (1, 2, 3)         → Ordered, Immutable, Allows Duplicates, Indexed
  Set      {1, 2, 3}         → Unordered, Mutable, NO Duplicates, NOT Indexed
  Dict     {"a": 1}          → Ordered (Py3.7+), Mutable, Unique Keys, Key-indexed
  String   "hello"           → Ordered, Immutable, Sequence of Characters, Indexed
  frozenset frozenset({1,2}) → Immutable version of Set
  bytes    b"hello"          → Immutable sequence of bytes
  bytearray bytearray(b"hi")  → Mutable sequence of bytes
```

### Comparison Table

| Feature | List | Tuple | Set | Dict | String |
|---------|------|-------|-----|------|--------|
| Ordered | ✅ | ✅ | ❌ | ✅ (3.7+) | ✅ |
| Mutable | ✅ | ❌ | ✅ | ✅ | ❌ |
| Duplicates allowed | ✅ | ✅ | ❌ | ❌ (keys) | ✅ |
| Indexed | ✅ | ✅ | ❌ | ✅ (by key) | ✅ |
| Syntax | `[  ]` | `(  )` | `{  }` | `{k:v}` | `" "` |

### When to Use Which?

| Situation | Use |
|-----------|-----|
| Ordered, changeable collection | **List** |
| Fixed data that won't change | **Tuple** |
| Unique elements, fast lookup | **Set** |
| Key-value mapping | **Dictionary** |
| Sequence of characters | **String** |
| Return multiple values from function | **Tuple** |
| Remove duplicates from a list | **Set** |
| Fast member-check (`in` operator) | **Set** (O(1) vs List O(n)) |
| Ordered unique elements | **dict.fromkeys()** or `sorted(set())` |

---

## 2. List — Complete Guide

### What is a List?
A **List** is an **ordered, mutable, iterable** collection that can hold elements of any data type — including other lists.

```python
# Creating lists
empty_list = []
empty_list = list()

numbers = [1, 2, 3, 4, 5]
mixed = [1, 2.5, "apple", True, None, [10, 20]]
nested = [[1, 2], [3, 4], [5, 6]]

# From other iterables
from_string = list("hello")        # → ['h', 'e', 'l', 'l', 'o']
from_range  = list(range(1, 6))    # → [1, 2, 3, 4, 5]
from_tuple  = list((1, 2, 3))      # → [1, 2, 3]
```

### Indexing & Slicing

```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

# Positive indexing (left to right, starts at 0)
fruits[0]    # "apple"
fruits[2]    # "cherry"
fruits[-1]   # "elderberry" (last)
fruits[-2]   # "date" (second from last)

# Slicing: list[start:stop:step]
fruits[1:3]   # ["banana", "cherry"]       (index 1,2 — stop not included)
fruits[:3]    # ["apple", "banana", "cherry"] (from beginning)
fruits[2:]    # ["cherry", "date", "elderberry"] (to end)
fruits[::2]   # ["apple", "cherry", "elderberry"] (every 2nd)
fruits[::-1]  # ["elderberry", "date", "cherry", "banana", "apple"] (reversed)
fruits[1:4:2] # ["banana", "date"] (index 1,3)

# Slicing does NOT raise IndexError — out-of-range is safe
fruits[1:100]  # ["banana", "cherry", "date", "elderberry"] (stops at end)
```

### All List Methods — With Explanation

```python
lst = [3, 1, 4, 1, 5, 9, 2, 6]

# ─── ADDING ELEMENTS ─────────────────────────────────────────────
lst.append(7)
# Adds 7 to the END of list.
# O(1) time. Returns None (modifies in-place).
# [3, 1, 4, 1, 5, 9, 2, 6, 7]

lst.extend([8, 9, 10])
# Adds all elements from the iterable to the END.
# Like appending each element one by one.
# [3, 1, 4, 1, 5, 9, 2, 6, 7, 8, 9, 10]

lst.insert(0, 99)
# Inserts 99 at index 0 (shifts everything right).
# insert(index, value) → O(n) time
# [99, 3, 1, 4, 1, 5, 9, 2, 6, 7, 8, 9, 10]

# ─── REMOVING ELEMENTS ──────────────────────────────────────────
lst.remove(1)
# Removes the FIRST occurrence of value 1.
# Raises ValueError if value not found.
# O(n) time.

lst.pop()
# Removes and RETURNS the LAST element.
# O(1) time.

lst.pop(0)
# Removes and RETURNS element at index 0.
# O(n) time (shifts all elements).

lst.clear()
# Removes ALL elements. List becomes [].
# O(n) time.

lst = [3, 1, 4, 1, 5, 9, 2, 6]   # reset

# ─── SEARCHING ──────────────────────────────────────────────────
lst.index(4)
# Returns the index of FIRST occurrence of 4 → 2
# Raises ValueError if not found.
# Optional: lst.index(4, start, stop)

lst.count(1)
# Counts how many times 1 appears → 2

4 in lst
# Membership test → True (O(n))

4 not in lst
# → False

# ─── SORTING & ORDERING ─────────────────────────────────────────
lst.sort()
# Sorts in ASCENDING order IN-PLACE.
# O(n log n). Returns None.
# [1, 1, 2, 3, 4, 5, 6, 9]

lst.sort(reverse=True)
# Sorts in DESCENDING order in-place.
# [9, 6, 5, 4, 3, 2, 1, 1]

lst.sort(key=lambda x: -x)
# Custom sort: sort by negative value (same as reverse=True for numbers)

words = ["banana", "apple", "cherry", "date"]
words.sort(key=len)
# Sort by string LENGTH: ["date", "apple", "banana", "cherry"]

words.sort(key=str.lower)
# Sort case-insensitively

lst.reverse()
# Reverses the list IN-PLACE. O(n). Returns None.

# ─── COPYING ────────────────────────────────────────────────────
lst_copy = lst.copy()
# Returns a SHALLOW copy. Same as lst[:] or list(lst).

# ─── BUILT-IN FUNCTIONS WITH LISTS ──────────────────────────────
lst = [3, 1, 4, 1, 5, 9]

len(lst)       # → 6 (number of elements)
sum(lst)       # → 23 (sum of all elements)
min(lst)       # → 1 (smallest element)
max(lst)       # → 9 (largest element)
sorted(lst)    # → [1, 1, 3, 4, 5, 9] (returns NEW sorted list, original unchanged)
reversed(lst)  # → iterator of reversed list (use list() to convert)
enumerate(lst) # → (0,3),(1,1),(2,4)... — pairs of (index, value)
zip([1,2,3], ["a","b","c"])  # → (1,"a"),(2,"b"),(3,"c") — pair elements
any([0, 0, 1])  # → True (True if ANY element is truthy)
all([1, 2, 3])  # → True (True if ALL elements are truthy)
```

### List as Stack (LIFO)
```python
stack = []
stack.append("first")    # push
stack.append("second")
stack.append("third")
stack.pop()              # pop → "third" (last in, first out)
stack.pop()              # pop → "second"
```

### List as Queue (FIFO — but use deque for performance)
```python
# Slow (O(n) for pop from front)
queue = []
queue.append("first")
queue.append("second")
queue.pop(0)   # → "first" (slow — shifts all elements)

# Fast with deque (O(1) both ends)
from collections import deque
queue = deque()
queue.append("first")
queue.appendleft("priority")
queue.popleft()   # → "priority"
```

### Common List Operations

```python
# Concatenation
[1, 2] + [3, 4]    # → [1, 2, 3, 4]

# Repetition
[0] * 5            # → [0, 0, 0, 0, 0]
[1, 2] * 3         # → [1, 2, 1, 2, 1, 2]

# Unpack list
a, b, c = [1, 2, 3]
first, *rest = [1, 2, 3, 4, 5]   # first=1, rest=[2,3,4,5]
*init, last = [1, 2, 3, 4, 5]    # init=[1,2,3,4], last=5

# Check membership
3 in [1, 2, 3, 4]     # → True
10 not in [1, 2, 3]   # → True

# Delete element or slice
lst = [1, 2, 3, 4, 5]
del lst[2]          # removes index 2 → [1, 2, 4, 5]
del lst[1:3]        # removes slice → [1, 5]
del lst             # deletes the entire variable

# Flatten a nested list
nested = [[1,2],[3,4],[5,6]]
flat = [x for sublist in nested for x in sublist]   # [1,2,3,4,5,6]

# Remove duplicates (preserve order)
seen = set()
unique = [x for x in lst if not (x in seen or seen.add(x))]
```

### Quick Reference Table

| Method/Function | What It Does | Returns | In-place? |
|----------------|-------------|---------|-----------|
| `append(x)` | Add x to end | None | ✅ |
| `extend(iter)` | Add all items from iter | None | ✅ |
| `insert(i, x)` | Insert x at index i | None | ✅ |
| `remove(x)` | Remove first x | None | ✅ |
| `pop(i=-1)` | Remove & return item at i | element | ✅ |
| `clear()` | Remove all elements | None | ✅ |
| `index(x)` | First index of x | int | ❌ |
| `count(x)` | Count x occurrences | int | ❌ |
| `sort()` | Sort in-place | None | ✅ |
| `reverse()` | Reverse in-place | None | ✅ |
| `copy()` | Shallow copy | list | ❌ |
| `len(lst)` | Number of elements | int | ❌ |
| `sum(lst)` | Sum of elements | number | ❌ |
| `min(lst)` | Smallest element | element | ❌ |
| `max(lst)` | Largest element | element | ❌ |
| `sorted(lst)` | New sorted list | list | ❌ |
| `reversed(lst)` | Reversed iterator | iterator | ❌ |
| `enumerate(lst)` | (index, value) pairs | iterator | ❌ |
| `zip(l1, l2)` | Pair elements | iterator | ❌ |
| `any(lst)` | Any truthy element? | bool | ❌ |
| `all(lst)` | All truthy elements? | bool | ❌ |

---

## 3. Tuple — Complete Guide

### What is a Tuple?
A **Tuple** is an **ordered, immutable** collection. Once created, it cannot be changed.

```python
# Creating tuples
empty_tuple = ()
empty_tuple = tuple()

single = (42,)         # COMMA is required for single-element tuple!
single = 42,           # parentheses optional
wrong  = (42)          # This is just the integer 42, NOT a tuple!

t = (1, 2, 3, 4, 5)
mixed = (1, "hello", 3.14, True, None)

# From other iterables
from_list   = tuple([1, 2, 3])     # → (1, 2, 3)
from_string = tuple("abc")        # → ('a', 'b', 'c')
from_range  = tuple(range(5))     # → (0, 1, 2, 3, 4)
```

### Indexing & Slicing (Same as List)

```python
t = (10, 20, 30, 40, 50)

t[0]      # → 10
t[-1]     # → 50
t[1:3]    # → (20, 30)
t[::-1]   # → (50, 40, 30, 20, 10)
```

### All Tuple Methods — With Explanation

```python
t = (1, 2, 3, 2, 4, 2, 5)

t.count(2)
# Counts how many times 2 appears → 3
# O(n) time

t.index(3)
# Returns index of FIRST occurrence of 3 → 2
# Raises ValueError if not found
# Optional: t.index(3, start, stop)

# ─── BUILT-IN FUNCTIONS WITH TUPLES ─────────────────────────────
len(t)         # → 7
sum(t)         # → 19
min(t)         # → 1
max(t)         # → 5
sorted(t)      # → [1, 2, 2, 2, 3, 4, 5] (returns a LIST)
reversed(t)    # → iterator
any(t)         # → True
all(t)         # → True
```

### Packing & Unpacking

```python
# Packing — values automatically grouped into tuple
point = 3, 4               # → (3, 4)
person = "Alice", 30, "Mumbai"  # → ('Alice', 30, 'Mumbai')

# Unpacking — assign each element to a variable
x, y = (3, 4)
name, age, city = ("Alice", 30, "Mumbai")

# Extended unpacking
first, *middle, last = (1, 2, 3, 4, 5)
# first=1, middle=[2, 3, 4], last=5

# Swap variables (using tuple packing/unpacking)
a, b = 10, 20
a, b = b, a           # pythonic swap! a=20, b=10

# Function returning multiple values (actually returns a tuple)
def min_max(lst):
    return min(lst), max(lst)

low, high = min_max([3, 1, 4, 1, 5, 9])
print(low, high)    # 1 9
```

### Tuple vs List — When to Use Which?

| Situation | Choice | Reason |
|-----------|--------|--------|
| Data that should NOT change | **Tuple** | Immutable = safer |
| Data that will change | **List** | Mutable |
| Dictionary key | **Tuple** | Tuples are hashable |
| Multiple return values | **Tuple** | Convention |
| Performance critical iteration | **Tuple** | ~20% faster than list |
| CSV rows, database records | **Tuple** | Represents fixed record |

```python
# Tuple as dictionary key (List cannot be a dict key!)
locations = {
    (28.6139, 77.2090): "Delhi",
    (19.0760, 72.8777): "Mumbai"
}
print(locations[(28.6139, 77.2090)])   # "Delhi"
```

### Named Tuples — Readable Tuples

```python
from collections import namedtuple

# Create a named tuple class
Point = namedtuple("Point", ["x", "y"])
Person = namedtuple("Person", ["name", "age", "city"])

p = Point(3, 4)
print(p.x, p.y)    # 3 4 — access by name (much more readable)
print(p[0], p[1])  # 3 4 — also works by index

alice = Person("Alice", 30, "Mumbai")
print(alice.name)  # "Alice"

# namedtuple is immutable like tuple
# alice.age = 31  → AttributeError!
```

### Quick Reference Table

| Method/Function | What It Does | Returns |
|----------------|-------------|---------|
| `count(x)` | Count x occurrences | int |
| `index(x)` | First index of x | int |
| `len(t)` | Number of elements | int |
| `min(t)` | Smallest element | element |
| `max(t)` | Maximum element | element |
| `sum(t)` | Sum of elements | number |
| `sorted(t)` | New sorted list (not tuple!) | list |
| `reversed(t)` | Reversed iterator | iterator |
| `tuple(iter)` | Convert iterable to tuple | tuple |

---

## 4. Set — Complete Guide

### What is a Set?
A **Set** is an **unordered, mutable collection of unique elements**. Duplicates are automatically discarded. Internally uses a hash table — making `in` checks O(1).

```python
# Creating sets
empty_set = set()         # NOT {} — that creates an empty dict!
s = {1, 2, 3, 4, 5}
s = {1, 2, 2, 3, 3, 3}   # → {1, 2, 3} (duplicates removed)

# From other iterables
from_list   = set([1, 2, 2, 3, 3])   # → {1, 2, 3}
from_string = set("hello")            # → {'h', 'e', 'l', 'o'}
from_range  = set(range(1, 6))        # → {1, 2, 3, 4, 5}

# frozenset — immutable version of set (can be used as dict key)
fs = frozenset({1, 2, 3})
# fs.add(4)  → AttributeError — immutable!
```

### All Set Methods — With Explanation

```python
s = {1, 2, 3, 4, 5}

# ─── ADDING ELEMENTS ─────────────────────────────────────────────
s.add(6)
# Adds single element 6. If already exists, does nothing (no error).
# O(1) average. Returns None.
# {1, 2, 3, 4, 5, 6}

s.update([7, 8], {9, 10})
# Adds all elements from one or more iterables.
# Like extend() for lists but for sets.
# {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

# ─── REMOVING ELEMENTS ──────────────────────────────────────────
s.remove(10)
# Removes element. Raises KeyError if not found.
# O(1) average.

s.discard(99)
# Removes element. Does NOTHING if not found (no error).
# Safer than remove() when unsure if element exists.

s.pop()
# Removes and returns an ARBITRARY element (sets are unordered).
# Raises KeyError if set is empty.

s.clear()
# Removes ALL elements. Set becomes set().

s = {1, 2, 3, 4, 5}   # reset

# ─── SET OPERATIONS ─────────────────────────────────────────────
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Union — all elements from BOTH sets
a.union(b)            # → {1, 2, 3, 4, 5, 6}
a | b                 # → {1, 2, 3, 4, 5, 6} (operator shorthand)
a |= b                # updates a in-place with union

# Intersection — elements in BOTH sets
a.intersection(b)     # → {3, 4}
a & b                 # → {3, 4}
a &= b                # updates a in-place with intersection

# Difference — elements in a but NOT in b
a.difference(b)       # → {1, 2}
a - b                 # → {1, 2}
a -= b                # updates a in-place

# Symmetric Difference — elements in EITHER set but NOT BOTH
a.symmetric_difference(b)   # → {1, 2, 5, 6}
a ^ b                        # → {1, 2, 5, 6}
a ^= b                       # updates a in-place

# ─── SET RELATIONSHIPS ──────────────────────────────────────────
{1, 2}.issubset({1, 2, 3})    # → True  (all of {1,2} is in {1,2,3})
{1, 2} <= {1, 2, 3}           # → True  (same as issubset)
{1, 2} < {1, 2, 3}            # → True  (proper subset — not equal)

{1, 2, 3}.issuperset({1, 2})  # → True  (has all elements of {1,2})
{1, 2, 3} >= {1, 2}           # → True  (same as issuperset)
{1, 2, 3} > {1, 2}            # → True  (proper superset)

{1, 2}.isdisjoint({3, 4})     # → True  (no common elements)
{1, 2}.isdisjoint({2, 3})     # → False (2 is common)

# ─── OTHER ──────────────────────────────────────────────────────
s.copy()              # Shallow copy of the set

len(s)                # Number of elements
sorted(s)             # Sorted list from set (set itself unordered)
sum(s)                # Sum of elements
min(s), max(s)        # Min and max

3 in s                # Membership test → True  (O(1) — much faster than list!)
10 not in s           # → True
```

### Update Methods (In-place Set Operations)

```python
a = {1, 2, 3}
b = {3, 4, 5}

a.update(b)                      # same as: a |= b
a.intersection_update(b)         # same as: a &= b
a.difference_update(b)           # same as: a -= b
a.symmetric_difference_update(b) # same as: a ^= b
```

### Common Set Use Cases

```python
# 1. Remove duplicates from a list (fast)
nums = [1, 2, 2, 3, 3, 3, 4]
unique = list(set(nums))    # [1, 2, 3, 4] (order not guaranteed)

# Preserve order while deduplicating
seen = set()
unique_ordered = [x for x in nums if not (x in seen or seen.add(x))]

# 2. Fast membership test (O(1) vs list's O(n))
valid_users = {"alice", "bob", "charlie"}
if "alice" in valid_users:     # instant lookup
    print("Valid user")

# 3. Find common elements
students_A = {"Alice", "Bob", "Charlie"}
students_B = {"Bob", "Charlie", "Diana"}
both_classes = students_A & students_B   # {"Bob", "Charlie"}

# 4. Find elements not in another set
only_A = students_A - students_B    # {"Alice"}

# 5. Count unique elements
words = ["apple", "banana", "apple", "cherry", "banana"]
print(len(set(words)))   # → 3 unique words
```

### Quick Reference Table

| Method | What It Does | Raises Error If? | Returns |
|--------|-------------|-----------------|---------|
| `add(x)` | Add x | — | None |
| `update(iter)` | Add all from iterable | — | None |
| `remove(x)` | Remove x | x not found | None |
| `discard(x)` | Remove x (safe) | Never raises | None |
| `pop()` | Remove & return arbitrary | Set is empty | element |
| `clear()` | Remove all | — | None |
| `copy()` | Shallow copy | — | set |
| `union(s)` / `|` | All elements | — | set |
| `intersection(s)` / `&` | Common elements | — | set |
| `difference(s)` / `-` | In self, not in s | — | set |
| `symmetric_difference(s)` / `^` | In either, not both | — | set |
| `issubset(s)` / `<=` | Is subset? | — | bool |
| `issuperset(s)` / `>=` | Is superset? | — | bool |
| `isdisjoint(s)` | No common elements? | — | bool |

---

## 5. Dictionary — Complete Guide

### What is a Dictionary?
A **Dictionary** is an **ordered (Python 3.7+), mutable collection of key-value pairs**. Keys must be unique and immutable; values can be anything.

```python
# Creating dictionaries
empty_dict = {}
empty_dict = dict()

person = {"name": "Alice", "age": 30, "city": "Mumbai"}

# Various creation methods:
d1 = dict(name="Alice", age=30)          # keyword arguments
d2 = dict([("name", "Alice"), ("age", 30)])  # from list of tuples
d3 = dict.fromkeys(["a", "b", "c"], 0)   # → {"a":0, "b":0, "c":0}
d4 = {x: x**2 for x in range(1, 6)}      # dict comprehension
# → {1:1, 2:4, 3:9, 4:16, 5:25}

# Nested dictionary
student = {
    "name": "Bob",
    "grades": {"math": 90, "english": 85, "science": 92},
    "hobbies": ["coding", "reading"]
}
```

### Accessing Values

```python
d = {"name": "Alice", "age": 30, "city": "Mumbai"}

# Direct access — KeyError if key not found
d["name"]          # → "Alice"
d["email"]         # → KeyError!

# Safe access with .get() — returns None or default if key missing
d.get("name")           # → "Alice"
d.get("email")          # → None (no error)
d.get("email", "N/A")   # → "N/A" (custom default)

# Nested access
student["grades"]["math"]       # → 90
student.get("grades", {}).get("math", 0)  # safe nested access
```

### All Dictionary Methods — With Explanation

```python
d = {"name": "Alice", "age": 30, "city": "Mumbai"}

# ─── READING ─────────────────────────────────────────────────────
d.keys()
# Returns all KEYS as a dict_keys view object.
# → dict_keys(['name', 'age', 'city'])
# Convert to list: list(d.keys())

d.values()
# Returns all VALUES as a dict_values view object.
# → dict_values(['Alice', 30, 'Mumbai'])

d.items()
# Returns all KEY-VALUE PAIRS as a dict_items view object.
# → dict_items([('name', 'Alice'), ('age', 30), ('city', 'Mumbai')])
# Perfect for iteration: for key, value in d.items():

# ─── MODIFYING ───────────────────────────────────────────────────
d["email"] = "alice@mail.com"
# Add new key-value pair (or update existing key).

d.update({"age": 31, "phone": "9999999999"})
# Merges another dict into d. Updates existing keys, adds new ones.
# Can also use: d.update(age=31, phone="9999")
# Python 3.9+: d |= {"age": 31}   (merge operator)
# Python 3.9+: merged = d1 | d2   (new merged dict)

d.setdefault("country", "India")
# If "country" key doesn't exist → adds it with value "India".
# If key already exists → does NOTHING (preserves existing value).
# Returns the value for the key (existing or newly set).

# ─── REMOVING ────────────────────────────────────────────────────
d.pop("city")
# Removes and RETURNS the value for "city" → "Mumbai"
# Raises KeyError if key not found.
# Safe: d.pop("city", "default_value")   ← returns default if missing

d.pop("missing", None)
# → None (no error)

d.popitem()
# Removes and returns the LAST inserted (key, value) pair.
# → ('phone', '9999999999')
# Raises KeyError if dict is empty.

del d["name"]
# Deletes key "name". Raises KeyError if missing.

d.clear()
# Removes ALL key-value pairs. Dict becomes {}.

# ─── COPYING ─────────────────────────────────────────────────────
d.copy()
# Returns a SHALLOW copy of the dictionary.

import copy
deep = copy.deepcopy(d)
# Returns a DEEP copy (nested dicts/lists are also copied).

# ─── BUILT-IN FUNCTIONS ──────────────────────────────────────────
d = {"a": 3, "b": 1, "c": 2}

len(d)          # → 3 (number of key-value pairs)
"a" in d        # → True (checks KEYS only, O(1))
"a" not in d    # → False
"z" in d        # → False
sorted(d)       # → ['a', 'b', 'c'] (sorted list of KEYS)
```

### Iteration Patterns

```python
d = {"name": "Alice", "age": 30, "city": "Mumbai"}

# Iterate over KEYS (default)
for key in d:
    print(key)      # name, age, city

# Iterate over VALUES
for value in d.values():
    print(value)    # Alice, 30, Mumbai

# Iterate over KEY-VALUE pairs (most common)
for key, value in d.items():
    print(f"{key}: {value}")

# Iterate with index
for i, (key, value) in enumerate(d.items()):
    print(i, key, value)

# Sorted iteration
for key in sorted(d.keys()):
    print(key, d[key])

# Sort by value
for key in sorted(d, key=lambda k: str(d[k])):
    print(key, d[key])
```

### Merging Dictionaries

```python
d1 = {"a": 1, "b": 2}
d2 = {"b": 99, "c": 3}

# Method 1: update() — modifies d1 in-place
d1.update(d2)           # d1 = {"a":1, "b":99, "c":3}

# Method 2: dict unpacking (Python 3.5+)
merged = {**d1, **d2}   # {"a":1, "b":99, "c":3}

# Method 3: | operator (Python 3.9+)
merged = d1 | d2        # {"a":1, "b":99, "c":3}

# Method 4: |= in-place (Python 3.9+)
d1 |= d2
```

### Common Dictionary Patterns

```python
# Counting occurrences
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
count = {}
for word in words:
    count[word] = count.get(word, 0) + 1
# → {"apple":3, "banana":2, "cherry":1}

# Grouping items
students = [("Alice","A"), ("Bob","B"), ("Charlie","A"), ("Diana","B")]
groups = {}
for name, grade in students:
    groups.setdefault(grade, []).append(name)
# → {"A":["Alice","Charlie"], "B":["Bob","Diana"]}

# Invert a dictionary (swap keys and values)
original = {"a": 1, "b": 2, "c": 3}
inverted = {v: k for k, v in original.items()}
# → {1:"a", 2:"b", 3:"c"}

# Filter dictionary
scores = {"Alice": 85, "Bob": 72, "Charlie": 91, "Diana": 68}
passing = {k: v for k, v in scores.items() if v >= 75}
# → {"Alice":85, "Charlie":91}

# Default dict — no KeyError for missing keys
from collections import defaultdict
word_count = defaultdict(int)
for word in words:
    word_count[word] += 1   # no .get() needed!

groups = defaultdict(list)
for name, grade in students:
    groups[grade].append(name)   # no setdefault needed!
```

### Quick Reference Table

| Method | What It Does | Returns |
|--------|-------------|---------|
| `keys()` | All keys | dict_keys view |
| `values()` | All values | dict_values view |
| `items()` | All (k,v) pairs | dict_items view |
| `get(key, default)` | Value for key (safe) | value or default |
| `pop(key, default)` | Remove & return value | value |
| `popitem()` | Remove & return last (k,v) | (k,v) tuple |
| `update(other)` | Merge another dict | None |
| `setdefault(key, val)` | Set if not exists | value |
| `clear()` | Remove all | None |
| `copy()` | Shallow copy | dict |
| `fromkeys(keys, val)` | New dict from keys | dict |
| `len(d)` | Number of pairs | int |
| `key in d` | Key exists? | bool |

---

## 6. String — Complete Guide

### What is a String?
A **String** is an **ordered, immutable sequence of characters**. Can use single `'`, double `"`, or triple `'''`/`"""` quotes.

```python
s1 = 'Hello'
s2 = "World"
s3 = '''This is a
multiline string'''
s4 = """Also
multiline"""

# Raw string (ignores backslash escape sequences)
path = r"C:\Users\Alice\Desktop"    # r prefix

# f-string (formatted string — Python 3.6+)
name = "Alice"
age = 30
greeting = f"Hello {name}, you are {age} years old!"
greeting = f"2 + 2 = {2 + 2}"               # expressions
greeting = f"Pi is {3.14159:.2f}"           # format spec
greeting = f"{'left':<10} | {'right':>10}"  # alignment
```

### Indexing & Slicing (Same as List but immutable)

```python
s = "Python"
# Indices: P=0, y=1, t=2, h=3, o=4, n=5
#         P=-6, y=-5, t=-4, h=-3, o=-2, n=-1

s[0]      # "P"
s[-1]     # "n"
s[1:4]    # "yth"
s[:3]     # "Pyt"
s[3:]     # "hon"
s[::2]    # "Pto"
s[::-1]   # "nohtyP" (reverse)
```

### All String Methods — With Explanation

```python
s = "  Hello, World!  "

# ─── CASE METHODS ────────────────────────────────────────────────
"hello".upper()          # → "HELLO"     — all uppercase
"HELLO".lower()          # → "hello"     — all lowercase
"hello world".title()    # → "Hello World" — each word capitalized
"hello world".capitalize() # → "Hello world" — only first char
"HeLLo".swapcase()       # → "hEllO"    — swap upper↔lower

# ─── WHITESPACE REMOVAL ──────────────────────────────────────────
"  hello  ".strip()      # → "hello"    — remove both ends
"  hello  ".lstrip()     # → "hello  "  — remove left only
"  hello  ".rstrip()     # → "  hello"  — remove right only
"xxhelloxx".strip("x")   # → "hello"    — remove specific chars

# ─── SEARCHING ───────────────────────────────────────────────────
"hello world".find("world")     # → 6    — index of first match (-1 if not found)
"hello world".find("xyz")       # → -1   — safe (no error)
"hello world".rfind("l")        # → 9    — index of LAST match
"hello world".index("world")    # → 6    — same as find but raises ValueError
"hello world".rindex("l")       # → 9    — last match, raises ValueError

"hello".startswith("he")        # → True
"hello".endswith("lo")          # → True
"hello".startswith(("he","hi")) # → True  — tuple of prefixes

"hello world".count("l")        # → 3    — count occurrences
"hello world".count("ll")       # → 1

# ─── SPLIT & JOIN ────────────────────────────────────────────────
"a,b,c".split(",")          # → ['a', 'b', 'c']
"a,b,c".split(",", 1)       # → ['a', 'b,c']  (max 1 split)
"a  b  c".split()           # → ['a', 'b', 'c'] (split on whitespace)
"a,b,c".rsplit(",", 1)      # → ['a,b', 'c']  (split from right)
"line1\nline2\nline3".splitlines()  # → ['line1', 'line2', 'line3']

",".join(["a", "b", "c"])       # → "a,b,c"
" ".join(["Hello", "World"])    # → "Hello World"
"".join(["P","y","t","h","o","n"])  # → "Python"
"-".join(str(x) for x in [1,2,3])  # → "1-2-3"

# ─── REPLACE ─────────────────────────────────────────────────────
"hello world".replace("l", "L")      # → "heLLo WorLd" (all occurrences)
"hello world".replace("l", "L", 2)   # → "heLLo world"  (max 2 replacements)

# ─── CHECK CONTENT ───────────────────────────────────────────────
"abc".isalpha()    # → True   — all letters (no digits, spaces)
"123".isdigit()    # → True   — all digits
"abc123".isalnum() # → True   — letters or digits (no spaces)
"   ".isspace()    # → True   — all whitespace
"ABC".isupper()    # → True   — all uppercase
"abc".islower()    # → True   — all lowercase
"Hello World".istitle()  # → True — title case
"123".isnumeric()  # → True   — includes ², ½ etc.
"123.45".isdecimal() # → False — only pure digits
"123".isdecimal()  # → True

# ─── FORMATTING ──────────────────────────────────────────────────
"5".zfill(4)             # → "0005"  — zero-pad on left
"hi".center(10, "*")     # → "****hi****" — center with fill char
"hi".ljust(10, "-")      # → "hi--------"  — left-justify
"hi".rjust(10, "-")      # → "--------hi"  — right-justify

# str.format()
"Hello, {}!".format("Alice")            # → "Hello, Alice!"
"Name: {name}, Age: {age}".format(name="Alice", age=30)
"{0:.2f}".format(3.14159)               # → "3.14"
"{:,}".format(1000000)                  # → "1,000,000"

# f-strings (preferred in modern Python)
name = "Alice"; age = 30
f"Hello {name}! You are {age} years old."
f"{3.14159:.2f}"               # → "3.14"
f"{1000000:,}"                 # → "1,000,000"
f"{'hello':>10}"               # → "     hello" (right-align in 10 chars)
f"{'hello':<10}"               # → "hello     " (left-align)
f"{'hello':^10}"               # → "  hello   " (center)
f"{'hello':*^10}"              # → "**hello***" (center with * fill)

# ─── ENCODE / DECODE ─────────────────────────────────────────────
"hello".encode("utf-8")        # → b'hello' (bytes)
b"hello".decode("utf-8")       # → "hello" (string)

# ─── CHECKING ────────────────────────────────────────────────────
len("hello")         # → 5 (number of characters)
"l" in "hello"       # → True (membership)
"z" not in "hello"   # → True
"hello" == "hello"   # → True
"abc" < "abd"        # → True (lexicographic comparison)

# ─── MISC ────────────────────────────────────────────────────────
"hello\nworld".splitlines()    # → ['hello', 'world']
chr(65)                        # → 'A' (Unicode code point → character)
ord('A')                       # → 65 (character → Unicode code point)
"hello" * 3                    # → "hellohellohello" (repetition)
"hello" + " " + "world"        # → "hello world" (concatenation)
```

### String Formatting — All 3 Ways

```python
name, score, pi = "Alice", 95.678, 3.14159

# Style 1: % formatting (old C-style)
"Hello %s, score: %.2f" % (name, score)

# Style 2: .format() (Python 2.6+)
"Hello {}, score: {:.2f}".format(name, score)
"Hello {0}, score: {1:.2f}".format(name, score)
"Hello {name}, score: {score:.2f}".format(name=name, score=score)

# Style 3: f-strings (Python 3.6+ — recommended)
f"Hello {name}, score: {score:.2f}"
f"Pi = {pi:.4f}"
f"Score: {score:.0f}%"
f"{name!r}"    # → "'Alice'" (repr() applied)
f"{name!s}"    # → "Alice"   (str() applied)
f"{name!a}"    # → "'Alice'" (ascii() applied)
```

### Quick Reference Table

| Method | What It Does | Returns |
|--------|-------------|---------|
| `lower()` | All lowercase | str |
| `upper()` | All uppercase | str |
| `title()` | Title Case | str |
| `capitalize()` | First letter only | str |
| `swapcase()` | Swap case | str |
| `strip(chars)` | Remove leading/trailing | str |
| `lstrip(chars)` | Remove left | str |
| `rstrip(chars)` | Remove right | str |
| `split(sep, n)` | Split into list | list |
| `rsplit(sep, n)` | Split from right | list |
| `splitlines()` | Split on newlines | list |
| `join(iter)` | Join with separator | str |
| `replace(old, new, n)` | Replace substring | str |
| `find(sub)` | Index of first match (-1 if none) | int |
| `rfind(sub)` | Index of last match | int |
| `index(sub)` | Like find but raises ValueError | int |
| `count(sub)` | Count occurrences | int |
| `startswith(prefix)` | Starts with prefix? | bool |
| `endswith(suffix)` | Ends with suffix? | bool |
| `isalpha()` | All letters? | bool |
| `isdigit()` | All digits? | bool |
| `isalnum()` | All letters/digits? | bool |
| `isspace()` | All whitespace? | bool |
| `isupper()` | All uppercase? | bool |
| `islower()` | All lowercase? | bool |
| `zfill(width)` | Zero-pad on left | str |
| `center(w, char)` | Center in width | str |
| `ljust(w, char)` | Left-justify | str |
| `rjust(w, char)` | Right-justify | str |
| `encode(enc)` | Convert to bytes | bytes |
| `format(*args)` | Format string | str |
| `len(s)` | Character count | int |

---

## 7. Stack — Implementation & Usage

A **Stack** is a LIFO (Last In, First Out) data structure.

```python
# ─── Using a Python List (simplest) ──────────────────────────────
stack = []

stack.append(1)    # push 1
stack.append(2)    # push 2
stack.append(3)    # push 3
# stack = [1, 2, 3]

stack.pop()        # pop → 3 (LIFO!)
stack[-1]          # peek → 2 (look without removing)
len(stack)         # → 2
bool(stack)        # → True (not empty)

# ─── Using collections.deque (preferred — thread-safe) ───────────
from collections import deque

stack = deque()
stack.append(1)      # push
stack.append(2)
stack.pop()          # pop → 2
stack[-1]            # peek

# ─── OOP Stack Implementation ─────────────────────────────────────
class Stack:
    def __init__(self):
        self._data = []

    def push(self, item):
        self._data.append(item)

    def pop(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._data.pop()

    def peek(self):
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self._data[-1]

    def is_empty(self):
        return len(self._data) == 0

    def size(self):
        return len(self._data)

    def __repr__(self):
        return f"Stack({self._data})"

# Usage:
s = Stack()
s.push(10); s.push(20); s.push(30)
print(s.peek())  # 30
print(s.pop())   # 30
print(s.size())  # 2
```

---

## 8. Queue — Implementation & Usage

A **Queue** is a FIFO (First In, First Out) data structure.

```python
# ─── Using collections.deque (BEST — O(1) both ends) ─────────────
from collections import deque

queue = deque()

queue.append("first")     # enqueue — add to right
queue.append("second")
queue.append("third")

queue.popleft()           # dequeue → "first" (O(1)!)
queue[0]                  # peek front → "second"
len(queue)                # → 2

# double-ended queue
queue.appendleft("priority")  # add to front
queue.pop()                   # remove from right

# ─── Using queue.Queue (thread-safe — for multithreading) ─────────
import queue

q = queue.Queue(maxsize=10)    # bounded queue
q.put("task1")                 # enqueue (blocks if full)
q.put("task2")
q.get()                        # dequeue → "task1" (blocks if empty)
q.qsize()                      # queue size
q.empty()                      # True if empty
q.full()                       # True if maxsize reached
q.put_nowait("task")           # put without blocking (raises Full if full)
q.get_nowait()                 # get without blocking (raises Empty if empty)

# Priority Queue
pq = queue.PriorityQueue()
pq.put((1, "low priority"))
pq.put((0, "high priority"))
pq.put((2, "lowest"))
pq.get()   # → (0, "high priority")  — smallest number = highest priority

# ─── OOP Queue Implementation ─────────────────────────────────────
class Queue:
    def __init__(self):
        self._data = deque()

    def enqueue(self, item):
        self._data.append(item)          # O(1)

    def dequeue(self):
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self._data.popleft()      # O(1)

    def peek(self):
        return self._data[0]

    def is_empty(self):
        return len(self._data) == 0

    def size(self):
        return len(self._data)
```

---

## 9. Linked List — Implementation

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None    # points to next node

class LinkedList:
    def __init__(self):
        self.head = None

    def append(self, data):
        """Add to end"""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node

    def prepend(self, data):
        """Add to beginning — O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node

    def delete(self, data):
        """Delete first node with given data"""
        if not self.head:
            return
        if self.head.data == data:
            self.head = self.head.next
            return
        current = self.head
        while current.next:
            if current.next.data == data:
                current.next = current.next.next
                return
            current = current.next

    def search(self, data):
        """Return True if data exists"""
        current = self.head
        while current:
            if current.data == data:
                return True
            current = current.next
        return False

    def to_list(self):
        result = []
        current = self.head
        while current:
            result.append(current.data)
            current = current.next
        return result

    def __repr__(self):
        return " → ".join(str(x) for x in self.to_list()) + " → None"

# Usage:
ll = LinkedList()
ll.append(1); ll.append(2); ll.append(3)
ll.prepend(0)
print(ll)       # 0 → 1 → 2 → 3 → None
ll.delete(2)
print(ll)       # 0 → 1 → 3 → None
```

---

## 10. Comprehensions — List, Dict, Set

### List Comprehension

```python
# Syntax: [expression for item in iterable if condition]

# Basic
squares = [x**2 for x in range(1, 6)]
# → [1, 4, 9, 16, 25]

# With condition
evens = [x for x in range(20) if x % 2 == 0]
# → [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# With if-else expression
labels = ["even" if x % 2 == 0 else "odd" for x in range(6)]
# → ['even', 'odd', 'even', 'odd', 'even', 'odd']

# Nested loops
pairs = [(x, y) for x in [1, 2, 3] for y in [4, 5] if x != y]
# → [(1, 4), (1, 5), (2, 4), (2, 5), (3, 4), (3, 5)]

# Flatten nested list
nested = [[1,2,3], [4,5,6], [7,8,9]]
flat = [x for row in nested for x in row]
# → [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Apply function
words = ["  hello  ", "  world  "]
cleaned = [w.strip().upper() for w in words]
# → ['HELLO', 'WORLD']
```

### Dict Comprehension

```python
# Syntax: {key_expr: value_expr for item in iterable if condition}

# Square mapping
squares = {x: x**2 for x in range(1, 6)}
# → {1:1, 2:4, 3:9, 4:16, 5:25}

# Filter dict
scores = {"Alice": 85, "Bob": 72, "Charlie": 91}
passing = {k: v for k, v in scores.items() if v >= 80}
# → {"Alice": 85, "Charlie": 91}

# Invert dict
inv = {v: k for k, v in scores.items()}
# → {85:"Alice", 72:"Bob", 91:"Charlie"}

# Count characters
char_count = {ch: s.count(ch) for ch in set(s := "mississippi")}
# → {'m':1, 'i':4, 's':4, 'p':2}
```

### Set Comprehension

```python
# Syntax: {expression for item in iterable if condition}

# Unique squares
sq_set = {x**2 for x in [-2, -1, 0, 1, 2]}
# → {0, 1, 4} (duplicates removed!)

# Unique first letters
words = ["apple", "banana", "avocado", "blueberry"]
first_letters = {w[0] for w in words}
# → {'a', 'b'}
```

### Generator Expression (memory-efficient)

```python
# Like list comprehension but lazy — generates one value at a time
gen = (x**2 for x in range(1000000))   # No memory used yet
next(gen)   # → 0    (compute one at a time)
next(gen)   # → 1

# Use in functions that accept iterables
total = sum(x**2 for x in range(100))    # more memory-efficient than list
any(x > 90 for x in scores)
all(s.isdigit() for s in strings)
```

---

## 11. Sorting — sorted() & .sort()

```python
lst = [3, 1, 4, 1, 5, 9, 2, 6]

# sorted() — returns NEW list, original unchanged
new_sorted = sorted(lst)           # [1, 1, 2, 3, 4, 5, 6, 9]
new_desc   = sorted(lst, reverse=True)  # [9, 6, 5, 4, 3, 2, 1, 1]

# .sort() — sorts IN-PLACE, returns None
lst.sort()             # lst is now sorted
lst.sort(reverse=True) # lst is now reverse sorted

# ─── Sort by key function ─────────────────────────────────────────
words = ["banana", "apple", "cherry", "date"]

sorted(words, key=len)          # ["date", "apple", "banana", "cherry"]
sorted(words, key=str.lower)    # case-insensitive
sorted(words, key=lambda w: w[-1])  # sort by last character

# Sort list of dicts by a key
students = [
    {"name": "Alice", "age": 30, "score": 85},
    {"name": "Bob",   "age": 25, "score": 91},
    {"name": "Charlie", "age": 28, "score": 78}
]
sorted(students, key=lambda s: s["age"])    # by age
sorted(students, key=lambda s: s["score"], reverse=True)  # by score desc

from operator import itemgetter
sorted(students, key=itemgetter("name"))   # by name (faster than lambda)

# Sort by multiple criteria
sorted(students, key=lambda s: (s["score"], s["name"]))  # score, then name

# Sort list of tuples
pairs = [(1, "b"), (2, "a"), (1, "a")]
sorted(pairs)               # [(1,"a"), (1,"b"), (2,"a")] — default tuple sort
sorted(pairs, key=lambda x: x[1])  # by second element → [(2,"a"),(1,"a"),(1,"b")]

from operator import attrgetter
# For objects: sorted(people, key=attrgetter("name"))
```

---

## 12. Copying — Shallow vs Deep

```python
import copy

original = [[1, 2, 3], [4, 5, 6]]

# ─── Assignment (NOT a copy — same reference!) ───────────────────
alias = original
alias[0][0] = 99
print(original)   # [[99, 2, 3], [4, 5, 6]]  ← original changed!

# ─── Shallow Copy ────────────────────────────────────────────────
# Copies the outer container but inner objects are still shared references
shallow = original.copy()        # list method
shallow = original[:]            # slice
shallow = list(original)         # list constructor
shallow = copy.copy(original)    # copy module

shallow[0] = "new"
print(original[0])   # [99, 2, 3] — outer element unchanged ✅

shallow[1][0] = 777
print(original[1])   # [777, 5, 6] — inner list IS shared! ❌

# ─── Deep Copy ───────────────────────────────────────────────────
# Copies everything recursively — completely independent
original = [[1, 2, 3], [4, 5, 6]]
deep = copy.deepcopy(original)

deep[0][0] = 999
print(original[0])   # [1, 2, 3] — completely unchanged ✅
print(deep[0])       # [999, 2, 3]
```

---

## 13. Itertools & Collections Module

### itertools — Powerful Iteration Tools

```python
import itertools

# chain — combine multiple iterables
list(itertools.chain([1,2], [3,4], [5,6]))   # [1,2,3,4,5,6]

# chain.from_iterable — flatten one level
list(itertools.chain.from_iterable([[1,2],[3,4]]))  # [1,2,3,4]

# combinations — unique combinations (no repeat, order doesn't matter)
list(itertools.combinations([1,2,3], 2))
# [(1,2), (1,3), (2,3)]

# combinations_with_replacement — allow repeated elements
list(itertools.combinations_with_replacement([1,2,3], 2))
# [(1,1),(1,2),(1,3),(2,2),(2,3),(3,3)]

# permutations — all orderings
list(itertools.permutations([1,2,3], 2))
# [(1,2),(1,3),(2,1),(2,3),(3,1),(3,2)]

# product — cartesian product (all combinations with replacement)
list(itertools.product([1,2], ["a","b"]))
# [(1,"a"),(1,"b"),(2,"a"),(2,"b")]

# cycle — infinite cycle
counter = itertools.cycle([1, 2, 3])
[next(counter) for _ in range(7)]  # [1,2,3,1,2,3,1]

# repeat — repeat element n times
list(itertools.repeat("hi", 3))   # ["hi","hi","hi"]

# islice — lazy slice of iterator
list(itertools.islice(range(100), 5, 15, 2))  # [5,7,9,11,13]

# groupby — group consecutive identical elements
data = [("A",1),("A",2),("B",3),("B",4),("C",5)]
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))

# accumulate — running totals
list(itertools.accumulate([1,2,3,4,5]))           # [1,3,6,10,15]
import operator
list(itertools.accumulate([1,2,3,4,5], operator.mul))  # [1,2,6,24,120]

# takewhile / dropwhile
list(itertools.takewhile(lambda x: x < 5, [1,2,3,4,5,6]))  # [1,2,3,4]
list(itertools.dropwhile(lambda x: x < 5, [1,2,3,4,5,6]))  # [5,6]

# zip_longest — zip with fill value for shorter iterables
list(itertools.zip_longest([1,2,3], ["a","b"], fillvalue=None))
# [(1,"a"),(2,"b"),(3,None)]
```

### collections — Specialized Data Structures

```python
from collections import Counter, defaultdict, OrderedDict, deque, namedtuple, ChainMap

# ─── Counter — count hashable objects ────────────────────────────
words = ["apple","banana","apple","cherry","banana","apple"]
c = Counter(words)
# Counter({'apple': 3, 'banana': 2, 'cherry': 1})

c.most_common(2)      # [('apple',3),('banana',2)] — top 2
c["apple"]            # → 3
c["missing"]          # → 0  (no KeyError!)
c.update(["apple","cherry"])  # add more counts
c.subtract(["banana"])        # subtract counts
list(c.elements())    # ['apple','apple','apple','banana','banana','cherry']
sum(c.values())       # total count

# Counter arithmetic
c1 = Counter(a=3, b=2)
c2 = Counter(a=1, b=4, c=1)
c1 + c2    # Counter({'b':6,'a':4,'c':1})
c1 - c2    # Counter({'a':2}) — negative values dropped
c1 & c2    # Counter({'a':1,'b':2}) — intersection (min)
c1 | c2    # Counter({'b':4,'a':3,'c':1}) — union (max)

# ─── defaultdict — dict with default factory ─────────────────────
word_count = defaultdict(int)
for word in words:
    word_count[word] += 1   # no KeyError — default is 0

groups = defaultdict(list)
for name, team in [("Alice","A"),("Bob","B"),("Charlie","A")]:
    groups[team].append(name)
# {"A":["Alice","Charlie"], "B":["Bob"]}

nested = defaultdict(lambda: defaultdict(int))  # nested defaultdict

# ─── OrderedDict — dict that remembers insertion order ───────────
# (Regular dict in Python 3.7+ also maintains order, but OrderedDict has extra features)
od = OrderedDict()
od["first"] = 1
od["second"] = 2
od.move_to_end("first")       # move to end
od.move_to_end("second", last=False)  # move to beginning
od.popitem(last=True)         # pop last item
od.popitem(last=False)        # pop first item

# ─── deque — double-ended queue ──────────────────────────────────
dq = deque([1, 2, 3], maxlen=5)  # optional max size
dq.append(4)         # add to right
dq.appendleft(0)     # add to left — O(1)!
dq.pop()             # remove from right
dq.popleft()         # remove from left — O(1)!
dq.extend([5, 6])    # add iterable to right
dq.extendleft([7])   # add iterable to left (each element)
dq.rotate(2)         # rotate right by 2
dq.rotate(-2)        # rotate left by 2
dq.reverse()         # reverse in place

# ─── ChainMap — multiple dicts as one view ───────────────────────
defaults = {"color": "blue", "size": "medium"}
user_prefs = {"color": "red"}
config = ChainMap(user_prefs, defaults)  # user_prefs takes priority
config["color"]    # → "red" (from user_prefs)
config["size"]     # → "medium" (from defaults)
```

---

## 14. Common Patterns & Tricks

```python
# ─── SWAP WITHOUT TEMP ───────────────────────────────────────────
a, b = 10, 20
a, b = b, a      # a=20, b=10

# ─── MULTIPLE ASSIGNMENT ─────────────────────────────────────────
x = y = z = 0   # all point to same 0

# ─── UNPACKING ───────────────────────────────────────────────────
first, *rest = [1, 2, 3, 4, 5]   # first=1, rest=[2,3,4,5]
*init, last = [1, 2, 3, 4, 5]    # init=[1,2,3,4], last=5
a, _, c = (1, 2, 3)              # _ ignores middle value

# ─── CHECK IF LIST IS EMPTY ──────────────────────────────────────
lst = []
if not lst:          # pythonic (truthy/falsy)
    print("empty")
if len(lst) == 0:    # verbose but explicit

# ─── CONDITIONAL EXPRESSION (TERNARY) ────────────────────────────
result = "yes" if condition else "no"
max_val = a if a > b else b

# ─── DEFAULT PARAMETER TRAP ──────────────────────────────────────
# ❌ WRONG — mutable default argument is shared across calls!
def add_item(item, lst=[]):
    lst.append(item)
    return lst
# ✅ CORRECT
def add_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst

# ─── DICTIONARY AS SWITCH/MATCH ──────────────────────────────────
def process(command):
    actions = {
        "start": lambda: print("Starting..."),
        "stop":  lambda: print("Stopping..."),
        "help":  lambda: print("Help!")
    }
    actions.get(command, lambda: print("Unknown"))()

# ─── FLATTEN NESTED LIST ─────────────────────────────────────────
nested = [[1, 2], [3, [4, 5]], [6]]
import itertools
flat = list(itertools.chain.from_iterable(nested))

# ─── TRANSPOSE A MATRIX ──────────────────────────────────────────
matrix = [[1,2,3],[4,5,6],[7,8,9]]
transposed = list(zip(*matrix))
# [(1,4,7),(2,5,8),(3,6,9)]

# ─── ENUMERATE WITH START INDEX ──────────────────────────────────
for i, item in enumerate(["a","b","c"], start=1):
    print(i, item)   # 1 a, 2 b, 3 c

# ─── ZIP AND UNZIP ───────────────────────────────────────────────
keys = ["a", "b", "c"]
values = [1, 2, 3]
d = dict(zip(keys, values))    # {"a":1, "b":2, "c":3}
k, v = zip(*d.items())         # unzip: k=("a","b","c"), v=(1,2,3)

# ─── IN-PLACE OPERATIONS ─────────────────────────────────────────
x = 5
x += 1    # x = 6  (same as x = x + 1)
x *= 2    # x = 12
x //= 3   # x = 4  (floor division)
x **= 2   # x = 16

# ─── WALRUS OPERATOR := (Python 3.8+) ────────────────────────────
if (n := len(lst)) > 10:
    print(f"List is too long: {n} elements")

while chunk := file.read(8192):   # read until empty
    process(chunk)

# ─── TYPE HINTS ──────────────────────────────────────────────────
def greet(name: str, age: int) -> str:
    return f"Hello {name}, age {age}"

numbers: list[int] = [1, 2, 3]
mapping: dict[str, int] = {"a": 1}
```

---

## 15. Time Complexity Cheat Sheet

### List Operations

| Operation | Time Complexity |
|-----------|----------------|
| `lst[i]` (index access) | O(1) |
| `lst.append(x)` | O(1) amortized |
| `lst.pop()` (from end) | O(1) |
| `lst.pop(0)` (from front) | O(n) |
| `lst.insert(i, x)` | O(n) |
| `lst.remove(x)` | O(n) |
| `x in lst` | O(n) |
| `lst.index(x)` | O(n) |
| `len(lst)` | O(1) |
| `lst.sort()` | O(n log n) |
| `lst + lst2` | O(n + m) |
| `lst.reverse()` | O(n) |

### Set & Dict Operations

| Operation | Time Complexity |
|-----------|----------------|
| `x in set` | O(1) average |
| `set.add(x)` | O(1) average |
| `set.remove(x)` | O(1) average |
| `set1 & set2` | O(min(len(s1),len(s2))) |
| `dict[key]` | O(1) average |
| `dict.get(key)` | O(1) average |
| `key in dict` | O(1) average |
| `dict[key] = val` | O(1) average |
| `del dict[key]` | O(1) average |
| `dict.items()` | O(n) |

### deque Operations

| Operation | Time Complexity |
|-----------|----------------|
| `deque.appendleft(x)` | O(1) |
| `deque.popleft()` | O(1) |
| `deque.append(x)` | O(1) |
| `deque.pop()` | O(1) |
| `deque[0]` | O(1) |
| `deque[n//2]` | O(n) — slow middle access |

---

## 16. Common Interview Questions

### Q1: What is the difference between a List and a Tuple?
**Answer:** A List is mutable (can be changed after creation), while a Tuple is immutable. Lists use `[]`; Tuples use `()`. Tuples are faster for iteration (~20% faster) and can be used as dictionary keys or set elements (because they're hashable). Use Tuples for fixed data (coordinates, database records) and Lists for changeable collections.

### Q2: How is a Set different from a List?
**Answer:** A Set is unordered (no guaranteed order), does not allow duplicates (unique elements only), and does not support indexing. However, membership testing (`in`) is O(1) in a Set vs O(n) in a List. Sets also support mathematical operations like union, intersection, and difference. Use Sets when you need uniqueness or fast lookups.

### Q3: How does Python's dictionary work internally?
**Answer:** Python dictionaries use a hash table internally. When you set `d[key] = value`, Python computes `hash(key)` and stores the value at that hash location. This gives O(1) average lookup, insertion, and deletion. Keys must be hashable (immutable). Since Python 3.7, dictionaries maintain insertion order.

### Q4: What is the difference between `.append()` and `.extend()`?
**Answer:** `append(x)` adds `x` as a single element (even if x is a list, it becomes a nested element). `extend(iterable)` unpacks the iterable and adds each element individually. Example: `[1].append([2,3])` → `[1, [2,3]]` vs `[1].extend([2,3])` → `[1, 2, 3]`.

### Q5: What is the difference between `remove()`, `pop()`, and `del`?
**Answer:** `remove(x)` finds and removes the FIRST occurrence of value x (raises ValueError if not found). `pop(i)` removes and RETURNS the element at index i (default: last element, O(1)). `del lst[i]` deletes by index without returning the value — can also delete slices (`del lst[1:3]`) or the variable itself (`del lst`).

### Q6: What is a shallow copy vs a deep copy?
**Answer:** A shallow copy creates a new outer container, but inner objects are still shared references. Modifying a nested list in the shallow copy affects the original. A deep copy (using `copy.deepcopy()`) recursively copies all nested objects — completely independent. Use shallow copy for flat structures; deep copy for nested structures like lists of lists or dicts of dicts.

### Q7: How do you remove duplicates from a list while preserving order?
**Answer:** `list(set(lst))` removes duplicates but destroys order. To preserve order: `seen = set(); result = [x for x in lst if not (x in seen or seen.add(x))]`. Or in Python 3.7+: `list(dict.fromkeys(lst))` — dict keys are unique and ordered.

### Q8: What is the difference between `sort()` and `sorted()`?
**Answer:** `.sort()` sorts a list IN-PLACE (modifies original, returns None). `sorted()` returns a NEW sorted list (original unchanged) and works on ANY iterable (tuples, sets, strings), not just lists. Both accept `key=` and `reverse=` parameters.

### Q9: What is a dictionary comprehension? Give an example.
**Answer:** A dictionary comprehension creates a dict in a concise, readable way. Syntax: `{key_expr: value_expr for item in iterable if condition}`. Example: `{x: x**2 for x in range(1, 6)}` → `{1:1, 2:4, 3:9, 4:16, 5:25}`. Useful for filtering: `{k: v for k, v in d.items() if v > 0}`.

### Q10: When should you use `collections.Counter`, `defaultdict`, and `deque`?
**Answer:** `Counter` — when you need to count hashable elements or find most common items. `defaultdict` — when you want a dict that automatically creates a default value for missing keys (avoids KeyError, cleaner than `.get(key, default)`). `deque` — when you need a queue or stack with efficient O(1) operations at BOTH ends; regular lists have O(n) for operations at the front.

---

## 🔄 Python Data Structures — Visual Summary

```
           MUTABLE?    ORDERED?    DUPLICATES?    SYNTAX     USE WHEN
           ─────────   ─────────   ───────────    ──────     ─────────
List       ✅ Yes      ✅ Yes      ✅ Yes         [1,2,3]    General collection
Tuple      ❌ No       ✅ Yes      ✅ Yes         (1,2,3)    Fixed data, dict key
Set        ✅ Yes      ❌ No       ❌ No          {1,2,3}    Unique + fast lookup
Dict       ✅ Yes      ✅ Yes      ❌ (keys)      {"k":v}    Key-value mapping
String     ❌ No       ✅ Yes      ✅ Yes         "abc"      Text/characters
deque      ✅ Yes      ✅ Yes      ✅ Yes         deque()    Queue/stack (O(1))
Counter    ✅ Yes      ✅ Yes      ✅ Yes         Counter()  Counting/frequency
defaultdict ✅ Yes     ✅ Yes      ❌ (keys)      defaultdict() Auto-default values
namedtuple ❌ No       ✅ Yes      ✅ Yes         namedtuple() Readable tuples
frozenset  ❌ No       ❌ No       ❌ No          frozenset() Immutable set
```

---

## 📝 Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| List | Ordered, mutable, duplicates OK; `[1, 2, 3]` |
| Tuple | Ordered, immutable, duplicates OK; `(1, 2, 3)` |
| Set | Unordered, mutable, NO duplicates; `{1, 2, 3}` |
| Dict | Ordered (3.7+), mutable, unique keys; `{"k": v}` |
| String | Ordered, immutable, sequence of chars; `"hello"` |
| `append()` | Add ONE element to end of list |
| `extend()` | Add ALL elements from iterable to list |
| `insert(i,x)` | Insert x at index i |
| `pop(i)` | Remove & return element at i (default: last) |
| `remove(x)` | Remove FIRST occurrence of value x |
| `sort()` | Sort list IN-PLACE; returns None |
| `sorted()` | Return NEW sorted list; works on any iterable |
| `copy()` | Shallow copy — nested objects still shared |
| `deepcopy()` | Deep copy — completely independent |
| `count(x)` | Count occurrences of x (list, tuple, string) |
| `index(x)` | First index of x (list, tuple, string) |
| Set `add(x)` | Add x; no error if exists |
| Set `remove(x)` | Remove x; KeyError if not found |
| Set `discard(x)` | Remove x; NO error if not found |
| `union` / `|` | All elements from both sets |
| `intersection` / `&` | Common elements only |
| `difference` / `-` | In self but not in other |
| `symmetric_difference` / `^` | In either but not both |
| Dict `get(k, default)` | Safe access; no KeyError |
| Dict `setdefault(k, v)` | Set only if key doesn't exist |
| Dict `update(other)` | Merge another dict |
| Dict `pop(k)` | Remove & return value; KeyError if missing |
| `Counter` | Count frequencies; `most_common(n)` |
| `defaultdict` | Dict with auto-default factory |
| `deque` | Double-ended queue; O(1) both ends |
| `namedtuple` | Tuple with named fields |
| List comp | `[expr for x in iter if cond]` |
| Dict comp | `{k: v for x in iter if cond}` |
| Set comp | `{expr for x in iter if cond}` |
| Generator | `(expr for x in iter)` — lazy, memory-efficient |
| `x in list` | O(n) — slow |
| `x in set` | O(1) — fast |
| `x in dict` | O(1) — checks keys only |
| Walrus `:=` | Assign + use in same expression (Python 3.8+) |
| Unpacking | `a, *b, c = [1,2,3,4,5]` |
| Swap | `a, b = b, a` |
| `enumerate()` | `(index, value)` pairs |
| `zip()` | Pair elements from multiple iterables |

---

> 💡 **Golden Rules of Python Data Structures:**
> 1. Need fast `in` check? → Use **Set** (O(1) vs List's O(n))
> 2. Need key-value mapping? → Use **Dict**
> 3. Data that shouldn't change? → Use **Tuple** (safer + faster)
> 4. Need both ends efficient? → Use **deque** not **List**
> 5. Counting elements? → Use **Counter**
> 6. Building groups/buckets? → Use **defaultdict**
> 7. Flat structure copy? → `.copy()` is fine; Nested? → `deepcopy()`

---
*Notes prepared with research from: Python Official Docs, Real Python (2025), GeeksForGeeks, Python Data Structures & Algorithms (2025), LeetCode Patterns (2026)*

---
---

# 🐍 Python — Complete Language Notes (Part 2)
> All important Python concepts: Variables, Operators, Loops, Functions, OOP, Exceptions, File Handling & more.

---

## 📌 Part 2 — Table of Contents
17. [Variables, Data Types & Input/Output](#17-variables-data-types--inputoutput)
18. [Operators — All Types](#18-operators--all-types)
19. [Control Flow — if/elif/else](#19-control-flow--ifelifelse)
20. [Loops — for & while](#20-loops--for--while)
21. [Functions — Complete Guide](#21-functions--complete-guide)
22. [Lambda, Map, Filter, Reduce](#22-lambda-map-filter-reduce)
23. [Decorators](#23-decorators)
24. [args and kwargs](#24-args-and-kwargs)
25. [Object-Oriented Programming (OOP)](#25-object-oriented-programming-oop)
26. [Inheritance & Polymorphism](#26-inheritance--polymorphism)
27. [Encapsulation & Abstraction](#27-encapsulation--abstraction)
28. [Iterators & Generators](#28-iterators--generators)
29. [Exception Handling](#29-exception-handling)
30. [File Handling](#30-file-handling)
31. [Important Built-in Functions](#31-important-built-in-functions)
32. [Modules & Packages](#32-modules--packages)

---

## 17. Variables, Data Types & Input/Output

### Variables
A variable is a **named container** that stores a value in memory. Python is dynamically typed — no need to declare the type explicitly.

```python
# Variable assignment
name = "Alice"        # str
age = 25              # int
height = 5.7          # float
is_active = True      # bool
data = None           # NoneType

# Multiple assignment
x = y = z = 0
a, b, c = 1, 2, 3

# Type checking
type(name)            # <class 'str'>
isinstance(age, int)  # True

# Variable naming rules:
# ✅ my_var, _private, var1, CamelCase
# ❌ 1var (starts with digit), my-var (hyphen), class (keyword)
```

### All Data Types in Python

```python
# Numeric Types
x = 10           # int   — whole numbers (unlimited size)
y = 3.14         # float — decimal numbers (64-bit)
z = 3 + 4j       # complex — real + imaginary

# Sequence Types
s = "hello"      # str   — immutable text
lst = [1, 2, 3]  # list  — mutable ordered
t = (1, 2, 3)    # tuple — immutable ordered

# Set Types
s1 = {1, 2, 3}           # set       — mutable, unique
s2 = frozenset({1, 2})   # frozenset — immutable, unique

# Mapping Type
d = {"key": "value"}     # dict — key-value pairs

# Boolean
flag = True    # bool (subclass of int: True=1, False=0)

# None
result = None  # NoneType — absence of value

# Binary Types
b = b"hello"           # bytes     — immutable
ba = bytearray(b"hi")  # bytearray — mutable
mv = memoryview(b)     # memoryview

# Type Conversion
int("42")        # → 42
float("3.14")    # → 3.14
str(100)         # → "100"
bool(0)          # → False
bool("hello")    # → True
list((1,2,3))    # → [1, 2, 3]
tuple([1,2,3])   # → (1, 2, 3)
set([1,1,2,2])   # → {1, 2}
```

### Input & Output

```python
# Output — print()
print("Hello")                        # Hello
print("Name:", "Alice", "Age:", 25)  # Name: Alice Age: 25
print("a", "b", "c", sep="-")        # a-b-c (custom separator)
print("Hello", end=" ")               # no newline at end
print("World")                        # Hello World

# Formatted output
name, score = "Alice", 95.6789
print(f"Name: {name}, Score: {score:.2f}")   # 2 decimal places
print(f"{'Item':<10} {'Price':>8}")          # left/right align

# Input — input() always returns a string
name = input("Enter your name: ")
age  = int(input("Enter your age: "))       # convert to int
x    = float(input("Enter a number: "))     # convert to float

# Multiple inputs in one line
a, b = map(int, input("Enter two numbers: ").split())
nums = list(map(int, input("Enter numbers: ").split()))

# Print without newline + flush (useful in loops)
import sys
sys.stdout.write("Loading...")
sys.stdout.flush()
```

---

## 18. Operators — All Types

### Arithmetic Operators
```python
10 + 3    # → 13   (Addition)
10 - 3    # → 7    (Subtraction)
10 * 3    # → 30   (Multiplication)
10 / 3    # → 3.333 (Division — always float)
10 // 3   # → 3    (Floor Division — integer result)
10 % 3    # → 1    (Modulo — remainder)
10 ** 3   # → 1000 (Exponentiation — power)
-10 // 3  # → -4   (floor towards -infinity)
```

### Comparison Operators
```python
5 == 5    # → True  (equal to)
5 != 4    # → True  (not equal)
5 > 3     # → True  (greater than)
5 < 3     # → False (less than)
5 >= 5    # → True  (greater than or equal)
5 <= 4    # → False (less than or equal)

# Chaining comparisons (Python-specific)
1 < 5 < 10    # → True  (equivalent to 1<5 and 5<10)
10 > 5 > 1    # → True
```

### Logical Operators
```python
True and False   # → False  (both must be True)
True or False    # → True   (at least one True)
not True         # → False  (negation)

# Short-circuit evaluation
0 and print("x")   # print never runs — 0 is falsy, stops at first False
1 or print("x")    # print never runs — 1 is truthy, stops at first True

# Falsy values in Python:
# False, 0, 0.0, "", [], (), {}, set(), None
```

### Assignment Operators
```python
x = 10
x += 5    # x = 15
x -= 3    # x = 12
x *= 2    # x = 24
x /= 4    # x = 6.0
x //= 2   # x = 3.0
x %= 2    # x = 1.0
x **= 3   # x = 1.0
x &= 3    # bitwise AND assign
x |= 3    # bitwise OR assign
x ^= 3    # bitwise XOR assign
x >>= 1   # right shift assign
x <<= 1   # left shift assign
```

### Bitwise Operators
```python
a, b = 0b1100, 0b1010   # 12 and 10 in binary

a & b    # → 8  (0b1000) — AND:  both bits must be 1
a | b    # → 14 (0b1110) — OR:   at least one bit is 1
a ^ b    # → 6  (0b0110) — XOR:  bits are different
~a       # → -13          — NOT:  flips all bits
a << 2   # → 48           — Left shift:  multiply by 2²
a >> 2   # → 3            — Right shift: divide by 2²
```

### Identity & Membership Operators
```python
# Identity — checks if two variables point to SAME object in memory
a = [1, 2, 3]
b = a
c = [1, 2, 3]
a is b     # → True  (same object)
a is c     # → False (equal value but different objects)
a is not c # → True

# Membership — checks if value is IN a collection
3 in [1, 2, 3]          # → True
"a" in "apple"           # → True
"z" not in "apple"       # → True
"key" in {"key": "val"} # → True (checks dict KEYS)
```

### Operator Precedence (High → Low)
```
()         → Parentheses
**         → Exponentiation (right to left)
+x, -x, ~x → Unary operators
*, /, //, % → Multiplication, Division, Floor, Modulo
+, -       → Addition, Subtraction
<<, >>     → Bitwise shift
&          → Bitwise AND
^          → Bitwise XOR
|          → Bitwise OR
==,!=,>,<,>=,<=,is,is not,in,not in → Comparisons
not        → Logical NOT
and        → Logical AND
or         → Logical OR
```

---

## 19. Control Flow — if/elif/else

```python
# Basic if-elif-else
score = 85

if score >= 90:
    print("A grade")
elif score >= 75:
    print("B grade")     # ← this runs
elif score >= 60:
    print("C grade")
else:
    print("Fail")

# One-liner (ternary / conditional expression)
result = "Pass" if score >= 60 else "Fail"
grade = "A" if score >= 90 else "B" if score >= 75 else "C"

# Nested if
age = 20
has_id = True
if age >= 18:
    if has_id:
        print("Entry allowed")
    else:
        print("No ID — denied")
else:
    print("Too young")

# match statement (Python 3.10+) — like switch/case
command = "quit"
match command:
    case "start":
        print("Starting...")
    case "stop" | "quit":    # OR with |
        print("Stopping...")
    case _:                  # default case
        print("Unknown command")

# match with conditions
match score:
    case n if n >= 90:
        print("A")
    case n if n >= 75:
        print("B")
    case _:
        print("C or below")
```

---

## 20. Loops — for & while

### for Loop
```python
# Iterate over a sequence
for i in range(5):          # 0,1,2,3,4
    print(i)

for i in range(1, 6):       # 1,2,3,4,5
    print(i)

for i in range(0, 10, 2):   # 0,2,4,6,8 (step=2)
    print(i)

for i in range(10, 0, -1):  # 10,9,8,...,1 (countdown)
    print(i)

# Iterate over a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Iterate with index
for i, fruit in enumerate(fruits):
    print(i, fruit)   # 0 apple, 1 banana, 2 cherry

for i, fruit in enumerate(fruits, start=1):
    print(i, fruit)   # 1 apple, 2 banana, 3 cherry

# Iterate over string
for char in "hello":
    print(char)

# Iterate over dict
person = {"name": "Alice", "age": 30}
for key in person:                 # keys
    print(key)
for key, val in person.items():    # key-value pairs
    print(f"{key}: {val}")

# Iterate multiple lists together
names = ["Alice", "Bob", "Charlie"]
scores = [85, 92, 78]
for name, score in zip(names, scores):
    print(f"{name}: {score}")

# for-else (else runs if loop completes without break)
for i in range(5):
    if i == 10:
        break
else:
    print("Loop finished without break")  # ← this prints
```

### while Loop
```python
# Basic while
count = 0
while count < 5:
    print(count)
    count += 1

# while with input validation
while True:
    user = input("Enter 'quit' to exit: ")
    if user == "quit":
        break
    print(f"You entered: {user}")

# while-else
n = 5
while n > 0:
    print(n)
    n -= 1
else:
    print("Done!")    # runs after loop finishes normally

# Infinite loop with counter
attempts = 0
while True:
    attempts += 1
    if attempts >= 3:
        print("Max attempts reached")
        break
```

### Loop Control Statements
```python
# break — exit the loop immediately
for i in range(10):
    if i == 5:
        break           # stops at 5; 5,6,7,8,9 not printed
    print(i)

# continue — skip current iteration, go to next
for i in range(10):
    if i % 2 == 0:
        continue        # skip even numbers
    print(i)            # prints: 1,3,5,7,9

# pass — placeholder; does nothing
for i in range(5):
    if i == 3:
        pass            # does nothing, loop continues
    print(i)

# Nested loop with break
found = False
for row in matrix:
    for val in row:
        if val == target:
            found = True
            break       # only breaks inner loop
    if found:
        break           # then break outer loop
```

---

## 21. Functions — Complete Guide

### Defining & Calling Functions

```python
# Basic function
def greet():
    print("Hello, World!")

greet()    # call the function

# Function with parameters
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")    # positional argument

# Function with return value
def add(a, b):
    return a + b

result = add(3, 4)    # → 7

# Return multiple values (returns a tuple)
def min_max(lst):
    return min(lst), max(lst)

low, high = min_max([3, 1, 4, 1, 5, 9])
```

### Parameter Types

```python
# 1. Default parameters
def greet(name, message="Hello"):
    print(f"{message}, {name}!")

greet("Alice")                # "Hello, Alice!"
greet("Bob", "Good morning") # "Good morning, Bob!"

# 2. Keyword arguments (named)
def describe(name, age, city):
    print(f"{name}, {age}, from {city}")

describe(age=30, city="Mumbai", name="Alice")  # any order with keywords

# 3. Positional-only (Python 3.8+) — before /
def pos_only(x, y, /):
    return x + y
# pos_only(x=1, y=2)  → TypeError!

# 4. Keyword-only — after *
def kw_only(*, name, age):
    return f"{name} is {age}"
# kw_only("Alice", 30)  → TypeError!
kw_only(name="Alice", age=30)  # ✅

# 5. Mixed
def mixed(pos1, pos2, /, normal, *, kw_only):
    pass
```

### Variable Scope (LEGB Rule)
```python
x = "global"     # G — Global scope

def outer():
    x = "outer"  # E — Enclosing scope

    def inner():
        x = "inner"  # L — Local scope
        print(x)     # "inner"

    inner()
    print(x)     # "outer"

outer()
print(x)         # "global"

# L — Local
# E — Enclosing (outer function)
# G — Global
# B — Built-in (len, print, etc.)

# global keyword — access/modify global inside function
count = 0
def increment():
    global count
    count += 1

increment()
print(count)   # 1

# nonlocal keyword — access/modify enclosing function's variable
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 5
    inner()
    print(x)   # 15
```

### Recursion
```python
# A function that calls itself
def factorial(n):
    if n == 0 or n == 1:    # base case — must have one!
        return 1
    return n * factorial(n - 1)   # recursive case

factorial(5)   # 5 * 4 * 3 * 2 * 1 = 120

# Fibonacci
def fib(n):
    if n <= 1:
        return n
    return fib(n-1) + fib(n-2)

# Binary search (recursive)
def binary_search(arr, target, low, high):
    if low > high:
        return -1
    mid = (low + high) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid+1, high)
    else:
        return binary_search(arr, target, low, mid-1)

# Set recursion limit
import sys
sys.setrecursionlimit(10000)   # default is 1000
```

### First-Class Functions
```python
# Functions are objects — can be assigned, passed, returned

# Assign to variable
def say_hello():
    return "Hello!"
greet = say_hello     # no () — just reference
greet()               # "Hello!"

# Pass as argument (Higher-order function)
def apply(func, value):
    return func(value)

apply(str.upper, "hello")   # "HELLO"
apply(len, "python")        # 6

# Return from function (closure)
def make_multiplier(factor):
    def multiply(x):
        return x * factor    # factor is captured from outer scope
    return multiply           # returns the function itself

double = make_multiplier(2)
triple = make_multiplier(3)
double(5)    # 10
triple(5)    # 15
```

---

## 22. Lambda, Map, Filter, Reduce

### Lambda Functions
A **lambda** is an anonymous (nameless), single-expression function.

```python
# Syntax: lambda parameters: expression

# Regular function
def square(x):
    return x ** 2

# Equivalent lambda
square = lambda x: x ** 2
square(5)   # 25

# Multiple parameters
add = lambda x, y: x + y
add(3, 4)   # 7

# With conditional
classify = lambda x: "even" if x % 2 == 0 else "odd"
classify(4)   # "even"

# Most useful with sort, filter, map
students = [{"name": "Alice", "grade": 85}, {"name": "Bob", "grade": 92}]
students.sort(key=lambda s: s["grade"], reverse=True)   # sort by grade

pairs = [(1, "b"), (2, "a"), (3, "c")]
sorted(pairs, key=lambda p: p[1])   # sort by second element
```

### map() — Apply Function to Every Element
```python
# map(function, iterable) → returns a map object (iterator)
numbers = [1, 2, 3, 4, 5]

# Square each element
squares = list(map(lambda x: x**2, numbers))
# → [1, 4, 9, 16, 25]

# Convert strings to integers
str_nums = ["1", "2", "3", "4"]
ints = list(map(int, str_nums))
# → [1, 2, 3, 4]

# With multiple iterables
a = [1, 2, 3]
b = [4, 5, 6]
sums = list(map(lambda x, y: x + y, a, b))
# → [5, 7, 9]

# map with named function
names = ["alice", "bob", "charlie"]
caps = list(map(str.capitalize, names))
# → ["Alice", "Bob", "Charlie"]

# Alternative: list comprehension (usually preferred)
squares = [x**2 for x in numbers]
```

### filter() — Keep Elements that Pass a Test
```python
# filter(function, iterable) → returns filter object (iterator)
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# Keep only even numbers
evens = list(filter(lambda x: x % 2 == 0, numbers))
# → [2, 4, 6, 8, 10]

# Keep only positive numbers
mixed = [-3, -1, 0, 2, 5, -2, 8]
positives = list(filter(lambda x: x > 0, mixed))
# → [2, 5, 8]

# filter with None removes falsy values
values = [0, 1, "", "hello", None, True, False, [], [1,2]]
truthy = list(filter(None, values))
# → [1, "hello", True, [1,2]]

# Alternative: list comprehension
evens = [x for x in numbers if x % 2 == 0]
```

### reduce() — Accumulate to a Single Value
```python
from functools import reduce

numbers = [1, 2, 3, 4, 5]

# Sum all elements
total = reduce(lambda acc, x: acc + x, numbers)
# step by step: ((((1+2)+3)+4)+5) = 15

# Product
product = reduce(lambda acc, x: acc * x, numbers)
# → 120

# Find maximum
max_val = reduce(lambda a, b: a if a > b else b, numbers)
# → 5

# With initial value
reduce(lambda acc, x: acc + x, numbers, 100)
# → 115 (starts from 100)

# Flatten a list of lists
nested = [[1,2],[3,4],[5,6]]
flat = reduce(lambda a, b: a + b, nested)
# → [1, 2, 3, 4, 5, 6]
```

### Comparison Table
| Function | Input | Output | Purpose |
|----------|-------|--------|---------|
| `map(f, iter)` | Iterable | New iterable | Transform every element |
| `filter(f, iter)` | Iterable | Subset iterable | Keep elements that return True |
| `reduce(f, iter)` | Iterable | Single value | Accumulate to one result |
| List comprehension | Iterable | List | Transform + filter (usually preferred) |

---

## 23. Decorators

A **decorator** is a function that wraps another function to add extra behavior — without modifying the original function's code.

```python
# Concept: functions as arguments
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function runs")
        result = func(*args, **kwargs)    # call original function
        print("After function runs")
        return result
    return wrapper

# Apply with @syntax (syntactic sugar)
@my_decorator
def say_hello(name):
    print(f"Hello, {name}!")
    return f"Hello, {name}!"

say_hello("Alice")
# Output:
# Before function runs
# Hello, Alice!
# After function runs
```

### Practical Decorator Examples

```python
import time
import functools

# 1. Timer decorator — measure execution time
def timer(func):
    @functools.wraps(func)    # preserves original function's metadata
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end-start:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "done"

slow_function()   # "slow_function took 1.0012s"

# 2. Logger decorator
def logger(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@logger
def add(a, b):
    return a + b

add(3, 4)
# "Calling add with args=(3, 4), kwargs={}"
# "add returned 7"

# 3. Retry decorator
def retry(max_times=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_times):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt+1} failed: {e}")
                    if attempt == max_times - 1:
                        raise
        return wrapper
    return decorator

@retry(max_times=3)
def unreliable_api():
    import random
    if random.random() < 0.7:
        raise ConnectionError("Network error")
    return "Success!"

# 4. Memoize decorator — cache results
def memoize(func):
    cache = {}
    @functools.wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

@memoize
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

fib(50)   # instant! without memoize it would be very slow

# Built-in: functools.lru_cache
from functools import lru_cache

@lru_cache(maxsize=128)
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# 5. Stacking multiple decorators
@timer
@logger
def compute(x, y):
    return x ** y

# Applied bottom-up: logger first, then timer wraps the result
```

---

## 24. args and kwargs

### *args — Variable Positional Arguments
```python
# *args collects extra positional arguments into a TUPLE
def add_all(*args):
    print(type(args))     # <class 'tuple'>
    return sum(args)

add_all(1, 2, 3)          # → 6
add_all(1, 2, 3, 4, 5)   # → 15
add_all()                 # → 0

# Mix with regular parameters
def log(level, *messages):
    for msg in messages:
        print(f"[{level}] {msg}")

log("INFO", "Server started", "Connected to DB", "Ready")

# Unpacking into function call
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
add(*nums)    # same as add(1, 2, 3)
```

### **kwargs — Variable Keyword Arguments
```python
# **kwargs collects extra keyword arguments into a DICT
def display_info(**kwargs):
    print(type(kwargs))     # <class 'dict'>
    for key, value in kwargs.items():
        print(f"  {key}: {value}")

display_info(name="Alice", age=30, city="Mumbai")
# name: Alice
# age: 30
# city: Mumbai

# Mix with regular parameters
def create_user(username, email, **extra):
    user = {"username": username, "email": email}
    user.update(extra)
    return user

create_user("alice", "alice@mail.com", age=30, role="admin")

# Unpacking dict into function call
info = {"name": "Alice", "age": 30}
def greet(name, age):
    print(f"Hi {name}, you are {age}")

greet(**info)   # same as greet(name="Alice", age=30)
```

### Combining All Parameter Types
```python
# Order MUST be: regular → *args → keyword_only → **kwargs
def full_example(a, b, *args, key1="default", **kwargs):
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"key1={key1}")
    print(f"kwargs={kwargs}")

full_example(1, 2, 3, 4, 5, key1="custom", extra="value")
# a=1, b=2
# args=(3, 4, 5)
# key1=custom
# kwargs={'extra': 'value'}
```

---

## 25. Object-Oriented Programming (OOP)

### Classes & Objects

```python
# Class definition
class Dog:
    # Class variable (shared by ALL instances)
    species = "Canis lupus familiaris"

    # __init__ = constructor (called when object is created)
    def __init__(self, name, age, breed):
        # Instance variables (unique per object)
        self.name = name       # self = reference to current instance
        self.age = age
        self.breed = breed

    # Instance method
    def bark(self):
        return f"{self.name} says: Woof!"

    def info(self):
        return f"{self.name} ({self.breed}), {self.age} years old"

    # String representation
    def __str__(self):        # for print() and str()
        return f"Dog({self.name})"

    def __repr__(self):       # for developers, repr()
        return f"Dog(name='{self.name}', age={self.age})"

    # Class method (works on class, not instance)
    @classmethod
    def from_string(cls, dog_string):
        name, age = dog_string.split("-")
        return cls(name, int(age), "Unknown")

    # Static method (no access to class or instance)
    @staticmethod
    def is_adult(age):
        return age >= 2


# Creating objects (instances)
dog1 = Dog("Buddy", 3, "Labrador")
dog2 = Dog("Max", 1, "Poodle")

# Accessing attributes and methods
dog1.name        # "Buddy"
dog1.bark()      # "Buddy says: Woof!"
dog1.info()      # "Buddy (Labrador), 3 years old"
print(dog1)      # "Dog(Buddy)"

# Class method
dog3 = Dog.from_string("Charlie-5")

# Static method
Dog.is_adult(3)   # True
```

### Magic / Dunder Methods
```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):      # str(obj), print(obj)
        return f"Vector({self.x}, {self.y})"

    def __repr__(self):     # repr(obj), developer view
        return f"Vector(x={self.x}, y={self.y})"

    def __add__(self, other):   # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):   # v1 - v2
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar):  # v * 3
        return Vector(self.x * scalar, self.y * scalar)

    def __eq__(self, other):    # v1 == v2
        return self.x == other.x and self.y == other.y

    def __lt__(self, other):    # v1 < v2 (enables sorting)
        return abs(self) < abs(other)

    def __len__(self):          # len(obj)
        return 2

    def __abs__(self):          # abs(obj)
        return (self.x**2 + self.y**2) ** 0.5

    def __bool__(self):         # bool(obj), if obj:
        return self.x != 0 or self.y != 0

    def __getitem__(self, index):   # obj[0]
        return (self.x, self.y)[index]

    def __contains__(self, val):    # val in obj
        return val in (self.x, self.y)

    def __iter__(self):         # for item in obj:
        yield self.x
        yield self.y

    def __call__(self, *args):  # obj()
        return self.x * self.y

# Usage
v1 = Vector(2, 3)
v2 = Vector(1, 4)
v1 + v2      # Vector(3, 7)
v1 * 3       # Vector(6, 9)
len(v1)      # 2
abs(v1)      # 3.605...
v1 == v2     # False
2 in v1      # True
```

---

## 26. Inheritance & Polymorphism

### Inheritance
```python
# Parent class (Base class)
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def speak(self):
        return "..."    # to be overridden

    def info(self):
        return f"{self.name} (age {self.age})"

    def __str__(self):
        return f"{type(self).__name__}({self.name})"


# Child class inherits from Animal
class Dog(Animal):
    def __init__(self, name, age, breed):
        super().__init__(name, age)   # call parent's __init__
        self.breed = breed

    def speak(self):                  # Method Overriding
        return f"{self.name} says: Woof!"

    def fetch(self):                  # New method
        return f"{self.name} fetches the ball!"


class Cat(Animal):
    def speak(self):
        return f"{self.name} says: Meow!"


class Bird(Animal):
    def speak(self):
        return f"{self.name} says: Tweet!"

    def fly(self):
        return f"{self.name} is flying!"


# Creating objects
dog = Dog("Buddy", 3, "Labrador")
cat = Cat("Whiskers", 5)
bird = Bird("Tweety", 2)

dog.speak()    # "Buddy says: Woof!"
cat.speak()    # "Whiskers says: Meow!"
dog.info()     # inherited from Animal: "Buddy (age 3)"

# isinstance() and issubclass()
isinstance(dog, Dog)     # True
isinstance(dog, Animal)  # True — Dog IS-A Animal
isinstance(cat, Dog)     # False
issubclass(Dog, Animal)  # True
issubclass(Animal, Dog)  # False
```

### Polymorphism
```python
# Same method name, different behavior per class

# 1. Method Overriding — child redefines parent method
animals = [Dog("Buddy",3,"Lab"), Cat("Whiskers",5), Bird("Tweety",2)]

for animal in animals:
    print(animal.speak())   # Each calls its OWN speak() — polymorphism!
# "Buddy says: Woof!"
# "Whiskers says: Meow!"
# "Tweety says: Tweet!"

# 2. Duck Typing — if it walks like a duck and quacks like a duck...
class Robot:
    def speak(self):
        return "Beep boop!"

things = [Dog("Buddy",3,"Lab"), Robot()]
for thing in things:
    print(thing.speak())   # Works! Python doesn't care about type
                           # as long as the method exists

# 3. Operator Overloading (already seen with __add__, etc.)
```

### Multiple Inheritance & MRO
```python
class A:
    def method(self):
        return "A"

class B(A):
    def method(self):
        return "B"

class C(A):
    def method(self):
        return "C"

class D(B, C):      # Multiple inheritance
    pass

d = D()
d.method()    # → "B" (follows MRO: D → B → C → A)

# Method Resolution Order (MRO)
D.__mro__
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)

# super() respects MRO
class E(B, C):
    def method(self):
        return super().method() + " → E"

E().method()   # "B → E"
```

---

## 27. Encapsulation & Abstraction

### Encapsulation — Hiding Internal Details
```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner            # public attribute
        self._account_no = "ACC001"  # protected (convention: single _)
        self.__balance = balance      # private (name-mangled: double __)

    # Getter — controlled access to private attribute
    def get_balance(self):
        return self.__balance

    # Setter — validation before changing
    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount
        return self.__balance

    def withdraw(self, amount):
        if amount > self.__balance:
            raise ValueError("Insufficient funds")
        self.__balance -= amount
        return self.__balance

    # Property — access like an attribute but with getter/setter logic
    @property
    def balance(self):
        return self.__balance

    @balance.setter
    def balance(self, value):
        if value < 0:
            raise ValueError("Balance cannot be negative")
        self.__balance = value


acc = BankAccount("Alice", 1000)
acc.balance           # 1000 (via @property)
acc.deposit(500)      # 1500
acc.withdraw(200)     # 1300
# acc.__balance       # AttributeError! (name-mangled to _BankAccount__balance)
acc._BankAccount__balance   # 1300 (can access but shouldn't)


# @property decorator
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius must be non-negative")
        self._radius = value

    @radius.deleter
    def radius(self):
        del self._radius

    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2   # computed property (no setter)

c = Circle(5)
c.radius         # 5
c.radius = 10    # calls setter
c.area           # 314.15...
```

### Abstraction — Abstract Classes & Methods
```python
from abc import ABC, abstractmethod

# Abstract Base Class — cannot be instantiated directly
class Shape(ABC):
    @abstractmethod
    def area(self):
        pass    # subclasses MUST implement this

    @abstractmethod
    def perimeter(self):
        pass

    def describe(self):    # concrete method (can be used by all subclasses)
        return f"Area: {self.area():.2f}, Perimeter: {self.perimeter():.2f}"


class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):           # MUST implement
        return self.width * self.height

    def perimeter(self):      # MUST implement
        return 2 * (self.width + self.height)


class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

    def perimeter(self):
        import math
        return 2 * math.pi * self.radius


# Shape()   → TypeError: Can't instantiate abstract class Shape
rect = Rectangle(4, 5)
rect.area()        # 20
rect.describe()    # "Area: 20.00, Perimeter: 18.00"
```

---

## 28. Iterators & Generators

### Iterators
```python
# An iterator is any object with __iter__() and __next__() methods

# Built-in iterators
lst = [1, 2, 3]
it = iter(lst)          # create iterator
next(it)                # → 1
next(it)                # → 2
next(it)                # → 3
# next(it)             # → StopIteration error

# Custom iterator class
class CountUp:
    def __init__(self, start, stop):
        self.current = start
        self.stop = stop

    def __iter__(self):
        return self     # iterator returns itself

    def __next__(self):
        if self.current >= self.stop:
            raise StopIteration
        value = self.current
        self.current += 1
        return value

for num in CountUp(1, 6):
    print(num)   # 1, 2, 3, 4, 5
```

### Generators — Lazy Evaluation
```python
# Generator function — uses yield instead of return
# Generates values ONE AT A TIME — very memory efficient

def count_up(start, stop):
    current = start
    while current < stop:
        yield current         # pause here, return value, resume on next()
        current += 1

gen = count_up(1, 6)
next(gen)    # 1
next(gen)    # 2
list(gen)    # [3, 4, 5]  (remaining values)

# Generator for infinite sequences
def infinite_counter(start=0):
    n = start
    while True:
        yield n
        n += 1

counter = infinite_counter()
next(counter)   # 0
next(counter)   # 1
# import itertools; list(itertools.islice(counter, 5))  → [2,3,4,5,6]

# Generator for large file reading (memory efficient)
def read_large_file(filepath):
    with open(filepath) as f:
        for line in f:
            yield line.strip()   # one line at a time, never loads full file

# Generator expression (like list comprehension but lazy)
squares_gen = (x**2 for x in range(1000000))   # ~no memory used
next(squares_gen)   # 0 (compute on demand)

# yield from — delegate to sub-generator
def chain(*iterables):
    for it in iterables:
        yield from it

list(chain([1,2], [3,4], [5,6]))   # [1,2,3,4,5,6]

# Generator vs List
import sys
lst = [x**2 for x in range(10000)]        # stores all 10000 in memory
gen = (x**2 for x in range(10000))        # stores just one at a time
sys.getsizeof(lst)   # ~87616 bytes
sys.getsizeof(gen)   # ~208 bytes!
```

---

## 29. Exception Handling

### What are Exceptions?
An **exception** is an error that occurs at runtime. Without handling, it crashes the program.

```python
# Common built-in exceptions:
# ValueError     — wrong value type (int("abc"))
# TypeError      — wrong type used (1 + "a")
# IndexError     — list index out of range ([1,2][5])
# KeyError       — dict key not found (d["missing"])
# AttributeError — object has no such attribute
# NameError      — variable not defined
# ZeroDivisionError — division by zero
# FileNotFoundError — file does not exist
# ImportError    — module not found
# StopIteration  — iterator exhausted
# RecursionError — max recursion depth exceeded
# MemoryError    — out of memory
# OverflowError  — number too large
# RuntimeError   — generic runtime error
```

### try / except / else / finally

```python
# Basic try-except
try:
    x = int(input("Enter a number: "))
    result = 10 / x
except ValueError:
    print("That's not a valid integer!")
except ZeroDivisionError:
    print("Cannot divide by zero!")
except (TypeError, AttributeError) as e:   # multiple exceptions
    print(f"Type/Attribute error: {e}")
except Exception as e:                     # catch-all (use sparingly)
    print(f"Unexpected error: {type(e).__name__}: {e}")
else:
    print(f"Result: {result}")   # runs ONLY if NO exception occurred
finally:
    print("This ALWAYS runs")    # cleanup: close files, DB connections, etc.

# Accessing exception details
try:
    x = 1 / 0
except ZeroDivisionError as e:
    print(type(e))           # <class 'ZeroDivisionError'>
    print(e.args)            # ('division by zero',)
    print(str(e))            # "division by zero"
    import traceback
    traceback.print_exc()    # full stack trace
```

### raise — Throw Exceptions
```python
# Raise a built-in exception
def divide(a, b):
    if b == 0:
        raise ValueError("Denominator cannot be zero")
    return a / b

# Re-raise the current exception
try:
    result = divide(10, 0)
except ValueError as e:
    print(f"Caught: {e}")
    raise   # re-raises the same exception

# Raise with context
try:
    x = int("abc")
except ValueError as e:
    raise RuntimeError("Failed to parse config") from e
    # "RuntimeError: Failed to parse config"
    # "The above exception was the direct cause of..."
```

### Custom Exceptions
```python
# Define custom exception by inheriting from Exception
class InsufficientFundsError(Exception):
    def __init__(self, amount, balance):
        self.amount = amount
        self.balance = balance
        super().__init__(f"Cannot withdraw {amount}. Balance: {balance}")

class NegativeAmountError(ValueError):
    pass

class BankAccount:
    def __init__(self, balance):
        self.balance = balance

    def withdraw(self, amount):
        if amount < 0:
            raise NegativeAmountError(f"Amount must be positive, got {amount}")
        if amount > self.balance:
            raise InsufficientFundsError(amount, self.balance)
        self.balance -= amount

# Using custom exceptions
try:
    acc = BankAccount(100)
    acc.withdraw(150)
except InsufficientFundsError as e:
    print(f"Error: {e}")
    print(f"Tried to withdraw: {e.amount}, Had: {e.balance}")
except NegativeAmountError as e:
    print(f"Invalid amount: {e}")
```

### Context Manager — with statement
```python
# with statement ensures cleanup happens automatically
# __enter__ runs at start, __exit__ runs at end (even on error)

with open("file.txt", "r") as f:
    data = f.read()
# File is automatically closed here

# Multiple context managers
with open("input.txt") as fin, open("output.txt", "w") as fout:
    fout.write(fin.read().upper())

# Custom context manager using class
class DatabaseConnection:
    def __enter__(self):
        print("Connecting to database...")
        self.conn = connect_to_db()
        return self.conn

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing connection...")
        self.conn.close()
        return False   # False = don't suppress exceptions

with DatabaseConnection() as db:
    db.execute("SELECT * FROM users")

# Custom context manager using contextlib
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    try:
        yield                          # code inside 'with' block runs here
    finally:
        print(f"Time: {time.time()-start:.4f}s")

with timer():
    # some slow operation
    [x**2 for x in range(1000000)]
```

---

## 30. File Handling

### Reading Files
```python
# Open modes:
# "r"  — read (default)
# "w"  — write (creates new / overwrites)
# "a"  — append (adds to end)
# "x"  — exclusive create (fails if exists)
# "b"  — binary mode (use with r/w: "rb", "wb")
# "t"  — text mode (default)
# "+"  — read and write ("r+", "w+", "a+")

# ─── Read entire file ─────────────────────────────────────────────
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()         # entire file as one string
    print(content)

# ─── Read line by line ────────────────────────────────────────────
with open("file.txt", "r") as f:
    for line in f:             # memory efficient — one line at a time
        print(line.strip())    # strip() removes \n at end

# ─── Read all lines into list ─────────────────────────────────────
with open("file.txt", "r") as f:
    lines = f.readlines()      # → ["line1\n", "line2\n", "line3\n"]

with open("file.txt", "r") as f:
    first_line = f.readline()  # read ONE line at a time

# ─── File pointer methods ─────────────────────────────────────────
with open("file.txt", "r") as f:
    content = f.read(100)      # read first 100 characters
    f.seek(0)                  # move pointer to position 0 (beginning)
    f.tell()                   # current pointer position
    f.read()                   # read from current position
```

### Writing Files
```python
# ─── Write (overwrites if exists) ────────────────────────────────
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("Hello, World!\n")
    f.write("Second line\n")

# ─── Write multiple lines ─────────────────────────────────────────
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("output.txt", "w") as f:
    f.writelines(lines)           # writelines — no separator added!

# ─── Append (adds to end, doesn't overwrite) ─────────────────────
with open("log.txt", "a") as f:
    f.write(f"New log entry: {datetime.now()}\n")

# ─── Read and Write ───────────────────────────────────────────────
with open("file.txt", "r+") as f:
    content = f.read()
    f.seek(0)
    f.write(content.upper())
```

### Working with CSV Files
```python
import csv

# Write CSV
data = [["Name", "Age", "City"],
        ["Alice", 30, "Mumbai"],
        ["Bob", 25, "Delhi"]]

with open("data.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerows(data)

# Read CSV
with open("data.csv", "r") as f:
    reader = csv.reader(f)
    headers = next(reader)          # skip header row
    for row in reader:
        print(row)

# Dict-based CSV
with open("data.csv", "w", newline="") as f:
    fieldnames = ["Name", "Age", "City"]
    writer = csv.DictWriter(f, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerow({"Name": "Alice", "Age": 30, "City": "Mumbai"})

with open("data.csv", "r") as f:
    for row in csv.DictReader(f):
        print(row["Name"], row["Age"])
```

### Working with JSON Files
```python
import json

data = {"name": "Alice", "age": 30, "scores": [85, 90, 92]}

# Write JSON
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# Read JSON
with open("data.json", "r") as f:
    loaded = json.load(f)

# String conversions
json_str = json.dumps(data, indent=2)   # dict → JSON string
parsed  = json.loads(json_str)          # JSON string → dict
```

### OS Module — File System Operations
```python
import os

os.getcwd()                          # current working directory
os.chdir("/path/to/dir")             # change directory
os.listdir(".")                      # list files/dirs in directory
os.path.exists("file.txt")           # does path exist?
os.path.isfile("file.txt")           # is it a file?
os.path.isdir("mydir")               # is it a directory?
os.path.join("folder", "file.txt")  # platform-safe path join
os.path.split("/dir/file.txt")       # → ("/dir", "file.txt")
os.path.basename("/dir/file.txt")    # → "file.txt"
os.path.dirname("/dir/file.txt")     # → "/dir"
os.path.getsize("file.txt")         # file size in bytes
os.path.abspath("file.txt")          # absolute path

os.mkdir("newdir")                   # create directory
os.makedirs("a/b/c", exist_ok=True) # create nested directories
os.rename("old.txt", "new.txt")      # rename file/dir
os.remove("file.txt")                # delete a file
os.rmdir("emptydir")                 # delete empty directory

import shutil
shutil.copy("src.txt", "dst.txt")    # copy file
shutil.copytree("src_dir", "dst_dir") # copy entire directory
shutil.rmtree("dir")                  # delete directory (even non-empty)
shutil.move("src", "dst")            # move file or directory

# Walk directory tree
for dirpath, dirnames, filenames in os.walk("."):
    for filename in filenames:
        filepath = os.path.join(dirpath, filename)
        print(filepath)

# pathlib (modern, OOP approach — Python 3.4+)
from pathlib import Path

p = Path(".")
list(p.iterdir())              # list contents
list(p.glob("*.txt"))          # find all .txt files
list(p.rglob("*.py"))          # recursive search

p = Path("data") / "output" / "file.txt"  # path joining with /
p.exists()
p.is_file()
p.parent          # → Path("data/output")
p.name            # → "file.txt"
p.stem            # → "file"
p.suffix          # → ".txt"
p.mkdir(parents=True, exist_ok=True)
p.write_text("Hello!")
p.read_text()
p.unlink()        # delete file
```

---

## 31. Important Built-in Functions

```python
# ─── Type Conversion ─────────────────────────────────────────────
int(3.7)          # → 3 (truncates, does NOT round)
int("42")         # → 42
float("3.14")     # → 3.14
str(100)          # → "100"
bool(0)           # → False (falsy: 0, "", [], {}, None, False)
bool("hello")     # → True
list("abc")       # → ['a','b','c']
tuple([1,2,3])    # → (1,2,3)
set([1,1,2])      # → {1,2}
dict(a=1, b=2)    # → {'a':1,'b':2}

# ─── Math Functions ───────────────────────────────────────────────
abs(-5)           # → 5 (absolute value)
round(3.567, 2)   # → 3.57 (round to 2 decimal places)
round(3.5)        # → 4 (rounds to nearest even — banker's rounding)
pow(2, 10)        # → 1024 (same as 2**10)
pow(2, 10, 1000)  # → 24 (modular exponentiation: 2^10 % 1000)
divmod(17, 5)     # → (3, 2) — (quotient, remainder) in one call
max(1, 2, 3)      # → 3
max([1,2,3])      # → 3
min(1, 2, 3)      # → 1
sum([1,2,3,4,5])  # → 15
sum([1,2,3], 10)  # → 16 (start=10)

# ─── Sequence Functions ───────────────────────────────────────────
len([1,2,3])          # → 3
sorted([3,1,2])        # → [1,2,3] (returns new list)
sorted("hello")        # → ['e','h','l','l','o']
reversed([1,2,3])      # → iterator (use list() to convert)
list(reversed([1,2,3])) # → [3,2,1]
enumerate(["a","b"],1) # → (1,"a"),(2,"b")
zip([1,2],[3,4])       # → (1,3),(2,4)
zip(*[[1,2],[3,4]])    # transpose: (1,3),(2,4)

# ─── Object Inspection ────────────────────────────────────────────
type(42)              # <class 'int'>
isinstance(42, int)   # True
isinstance(42, (int, float))  # True — check against multiple types
issubclass(bool, int) # True
id(obj)               # memory address (unique identity)
dir(obj)              # list all attributes and methods of obj
help(str.split)       # documentation
vars(obj)             # object's __dict__
hasattr(obj,"method") # does obj have this attribute?
getattr(obj,"name")   # get attribute by string name
setattr(obj,"name","Alice")  # set attribute by string name
delattr(obj,"name")   # delete attribute by string name

# ─── Iteration Helpers ────────────────────────────────────────────
any([0, 0, 1, 0])     # → True  (at least one truthy)
all([1, 2, 3])        # → True  (all truthy)
any([])               # → False (empty iterable)
all([])               # → True  (vacuously true)

map(str, [1,2,3])     # → iterator of ["1","2","3"]
filter(None, [0,1,2]) # → iterator of [1,2]

# ─── Functional ───────────────────────────────────────────────────
from functools import reduce
reduce(lambda a,b: a+b, [1,2,3,4])   # → 10

# ─── Input/Output ─────────────────────────────────────────────────
print(*[1,2,3], sep=", ")  # → "1, 2, 3"
input("Enter: ")           # reads string from user

# ─── Range ────────────────────────────────────────────────────────
range(5)          # 0,1,2,3,4
range(1,6)        # 1,2,3,4,5
range(0,10,2)     # 0,2,4,6,8
range(10,0,-1)    # 10,9,...,1
list(range(5))    # [0,1,2,3,4]

# ─── String/Repr ──────────────────────────────────────────────────
repr("hello")     # → "'hello'" (unambiguous representation)
ascii("héllo")    # → "'h\\xe9llo'" (ASCII version)
chr(65)           # → 'A' (code point → character)
ord('A')          # → 65 (character → code point)
hex(255)          # → '0xff'
oct(8)            # → '0o10'
bin(10)           # → '0b1010'
format(3.14, ".2f")  # → "3.14"

# ─── Callable & Execution ─────────────────────────────────────────
callable(print)   # → True (is it callable?)
eval("2 + 2")     # → 4 (evaluate Python expression string)
exec("x = 10")    # execute Python statement string (no return)
compile("x=1","<string>","exec")  # compile to code object

# ─── Hash & Identity ──────────────────────────────────────────────
hash("hello")     # integer hash value (used by sets/dicts internally)
hash(42)          # same as 42 for ints
id([1,2,3])       # memory address

# ─── Numeric Bases ────────────────────────────────────────────────
int("ff", 16)     # → 255 (hex string → int)
int("1010", 2)    # → 10  (binary string → int)
int("17", 8)      # → 15  (octal string → int)
```

---

## 32. Modules & Packages

### Importing Modules
```python
# Import entire module
import math
math.sqrt(16)    # 4.0
math.pi          # 3.14159...
math.ceil(4.2)   # 5
math.floor(4.9)  # 4
math.factorial(5) # 120

# Import specific items
from math import sqrt, pi, ceil
sqrt(16)   # 4.0 (no math. prefix needed)

# Import with alias
import numpy as np
import pandas as pd
from collections import defaultdict as dd

# Import all (avoid in production — pollutes namespace)
from math import *

# Check what's available in a module
import math
dir(math)   # list all attributes/functions
help(math)  # documentation
```

### Commonly Used Standard Library Modules

```python
# ─── math ────────────────────────────────────────────────────────
import math
math.sqrt(25)         # 5.0
math.pow(2, 8)        # 256.0
math.log(100, 10)     # 2.0
math.log2(8)          # 3.0
math.log10(1000)      # 3.0
math.gcd(12, 8)       # 4
math.lcm(4, 6)        # 12 (Python 3.9+)
math.inf              # infinity
math.isnan(float('nan'))  # True
math.isfinite(math.inf)   # False
math.sin(math.pi/2)   # 1.0
math.degrees(math.pi) # 180.0
math.radians(180)     # 3.14159...
math.e                # 2.71828... (Euler's number)

# ─── random ──────────────────────────────────────────────────────
import random
random.random()           # float between 0.0 and 1.0
random.randint(1, 10)     # int between 1 and 10 (inclusive)
random.uniform(1.0, 5.0)  # float between 1.0 and 5.0
random.choice([1,2,3,4])  # random element from sequence
random.choices([1,2,3], k=5)        # with replacement
random.sample([1,2,3,4,5], k=3)     # without replacement
random.shuffle([1,2,3,4,5])          # shuffle list IN-PLACE
random.seed(42)           # for reproducibility

# ─── datetime ────────────────────────────────────────────────────
from datetime import datetime, date, timedelta

now = datetime.now()
today = date.today()
print(now.strftime("%Y-%m-%d %H:%M:%S"))  # format
dt = datetime.strptime("2026-05-01", "%Y-%m-%d")  # parse

now.year, now.month, now.day
now.hour, now.minute, now.second

delta = timedelta(days=30)
future = today + delta
diff = datetime(2026,12,31) - datetime(2026,1,1)
diff.days   # 364

# ─── os ──────────────────────────────────────────────────────────
import os
os.environ.get("HOME")     # get environment variable
os.environ["PATH"]         # access env var (KeyError if missing)
os.system("ls -la")        # run shell command
os.getpid()                # current process ID
os.cpu_count()             # number of CPUs

# ─── sys ─────────────────────────────────────────────────────────
import sys
sys.argv          # command-line arguments list
sys.argv[0]       # script name
sys.path          # Python module search paths
sys.version       # Python version string
sys.platform      # "linux", "win32", "darwin"
sys.exit(0)       # exit program (0 = success, 1 = error)
sys.stdin, sys.stdout, sys.stderr  # standard streams

# ─── re — Regular Expressions ────────────────────────────────────
import re

text = "Hello, my email is alice@example.com and age is 30"

re.search(r'\d+', text)          # first match → Match object
re.findall(r'\d+', text)         # all matches → ['30']
re.sub(r'\d+', 'NUM', text)      # replace → "...age is NUM"
re.split(r'\s+', text)           # split on whitespace
re.match(r'Hello', text)         # match at START of string only
re.fullmatch(r'\d+', "123")      # must match ENTIRE string

# Compiled pattern (for repeated use)
pattern = re.compile(r'[a-z]+@[a-z]+\.[a-z]+')
pattern.findall(text)   # → ['alice@example.com']

# Common patterns
r'\d'      # any digit [0-9]
r'\D'      # not digit
r'\w'      # word char [a-zA-Z0-9_]
r'\W'      # not word char
r'\s'      # whitespace
r'\S'      # not whitespace
r'^hello'  # starts with hello
r'end$'    # ends with end
r'a.c'     # a + any char + c
r'a*'      # 0 or more a
r'a+'      # 1 or more a
r'a?'      # 0 or 1 a
r'a{3}'    # exactly 3 a
r'a{2,5}'  # 2 to 5 a

# ─── collections ─────────────────────────────────────────────────
from collections import Counter, defaultdict, deque, namedtuple, OrderedDict
# (covered in detail in section 13)

# ─── itertools ────────────────────────────────────────────────────
import itertools
# (covered in detail in section 13)

# ─── functools ────────────────────────────────────────────────────
from functools import reduce, lru_cache, partial, wraps

# partial — pre-fill some arguments
from functools import partial
def multiply(x, y): return x * y
double = partial(multiply, y=2)
double(5)   # 10

# ─── json ─────────────────────────────────────────────────────────
import json
# (covered in file handling section)

# ─── time ────────────────────────────────────────────────────────
import time
time.time()         # Unix timestamp (seconds since 1970)
time.sleep(2)       # pause for 2 seconds
time.perf_counter() # high-resolution timer for benchmarking

# ─── copy ────────────────────────────────────────────────────────
import copy
copy.copy(obj)       # shallow copy
copy.deepcopy(obj)   # deep copy
```

### Creating Your Own Module
```python
# mymodule.py
def greet(name):
    return f"Hello, {name}!"

PI = 3.14159

class Calculator:
    def add(self, a, b):
        return a + b

# In another file:
import mymodule
mymodule.greet("Alice")   # "Hello, Alice!"
mymodule.PI               # 3.14159

from mymodule import greet, PI
greet("Bob")

# __name__ == "__main__" — code only runs when file is executed directly
if __name__ == "__main__":
    print("Running as main script")
    # This block does NOT run when the file is imported as a module
```

### Package Structure
```
mypackage/
├── __init__.py        ← makes folder a package (can be empty)
├── module1.py
├── module2.py
└── subpackage/
    ├── __init__.py
    └── module3.py
```
```python
# Importing from a package
from mypackage import module1
from mypackage.module1 import my_function
from mypackage.subpackage.module3 import another_function
```

---

## 📝 Part 2 — Quick Revision Card

| Concept | In One Line |
|---------|-------------|
| Variables | Named containers; dynamically typed; no declaration needed |
| `type()` | Returns the type of an object |
| `isinstance()` | Check if object is instance of a type |
| `//` | Floor division — integer result |
| `**` | Exponentiation — power |
| `%` | Modulo — remainder |
| `and/or/not` | Logical operators; short-circuit evaluated |
| `is` | Identity check — same object in memory |
| `in` | Membership check — value exists in collection |
| `if/elif/else` | Conditional branching |
| `match` | Switch-like pattern matching (Python 3.10+) |
| `for` | Iterate over any iterable |
| `while` | Loop while condition is True |
| `break` | Exit loop immediately |
| `continue` | Skip current iteration |
| `pass` | Placeholder — does nothing |
| `def` | Define a function |
| `return` | Return value(s) from function |
| `global` | Access/modify global variable inside function |
| `nonlocal` | Access/modify enclosing function's variable |
| `*args` | Collect extra positional args into tuple |
| `**kwargs` | Collect extra keyword args into dict |
| `lambda` | Anonymous single-expression function |
| `map()` | Apply function to every element |
| `filter()` | Keep elements where function returns True |
| `reduce()` | Accumulate iterable to single value |
| Decorator | Wrap function to add behavior; uses `@` syntax |
| `@property` | Turn method into attribute-style access |
| `class` | Blueprint for creating objects |
| `self` | Reference to current instance in methods |
| `__init__` | Constructor — called on object creation |
| `__str__` | String representation for print() |
| `__repr__` | Developer representation for repr() |
| `super()` | Call parent class method |
| `@classmethod` | Method bound to class, not instance |
| `@staticmethod` | Method not bound to class or instance |
| Inheritance | Child class inherits parent's attributes/methods |
| Polymorphism | Same method name, different behavior per class |
| `@abstractmethod` | Must be overridden in subclasses |
| Encapsulation | Hide internal details; `_` protected, `__` private |
| `@property` | Getter; use `.setter` and `.deleter` for validation |
| Iterator | Object with `__iter__` and `__next__` methods |
| Generator | Function with `yield`; lazy evaluation; memory-efficient |
| `yield` | Pause generator, return value, resume on next() |
| `try/except` | Handle exceptions gracefully |
| `raise` | Throw an exception |
| `finally` | Always runs — cleanup code |
| `with` | Context manager — auto cleanup (`__enter__/__exit__`) |
| Custom Exception | Inherit from `Exception` class |
| `open()` | Open file; always use `with` |
| `"r"/"w"/"a"` | Read / Write / Append file modes |
| `json.dump/load` | Write/read JSON files |
| `csv.writer/reader` | Write/read CSV files |
| `os.path.join()` | Platform-safe path building |
| `pathlib.Path` | Modern OOP file system operations |
| `import` | Load a module |
| `from x import y` | Import specific item from module |
| `if __name__=="__main__"` | Code runs only when script executed directly |

---

> 💡 **Golden Rules of Python Programming:**
> 1. **Readability counts** — write code that humans can read, not just computers
> 2. **Don't repeat yourself (DRY)** — use functions, loops, comprehensions
> 3. **Explicit is better than implicit** — clear code beats clever code
> 4. **Errors should never pass silently** — always handle exceptions
> 5. **Use `with` for file/resource handling** — never forget to close
> 6. **Prefer generators over lists** — when you only iterate once
> 7. **Use `isinstance()` not `type()`** — works with inheritance too
> 8. **`_single` = protected; `__double` = private** — convention matters

---
*Extended notes compiled from: Python Official Docs (3.13), Real Python (2025-2026), GeeksForGeeks Python Tutorial (2026), Python Cookbook (O'Reilly), Fluent Python (2nd Edition)*
