---
layout: post

title: optimize codes with closure
tip-number: 14
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: one good example to illustrate code optimization through closure

categories:
    - en
---

### Question: implement one function to filter the page through one field

### Approach 1: Classes

```python
class PageCategoryFilter(object):
    def __init__(self, config):
        self.mode = config["mode"]
        self.categories = config["categories"]

    def filter(self, bid_request):
        if self.mode == "whitelist":
            return bool(
                bid_request["categories"] & self.categories
            )
        else:
            return bool(
                self.categories and not
                bid_request["categories"] & self.categories
            )
```

**Performance issue**: _the **bound** method_

An instance’s methods are access via a **descriptor**, a Python feature that allows some code to be executed to satisfy the results of 
an attribute access expression. When Python executes the definition of a class, it wraps each function in a **descriptor** 
whose job is to supply the `self` argument. Later, when you access the attribute for the method, Python calls `__get__` on the descriptor,
and supplies the instance as an argument. This allows the method descriptor to rewrite the call to the underlying function to include the `self` argument.

### Approach 1: Functions

To avoid the `self` attribute, an ordinary funcion could be one solution

```python
def page_category_filter(bid_request, config):
    if config["mode"] == "whitelist":
        return bool(
            bid_request["categories"] & config["categories"]
        )
    else:
        return bool(
            config["categories"] and not
            bid_request["categories"] & config["categories"]
        )
```

**The dictionary access problem**:

We do three dictionary accesses: 
- one for `mode`, 
- one for the `“categories”` item in the bid request dictionary; 
- one for the “categories” item in the configuration dictionary. 

Since none of these accesses is repeated in any individual call of this function, it makes no sense to assign the result of
the dictionary accesses to a local variable and thus `“cache”` the result.

Ordinarily, and algorithmically, yes, dictionaries are quite fast. However, hidden behind those square brackets is quite a bit of work:
Python must hash the key, apply a bit-mask to the hash, and look for the item in an array that represents the storage for the dictionary. 
Edge cases can create collisions which take even longer to resolve. 

### Approach 3: Closure

```python
def make_page_category_filter(config):
    categories = config["categories"]
    mode = config["mode"]
    def page_category_filter(bid_request):
        if mode == "whitelist":
            return bool(bid_request["categories"] & categories)
        else:
            return bool(
                categories and not
                bid_request["categories"] & categories
            )
    return page_category_filter
```

### Source:

[A](http://tech.magnetic.com/2015/05/optimize-python-with-closures.html)
