---
layout: post

title: list deduction
tip-number: 25
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: one example to introduce the list deduction in python

categories:
    - en
---

### Question: filter list through rules

```python
num = [1,3,-5, 10, -9, 2, 5, -1]
filtered_and_squared = []
for number in num:
    if number > 0:
        filtered_and_squared.append(number**2)
print filtered_and_squared
```

### Better solution

```python
filtered_and_squared = map(lambda x: x**2, filter(lambda x: x > 0, num))
```

### Even Better solution

```pyton
'''[issue: load the whole list into memory ]'''

filtered_and_squared = [ x**2 for x in num if x > 0]
```

### Best solution

Generator

```python
filtered_and_squared = (x**2 for x in num if x > 0)

for item in filtered_and_squared:
    print item
```

### Improvement

```python
def square_generator(optional_parameter):
    return (x**2 for x in num if x > optional_parameter)
```


