---
layout: post

title: sum column in multiple array
tip-number: 09
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to sum column in multiple array

categories:
    - en
---

### Question:

Consider I have a list of lists as:

```
[[5, 10, 30, 24, 100], [1, 9, 25, 49, 81]]
[[15, 10, 10, 16, 70], [10, 1, 25, 11, 19]]
[[34, 20, 10, 10, 30], [9, 20, 25, 30, 80]]
```

Now I want the sum of all indexes of first list's index wise and then the 2nd list `5+15+34=54` `10+10+20=40` and so on as:

`[54,40,50, 50,200], [20,30,75,90,180]`

### Solution 1:

```python
>>> data = [[[5, 10, 30, 24, 100], [1, 9, 25, 49, 81]],
...         [[15, 10, 10, 16, 70], [10, 1, 25, 11, 19]],
...         [[34, 20, 10, 10, 30], [9, 20, 25, 30, 80]]]
>>> for res in zip(*data):
...     print [sum(j) for j in zip(*res)] 
... 
[54, 40, 50, 50, 200]
[20, 30, 75, 90, 180]
```

### Solution 2:

```python
>>> [[sum(item) for item in zip(*items)] for items in zip(*data)]
[[54, 40, 50, 50, 200], [20, 30, 75, 90, 180]]
```
