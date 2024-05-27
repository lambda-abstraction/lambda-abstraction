# type-variable-bounds-and-constraints

so, after [`defining-generics`](defining-generics.md) we already can define
generic types, but sometimes you want to represent that a type variable should
not be arbitrary, but be a subclass of some other type, or implement a protocol
/ abc, stuff like that, for that, in [`PEP 695 - Type Parameter
Syntax`](https://peps.python.org/pep-0695/) there's a special syntax:
`[TypeVariableName: BoundType]` this means that the `TypeVariableName` should be
a valid subclass of `BoundType`

this is very useful even for simple functions like `f(x: str) -> str` that
operate on concrete types when you take into consideration that people can
subclass those types and you want to preserve something like "the returned type
is same as given".

what about saying that a type variable can only be one of a set of some
predefined variants? this can be achieved by defining constraints on it, like
so: `[TypeVariableName: (Variant1, ..., VariantN)]`. Note that atleast 2 variants are
required.
