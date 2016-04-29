---
layout: post

title: pairewise circular in list values
tip-number: 16
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to pairwise circular in list values

categories:
    - en
---

### Question

If I have the list `[1, 2, 3]`, I would like to get the following pairs:

- 1 - 2
- 2 - 3
- 3 - 1

### Solution 1: `tee` in `itertools`

```python
def pairwise_circle(iterable):
    "s -> (s0,s1), (s1,s2), (s2, s3), ... (s<last>,s0)"
    a, b = itertools.tee(iterable)
    first_value = next(b, None)
    return itertools.zip_longest(a, b,fillvalue=first_value)
```

### Solution 2: `deque`

```python
>>> from collections import deque
>>>
>>> l = [1,2,3]
>>> d = deque(l)
>>> d.rotate(-1)
>>> zip(l, d)
[(1, 2), (2, 3), (3, 1)]
```

### Solution 3: `zip`

```python
>>> L = [1, 2, 3]
>>> zip(L, L[1:] + L[:1])
[(1, 2), (2, 3), (3, 1)]
```



