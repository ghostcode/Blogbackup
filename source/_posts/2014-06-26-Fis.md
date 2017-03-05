title: 'Fis 快速入门'
date: 2014-06-26 22:48:48
tags:
---
最近一直在学习百度的[Fis](http:http://fis.baidu.com)。

__何为Fis?__

>FIS是专为解决前端开发中自动化工具、性能优化、模块化框架、开发规范、代码部署、开发流程等问题的工具框架。

现在公司的前端项目主要用到合并、压缩、合并图片（css sprite）、解决缓存问题。

__如何使用Fis?__

1.安装 Fis

Fis是基于NodeJs的，所以开始前请自行安装Node。

安装Fis的命令为：

```
npm install -g fis
```
<!-- more -->

2.资源压缩

资源压缩对前端来说是相当重要，应该说对整个Web的开发很重要。Fis内部集成压缩功能，不需要额外配置。

```
fis release --optimize
```

如果觉得麻烦还有简写：

```
fis release -o
```
3.资源合并

合并需要插件来扩展Fis,安装 [__fis-postpackager-simple__](https://github.com/hefangshi/fis-postpackager-simple)

```
npm install -g fis-postpackager-simple
```
安装好后还需要一些简单的配置，一般一个项目下都会有一个fis-conf.js文件，添加：

```
fis.config.set('modules.postpackager', 'simple');
```

然后就可以合并：

```
fis release --pack
```

4.缓存问题

以前我们解决缓存问题都是这样的：

```
dir/css/demo.css?time=20144231213
```

这种解决方法有弊端？

* 不可避免页面错乱
* 没有更改的资源版本也被刷新

而Fis使用的是一种MD5文件名加密的功能达到的：

```
fis release --md5
```
还是刚才的那个demo.css文件，处理之后变成：

```
dir/css/demo_291db1d.css
```

当然命令也可以连起来写：

```
fis release --optimize --pack --md5
```
或者

```
fis release -omp
```

更进一步了解请看[视频](http://v.youku.com/v_show/id_XNzI1MjQ2OTI0.html)。












