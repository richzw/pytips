---
layout: post

title: type comparison
tip-number: 03
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: weriod codes behavior to learn type comparison in Python

categories:
    - en
---

### Question: Weriod codes behavior under Python 2.7

```python
>>> x = [1,2,3,4,5]
>>> x[x<2]
1
>>> x[x>2]
2
>>> x[x<2]=0
>>> x
[0, 2, 3, 4, 5]
```

### Answer:

Per [manual](https://docs.python.org/2/library/stdtypes.html#comparisons):

> Objects of different types except numbers are ordered by their type names; objects of the same types that don’t support proper comparison are ordered by their address.

The rules for comparison are:

- When you order two strings or two numeric types the ordering is done in the expected way (lexicographic ordering for string, numeric ordering for integers).
- When you order a numeric and a non-numeric type, the numeric type comes first.

```python
>>> 5 < 'foo'
True
>>> 5 < (1, 2)
True
>>> 5 < {}
True
>>> 5 < [1, 2]
True
```

- When you order two incompatible types where neither is numeric, they are ordered by the alphabetical order of their typenames:

```python
>>> [1, 2] > 'foo'   # 'list' < 'str' 
False
>>> (1, 2) > 'foo'   # 'tuple' > 'str'
True

>>> class Foo(object): pass
>>> class Bar(object): pass
>>> Bar() < Foo()
True

# One exception is old-style classes that always come before new-style classes.

>>> class Foo: pass           # old-style
>>> class Bar(object): pass   # new-style
>>> Bar() < Foo()
False
```

So, we have the following order `numeric < list < string < tuple`

-------------------------------------------

Since `x < 2` is `False`, therefore `x[x<2]` is `x[0]`. Conversely, `x[x>2]` is `x[True]` or `x[1]`.


### Source:

[1](http://stackoverflow.com/questions/36603042/what-does-this-syntax-mean-in-python-xx-2-0)
[2](http://stackoverflow.com/questions/3270680/how-does-python-compare-string-and-int)
