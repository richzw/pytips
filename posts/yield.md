Source: https://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do

- A function with yield, when called, returns a Generator.
- Generators are iterators because they implement the iterator protocol, so you can iterate over them.
- A generator can also be sent information, making it conceptually a coroutine.
- In Python 3, you can delegate from one generator to another in both directions with yield from.

```python
# coroutine
def bank_account(deposited, interest_rate):
    while True:
        calculated_interest = interest_rate * deposited 
        received = yield calculated_interest
        if received:
            deposited += received


>>> my_account = bank_account(1000, .05)
>>> first_year_interest = next(my_account)
>>> first_year_interest
50.0
>>> next_year_interest = my_account.send(first_year_interest + 1000)
>>> next_year_interest
102.5

# yield from
def money_manager(expected_rate):
    under_management = yield     # must receive deposited value
    while True:
        try:
            additional_investment = yield expected_rate * under_management 
            if additional_investment:
                under_management += additional_investment
        except GeneratorExit:
            '''TODO: write function to send unclaimed funds to state'''
        finally:
            '''TODO: write function to mail tax info to client'''


def investment_account(deposited, manager):
    '''very simple model of an investment account that delegates to a manager'''
    next(manager) # must queue up manager
    manager.send(deposited)
    while True:
        try:
            yield from manager
        except GeneratorExit:
            return manager.close()
            
>>> my_manager = money_manager(.06)
>>> my_account = investment_account(1000, my_manager)
>>> first_year_return = next(my_account)
>>> first_year_return
60.0
>>> next_year_return = my_account.send(first_year_return + 1000)
>>> next_year_return
123.6

```
