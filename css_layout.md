## CSS Layout

#### no layout

after each line your eyes have a long distance to travel right-to-left to the next line

#### position

* static  默认的
* relative  相对默认位置
* absolate  相对 positioned 父层的位置
* fixed

#### display

* inline 
* inline-block
* block
* none

#### width

> Setting the `width` of a **block-level** element will **prevent** it from **stretching out to the edges** of its container to the left and right.

#### box-sizing

```
* {
  -webkit-box-sizing: border-box;
  	 -moz-box-sizing: border-box;
  		  box-sizing: border-box;
}
```

`box-sizing: border-box;` 
the padding and border of that element no longer increase its width.

#### float

