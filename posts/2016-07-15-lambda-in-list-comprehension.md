---
layout: post

title: lambda in list comprehension
tip-number: 39
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the different behavior of lambda in list comprehension between python 2 and python 3

categories:
    - en
---

### Question

I am trying to iterate the lambda func over a list as in test.py, and I want to get the call result of the lambda, not the function 
object itself. However, the following output really confused me.

```python
------test.py---------
#!/bin/env python
#coding: utf-8

a = [lambda: i for i in range(5)]
for i in a:
    print i()

--------output---------
<function <lambda> at 0x7f489e542e60>
<function <lambda> at 0x7f489e542ed8>
<function <lambda> at 0x7f489e542f50>
<function <lambda> at 0x7f489e54a050>
<function <lambda> at 0x7f489e54a0c8>
```

I modified the variable name when print the call result to t as following, and everything goes well. I am wondering what is all about 
of that. ?

```python
--------test.py(update)--------
a = [lambda: i for i in range(5)]
for t in a:
    print t()

-----------output-------------
4
4
4
4
4
```

### Answer

In Python 2 list comprehension 'leaks' the variables to outer scope:

```python
>>> [i for i in xrange(3)]
[0, 1, 2]
>>> i
2
```

Note that the behavior is different on Python 3:

```python
>>> [i for i in range(3)]
[0, 1, 2]
>>> i
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'i' is not defined
```

When you define lambda it's bound to variable i, not its' current value as your second example shows. Now when you assign new value to i the lambda will return whatever is the current value:

```python
>>> a = [lambda: i for i in range(5)]
>>> a[0]()
4
>>> i = 'foobar'
>>> a[0]()
'foobar'
```

Since the value of i within the loop is the lambda itself you'll get it as a return value:

```python
>>> i = a[0]
>>> i()
<function <lambda> at 0x01D689F0>
>>> i()()()()
<function <lambda> at 0x01D689F0>
```

### Source

http://stackoverflow.com/questions/38369470/lambdas-from-a-list-comprehension-are-returning-a-lambda-when-called
