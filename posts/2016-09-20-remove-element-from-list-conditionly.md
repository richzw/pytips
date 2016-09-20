---
layout: post

title: remove elements from list conditionly
tip-number: 46
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Pythonic way to remove the first N items in a list that match a condition

categories:
    - en
---

### Question

If I have a function `matchCondition(x)`, how can I remove the first N items in a Python list that match that condition?

```python
N = 3
def matchCondition(x):
    return x < 5

l = [1, 10, 2, 9, 3, 8, 4, 7]

# l = do_remove(l, N, matchCondition)

# l is now [10, 9, 8, 4, 7]  (1, 2, and 3 have been removed)
```

### Solution 1

Write a generator that takes the iterable, a condition, and an amount to drop. Iterate over the data and yield items that don't meet
the condition. If the condition is met, increment a counter and don't yield the value. Always yield items once the counter reaches the amount you want to drop.

```python
def iter_drop_n(data, condition, drop):
    dropped = 0

    for item in data:
        if dropped >= drop:
            yield item
            continue

        if condition(item):
            dropped += 1
            continue

        yield item

data = [1, 10, 2, 9, 3, 8, 4, 7]
out = list(iter_drop_n(data, lambda x: x < 5, 3))
```

### Solution 2

One way using `itertools`:

```python
from itertools import count, filterfalse

data = [1, 10, 2, 9, 3, 8, 4, 7]
output = filterfalse(lambda L, c=count(): L < 5 and next(c) < 3, data)
```

Then `list(output)`, gives you:

```
[10, 9, 8, 4, 7]
```
