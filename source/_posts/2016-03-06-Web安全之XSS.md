title: Web安全之XSS
date: 2016-03-06 12:12:02
categories:
tags:
---
安全方面在写代码的时候从来没有考虑过，自从参加了公司的一次安全分享的会议，就多少在写代码的时候多加注意。理解了“一起用户的输入都是不安全”的含义，这次先总结一下安全里面的XSS的内容。

__名词解释__

>跨站脚本攻击（Cross Site Script）,通常指黑客通过“HTML注入”篡改网页，插入恶意脚本，从而在用户浏览网页时，控制用户浏览器的一种攻击。

__攻击方式：__

* 反射型XSS

>诱导用户点击恶意链接，反射型XSS又称“非持久型XSS”（Non-persistent XSS）

* 存储型XSS

>恶意代码存储在服务器端，浏览带有恶意代码的页面就会受到攻击，又称“持久型XSS”（Persistent XSS）

__危害__

* 破坏页面显示
* 窃取Cookie
* session劫持
* 重定向网址
* 非法转账

__攻击__

1. 反射型

页面有个输入框是根据URL的查询字符串显示的（最常见的是搜索）：

```
http://xxx.com?search=searchkey

<input type="text" value="searchkey" />
```
根据用户输入，则显示相应的关键词，如果“用户”不输入正常的字符，若输入：
`"/><script>alert(document.cookie)</script><!-`

然后把构造好的连接发给用户，诱导其点击：

```
http://xxx.com?search="/><script>alert(document.cookie)</script><!-
```

渲染的结果可能为：

```
<input type="text" value=" "/><script>alert(document.cookie)</script><!-" />
```

此时页面将弹出用户的cookie信息，这只是简单的模拟，只要存在XSS。脚本则可以发挥很大的破坏性。

防御：

>将重要的cookie标记为http only,这样的话js中的document.cookie语句就不能获取到cookie了. 

2. 存储型XSS

常见于留言板，评论等

存储类型的比反射型多了一个步骤，就是要把恶意代码提交到服务器，然后用户访问相应的页面（服务器返回带有恶意代码的页面）。

比如一篇文章中含有`<script>alert(document.cookie)</script>`，如果服务器没有做相应的处理，原样输出。当用户访问该文章时，则会弹出cookie信息。

防御:

>做特殊字符过滤，转码


<script src="https://gist.github.com/ghostcode/cdaa7ae95965ab0b6b2f.js"></script>

不要相信或假设用户的输入。


1. http://www.cnblogs.com/index-html/p/xss_long_live.html
2. http://www.cnblogs.com/hustskyking/p/xss-snippets.html
3. http://www.cnblogs.com/TankXiao/archive/2012/03/21/2337194.html
4. http://blog.csdn.net/ghsau/article/details/17027893