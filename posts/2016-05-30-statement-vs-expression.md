---
layout: post

title: statement vs expression
tip-number: 30
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the difference between statement and expression in python

categories:
    - en
---

### Definition

**Expression** is composed of a syntactically legal combination of Atoms, Primaries and Operators. It contains identifiers, literals and
operators, where operators include arithmetic and boolean operators, the function `call operator ()` the `subscription operator []` and
similar, and can be reduced to some kind of "value", which can be any Python object. 

```python
3 + 5
map(lambda x: x*x, range(10))
[a.x for a in some_iterable]
yield 7
```

**Statement** everything that can make up a line (or several lines) of Python code. Note that expressions are statements as well.
In gross general terms: **Statements Do Something** and are often composed of expressions.

```python
# all the above expressions
print 42
if x: do_y()
return
a = 7
```

### Assignment is a statement

Assignment is a statement, not an expression, and can therefore not be used inside an arbitrary expression. This means that common C 
idioms like:

```c
while (line = readline(file)) {
    ... do something...
}
```

Or

```c
if (match = search(target)) {
    ... do something...
}
```

**cannot** be used as is in Python, it is better written using an iterator:

```python
for line in file:
    ... do something with line ...
```

And

```python
match = search(target)
if match:
    ... do something with match ...
```

Or even better

```python
for line in iter(f.readline, ""):
    ... do something with line ...
```

### Source

[1](http://stackoverflow.com/questions/4728073/what-is-the-difference-between-an-expression-and-a-statement-in-python)
[2](http://effbot.org/pyfaq/why-can-t-i-use-an-assignment-in-an-expression.htm)

