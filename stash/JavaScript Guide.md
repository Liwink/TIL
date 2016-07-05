### 5.10 性能开销

> 编写处理数组的程序时，最优雅的方式是讲程序描述成清晰分离的步骤序列，每一步完成一些任务，并生成一个新数组。但构造这些中间数组却需要很高的性能开销。

### 5.11 曾祖父

可配置的迭代

```javascript
function reduceAncestors(person, f, defaultValue) {
  function valueFor(person) {
    if (person == null)
      return defualtValue;
    else
      return f(person, valueFor(byName[person.mother]),
                       valueFor(byName(person.father)));
  }
  valueFor(person);
}
```

对应 Python 版

```python
def reduce_ancestors(person, f, defualt_value):
    def value_for(p):
        if(p == null):
            return defualt_value
        else:
            return f(p, value_for(by_name[p.mother]),
                             value_for(by_name[p.father]))
    return value_for(person)
```



### 6.4 构造函数

```javascript
function Rabbit(type) {
  this.type = type;
}
var = killerRabbit = new Rabbit("killer")

Rabbit.prototype.speak = function(line) {
  console.log("The" + this.type + "says '" +
              line + "'")
};
blackRabbit.speak("Doom...")
// > The killer says 'Doom...'
```

可以动态添加原型的属性。



### 6.11 继承

> 封装与多态可以用来分隔代码，减少整个程序的耦合性，而继承则使类型紧密耦合，增加了耦合性。



```javascript
function Vector(x, y) {
  this.x = x
  this.y = y
  
  this.plus = function(vector) {
    return new Vector(this.x + vector.x, this.y + vector.y)
  }
}
Object.defineProperty(Vector.prototype, "length", {
  get: function() { return Math.sqrt(Math.pow(this.x, 2), Math.pow(this.y, 2)) }
})
```





|        | JavaScript                               | Python                                   | Ruby                                     |
| ------ | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| Class  | function Rabbit(type) { this.type = type } | `class Rabbit: def __init__(self, type): self.type = type` | `class Rabbit def initialize(type) @type = type end end  ` |
| Object | object = new Class                       | object = Class()                         | object = Class.new                       |
| 继承     |                                          |                                          |                                          |
|        |                                          |                                          |                                          |
|        |                                          |                                          |                                          |
|        |                                          |                                          |                                          |



### [Javascript模块化编程](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)

> Javascript不是一种模块化编程语言，它不支持"[类](http://www.ruanyifeng.com/blog/2012/07/three_ways_to_define_a_javascript_class.html)"（class），更遑论"模块"（module）了。（正在制定中的[ECMAScript标准](http://en.wikipedia.org/wiki/ECMAScript)第六版，将正式支持"类"和"模块"，但还需要很长时间才能投入实用。）