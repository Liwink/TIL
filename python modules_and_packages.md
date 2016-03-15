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

### 利用命名空间导入目录分散的代码

##### 解决

```
	foo-package/
    	spam/
        	blah.py

	bar-package/
    	spam/
        	grok.py
```

```
	>>> import sys
	>>> sys.path.extend(['foo-package', 'bar-package'])
	>>> import spam.blah
	>>> import spam.grok

```

### 重新加载模块

```
	>>> import spam
	>>> import imp
	>>> imp.reload(spam)
```

### 运行目录或压缩文件

```
	myapplication/
	    spam.py
	    bar.py
	    grok.py
	    __main__.py
```

```
python3 myapplication
```

### 读取位于包中的数据文件

### 将文件加入到 sys.path

##### 解决方案

三种方法：

1. 用 PYTHONPATH 环境变量来添加

		bash % env PYTHONPATH=/some/dir/:/other/dir python3

2. 创建一个 .pth 文件，将目录列举出来

		# myapplication.pth
		/some/dir
		/other/dir
	
	这个 .pth 文件需要放在某个 Python 的 site-packages 目录，通常位于 /usr/local/lib/python3.3/site-packages

3. 手动调节 `sys.path`的值

		import sys
		from os.path import abspath, join, dirname
		
		sys.path.insert(0, abspath(dirname('__file__'), 'src'))

### 通过字符串名导入模块

##### 解决方案

```
>>> import importlib
>>> math = importlib.import_module('math')
```