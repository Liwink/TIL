## Python descriptor 

#### python descriptor 的实例：

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
4. `b.__getattr__('x')`

所以如果将 `Grade()` 设置为实例变量，调用机制就不会使用 `__get__`  `__set__`方法





- 类型（class）存储了所有是静态字段和方法（包括实例方法），而实例（instance）仅存储实例字段。
- 访问对象成员时，查找顺序：
  `instance.__dict__` -> `class.__dict__` -> `baseclass.__dict__`
  而非以往 globals、locals。