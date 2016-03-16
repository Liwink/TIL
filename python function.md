## 函数

### 将但方法的类转换为函数

##### 解决

```
from urllib.request import urlopen

class UrlTemplate:
	def __init__(self, template):
		self.template = template
	
	def open(self, **kwargs):
		return urlopen(self.template.format_map(kwargs))
```

这个类可以被一个更简单的函数来代替：

```
def urltemplate(template):
	def opener(**kwargs):
		return urlopen(template.format_map(kwargs))
	return opener
```

##### 讨论

大部分情况下，使用一个单方法类的原因是需要存储某些额外的状态来给方法使用。

使用一个内部函数或者闭包的方案通常会更优雅一些。简单说闭包就是一个函数，只不过在函数内部带上了一个额外的**变量环境**。

任何时候只要你需要给某个函数增加额外的状态信息，都可以考虑使用闭包。