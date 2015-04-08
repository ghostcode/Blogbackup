title: reload vs href
date: 2015-04-08 19:07:23
categories:
tags:
---
有时候会遇到一些需求，操作完某些任务后刷新页面。

一般你会怎么做？

是不是这样:

	window.location.href = window.location.href

vs:

	window.location.reload()

那你知道这两种有和区别吗？如果看官知道，请绕道，若不是很清楚请随我一起一探究竟。

* reload()会重新发送当前页面的POST数据，href 不会。
* 当前URl即使有hash值也会刷新，href不会。
* reload()有个可选参数 reload(true) 表示跳过缓存直接请求服务器数据，reload(false)相反。


http://stackoverflow.com/questions/2405117/difference-between-window-location-href-window-location-href-and-window-location


http://jsperf.com/location-href-vs-location-reload

https://developer.mozilla.org/en-US/docs/Web/API/Window/location