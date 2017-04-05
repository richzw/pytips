### Question

```python
p = [1,2]
*x ,= p    
print(x)
```

Just gives

```
[1, 2]
```

### Answer

`*x ,= p` is basically an obfuscated version of `x = list(p)` using [extended iterable unpacking](https://www.python.org/dev/peps/pep-3132/). The comma after x is required to
make the assignment target a tuple (it could also be a list though).

`*x, = p` is different from x = p because the former creates a copy of p (i.e. a new list) while the latter creates a 
reference to the original list. To illustrate:

```python
>>> p = [1, 2]
>>> *x, = p 
>>> x == p
True
>>> x is p
False
>>> x = p
>>> x == p
True
>>> x is p
True
```
