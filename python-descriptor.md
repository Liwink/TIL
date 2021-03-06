## Python descriptor

#### python descriptor 的实例：

> A descriptor is a class that implements a protocol consisting of the `__get__`, `__set__` and `__delete__` methods. The *property* class implements the full descriptor protocol.

```python
class Grade():
    def __init__(self):
        self._value = {}
       
    def __get__(self, instance, instance_type):
        if instance is None: return self
        return self._value.get(instance, 0)
    
    def __set__(self, instance, value):
        if not (0 <= value <= 100):
            raise ValueError("Grade must be between 0 and 100")
        self._value[instance] = value
        
class Exam():
    math_grade = Grade()
    english_grade = Grade()

>>> e = Exam()
>>> e.math_grade = 101
ValueError("Grade must be between 0 and 100")

```

1. `__set__` 和 `__get__` 可以对数值实时计算，赋值前包装处理
2. `@property` 可以简单实现类似功能，但不便于复用
3. 由于 `math_grade` 是类变量，所以为了区分每个实例的值，在 `Grade()` 内部用 dict 处理

上面实现很精妙，疑问在，为什么不将 `math_grade` 设为实例变量，这样就不需 `Grade()` 内部处理

#### Invoking Descriptors

> For objects, the machinery is in [`object.__getattribute__()`](https://docs.python.org/3/reference/datamodel.html#object.__getattribute__) which transforms `b.x` into `type(b).__dict__['x'].__get__(b,type(b))`. The implementation works through a precedence chain that gives data descriptors priority over instance variables, instance variables priority over non-data descriptors, and assigns lowest priority to [`__getattr__()`](https://docs.python.org/3/reference/datamodel.html#object.__getattr__) if provided.

调用 `obj.arg` 时，如果 obj 是一个实例（object），Python 的查询顺序是：

1. [`obj.__getattribute__()`](https://docs.python.org/3/reference/datamodel.html#object.__getattribute__)  唤起  `type(b).__dict__['x'].__get__(b, type(b))`
2. `b.__dict__['x']`
3. `type(b).__dict__['x']`
4. `b.__getattr__('x')`  (only handles nonexisting attribute name)

所以如果将 `Grade()` 设置为实例变量，调用机制就不会使用 `__get__`  `__set__`方法



- 类型（class）存储了所有是静态字段和方法（包括实例方法），而实例（instance）仅存储实例字段。
- 访问对象成员时，查找顺序：
  `instance.__dict__` -> `class.__dict__` -> `baseclass.__dict__`
  而非以往 globals、locals。



---

### Fluent Python Way

#### property

在 Fluent Python 中看到利用 property 实现复用的方式：

```python
def quantity(storage_name):
    def qty_getter(instance):
        return instance.__dict__[storage_name]
    def qty_setter(instance, value):
        if value > 0:
            instance.__dict__[storage_name] = value
        else:
            raise ValueError("value must be > 0")
    return property(qty_getter, qty_setter)

class LineItem:
    weight = quantity("weight")
    price = quantity("price")

    def __init__(self, weight, price):
        self.weight = weight
        self.price = price
```

这里可以实现功能，但确实比 descriptor 复杂。



#### descriptor

Fluent Python 给出的 descriptor 实现也比 Effective Python 中要好。原因如下：

> When coding a `__set__`  method, you must keep in mind what the `self` and `instance` arguments mean: `self` is the descriptor instance, and `instance` is the managed instance.
>
> Descriptors managing instance attributes should store values in the managed instances. That's why Python provides the instance argument to the descriptor methods.

```python
class Quantity(self, storage_name):
    def __init__(self, storage_name):
	    self.storage_name = storage_name
    def __set__(self, instance, value):
        if value > 0:
            instance.__dict__[self.storage_name] = value
        else:
            raise ValueError("value must be > 0")
    
class LineItem:
    weight = Quantity("weight")
	price = Quantity("price")
    
    def __init__(self, weight, price):
        self.weight = weight
        self.price = price
```

为 managed instance 赋值是需要使用 `instance.__dict__`，如果直接使用 dot 或 setattr 会无限递归。

但原文还是觉得 `price = Quantity("price")` 做法 *not-so-elegant*。更好的做法需要 21 章的 class decorator 或者 metaclass。

#### better descriptor

```python
class Quantity:
    __counter = 0
    
    def __init__(self):
        cls = self.__class__
        prefix = cls.__name__
        index = cls.__counter
        self.storage_name = "_{}#{}".format(prefix, index)
        cls.__counter += 1
    
    def __get__(self, instance, owner):
        if instance is None:
            return self
      	else:
            return getattr(instance, self.storage_name)
        
    def __set__(self, instance, value):
        if value > 0:
            setattr(instance, self.storage_name, value)
        else:
            raise ValueError("value must be > 0")
```

1. Quantity 初始化时不需要再传参数了
2. 兼容了 Class 访问时的异常