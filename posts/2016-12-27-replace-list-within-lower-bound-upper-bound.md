---
layout: post

title: Replace list value within lower bound and upper bound
tip-number: 50
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to replace list value within lower bound and upper bound

categories:
    - en
---

### Question

Is there a shorter way to do this?

```python
import numpy as np

lowerBound, upperBound = 3, 7

arr = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

arr[arr > upperBound] = upperBound
arr[arr < lowerBound] = lowerBound

# [3 3 3 3 4 5 6 7 7 7]
print(arr)
```

### Solution 1

For an alternative that doesn't rely on numpy, you could always do

```python
arr = [max(min(x, upper_bound), lower_bound) for x in arr]
```

### Solution 2

You can use `numpy.clip`:

```python
In [1]: import numpy as np

In [2]: arr = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

In [3]: lowerBound, upperBound = 3, 7

In [4]: np.clip(arr, lowerBound, upperBound, out=arr)
Out[4]: array([3, 3, 3, 3, 4, 5, 6, 7, 7, 7])

In [5]: arr
Out[5]: array([3, 3, 3, 3, 4, 5, 6, 7, 7, 7])
```

