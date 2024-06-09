# duck typing, collections.abc and "the typing rule"

## duck typing

in a lot of cases, your parameter doesnt have to be strictly of some specific
type, but rather of a type that supports some operation - like, having length,
or being able to be iterated over. thats the idea behind "duck typing",
supported in python through use of protocols and abstract base classes ("abcs")

luckily, a lot of such types are defined in the standard
[``collections.abc``](https://docs.python.org/3/library/collections.abc.html)
module, for example:

## "can be looped over" type - Iterable

if you are implementing something like a ``sum`` function, the only thing you
really want from the parameter is for you to be able to iterate (loop) over it
and get numbers when doing so

instead of using ``list[float]``, you could use
[``collections.abc.Iterable[float]``](https://docs.python.org/3/library/collections.abc.html#collections.abc.Sequence),
then its valid to pass in anything thats an iterable of floats, be it a list of
them, or something else - like a tuple, or a dict whose keys are floats.

## "have length and can be indexed" type - Sequence

if you accept something that you expect to: have length, can be iterated over
and indexed with an int (like a tuple, or a list) - use
[``collections.abc.Sequence``](https://docs.python.org/3/library/collections.abc.html#collections.abc.Sequence)
if you also want that type to support being mutable (so, tuple wouldnt comply
here anymore, only the list) - use
[``collections.abc.MutableSequence``](https://docs.python.org/3/library/collections.abc.html#collections.abc.MutableSequence)

## "from key to value" type - Mapping

if you accept something that you expect to be like a dict, in the sense that it
maps values of one type to another (like a dict) - use
[``collections.abc.Mapping``](https://docs.python.org/3/library/collections.abc.html#collections.abc.Mapping)
(so, instead of a ``dict[K, V]``, you'd use ``Mapping[K, V]``) if you want it to
be mutable - same story as with sequence, there is a
[``collections.abc.MutableMapping``](https://docs.python.org/3/library/collections.abc.html#collections.abc.MutableMapping)
type. generally, the immutable one is the "base case", and a mutable version is
a special case with some additional methods

## "can be called with those parameters and return this value" type - Callable

sometimes, you accept things that can be called (like, other functions, commonly
called "callbacks") as parameters. but what is their type? well, it might be a
bunch of different types, as not only functions can be called, but anything with
a properly defined ``__call__`` dunder method, the
[``collections.abc.Callable``](https://docs.python.org/3/library/collections.abc.html#collections.abc.Callable)
type helps you typehint those, the syntax is ``Callable[[Arg1, ..., ArgN],
ReturnType]``, so, for example, in:

```py
def f(x: str, y: int) -> str:
    ...
```

``Callable[[str, int], str]`` could be used to represent the type of such a
function.

### protocols introduction

There is another option, to use
[``Protocols``](https://peps.python.org/pep-0544/) and define the ``__call__``
method signature yourself, in which case you'd also be able to specify the names
of parameters, making it more explanatory.

so, continuing the example, it would be like:

```python
from typing import Protocol

class Callback(Protocol):
    def __call__(self, x: str, y: int) -> str:
        ...
```

as you could guess - this is usable not only with ``__call__``, you can use this
to define things like in ``collections.abc`` yourself :d

!!! resource
    for magic methods like ``__call__`` you can look into the
    [``python data model``](https://docs.python.org/3/reference/datamodel.html)
    page.

## the typing rule

overall, try to be generic in parameter types, so your functions will be usable
with lots of different types and you wont need to convert everything to a
concrete type just to pass the typechecker, and strict in return types, so it
will be known exactly what is the result and what you can do with it
