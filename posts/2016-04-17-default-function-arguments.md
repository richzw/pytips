---
layout: post

title: default function arguments
tip-number: 05
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: one case which use function defualt argument incorrectly

categories:
    - en
---

### Misusing expressions as defaults for function arguments

```python
>>> def foo(bar=[]):        # bar is optional and defaults to [] if not specified
...    bar.append("baz")    # but this line could be problematic, as we'll see...
...    return bar

>>> foo()
["baz"]
>>> foo()
["baz", "baz"]
>>> foo()
["baz", "baz", "baz"]
```

The more advanced Python programming answer is that the **default value for a function argument is only evaluated once, at the time that the function is defined.**

One common workaround

```python
>>> def foo(bar=None):
...    if bar is None:      # or if not bar:
...        bar = []
...    bar.append("baz")
...    return bar
```
