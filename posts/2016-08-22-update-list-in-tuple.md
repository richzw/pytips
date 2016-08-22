---
layout: post

title: update list in tuple
tip-number: 43
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Error behavior when updating list in tuple

categories:
    - en
---

### Question

when I run this:

```python
tup = (1,2,3,[4,5])
tup[3] += [6]
```

I get: `TypeError: 'tuple' object does not support item assignment`
Which is exactly what I expected. However then when I reference the tuple again, I get:

```python
>>> tup
(1, 2, 3, [4, 5, 6])
```

### Answer

A tuple cannot be changed. A tuple, like a list, is a structure that points to other objects. It doesn't care about what those objects are. They could be strings, numbers, tuples, lists, or other objects.

So doing anything to one of the objects contained in the tuple, including appending to that object if it's a list, isn't relevant to the semantics of the tuple.

Here's a summary so that this is a more complete answer.

- When we use `+=`, Python calls the `__iadd__` magic method on the item, then uses the return value in the subsequent item assignment.
- For lists, `__iadd__` is equivalent to calling extend on the list and then returning the list.
- Therefore, when we call `tup[3] += [6]`, it is equivalent to:

```python
result = tup[3].__iadd__([6])
tup[3] = result
```

- From #2, we can determine this is equivalent to:

```python
result = tup[3].extend([6])
tup[3] = result
```

- The first line succeeds in calling extend on the list, and since the list is mutable, it updates. However, the subsequent assignment fails because tuples are immutable, and throws the error.

