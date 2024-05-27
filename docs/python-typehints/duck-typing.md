# duck-typing

now, knowing a bit about typehints, you might want to annotate your functions
that loop over something as expecting a `list` because thats what you usually
called it with, but there are other types that we can loop over too, right?

thats where duck typing, abstract classes and
protocols come into play: the idea behind it is that if something acts like a
duck, then it can be treated as a duck.

regarding the loop example: you can loop
over something if that something is "iterable", and for that there is a
[`collections.abc.Iterable`](https://docs.python.org/3/library/collections.abc.html#collections.abc.Iterable)
type, that is also generic over the type of the item you'll get when iterating
over that thing (e.g. a `list[int]` is an `Iterable[int]`), so, when you are typehinting a parameter as `list`: think,
should it really be specially a `list`, or just an `Iterable`? or maybe a
[`collections.abc.MutableSequence`](https://docs.python.org/3/library/collections.abc.html#collections.abc.MutableSequence)?
for something dict-like this would be the
[`collections.abc.Mapping`](https://docs.python.org/3/library/collections.abc.html#collections.abc.Mapping)
abstract class.

i'd recommend to take a look at the types in this module if you
want to write nice generic annotations :d
