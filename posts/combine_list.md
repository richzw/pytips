### Question

I would like to construct list x from two lists y and z. I want all elements from y be placed where ypos elements point. 
For example:

```
y = [11, 13, 15]
z = [12, 14]
ypos = [1, 3, 5]
```

So, x must be `[11, 12, 13, 14, 15]`

Another example:

```
y = [77]
z = [35, 58, 74]
ypos = [3]
```

So, x must be `[35, 58, 77, 74]`

### Answer1

You should use list.insert, this is what it was made for!

```
def func(y, z, ypos):
    x = z[:]
    for pos, val in zip(ypos, y):
        x.insert(pos-1, val)
    return x
```

and a test:

>>> func([11, 13, 15], [12, 14], [1,3,5])
[11, 12, 13, 14, 15]

### Answer2

If the lists are very long, repeatedly calling insert might not be very efficient. Alternatively, you could create
two iterators from the lists and construct a list by getting the next element from either of the iterators depending on
whether the current index is in ypos (or a set thereof):

```
>>> ity = iter(y)
>>> itz = iter(z)
>>> syp = set(ypos)
>>> [next(ity if i+1 in syp else itz) for i in range(len(y)+len(z))]
[11, 12, 13, 14, 15]
````


