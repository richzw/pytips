---
layout: post

title: swap adjcent element in list
tip-number: 44
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Efficiently swap two adjcent elements in list

categories:
    - en
---

### Question

I have a bunch of lists that look like this one:

`l = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

I want to swap elements as follows:

`final_l = [2, 1, 4, 3, 6, 5, 8, 7, 10, 9]`

The size of the lists may vary, but they will always contain an even number of elements.

### Solution 1

```python
In [1]: l = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

In [2]: l[::2], l[1::2] = l[1::2], l[::2]

In [3]: l
Out[3]: [2, 1, 4, 3, 6, 5, 8, 7, 10, 9]
```

```
a[start:end] # items start through end-1
a[start:]    # items start through the rest of the array
a[:end]      # items from the beginning through end-1
a[:]         # a copy of the whole array
```

There is also the step value, which can be used with any of the above:

`a[start:end:step]` # start through not past end, by step
Let's look at OP's requirements:

```
 [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  # list l
  ^  ^  ^  ^  ^  ^  ^  ^  ^  ^
  0  1  2  3  4  5  6  7  8  9    # respective index of the elements
l[0]  l[2]  l[4]  l[6]  l[8]      # first tier : start=0, step=2
   l[1]  l[3]  l[5]  l[7]  l[9]   # second tier: start=1, step=2
-----------------------------------------------------------------------
l[1]  l[3]  l[5]  l[7]  l[9]
   l[0]  l[2]  l[4]  l[6]  l[8]   # desired output
```

First tier will be: `l[::2] = [1, 3, 5, 7, 9]` Second tier will be: `l[1::2] = [2, 4, 6, 8, 10]`

As we want to re-assign `first = second & second = first`, we can use multiple assignment, and update the original list in place:

`first , second  = second , first`

that is:

`l[::2], l[1::2] = l[1::2], l[::2]`

### Solution 2

Here a single list comprehension that does the trick:

```python
In [1]: l = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

In [2]: [l[i^1] for i in range(len(l))]
Out[2]: [2, 1, 4, 3, 6, 5, 8, 7, 10, 9]
```

The key to understanding it is the following demonstration of how it permutes the list indices:

```python
In [3]: [i^1 for i in range(10)]
Out[3]: [1, 0, 3, 2, 5, 4, 7, 6, 9, 8]
```

The `^` is the exclusive or operator. All that i^1 does is flip the least-significant bit of i, effectively swapping 0 with 1, 2 with 3 
and so on.

### solution 3

You can use the pairwise iteration and chaining to flatten the list:

```python
>>> from itertools import chain
>>>
>>> list(chain(*zip(l[1::2], l[0::2])))
[2, 1, 4, 3, 6, 5, 8, 7, 10, 9]
```

Or, you can use the `itertools.chain.from_iterable()` to avoid the extra unpacking:

```python
>>> list(chain.from_iterable(zip(l[1::2], l[0::2])))
[2, 1, 4, 3, 6, 5, 8, 7, 10, 9]
```
