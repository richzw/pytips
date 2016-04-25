---
layout: post

title: demystify python descriptor
tip-number: 13
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: what is the feature of descriptor in python?

categories:
    - en
---

### The punchline: descriptors are reusable properties

`descriptor` is the property that you can reuse. That is, descriptors let you write code that looks like this

```python
f = Foo()
b = f.bar
f.bar = c
del f.bar
```

and, behind the scenes, call custom methods when trying to access (`b = f.bar`), assign to (`f.bar = c`), or delete an instance variable (`del f.bar`)

### Properties disguise function calls as attributes

```python
class Movie(object):
    def __init__(self, title, rating, runtime, budget, gross):
        self._budget = None
        
    @property
    def budget(self):
        return self._budget
    
    @budget.setter
    def budget(self, value):
        if value < 0:
            raise ValueError("Negative value not allowed: %s" % value)
        self._budget = value
        
m = Movie('Casablanca', 97, 102, 964000, 1300000)
print m.budget       # calls m.budget(), returns result
try:
    m.budget = -100  # calls budget.setter(-100), and raises ValueError
except ValueError:
    print "Woops. Not allowed"        
```

We specify a getter method with a `@property` decorator, and a setter method with a `@budget.setter` decorator.

### Descriptor class for reusable property logic

```python
from weakref import WeakKeyDictionary

class NonNegative(object):
    """A descriptor that forbids negative values"""
    def __init__(self, default):
        self.default = default
        self.data = WeakKeyDictionary()
        
    def __get__(self, instance, owner):
        # we get here when someone calls x.d, and d is a NonNegative instance
        # instance = x
        # owner = type(x)
        return self.data.get(instance, self.default)
    
    def __set__(self, instance, value):
        # we get here when someone calls x.d = val, and d is a NonNegative instance
        # instance = x
        # value = val
        if value < 0:
            raise ValueError("Negative value not allowed: %s" % value)
        self.data[instance] = value

        
class Movie(object):
    
    #always put descriptors at the class-level
    rating = NonNegative(0)
    runtime = NonNegative(0)
    budget = NonNegative(0)
    gross = NonNegative(0)
    
    def __init__(self, title, rating, runtime, budget, gross):
        self.title = title
        self.rating = rating
        self.runtime = runtime
        self.budget = budget
        self.gross = gross
    
    def profit(self):
        return self.gross - self.budget
    
    
m = Movie('Casablanca', 97, 102, 964000, 1300000)
print m.budget  # calls Movie.budget.__get__(m, Movie)
m.rating = 100  # calls Movie.budget.__set__(m, 100)
try:
    m.rating = -1   # calls Movie.budget.__set__(m, -100)
except ValueError:
    print "Woops, negative value"
```

### Source

[Python Descriptors Demystified](http://nbviewer.jupyter.org/urls/gist.github.com/ChrisBeaumont/5758381/raw/descriptor_writeup.ipynb)

