
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
5          qZR       4.0       6.0
```

Q: I want to increment the column "Expected output" only if "servo_in_position" changes from 0 to 1. I want also to assume "Expected output" to be 0 (null) if "servo_in_position" equals to 0.

```
 servo_in_position   second_servo_in_position    Expected output
0   0   1   0
1   0   1   0
2   1   2   1
3   0   3   0
4   1   4   2
5   1   4   2
6   0   5   0
7   0   5   0
8   1   6   3
9   0   7   0
10  1   8   4
11  0   9   0
12  1   10  5
13  1   10  5
14  1   10  5
15  0   11  0
16  0   11  0
17  0   11  0
18  1   12  6
19  1   12  6
20  0   13  0
21  0   13  0
22  0   13  0
```

A1:

```python
df['E_output'] = df['servo_in_position'].diff().eq(1).cumsum()\
                                        .mask(df['servo_in_position'] == 0, 0)
```

```python
df.servo_in_position.diff().eq(1).cumsum().mul(df.servo_in_position.eq(1),axis=0)
```

