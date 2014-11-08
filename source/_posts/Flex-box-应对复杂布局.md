title: 'Flex box 应对复杂布局'
date: 2014-07-20 22:32:32
tags:
---
###什么是Flex box?###

>The CSS3 Flexible Box, or flexbox, is a layout mode providing for the arrangement of elements on a page such that the elements behave predictably when the page layout must accommodate different screen sizes and different display devices. 

大体意思：

>css3的flexbox是一个网页布局模式，例如，当网页布局适配不同屏幕尺寸和不同设备的显示器时，元素表现的可预测。

通俗点说：flexbox布局模式可以调整子元素的宽、高，使元素最大限度的使用屏幕大小。一个伸缩容器，会扩展子元素来适应屏幕或者收缩子元素防止溢出。

###如何定义Flex box?###

```
display:flex
```
or
```
display:inline-flex
```
<!--more-->
注：
__2009年7月 工作草案 (display: box;)
2011年3月 工作草案 (display: flexbox;)
2011年11月 工作草案 (display: flexbox;)
2012年3月 工作草案 (display: flexbox;)
2012年6月 工作草案 (display: flex;)
2012年9月 候选推荐 (display: flex;)__

__目前来说(2013-12-11日)，手机端上的浏览器（uc,iphone自带浏览器等）还不支持候选推荐的方法，它仍然支持的方案是第一个(display:box;)，所以如果是手机h5页面，要使用这个版本的的语法。__

别以为移动端web开发不要处理兼容性，其实都是坑，都是浪费时间：
```
.flex-container { 
	display: -moz-box; /* Firefox */ 
	display: -ms-flexbox; /* IE10 */ 
	display: -webkit-box; /* Safari */ 
	display: -webkit-flex; /* Chrome, WebKit */ 
	display: box; 
	display: flexbox; 
	display: flex; 
	width: 100%; 
	height: 100%; 
	background-color: gray; 
} 
```

参考文章：

*	https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Flexible_boxes 
*	http://css-tricks.com/snippets/css/a-guide-to-flexbox/
*	http://learnlayout.com/flexbox.html
*	http://www.smashingmagazine.com/2011/09/19/css3-flexible-box-layout-explained/
*	http://www.smashingmagazine.com/2013/05/22/centering-elements-with-flexbox/
