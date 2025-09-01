# 🐍 Python Data Structures Cheat Sheet


## 📘 Python List 
---
🔹 1. What is a List?

    - A list is an ordered, mutable (changeable), iterable collection in Python.
  
    - It can hold different data types (integer, float, string, boolean, object, even another list).
  
    - Lists are defined using square brackets [ ].
  
Example:

            my_list = [1, 2.5, "apple", True, [10, 20]]
---

```
| Method                          | Description                                  | Example                         |
| ------------------------------- | -------------------------------------------- | ------------------------------- |
| `append(x)`                     | Add `x` to end                               | `[1,2].append(3)` → `[1,2,3]`   |
| `extend(iterable)`              | Add all items from iterable                  | `[1].extend([2,3])` → `[1,2,3]` |
| `insert(i, x)`                  | Insert `x` at index `i`                      | `[1,3].insert(1,2)` → `[1,2,3]` |
| `remove(x)`                     | Remove first occurrence of `x`               | `[1,2,3].remove(2)` → `[1,3]`   |
| `pop(i=-1)`                     | Remove and return item at `i` (default last) | `[1,2,3].pop()` → `3`           |
| `clear()`                       | Remove all elements                          | `[1,2].clear()` → `[]`          |
| `index(x, start, end)`          | Return first index of `x`                    | `[1,2,3].index(2)` → `1`        |
| `count(x)`                      | Count occurrences of `x`                     | `[1,2,2].count(2)` → `2`        |
| `sort(key=None, reverse=False)` | Sort list                                    | `[3,1].sort()` → `[1,3]`        |
| `reverse()`                     | Reverse the list                             | `[1,2].reverse()` → `[2,1]`     |
| `copy()`                        | Return shallow copy                          | `[1,2].copy()` → `[1,2]`        |
| `len(list)`                     | Length of list                               | `len([1,2,3])` → `3`            |
| `sum(list)`                     | Sum of numeric elements                      | `sum([1,2,3])` → `6`            |
| `min(list)`                     | Minimum element                              | `min([1,2,3])` → `1`            |
| `max(list)`                     | Maximum element                              | `max([1,2,3])` → `3`            |
```


## 📘 Set in Python – Detailed Explanation
---
🔹 Definition

    - A Set is an unordered, unindexed, mutable collection of unique elements.
  
    - Duplicates are automatically removed.
  
    - Mutable: Elements can be added or removed (but set itself can’t contain mutable objects like lists/dictionaries).
  
    - Supports mathematical operations like union, intersection, difference.
  
Syntax:

        my_set = {1, 2, 3, 4}

        empty_set = set()   # correct way ({} creates a dictionary)
---
```
| Method                          | Description                            | Example                          |       |                   |
| ------------------------------- | -------------------------------------- | -------------------------------- | ----- | ----------------- |
| `add(x)`                        | Add element `x`                        | `{1,2}.add(3)` → `{1,2,3}`       |       |                   |
| `remove(x)`                     | Remove `x` (error if not exist)        | `{1,2,3}.remove(2)` → `{1,3}`    |       |                   |
| `discard(x)`                    | Remove `x` (no error if not exist)     | `{1,2,3}.discard(4)` → `{1,2,3}` |       |                   |
| `pop()`                         | Remove and return an arbitrary element | `{1,2,3}.pop()` → `1` (example)  |       |                   |
| `clear()`                       | Remove all elements                    | `{1,2}.clear()` → `set()`        |       |                   |
| `union(s)` / \`                 | \`                                     | Return union with another set    | \`{1} | {2,3}`→`{1,2,3}\` |
| `intersection(s)` / `&`         | Return common elements                 | `{1,2}&{2,3}` → `{2}`            |       |                   |
| `difference(s)` / `-`           | Elements in self not in s              | `{1,2}-{2,3}` → `{1}`            |       |                   |
| `symmetric_difference(s)` / `^` | Elements in either set, not both       | `{1,2}^{2,3}` → `{1,3}`          |       |                   |
| `issubset(s)`                   | Check if subset                        | `{1}.issubset({1,2})` → `True`   |       |                   |
| `issuperset(s)`                 | Check if superset                      | `{1,2}.issuperset({1})` → `True` |       |                   |
| `copy()`                        | Return shallow copy                    | `{1,2}.copy()` → `{1,2}`         |       |                   |
| `len(set)`                      | Number of elements                     | `len({1,2,3})` → `3`             |       |                   |
| `sorted(set)`                   | Sorted list from set                   | `sorted({3,1,2})` → `[1,2,3]`    |       |                   |
```


## 📘 Tuple in Python
---
🔹 1. Definition

    - A Tuple is an ordered, immutable collection of elements.
  
    - Once created, you cannot add, remove, or modify its elements.

    - Tuples are faster than lists and are often used for fixed data.
  
---
```
| Method                 | Description               | Example                       |
| ---------------------- | ------------------------- | ----------------------------- |
| `count(x)`             | Count occurrences of `x`  | `(1,2,2).count(2)` → `2`      |
| `index(x, start, end)` | First index of `x`        | `(1,2,3).index(2)` → `1`      |
| `len(tuple)`           | Length of tuple           | `len((1,2,3))` → `3`          |
| `min(tuple)`           | Minimum element           | `min((1,2,3))` → `1`          |
| `max(tuple)`           | Maximum element           | `max((1,2,3))` → `3`          |
| `sum(tuple)`           | Sum of numeric elements   | `sum((1,2,3))` → `6`          |
| `tuple(iterable)`      | Convert iterable to tuple | `tuple([1,2,3])` → `(1,2,3)`  |
| `sorted(tuple)`        | Return sorted list        | `sorted((3,1,2))` → `[1,2,3]` |
```


## 📘 Dictionary in Python (dict)
---
🔹 Definition

    - A Dictionary is an unordered, mutable, and indexed collection in Python.

    - It stores data in the form of key–value pairs.

    - Keys must be unique and immutable (string, number, tuple), while values can be of any type.

Syntax:

        my_dict = {
            "name": "Raj",
            "age": 23,
            "branch": "Electrical"
        }
---
```
| Method                        | Description                    | Example                                        |
| ----------------------------- | ------------------------------ | ---------------------------------------------- |
| `dict.keys()`                 | Return all keys                | `{'a':1}.keys()` → `dict_keys(['a'])`          |
| `dict.values()`               | Return all values              | `{'a':1}.values()` → `dict_values([1])`        |
| `dict.items()`                | Return key-value pairs         | `{'a':1}.items()` → `dict_items([('a',1)])`    |
| `dict.get(key, default)`      | Return value for key           | `{'a':1}.get('b',0)` → `0`                     |
| `dict.pop(key)`               | Remove key and return value    | `{'a':1}.pop('a')` → `1`                       |
| `dict.popitem()`              | Remove last inserted item      | `{'a':1}.popitem()` → `('a',1)`                |
| `dict.update(other_dict)`     | Add key-values from other dict | `{'a':1}.update({'b':2})` → `{'a':1,'b':2}`    |
| `dict.clear()`                | Remove all items               | `{'a':1}.clear()` → `{}`                       |
| `dict.copy()`                 | Return shallow copy            | `{'a':1}.copy()` → `{'a':1}`                   |
| `dict.setdefault(key, value)` | Set key if not exist           | `d.setdefault('b',2)`                          |
| `len(dict)`                   | Number of key-value pairs      | `len({'a':1,'b':2})` → `2`                     |
| `dict.fromkeys(keys, value)`  | Create dict from keys          | `dict.fromkeys(['a','b'],0)` → `{'a':0,'b':0}` |
```


## 📝 Python String (str)
---
🔹 Definition:

    - A string is a sequence of characters enclosed in single quotes ('), double quotes ("), or triple quotes (''' or """).
  
    - Strings are immutable → once created, they cannot be changed.

    - Example:
  
                s1 = 'Hello'
                s2 = "Python"
                s3 = '''This is
                multiline'''
---
```
| Method                        | Description                      | Example                                   |
| ----------------------------- | -------------------------------- | ----------------------------------------- |
| `str.lower()`                 | Convert to lowercase             | `'HELLO'.lower()` → `'hello'`             |
| `str.upper()`                 | Convert to uppercase             | `'hi'.upper()` → `'HI'`                   |
| `str.capitalize()`            | Capitalize first letter          | `'hello'.capitalize()` → `'Hello'`        |
| `str.title()`                 | Capitalize each word             | `'hello world'.title()` → `'Hello World'` |
| `str.strip()`                 | Remove leading/trailing spaces   | `' hi '.strip()` → `'hi'`                 |
| `str.lstrip()`                | Remove leading spaces            | `' hi '.lstrip()` → `'hi '`               |
| `str.rstrip()`                | Remove trailing spaces           | `' hi '.rstrip()` → `' hi'`               |
| `str.split(sep)`              | Split into list                  | `'a,b'.split(',')` → `['a','b']`          |
| `str.rsplit(sep)`             | Split from right                 | `'a,b,c'.rsplit(',',1)` → `['a,b','c']`   |
| `str.join(iterable)`          | Join elements of iterable        | `','.join(['a','b'])` → `'a,b'`           |
| `str.replace(old, new)`       | Replace substring                | `'hi'.replace('i','o')` → `'ho'`          |
| `str.find(sub)`               | Return index of first occurrence | `'hello'.find('e')` → `1`                 |
| `str.rfind(sub)`              | Return index of last occurrence  | `'hello'.rfind('l')` → `3`                |
| `str.index(sub)`              | Same as find, error if not found | `'hi'.index('i')` → `1`                   |
| `str.startswith(prefix)`      | Check if starts with             | `'hello'.startswith('he')` → `True`       |
| `str.endswith(suffix)`        | Check if ends with               | `'hello'.endswith('lo')` → `True`         |
| `str.isalpha()`               | All letters                      | `'abc'.isalpha()` → `True`                |
| `str.isdigit()`               | All digits                       | `'123'.isdigit()` → `True`                |
| `str.isalnum()`               | Letters or digits                | `'a1'.isalnum()` → `True`                 |
| `str.isspace()`               | All spaces                       | `'  '.isspace()` → `True`                 |
| `str.count(sub)`              | Count occurrences                | `'hello'.count('l')` → `2`                |
| `str.zfill(width)`            | Pad with zeros                   | `'5'.zfill(3)` → `'005'`                  |
| `len(str)`                    | Length of string                 | `len('abc')` → `3`                        |
| `str.swapcase()`              | Swap uppercase/lowercase         | `'HeLLo'.swapcase()` → `'hEllO'`          |
| `str.center(width, fillchar)` | Center string with fillchar      | `'hi'.center(5,'*')` → `'*hi**'`          |
| `str.ljust(width)`            | Left-justify string              | `'hi'.ljust(5,'-')` → `'hi---'`           |
| `str.rjust(width)`            | Right-justify string             | `'hi'.rjust(5,'-')` → `'---hi'`           |
```