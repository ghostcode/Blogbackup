title: Grunt遇到的问题
date: 2014-08-05 16:51:03
tags: Grunt
---
在用YEOMAN构建应用时，遇到一个问题：

>D:\nodeJS\node_modules\grunt-html>grunt
	grunt-cli: The grunt command line interface. (v0.1.6)
	Fatal error: Unable to find local grunt.
	If you're seeing this message, either a Gruntfile wasn't found or grunt
	hasn't been installed locally to your project. For more information about
	installing and configuring grunt, please see the Getting Started guide:
	http://gruntjs.com/getting-started

一般我们使用Grunt之前安装：

	npm install -g grunt-cli

然后在项目中使用：

	grunt serve

这时就会报错。

经过查资料，看文档，找到解决方法。

官网资料：

>Note that installing grunt-cli does not install the Grunt task runner! The job of the Grunt CLI is simple: run the version of Grunt which has been installed next to a Gruntfile. This allows multiple versions of Grunt to be installed on the same machine simultaneously.

翻译：

>注意，安装grunt-cli并不等于安装了grunt任务运行器！Grunt CLI的工作很简单：在Gruntfile所在目录调用运行已经安装好的相应版本的Grunt。这就意味着可以在同一台机器上同时安装多个版本的Grunt。

在项目根目录运行：

	npm install grunt --save-dev

__save-dev__会自动将__grunt__添加到__package.json__的__dev-dependency__中，不需要手动添加。

参考：

http://stackoverflow.com/questions/15483735/fatal-error-unable-to-find-local-grunt-when-running-grunt-command