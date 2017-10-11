### Question

A tuple takes less memory space in Python:

```
>>> a = (1,2,3)
>>> a.__sizeof__()
48
```

whereas lists takes more memory space:

```
>>> b = [1,2,3]
>>> b.__sizeof__()
64
```

What happens internally on the Python memory management?

### Answer

I assume you're using CPython (I got the same results on Python 2.7 64-bit CPython). There could be differences in other Python implementations.

Regardless of the implementation, lists are variable-sized while tuples are fixed-size.

So tuples can store the elements directly inside the struct, lists on the other hand need a layer of indirection (it stores a pointer to the elements). This layer of indirection is a pointer, on 64bit systems that's 64bit, hence 8bytes.

But there's another thing that lists do: They over-allocate. Otherwise list.append would be an  O(n) operation always - to make it amortized O(1) (much faster!!!) it over-allocates. But now it has to keep track of the allocated size and the filled size (tuples only need to store one size, because allocated and filled size are always identical). That means each list has to store another "size" which on 64bit systems is a 64bit integer, again 8 bytes.

So lists need at least 16 bytes more memory than tuples. Why did I say "at least"? Because of the over-allocation. Over-allocation means it allocates more space than needed. However, the amount of over-allocation depends on "how" you create the list and the append/deletion history:

```python
>>> l = [1,2,3]
>>> l.__sizeof__()
64
>>> l.append(4)  # triggers re-allocation (with over-allocation), because the original list is full
>>> l.__sizeof__()
96

>>> l = []
>>> l.__sizeof__()
40
>>> l.append(1)  # re-allocation with over-allocation
>>> l.__sizeof__()
72
>>> l.append(2)  # no re-alloc
>>> l.append(3)  # no re-alloc
>>> l.__sizeof__()
72
>>> l.append(4)  # still has room, so no over-allocation needed (yet)
>>> l.__sizeof__()
72
```


