---
layout: post

title: split list into uneven group
tip-number: 42
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Split list into uneven group

categories:
    - en
---

### Question

 I want to do is divide mylist into uneven groups by the spacing in second_list?
 
 
### Solution

You can create an iterator and `itertools.islice`:

```python
mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
seclist = [2,4,6]

from itertools import islice
it = iter(mylist)

sliced =[list(islice(it, 0, i)) for i in seclist]
```

Which would give you:

```python
[[1, 2], [3, 4, 5, 6], [7, 8, 9, 10, 11, 12]]
```

Once i elements are consumed they are gone so we keep getting the next i elements.

Not sure what should happen with any remaining elements, if you want them added, you could add something like:

```python
mylist = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13 ,14]
seclist = [2, 4, 6]

from itertools import islice

it = iter(mylist)

slices = [sli for sli in (list(islice(it, 0, i)) for i in seclist)]
remaining = list(it)
if remaining:
    slices.append(remaining)
print(slices)
```

Which would give you:

```python
 [[1, 2], [3, 4, 5, 6], [7, 8, 9, 10, 11, 12], [13, 14]]
```
 
Or in contrast if there were not enough, you could use a couple of approaches to remove empty lists, one an inner generator expression:

```python
from itertools import islice

it = iter(mylist)
slices = [sli for sli in (list(islice(it, 0, i)) for i in seclist) if sli]
```

Or combine with `itertools.takewhile`:

```python
from itertools import islice, takewhile

it = iter(mylist)
slices = list(takewhile(bool, (list(islice(it, 0, i)) for i in seclist)))
```

Which for:

```python
mylist = [1, 2, 3, 4, 5, 6]
seclist = [2, 4, 6,8]
```

would give you:

```python
[[1, 2], [3, 4, 5, 6]]
```

As opposed to:

```python
[[1, 2], [3, 4, 5, 6], [], []]
```
