---
layout: post

title: split a string
tip-number: 37
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to split a string reliably

categories:
    - en
---

### Question

In Python, however this won't work:

```python
a, b = "foo".split(":")  # ValueError: not enough values to unpack
```

What's the canonical way to prevent errors in such cases?

### Solution 1

If you're splitting into just two parts (like in your example) you can use `str.partition()` to get a guaranteed argument unpacking size 
of `3`:

```python
>>> a, sep, b = "foo".partition(":")
>>> a, sep, b
('foo', '', '')
```

`str.partition()` always returns a `3-tuple`, whether the separator is found or not.

### Solution 2

Another alternative for Python 3 is to use `extended unpacking`, 

```python
>>> a, *b = "foo".split(":")
>>> a, b
('foo', [])
```

This assigns the first split item to `a` and the list of remaining items (if any) to `b`.

### Source

http://stackoverflow.com/questions/38149470/how-do-i-reliably-split-a-string-in-python
