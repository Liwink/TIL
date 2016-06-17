##### jQuery Features

* **利用 CSS 的优势**。通过将查找页面元素的机制构建在 CSS 选择符支行，jQuery 继承了简明清晰地表达文档结构的方式
* **支持扩展**。jQuery 将特殊情况下使用的工具归入插件当中。
* **抽象浏览器不一致性**。jQuery 添加一个抽象层来标准化常见的任务。
* **总是面向集合**。`.hide()` 之类的方法被设计为自动操作对象集合，而不是单独的对象。
* **将多重操作集于一行**。jQuery 在其多数方法中采用了连缀(chaining)的编程模式。这种模式下，对一个对象进行的多数操作，对象自身都会作为结果返回。

##### jQuery 代码

```javascript
$(document).ready(function() {
  $('div.poem-stanza').addClass('highlight');
});
```



* `$()` 函数来选择文档中的一部分，参数中可以包含任何「CSS选择符表达式」
* `.addClass()`加入新类，并且使用了隐式迭代机制，一次函数调用就可以完成对所有被选部分修改
* 执行代码。JavaScript代码在浏览器初次遇到它们时就会执行，而浏览器在处理头部时，**HTML 还不会呈现样式**，因此我们需要将代码延迟到 **DOM 可用**时在执行
  `$(document).ready()` 支持我们预定在DOM加载完毕后调用某个*函数*，同时这个函数解决了跨浏览器的支持

##### 纯 JavaScript 代码

```javascript
window.online = functions() {
  var divs = document.getElementsByTagName('div');
  for (var i = 0; i < divs.length; i++) { 
    if (hasClass(divs[i], 'poem-stanza') 
        && !hasClass(divs[i], 'highlight')) { 
      divs[i].className += 'highlight';
    }
  }
  function hasClass( elem, cls ) { 
  	var reClass = new RegExp(' ' + cls + ' ');
    return reClass.test(' ' + elem.className + ' ');
  }
}
```

上面代码并不能：

- 正确处理其他 window.onload 事件处理程序；
- 在 DOM 就绪后马上执行；
- 利用较新的 DOM 方法检索元素和执行其他任务，从而优化性能。

##### jQuery 和 React

从志军那了解到：

- React 可以算框架，而 jQuery 更像一个库，操作 DOM
- React 和 jQuery 的关系？React 组件化，是避免直接操作 DOM，更像 MCV。视图层捕获一个操作，事件传到 Controller 层，修改相应的状态，视图层再随着改变。





