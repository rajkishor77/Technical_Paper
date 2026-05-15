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
