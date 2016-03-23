title: ionic起步
date: 2016-03-13 11:35:48
categories:
tags:
---
Ionic:

>Build mobile apps faster with the web technologies you know and love.

>More than code. Ionic is an ecosystem.

1.安装：（确保已经安装Node）

```
$ npm install -g cordova ionic
```

Cordova：http://cordova.apache.org/

>Mobile apps with HTML, CSS & JS Target multiple platforms with one code base Free and open source

2.创建项目：

```
$ ionic start myApp tabs
```

三种选择:

>$ ionic start myApp blank

![img](/assets/images/blank-app@2x.png)

>$ ionic start myApp tabs

![img](/assets/images/tabs-app@2x.png)

>$ ionic start myApp sidemenu

![img](/assets/images/menu-app@2x.png)

3.运行：

```
$ cd myApp
$ ionic platform add ios
$ ionic build ios
$ ionic emulate ios
```


