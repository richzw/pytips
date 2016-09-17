---
layout: post

title: recursive function
tip-number: 45
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: None

categories:
    - en
---

### Question

Implement the `multiadder(n)` function, which takes in a nonnegative integer `n` and adds `n` integers together. Each integer to be added must
be written as a separate parameter. For example:

```python
>>> multi_three = multiadder(3)
>>> multi_three(1)(2)(3)
6

>>> multiadder(5)(1)(2)(3)(4)(5)
15
```

The code must be written by filling in the blanks.

```python
def multiadder(n):
    assert n > 0
    if _________________________ :
        return _________________________
    else:
        return _________________________
```

### Answer

```python
def multiadder(n):
  if n <= 1:
    return lambda t: t
  else:
    return lambda a: lambda b: multiadder(n-1)(a+b)

if __name__ == '__main__':
  print(multiadder(5)(1)(2)(3)(4)(5))
```

For `n == 1`, the result must be a function returning the input.

For `n > 1`, wrap the result of `n-1`, by adding input.



