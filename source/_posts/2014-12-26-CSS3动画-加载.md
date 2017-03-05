title: CSS3动画-加载
date: 2014-12-26 16:54:33
categories:
tags: css3
---
css3的动画还是值得看看的，争取多看多写多分享一些动画效果。这次就以网站中常见的加载动画开始：

<a class="jsbin-embed" href="http://jsbin.com/rediko/1/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

这里用到的css3的属性：

>animation

语法:

	animation: name duration timing-function delay iteration-count direction

详情：[animation](https://developer.mozilla.org/en-US/docs/Web/CSS/animation)

<!-- more -->

>keyframes

__keyframes就是对应上面animation-name的__

	@keyframes animationName {
	  0% {
	    font-size: 10px;
	  }
	  30% {
	    font-size: 15px;
	  }
	  100% {
	    font-size: 12px;
	  }
	}

详情：[keyframes](http://www.smashingmagazine.com/2011/05/17/an-introduction-to-css3-keyframe-animations/)

>box-shadow

语法：

	box-shadow: h-shadow v-shadow blur spread color inset;

关键点：

1. `animation-delay:`有时间差才会有高低区分
2. keyframes中的`box-shadow`，来增加向上伸展的部分