## Hashable

The key of `dict` must be hashable.

> Hashability makes an object usable as a dictionary key and a set member, because these data structures use the hash value internally.

#### What Is Hashable

Hashing is a process of converting some large amount of data to small amount in a repeatable way, so that it can be looked up in constant-time(O(1)).

In Python:

> An object is hashable if a it has a hash value which never changed during its lifetime(it needs a `__hash__` method), and can be compared to other objects(it needs an `__eq__` method). Hashable objects which compare equal must have the same hash value.

#### Hashable Types 

- The *atomic* immutable types are all hashable, such as `str`, `bytes`, `numeric` types
- A `frozen set` is always hashable(its elements must be hashable by definition)
- A `tuple` is hashable only if all its elements are hashable
- `User-defined types` are hashable by default because their hash value is their id()



```python
>>> tt = (1, 2, (30, 40))
>>> hash(tt)
8027212646858338501
>>> tl = (1, 2, [30, 40])
>>> hash(tl)
TypeError: unhashable type: 'list'
```





