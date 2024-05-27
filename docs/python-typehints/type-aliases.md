# type-alises

so.. sometimes, when we typehint our code, types can get a bit big and the code starts to look cluttered and its hard to write..
wouldnt it be nice if we could define shorthands for types?

and we can! using type aliases, and the `type` keyword-statement introduced with [`PEP 695 - Type Parameter
Syntax`](https://peps.python.org/pep-0695/)!

lets re-use the `make_pair` example from [`defining-generics`](defining-generics.md)

```py
def make_pair[T](x: T) -> tuple[T, T]:
    return (x, x)
```

why do we gotta write `tuple[T, T]`, if all we want is a "pair" of `T`'s? why dont we create a `Pair` type alias, such that `Pair[T]` is synonymous to `tuple[T, T]`?
here's how we'd do it:

```py
type Pair[T] = tuple[T, T]
```

now, `Pair[T]` is (for typing purposes) exactly the same as `tuple[Any, Any]` (and `Pair` with no type argument is like `tuple[Any, Any]`, although in a later version of python we should be able to override the default types for type variables)
