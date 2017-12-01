### Question: 

Given a string of a million numbers (Pi for example), write a function/program that returns all repeating 3 digit numbers and number of repetition greater than 1
For example: if the string was: 123412345123456 then the function/program would return:

123 - 3 times
234 - 3 times
345 - 2 times

### Answer

There is no way to process an arbitrarily-sized data structure in O(1) if, as in this case, you need to visit every element at
least once. The best you can hope for is O(n) in this case, where n is the length of the string.

First, by informing them that it's not possible to do it in O(1), unless you use the "suspect" reasoning given above.

Second, by showing your elite skills by providing Pythonic code such as:

```
str = '123412345123456'

# O(1) array creation.
freq = [0] * 1000

# O(n) string processing.
for val in [int(str[pos:pos+3]) for pos in range(len(str) - 2)]:
    freq[val] += 1
```

# O(1) output of relevant array values.

```
print ([(num, freq[num]) for num in range(1000) if freq[num] > 1])
```

This outputs:

`[(123, 3), (234, 3), (345, 2)]`

And, if they need better than that, there are ways to parallelise this sort of stuff that can greatly speed it up.

Not within a single Python interpreter of course, due to the GIL, but you could split the string into something like:

```
123412345
       45123456
```

then farm them out to separate workers and combine the results afterwards.


