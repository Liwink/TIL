#### MongoKit 源码

`MongoKitConnection`

```python
class MongoKitConnection(object):
    def __init__(self, *args, **kwargs):
        self._databases = {}
        self._registered_documents = {}
        
    def register(self, obj_list):
        decorator = None
        if not isinstance(obj_list, _iterables):
            decorator = obj_list
            obj_list = [obj_list]    
        # cleanup
        # some code ...
        
        for obj in obj_list:
            CallableDocument = type(
                "Callable{0}".format(obj.__name__),
                (obj, CallableMixin),
                {"_obj_class": obj, "__repr__": object.__repr__}
            )
            self._registered_documents[obj.__name__] = CallableDocument
        
        if decorator is not None:
            return decorator
        
    def __getattr__(self, key):
        if key in self._registered_documents:
            document = self._registered_documents[key]
            try:
                return getattr(self[document.__database][document.__collection], key)
            except AttributeError:
                raise AttributeError("error...")
        else:
            if key not in self._databases:
                self._databases[key] = Database(self, key)
            return self._databases[key]
```



1. 动态生成 object，`type`
2. 动态获取对象，`__getattr__`