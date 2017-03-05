title: 'window 查看端口占用'
date: 2014-08-22 16:38:26
tags:
---
查看端口命令：

	C:\netstat -aon|findstr 8080

结果：

	TCP  127.0.0.1:80   0.0.0.0:0     LISTENING  2448

端口被进程号为2448的进程占用，继续执行下面命令：

	C:\tasklist|findstr 2448

结果：

	thread.exe       2016 Console      0  16,064 K

很清楚，thread占用了你的端口,Kill it

命令：

	taskkill -F -PID 2448