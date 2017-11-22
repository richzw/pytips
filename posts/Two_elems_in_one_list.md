### Question

I have a few lists, each containing several cities. I need to check for any two random elements if they belong to the same list.

Simple example:

```
list1 = ['London', 'Manchester', 'Liverpool', 'Edimburgh']
list2 = ['Dublin', 'Cork', 'Galway']
list3 = ['Berlin', 'Munich', 'Frankfurt', 'Paris', 'Milan', 'Rome', 'Madrid', 'Barcelona', 'Lisbon', ...]
list4 = ['Washington', 'New York', 'San Francisco', 'LA', 'Boston', ...]
```

Expected results:

```
> in_same_group('London', 'Liverpool')
> True
>
> in_same_group('Berlin', 'Washington')
> False
```

The function is called very often, so speed is critical. The biggest list might have up to 1000 elements.

### Answer

Dict of sets of indices

Here's a mix of Horia's proposal and my original one. You can define a dict with cities as key and sets of indices as values:

```
list1 = ['London', 'Manchester', 'Liverpool', 'Edimburgh']
list2 = ['Dublin', 'Cork', 'Galway', 'Paris', 'Rome']
list3 = ['Berlin', 'Munich', 'Frankfurt', 'Paris', 'Milan', 'Rome', 'Madrid', 'Barcelona', 'Lisbon'] 
list4 = ['Washington', 'New York', 'San Francisco', 'LA', 'Boston']
# Note that 'Paris' and 'Rome' are both in list2 and list3

groups = [list1, list2, list3, list4]

indices = {}

for i, group in enumerate(groups):
    for city in group:
        indices.setdefault(city, set()).add(i)
```

The structure is compact and looks like this:

```
print(indices)
#{'London': {0}, 'Manchester': {0}, 'Liverpool': {0}, 'Edimburgh': {0}, 'Dublin': {1}, 'Cork': {1}, 'Galway': {1}, 'Paris': {1, 2}, 'Rome': {1, 2}, 'Berlin': {2}, 'Munich': {2}, 'Frankfurt': {2}, 'Milan': {2}, 'Madrid': {2}, 'Barcelona': {2}, 'Lisbon': {2}, 'Washington': {3}, 'New York': {3}, 'San Francisco': {3}, 'LA': {3}, 'Boston': {3}}
```

For any city pair, you can get a set of common indices thanks to set intersection:

```
def common_groups(city1, city2):
    return indices.get(city1, set()) & indices.get(city2, set())

print(common_groups('London', 'Liverpool'))
#  {0}
print(common_groups('London', 'Paris'))
#  set()
print(common_groups('Cork', 'Paris'))
# {1}
print(common_groups('Rome', 'Paris'))
# {1, 2}
print(common_groups('Rome', 'Nowhere'))
# set()
```

An empty set is falsey in Python.

With n cities, creating the dict will be O(n), space requirement should be O(n) and lookup performance will be O(1). As a bonus, the query doesn't just return a boolean but a set of indices.

Finally, thanks to set intersections, this method would also work if you want to check that three or more cities are in the same group.

