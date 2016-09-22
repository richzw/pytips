---
layout: post

title: difference between float number 3*0.1 and 4*0.1
tip-number: 47
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: None

categories:
    - en
---

### Question

I know that most decimals don't have an exact floating point representation.

But I don't see why `4*0.1` is printed nicely as `0.4`, but `3*0.1` isn't, when both values actually have ugly decimal representations:

```python
>>> 3*0.1
0.30000000000000004
>>> 4*0.1
0.4
>>> from decimal import Decimal
>>> Decimal(3*0.1)
Decimal('0.3000000000000000444089209850062616169452667236328125')
>>> Decimal(4*0.1)
Decimal('0.40000000000000002220446049250313080847263336181640625')
```

### Answer

The simple answer is because `3*0.1 != 0.3` due to quantization (roundoff) error (whereas `4*0.1 == 0.4` because multiplying by a power
of two is usually an "exact" operation).

You can use the `.hex` method in Python to view the internal representation of a number (basically, the exact binary floating point 
value, rather than the base-10 approximation). This can help to explain what's going on under the hood.

```python
>>> (0.1).hex()
'0x1.999999999999ap-4'
>>> (0.3).hex()
'0x1.3333333333333p-2'
>>> (0.1*3).hex()
'0x1.3333333333334p-2'
>>> (0.4).hex()
'0x1.999999999999ap-2'
>>> (0.1*4).hex()
'0x1.999999999999ap-2'
```

`0.1` is `0x1.999999999999a` times `2^-4`. The `"a"` at the end means the digit `10` - in other words, `0.1` in binary floating point is
very slightly larger than the "exact" value of `0.1` (because the final `0x0.99` is rounded up to `0x0.a`). When you multiply this by `4`,
a power of two, the exponent shifts up (from `2^-4` to `2^-2`) but the number is otherwise unchanged, so `4*0.1 == 0.4.`

However, when you multiply by `3`, the little tiny difference between `0x0.99` and `0x0.a0 (0x0.07)` magnifies into a `0x0.15` error,
which shows up as a one-digit error in the last position. This causes `0.1*3` to be very slightly larger than the rounded value of `0.3`.

Python 3's float `repr` is designed to be round-trippable, that is, the value shown should be exactly convertible into the original 
value. Therefore, it cannot display `0.3` and `0.1*3` exactly the same way, or the two different numbers would end up the same after
round-tripping. Consequently, Python 3's repr engine chooses to display one with a slight apparent error.


