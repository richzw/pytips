---
layout: post

title: get values of same key in two dictioinaries
tip-number: 04
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: return values of same key in two large dictionaries

categories:
    - en
---

### Question: 

Given two dictionaries:

```python
dict1 = { (1,2) : 2, (2,3): 3, (1,3): 3}
dict2 = { (1,2) : 1, (1,3): 2}
```

I want to get values of same key in those two dictionaries.

```
list1 = [2,3]
list2 = [1,2]
```

### Solution 1: `set` intersaction

```python
commons = set(dict1).intersection(set(dict2))
list1 = [dict1[k] for k in commons]
list2 = [dict2[k] for k in commons]
```

### Solution 2: 

```python
for key in dict1.viewkeys() & dict2:
    list1.append(dict1[key]))
    list2.append(dict2[key]))
```

### Solution 3: functional and robust approach 

```python
>>> from operator import itemgetter

try:
  intersect = dict1.viewkeys() & dict2.viewkeys()
  list1 = itemgetter(*intersect)(dict1)
  list2 = itemgetter(*intersect)(dict2)
except TypeError:
    vals1 = vals2 = []
```

### Benchmark test

```python
from timeit import timeit

inp1 = """
commons = set(dict1).intersection(set(dict2))
list1 = [dict1[k] for k in commons]
list2 = [dict2[k] for k in commons]
   """

inp2 = """
zip(*[(dict1[k], v) for k, v in dict2.items() if k in dict1])
   """
inp3 = """
intersect = dict1.viewkeys() & dict2.viewkeys()
list1 = itemgetter(*intersect)(dict1)
list2 = itemgetter(*intersect)(dict2)
"""
dict1 = {(1, 2): 2, (2, 3): 3, (1, 3): 3}
dict2 = {(1, 2): 1, (1, 3): 2}
print 'inp1 ->', timeit(stmt=inp1,
                        number=1000000,
                        setup="dict1 = {}; dict2 = {}".format(dict1, dict2))
print 'inp2 ->', timeit(stmt=inp2,
                        number=1000000,
                        setup="dict1 = {}; dict2 = {}".format(dict1, dict2))
print 'inp3 ->', timeit(stmt=inp3,
                        number=1000000,
                        setup="dict1 = {}; dict2 = {};from operator import itemgetter".format(dict1, dict2))
```

### Source

[1](http://stackoverflow.com/questions/36628586/python-compare-two-large-dictionaries-and-return-list-of-values-for-similar-key)
