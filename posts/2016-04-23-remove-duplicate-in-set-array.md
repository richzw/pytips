---
layout: post

title: remove duplicate in set array
tip-number: 11
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to remove duplicate in set array

categories:
    - en
---

### Question:

Given `allsets = [set([1, 2, 4]), set([4, 5, 6]), set([4, 5, 7])]`

What is a pythonic way to compute the corresponding list of sets of elements having **no overlap** with other sets?

`nondupset = [set([1, 2]), set([6]), set([7])]`

### Solution 1

```python
>>> allsets = [set([1, 2, 4]), set([4, 5, 6]), set([4, 5, 7])]
>>> import itertools
>>> import collections
>>> elem_cnt = collections.Counter(itertools.chain.from_iterable(allsets))
>>> non_dup_set = [{elem for elem in s if elem_cnt[elem] == 1} for s in allsets]
>>> non_dup_set
[set([1, 2]), set([6]), set([7])]
```

### Solution 2

```python
>>> allsets = [set([1, 2, 4]), set([4, 5, 6]), set([4, 5, 7])]
>>> import itertools
>>> import collections
>>> elem_cnt = collections.Counter(itertools.chain.from_iterable(allsets))
>>> non_dup_set = [s & uniq_set for s in allsets]
>>> non_dup_set
[set([1, 2]), set([6]), set([7])]
```
