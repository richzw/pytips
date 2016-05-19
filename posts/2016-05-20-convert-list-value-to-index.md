---
layout: post

title: convert list value to index
tip-number: 26
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to convert list value to index

categories:
    - en
---

### Question

I have a list say `l = [10,10,20,15,10,20]`. I want to assign each unique value a certain "index" to get `[1,1,2,3,1,2]`.

### Answer:

```python
>>> from itertools import count
>>> from collections import defaultdict
>>> lst = [10, 10, 20, 15, 10, 20]
>>> d = defaultdict(count(1).next)
>>> [d[k] for k in lst]
[1, 1, 2, 3, 1, 2]
```

The default_factory(i.e `count(1).next` in this case) passed to defaultdict is called only when Python encounters a missing key, 
so for `10` the value is going to be `1`, then for the next ten it is not a missing key anymore hence the previously calculated `1` is used, 
now `20` is again a missing key and Python will call the `default_factory` again to get its value and so on.

