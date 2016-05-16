---
layout: post

title: empty string
tip-number: 22
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: what the empty string behavior in python

categories:
    - en
---

### Question

```python
>>> '' in 'spam'
True
```

Why the `empty string` return `True`?

### 

`''` is the empty string, same as `""`. The empty string is a substring of every other string.

When `a` and `b` are strings, the expression `a in b` checks that `a` is a substring of `b`. That is, the sequence of characters of `a`
must exist in `b`; there must be an index `i` such that `b[i:i+len(a)] == a`. If `a` is empty, then any index `i` satisfies this condition.

This does not mean that when you iterate over `b`, you will get `a`. Unlike other sequences, while every element produced by `for a in b`
satisfies `a in b`, `a in b` does not imply that `a` will be produced by iterating over `b`.

So `''` in x and `""` in `x` returns `True` for any string `x`:

```python
>>> '' in 'spam'
True
>>> "" in 'spam'
True
>>> "" in ''
True
>>> '' in ""
True
>>> '' in ''
True
>>> '' in ' ' 
True
>>> "" in " "
True

>>> s = "foobar"
>>> s[0:0]
''
>>> s[1:1]
''
>>> s[2:2]
''
```
