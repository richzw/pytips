---
layout: post

title: Sort array with negative number
tip-number: 49
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to sort negative number array

categories:
    - en
---

### Question

I have a list that contains a mixture of positive and negative numbers, as the following

`lst = [1, -2, 10, -12, -4, -5, 9, 2]`

What I am trying to accomplish is to sort the list with the positive numbers coming before the negative numbers, 
respectively sorted as well.

Desired output:

`[1, 2, 9, 10, -12, -5, -4, -2]`


### Solution

You could just use a regular sort, and then bisect the list at 0:

```python
>>> lst
[1, -2, 10, -12, -4, -5, 9, 2]
>>> from bisect import bisect
>>> lst.sort()
>>> i = bisect(lst, 0)
>>> lst[i:] + lst[:i]
[1, 2, 9, 10, -12, -5, -4, -2]
```

The last line here takes advantage of a slice invariant `lst == lst[:n] + lst[n:]`

Another option would be to use a tuple as a sort key, and rely on lexicographical ordering of tuples:

```python
>>> sorted(lst, key=lambda x: (x<0, x))
[1, 2, 9, 10, -12, -5, -4, -2]
```
