
Q:

Given

```
id  v0  v1  v2  v3  v4
1   10  5   10  22  50
2   22  23  55  60  50
3   8   2   40  80  110
4   15  15  25  100 101
```

And would like to create an additional column that is either 1 or 0. 1 if v0 value is in the values of v1 to v4, and 0 if it's not. So, in this example for id 1 then the value should be 1 (since v2 = 10) and for id 2 value should be 0 since 22 is not in v1 thru v4.

A:

```python
a = df.v0.values
b = df.iloc[:, 2:].values
df.assign(out=(a[:, None]==b).any(1).astype(int))
```

performance compare

```python
def user_chris(df):
    a = df.v0.values
    b = df.iloc[:, 2:].values
    return (a[:, None]==b).any(1).astype(int)

def rahlf23(df):
    df = df.set_index('id')
    return df.drop('v0', 1).isin(df['v0']).any(1).astype(int)

def chris_a(df):
    return df.loc[:, "v1":].eq(df['v0'], 0).any(1).astype(int)

def chris(df):
    return df.apply(lambda x: int(x['v0'] in x.values[2:]), axis=1)

def anton_vbr(df):
    df.set_index('id', inplace=True)
    return df.isin(df.pop('v0')).any(1).astype(int)

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from timeit import timeit

res = pd.DataFrame(
       index=['user_chris', 'rahlf23', 'chris_a', 'chris', 'anton_vbr'],
       columns=[10, 50, 100, 500, 1000, 5000],
       dtype=float
)

for f in res.index:
    for c in res.columns:
        vals = np.random.randint(1, 100, (c, c))
        vals = np.column_stack((np.arange(vals.shape[0]), vals))
        df = pd.DataFrame(vals, columns=['id'] + [f'v{i}' for i in range(0, vals.shape[0])])
        stmt = '{}(df)'.format(f)
        setp = 'from __main__ import df, {}'.format(f)
        res.at[f, c] = timeit(stmt, setp, number=50)

ax = res.div(res.min()).T.plot(loglog=True)
ax.set_xlabel("N");
ax.set_ylabel("time (relative)");

plt.show()
    
```
