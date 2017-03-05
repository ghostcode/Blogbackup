title: 'weinre远程调试web mobile'
date: 2014-07-20 16:34:07
tags:
---
<!-- ![weinre](http://img1.tuchuang.org/uploads/2014/07/weinre_demo.jpg) -->
__什么是weinre?__

>weinre is a debugger for web pages, like FireBug (for FireFox) and Web Inspector (for WebKit-based browsers), except it's designed to work remotely, and in particular, to allow you debug web pages on a mobile device such as a phone.

大体意思：

>weinre是网页的调试工具，就像FireFox里面的firebug和webkit内核的开发者工具，唯一的区别它是为远程调试设计的，特点是它可以调试手机上的网页。

__如何使用？__

* 获取Weinre

在任何支持Node.js环境的系统下，通过包管理器（NPM）即可安装Weinre
```
 npm install -g weinre
```

* 开启Weinre调试

```
weinre --boundHost -all-
```
<!--more-->
* 通过PC浏览器（webkit）打开调试页面

	http://localhost:8080

![img](/assets/images/weinre-demo.jpg)

* 在调试页面添加脚本

	<script src="http://localhost[Your IP]:8080/target/target-script-min.js#anonymous"></script>

__注意：这里的localhost要改为自己本机的ip地址__


* 手机访问你要调试的页面，看是否通信成功。看targets下面有连接基本就对了。

* 最后这就像chrome的debug一样了

[weinre](http://people.apache.org/~pmuellr/weinre-docs/latest/Home.html)

[参考](http://dearb.me/archive/2013-11-21/web-inspector-remote-debugger-weinre/)

[youtube](https://www.youtube.com/results?search_query=weinre)