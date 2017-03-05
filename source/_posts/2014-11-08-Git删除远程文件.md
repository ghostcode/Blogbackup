title: Git删除远程文件
date: 2014-11-08 20:10:37
categories:
tags:
---
若你不小心把文件A传到GitHub上，但你在本地又将其添加到__.gitignore__中。应该如何操作才能删掉远程的文件？

只需要如下的命令：

＊ Remove all items from index.

	git rm -r -f --cached .

＊ Rebuild index.

	git add .

＊ Make new commit

	git commit -m "Removed mylogfile.log"

