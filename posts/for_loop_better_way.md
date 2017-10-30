Ref: https://stackoverflow.com/questions/46996315/python-for-loop-better-way


```python
import timeit

repeat = 10
total = 10

setup = """
count = 100000
"""

test1 = """
for _ in range(count):
    pass
"""

test2 = """
for _ in [0] * count:
    pass
"""

test3 = """
i = 0
while i < count:
    i += 1
"""

test4 = """
for _ in itertools.repeat(None, count):
    pass
"""
print(min(timeit.Timer(test1, setup=setup).repeat(repeat, total)))
print(min(timeit.Timer(test2, setup=setup).repeat(repeat, total)))
print(min(timeit.Timer(test3, setup=setup).repeat(repeat, total)))
print(min(timeit.Timer(test4, setup=setup).repeat(repeat, total)))

# Gives
0.02306803115612352
0.013021619340942758
0.06400113461638746
0.008105080015739174

```

for _ in itertools.repeat(None, count)
    do something
is the non-obvious way of getting the best of all worlds: tiny constant space requirement, and no new objects created per iteration. Under the covers, the C code for repeat uses a native C integer type (not a Python integer object!) to keep track of the count remaining.
