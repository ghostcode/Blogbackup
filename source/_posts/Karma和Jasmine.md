title: Karma和Jasmine
date: 2014-08-20 10:28:44
tags:
---
###什么是Karma?###

Karma是Google出品，前身叫“Testacular”，主要目的：

	The main goal for Karma is to bring a productive testing environment to developers.

背景：

	We found that we were struggling with existing tools, so we decided to write
	our own test runner. We wanted a test runner that would meet all of our needs 
	for both quick development and continuous integration -- a truly spectacular test runner. We've called it Testacular.

大意：

	为了满足我们（google）自己的测试运行器，满足快速开发和持续集成的需求。


###什么是Jasmine?###

	Jasmine is a behavior-driven development framework for testing JavaScript code.
	It does not depend on any other JavaScript frameworks. It does not require a DOM. 
	And it has a clean, obvious syntax so that you can easily write tests.

类似的框架：Mocha、QUnit。

<!--more-->

实例：（该实例基于node,请先安装。）

1). 新建项目文件夹：


	mkdir karma &&　cd karma


2). 安装Karma


	npm install -g karma


3). 运行karma


	karma start


得到下面类似信息：


	INFO [karma]: Karma v0.10.2 server started at http://localhost:9876/INFO 
	[Chrome 28.0.1500 (Windows 7)]: Connected on socket nIlM1yUy6ELMp5ZTN9Ek



4). karma+Jasmine配置


	karma init


接着会看到许多选择信息

```
Which testing framework do you want to use ?
Press tab to list possible options. Enter to move to the next question.
> jasmine

Do you want to use Require.js ?
This will add Require.js plugin.
Press tab to list possible options. Enter to move to the next question.
> no

Do you want to capture a browser automatically ?
Press tab to list possible options. Enter empty string to move to the next question.
> Chrome
>

What is the location of your source and test files ?
You can use glob patterns, eg. "js/*.js" or "test/**/*Spec.js".
Enter empty string to move to the next question.
>

Should any of the files included by the previous patterns be excluded ?
You can use glob patterns, eg. "**/*.swp".
Enter empty string to move to the next question.
>

Do you want Karma to watch all the files and run the tests on change ?
Press tab to list possible options.
> yes

Config file generated at "F:\workspace\karma\karma.conf.js".
```

5). 安装karma-jasmine集成包：

	npm install karma-jasmine

6). 安装karma-chrome-launcher（注意如果上面要测试的浏览器还有别的，其余的也要安装）

	npm install karma-chrome-launcher --save-dev

__开始测试，需求：实现单词倒写功能：“ABCD”=>“CDBA”__

1). 新建文件 src.js：


	function reverse(name){
	    return name.split("").reverse().join("");
	}


2). 测试文件 test.js


	describe("A suite of basic functions", function() {
	    it("reverse word",function(){
	        expect("DCBA").toEqual(reverse("ABCD"));
	    });
	});


3). 修改配置karma.conf.js


	module.exports = function (config) {
	    config.set({
	        basePath: '',
	        frameworks: ['jasmine'],
	        files: ['*.js'],
	        exclude: ['karma.conf.js'],
	        reporters: ['progress'],
	        port: 9876,
	        colors: true,
	        logLevel: config.LOG_INFO,
	        autoWatch: true,
	        browsers: ['Chrome'],
	        captureTimeout: 60000,
	        singleRun: false
	    });
	};


4). 运行karma


	karma start karma.conf.js


得到如下类似信息：

	INFO [karma]: Karma v0.12.22 server started at http://localhost:9876/
	INFO [launcher]: Starting browser Chrome
	INFO [Chrome 36.0.1985 (Windows 7)]: Connected on socket lJRdB1wQiahW6-7IpqoI wi
	th id 82373062
	Chrome 36.0.1985 (Windows 7): Executed 1 of 1 SUCCESS (0.01 secs / 0.002 secs)



5. 修改test.js


	describe("A suite of basic functions", function() {
	    it("reverse word",function(){
	        expect("DCBA").toEqual(reverse("ABCD"));
	        expect("Conan").toEqual(reverse("nano"));
	    });
	});

此时会得到错误：

	INFO [karma]: Karma v0.12.22 server started at http://localhost:9876/
	INFO [launcher]: Starting browser Chrome
	INFO [Chrome 36.0.1985 (Windows 7)]: Connected on socket Kta8N3MgRiMy4FLyp82H wi
	th id 72338317
	Chrome 36.0.1985 (Windows 7) A suite of basic function reverse word FAILED
	        Expected 'Conan' to equal 'onan'.
	        Error: Expected 'Conan' to equal 'onan'.
	            at Object.<anonymous> (F:/wordspace/karma/test.js:8:19)
	Chrome 36.0.1985 (Windows 7): Executed 1 of 1 (1 FAILED) ERROR (0.015 secs / 0.0
	04 secs)
	
这错误是正常的。

参考：

1. http://googletesting.blogspot.jp/2012/11/testacular-spectacular-test-runner-for.html
2. http://blog.fens.me/nodejs-karma-jasmine/
3. https://egghead.io/lessons/unit-testing-introduction-to-karma
4. http://www.slideshare.net/IgorNapierala/jasmine-presentation-23905530?next_slideshow=1（翻墙）
5. http://blog.segmentfault.com/bornkiller/1190000000623274