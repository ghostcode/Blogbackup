title: JavaScript Template实现
date: 2016-12-23 09:27:46
categories:
tags:
---


模板引擎做了两件事：

1. 拼接字符串
2. 执行字符串（new Function）

var re = /<%([^%>]+)?%>/g;

这句正则表达式会捕获所有以<%开头，以%>结尾的片段。