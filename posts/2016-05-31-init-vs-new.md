---
layout: post

title: __init__ vs __new__
tip-number: 31
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: the difference between __init__ and __new__ in python

categories:
    - en
---

> Use **__new__** when you need to control the creation of a new instance. 
> Use **__init__** when you need to control initialization of a new instance.

> **__new__** is the first step of instance creation. It's called first, and is responsible for returning a new instance of your class. 
In contrast, **__init__** doesn't return anything; it's only responsible for initializing the instance after it's been created.

> In general, you shouldn't need to override **__new__** unless you're subclassing an immutable type like `str`, `int`, `unicode` or `tuple`.

`__new__` is static class method, while `__init__` is instance method. `__new__` has to create the instance first, so `__init__` can initialize it.
Note that `__init__` takes `self` as parameter. Until you create instance there is no `self`.



