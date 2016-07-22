---
layout: post

title: hashable function
tip-number: 40
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: why the function of python is hashable

categories:
    - en
---

### Question

I recently tried the following commands in Python:

```python
>>> {lambda x: 1: 'a'}
{<function __main__.<lambda>>: 'a'}

>>> def p(x): return 1
>>> {p: 'a'}
{<function __main__.p>: 'a'}
```

The success of both dict creations indicates that both lambda and regular functions are hashable. (Something like `{[]: 'a'}` fails with
`TypeError: unhashable type: 'list'`).

The hash is apparently not necessarily the ID of the function:

```python
>>> m = lambda x: 1
>>> id(m)
140643045241584
>>> hash(m)
8790190327599
>>> m.__hash__()
8790190327599
```

The last command shows that the `__hash__` method is explicitly defined for lambdas, i.e., this is not some automagical thing Python 
computes based on the type. What is the motivation behind making functions hashable? For a bonus, what is the hash of a function?

### Answer

It's nothing special. As you can see if you examine the unbound `__hash__` method of the function type:

```python
>>> def f(): pass
...
>>> type(f).__hash__
<slot wrapper '__hash__' of 'object' objects>
```

it just inherits `__hash__` from object. Function `==` and hash work by identity. The difference between id and hash is normal for any
type that inherits `object.__hash__`:

```python
>>> x = object()
>>> id(x)
40145072L
>>> hash(x)
2509067
```

You might think `__hash__` is only supposed to be defined for immutable objects, but that's not true. `__hash__` should only be defined
for objects where everything involved in `==` comparisons is immutable. For objects whose `==` is based on identity, it's completely 
standard to base hash on identity as well, since even if the objects are mutable, they can't possibly be mutable in a way that would 
change their identity. Files, modules, and other mutable objects with identity-based `==` all behave this way.

### Source

http://stackoverflow.com/questions/38518849/why-and-how-are-python-functions-hashable
