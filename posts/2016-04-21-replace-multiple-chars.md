---
layout: post

title: replace multiple chars
tip-number: 05
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to replace multiple chars

categories:
    - en
---

### Question:

Given `'ab@cde@@fghi@jk@lmno@@@p@qrs@tuvwxy@z'` and want to get new string `'ab1cde23fghi1jk2lmno312p3qrs1tuvwxy2z'`,
for `replace_chars = ['1', '2', '3']`.

### Answer:

```python
>>> from itertools import cycle
>>> s = 'ab@cde@@fghi@jk@lmno@@@p@qrs@tuvwxy@z'
>>> replace_chars = ['1', '2', '3']
>>>
>>> replacer = cycle(replace_chars)
>>> ''.join([next(replacer) if c == '@' else c for c in s])
'ab1cde23fghi1jk2lmno312p3qrs1tuvwxy2z'
```
