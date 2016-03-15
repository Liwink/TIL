## Modules and Packages

### 构建一个模块的层级包

> 即使没有 `__init__.py` 文件，python 仍然会导入包，这时实际上创建了一个所谓的「命名空间包」。

### 控制模块被全部导入的内容

```

	# somemodule.py
	def spam():
		pass
	
	def grok():
		pass
	
	blah = 42
	
	__all__ = ['spam', 'grok']
```

### 使用相对路径名导入包中子模块

```
mypackage/
    __init__.py
    A/
        __init__.py
        spam.py
        grok.py
    B/
        __init__.py
        bar.py
```

导入
```

	# mypackage/A/spam.py
	from . import grop

	# mypackage/A/spam.py
	from ..B import bar

```   

##### 讨论

* 使用相对导入，是不能到定义包的目录之外。也就是说，使用点的这种模式从不是包的目录中导入将会引发错误。
* 如果包的部分作为脚步直接执行，那它们将不起作用。 `python3 mypackage/A/spam.py   # Relative imports fail`

### 将模块分割成多个文件






