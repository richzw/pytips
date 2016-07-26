---
layout: post

title: floor division vs floor of division
tip-number: 41
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: The difference of floor division and floor of division

categories:
    - en
---

### Question

I know rounding errors happen in floating point arithmetic but can somebody explain the reason for this one:

```python
>>> 8.0 / 0.4  # as expected
20.0
>>> floor(8.0 / 0.4)  # int works too
20
>>> 8.0 // 0.4  # expecting 20.0
19.0
This happens on both Python 2 and 3 on x64.
```

As far as I see it this is either a bug or a very dumb specification of `//` since I don't see any reason why the last expression should
evaluate to `19.0`.

### Answer

As you and khelwood already noticed, `0.4` cannot be exactly represented as a float. Why? It is two fifth `(4/10 == 2/5)` which does not
have a finite binary fraction representation.

Try this:

```python
from fractions import Fraction
Fraction('8.0') // Fraction('0.4')
    # or equivalently
    #     Fraction(8, 1) // Fraction(2, 5)
    # or
    #     Fraction('8/1') // Fraction('2/5')
# 20
```

However

```python
Fraction('8') // Fraction(0.4)
# 19
```

Here, 0.4 is interpreted as a float literal (and thus a floating point binary number) which requires (binary) rounding, and only then
converted to the rational number `Fraction(3602879701896397, 9007199254740992)`, which is almost but not exactly `4 / 10`. Then the floored
division is executed, and because

```
19 * Fraction(3602879701896397, 9007199254740992) < 8.0
```

and

```
20 * Fraction(3602879701896397, 9007199254740992) > 8.0
```

the result is 19, not 20.

The same probably happens for

  `8.0 // 0.4`
  
I.e., it seems floored division is determined atomically (but on the only approximate float values of the interpreted float literals).

So why does

`floor(8.0 / 0.4)`
give the "right" result? Because there, two rounding errors cancel each other out. First 1) the division is performed, yielding
something slightly smaller than 20.0, but not representable as float. It gets rounded to the closest float, which happens to be 20.0.
Only then, the floor operation is performed, but now acting on exactly 20.0, thus not changing the number any more.

### Source

http://stackoverflow.com/questions/38588815/rounding-errors-in-python-floor-division

https://bugs.python.org/issue27463
