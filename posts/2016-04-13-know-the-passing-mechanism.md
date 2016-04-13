---
layout: post

title: know the passing mechanism 
tip-number: 01
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: Python function parameter passing mechanism

categories:
    - en
---

### Question: whether Python function calling model is `call-by-value` or `call-by-reference`?

The correct answer is: **neither**. Indeed, to try to shoe-horn those terms into a conversation about Python's model is misguided. 
`call-by-object` or `call-by-object-reference` is a more accurate way of describing it. 

- If you pass a **mutable object** into a method, the method gets a _reference_ to that same object and you can mutate it to your heart's
delight, but if you **rebind the reference** in the method, the outer scope will know nothing about it, and after you're done, 
the outer reference will still point at the original object.

- If you pass an **immutable object** to a method, you still **can't rebind** the outer reference, and you can't even _mutate_ the object.

Diagram

![](http://i.stack.imgur.com/hKDcu.png)

Sample codes

```python
# list - mutable type
def try_to_change_list_contents(the_list):
    print 'got', the_list
    the_list.append('four')
    print 'changed to', the_list

outer_list = ['one', 'two', 'three']

print 'before, outer_list =', outer_list
try_to_change_list_contents(outer_list)
print 'after, outer_list =', outer_list

'''
before, outer_list = ['one', 'two', 'three']
got ['one', 'two', 'three']
changed to ['one', 'two', 'three', 'four']
after, outer_list = ['one', 'two', 'three', 'four']
'''

# list - mutble type, change reference

def try_to_change_list_reference(the_list):
    print 'got', the_list
    the_list = ['and', 'we', 'can', 'not', 'lie']
    print 'set to', the_list

outer_list = ['we', 'like', 'proper', 'English']

print 'before, outer_list =', outer_list
try_to_change_list_reference(outer_list)
print 'after, outer_list =', outer_list

'''
before, outer_list = ['we', 'like', 'proper', 'English']
got ['we', 'like', 'proper', 'English']
set to ['and', 'we', 'can', 'not', 'lie']
after, outer_list = ['we', 'like', 'proper', 'English']
'''

# string - an immutable type

def try_to_change_string_reference(the_string):
    print 'got', the_string
    the_string = 'In a kingdom by the sea'
    print 'set to', the_string

outer_string = 'It was many and many a year ago'

print 'before, outer_string =', outer_string
try_to_change_string_reference(outer_string)
print 'after, outer_string =', outer_string

'''
before, outer_string = It was many and many a year ago
got It was many and many a year ago
set to In a kingdom by the sea
after, outer_string = It was many and many a year ago
'''

```

### Source:

[Is Python call-by-value or call-by-reference? Neither](https://jeffknupp.com/blog/2012/11/13/is-python-callbyvalue-or-callbyreference-neither/)

[SO Question](http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference)
