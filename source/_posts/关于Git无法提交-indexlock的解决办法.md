title: '关于Git无法提交 index.lock的解决办法'
date: 2014-08-05 17:26:32
tags:
---
有时候提交代码时，会报错：

	$ git commit -a

>fatal: Unable to create 'e:/git/Android/XXXXXX/.git/index.lock': File exists.
If no other git process is currently running, this probably means a
git process crashed in this repository earlier. Make sure no other git
process is running and remove the file manually to continue.

解决方法：

找到index.lock 删除即可