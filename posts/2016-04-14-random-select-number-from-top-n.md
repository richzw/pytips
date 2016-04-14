---
layout: post

title: random select number from top N
tip-number: 02
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: random select number from top N in dictionary

categories:
    - en
---

### Question:

Given the dictionary `dic = {'a': 100, 'b': 15, 'c': 180, 'd': 50}`, we can get the key which has greatest value through

```python
greatest_key = max(dic.iteritems(), key=operator.itemgetter(1))[0]
```

How can I select one number from top 3 (3 greatest values) randomly?

### Solution 1: Counter collection

```python
>>> from collections import Counter
>>> from random import choice
>>> choice(Counter(dic).most_common(3))[0]
'd'
```

### Solution 2: heapq

```python
>>> import heapq
>>> top_n = heapq.nlargest(3, dic, key=dic.__getitem__)
>>> random.choice(top_n)
'd'
```

### Solution 3: sort 

```python
>>> random.choice(sorted(dic, reverse=True, key=dic.get)[:3])
'c'
```
