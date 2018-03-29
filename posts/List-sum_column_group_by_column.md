
Question

I have a list as follows.

`[['Andrew', '1', '9'], ['Peter', '1', '10'], ['Andrew', '1', '8'], ['Peter', '1', '11'], ['Sam', '4', '9'], ['Andrew', '2', '2']]`

I would like sum up the last column grouped by the other columns.The result is like this

`[['Andrew', '1', '17'], ['Peter', '1', '21'], ['Sam', '4', '9'], ['Andrew', '2', '2']]`

Solution

1.

```python
In [24]: df = pd.DataFrame(data)

In [25]: df.groupby(df.columns[:-1].tolist(), as_index=False).agg(lambda x: x.astype(int).sum()).values.tolist()
Out[25]: [['Andrew', '1', 17], ['Andrew', '2', 2], ['Peter', '1', 21], ['Sam', '4', 9]]
```

2. 

This is an O(n) solution via collections.Counter, adaptable to any number of keys.

If your desired output is a list, then this may be preferable to a solution via pandas, which is generally O(n log n).

```
from collections import Counter

lst = [['Andrew', '1', '9'], ['Peter', '1', '10'], ['Andrew', '1', '8'],
       ['Peter', '1', '11'], ['Sam', '4', '9'], ['Andrew', '2', '2']]

c = Counter()

for i in lst:
    keys, val = i[:-1], i[-1]
    c[tuple(keys)] += int(val)

res = [list(k) + [v] for k, v in sorted(c.items())]
```

Result

```
[['Andrew', '1', 17], ['Andrew', '2', 2], ['Peter', '1', 21], ['Sam', '4', 9]]
```

3.

Create to DataFrame and aggregate third column converted to integers by first and second columns, last convert back to lists:

```
df = pd.DataFrame(L)
L = df[2].astype(int).groupby([df[0], df[1]]).sum().reset_index().values.tolist()
print (L)
[['Andrew', '1', 17], ['Andrew', '2', 2], ['Peter', '1', 21], ['Sam', '4', 9]]
```

And solution with defaultdict, python 3.x only:

```
from collections import defaultdict

d = defaultdict(int)
#https://stackoverflow.com/a/10532492
for *head, tail in L:
    d[tuple(head)] += int(tail)

d = [[*i, j] for i, j in sorted(d.items())]
print (d)
[['Andrew', '1', 17], ['Andrew', '2', 2], ['Peter', '1', 21], ['Sam', '4', 9]]
```

```
from itertools import groupby
[k+[sum(int(v) for _,_, v in g)] for k, g in groupby(sorted(l), key = lambda x: [x[0],x[1]])]

Out[98]: [['Andrew', '1', 17], ['Andrew', '2', 2], ['Peter', '1', 21], ['Sam', '4', 9]]
```

