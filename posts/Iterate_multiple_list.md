Question
-----

Here is an example:

```
width = [0,1,2,3,4,6,7,8,9]
height = [0,1,2,3,4]
keys = [18,20,11]
```

The width and height are a range of integers up to the size of the width and height. The keys are any set of numbers (actually ASCII values) but are not ordered numbers.

I would like the output to be like this:

```
0 0 18
0 1 20
0 2 11
0 3 18
0 4 20
1 0 11
1 1 18
. . ..
9 0 20
9 1 11
9 2 18
9 3 20
9 4 11
```

 Answer
 ------
 
You can use `itertools.product` to get the product of your width and height, that is your whole grid. Finally, you want to cycle over the keys, thus use `itertools.cycle`. You then zip those together and get the desired result.

You can make this a generator using yield for memory efficieny.

```python
from itertools import product, cycle

def get_grid(width, height, keys):
    for pos, key in zip(product(width, height), cycle(keys)):
        yield (*pos, key)
```

Or if you do not want a generator.

```python
out = [(*pos, key) for pos, key in zip(product(width, height), cycle(keys))]
```
