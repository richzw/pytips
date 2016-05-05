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

### Handle Assignment

- If a class attribute is set by accessing the class, it will override the value for all instances.

```python
foo = MyClass(2)
foo.class_var
## 1
MyClass.class_var = 2
foo.class_var
## 2
```

- If a class variable is set by accessing an instance, it will override the value **only** for that instance. This essentially overrides the class variable and turns it into an instance variable available, intuitively, _only for that instance_.

```python
foo = MyClass(2)
foo.class_var
## 1
foo.class_var = 2
foo.class_var
## 2
MyClass.class_var
## 1
```

### Mutability

_What if your class attribute has a **mutable type**? You can manipulate (mutilate?) the class attribute by accessing it through a particular instance and, in turn, end up manipulating the referenced object that all instances are accessing_

```python
class Service(object):
    data = []

    def __init__(self, other_data):
        self.other_data = other_data

s1 = Service(['a', 'b'])
s2 = Service(['c', 'd'])

s1.data.append(1)

s1.data
## [1]
s2.data
## [1]

s2.data.append(2)

s1.data
## [1, 2]
s2.data
## [1, 2]        
```

You should do it as following

```python
s1 = Service(['a', 'b'])
s2 = Service(['c', 'd'])

s1.data = [1]
s2.data = [2]

s1.data
## [1]
s2.data
## [2]
```

Or better solution could be

```python
class Service(object):
    data = None

    def __init__(self, other_data):
        self.other_data = other_data
```

### When to use it?

- _Storing constants_. As class attributes can be accessed as attributes of the class itself, it’s often nice to use them for storing Class-wide, Class-specific constants.

```python
class Circle(object):
    pi = 3.14159

    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return Circle.pi * self.radius * self.radius

Circle.pi
## 3.14159

c = Circle(10)
c.pi
## 3.14159
c.area()
## 314.159
```

- define default value

```python
class MyClass(object):
    limit = 10

    def __init__(self):
        self.data = []

    def item(self, i):
        return self.data[i]

    def add(self, e):
        if len(self.data) >= self.limit:
            raise Exception("Too many elements")
        self.data.append(e)

MyClass.limit
## 10
```

- Tracking all data across all instances of a given class

```python
class Person(object):
    all_names = []

    def __init__(self, name):
        self.name = name
        Person.all_names.append(name)

joe = Person('Joe')
bob = Person('Bob')
print Person.all_names
## ['Joe', 'Bob']
```


### Source

[class attribute overly guide](https://www.toptal.com/python/python-class-attributes-an-overly-thorough-guide)
