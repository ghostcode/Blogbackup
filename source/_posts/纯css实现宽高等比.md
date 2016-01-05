title: 纯css实现宽高等比
date: 2015-12-30 22:22:04
categories:
tags:
---
动态实现宽高相等以及其它比例，一般使用的是JS。这次介绍一种使用纯CSS实现的方法。

基础结构与样式：

```
<div class="box">
</div>
```

```
.box{
    width:100px;
    background-color:#ddd;
}
```

1.宽：高 ＝ 1:1

```
.box:after{
    content:'';
    display:block;
    padding-top:100%;
}
```
2.宽：高 ＝ 1:2

```
.box:after{
    content:'';
    display:block;
    padding-top:200%;
}
```
3.宽：高 ＝ 2:1

```
.box:after{
    content:'';
    display:block;
    padding-top:50%;
}
```
<a class="jsbin-embed" href="http://jsbin.com/wesore/embed?css,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?3.35.5"></script>

动态修改一下宽度，会发现高度会自动跟着变化，并保持相应比例。原理很简单：

首先看下padding的取值：

> length: 长度表示法
percentage: 百分比表示法,padding百分比的计算是基于生成的框的包含块的宽度
auto: 自动

百分比的参考值是包含块的__宽度__，所以padding值的变化会随着宽度变化，其实颜色块的高度padding撑起来的。

