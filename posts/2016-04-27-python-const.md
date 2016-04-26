---
layout: post

title: python const
tip-number: 14
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: const in python

categories:
    - en
---

### const implementation 

```python 
# const.py
class _const:
    class ConstError(TypeError): pass
    def __setattr__(self,name,value):
        if self.__dict__.has_key(name):
            raise self.ConstError, "Can't rebind const(%s)"%name
        self.__dict__[name]=value
import sys
sys.modules[__name__]=_const()

```

### Usage

```python
import const
# and bind an attribute ONCE:
const.magic = 23
# but NOT re-bind it:
const.magic = 88      # raises const.ConstError
# you may also want to add the obvious __delattr__
```
