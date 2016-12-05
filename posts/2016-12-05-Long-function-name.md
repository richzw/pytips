---
layout: post

title: Solve long function name over 80 characters
tip-number: 50
tip-username: richzw
tip-username-profile: https://github.com/richzw
tip-tldr: pythonic way to solve function name over 80 characters

categories:
    - en
---

### Question

Our development team uses a PEP8 linter which requires a maximum line length of 80 characters.

When I'm writing unit tests in python, I like to have descriptive method names to describe what each test does. 
However this often leads to me exceeding the character limit.

Here is an example of a function that is too long...

```python
class ClientConnectionTest(unittest.TestCase):

    def test_that_client_event_listener_receives_connection_refused_error_without_server(self):
        self.given_server_is_offline()
        self.given_client_connection()
        self.when_client_connection_starts()
        self.then_client_receives_connection_refused_error()
```

### Solution

You could also write a decorator that `mutates .__name__` for the method.

```python
def test_name(name):
    def wrapper(f):
        f.__name__ = name
        return f
    return wrapper
Then you could write:

class ClientConnectionTest(unittest.TestCase):
    @test_name("test_that_client_event_listener_"
    "receives_connection_refused_error_without_server")
    def test_client_offline_behavior(self):
        self.given_server_is_offline()
        self.given_client_connection()
        self.when_client_connection_starts()
        self.then_client_receives_connection_refused_error()
```        
        
relying on the fact that Python concatenates source-adjacent string literals.
