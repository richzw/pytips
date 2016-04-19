---
layout: post

title: staticmethod vs classmethod
tip-number: 05
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the difference between staticmethod and classmethod

categories:
    - en
---

**`@staticmethod`** function is nothing more than a function defined inside a class. It is _callable without instantiating the class first_.
It’s definition is **immutable via inheritance**.

- Python does not have to instantiate a _bound-method_ for object.
- It eases the readability of the code: seeing @staticmethod, we know that the method does not depend on the state of object itself;

**`@classmethod`** function also callable _without instantiating the class_, but its definition follows Sub class, not Parent class, 
via inheritance, can be **overridden** by subclass. That’s because the first argument for `@classmethod` function must always be `cls (class)`

- **Factory methods** that are used to create an instance for a class using for example some sort of pre-processing.
- **Static methods calling static methods**: if you split a static methods in several static methods, you shouldn't hard-code the class name but use class methods

### Source

[method guide in python](https://julien.danjou.info/blog/2013/guide-python-static-class-abstract-methods)
