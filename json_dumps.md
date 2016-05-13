## json dumps

##### 问题

之前项目中，对 json.dumps() 做了一层包装，如下：

```

def custom_json_dumps(obj):
    """
    自定义json dumps 规范
    :param obj: json中key的取值
    :return:
    """
    if isinstance(obj, datetime.datetime):
        return obj.strftime('%Y-%m-%d %H:%M:%S')
    elif isinstance(obj, datetime.date):
        return obj.strftime('%Y-%m-%d')
    elif isinstance(obj, datetime.time):
        return '%s' % obj
    return str(obj)

def json_dumps(data, **kwargs):
    """
    json dumps
    :param data:
    :param kwargs:
    :return:
    """
    kwargs['default'] = kwargs.get('default') or custom_json_dumps
    return json.dumps(data, **kwargs)
```

1. 能否内部迭代
2. 这里 `default` 机制是怎么样，`default` 内部使用了 `return str(obj)`，是否会影响 `int` 等类型的转换。

##### 源码

最终到内部实现，内部其实有详细的说明：

```

class JSONEncoder(object):
    """Extensible JSON <http://json.org> encoder for Python data structures.

    Supports the following objects and types by default:

    +-------------------+---------------+
    | Python            | JSON          |
    +===================+===============+
    | dict              | object        |
    +-------------------+---------------+
    | list, tuple       | array         |
    +-------------------+---------------+
    | str               | string        |
    +-------------------+---------------+
    | int, float        | number        |
    +-------------------+---------------+
    | True              | true          |
    +-------------------+---------------+
    | False             | false         |
    +-------------------+---------------+
    | None              | null          |
    +-------------------+---------------+

    To extend this to recognize other objects, subclass and implement a
    ``.default()`` method with another method that returns a serializable
    object for ``o`` if possible, otherwise it should call the superclass
    implementation (to raise ``TypeError``).

    """
```

可以解答上面的问题。

##### 小结

1. 能去理解常用函数，这些基础太容易被忽视了，如果不是龙龙提到，我就不会注意。嗯，积累就是这么来的。
2. 迭代是怎么实现的，对list 和 dict，这个和 schema 有些类似。
3. 抛出异常，需要将异常显性暴露，所以默认 `str(obj)` 是不负责任的。