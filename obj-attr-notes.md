When you call `obj.attr`, how Python works?

here some notes:

#### getattribute

python doc : 
> `object.__getattribute__(self, name)`
>
> Called unconditionally to implement attribute accesses for instances of the class. If the class also defines `__getattr__()`, the latter will not be called unless `__getattribute__()` either calls it explicitly or raises an AttributeError. This method should return the (computed) attribute value or raise an [`AttributeError`](https://docs.python.org/3/library/exceptions.html#AttributeError) exception. In order to avoid infinite recursion in this method, its implementation should always call the base class method with the same name to access any attributes it needs, for example, `object.__getattribute__(self, name)`.

#### the order of looking up

1. `type(b).__dict__["x"].__get__(b, type(b))`
2. `b.__dict__["x"]`
3. `type(b).__dict__["x"]`
4. `b.__getattr__("x")`

#### scopes and namespaces

> A namespace is a mapping from names to objects.
>
> A scope is a textual region of a Python program where a namespace is directly accessible. "**Directly accessible**" here means that an unqualified reference to a name attempts to find the name in the namespace.

#### attribute

> I use the word *attribute* for any name following a dot - for example, in the expression `z.real`, `real` is an attribute of the object `z`.
>
> Attributes may be *read-only* or *writable*.

the two kinds of valid attribute names:

- data attributes
- methods: valid method names of an instance object depend on its class

the priority of data attributes and methods?

#### how to create namespace

- the build-in names is created when the Python interpreter starts up
- the global namespace is created when the module definition is read in
- the local namespace for a function is created when the function is called
- the local namespace for a class is created when a class definition is entered
- the local namespace for a object ?

#### nested scopes

> - the innermost scope contains the **local** names
> - the scopes of any **enclosing** function
> - the current modules's **global** names
> - the namespace containing **build-in** names

a read-one case:

> To rebind variables found outside of the innermost scope, the `nonlocal` statement can be used; if not declared nonlocal, those variables are *read-only* (an attempt to write to such a variable will simply create a *new* local variable in the innermost scope, leaving the identically name outer variable unchanged)



---



#### [descriptors](https://docs.python.org/3/howto/descriptor.html)

An object is considered a **descriptor** when it defines any of *descriptor protocol*: `__get__(self, obj, type=None)`, `__set__(self, obj, value)`, `__delete__(self, obj)`

- **data descriptor**: defines both `__get__()` and `__set__()`, whose priority over **instance variables**
- **non-data descriptor**: only defines `__get__()` and **instance variables** priority over **non-data descriptors**

>  Python methods (including `staticmethod()` and `classmethod()`) are implemented as non-data descriptors. Accordingly, instances can redefine and override methods.
>
> The `property()` function is implemented as a data descriptor.



#### how descriptors are called

> the machinery is in `object.__getattribute__()` which transforms `b.x` into `type(b).__dict__["x"].__get__(b, type(b))`. The implementation works through a precedence chain that gives **data descriptors** priority over instance variables, instance variables priority over **non-data descriptors**, and assigns lowest priority to `__getattr__()` if provided. The full C implementation can be found in [PyObject_GenericGetAttr()](https://docs.python.org/3.5/c-api/object.html#c.PyObject_GenericGetAttr) in [Objects/object.c](https://hg.python.org/cpython/file/3.5/Objects/object.c)



#### [invoking descriptors](https://docs.python.org/3/reference/datamodel.html#implementing-descriptors)

the starting point for descriptor invocation is a binding, `a.x`:

- Direct Call: `x.__get__(a)`, the simplest and least common call
- Instance Binding: `a.x` is transformed into the call: `type(a)__dict__["x"].__get__(a, type(a))`
- Class Binding: `A.x` is transformed into the call: `type(a).__dict__["x"].__get__(a, type(a))`
- Super Binding: if `a` is an instance of `super`, `super(B,obj).m()`  searches `obj.__class__.__mro__` for the base class `A` immediately preceding `B` and then invokes the descriptor with the call: `A.__dict__["m"].__get__(obj, obj.__class__)`









