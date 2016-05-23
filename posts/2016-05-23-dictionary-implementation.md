---
layout: post

title: python dictionary implementation
tip-number: 28
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the details of dictionary implementation in python

categories:
    - en
---

### Overview

- Python dictionaries are implemented as **hash tables**.
- Hash tables must allow for **hash collisions** i.e. even if two distinct keys have the same hash value, the table's implementation must have
a strategy to insert and retrieve the key and value pairs unambiguously.
- Python dict uses **open addressing** to resolve hash collisions (explained below) .
- Python hash table is just a contiguous block of memory (sort of like an array, so you can do an `O(1)` lookup by index).
- Each slot in the table can store one and _only one entry_. This is important.
- Each entry in the table actually a combination of the three values: `< hash, key, value >`. This is implemented as a C struct

### Probe equation

The pseudo random probing for an empty dictionary slot using the equation `j = ((j*5) + 1) % 2**i`.

It cycles through all the remainders of `2**i`. For instance, if `i = 3` for a total starting size of `8`.  `j` goes through the cycle:

0
1
6
7
4
5
2
3
0

This is [`linear congruential generators`](https://en.wikipedia.org/wiki/Linear_congruential_generator). A linear congruential generator is a sequence that follows the relationship 

`X_(n+1) = (a * X_n + c) mod m`.

> The period of a general LCG is at most m, and for some choices of factor a much less than that. The LCG will have a full period for all seed values if and only if:

> `m` and `c` are relatively prime.

> `a - 1` is divisible by all prime factors of `m`.

> `a - 1` is divisible by `4` if m is divisible by `4`.

It's clear to see that `5` is the smallest a to satisfy these requirements, namely

- `2^i` and `1` are relatively prime.
- `4` is divisible by `2`.
- `4` is divisible by `4`.

Also interestingly, `5` is not the only number that satisfies these conditions. `9` will also work. Taking m to be `16`, using 
`j=(9*j+1)%16` yields

`0 1 10 11 4 5 14 15 8 9 2 3 12 13 6 7`

### Source

[1](http://stackoverflow.com/questions/327311/how-are-pythons-built-in-dictionaries-implemented)
[2](http://stackoverflow.com/questions/37378874/in-python-dictionaries-how-does-j51-2i-cycle-through-all-2i)
