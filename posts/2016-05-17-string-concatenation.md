---
layout: post

title: empty concatenation
tip-number: 23
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the difference between += and .append in string operation

categories:
    - en
---

### What is the time complexity of the following codes?

```python
def urlify(string, length):
    '''function replaces single spaces with %20 and removes trailing spaces'''
    counter = 0
    output = ''
    for char in string:
        counter += 1
        if counter > length:
            return output
        elif char == ' ':
            output = output + '%20'
        elif char != ' ':
            output = output + char
    return output
```

Is `O(N)` or `o(N^2)`?

### Answer:

Per [Python Speed](https://wiki.python.org/moin/PythonSpeed#Use_the_best_algorithms_and_fastest_tools)

> String concatenation is best done with `''.join(seq)` which is an `O(n)` process. In contrast, using the `'+'` or `'+='` operators can 
result in an `O(n^2)` process because new strings may be built for each intermediate step. The CPython 2.4 interpreter mitigates this
issue somewhat; however, `''.join(seq)` remains the best practice.

Better solution could be

```python
output = []
    # ... loop thing
    output.append('%20')
    # ...
    output.append(char)
# ...
return ''.join(output)
```
