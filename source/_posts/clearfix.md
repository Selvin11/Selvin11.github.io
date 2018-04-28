---
title: 清除浮动
date: 2016-10-17
categories: CSS
tags: 
  - BFC
---

支持IE6及以上的浏览器，全浏览器通用的`clearfix`方案，并梳理了一下`BFC`。

<!-- more -->

> clearfix 

```css
// 引入了zoom以支持IE6/7
// 同时加入:before以解决现代浏览器上边距折叠的问题
.clearfix:before,
.clearfix:after {
    display: table;
    content: " ";
}
.clearfix:after {
    clear: both;
}
.clearfix{
    *zoom: 1;
}
```

> BFC

1. BFC就是页面上的一个独立的容器，容器内的子元素不会影响到外面的元素，外面的元素也不会影响到容器内的子元素。
2. 盒子垂直方向的距离由margin决定，属于同一个BFC的相邻盒子的上下margin会发生折叠，分别触发两个元素的BFC，就可以解决垂直边距折叠的问题
3. BFC可以包含浮动，通常用来解决浮动父元素高度坍塌的问题。

4. BFC的触发方式：

* float 为 left | right
* overflow 为 hidden | auto | scorll
* display 为 table-cell | table-caption | inline-block | flex | inline-flex
* position 为 absolute | fixed

通常为父级元素添加`overflow:hidden;`兼容IE
