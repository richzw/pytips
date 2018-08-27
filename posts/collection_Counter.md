
Basically, I'm trying to do remove any lists that begin with the same value. For example, two of the below begin with the number 1:

```python
a = [[1,2],[1,0],[2,4],[3,5]]
```

Because the value 1 exists at the start of two of the lists -- I need to remove both so that the new list becomes:

```python
b = [[2,4],[3,5]]
```

You can use `collections.Counter` with list comprehension to get sublists whose first item appears only once:

```python
from collections import Counter
c = Counter(n for n, _ in a)
b = [[x, y] for x, y in a if c[x] == 1]
```
