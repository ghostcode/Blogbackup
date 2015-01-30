title: python中urllib和urllib2的区别联系
date: 2015-01-27 17:49:40
categories:
tags:
---
在python中，urllib和urllib2不可相互替代的。 

整体来说，urllib2是urllib的增强，但是urllib中有urllib2中所没有的函数。

* urllib2可以用urllib2.openurl中设置Request参数，来修改Header头。如果你访问一个网站，想更改User Agent（可以伪装你的浏览器），你就要用urllib2.
* urllib支持设置编码的函数，urllib.urlencode,在模拟登陆的时候，经常要post编码之后的参数，所以要想不使用第三方库完成模拟登录，你就需要使用urllib。

urllib一般和urllib2一起搭配使用

相关阅读：

[Different urllib and urllib2](http://www.hacksparrow.com/python-difference-between-urllib-and-urllib2.html)