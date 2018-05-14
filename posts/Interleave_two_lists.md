###
Question

Suppose I have a list:

```
l=['a','b','c']
```

I'd like the desired output to be:

```
out_l = ['a','a_1','b','b_2','c','c_3']
```

###
Solution

```python
You can use a generator for an elegant solution. At each iteration, yield twice—once with the original element, and once with the element with the added suffix.

The generator will need to be exhausted; that can be done by tacking on a list call at the end.

def transform(l):
    for i, x in enumerate(l, 1):
        yield x
        yield f'{x}_{i}'  # {}_{}'.format(x, i)
You can also re-write this using the yield from syntax for generator delegation:

def transform(l):
    for i, x in enumerate(l, 1):
        yield from (x, f'{x}_{i}') # (x, {}_{}'.format(x, i))
out_l = list(transform(l))
print(out_l)
['a', 'a_1', 'b', 'b_2', 'c', 'c_3']
```

**Sliced list.__setitem__**

I'd recommend this from the perspective of performance. First allocate space for an empty list, and then assign list items to their appropriate positions using sliced list assignment. l goes into even indexes, and l' (l modified) goes into odd indexes.

```python
out_l = [None] * (len(l) * 2)
out_l[::2] = l
out_l[1::2] = [f'{x}_{i}' for i, x in enumerate(l, 1)]  # [{}_{}'.format(x, i) ...]
print(out_l)
['a', 'a_1', 'b', 'b_2', 'c', 'c_3']
```


yield
You can use a generator for an elegant solution. At each iteration, yield twice—once with the original element, and once with the element with the added suffix.

The generator will need to be exhausted; that can be done by tacking on a list call at the end.

```
def transform(l):
    for i, x in enumerate(l, 1):
        yield x
        yield f'{x}_{i}'  # {}_{}'.format(x, i)
You can also re-write this using the yield from syntax for generator delegation:

def transform(l):
    for i, x in enumerate(l, 1):
        yield from (x, f'{x}_{i}') # (x, {}_{}'.format(x, i))
out_l = list(transform(l))
print(out_l)
['a', 'a_1', 'b', 'b_2', 'c', 'c_3']
```

If you're on versions older than python-3.6, replace f'{x}_{i}' with '{}_{}'.format(x, i).

Sliced list.__setitem__
I'd recommend this from the perspective of performance. First allocate space for an empty list, and then assign list items to their appropriate positions using sliced list assignment. l goes into even indexes, and l' (l modified) goes into odd indexes.

```
out_l = [None] * (len(l) * 2)
out_l[::2] = l
out_l[1::2] = [f'{x}_{i}' for i, x in enumerate(l, 1)]  # [{}_{}'.format(x, i) ...]
print(out_l)
['a', 'a_1', 'b', 'b_2', 'c', 'c_3']
```

This is consistently the fastest from my timings (below).

**zip + chain.from_iterable**

A functional approach, similar to @chrisz' solution. Construct pairs using zip and then flatten it using itertools.chain.

```
from itertools import chain
# [{}_{}'.format(x, i) ...]
out_l = list(chain.from_iterable(zip(l, [f'{x}_{i}' for i, x in enumerate(l, 1)]))) 
print(out_l)
['a', 'a_1', 'b', 'b_2', 'c', 'c_3']
```
**Performance**

```python
def ajax1234(l):
    return [
        i for b in [[a, '{}_{}'.format(a, i)] 
        for i, a in enumerate(l, start=1)] 
        for i in b
    ]

def cs0(l):
    # this is in Ajax1234's answer, but it is my suggestion
    return [j for i, a in enumerate(l, 1) for j in [a, '{}_{}'.format(a, i)]]

def cs1(l):
    def _cs1(l):
        for i, x in enumerate(l, 1):
            yield x
            yield f'{x}_{i}'

    return list(_cs1(l))

def cs2(l):
    out_l = [None] * (len(l) * 2)
    out_l[::2] = l
    out_l[1::2] = [f'{x}_{i}' for i, x in enumerate(l, 1)]

    return out_l

def cs3(l):
    return list(chain.from_iterable(
        zip(l, [f'{x}_{i}' for i, x in enumerate(l, 1)]))
    )

def chrisz(l):
    return [
        val 
        for pair in zip(l, [f'{k}_{j+1}' for j, k in enumerate(l)]) 
        for val in pair
    ]

def sruthiV(l):
    return [ 
        l[int(i / 2)] + "_" + str(int(i / 2) + 1) if i % 2 != 0 else l[int(i/2)] 
        for i in range(0,2*len(l))
    ]
```

[Resource](https://stackoverflow.com/a/50312321/3011380)



