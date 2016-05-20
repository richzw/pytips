---
layout: post

title: tail comma
tip-number: 27
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: what the meaning of (1,) == 1,

categories:
    - en
---

### Question

```python
>>>  (1,) == 1,
Out: (False,)
```

When I assign these two expressions to a variable, the result is true:

```python
>>> a = (1,)
>>> b = 1,
>>> a==b
Out: True
```

### Answer

This is just operator precedence. Your first

```
(1,) == 1,
```

groups like so:

```
((1,) == 1),
```

so builds a tuple with a single element from the result of comparing the one-element tuple `1`, to the integer `1` for equality They're
not equal, so you get the `1-tuple False`, for a result.

### Analysis 

```python
>>> import ast
>>> source_code = '(1,) == 1,'
>>> print(ast.dump(ast.parse(source_code), annotate_fields=False))
Module([Expr(Tuple([Compare(Tuple([Num(1)], Load()), [Eq()], [Num(1)])], Load()))])
```

### Source

[S](http://stackoverflow.com/questions/37313471/whats-the-meaning-of-1-1-in-python)
