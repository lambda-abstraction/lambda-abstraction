# introduction

## what are typehints?

typehints (or, type annotations) are a way of "hinting" (so, not enforced) to
the reader of the code (which could be you, or another programmer, or a static
analysis tool (a typechecker: this will be our best friend when dealing with
types)) the **type** (like, ``int``, ``str``, ``list``) of variables, function
parameters and their return types, and attribute types (in classes).

## what do they do, why use them?

when developing, they help with formalizing what values you are going to work
with, and make it explicit, rather than just "thinking" (or, commenting, or
putting it in the name of a variable, e.g. ``things_list``) that e.g. your
function parameter should be a list and it will return a list back, and, in most
cases, given enough annotations typecheckers can prove that your program wont
have nasty runtime type-errors, like adding an int to a string, or somehing more
complicated that wont be deducible from an error traceback, without you needing
to actually run the code.

when using something thats typehinted they help with knowing what values to,
lets say, pass to a function, and what you'll get back, instead of guessing or
needing to open up documentation.

## what do they dont do?

sadly or not, typehints on their own do not impact code performance or make type
errors impossible. they are more like fancy comments in that sense, although
they can be inspected by code, and sometimes that makes great api's, like
dataclasses, to make good use of them you need to use a typechecker. there are 2
main ones: mypy and pyright, and a lot of editors have integration for them, for
example if you are using vscode - the pylance extension will bring the pyright
typechecker and some additional features, just dont forget to enable the
typechecking mode in settings (maybe a screenshot here? pyright > analysis >
typechecking mode)

## typehints syntax, and how to understand what is meant by typehinted code?

the syntax for them at the moment of python 3.12 is:

```python
variable_name: VariableType = value

def function_name(parameter: ParameterType) -> ReturnType:
    ...

class ClassName:
    attribute_name: AttributeType
```

so, what meaning does this have in practice? well, its kind of like an unsigned
contract that:

- `variable_name` is believed to have a value of type `VariableType`
- `function_name` was designed to work when you call it with an argument of type
  `ParameterType`, and is supposed to return a value of type `ReturnType`
- instances of the `ClassName` class should have an attribute `attribute_name`
  with a value of type `AttributeType`

## using typehints to prevent common errors

lets say you are making a function `f` for performing some calculations, like

```python
def f(x):
    return (x + 5) ** 2
```

and then you want to make the program interactive and call that function with
the user input, like ``f(input())``, but then, when running the code, oops..
there is a type error: "can only concatenate str (not "int") to str". if only we
could prevent such errors without needing to run the code, as in a lot of cases,
the types of values **can** be known.. and we can! using typehints and a
typechecker, we could typehint that in the function `f` the parameter `x` should
be a `float`, like so:

```python
def f(x: float):
    return (x + 5) ** 2
```

then, if we do just `f(input())` - our typechecker would be angry at us, and for
good reason! ``input()`` returns a ``str``, and a ``str`` is not a ``float``, so
it prevents us from making a type error, and now in all cases when we call our
function - it will help us match the argument type so we wont get an error when
actually running our code

## a bit about type inference

so, now that we know that we can typehint our variables wiht ``variable_name:
VariableType = value``, should we **always** do that? even when its, lets say,
``x: int = 42``, isnt that "obvious" that 42 is an int? well, yes it is! for
literals (like, ``1``, ``1.5``, ``"string"``), or expressions that "obviously"
evaluate to a certain type (like, calling a typehinted function, or accessing an
attribute thats typehinted) - typecheckers can infer the type without you
needing to specify it

## kind-of a cheatsheet/example of builtins and simplest generics

so, what are some simple types we can use in typehints that we work with in
almost every program?

```python
x: int = 1
x: float = 1.0
x: bool = True
x: str = "hello!"
x: bytes = b"hello!" # note the b""
```

what about storing "multiple things" in a variable, like what we use lists,
dicts, sets, tuples for? can we specify that we have a list where values are of
a specific type? yes we can!

```python
xs: list[int] = [1, 2, 3]
xs: set[int] = {1, 2, 3}
data: dict[str, int] = {"x": 0, "y": 1} # there can be multiple type parameters, in the case of dict, the first one is for the type of the keys, the second is for the values
xyz: tuple[int, int, int] = (1, 2, 3) # in tuples, we can even specify the type of each value separately!
xs: tuple[int, ...] = (0, 1, 2, 3) # in the context of typing, `...` can mean special things. for tuples, tuple[SomeType, ...] means a tuple of arbitrary size 
```

those types can be used without specifying a type parameter, but that way you
lose a lot of information and what we use typehints for, so some typecheckers
will even report that.

## the empty collection literal type inference problem

remember about "type inference", and that the typechecker can infer the type for
literals? well, thats true, but in the case of collections: like lists, dicts,
sets, ..., how would it know the type of an empty collection literal, like
``[]`` (an empty list)? so in that case, you should specify the type, like:

```python
numbers: list[float] = []
```

## subtypes / subclasses

so, does ``x: Type`` mean that ``x`` can only be of that specific type ``Type``?
well, not really. for example, booleans (``True`` and ``False``) can also be
used where ``int``'s are used, because the ``bool`` type is a subclass of
``int`` (if you are familiar with inheritance, its kinda like, ``class
bool(int):``), so ``x: int = True`` is perfectly valid.
