---
layout: post

title: interleave one list
tip-number: 06
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to interleave one list

categories:
    - en
---

### Question: interleave two strings

Given a list `a = [0,1,2,3,4,5,6,7,8,9]`, how can I get `b = [0,9,1,8,2,7,3,6,4,5]` after interleaving?

### Answer:

```python
>>> [a[-i//2] if i % 2 else a[i//2] for i in range(len(a))]
[0, 9, 1, 8, 2, 7, 3, 6, 4, 5]
```
