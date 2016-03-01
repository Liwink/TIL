## Intro to D3.JS

### Web Standards

##### HTML

* a **text format** that most web pages are written in
* using **a standard set of tags** to define the different structural components of a webpage
* `<div>` and `<span>` tags not applied default styles by browers 

##### CSS

CSS(Cascading Stylesheets) is a language for styling HTML pages.

##### The DOM

> When a browser displays an HTML page, it creates an interactive object graph from the tag hierarchy. This object graph is called the Document Object Model, or DOM.

```
// DOM API
document.getElementById('some-id');
```

##### SVG

SVG(Scalable Vector Graphics) is an XML format used for drawing.

### D3

#### Selecting an Element

select a single element and create a selection for it

```
var body = d3.select("body");
var div = body.append("div");
div.html("Hello, world!");
```

perform the same operation on many elements

```
var body = d3.selectAll("body");
var div = body.append("div");
div.html("Hello, world!");
```

#### Chaining Methods

Selection methods, such as `selection.attr`,, return the current selection.

This lets us easily apply multiple operations to the same elements.

```
d3.select("body")
	.style("color", "black")
	.style("background-color", "white");
```

`selection.append` returns a new selection containing the new elements.

```
d3.selectAll("section")
	.attr("class", "special")
  .append("div")
    .html("Hello, world!");
```

#### Coding a Chart, Manually


`select` 用于选中一个元素，`selectAll` 选中一组元素。
`selectAll` 选中了所有现有和**将来可能出现**的元素。
大部分 D3 的方法都返回 D3 对象实例，所以可以采用链式写法。

```
var svg = d3.select('svg');
var rects = svg.selectAll('rect')
  .data(sales);

var newRects = rects.enter();

var maxCount = d3.max(sales, function(d, i) {
  return d.count;
});
var x = d3.scale.linear()
  .range([0, 300])
  .domain([0, maxCount]);
var y = d3.scale.ordinal()
  .rangeRoundBands([0, 75])
  .domain(sales.map(function(d, i) {
    return d.product
  }));

newRects.append('rect')
  .attr('x', x(0))
  .attr('y', function(d, i) {
    return y(d.product);
  })
  .attr('height', y.rangeBand())
  .attr('width', function(d, i) {
    return x(d.count);
  });
```

