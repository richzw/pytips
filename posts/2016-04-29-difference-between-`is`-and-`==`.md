---
layout: post

title: what is the difference between is and ==
tip-number: 15
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the essential difference between is and == in python

categories:
    - en
---

### Summary

- `==` is for _value equality_. Use it when you would like to know if two objects have the **same value**.
- `is` is for _reference equality_. Use it when you would like to know if two references refer to the **same object**.

In general, when you are comparing something to a simple type, you are usually checking for value equality, so you should use `==`. 
For example, the intention of your example is probably to check whether `x` has a value equal to `2` (`==`), not whether `x` is literally
referring to the **same object** as `2`.

### Explaination with examples

```python
>>> a = [1, 2, 3]
>>> b = a
>>> b is a 
True
>>> b == a
True
>>> b = a[:]
>>> b is a
False
>>> b == a
True
```

In your case, the second test only works because Python caches small integer objects, which is an implementation detail. 
For larger integers, this does not work:

```python
>>> 1000 is 10**3
False
>>> 1000 == 10**3
True
```

The same holds `true` for string literals:

```python
>>> "a" is "a"
True
>>> "aa" is "a" * 2
True
>>> x = "a"
>>> "aa" is x * 2
False
>>> "aa" is intern(x*2)
True
```

```python
class Negator(object):
    def __eq__(self,other):
        return not other

thing = Negator()
print thing == None    #True
print thing is None    #False
```

`is` checks for object identity. There is only 1 object `None`, so when you do `my_var is None`, you're checking whether they actually 
are the same object (not just equivalent objects)

### Source

- [is vs ==](http://stackoverflow.com/questions/132988/is-there-a-difference-between-and-is-in-python)
- [check None with is or ==](http://stackoverflow.com/questions/14247373/python-none-comparison-should-i-use-is-or/14247383#14247383)
