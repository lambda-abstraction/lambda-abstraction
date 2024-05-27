# defining-generics

hmh.. so, we have already seen generic types like `list`, but, how do make your
own classes generic? and what about functions? can those be generic too? yes!
and in python 3.12 with the [`PEP 695 - Type Parameter
Syntax`](https://peps.python.org/pep-0695/)

lets say we want to define a function that takes a value of some type, lets
refer to it as `T`, and returns a tuple with this value appearing 2 times (like,
duplicating it) we could do it like that:

```py
def make_pair[T](x: T) -> tuple[T, T]:
    return (x, x)
```

the `[T]` after the function name means that it has a type variable `T`, that
needs to appear in the signature multiple times to be useful: and it does. what
is nice is that not only when we give it some `T` we get a `tuple[T, T]`
preserving that type, but that when we typehint something as `tuple[T, T]` but
assign it `make_pair(something_thats_not_a_T)` we get an error!

what about classes?
its the same syntax: `class ClassName[TypeVariables]` :d