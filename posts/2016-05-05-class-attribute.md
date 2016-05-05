---
layout: post

title: class attribute
tip-number: 19
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the details of class attribute

categories:
    - en
---

### Class attribute vs Instance attribute

```python
class MyClass(object):
    class_var = 1

    def __init__(self, i_var):
        self.i_var = i_var

foo = MyClass(2)
bar = MyClass(3)

foo.class_var, foo.i_var
## 1, 2
bar.class_var, bar.i_var
## 1, 3
MyClass.class_var ## <— This is key
## 1        
```

The class attribute is similar—but not identical—to the _static member_. Python _classes_ and _instances of classes_ each have their own
distinct namespaces represented by pre-defined attributes **`MyClass.__dict__`** and **`instance_of_MyClass.__dict__`**, respectively.

When you try to access an attribute from an instance of a class, it first looks at its _instance namespace_. If it finds the attribute, 
it returns the associated value. If not, it then looks in the _class namespace_ and returns the attribute (if it’s present, throwing an error otherwise

![](https://camo.githubusercontent.com/9de6dd5a5d69591d6715cf3ae1aa8a5c67c98eb8/687474703a2f2f6173736574732e746f7074616c2e696f2f75706c6f6164732f626c6f672f696d6167652f3330312f746f7074616c2d626c6f672d696d6167652d313339323832343539363538302e706e67)



### Source

[](https://www.toptal.com/python/python-class-attributes-an-overly-thorough-guide)
