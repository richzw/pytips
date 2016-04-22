---
layout: post

title: mess two strings
tip-number: 12
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to mesh two strings

categories:
    - en
---

### Question

```
Input:
    u = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    l = 'abcdefghijklmnopqrstuvwxyz'
Output:
    'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz'
```

### Solution 1

```python
>>> u = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
>>> l = 'abcdefghijklmnopqrstuvwxyz'
>>> "".join("".join(item) for item in zip(u, l))
'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz'
```

### Solution 2

```python
>>> res = ['']*len(u)*2
>>> res[::2] = u
>>> res[1::2] = l
>>> ''.join(res)
'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz'
```

### Solution 3

```python
>>> "".join(i + j for i, j in zip(u, l))
'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz'
```

### Solution 4

```python
>>> import itertools
>>> "".join(list(itertools.chain.from_iterable(zip(u, l))))
'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz'
```
