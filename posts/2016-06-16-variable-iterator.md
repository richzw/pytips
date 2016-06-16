---
layout: post

title: variable iterator
tip-number: 34
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to do variable iterator

categories:
    - en
---

### Question

I have the while loop code written, but want to write it more pythonic

```python
i = 0.01
while i < 10:
   #do something
   print i
   if i < 0.1:
       i += 0.01
   elif i < 1.0:
       i + 0.01
   else:
       i += 1
```

This will produce the `0.01, 0.02, 0.03, 0.04, 0.05, 0.06, 0.07, 0.08, 0.09, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1, 2, 3, 4, 5, 6, 7, 8, 9` I am looking for.

### Solution 1

A special-purse generator function might be the right way to go.

```python
def my_range():
    for j in .01, .1, 1.:
        for i in range(1, 10, 1):
            yield i * j

for x in my_range():
    print x
```

### Solution 2

```python
def my_range():
    i = 0
    for (end, step) in [(0.1, 0.01), (1, 0.1), (10, 1)]:
        while i < end:
            yield i += step
```

