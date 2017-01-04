---
layout: post

title: difference between `-=` and `=-`
tip-number: 00
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Werid result of between operator `-=` and `=-`

categories:
    - en
---

### Question: why the result of -= is different with =-?

```python
import numpy as np

a = np.array([1,2,3])
b = np.copy(a)

a[1:] -= a[:-1]
b[1:] = b[1:] - b[:-1]

print a     # [1 1 2]
print b     # [1 1 1] 
```

### Answer:

Mutating arrays while they're being used in computations can lead to unexpected results!

In the example in the question, the subtraction with `-=` modifies the second of element of `a` and then immediately uses that modified
second element in the operation on the third element of `a`.

Here is what happens with `a[1:] -= a[:-1]` step by step:

- `a` is the array with the data `[1, 2, 3]`.

- We have two views onto this data: `a[1:]` is `[2, 3]`, and `a[:-1]` is `[1, 2]`.

- The inplace subtraction `-=` begins. The first element of `a[:-1]`, `1`, is subtracted from the first element of `a[1:]`. This has modified a to be `[1, 1, 2]`. Now we have that `a[1:]` is a view of the data `[1, 3]`, and `a[:-1`] is a view of the data `[1, 1]` (the second element of array a has been changed).

- `a[:-1]` is now `[1, 1]` and NumPy must now subtract its second element which is `1` (not `2` anymore!) from the second element of `a[1:]`. This makes `a[1:]` a view of the values `[1, 2]`.

- `a` is now an array with the values `[1, 1, 2]`.

However, `b[1:] = b[1:] - b[:-1]` does not have this problem because `b[1:] - b[:-1]` creates a new array first and then assigns the values
in this array to `b[1:]`. It does not modify `b` itself during the subtraction, so the views `b[1:]` and `b[:-1]` do not change.

### similar issue

```python
import numpy as np

A = np.arange(12).reshape(4,3)
for a in A:
    a = a + 1

B = np.arange(12).reshape(4,3)
for b in B:
    b += 1
```

After running each for loop, `A` has not changed, but `B` has had one added to each element. I actually use the `B` version to write to a initialized NumPy array within `a` for loop.

### Answer

The difference is that one modifies the data-structure itself (in-place operation) b += 1 while the other just reassigns the variable a = a + 1.

Just for completeness:

x += y is not always doing an in-place operation, there are (at least) three exceptions:

If x doesn't implement an __iadd__ method then the x += y statement is just a shorthand for x = x + y. This would be the case if x was something like an int.
If __iadd__ returns NotImplemented, Python falls back to x = x + y.
The __iadd__ method could theoretically be implemented to not work in place. It'd be really weird to do that, though.
As it happens your bs are numpy.ndarrays which implements __iadd__ and return itself so your second loop modifies the original array in-place.


