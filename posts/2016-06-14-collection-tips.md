---
layout: post

title: collection tips
tip-number: 34
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: some collection tips in Python

categories:
    - en
---

### check whether a list is empty

```python
if mylist:
    # Do something with my list
else:
    # The list is empty
```    
    
### Getting indexes of elements while iterating over a list

```python
for i, element in enumerate(mylist):
    # Do something with i and element
    pass
```    
    
### sort a list

```python
class Person(object):
    def __init__(self, age):
        self.age = age

persons = [Person(age) for age in (14, 78, 42)]
from operator import attrgetter

for element in sorted(persons, key=attrgetter('age')):
    print "Age:", element.age
```    

### Grouping elements in a dictionary

```python
from collections import defaultdict

persons_by_age = defaultdict(list)

for person in persons:
    persons_by_age[person.age].append(person)
```
