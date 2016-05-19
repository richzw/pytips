---
layout: post

title: merge two lists with given order
tip-number: 24
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to merge two lists with one given order

categories:
    - en
---

### Question

Given I have first list equal to `[a, b, c]` and second list equal to `[d, e]` and 'merging' list equal to `[0, 1, 0, 0, 1]`. That means: 
to make merged list first I need to take element from first list, then second, then first, then first, then second... And I end up with
`[a, d, b, c, e]`.

### Solution

You could create `2 iterators` from those lists, loop through the ordering list, and call `next` on one of the 2 iterators:

```python
i1 = iter(['a', 'b', 'c'])
i2 = iter(['d', 'e'])
# Select the iterator to advance: `i2` if `x` == 1, `i1` otherwise
print([next(i2 if x else i1) for x in [0, 1, 0, 0, 1]]) # ['a', 'd', 'b', 'c', 'e']
```

General solution

```python
def ordered_merge(lists, selector):
    its = [iter(l) for l in lists]
    for i in selector:
        yield next(its[i])
```

