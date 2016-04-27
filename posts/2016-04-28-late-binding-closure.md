---
layout: post

title: late binding closure
tip-number: 15
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: one example to illustrate the late binding in python

categories:
    - en
---

### Late Binding Closure

**What you wrote**

```python
def create_multipliers():
  return [lambda x: i*x for i in range(5)]
  
for multiplier in create_multipliers():
  print multiplier(2)
```

**What you might have expected to happen**

Output: `0 2 4 6 8`

**What does happen**

`8 8 8 8 8`

Five functions are created; instead all of them just multiply x by 4.

Python’s closures are **late binding**. This means that the values of variables used in closures are looked up at the time the inner function is called.

Here, whenever any of the returned functions are called, the value of `i` is looked up in the surrounding scope at call time. By then,
the loop has completed and `i` is left with its final value of `4`.

**What you should do instead**

```python
def create_multipliers():
    return [lambda x, i=i : i * x for i in range(5)]
```

Or

```python
def create_multipliers():
  flist = []
  
  for i in xrange(3):
      def funcC(j):
          def func(x): return x * j
          return func
      flist.append(funcC(i))
      
  return flist
```

Or

```python
from functools import partial
from operator import mul

def create_multipliers():
    return [partial(mul, i) for i in range(5)]
```

