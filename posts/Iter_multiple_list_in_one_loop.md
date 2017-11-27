### Question
In Python 3.6.3 Is there a way to loop though one list after another?

For example:

deck = [(value, suit) for value in range(2, 11) +
            ["J", "Q", "K", "A"] for suit in ["H", "C", "D", "S"]]
          
(In this case, I want to loop through the face cards right after the non-face cards.)

For clarification: The above line throws a:

TypeError: unsupported operand type(s) for +: 'range' and 'list'

### Answer

range doesn't return a list in Python3, so range(2, 10) + ["J", "Q", "K", "A"] doesn't work, 
but list(range(2, 10)) + ["J", "Q", "K", "A"] does. You can also use itertools.chain to concatenate iterables:

from itertools import chain 

chain(range(2, 10), ["J", "Q", "K", "A"])
# or even shorter:
chain(range(2, 10), "JQKA")  # as strings themselves are iterables

# so this comprehension will work
deck = [
   (value, suit) 
   for value in chain(range(2, 10), "JQKA") 
   for suit in "HCDS"
]

The nested comprehension does, of course, constitute a cartesian product which you can also use a util for:

from itertools import product
deck = list(product(chain(range(2, 10), "JQKA"), "HCDS"))


