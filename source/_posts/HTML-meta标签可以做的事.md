title: HTML meta标签可以做的事
date: 2014-11-16 21:21:36
categories:
tags: HTML
---
![meta](http://zhuxinyong.com/assets/images/meta-tag.jpg)

Meta标签是用来存储一些页面信息，目的是使浏览器和搜索引擎更好的理解页面。

作为网站开发者你可以通过meta存储页面描述，作者和关键词。或许有些标签我们不是很熟悉，这里我搜集了5个：

＊ 控制浏览器缓存

当你浏览器某个网站时，浏览器会帮你缓存一些东西以使后来的访问更快速。你或许经历过网站已经更新但你浏览的依旧是以前的，这是因为浏览器显示缓存的页面，你可以禁止浏览器缓存：

	<meta http-equiv="Cache-Control" content="no-store" />  

这在火狐，谷歌和IE中都可以使用，在IE中你还可以声明更多：

	<meta http-equiv="Cache-Control" content="no-cache" />  
	<meta http-equiv="Pragma" content="no-cache" />  

你依旧可以设置过期时间，当过期时就从服务器获取新数据：

	<meta http-equiv="expires" content="Fri, 18 Jul 2014 1:00:00 GMT" />  

当你指定为“0”时，每次浏览页面时都会去请求新数据。

＊ 设置Cookies

类似于缓存，cookie是浏览器存储在本地的一些信息。站点可以通过它来定制网站功能。一个真实的例子就是购物网站，当你向购物车添加一些商品后，就算你离开网站好久但没有登出，商品依旧会留在购物车里面。

	<meta http-equiv="Set-Cookie" content="name=data; path=path; expires=Day, DD-MMM-YY HH:MM:SS ZONE">

name＝data是数据的唯一标示，path是存储的位置，expires是过期时间，若设置为空，一旦你退出浏览器cookie就会被删除。

＊ 刷新网页

你可以定时去刷新页面，__meta http-equiv="refresh"__可以指定一个刷新时间：

	<meta http-equiv="refresh" content="5">  

![meta-tag-refresh](http://zhuxinyong.com/assets/images/meta-tag-refresh.gif)

* 重定向

你依旧可以指定时间后跳转到另一个页面：

	<meta http-equiv="refresh" content="5; url=http://example.com/"> 

![meta-tag-refresh](http://zhuxinyong.com/assets/images/meta-tag-redirect.gif)

如果向立即跳转，你需要设置时间为0.

	<meta http-equiv="refresh" content="0; url=http://example.com/">  

* 页面动画

你可以像PPT一样为你的页面添加动画：

	<meta http-equiv="page-enter" content="revealtrans(duration=seconds,transition=num)" /> 

	<meta http-equiv="page-exit" content="revealtrans(duration=seconds,transition=num)" />  

	<meta http-equiv="page-enter" content="blendTrans(duration=sec)" />  

注意这只在老版本的IE中可以使用。

原文：[HTML meta](http://www.hongkiat.com/blog/meta-tag-hidden-features/)



















