--
layout: post

title: New dict performance in Python 3.6
tip-number: 48
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: dict() now uses a “compact” representation pioneered by PyPy

categories:
    - en
---

### Description

> How does the Python 3.6 dictionary implementation perform better than the older one while preserving element order?

Essentially by keeping `two arrays`, one holding the entries for the dictionary in the order that they were inserted and the 
other holding a list of indices.

In the previous implementation a sparse array of type dictionary entries had to be allocated; unfortunately, it also resulted 
in a lot of empty space since that array was not allowed to be more than `2/3s` full. This is not the case now since only the
required entries are stored and a sparse array of type integer `2/3s` full is kept.

Obviously creating a sparse array of type `"dictionary entries"` is much more memory demanding than a sparse array for 
storing ints (sized 8 bytes tops in cases of really large dictionaries)

[In the original proposal made by Raymond Hettinger](https://mail.python.org/pipermail/python-dev/2012-December/123028.html), a visualization of the data structures used can be seen which captures
the gist of the idea.

For example, the dictionary:

```python
d = {'timmy': 'red', 'barry': 'green', 'guido': 'blue'}
is currently stored as:

entries = [['--', '--', '--'],
           [-8522787127447073495, 'barry', 'green'],
           ['--', '--', '--'],
           ['--', '--', '--'],
           ['--', '--', '--'],
           [-9092791511155847987, 'timmy', 'red'],
           ['--', '--', '--'],
           [-6480567542315338377, 'guido', 'blue']]
Instead, the data should be organized as follows:

indices =  [None, 1, None, None, None, 0, None, 2]
entries =  [[-9092791511155847987, 'timmy', 'red'],
            [-8522787127447073495, 'barry', 'green'],
            [-6480567542315338377, 'guido', 'blue']]
```

As you can visually now see, in the original proposal, a lot of space is essentially empty to reduce collisions and make 
look-ups faster. With the new approach, you reduce the memory required by moving the sparseness where it's really required, 
in the indices.
