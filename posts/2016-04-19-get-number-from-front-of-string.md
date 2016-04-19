---
layout: post

title: get number from front of string
tip-number: 05
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to retrieve number from front of string

categories:
    - en
---

### Question: 

Given the string `'123abc456def'`, what is the cleanest way to obtain the string `'123'`?

### Solution 1: regex

```python
>>> import re
>>> input = '123abc456def'
>>> re.findall('\d+', s)
['123','456']
```

### Solution 2: `takewile` operator

```python
>>> from itertools import takewhile
>>> input = '123abc456def'
>>> ''.join(takewhile(str.isdigit, input))
'123'
```
