---
layout: post

title: variable id
tip-number: 38
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the variable id in python

categories:
    - en
---

### Question

Two variables in Python have the same id:

```python
a = 10
b = 10
a is b
>>> True
```

If I take two lists:

```python
a = [1, 2, 3]
b = [1, 2, 3]
a is b
>>> False
```

The immutable object references have the same id and mutable objects like lists have different ids.

However, tuples should have the same ids - meaning:

```python
a = (1, 2, 3)
b = (1, 2, 3)
a is b
>>> False
```

Ideally, as tuples are not mutable, it should return True, but it is returning `False`!

### Answer:

Immutable objects **don't** have the same id, and as a mater of fact this is not true for any type of objects that you define separately.
Every time you define an object in Python, you'll create a new object with a new identity.

But there are some exceptions for small integers (between `-5` and `256`) and small strings (interned strings, with a special length 
(usually less than `20` character)) which are singletons and have same id (actually one object with multiple pointer). You can check 
this fact with larger numbers:

```python
>>> 300 is 3*100
False
```

Also note that the is operator will check the object's identity, not the value. If you want to check the value you should use `==`:

```python
>>> 300 == 3*100
True
```

And since there is no such rule for tuples (other types) if you define the two same tuples in any size they'll get their own ids:

```python
>>> a = (1,)
>>> b = (1,)
>>>
>>> a is b
False
```

And note that the fact of singleton integers and interned strings is true even when you define them within mutable and immutable objects:

```python
>>> a = (100, 700, 400)
>>>
>>> b = (100, 700, 400)
>>>
>>> a[0] is b[0]
True
>>> a[1] is b[1]
False
```

### Source

[1](http://stackoverflow.com/questions/38189660/two-variables-in-python-have-same-id-but-not-lists-or-tuples-why)
