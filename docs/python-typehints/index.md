# introduction

## who is this for?

for people who already know the basics of python, and are interested in learning
how to enhance their code with type annotations

## what are typehints, what do they do (and dont)?

so, for the python interpreter itself, typehints are kind-of like comments that
can be attached to variable assignments, function parameters, their return
values, and instance / class attributes, but because of its dynamically typed
nature, it was chosen for them to be allowed to be be any python expression, so:

```python
x: ["in theory"] * 2 = "this"
def f(y: ("is", [2, 4])) -> (lambda z: "valid"):
    ...
class C:
    attr: ("python", "code")
```

but in practice, typehints are **optional** indicators of **types**, so instead
of arbitrary python expressions, we only use types (classes) (like
[`int`](https://docs.python.org/3/library/functions.html#int),
[`str`](https://docs.python.org/3/library/functions.html#int)).

to clear out some misconceptions, without using any tools, they:

- do not make your code run faster
- do not prevent from assigning a value of the wrong type to some variable
- do not make it impossible to call a function with the wrong set of parameters

their usefulness comes largely from the typing ecosystem of libraries and tools
built **on top** of the language, and the fact that they are introspectable (you
can write code that would check the type annotations of something, and do
something with them: a good example is the [`@dataclasses.dataclass`
decorator](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass),
that helps generating boilerplate stuff like ``__init__`` and ``__repr__`` from
attribute annotations that are stored in the class's [``__annotations__``
attribute](https://docs.python.org/3/howto/annotations.html)).

the syntax for them (at the moment of python 3.12), is, like shown above:

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

tools, like typecheckers and IDE's can do a lot of stuff with this information

for example, if you annotated `x` as a `str`, then you could get autocompletion
of `str` methods on `x`, like, you could start typing `x.`, and stuff like
`.split()` would be suggested

typecheckers (like mypy and pyright) can verify that your code does work
according to the typehints and doesn't try to access methods that dont exist on
the type you annotated (e.g. you typehint `x` as an `int`, but try to `.split()`
it: `int` doesnt have `split` defined, so it'd spot out this as a type error)

the general rule i like to follow is to annotate types of parameters as generic
as possible, so they can be reusable in more places, and typehint return values / attributes / values
as strict as possible, so it is known exactly what the
data is and what you can do with it
