
How to replace 'any strings' with nan in pandas DataFrame using a boolean mask?

```python
import pandas as pd
import random
import string
import numpy as np
pdn = pd.DataFrame(["".join([random.choice(string.ascii_letters) for i in range(3)]) for j in range (6)], columns =['Country Name'])
measures = pd.DataFrame(np.random.random_integers(10,size=(6,2)), columns=['Measure1','Measure2'])
df = pdn.merge(measures, how= 'inner', left_index=True, right_index =True)

df.iloc[4,1] = 'str'
df.iloc[1,2] = 'stuff'
print(df)

  Country Name Measure1 Measure2
0          tua        6        3
1          MDK        3    stuff
2          RJU        7        2
3          WyB        7        8
4          Nnr      str        3
5          rVN        7        4
```

How do I replace string values with np.nan in all columns without touching the country names

A1:

```python
cols = ['Measure1','Measure2']
mask = df[cols].applymap(lambda x: isinstance(x, (int, float)))

df[cols] = df[cols].where(mask)
print (df)
  Country Name Measure1 Measure2
0          uFv        7        8
1          vCr        5      NaN
2          qPp        2        6
3          QIC       10       10
4          Suy      NaN        8
5          eFS        6        4
```

A2:

```python
cols = ['Measure1','Measure2']
df[cols] = df[cols].applymap(lambda x: x if not isinstance(x, str) else np.nan)
or

df[cols] = df[cols].applymap(lambda x: np.nan if isinstance(x, str) else x)
Result:

In [22]: df
Out[22]:
  Country Name  Measure1  Measure2
0          nBl      10.0       9.0
1          Ayp       8.0       NaN
2          diz       4.0       1.0
3          aad       7.0       3.0
4          JYI       NaN      10.0
5          BJO       9.0       8.0
```

A3:

```python
cols = ['Measure1','Measure2']
df[cols] = df[cols].apply(pd.to_numeric,errors='coerce')
 Country Name  Measure1  Measure2
0          PuB       7.0       6.0
1          JHq       2.0       NaN
2          opE       4.0       3.0
3          pxl       3.0       6.0
4          ouP       NaN       4.0
```
5          qZR       4.0       6.0
