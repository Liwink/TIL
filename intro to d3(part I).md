## Intro to D3.JS (part I)

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

create a bar chart *without* javaScript

...

#### Coding a Chart, Automatically

[`selection.enter()`](https://github.com/mbostock/d3/wiki/Selections#enter)
Returns the enter selection: placeholder nodes for each data element for which no corresponding existing DOM element was found in the current selection.
The enter selection only defines the `append`, `insert`, `select` and `call` operators; you must use these operators to **instantiate** the entering elements before modifying any content.

```
d3.select(".chart")  // select the chart container
  .selectAll("div")  
    .data(data)		 // join the data to the selection
  .enter().append("div")
    .style("width", function(d) { return d * 10 + "px"; })
    .text(function(d) { return d; });
```

#### Scaling to Fit

D3's scales specify a mapping from data space(domain) to display space(range).

```
var x = d3.scale.linear()
	.domain([0, d3.max(data)])
	.range([0, 420]);
```

