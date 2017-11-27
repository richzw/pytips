### Question
I have a dataframe that looks like this:

```
from random import randint
import pandas as pd

df = pd.DataFrame({"ID": ["a", "b", "c", "d", "e", "f", "g"], 
                   "Size": [randint(0,9) for i in range(0,7)]})

df

  ID  Size
0  a     4
1  b     3
2  c     0
3  d     2
4  e     9
5  f     5
6  g     3
```

And what I would like to obtain is this (could be a matrix as well):

```
sums_df

      a     b    c     d     e     f     g
a   8.0   7.0  4.0   6.0  13.0   9.0   7.0
b   7.0   6.0  3.0   5.0  12.0   8.0   6.0
c   4.0   3.0  0.0   2.0   9.0   5.0   3.0
d   6.0   5.0  2.0   4.0  11.0   7.0   5.0
e  13.0  12.0  9.0  11.0  18.0  14.0  12.0
f   9.0   8.0  5.0   7.0  14.0  10.0   8.0
g   7.0   6.0  3.0   5.0  12.0   8.0   6.0
```

### Solution

use np.add.outer():

```
In [65]: pd.DataFrame(np.add.outer(df['Size'], df['Size']),
                      columns=df['ID'].values,
                      index=df['ID'].values)
Out[65]:
    a   b  c   d   e   f   g
a   8   7  4   6  13   9   7
b   7   6  3   5  12   8   6
c   4   3  0   2   9   5   3
d   6   5  2   4  11   7   5
e  13  12  9  11  18  14  12
f   9   8  5   7  14  10   8
g   7   6  3   5  12   8   6
```

Use Numpy's broadcasting

```
size = df.Size.values
ids = df.ID.values

pd.DataFrame(
    size[:, None] + size,
    ids, ids
)

    a   b  c   d   e   f   g
a   8   7  4   6  13   9   7
b   7   6  3   5  12   8   6
c   4   3  0   2   9   5   3
d   6   5  2   4  11   7   5
e  13  12  9  11  18  14  12
f   9   8  5   7  14  10   8
g   7   6  3   5  12   8   6
```



