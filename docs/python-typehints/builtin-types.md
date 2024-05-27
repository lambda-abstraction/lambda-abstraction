# builtin-types

so, what types are there even? what do the usual values we use correspond to in
types?

```python
x: int = 1 # (1)
x: float = 1.0
x: bool = True 
x: int = True # (2) 
x: str = "hi!"
x: bytes = b"hi!" # (3) 
```

1. `int`s can be passed to where `float`s are expected
2. `bool` is a sublcass of `int`, so by liskov substitution principle, we can
   use `bool`s where ints are expected
3. note the `b` before the string literal: thats a byte string literal :d

!!! note
    in all of those cases, the type could be inferred (like, determined
    without you specifying it) by the type-checker, so providing it was a bit
    redundant.

those are some simple types that make up the values we use most of the time

but, what about types that store other things? like lists, dictionaries?

those actually have a type parameter, representing the things they store,
although when not given, its kind of like saying that it could be anything.

```python
xs: list = [0, ""] # (1)
xs: list[int] = [1, 2, 3] # (2)
xs: set[int] = {0, 1}
xs: dict[str, int] = {"x": 0, "y": 1} # (3)
xs: tuple[int, str, float] = (1, "hi", 0.0)
xs: tuple[int, ...] = (1, 2, 3) # (4) 
```

1. list of `Any`thing
2. the `Type[OtherType]` is syntax for so called "generic" types: types that
   have other types as parameters, in this case, this can be read as "`list` of
   `int`s".
3. there can be multiple type parameters, in dictionaries the first one is for
   the type of keys, the second one is for values
4. arbitrary length tuple of integers, the `...` can have some special meaning
   in the context of typing

!!! note
    in the case of mutable collections (like `list`s), providing the type
    is not redundant: the type cannot be inferred, as you could have e.g. an
    empty `list`, that then later gets an `int` appended to it.

what about cases, where we want to represent that a variable / parameter could
be of multiple types? for that, we can use union types:

```python
x: int | str = 0
x: int | str = ""
```

how about typehinting arguments with values that default to `None` (common
python practice)? well, just use a union of your argument type and `None`: by
typecheckers, `None` is treated as the type of `None`
