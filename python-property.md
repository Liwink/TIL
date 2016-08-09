## Python property

> Properties are always class attributes, but they actually manage attribute access in the instaances of the class.

#### Instance data attribute shadows class attribute

When an instance and its class both have a data sttribute by the same name, the instance data attribute shadows the class attribute

```python
class Class:
    data = "the class data attr"
    @property
    def prop(self):
        return "the prop value"
    
obj = Class()
vars(obj) 	# {}
obj.data = "bar"
vars(obj) 	# {"data": "bar"}
obj.data 	# "bar"
Class.data	# "the class data atte"
```

#### Properties Override Instance Attributes

```python
Class.prop 					# <property object at 0x152>
obj.prop					# "the prop value"
obj.prop = "foo"			# AttributeError:
obj.__dict__["prop"] = "foo"
obj.prop					# "the prop value"
Class.prop = "bar"
obj.prop					# "foo"
```

